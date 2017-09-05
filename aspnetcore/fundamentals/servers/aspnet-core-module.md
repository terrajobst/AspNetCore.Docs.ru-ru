---
title: "Модуль ASP.NET Core"
author: tdykstra
description: "Вводит ASP.NET Core модуля (ANCM), модуль, позволяющий использовать в качестве обратного прокси-сервера IIS или IIS Express Kestrel веб-сервере IIS."
keywords: "ASP.NET Core, IIS, IIS Express, модуль ASP.NET Core UseIISIntegration"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: 4661af33-34c5-4d71-93a0-8c7632f43580
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/aspnet-core-module
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c4124f71f30b758d82a6bf641328a8d5abf779f2
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-aspnet-core-module"></a>Введение в ASP.NET Core модуля

По [Tom Dykstra](http://github.com/tdykstra), [Рик Штраль](https://github.com/RickStrahl), и [Ross Крис](https://github.com/Tratcher) 

Модуль Core ASP.NET (ANCM) позволяет запускать приложениям за IIS, ASP.NET Core с помощью служб IIS для что такое эффективное (безопасности, управляемости и много больше) и [Kestrel](kestrel.md) для что такое эффективное (быстротой действительно) и получение преимущества обеих технологий за один раз. **ANCM работает только с Kestrel; оно не совместимо с WebListener (в ASP.NET Core 1.x) или HTTP.sys (в 2.x).** 

Поддерживаемые версии Windows:

* Windows 7 и Windows Server 2008 R2 и более поздних версий

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)

## <a name="what-aspnet-core-module-does"></a>Назначение модуля ASP.NET Core

ANCM имеет собственный модуль IIS, который подключается к конвейеру IIS и перенаправляет трафик к серверной части приложения ASP.NET Core. Большинство других модулей, такие как проверка подлинности windows, по-прежнему будет возможность запустить. ANCM берет управление, только если сопоставления обработчика, определенных в приложении, и обработчик выбран для запроса *web.config* файла.

Поскольку приложения ASP.NET Core, выполняемые в процессе разделения из рабочего процесса IIS, ANCM также управление процессами. ANCM запускает процесс для приложения ASP.NET Core, когда первый запрос поступает и перезапускает ее при сбоях. Это по сути совпадает с поведением классических приложениях ASP.NET, запустите в работе в IIS и управляются с помощью WAS (службы активации Windows).

Вот на диаграмме, которая показана связь между приложениями служб IIS, ANCM и ASP.NET Core.

![Модуль ASP.NET Core](aspnet-core-module/_static/ancm.png)

Запросы поступают из Интернета и попадания драйвер Http.Sys режима ядра, который направляет их в службы IIS на основном порту (80) или SSL-порта (443). ANCM направляет запросы к приложению ASP.NET Core на HTTP-порта, настроенного для приложения, который не используется порт 80 или 443.

Kestrel прослушивает трафик, поступающий из ANCM.  ANCM указывает порт, через переменную среды во время запуска и [UseIISIntegration](#call-useiisintegration) метод настраивает сервер на прослушивание `http://localhost:{port}`. Существуют дополнительные проверки, чтобы отклонять запросы не ANCM. (ANCM не поддерживает пересылка HTTPS, поэтому запросы направляются по протоколу HTTP, даже если получен службой IIS, по протоколу HTTPS.)

Kestrel принимает запросы от ANCM и помещает их в конвейере ASP.NET Core по промежуточного слоя, который их обрабатывает и передает их в качестве `HttpContext` экземпляров для логики приложения. Ответы приложений, затем передаются обратно в IIS, какие отправлений их обратно в HTTP-клиента, который инициировал запросы.

ANCM имеет несколько других функций.

* Задает переменные среды.
* Журналы `stdout` выходных данных для хранения файлов.
* Пересылает токены проверки подлинности Windows.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>Как использовать в приложениях ASP.NET Core ANCM

В этом разделе Обзор процесса настройки сервера IIS и ASP.NET Core приложения. Подробные инструкции см. в разделе [публикация в службах IIS](../../publishing/iis.md).

### <a name="install-ancm"></a>Установить ANCM

Модуль Core ASP.NET должно быть установлено в службах IIS на серверах и в IIS Express на компьютерах разработки. Для серверов, ANCM включается в [пакета .NET Core Windows Server, где размещены](https://aka.ms/dotnetcore.2.0.0-windowshosting). Для компьютеров разработчиков Visual Studio автоматически устанавливает ANCM в IIS Express и в службах IIS, если он еще не установлен на компьютере.

### <a name="install-the-iisintegration-nuget-package"></a>Установка пакета IISIntegration NuGet

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) пакет включен в ASP.NET Core metapackages ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) и [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ). Если вы не используете один из metapackages, установите `Microsoft.AspNetCore.Server.IISIntegration` отдельно. `IISIntegration` Пакета — пакет взаимодействие, который считывает переменные среды вещания по ANCM для настройки приложения. Переменные среды предоставляют сведения о конфигурации, такие как порт для прослушивания. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

В приложении, установите [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/). `IISIntegration` Пакета — пакет взаимодействие, который считывает переменные среды вещания по ANCM для настройки приложения. Переменные среды предоставляют сведения о конфигурации, такие как порт для прослушивания. 

---

### <a name="call-useiisintegration"></a>Вызов UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`UseIISIntegration` Метод расширения в [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) вызывается автоматически при выполнении с IIS.

Если не используется один из metapackages ASP.NET Core и еще не установили `Microsoft.AspNetCore.Server.IISIntegration` пакета, возникнет ошибка времени выполнения. При вызове метода `UseIISIntegration` явным образом, если пакет не установлен получить ошибку времени компиляции.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

В вашем приложении `Main` метод, вызовите `UseIISIntegration` метод расширения в [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder). 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

`UseIISIntegration` Метод ищет переменные среды, которые задает ANCM и его ops нет, если они не найдены. Это облегчает сценариев разработки и тестирования на macOS или Linux, и развертывание на сервере под управлением служб IIS. Во время выполнения с macOS или Linux, Kestrel выступает в качестве веб-сервера. Однако при развертывании приложения в среде IIS, он автоматически использует ANCM и IIS.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>Привязка порта ANCM переопределяет другие привязки портов

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM создает динамический порт для назначения для внутренней обработки. `UseIISIntegration` Метод принимает этот динамический порт и настраивает Kestrel для прослушивания `http://locahost:{dynamicPort}/`. Это значение переопределяет другие конфигурации URL-адрес, например вызовы `UseUrls` или [API прослушивания по Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). Таким образом, не нужно вызывать `UseUrls` или Kestrel в `Listen` API при использовании ANCM. Если вы вызываете `UseUrls` или `Listen`, Kestrel прослушивает порт, укажите при запуске приложения без IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM создает динамический порт для назначения для внутренней обработки. `UseIISIntegration` Метод принимает этот динамический порт и настраивает Kestrel для прослушивания `http://locahost:{dynamicPort}/`. Это значение переопределяет другие конфигурации URL-адрес, например вызовы `UseUrls`. Таким образом, не нужно вызывать `UseUrls` при использовании ANCM. Если вы вызываете `UseUrls`, Kestrel прослушивает порт, укажите при запуске приложения без IIS.

В ASP.NET Core 1.0, при вызове метода `UseUrls`, она вызывается **перед** вызове `UseIISIntegration` , чтобы ANCM настроенный порт не перезаписывается. Этот порядок вызова не требуется в ASP.NET Core 1.1, так как параметр ANCM переопределяет `UseUrls`.

---

### <a name="configure-ancm-options-in-webconfig"></a>Настройка параметров ANCM в файле Web.config

Конфигурация для модуля ASP.NET Core хранится в *Web.config* файл, расположенный в корневой папке приложения. Параметры в этом файле точки для запуска команды и аргументы, которые запуска приложения ASP.NET Core. Образец кода Web.config и рекомендации по настройке см. в разделе [модуля конфигурации ASP.NET Core Reference](../../hosting/aspnet-core-module.md).

### <a name="run-with-iis-express-in-development"></a>Запустите сервер IIS Express в разработке

IIS Express можно запускать Visual Studio с использованием профиля по умолчанию, определенным в шаблонах ASP.NET Core.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих ресурсах:

* [Образец приложения для этой статьи](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [Исходный код ASP.NET модуль Core](https://github.com/aspnet/AspNetCoreModule)
* [Справочник по конфигурации модуля ASP.NET Core](../../hosting/aspnet-core-module.md)
* [Публикация в IIS](../../publishing/iis.md)
