---
title: Реализации веб-сервера в ASP.NET Core
author: guardrex
description: Откройте возможности веб-серверов Kestrel и HTTP.sys для ASP.NET Core. Рекомендации по выбору сервера и сведения о сценариях использования обратного прокси-сервера.
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/servers/index
---
# <a name="web-server-implementations-in-aspnet-core"></a>Реализации веб-сервера в ASP.NET Core

Авторы: [Том Дисктра](https://github.com/tdykstra) (Tom Dykstra), [Стив Смит](https://ardalis.com/) (Steve Smith), [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)

Приложение ASP.NET Core выполняется вместе с внутрипроцессной реализацией HTTP-сервера. Реализация сервера прослушивает HTTP-запросы и передает их в приложение как набор [функций запросов](xref:fundamentals/request-features), объединенных в <xref:Microsoft.AspNetCore.Http.HttpContext>.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

В состав ASP.NET Core входит следующее:

* Сервер [Kestrel](xref:fundamentals/servers/kestrel) — это реализация кроссплатформенного HTTP-сервера по умолчанию.
* HTTP-сервер IIS — это [внутрипроцессный сервер для службы IIS](#in-process-hosting-model).
* [Сервер HTTP.sys](xref:fundamentals/servers/httpsys) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра HTTP.sys и API HTTP-сервера](/windows/desktop/Http/http-api-start-page).

При использовании [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) приложение запускается одним из следующих способов:

* В том же процессе, что и рабочий процесс IIS ([модель внутрипроцессного размещения](#in-process-hosting-model)), с использованием [HTTP-сервера IIS](#iis-http-server). *Внутрипроцессное размещение* является рекомендуемой конфигурацией.
* В процессе отдельно от рабочего процесса IIS ([модель внепроцессного размещения](#out-of-process-hosting-model)) с использованием [сервера Kestrel](#kestrel).

[Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) — это собственный модуль IIS, который обрабатывает собственные запросы IIS между службами IIS и HTTP-сервером IIS (внутрипроцессно) или сервером Kestrel. Для получения дополнительной информации см. <xref:host-and-deploy/aspnet-core-module>.

## <a name="hosting-models"></a>Модели размещения

### <a name="in-process-hosting-model"></a>Модель внутрипроцессного размещения

При внутрипроцессном размещении приложение ASP.NET Core выполняется в том же процессе, что и рабочий процесс IIS. При этом повышается производительность по сравнению с внепроцессным размещением, так как запросы не передаются через адаптер замыкания на себя (сетевой интерфейс, который возвращает исходящий сетевой трафик на тот же компьютер). IIS обрабатывает управление процессом с помощью [службы активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Модуль ASP.NET Core:

* Инициализирует приложение.
  * Загружает [CoreCLR](/dotnet/standard/glossary#coreclr).
  * Вызывает `Program.Main`.
* Управляет жизненным циклом собственного запроса IIS.

Модель внутрипроцессного размещения не поддерживается для приложений ASP.NET Core, предназначенных для .NET Framework.

На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением, размещенным внутри процесса.

![Модуль ASP.NET Core](_static/ancm-inprocess.png)

Запрос поступает из Интернета в драйвер HTTP.sys в режиме ядра. Драйвер направляет собственный запрос к IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS). Модуль получает собственный запрос и передает его на HTTP-сервер IIS (`IISHttpServer`). HTTP-сервер IIS — это реализация внутрипроцессного сервера для IIS, в которой запрос преобразовывается из собственной формы в управляемую.

После того как HTTP-сервер IIS обрабатывает запрос, он передается в конвейер ПО промежуточного слоя ASP.NET Core. Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения. Ответ приложения передается обратно в службы IIS через HTTP-сервер IIS. IIS отправляет ответ клиенту, который инициировал запрос.

Внутрипроцессное размещение необходимо явно выбирать в существующих приложениях, но в шаблонах [dotnet new](/dotnet/core/tools/dotnet-new) оно включено по умолчанию для всех сценариев IIS и IIS Express.

### <a name="out-of-process-hosting-model"></a>Модель размещения вне процесса

Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль обрабатывает управление процессами. Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое или завершении работы. Это, по сути, совпадает с поведением приложений, выполняемых внутрипроцессно и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением, размещенным вне процесса.

![Модуль ASP.NET Core](_static/ancm-outofprocess.png)

Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра. Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS). Модуль перенаправляет запросы Kestrel на случайный порт для приложения, отличающийся от порта 80 или 443.

Модуль задает порт с помощью переменной среды во время запуска, а ПО промежуточного слоя для интеграции IIS настраивает сервер для прослушивания `http://localhost:{PORT}`. Выполняются дополнительные проверки, и запросы не из модуля отклоняются. Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.

После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core. Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения. ПО промежуточного слоя, добавленное интеграцией IIS, обновляет схему, удаленный IP-адрес и базовый путь для переадресации запроса в Kestrel. Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.

Инструкции по настройке модуля ASP.NET Core и IIS см. в следующих статьях:

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

В состав ASP.NET Core входит следующее:

* Сервер [Kestrel](xref:fundamentals/servers/kestrel) — кроссплатформенный HTTP-сервер по умолчанию.
* [Сервер HTTP.sys](xref:fundamentals/servers/httpsys) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра HTTP.sys и API HTTP-сервера](/windows/desktop/Http/http-api-start-page).

При использовании [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) приложение запускается отдельно от рабочего процесса IIS (*внепроцессно*) с [сервером Kestrel](#kestrel).

Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль обрабатывает управление процессами. Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое или завершении работы. Это, по сути, совпадает с поведением приложений, выполняемых внутрипроцессно и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложением, размещенным вне процесса.

![Модуль ASP.NET Core](_static/ancm-outofprocess.png)

Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра. Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS). Модуль перенаправляет запросы Kestrel на случайный порт для приложения, отличающийся от порта 80 или 443.

Модуль задает порт с помощью переменной среды во время запуска, а ПО промежуточного слоя для интеграции IIS настраивает сервер для прослушивания `http://localhost:{port}`. Выполняются дополнительные проверки, и запросы не из модуля отклоняются. Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.

После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core. Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения. ПО промежуточного слоя, добавленное интеграцией IIS, обновляет схему, удаленный IP-адрес и базовый путь для переадресации запроса в Kestrel. Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.

Инструкции по настройке модуля ASP.NET Core и IIS см. в следующих статьях:

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.

---

::: moniker-end

## <a name="kestrel"></a>Kestrel

Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.

::: moniker range=">= aspnetcore-2.0"

Kestrel можно использовать:

* Сам по себе как пограничный сервер обработки запросов непосредственно из сети, включая Интернет.

  ![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

* С *обратным прокси-сервером*, таким как [службы IIS](https://www.iis.net/), [Nginx](http://nginx.org) или [Apache](https://httpd.apache.org/). Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel.

  ![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

Любая из этих конфигураций размещения&mdash;с обратным прокси-сервером и без него&mdash; поддерживается для приложений ASP.NET Core версии 2.1 и выше.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Если приложение принимает запросы только от внутренней сети, можно использовать Kestrel сам по себе.

![Kestrel взаимодействует с внутренней сетью напрямую](kestrel/_static/kestrel-to-internal.png)

Если приложение имеет доступ к Интернету, Kestrel должен использовать *обратный прокси-сервер*, такой как [службы IIS](https://www.iis.net/), [Nginx](http://nginx.org) или [Apache](https://httpd.apache.org/). Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel.

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

Основная причина использования обратного прокси-сервера для развертываний пограничных серверов, открытых для общего доступа, — это безопасность. Версии Kestrel 1.x не обладают важными функциями безопасности для защиты от атак из Интернета. Сюда входят, например, соответствующее время ожидания, предельные размеры запроса и максимальное количество одновременных подключений.

::: moniker-end

Инструкции по настройке Kestrel и сведения о том, когда следует использовать Kestrel в конфигурации обратного прокси-сервера, см. в статье <xref:fundamentals/servers/kestrel>.

### <a name="nginx-with-kestrel"></a>Nginx с Kestrel

Сведения о том, как использовать Nginx в Linux в качестве обратного прокси-сервера для Kestrel, см. в <xref:host-and-deploy/linux-nginx>.

### <a name="apache-with-kestrel"></a>Apache с Kestrel

Сведения о том, как использовать Apache в Linux в качестве обратного прокси-сервера для Kestrel, см. в <xref:host-and-deploy/linux-apache>.

::: moniker range=">= aspnetcore-2.2"

## <a name="iis-http-server"></a>HTTP-сервер IIS

HTTP-сервер IIS — это [внутрипроцессный сервер](#in-process-hosting-model) для служб IIS, который требуется для внутрипроцессных развертываний. [Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) обрабатывает собственные запросы IIS между HTTP-сервером IIS и службами IIS. Для получения дополнительной информации см. <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

## <a name="httpsys"></a>HTTP.sys

Если приложение ASP.NET Core запускается в Windows, вместо Kestrel можно использовать HTTP.sys. Как правило, для оптимальной производительности рекомендуется Kestrel. HTTP.sys можно использовать в сценариях, где приложение имеет доступ к Интернету и необходимые возможности поддерживаются HTTP.sys, но не Kestrel. Для получения дополнительной информации см. <xref:fundamentals/servers/httpsys>.

![HTTP.sys взаимодействует с Интернетом напрямую](httpsys/_static/httpsys-to-internet.png)

HTTP.sys можно также использовать для приложений, которые имеют доступ только к внутренней сети.

![HTTP.sys взаимодействует с внутренней сетью напрямую](httpsys/_static/httpsys-to-internal.png)

Инструкции по настройке HTTP.sys см. в статье <xref:fundamentals/servers/httpsys>.

## <a name="aspnet-core-server-infrastructure"></a>Инфраструктура сервера ASP.NET Core

<xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>, доступный в методе `Startup.Configure`, предоставляет свойство <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> типа <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>. Kestrel и HTTP.sys предоставляют только один компонент, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>, но разные реализации сервера могут предоставлять дополнительные возможности.

`IServerAddressesFeature` можно использовать для того, чтобы узнать, какой порт в реализации сервера привязан к среде выполнения.

## <a name="custom-servers"></a>Пользовательские серверы

Если встроенные серверы не отвечают требованиям приложения, можно создать реализацию пользовательского сервера. В [руководстве по открытому веб-интерфейсу .NET (OWIN)](xref:fundamentals/owin) демонстрируется запись реализации <xref:Microsoft.AspNetCore.Hosting.Server.IServer> на основе [Nowin](https://github.com/Bobris/Nowin). Требуют реализации только интерфейсы компонентов, используемых приложением, но как минимум должны поддерживаться <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> и <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature>.

## <a name="server-startup"></a>Запуск сервера

Сервер запускается, когда интегрированная среда разработки (IDE) или редактор запускает приложение:

* [Visual Studio](https://www.visualstudio.com/vs/) — профили запуска можно использовать для запуска приложения и сервера с помощью [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) или консоли.
* [Visual Studio Code](https://code.visualstudio.com/) — приложение и сервер запускаются решением [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), которое активирует отладчик CoreCLR.
* [Visual Studio для Mac](https://www.visualstudio.com/vs/mac/) — приложение и сервер запускаются отладчиком [Mono Soft Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).

При запуске приложения из командной строки в папке проекта [dotnet run](/dotnet/core/tools/dotnet-run) запускает приложение и сервер (только Kestrel и HTTP.sys). Конфигурация определяется параметром `-c|--configuration`, который может принимать значение `Debug` (по умолчанию) или `Release`. Если профили запуска указаны в файле *launchSettings.json*, используйте параметр `--launch-profile <NAME>`, чтобы настроить профиль запуска (например, `Development` или `Production`). Дополнительные сведения см. в статьях [dotnet run](/dotnet/core/tools/dotnet-run) и [Упаковка дистрибутивов .NET Core](/dotnet/core/build/distribution-packaging).

## <a name="http2-support"></a>Поддержка HTTP/2

[HTTP/2](https://httpwg.org/specs/rfc7540.html) поддерживается в ASP.NET Core для следующих сценариев развертывания:

::: moniker range=">= aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel#http2-support)
  * Операционная система
    * Windows Server 2016 / Windows 10 или более поздних версий&dagger;
    * Linux с OpenSSL 1.0.2 или более поздней версии (например, Ubuntu 16.04 или более поздней версии).
    * HTTP/2 будет поддерживаться для macOS в будущих выпусках.
  * Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней
* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016 / Windows 10 или более поздних версий
  * Целевая платформа: неприменимо к развертываниям HTTP.sys.
* [IIS (внутрипроцессный)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии
  * Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней
* [IIS (внепроцессный)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии
  * Подключения к пограничным серверам, открытых для общего доступа, выполняются по протоколу HTTP/2, а подключения к серверу Kestrel через обратный прокси-сервер — по протоколу HTTP/1.1.
  * Целевая платформа: неприменимо к внепроцессным развертываниям IIS.

&dagger;Для Kestrel предусмотрена ограниченная поддержка HTTP/2 в Windows Server 2012 R2 и Windows 8.1. Поддержка ограничена из-за небольшого числа поддерживаемых комплектов шифров TLS, доступных для этих операционных систем. Для обеспечения безопасности TLS-подключений может потребоваться сертификат, созданный с использованием алгоритма ECDSA.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016 / Windows 10 или более поздних версий
  * Целевая платформа: неприменимо к развертываниям HTTP.sys.
* [IIS (внепроцессный)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии
  * Подключения к пограничным серверам, открытых для общего доступа, выполняются по протоколу HTTP/2, а подключения к серверу Kestrel через обратный прокси-сервер — по протоколу HTTP/1.1.
  * Целевая платформа: неприменимо к внепроцессным развертываниям IIS.

::: moniker-end

Подключение HTTP/2 должно использовать [согласование протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) и TLS 1.2 или более поздней версии. Дополнительные сведения см. в разделах, о конкретных сценариях развертывания сервера.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/servers/kestrel>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys>
