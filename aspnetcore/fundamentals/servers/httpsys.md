---
title: "Реализация веб-сервера, HTTP.sys, в ASP.NET Core"
author: rick-anderson
description: "Вводит HTTP.sys, веб-сервер для ASP.NET Core в Windows. Основанное на драйвер режима ядра Http.Sys, HTTP.sys — это альтернатива Kestrel, который можно использовать для прямого подключения к Интернету, без IIS."
keywords: "Префиксы ASP.NET Core,HttpSys,HTTP.sys,HttpListener,url, протокол SSL"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: d3f9eb4943ed62b674d6bb2ab1b275b0a3c02343
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2017
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Реализация веб-сервера, HTTP.sys, в ASP.NET Core

По [Tom Dykstra](https://github.com/tdykstra) и [Ross Крис](https://github.com/Tratcher)

> [!NOTE]
> Этот раздел относится только к ASP.NET Core 2.0 и более поздних версий. В более ранних версиях ASP.NET Core, называется HTTP.sys [WebListener](xref:fundamentals/servers/weblistener).

Компонент HTTP.sys — [веб-сервер для ASP.NET Core](index.md) , которое будет выполняться только в Windows. Она была основана на [драйвер режима ядра Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). Компонент HTTP.sys — это альтернатива [Kestrel](kestrel.md) , где есть некоторые функции, которые не Kestel. **HTTP.sys не может использоваться с IIS или IIS Express, как оно не совместимо с [модуль ASP.NET Core](aspnet-core-module.md).**

HTTP.sys поддерживает следующие функции:

- [Проверка подлинности Windows](xref:security/authentication/windowsauth)
- Совместное использование порта
- HTTPS с сетевого Адаптера
- HTTP/2 через TLS (Windows 10)
- Файл прямой передачи данных
- Кэширование ответов
- WebSockets (Windows 8)

Поддерживаемые версии Windows:

- Windows 7 и Windows Server 2008 R2 и более поздних версий

[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([загрузке](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Когда следует использовать HTTP.sys

HTTP.sys полезен для развертываний, где нужно предоставить сервера к Интернету напрямую без использования служб IIS.

![HTTP.sys взаимодействует с Интернетом напрямую.](httpsys/_static/httpsys-to-internet.png)

Так как она была основана на Http.Sys, HTTP.sys не требует обратного прокси-сервера для защиты от атак на систему. Компонент Http.Sys — это надежная технология, которое защищает от атак многих типов, а также обеспечивает надежности, безопасности и масштабируемости полнофункциональное веб-сервера. Сами службы IIS выполняется как HTTP-прослушивателем на базе Http.Sys. 

Компонент HTTP.sys — хороший выбор для внутреннего развертывания при необходимости компонент, не поддерживаемых Kestrel, такие как проверка подлинности Windows.

![HTTP.sys взаимодействует с вашей внутренней сетью напрямую.](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>Как использовать HTTP.sys

Ниже приведен обзор задач установки для ОС и приложения ASP.NET Core.

### <a name="configure-windows-server"></a>Настройка Windows Server

* Установите версию .NET с требованиями приложения, такие как [.NET Core](https://www.microsoft.com/net/download/core) или [.NET Framework](https://www.microsoft.com/net/download/framework).

* Предварительной регистрации префиксы URL-адрес, привязку к HTTP.sys и настроить сертификаты SSL

   Если не предварительной регистрации URL-префиксы в Windows, необходимо запустить приложение с правами администратора. Единственное исключение — если привязать к локальному компьютеру с помощью HTTP (не HTTPS) с номерами выше 1024; номер порта в этом случае правами администратора, не являются обязательными.

   Дополнительные сведения см. в разделе [предварительной регистрации префиксов и настройке SSL](#preregister-url-prefixes-and-configure-ssl) далее в этой статье.

* Открыть порты брандмауэра, разрешающее трафик для достижения HTTP.sys.

   Можно использовать *netsh.exe* или [командлеты PowerShell](https://technet.microsoft.com/library/jj554906).

Существуют также [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>Настройка приложения ASP.NET Core для использования HTTP.sys

* Программа установки пакета не требуется, при использовании [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage. [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) пакет включен в metapackage.

* Вызовите `UseHttpSys` метод расширения в `WebHostBuilder` в вашей `Main` метод, указывая любой [параметры HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) , требуется, как показано в следующем примере:

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>Настройка параметров HTTP.sys

Ниже приведены некоторые параметры HTTP.sys и ограничений, которые можно настроить.

**Максимальное клиентских подключений**

Можно задать максимальное число открытых подключений TCP для всего приложения на следующий код в *Program.cs*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

Максимальное число подключений — неограниченное (null) по умолчанию.

**Размер максимального запроса**

Размер максимального запроса текст по умолчанию — 30,000,000 байт которого составляет приблизительно 28.6 МБ.

Чтобы переопределить ограничение в приложении ASP.NET Core MVC рекомендуется использовать [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) атрибута в методе действия:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Ниже приведен пример, в котором показано, как настроить ограничения для всего приложения, каждый запрос:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

Можно переопределить параметр на конкретный запрос в *файла Startup.cs*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
Исключение при попытке настроить ограничение на запрос после начала чтение запроса приложением. Отсутствует `IsReadOnly` свойство, определяющее, если `MaxRequestBodySize` свойства находится в состоянии только для чтения, то есть ограничение уже слишком поздно.

Сведения о других параметрах HTTP.sys см. в разделе [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Настройка URL-адреса и порты прослушивания 

По умолчанию ASP.NET Core привязывается к `http://localhost:5000`. Чтобы настроить префиксы URL-адреса и порты, можно использовать `UseUrls` метод расширения `urls` аргумент командной строки, переменной среды ASPNETCORE_URLS или `UrlPrefixes` свойство [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). Следующий пример кода использует `UrlPrefixes`.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Преимущество `UrlPrefixes` , могут быть получены сообщение об ошибке сразу же при попытке добавить префикс, который неправильно отформатирован. Преимущество `UseUrls` (совместно используется с `urls` и ASPNETCORE_URLS) — что можно легко переключаться между Kestrel и HTTP.sys.

Если используются и `UseUrls` (или `urls` или ASPNETCORE_URLS) и `UrlPrefixes`, параметры в `UrlPrefixes` переопределить значения в `UseUrls`. Дополнительные сведения см. в разделе [размещения](xref:fundamentals/hosting).

Использует HTTP.sys [HTTP Server API UrlPrefix строковые форматы](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Убедитесь, что указан одинаковые строки префикса в `UseUrls` или `UrlPrefixes` , вы предварительной регистрации на сервере. 

### <a name="dont-use-iis"></a>Не используйте IIS

Убедитесь, что приложение не настроено для запуска служб IIS или IIS Express.

В Visual Studio для IIS Express указан профиль запуска по умолчанию. Чтобы запустить проект как консольное приложение, вручную измените выбранного профиля, как показано на следующем снимке экрана.

![Выберите профиль приложения консоли](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Предварительной регистрации префиксы URL-адреса и настроить протокол SSL

Службы IIS и HTTP.sys основаны на базовый драйвер режима ядра Http.Sys для прослушивания запросов и первоначального обработки. В службах IIS пользовательский Интерфейс управления позволяет сравнительно легко настраивать все. Тем не менее необходимо настроить Http.Sys самостоятельно. Встроенное средство для выполнения, то есть *netsh.exe*. 

С *netsh.exe* можно зарезервировать префиксы URL-адрес и назначьте SSL-сертификаты. Средство требуются права администратора.

Ниже приведен пример необходимый минимум для резервирования URL-префиксы для портов 80 и 443.

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Приведенный ниже показано, как назначить SSL-сертификата:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

Ниже приведен в справочной документации для *netsh.exe*:

* [Команды Netsh для гипертекста передачи протокол (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix строк](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Подробные инструкции для нескольких сценариев на следующих ресурсах. Статьи, которые ссылаются на HttpListener применяются при HTTP.sys, как они строятся на Http.Sys.

* [Как: Настройка порта с SSL-сертификата](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [Подключения по протоколу HTTPS - HttpListener и на основе размещения сертификацию клиента](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) это является блог сторонних разработчиков и довольно старый, но по-прежнему содержит полезные сведения.
* [Практическое руководство: Пошаговое руководство с помощью HttpListener или HTTP-сервере неуправляемого типов кода (C++) как простой сервера SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) это тоже старые блог полезные сведения.

Ниже приведены некоторые сторонних средств, которые могут быть удобнее, чем *netsh.exe* командной строки. Они не предоставляется или не поддерживается корпорацией Майкрософт. Средства Запуск от имени администратора по умолчанию, поскольку *netsh.exe* сам требуются права администратора.

* [HTTP.sys Manager](http://httpsysmanager.codeplex.com/) предоставляет пользовательский Интерфейс для включения в список и Настройка SSL-сертификатов и параметров, префиксов резервирования и списки доверия сертификатов. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) позволяет перечислить или настроить сертификаты SSL и префиксы URL-адрес. Пользовательский Интерфейс будет более http.sys Manager предоставляет несколько дополнительных параметров конфигурации, а в противном случае она предоставляет аналогичные функциональные возможности. Не удается создать новый список доверия сертификатов (CTL), но можно назначить уже существующую.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих ресурсах:

* [Образец приложения для этой статьи](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [HTTP.sys исходного кода](https://github.com/aspnet/HttpSysServer/)
* [Размещение](xref:fundamentals/hosting)
