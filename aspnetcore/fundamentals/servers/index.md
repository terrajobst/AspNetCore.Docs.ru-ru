---
title: Реализации веб-сервера в ASP.NET Core
author: rick-anderson
description: Откройте возможности веб-серверов Kestrel и HTTP.sys для ASP.NET Core. Рекомендации по выбору сервера и сведения о сценариях использования обратного прокси-сервера.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/13/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 0f1460af5bc1cd879ff11e43775ac16ca36b150e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011759"
---
# <a name="web-server-implementations-in-aspnet-core"></a>Реализации веб-сервера в ASP.NET Core

Авторы: [Том Дисктра](https://github.com/tdykstra) (Tom Dykstra), [Стив Смит](https://ardalis.com/) (Steve Smith), [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)

Приложение ASP.NET Core выполняется вместе с внутрипроцессной реализацией HTTP-сервера. Реализация сервера прослушивает HTTP-запросы и передает их в приложение как наборы [функций запросов](xref:fundamentals/request-features), объединенных в [HttpContext](/dotnet/api/system.web.httpcontext).

ASP.NET Core предоставляет две реализации серверов.

* [Kestrel](xref:fundamentals/servers/kestrel) — кросс-платформенный HTTP-сервер по умолчанию для ASP.NET Core.
* [HTTP.sys](xref:fundamentals/servers/httpsys) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра HTTP.sys и API HTTP-сервера](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). (В ASP.NET Core 1.x HTTP.sys называется [WebListener](xref:fundamentals/servers/weblistener).)

## <a name="kestrel"></a>Kestrel

Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.

::: moniker range=">= aspnetcore-2.0"

Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как IIS, Nginx или Apache. Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

Любая из этих конфигураций &mdash; с обратным прокси-сервером и без него &mdash; является допустимой и поддерживаемой для размещения основных компонентов приложений ASP.NET версии 2.0 и выше. Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Если приложение принимает запросы только от внутренней сети, можно использовать Kestrel сам по себе.

![Kestrel взаимодействует с внутренней сетью напрямую](kestrel/_static/kestrel-to-internal.png)

Если приложение имеет доступ к Интернету, Kestrel должен использовать IIS, Nginx или Apache как *обратный прокси-сервер*. Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки, как показано на следующей схеме.

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

Основная причина использования обратного прокси-сервера для развертывания пограничного сервера (открытого для Интернет-трафика) — это безопасность. Версии Kestrel 1.x не обладают важными функциями безопасности для защиты от атак из Интернета. Сюда входят, например, соответствующее время ожидания, предельные размеры запроса и максимальное количество одновременных подключений.

Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

IIS, Nginx или Apache нельзя использовать без Kestrel или какой-либо [реализации пользовательского сервера](#custom-servers). ASP.NET Core рассчитан на выполнение в качестве отдельного процесса, что позволяет ему на разных платформах вести себя одинаково. IIS, Nginx и Apache сами определяют свою процедуру запуска и среду. Чтобы использовать эти технологии сервера напрямую, ASP.NET Core придется адаптироваться к требованиям каждого сервера. Реализация веб-сервера, такого как Kestrel, позволяет ASP.NET Core контролировать процесс запуска и среду при размещении на различных технологиях сервера.

### <a name="iis-with-kestrel"></a>IIS с Kestrel

При использовании [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) в качестве обратного прокси-сервера для ASP.NET Core приложение ASP.NET Core выполняется отдельно от рабочего процесса IIS. В процессе IIS [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) координирует связь с обратным прокси-сервером. Основные функции модуля ASP.NET Core — это запуск приложения ASP.NET Core, его перезапуск в случае сбоя и перенаправление HTTP-трафика в это приложение. Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). 

### <a name="nginx-with-kestrel"></a>Nginx с Kestrel

Сведения о том, как использовать Nginx в Linux в качестве обратного прокси-сервера для Kestrel, см. в статье [Размещение в Linux с использованием Nginx](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Apache с Kestrel

Сведения о том, как использовать Apache в Linux в качестве обратного прокси-сервера для Kestrel, см. в статье [Размещение в Linux с использованием Apache](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

::: moniker range=">= aspnetcore-2.0"

Если приложение ASP.NET Core запускается в Windows, вместо Kestrel можно использовать HTTP.sys. Как правило, для оптимальной производительности рекомендуется Kestrel. HTTP.sys можно использовать в сценариях, где приложение имеет доступ к Интернету и необходимые возможности поддерживаются HTTP.sys, но не Kestrel. Сведения о HTTP.sys см. [здесь](xref:fundamentals/servers/httpsys).

![HTTP.sys взаимодействует с Интернетом напрямую](httpsys/_static/httpsys-to-internet.png)

HTTP.sys можно также использовать для приложений, которые имеют доступ только к внутренней сети.

![HTTP.sys взаимодействует с внутренней сетью напрямую](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

В ASP.NET Core 1.x HTTP.sys называется [WebListener](xref:fundamentals/servers/weblistener). Если приложения ASP.NET Core запускаются в Windows, WebListener можно использовать в случаях, когда служба IIS недоступна для размещения приложений.

![Weblistener взаимодействует с Интернетом напрямую.](weblistener/_static/weblistener-to-internet.png)

WebListener можно также использовать вместо Kestrel для приложений, которые имеют доступ только к внутренней сети, если вам требуются возможности, поддерживаемые WebListener, но не Kestrel. Сведения о WebListener см. [здесь](xref:fundamentals/servers/weblistener).

![Weblistener взаимодействует с внутренней сетью напрямую](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a>Инфраструктура сервера ASP.NET Core

[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) в методе `Startup.Configure` предоставляет свойство [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) типа [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel и HTTP.sys (WebListener в ASP.NET Core 1.x) предоставляют только один компонент, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), но разные реализации сервера могут предоставлять дополнительные возможности.

`IServerAddressesFeature` можно использовать для того, чтобы узнать, какой порт в реализации сервера привязан к среде выполнения.

## <a name="custom-servers"></a>Пользовательские серверы

Если встроенные серверы не отвечают требованиям приложения, можно создать реализацию пользовательского сервера. В [руководстве по открытому веб-интерфейсу .NET (OWIN)](xref:fundamentals/owin) демонстрируется запись реализации [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) на основе [Nowin](https://github.com/Bobris/Nowin). Требуют реализации только интерфейсы компонентов, используемых приложением, но как минимум должны поддерживаться [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) и [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).

## <a name="server-startup"></a>Запуск сервера

При использовании [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio для Mac](https://www.visualstudio.com/vs/mac/), или [Visual Studio Code](https://code.visualstudio.com/) сервер запускается при запуске приложения интегрированной средой разработки (IDE). В Visual Studio в Windows профили запуска можно использовать для запуска приложения и сервера с помощью [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) или консоли. В Visual Studio Code приложение и сервер запускаются [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), который активирует отладчик CoreCLR. При использовании Visual Studio для Mac приложение и сервер запускаются отладчиком [Mono Soft Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).

При запуске приложения из командной строки в папке проекта [dotnet run](/dotnet/core/tools/dotnet-run) запускает приложение и сервер (только Kestrel и HTTP.sys). Конфигурация определяется параметром `-c|--configuration`, который может принимать значение `Debug` (по умолчанию) или `Release`. Если профили запуска указаны в файле *launchSettings.json*, используйте параметр `--launch-profile <NAME>`, чтобы настроить профиль запуска (например, `Development` или `Production`). Дополнительные сведения см. в разделах [dotnet run](/dotnet/core/tools/dotnet-run) и [Упаковка дистрибутивов .NET Core](/dotnet/core/build/distribution-packaging).

## <a name="http2-support"></a>Поддержка HTTP/2

[HTTP/2](https://httpwg.org/specs/rfc7540.html) поддерживается в ASP.NET Core для следующих сценариев развертывания:

::: moniker range=">= aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel#http2-support)
  * Операционная система
    * Windows Server 2012 R2/Windows 8.1 или более поздней версии
    * Linux с OpenSSL 1.0.2 или более поздней версии (например, Ubuntu 16.04 или более поздней версии)
    * HTTP/2 будет поддерживаться для macOS в будущих выпусках.
  * Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней
* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016 / Windows 10 или более поздних версий
  * Требуемая версия .NET Framework: не применимо к развертываниям HTTP.sys.
* [IIS (внутрипроцессный)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии
  * Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней
* [IIS (внепроцессный)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии
  * Подключения Edge используют протокол HTTP/2, но подключения к Kestrel через обратный прокси-сервер выполняются по HTTP/1.1.
  * Требуемая версия .NET Framework: не применимо к внепроцессным развертываниям IIS.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016 / Windows 10 или более поздних версий
  * Требуемая версия .NET Framework: не применимо к развертываниям HTTP.sys.
* [IIS (внепроцессный)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии
  * Подключения Edge используют протокол HTTP/2, но подключения к Kestrel через обратный прокси-сервер выполняются по HTTP/1.1.
  * Требуемая версия .NET Framework: не применимо к внепроцессным развертываниям IIS.

::: moniker-end

Подключение HTTP/2 должно использовать [согласование протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) и TLS 1.2 или более поздней версии. Дополнительные сведения см. в разделах, о конкретных сценариях развертывания сервера.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys> (или <xref:fundamentals/servers/weblistener> для ASP.NET Core 1.x)
