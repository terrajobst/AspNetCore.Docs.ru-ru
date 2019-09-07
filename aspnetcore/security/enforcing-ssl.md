---
title: Принудительное применение HTTPS в ASP.NET Core
author: rick-anderson
description: Узнайте, как требовать использования HTTPS/TLS в ASP.NET Core веб-приложении.
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: security/enforcing-ssl
ms.openlocfilehash: 654b083a0dade2fc8df5cccf9fa434f30627794b
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773963"
---
# <a name="enforce-https-in-aspnet-core"></a>Принудительное применение HTTPS в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом документе показано, как:

* Требовать протокол HTTPS для всех запросов.
* Перенаправляет все запросы HTTP на HTTPS.

Ни один API не может предотвратить отправку конфиденциальных данных клиентом при первом запросе.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a>Проекты API
>
> **Не** используйте [Рекуирехттпсаттрибуте](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) в веб-API, которые получают конфиденциальную информацию. `RequireHttpsAttribute`использует коды состояния HTTP для перенаправления браузеров из HTTP в HTTPS. Клиенты API могут не распознать или соблюдать перенаправления от HTTP к HTTPS. Такие клиенты могут передавать данные по протоколу HTTP. Веб-API должны быть:
>
> * Не прослушивать по протоколу HTTP.
> * Закройте подключение с кодом состояния 400 (недопустимый запрос) и не обслуживает запрос.
>
> ## <a name="hsts-and-api-projects"></a>Проекты HSTS и API
>
> Проекты API по умолчанию не включают [HSTS](#hsts) , так как HSTS обычно является инструкцией только браузера. Другие вызывающие объекты, такие как приложения для телефонов или настольных систем, **не** подчиняются инструкции. Даже в обозревателях, один вызов API через HTTP, прошедший проверку подлинности, имеет риски в незащищенных сетях. Безопасный подход заключается в настройке проектов API для прослушивания и реагирования по протоколу HTTPS.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

> [!WARNING]
> ## <a name="api-projects"></a>Проекты API
>
> **Не** используйте [Рекуирехттпсаттрибуте](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) в веб-API, которые получают конфиденциальную информацию. `RequireHttpsAttribute`использует коды состояния HTTP для перенаправления браузеров из HTTP в HTTPS. Клиенты API могут не распознать или соблюдать перенаправления от HTTP к HTTPS. Такие клиенты могут передавать данные по протоколу HTTP. Веб-API должны быть:
>
> * Не прослушивать по протоколу HTTP.
> * Закройте подключение с кодом состояния 400 (недопустимый запрос) и не обслуживает запрос.

::: moniker-end

## <a name="require-https"></a>Требование к использованию протокола HTTPS

В рабочей ASP.NET Core веб-приложений рекомендуется использовать:

* По промежуточного слоя перенаправления<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>HTTPS () для перенаправления HTTP-запросов к HTTPS.
* HSTS по промежуточного слоя ([усехстс](#http-strict-transport-security-protocol-hsts)) для отправки клиентам заголовков протокола безопасности транспорта HTTP (HSTS).

> [!NOTE]
> Приложения, развернутые в конфигурации обратных прокси-серверов, позволяют прокси-серверу управлять безопасностью подключения (HTTPS). Если прокси-сервер также обрабатывает перенаправление HTTPS, нет необходимости использовать по промежуточного слоя перенаправления HTTPS. Если прокси-сервер также обрабатывает запись заголовков HSTS (например, [поддержка собственного HSTS в IIS 10,0 (1709) или более поздней версии](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), по промежуточного слоя HSTS не требуется для приложения. Дополнительные сведения см. [в разделе отказ от HTTPS/HSTS при создании проекта](#opt-out-of-httpshsts-on-project-creation).

### <a name="usehttpsredirection"></a>усехттпсредиректион

Следующий код вызывает `UseHttpsRedirection` `Startup` в классе:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=13)]

::: moniker-end

Предыдущий выделенный код:

* Использует значение по умолчанию [хттпсредиректионоптионс. редиректстатускоде](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).
* Использует значение по умолчанию [хттпсредиректионоптионс. HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), если оно `ASPNETCORE_HTTPS_PORT` не переопределено переменной среды или [исервераддрессесфеатуре](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

Рекомендуется использовать временные перенаправления, а не постоянные перенаправления. Кэширование ссылок может привести к нестабильной работе в средах разработки. Если вы предпочитаете отправить код состояния с постоянным перенаправлением, когда приложение находится в среде, не являющейся средой разработки, см. раздел [Настройка постоянных перенаправлений в производстве](#configure-permanent-redirects-in-production) . Мы рекомендуем использовать [HSTS](#http-strict-transport-security-protocol-hsts) для передачи клиентам, что только защищенные запросы ресурсов должны отправляться в приложение (только в рабочей среде).

### <a name="port-configuration"></a>Конфигурация порта

Порт должен быть доступен для промежуточного слоя, чтобы перенаправить незащищенный запрос на HTTPS. Если порт недоступен:

* Перенаправление на HTTPS не выполняется.
* По промежуточного слоя регистрируется предупреждение "не удалось определить HTTPS порт для перенаправления".

Укажите HTTPS порт, используя любой из следующих подходов:

* Задайте [хттпсредиректионоптионс. HttpsPort](#options).

::: moniker range=">= aspnetcore-3.0"

* Задайте параметр [узла:](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port) `https_port`

  * В конфигурации узла.
  * Путем задания `ASPNETCORE_HTTPS_PORT` переменной среды.
  * Путем добавления записи верхнего уровня в *appSettings. JSON*:

    [!code-json[](enforcing-ssl/sample-snapshot/3.x/appsettings.json?highlight=2)]

* Укажите порт с защитой схемы с помощью [переменной среды ASPNETCORE_URLS](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls). Переменная среды настраивает сервер. По промежуточного слоя косвенно обнаруживает HTTPS порт с <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>помощью. Этот подход не работает в обратных развертываниях прокси-сервера.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

* Задайте параметр [узла:](xref:fundamentals/host/web-host#https-port) `https_port`

  * В конфигурации узла.
  * Путем задания `ASPNETCORE_HTTPS_PORT` переменной среды.
  * Путем добавления записи верхнего уровня в *appSettings. JSON*:

    [!code-json[](enforcing-ssl/sample-snapshot/2.x/appsettings.json?highlight=2)]

* Укажите порт с защитой схемы с помощью [переменной среды ASPNETCORE_URLS](xref:fundamentals/host/web-host#server-urls). Переменная среды настраивает сервер. По промежуточного слоя косвенно обнаруживает HTTPS порт с <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>помощью. Этот подход не работает в обратных развертываниях прокси-сервера.

::: moniker-end

* В среде разработки задайте URL-адрес HTTPS в *launchsettings. JSON*. При использовании IIS Express включите протокол HTTPS.

* Настройте конечную точку HTTPS URL-адреса для общедоступного пограничной развертывания сервера [Kestrel](xref:fundamentals/servers/kestrel) или [http. sys](xref:fundamentals/servers/httpsys) . Приложение использует только **один HTTPS-порт** . По промежуточного слоя обнаруживает порт через <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.

> [!NOTE]
> Если приложение запускается в конфигурации обратного прокси-сервера, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> оно недоступно. Задайте порт с помощью одного из других подходов, описанных в этом разделе.

### <a name="edge-deployments"></a>Развертывания пограничных устройств 

Если Kestrel или HTTP. sys используется в качестве общедоступного пограничной сервера, то Kestrel или HTTP. sys должны быть настроены на прослушивание обоих типов:

* Безопасный порт, на который перенаправляется клиент (как правило, 443 в рабочей среде и 5001 в разработке).
* Небезопасный порт (как правило, 80 в рабочей среде и 5000 в разработке).

Клиент должен получить доступ к незащищенному порту, чтобы приложение получило незащищенный запрос и перенаправлять клиент на безопасный порт.

Дополнительные сведения см. в разделе [Kestrel Endpoint Configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) или <xref:fundamentals/servers/httpsys>.

### <a name="deployment-scenarios"></a>Сценарии развертывания

Все брандмауэры между клиентом и сервером также должны иметь открытые порты связи для трафика.

Если запросы перенаправляются в конфигурации обратных прокси-серверов, используйте [промежуточный слой перенаправленных заголовков](xref:host-and-deploy/proxy-load-balancer) перед вызовом по промежуточного слоя перенаправления HTTPS. По промежуточного слоя перенаправленных заголовков обновляет `Request.Scheme`, `X-Forwarded-Proto` используя заголовок. По промежуточного слоя позволяет правильно работать с URI перенаправления и другими политиками безопасности. Если по промежуточного слоя перенаправленных заголовков не используется, серверное приложение может не получить правильную схему и завершить цикл перенаправления. Обычное сообщение об ошибке конечного пользователя состоит в том, что произошло слишком много перенаправлений.

При развертывании в службе приложений Azure следуйте указаниям в [учебнике: Привязывание существующего настраиваемого SSL-сертификата к веб-приложениям Azure](/azure/app-service/app-service-web-tutorial-custom-ssl).

### <a name="options"></a>Параметры

Следующий выделенный код вызывает [аддхттпсредиректион](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) для настройки параметров промежуточного слоя:


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end


Вызов `AddHttpsRedirection` необходим только для изменения `HttpsPort` значений или `RedirectStatusCode`.

Предыдущий выделенный код:

* Устанавливает [хттпсредиректионоптионс. редиректстатускоде](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) в <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>значение, которое является значением по умолчанию. Используйте поля <xref:Microsoft.AspNetCore.Http.StatusCodes> класса для назначений в `RedirectStatusCode`.
* Устанавливает HTTPS порт 5001. Значение по умолчанию — 443.

#### <a name="configure-permanent-redirects-in-production"></a>Настройка постоянных перенаправлений в рабочей среде

По промежуточного слоя по умолчанию отправляется [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) со всеми перенаправлениями. Если вы предпочитаете отправить код состояния с постоянным перенаправлением, когда приложение находится в среде, не являющейся средой разработки, заключите конфигурацию параметров по промежуточного слоя в условную проверку для среды без разработки.

::: moniker range=">= aspnetcore-3.0"

При настройке служб в *Startup.CS*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IWebHostEnvironment (stored in _env) is injected into the Startup class.
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

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

При настройке служб в *Startup.CS*:

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

::: moniker-end


## <a name="https-redirection-middleware-alternative-approach"></a>Альтернативный подход по промежуточного слоя перенаправления HTTPS

Альтернативой по промежуточного слоя перенаправления HTTPS (`UseHttpsRedirection`) является использование по промежуточного слоя переписывания`AddRedirectToHttps`URL-адресов (). `AddRedirectToHttps`также может задавать код состояния и порт при выполнении перенаправления. Дополнительные сведения см. в разделе по [промежуточного слоя перезаписи URL-адресов](xref:fundamentals/url-rewriting).

При перенаправлении по протоколу HTTPS без требования дополнительных правил перенаправления рекомендуется использовать по промежуточного слоя перенаправления HTTPS (`UseHttpsRedirection`), описанные в этом разделе.

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a>Протокол HTTPS протокола безопасности транспорта HTTP (HSTS)

В [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [обеспечение безопасности транспорта по протоколу HTTP (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) является расширением безопасности, которое определяется веб-приложением с помощью заголовка ответа. Когда [браузер, поддерживающий HSTS,](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) получает этот заголовок:

* В браузере хранится конфигурация домена, которая предотвращает отправку обмена данными по протоколу HTTP. Браузер принудительно выполняет все связи по протоколу HTTPS.
* Браузер не разрешает пользователю использовать недоверенные или недопустимые сертификаты. Браузер отключает запросы, позволяющие пользователю временно доверять такому сертификату.

Так как HSTS обеспечивается клиентом, он имеет некоторые ограничения:

* Клиент должен поддерживать HSTS.
* HSTS требует по крайней мере одного успешного запроса HTTPS для установки политики HSTS.
* Приложение должно проверять каждый HTTP-запрос и перенаправлять или отклонять HTTP-запрос.

ASP.NET Core 2,1 и более поздних версий реализует `UseHsts` HSTS с помощью метода расширения. Следующий код вызывает `UseHsts` , когда приложение не находится в [режиме разработки](xref:fundamentals/environments):

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=10)]

::: moniker-end

`UseHsts`не рекомендуется в разработке, так как параметры HSTS очень кэшируются браузерами. По умолчанию `UseHsts` исключает локальный петлевой адрес.

Для рабочих сред, которые реализуют протокол HTTPS в первый раз, задайте для свойства Initial [хстсоптионс. MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) небольшое значение с помощью одного из <xref:System.TimeSpan> методов. Задайте в качестве значения в часах не более одного дня на случай, если необходимо вернуть инфраструктуру HTTPS к протоколу HTTP. Убедившись в устойчивости конфигурации HTTPS, увеличьте значение HSTS max-age. часто используемое значение — один год.

В приведенном ниже коде


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end


* Задает параметр предварительной загрузки для заголовка с уровнем безопасности "Долгосрочный транспорт — Безопасность". Предварительная загрузка не является частью [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается веб-браузерами для предварительной загрузки HSTS сайтов при новой установке. Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).
* Включает [инклудесубдомаин](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS для размещения поддоменов.
* Явно задает параметр max-age для заголовка с ограничением транспорта-безопасности до 60 дней. Если значение не задано, по умолчанию используется значение 30 дней. Дополнительные сведения см. в [директиве max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) .
* Добавляет `example.com` в список узлов для исключения.

`UseHsts`исключает следующие узлы замыкания на себя:

* `localhost`: Адрес замыкания на себя IPv4.
* `127.0.0.1`: Адрес замыкания на себя IPv4.
* `[::1]`: IPv6-адрес замыкания на себя.

## <a name="opt-out-of-httpshsts-on-project-creation"></a>Отказ от HTTPS/HSTS при создании проекта

В некоторых сценариях серверной службы, где безопасность подключения обрабатывается на общедоступной границе сети, Настройка безопасности подключения на каждом узле не требуется. Веб-приложения, созданные на основе шаблонов в Visual Studio или команды [DotNet New](/dotnet/core/tools/dotnet-new) , включают [Перенаправление HTTPS](#require-https) и [HSTS](#http-strict-transport-security-protocol-hsts). Для развертываний, которые не нуждаются в этих сценариях, можно отказаться от HTTPS/HSTS при создании приложения из шаблона.

Чтобы отказаться от HTTPS/HSTS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Снимите флажок **настроить для HTTPS** .

::: moniker range=">= aspnetcore-3.0"

![Диалоговое окно нового ASP.NET Core веб-приложения с снятым флажком настроить для HTTPS.](enforcing-ssl/_static/out-vs2019.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

![Диалоговое окно нового ASP.NET Core веб-приложения с снятым флажком настроить для HTTPS.](enforcing-ssl/_static/out.png)

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli) 

Использовать параметр `--no-https`. Пример

```console
dotnet new webapp --no-https
```

---

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a>Доверять сертификату разработки ASP.NET Core HTTPS в Windows и macOS

Пакет SDK для .NET Core включает сертификат разработки HTTPS. Сертификат устанавливается в рамках первого запуска. Например, выводит результат, `dotnet --info` аналогичный приведенному ниже:

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

При установке пакета SDK для .NET Core в локальное хранилище сертификатов пользователя устанавливается сертификат разработки HTTPS ASP.NET Core. Сертификат установлен, но не является доверенным. Чтобы сделать сертификат доверенным, выполните однократный шаг для запуска средства DotNet `dev-certs` :

```console
dotnet dev-certs https --trust
```

Следующая команда вызывает справку по средству `dev-certs`.

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Как настроить сертификат разработчика для DOCKER

Также см. [эту проблему в GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6199).

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a>Доверенный HTTPS сертификат из подсистемы Windows для Linux

Подсистема Windows для Linux (WSL) создает самозаверяющий сертификат HTTPS. Чтобы настроить хранилище сертификатов Windows для доверия к сертификату WSL, сделайте следующее:

* Выполните следующую команду, чтобы экспортировать созданный сертификат WSL:`dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`
* В окне WSL выполните следующую команду:`ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`

  Приведенная выше команда задает переменные среды, чтобы в Linux использовался доверенный сертификат Windows.

## <a name="additional-information"></a>Дополнительные сведения

* <xref:host-and-deploy/proxy-load-balancer>
* [ASP.NET Core узла в Linux с помощью Apache: Конфигурация HTTPS](xref:host-and-deploy/linux-apache#https-configuration)
* [ASP.NET Core узла в Linux с помощью nginx: Конфигурация HTTPS](xref:host-and-deploy/linux-nginx#https-configuration)
* [Настройка SSL в службах IIS](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [Поддержка браузера OWASP HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
