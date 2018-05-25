---
title: Реализации веб-сервера WebListener в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о WebListener, веб-сервере для ASP.NET Core в Windows, который можно использовать для прямого подключения к Интернету без IIS.
manager: wpickett
ms.author: riande
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 46871edb744ad152df8eb958b344068b7408dd1e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/17/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>Реализации веб-сервера WebListener в ASP.NET Core

Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)

> [!NOTE]
> Сведения из этого раздела распространяются только на ASP.NET Core 1.x. В ASP.NET Core 2.0 WebListener называется [HTTP.sys](httpsys.md).

WebListener — это [веб-сервер для ASP.NET Core](index.md), который запускается только в Windows. Он основан на [работающем в режиме ядра драйвере Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener является альтернативой для [Kestrel](kestrel.md), которую можно использовать для прямого подключения к Интернету, не используя IIS в качестве обратного прокси-сервера. Фактически **WebListener не подходит для использования с IIS или IIS Express, так как он не совместим с [модулем ASP.NET Core](aspnet-core-module.md).**

Хотя WebListener был разработан для ASP.NET Core, его невозможно использовать напрямую в каком-либо приложении .NET Core или .NET Framework через пакет NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).

WebListener поддерживает следующие функции:

- [Проверка подлинности Windows](xref:security/authentication/windowsauth)
- Совместное использование портов
- HTTPS с SNI
- HTTP/2 через TLS (Windows 10)
- Прямая передача файлов
- Кэширование откликов
- WebSockets (Windows 8)

Поддерживаемые версии Windows:

- Windows 7 и Windows Server 2008 R2 и более поздних версий

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Условия использования WebListener

WebListener удобен для развертываний, где нужно подключить сервер к Интернету напрямую без использования служб IIS.

![Weblistener взаимодействует с Интернетом напрямую.](weblistener/_static/weblistener-to-internet.png)

Так как WebListener основан на Http.Sys, ему не нужен обратный прокси-сервер для защиты от атак. Http.Sys — это проверенная технология, которая защищает от многих типов атак, а также обеспечивает надежность, безопасность и масштабируемость полнофункционального веб-сервера. Сами службы IIS выполняются в качестве HTTP-прослушивателя поверх Http.Sys. 

WebListener также хорошо подходит для внутренних развертываний, когда нужна функция, отсутствующая в Kestrel.

![Weblistener взаимодействует с вашей внутренней сетью напрямую.](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>Способы использования WebListener

Ниже приведен обзор задач установки для ОС узла и приложения ASP.NET Core.

### <a name="configure-windows-server"></a>Настройка Windows Server

* Установите нужную вашему приложению версию платформы .NET, например [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) или .NET Framework 4.5.1.

* Предварительно зарегистрируйте префиксы URL-адресов для привязки к WebListener и настройте SSL-сертификаты.

   Если вы не выполняете предварительную регистрацию префиксов URL-адресов в Windows, приложение потребуется запустить с правами администратора. Единственным исключением является привязка к localhost через HTTP (не HTTPS) с номером порта больше 1024. В этом случае права администратора не требуются.

   Дополнительные сведения см. в разделе [Предварительная регистрация префиксов и настройка SSL](#preregister-url-prefixes-and-configure-ssl) ниже.

* Откройте порты брандмауэра, чтобы разрешить трафик в WebListener.

   Можно использовать netsh.exe или [командлеты PowerShell](https://technet.microsoft.com/library/jj554906).

Кроме того, существуют [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Настройка приложения ASP.NET Core

* Установите пакет NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Он также устанавливает [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) в качестве зависимости.

* Вызовите метод расширения `UseWebListener` для [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) в вашем методе `Main`, указав [параметры](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) WebListener и другие нужные [параметры](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs), как показано в следующем примере:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Настройка URL-адресов и портов для прослушивания 

  По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`. Чтобы настроить префиксы URL-адресов и порты, можно использовать метод расширения `UseURLs`, аргумент командной строки `urls` или систему конфигурации ASP.NET Core. Дополнительные сведения см. в разделе [Размещение в ASP.NET Core(xref:fundamentals/host/index).

  WebListener использует [форматы строк префикса Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). Для WebListener не предъявляются отдельные требования к формату строк префикса.

  > [!WARNING]
  > **Не используйте** привязки с подстановочными знаками (`http://*:80/` и `http://+:80`) на верхнем уровне. Это может создать уязвимость и поставить ваше приложение под угрозу. Сюда относятся и строгие, и нестрогие подстановочные знаки. Вместо этого используйте имена узлов в явном виде. Привязки с подстановочными знаками на уровне дочерних доменов (например `*.mysub.com`) не создают таких угроз безопасности, если вы полностью контролируете родительский домен (в отличие от варианта `*.com`, создающего уязвимость). Дополнительные сведения см. в документе [rfc7230, раздел 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).

  > [!NOTE]
  > Убедитесь, что указали одинаковые строки префикса в `UseUrls`, предварительно регистрируемого на сервере. 

* Убедитесь, что приложение не настроено для запуска IIS или IIS Express.

  В Visual Studio профиль запуска по умолчанию использует IIS Express.  Чтобы запустить проект как консольное приложение, вручную измените выбранный профиль, как показано на следующем снимке экрана.

  ![Выбор профиля консольного приложения](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Использование WebListener за пределами ASP.NET Core

* Установите пакет NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).

* [Предварительно зарегистрируйте префиксы URL-адресов для привязки к WebListener и настройте SSL-сертификаты](#preregister-url-prefixes-and-configure-ssl), как и при использовании в ASP.NET Core.

Кроме того, существуют [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).


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

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Предварительная регистрация префиксов URL-адресов и настройка SSL

Как IIS, так и WebListener основаны на работающем в режиме ядра драйвере Http.Sys, который осуществляет прослушивание запросов и первоначальную обработку. В IIS пользовательский интерфейс управления позволяет сравнительно легко настраивать все необходимое. Однако при использовании WebListener вам нужно настроить Http.Sys самостоятельно. Для этого служит встроенное средство netsh.exe. 

Чаще всего netsh.exe используется для резервирования префиксов URL-адресов и назначения SSL-сертификатов.

Средство NetSh.exe не слишком удобно для начинающих. Следующий пример показывает минимальный код, позволяющий зарезервировать префиксы URL-адресов для портов 80 и 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Следующий пример показывает, как назначить SSL-сертификат:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

Ниже указана официальная справочная документация:

* [Команды Netsh для протокола HTTP](https://technet.microsoft.com/library/cc725882.aspx)
* [Строки UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Приведенные ниже ресурсы содержат подробные инструкции по нескольким сценариям. Статьи, описывающие `HttpListener`, в равной степени применимы и к `WebListener`, так как оба этих компонента основаны на Http.Sys.

* [Практическое руководство. Настройка порта с использованием SSL-сертификата](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [Взаимодействие через HTTPS: размещение и сертификация клиента на основе HttpListener](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) — это довольно старый блог сторонних разработчиков, однако в нем все равно есть полезные сведения.
* [Практическое руководство. Пошаговые инструкции по использованию HttpListener или неуправляемого кода HTTP-сервера (C++) в качестве простого сервера SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) — это еще один старый блог с полезными сведениями.
* [Как настроить .NET Core WebListener с использованием SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Ниже приведены некоторые сторонние средства, которые могут быть удобнее, чем командная строка netsh.exe. Они не предоставляются и не поддерживаются корпорацией Майкрософт. По умолчанию эти средства запускаются от имени администратора, так как права администратора нужны и netsh.exe.

* [http.sys Manager](http://httpsysmanager.codeplex.com/) предоставляет пользовательский интерфейс для перечисления и настройки SSL-сертификатов и параметров, резервирований префиксов и списков доверия сертификатов. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) позволяет перечислить или настроить SSL-сертификаты и префиксы URL-адресов. По сравнению с http.sys Manager пользовательский интерфейс более подробный и предоставляет несколько дополнительных параметров конфигурации, а в остальном аналогичен. В нем невозможно создать список доверия сертификатов (CTL), но можно назначить уже существующий.

Для создания самозаверяющих SSL-сертификатов корпорация Майкрософт предоставляет средства командной строки: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) и командлет PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). Существуют и сторонние средства пользовательского интерфейса, облегчающие создание самозаверяющих SSL-сертификатов:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Makecert UI](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих ресурсах:

* [Пример приложения для этой статьи](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [Исходный код WebListener](https://github.com/aspnet/HttpSysServer/)
* [Размещение](xref:fundamentals/host/index)
