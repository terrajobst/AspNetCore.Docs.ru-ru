---
title: "Реализация WebListener веб-сервера в ASP.NET Core"
author: rick-anderson
description: "Вводит WebListener веб-сервер для ASP.NET Core в Windows. Основанное на драйвер режима ядра Http.Sys, WebListener является альтернативой Kestrel, который можно использовать для прямого подключения к Интернету, без IIS."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: f1bdbc723e4602c2e53723aff91ec5d254f4bd93
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>Реализация WebListener веб-сервера в ASP.NET Core

По [Tom Dykstra](https://github.com/tdykstra) и [Ross Крис](https://github.com/Tratcher)

> [!NOTE]
> Этот раздел относится только к ASP.NET Core 1.x. В ASP.NET Core 2.0, называется WebListener [HTTP.sys](httpsys.md).

— WebListener [веб-сервер для ASP.NET Core](index.md) , которое будет выполняться только в Windows. Она была основана на [драйвер режима ядра Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener является альтернативой [Kestrel](kestrel.md) , можно использовать для прямого подключения к Интернету без использования IIS в качестве обратного прокси-сервера. На самом деле **WebListener не может использоваться с IIS или IIS Express, как оно не совместимо с [модуль ASP.NET Core](aspnet-core-module.md).**

Несмотря на то, что WebListener был разработан для ASP.NET Core, он может использоваться непосредственно в любом приложении .NET Core или .NET Framework, через [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) пакет NuGet.

WebListener поддерживает следующие функции:

- [Проверка подлинности Windows](xref:security/authentication/windowsauth)
- Совместное использование порта
- HTTPS с сетевого Адаптера
- HTTP/2 через TLS (Windows 10)
- Файл прямой передачи данных
- Кэширование ответов
- WebSockets (Windows 8)

Поддерживаемые версии Windows:

- Windows 7 и Windows Server 2008 R2 и более поздних версий

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Когда следует использовать WebListener

WebListener полезен для развертываний, где нужно предоставить сервера к Интернету напрямую без использования служб IIS.

![Weblistener взаимодействует с Интернетом напрямую.](weblistener/_static/weblistener-to-internet.png)

Так как она была основана на Http.Sys, WebListener не требует обратного прокси-сервера для защиты от атак на систему. Компонент Http.Sys — это надежная технология, которое защищает от атак многих типов, а также обеспечивает надежности, безопасности и масштабируемости полнофункциональное веб-сервера. Сами службы IIS выполняется как HTTP-прослушивателем на базе Http.Sys. 

WebListener при также хорошо подходит для развертываний внутренней требуется одна из возможностей, которой он обеспечивает, что не удается получить с помощью Kestrel.

![Weblistener взаимодействует с вашей внутренней сетью напрямую.](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>Как использовать WebListener

Ниже приведен обзор задач установки для ОС и приложения ASP.NET Core.

### <a name="configure-windows-server"></a>Настройка Windows Server

* Установите версию .NET с требованиями приложения, такие как [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) или .NET Framework 4.5.1.

* Предварительной регистрации префиксы URL-адрес, привязку к WebListener и настроить сертификаты SSL

   Если не предварительной регистрации URL-префиксы в Windows, необходимо запустить приложение с правами администратора. Единственное исключение — если привязать к локальному компьютеру с помощью HTTP (не HTTPS) с номерами выше 1024; номер порта в этом случае правами администратора, не являются обязательными.

   Дополнительные сведения см. в разделе [предварительной регистрации префиксов и настройке SSL](#preregister-url-prefixes-and-configure-ssl) далее в этой статье.

* Открыть порты брандмауэра, разрешающее трафик для достижения WebListener.

   Можно использовать netsh.exe или [командлеты PowerShell](https://technet.microsoft.com/library/jj554906).

Существуют также [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Настройка приложения ASP.NET Core

* Установка пакета NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Также устанавливает [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) как зависимость.

* Вызовите `UseWebListener` метод расширения в [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) в вашей `Main` метод, указывая любой WebListener [параметры](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) и [параметры](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) нужно , как показано в следующем примере:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Настройка URL-адреса и порты прослушивания 

  По умолчанию ASP.NET Core привязывается к `http://localhost:5000`. Чтобы настроить префиксы URL-адреса и порты, можно использовать `UseURLs` метод расширения `urls` аргумент командной строки или система конфигурации ASP.NET Core. Дополнительные сведения см. в разделе [Размещение](../../fundamentals/hosting.md).

  Веб-прослушиватель использует [Http.Sys префикс строковые форматы](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). Требования к формат строки префикса, относящиеся к WebListener отсутствуют.

  > [!NOTE]
  > Убедитесь, что указан одинаковые строки префикса в `UseUrls` , вы предварительной регистрации на сервере. 

* Убедитесь, что приложение не настроено для запуска служб IIS или IIS Express.

  В Visual Studio для IIS Express указан профиль запуска по умолчанию.  Чтобы запустить проект как консольное приложение необходимо вручную изменить выбранного профиля, как показано на следующем снимке экрана.

  ![Выберите профиль приложения консоли](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Как использовать WebListener за пределами ASP.NET Core

* Установка [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) пакет NuGet.

* [Префиксы URL-адрес, привязку к WebListener и настроить сертификаты SSL предварительной регистрации](#preregister-url-prefixes-and-configure-ssl) как для использования в ASP.NET Core.

Существуют также [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).


Ниже приведен пример кода, демонстрирующий применение WebListener за пределами ASP.NET Core:

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Предварительной регистрации префиксы URL-адреса и настроить протокол SSL

Службы IIS и WebListener основаны на базовый драйвер режима ядра Http.Sys для прослушивания запросов и первоначального обработки. В службах IIS пользовательский Интерфейс управления позволяет сравнительно легко настраивать все. Если вы используете WebListener необходимо настроить Http.Sys самостоятельно. Встроенное средство выполнения этих процедур является netsh.exe. 

Наиболее распространенные задачи, необходимо использовать netsh.exe для резервирования URL-префиксы и назначение SSL-сертификаты.

NetSh.exe не удобное средство для начинающих. Ниже приведен пример минимальный набор необходимых для резервирования URL-префиксы для портов 80 и 443.

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Приведенный ниже показано, как назначить SSL-сертификата:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

Ниже приведен в официальной документации.

* [Команды Netsh для гипертекста передачи протокол (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix строк](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Подробные инструкции для нескольких сценариев на следующих ресурсах. Статьи, которые ссылаются на `HttpListener` применяются как к `WebListener`, как они строятся на Http.Sys.

* [Практическое руководство. Настройка порта с использованием SSL-сертификата](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [Подключения по протоколу HTTPS - HttpListener и на основе размещения сертификацию клиента](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) это является блог сторонних разработчиков и довольно старый, но по-прежнему содержит полезные сведения.
* [Практическое руководство: Пошаговое руководство с помощью HttpListener или HTTP-сервере неуправляемого типов кода (C++) как простой сервера SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) это тоже старые блог полезные сведения.
* [Как настроить WebListener Core .NET с помощью протокола SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Ниже приведены некоторые сторонних средств, которые могут быть проще в использовании, чем netsh.exe командной строки. Они не предоставляется или не поддерживается корпорацией Майкрософт. Средства Запуск от имени администратора по умолчанию, поскольку сам netsh.exe требуются права администратора.

* [HTTP.sys Manager](http://httpsysmanager.codeplex.com/) предоставляет пользовательский Интерфейс для включения в список и Настройка SSL-сертификатов и параметров, префиксов резервирования и списки доверия сертификатов. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) позволяет перечислить или настроить сертификаты SSL и префиксы URL-адрес. Пользовательский Интерфейс будет более http.sys Manager предоставляет несколько дополнительных параметров конфигурации, а в противном случае она предоставляет аналогичные функциональные возможности. Не удается создать новый список доверия сертификатов (CTL), но можно назначить уже существующую.

Для создания самозаверяющего SSL-сертификаты, корпорация Майкрософт предоставляет средства командной строки: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) и командлет PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). Существуют также сторонние пользовательского интерфейса средства, облегчающие создавать самозаверяющие сертификаты SSL:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Makecert пользовательского интерфейса](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих ресурсах:

* [Образец приложения для этой статьи](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [WebListener исходного кода](https://github.com/aspnet/HttpSysServer/)
* [Размещение](../hosting.md)
