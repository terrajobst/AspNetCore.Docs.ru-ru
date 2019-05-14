---
title: Принудительное использование HTTPS в ASP.NET Core
author: rick-anderson
description: Узнайте, как требовать HTTPS/TLS в веб-приложения ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: security/enforcing-ssl
ms.openlocfilehash: e27e0c31b128cbd7d71bf7b83a2d33cc89ea3ab1
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610428"
---
# <a name="enforce-https-in-aspnet-core"></a>Принудительное использование HTTPS в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом документе показано, как:

* Требование HTTPS для всех запросов.
* Перенаправлять все запросы HTTP на HTTPS.

Интерфейс API может препятствовать передачи конфиденциальных данных при первом запросе клиента.

> [!WARNING]
> Сделать **не** использовать [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) на веб-API, получения конфиденциальной информации. `RequireHttpsAttribute` использует коды состояния HTTP для перенаправления браузеров с HTTP на HTTPS. Клиенты API не может понять и подчиняются перенаправление с HTTP на HTTPS. Такие клиенты могут отправлять данные по протоколу HTTP. Веб-API должен:
>
> * Прослушивает HTTP.
> * Закрыть соединение с кодом состояния 400 (неправильный запрос) и не обработать запрос.

## <a name="require-https"></a>Требование к использованию протокола HTTPS

::: moniker range=">= aspnetcore-2.1"

Мы рекомендуем производственные ASP.NET Core, веб-приложений вызова:

* По промежуточного слоя переадресации HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) для перенаправления запросов HTTP на HTTPS.
* По промежуточного слоя HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) для отправки заголовков HTTP Strict транспорта безопасности протокола (HSTS) клиентам.

> [!NOTE]
> Приложения, развернутые в конфигурации обратного прокси-сервера разрешить прокси-сервер для обработки безопасность подключения (HTTPS). Если прокси-сервер также обрабатывает перенаправления HTTPS, нет необходимости использовать по промежуточного слоя переадресации HTTPS. Если прокси-сервера также обрабатывает операции записи HSTS заголовков (например, [собственного HSTS поддержки в IIS 10.0 (1709) или более поздней версии](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), по промежуточного слоя HSTS не требуется приложением. Дополнительные сведения см. в разделе [выхода из HTTPS/HSTS при создании проекта](#opt-out-of-httpshsts-on-project-creation).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

Следующий код вызывает `UseHttpsRedirection` в `Startup` класса:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Выделенный выше код:

* Использует значение по умолчанию [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).
* Использует значение по умолчанию [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), если не переопределено `ASPNETCORE_HTTPS_PORT` переменной среды или [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

Мы рекомендуем использовать временный перенаправления, а не постоянной переадресации. Кэширование ссылок может привести к нестабильной работе поведению в средах разработки. Если вы хотите отправить код состояния постоянное перенаправление, во время работы приложения в среде не разработки, см. в разделе [Настройка постоянной переадресации в рабочей среде](#configure-permanent-redirects-in-production) раздел. Мы рекомендуем использовать [HSTS](#http-strict-transport-security-protocol-hsts) сигнал для клиентов, которые только защитить ресурс запросы следует отправлять в приложение (только в рабочей среде).

### <a name="port-configuration"></a>Конфигурация порта

Порт должен быть доступен для по промежуточного слоя для перенаправления небезопасный запрос HTTPS. Если порт не доступен:

* Не произойдет перенаправление на HTTPS.
* По промежуточного слоя записывает предупреждение «Не удалось определить https-порт для перенаправления.»

Укажите порт HTTPS, с помощью любого из следующих подходов:

* Задайте [HttpsRedirectionOptions.HttpsPort](#options).
* Задайте `ASPNETCORE_HTTPS_PORT` переменной среды или [параметр конфигурации веб-узел https_port](xref:fundamentals/host/web-host#https-port):

  **Ключ**: `https_port`  
  **Тип**: *string*  
  **По умолчанию**: значение по умолчанию не задано.  
  **Задается с помощью**: `UseSetting`  
  **Переменная среды**: `<PREFIX_>HTTPS_PORT` (Используется префикс `ASPNETCORE_` при использовании [веб-узел](xref:fundamentals/host/web-host).)

  При настройке <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> в `Program`:

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* Указать порт с безопасной схемы с помощью `ASPNETCORE_URLS` переменной среды. Переменная среды настраивает сервер. По промежуточного слоя косвенно обнаруживает HTTPS-порт, через <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>. Этот подход не работает в развертываниях обратного прокси-сервера.
* В разработке, задать URL-адрес HTTPS в *launchsettings.json*. Включите протокол HTTPS, если используется IIS Express.
* Настроить конечную точку HTTPS URL-адрес для развертывания общедоступных edge [Kestrel](xref:fundamentals/servers/kestrel) сервера или [HTTP.sys](xref:fundamentals/servers/httpsys) сервера. Только **один порт HTTPS** используется приложением. По промежуточного слоя обнаруживает порт с помощью <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.

> [!NOTE]
> Когда приложение выполняется в конфигурации обратного прокси-сервера, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> недоступна. Задайте номер порта, с помощью одного из других подходов, описанных в этом разделе.

Если Kestrel и HTTP.sys используется в качестве общедоступных пограничного сервера, Kestrel и HTTP.sys должны быть настроены на прослушивание оба:

* Безопасного порта, где перенаправляется клиент (как правило, 443 в рабочей среде и 5001 в разработке).
* Небезопасный порт (как правило, 80 в рабочей среде) до 5000 при разработке.

Небезопасный порт должен быть доступны клиенту в порядке для приложения для получения небезопасный запрос и перенаправления клиента для безопасного порта.

Дополнительные сведения см. в разделе [конфигурации конечной точки Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) или <xref:fundamentals/servers/httpsys>.

### <a name="deployment-scenarios"></a>Сценарии развертывания

Любой брандмауэра между клиентом и сервером также должны быть открыты для трафика порты связи.

Если запросы перенаправляются в конфигурации обратного прокси-сервера, используйте [пересылаемые по промежуточного слоя заголовки](xref:host-and-deploy/proxy-load-balancer) перед вызовом по промежуточного слоя переадресации HTTPS. Пересылаемые обновлений по промежуточного слоя заголовки `Request.Scheme`, с использованием `X-Forwarded-Proto` заголовка. По промежуточного слоя позволяет перенаправлять URI и других политик безопасности, для правильной работы. При пересылаемые по промежуточного слоя заголовки не используется, во внутреннее приложение может не получать правильные схему и цикл перенаправления. Сообщение об ошибке распространенных конечного пользователя — в том, что произошло слишком много перенаправлений.

При развертывании в службе приложений Azure, следуйте указаниям в [руководства: Привязывание существующего настраиваемого SSL-сертификата к веб-приложениям Azure](/azure/app-service/app-service-web-tutorial-custom-ssl).

### <a name="options"></a>Параметры

Следующий выделенный код вызывает метод [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) настроить параметры по промежуточного слоя:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Вызов `AddHttpsRedirection` необходимо изменить значения `HttpsPort` или `RedirectStatusCode`.

Выделенный выше код:

* Наборы [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) для <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, который является значением по умолчанию. Используйте поля <xref:Microsoft.AspNetCore.Http.StatusCodes> класс для назначений `RedirectStatusCode`.
* Задать HTTPS-порт на 5001. Значение по умолчанию — 443.

#### <a name="configure-permanent-redirects-in-production"></a>Настройка постоянной переадресации в рабочей среде

По умолчанию по промежуточного слоя на отправку [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) с все операции перенаправления. Если вы хотите отправить код состояния постоянное перенаправление, во время работы приложения в среде не разработки, поместите параметры конфигурации по промежуточного слоя в условную проверку на среду не разработки.

При настройке `IWebHostBuilder` в *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

## <a name="https-redirection-middleware-alternative-approach"></a>Альтернативный подход по промежуточного слоя переадресации HTTPS

Альтернативой использованию по промежуточного слоя переадресации HTTPS (`UseHttpsRedirection`) является использование по промежуточного слоя для переопределения URL-адрес (`AddRedirectToHttps`). `AddRedirectToHttps` Можно также задать код состояния и порт при выполнении перенаправления. Дополнительные сведения см. в разделе [по промежуточного слоя для переопределения URL-адрес](xref:fundamentals/url-rewriting).

При перенаправлении HTTPS без необходимости для перенаправления дополнительных правил, мы рекомендуем использовать по промежуточного слоя переадресации HTTPS (`UseHttpsRedirection`) описано в этом разделе.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется для обязательного использования протокола HTTPS. `[RequireHttpsAttribute]` можно снабдить контроллеров или методы, или могут применяться глобально. Чтобы применить атрибут глобально, добавьте следующий код, чтобы `ConfigureServices` в `Startup`:

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Выделенный выше код требует использовать все запросы `HTTPS`; таким образом, HTTP-запросы учитываются. Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Дополнительные сведения см. в разделе [по промежуточного слоя для переопределения URL-адрес](xref:fundamentals/url-rewriting). По промежуточного слоя также позволяет приложению задать код состояния или код состояния и номер порта, если выполняется перенаправление.

Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) — это рекомендации по безопасности. Применение `[RequireHttps]` атрибута ко всем страницам контроллеров/Razor не считается так безопасен, как требования HTTPS глобально. Не может гарантировать `[RequireHttps]` атрибут применяется, когда добавляются новые контроллеры и Razor Pages.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a>Безопасность строгой транспортный протокол HTTP (HSTS)

На [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP строгой безопасности транспорта (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) представляет собой улучшение центра безопасности, который задается параметром веб-приложения при помощи заголовка ответа. Когда [браузера, поддерживающего HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) получения этого заголовка:

* Браузер хранит конфигурацию для домена, который предотвращает отправку любые данные, передаваемые по протоколу HTTP. Браузер заставляет весь обмен данными по протоколу HTTPS.
* Браузер запрещает пользователю с помощью недействителен или сертификаты. Браузер отключает подсказок, которые позволяют пользователю временно доверять такой сертификат.

Поскольку HSTS обеспечивается с помощью клиента он имеет некоторые ограничения:

* Клиент должен поддерживать HSTS.
* HSTS требуется по крайней мере один успешный запрос HTTPS для задания политики HSTS.
* Приложение должно проверить каждый HTTP-запрос и перенаправления или отклонить запрос HTTP.

ASP.NET Core 2.1 или более поздних версий реализует HSTS с `UseHsts` метода расширения. Следующий код вызывает `UseHsts` когда приложение не [режим разработки](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` не рекомендуется в разработке, так как параметры HSTS высокой кэшируемых обозревателями. По умолчанию `UseHsts` исключает локальный петлевой адрес.

Для рабочих сред реализации HTTPS в первый раз установить начального [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) на малое значение, с помощью одного из <xref:System.TimeSpan> методы. Задайте значение от часов не больше, чем в один день в случае необходимости использования инфраструктуры HTTPS-HTTP. После вы уверены в устойчивости конфигурации HTTPS, увеличьте значение max-age HSTS; часто используемые значение — один год.

В приведенном ниже коде

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Задает параметр предварительной загрузки заголовок Strict-Transport-Security. Предварительной загрузки не входит в состав [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается веб-браузеры для предварительной загрузки HSTS узлов на установку обновления. Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).
* Позволяет [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS для размещения таких поддоменов.
* Явно задает параметр max-age заголовок Strict-Transport-Security 60 дней. Если не установлено значение по умолчанию — 30 дней. См. в разделе [директива максимального](https://tools.ietf.org/html/rfc6797#section-6.1.1) Дополнительные сведения.
* Добавляет `example.com` в список узлов для исключения.

`UseHsts` исключает следующими узлами замыкания на себя:

* `localhost`: IPv4-адрес замыкания на себя.
* `127.0.0.1`: IPv4-адрес замыкания на себя.
* `[::1]`: Петлевой адрес IPv6.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a>Отказаться от HTTPS/HSTS при создании проекта

В некоторых сценариях службы серверной части, где безопасность подключения обрабатывается на границе общедоступные сети Настройка безопасности подключения на каждом узле не требуется. Веб-приложений, созданных из шаблонов в Visual Studio или из [команды dotnet new](/dotnet/core/tools/dotnet-new) команду enable [перенаправления HTTPS](#require-https) и [HSTS](#http-strict-transport-security-protocol-hsts). Для развертываний, которые не требуют этих сценариев вы можете отказаться от HTTPS/HSTS создания приложения на основе шаблона.

Чтобы отказаться от HTTPS/HSTS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Снимите флажок **настроить для использования протокола HTTPS** "флажок".

![Новое диалоговое окно веб-приложение ASP.NET Core, показывающий настройки для HTTPS флажок снят.](enforcing-ssl/_static/out.png)

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli) 

Использовать параметр `--no-https`. Пример

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a>Доверять сертификату разработки ASP.NET Core HTTPS в Windows и macOS

Пакет SDK для .NET core включает в себя HTTPS-сертификат разработки. Сертификат установлен как часть при первом запуске. Например `dotnet --info` выходные данные выглядят примерно следующим:

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

При установке пакета SDK для .NET Core в локальное хранилище сертификатов пользователя устанавливается сертификат разработки HTTPS ASP.NET Core. Сертификат был установлен, но он не является доверенным. Доверие сертификата выполнить однократное действие для выполнения dotnet `dev-certs` средство:

```console
dotnet dev-certs https --trust
```

Следующая команда вызывает справку по средству `dev-certs`.

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Как настроить сертификат разработчика для Docker

См. в разделе [проблема GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6199).

::: moniker-end

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a>Доверять сертификату HTTPS из подсистемы Windows для Linux

Подсистема Windows для Linux (WSL) создает самозаверяющий сертификат HTTPS. Чтобы настроить хранилище сертификатов Windows, чтобы доверять сертификату WSL:

* Выполните следующую команду, чтобы экспортировать сертификат создан WSL: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`
* В окне WSL выполните следующую команду: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`

  Приведенная выше команда устанавливает переменные среды, поэтому Linux использует доверенных сертификатов Windows.

## <a name="additional-information"></a>Дополнительные сведения

* <xref:host-and-deploy/proxy-load-balancer>
* [Размещение ASP.NET Core в Linux с использованием Apache: Конфигурация HTTPS](xref:host-and-deploy/linux-apache#https-configuration)
* [Размещение ASP.NET Core в Linux с Nginx: Конфигурация HTTPS](xref:host-and-deploy/linux-nginx#https-configuration)
* [Настройка SSL в IIS](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [Поддержка браузеров OWASP HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
