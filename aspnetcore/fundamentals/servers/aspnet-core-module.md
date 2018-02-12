---
title: "Модуль ASP.NET Core"
author: tdykstra
description: "Вводные сведения о модуле ASP.NET Core (ANCM), позволяющем веб-серверу Kestrel использовать IIS или IIS Express в качестве обратного прокси-сервера."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4337bc42c5454d6a9634a396d9c89f3518af148b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-aspnet-core-module"></a>Введение в модуль ASP.NET Core

Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Рик Штраль](https://github.com/RickStrahl) (Rick Strahl) и [Крис Росс](https://github.com/Tratcher) (Chris Ross) 

Модуль Core ASP.NET (ANCM) позволяет запускать приложения ASP.NET Core за службами IIS, используя преимущества обеих технологий — IIS (безопасность, управляемость и многое другое) и [Kestrel](kestrel.md) (высокое быстродействие). **ANCM работает только с Kestrel, он не совместим с WebListener (в ASP.NET Core 1.x) или HTTP.sys (в 2.x).** 

Поддерживаемые версии Windows:

* Windows 7 и Windows Server 2008 R2 и более поздних версий

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>Назначение модуля ASP.NET Core

ANCM — это собственный модуль IIS, который подключается к конвейеру IIS и перенаправляет трафик в серверное приложение ASP.NET Core. Большинство других модулей, таких как проверка подлинности Windows, по-прежнему имеют возможность запуска. ANCM принимает управление, только если для запроса выбран обработчик, а в файле *web.config* приложения определено сопоставление этого обработчика.

Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, модуль ANCM также осуществляет управление процессами. ANCM запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает его при сбое. Это, по сути, совпадает с поведением классических приложений ASP.NET, выполняемых внутрипроцессно в IIS и управляемых службой активации Windows (WAS).

Ниже приведена схема, показывающая связь между IIS, ANCM и приложениями ASP.NET Core.

![Модуль ASP.NET Core](aspnet-core-module/_static/ancm.png)

Запросы поступают из Интернета и попадают в драйвер режима ядра Http.Sys, который направляет их в IIS по основному порту (80) или SSL-порту (443). ANCM переадресует запросы в приложение ASP.NET Core по настроенному для него HTTP-порту, который отличается от порта 80/443.

Kestrel прослушивает трафик, поступающий из модуля ANCM.  ANCM задает порт с помощью переменной среды во время запуска, а метод [UseIISIntegration](#call-useiisintegration) настраивает сервер для прослушивания `http://localhost:{port}`. Существуют и дополнительные проверки, позволяющие отклонять запросы, поступающие не из ANCM. (ANCM не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.)

Kestrel принимает запросы от ANCM и помещает их в конвейер ПО промежуточного слоя ASP.NET Core, который затем обрабатывает их и передает в качестве экземпляров `HttpContext` в логику приложения. После этого отклики приложений передаются обратно в службы IIS, которые отправляют их обратно в HTTP-клиент, инициировавший соответствующие запросы.

ANCM выполняет и некоторые другие функции:

* задает переменные среды;
* регистрирует в журнале выходные данные `stdout` для хранилища файлов;
* переадресовывает токены проверки подлинности Windows.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>Условия использования ANCM в приложениях ASP.NET Core

Этот раздел содержит обзор настройки сервера IIS и приложения ASP.NET Core. Подробные инструкции см. в разделе [Размещение в Windows с помощью IIS](xref:host-and-deploy/iis/index).

### <a name="install-ancm"></a>Установка ANCM


Модуль Core ASP.NET нужно установить в службах IIS на серверах, а также в IIS Express на компьютерах разработки. Для серверов модуль ANCM включен в [пакет размещения .NET Core для Windows Server](https://aka.ms/dotnetcore-2-windowshosting). Для компьютеров разработки Visual Studio автоматически устанавливает ANCM в IIS Express и IIS, если они уже установлены на компьютере.

### <a name="install-the-iisintegration-nuget-package"></a>Установка пакета NuGet IISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Пакет [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) входит в состав метапакетов ASP.NET Core ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) и [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)). Если вы не используете один из метапакетов, установите `Microsoft.AspNetCore.Server.IISIntegration` отдельно. `IISIntegration` представляет собой пакет взаимодействия, который считывает переменные среды, транслируемые модулем ANCM, для настройки приложения. Переменные среды предоставляют сведения о конфигурации, например прослушиваемый порт. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Установите [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) в приложении. `IISIntegration` представляет собой пакет взаимодействия, который считывает переменные среды, транслируемые модулем ANCM, для настройки приложения. Переменные среды предоставляют сведения о конфигурации, например прослушиваемый порт. 

---

### <a name="call-useiisintegration"></a>Вызов UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Метод расширения `UseIISIntegration` в [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) вызывается автоматически при выполнении в IIS.

Если вы не используете один из метапакетов ASP.NET Core и еще не установили пакет `Microsoft.AspNetCore.Server.IISIntegration`, возникает ошибка времени выполнения. Если пакет не установлен, при явном вызове `UseIISIntegration` возникает ошибка времени компиляции.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Метод `Main` в приложении вызывает метод расширения `UseIISIntegration` для [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder). 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

Метод `UseIISIntegration` ищет переменные среды, задаваемые модулем ANCM, а при их отсутствии не выполняет никакие операции. Это поведение упрощает реализацию таких сценариев, как разработка и тестирование в macOS или Linux и развертывание на сервере под управлением IIS. Kestrel при запуске в macOS или Linux выступает в качестве веб-сервера. Однако при развертывании приложения в среде IIS оно автоматически использует ANCM и IIS.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>Привязка порта ANCM переопределяет все другие привязки портов

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM создает динамический порт для назначения серверному процессу. Метод `UseIISIntegration` принимает этот динамический порт и настраивает Kestrel для прослушивания адресу `http://locahost:{dynamicPort}/`. Это переопределяет другие конфигурации URL-адресов, такие как вызовы `UseUrls` или [API прослушивания Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). Таким образом, вам не нужно вызывать `UseUrls` или API `Listen` Kestrel при использовании ANCM. Если вы вызываете `UseUrls` или `Listen`, Kestrel прослушивает заданный вами порт при запуске приложения без IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM создает динамический порт для назначения серверному процессу. Метод `UseIISIntegration` принимает этот динамический порт и настраивает Kestrel для прослушивания адресу `http://locahost:{dynamicPort}/`. Это переопределяет другие конфигурации URL-адресов, такие как вызовы `UseUrls`. Таким образом, вам не нужно вызывать `UseUrls` при использовании ANCM. Если вы вызываете `UseUrls`, Kestrel прослушивает заданный вами порт при запуске приложения без IIS.

Если вы вызываете `UseUrls` в ASP.NET Core 1.0, вызывайте его **до** вызова `UseIISIntegration`, чтобы предотвратить перезапись порта, настроенного модулем ANCM. В ASP.NET Core 1.1 соблюдать этот порядок вызовов не требуется, так как параметр ANCM переопределяет `UseUrls`.

---

### <a name="configure-ancm-options-in-webconfig"></a>Настройка параметров ANCM в файле Web.config

Конфигурация для модуля ASP.NET Core хранится в файле *web.config*, расположенном в корневой папке приложения. Параметры в этом файле указывают на команду и аргументы, служащие для запуска приложения ASP.NET Core. Пример кода *web.config* и сведения о параметрах конфигурации см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

### <a name="run-with-iis-express-in-development"></a>Выполнение с IIS Express в среде разработки

Visual Studio может запустить службы IIS Express с использованием профиля по умолчанию, определенного в шаблонах ASP.NET Core.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Конфигурация прокси-сервера использует протокол HTTP и токен связывания

Прокси-сервер, созданный между ANCM и Kestrel, использует протокол HTTP. Использование HTTP позволяет оптимизировать производительность, так как трафик между ANCM и Kestrel передается на петлевой адрес в сетевом интерфейсе. Отсутствует риск перехвата трафика между ANCM и Kestrel из расположения на сервере.

Токен связывания гарантирует, что полученные Kestrel запросы были переданы службами IIS, а не из какого-либо другого источника. Этот токен создается и задается модулем ANCM в переменной среды (`ASPNETCORE_TOKEN`). Он также задается в заголовке (`MSAspNetCoreToken`) каждого запроса, переданного через прокси-сервер. ПО промежуточного слоя IIS проверяет каждый получаемый запрос, чтобы убедиться, что заголовок с токеном связывания соответствует значению переменной среды. Если значения токена не совпадают, запрос заносится в журнал и отклоняется. Переменная среды с токеном связывания и трафик между ANCM и Kestrel недоступны из расположения на сервере. Не зная значение токена связывания, злоумышленник не может отправлять запросы, обходящие проверку в ПО промежуточного слоя IIS.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих ресурсах:

* [Пример приложения для этой статьи](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [Исходный код модуля ASP.NET Core](https://github.com/aspnet/AspNetCoreModule)
* [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Размещение в Windows с помощью IIS](xref:host-and-deploy/iis/index)
