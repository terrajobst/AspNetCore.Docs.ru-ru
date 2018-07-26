---
title: Принудительное использование HTTPS в ASP.NET Core
author: rick-anderson
description: Показано, как требовать HTTPS/TLS, в ASP.NET Core веб-приложения.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: c3d92994c0331b1408e246953454910ca1f4dc43
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254835"
---
# <a name="enforce-https-in-aspnet-core"></a>Принудительное использование HTTPS в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом документе показано, как:

* Требование HTTPS для всех запросов.
* Перенаправлять все запросы HTTP на HTTPS.

> [!WARNING]
> Сделать **не** использовать [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) на веб-API, получения конфиденциальной информации. `RequireHttpsAttribute` использует коды состояния HTTP для перенаправления браузеров с HTTP на HTTPS. Клиенты API не может понять и подчиняются перенаправление с HTTP на HTTPS. Такие клиенты могут отправлять данные по протоколу HTTP. Веб-API должен:
>
> * Прослушивает HTTP.
> * Закрыть соединение с кодом состояния 400 (неправильный запрос) и не обработать запрос.

<a name="require"></a>
## <a name="require-https"></a>Требование HTTPS

::: moniker range=">= aspnetcore-2.1"

Рекомендуется, чтобы все веб-приложения ASP.NET Core вызвать по промежуточного слоя переадресации HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) для перенаправления всех запросов HTTP на HTTPS.

Следующий код вызывает `UseHttpsRedirection` в `Startup` класса:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Выделенный выше код:

* Использует значение по умолчанию [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`). Рабочие приложения должны вызывать [UseHsts](#hsts).
* Использует значение по умолчанию [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).

Следующий код вызывает [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) настроить параметры по промежуточного слоя:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Выделенный выше код:

* Наборы [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) для `Status307TemporaryRedirect`, который является значением по умолчанию. Рабочие приложения должны вызывать [UseHsts](#hsts).
* Задать HTTPS-порт на 5001. Значение по умолчанию — 443.

Следующие механизмы автоматически задать порт:

* По промежуточного слоя можно обнаружить порты, через [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) когда применяются следующие условия:
  - Kestrel и HTTP.sys используется непосредственно с помощью конечных точек HTTPS (также относится к выполняющемуся приложению с помощью отладчика Visual Studio Code).
  - Только **один порт HTTPS** используется приложением.
* Visual Studio используется:
  - IIS Express поддерживает протокол HTTPS.
  - *launchSettings.json* задает `sslPort` для IIS Express.

> [!NOTE]
> При запуске приложения позади обратного прокси-сервера (например, IIS, IIS Express), `IServerAddressesFeature` недоступна. Необходимо вручную настроить порт. Если порт не задан, не перенаправляет запросы.

Порт можно настроить, задав:

* Переменная среды `ASPNETCORE_HTTPS_PORT`.
* `http_port` ключ конфигурации узла (например, с помощью *hostsettings.json* или аргумент командной строки).
* [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport). См. предыдущий пример, в котором показано, как задать порт на 5001.

> [!NOTE]
> Порт можно настроить, задав URL-адрес с косвенно `ASPNETCORE_URLS` переменной среды. Настраивает сервер в переменной среды, а затем по промежуточного слоя косвенно за HTTPS-порт, через `IServerAddressesFeature`.

Если порт не задан:

* Не перенаправлять запросы.
* По промежуточного слоя заносит в журнал предупреждение.

> [!NOTE]
> Альтернативой использованию по промежуточного слоя переадресации HTTPS (`UseHttpsRedirection`) является использование по промежуточного слоя для переопределения URL-адрес (`AddRedirectToHttps`). `AddRedirectToHttps` Можно также задать код состояния и порт при выполнении перенаправления. Дополнительные сведения см. в разделе [по промежуточного слоя для переопределения URL-адрес](xref:fundamentals/url-rewriting).
>
> При перенаправлении HTTPS без необходимости для перенаправления дополнительных правил, мы рекомендуем использовать по промежуточного слоя переадресации HTTPS (`UseHttpsRedirection`) описано в этом разделе.

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

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>Безопасность строгой транспортный протокол HTTP (HSTS)

На [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP строгой безопасности транспорта (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) представляет собой улучшение центра безопасности, который задается параметром веб-приложения при помощи специального заголовка ответа. При получении этого заголовка поддерживаемый браузер этого браузера предотвратит передачу любых данных отправленных по протоколу HTTP к указанному домену и вместо этого будет отправлять весь обмен данными по протоколу HTTPS. Она также препятствует щелкните HTTPS через запросы в браузерах.

ASP.NET Core 2.1 или более поздних версий реализует HSTS с `UseHsts` метода расширения. Следующий код вызывает `UseHsts` когда приложение не [режим разработки](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` не следует использовать в разработки так, как заголовок HSTS высокой кэширован в браузерах. По умолчанию `UseHsts` исключает локальный петлевой адрес.

В приведенном ниже коде

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Задает параметр предварительной загрузки заголовок Strict-Transport-Security. Предварительная загрузка не является частью [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается веб-браузеры для предварительной загрузки HSTS узлов на установку обновления. Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).
* Позволяет [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS для размещения таких поддоменов. 
* Явно задает параметр max-age заголовок Strict-Transport-Security, чтобы до 60 дней. Если не установлено значение по умолчанию — 30 дней. См. в разделе [директива максимального](https://tools.ietf.org/html/rfc6797#section-6.1.1) Дополнительные сведения.
* Добавляет `example.com` в список узлов для исключения.

`UseHsts` исключает следующими узлами замыкания на себя:

* `localhost` : IPv4-адрес замыкания на себя.
* `127.0.0.1` : IPv4-адрес замыкания на себя.
* `[::1]` : IPv6-адрес замыкания на себя.

В предыдущем примере показано, как добавить дополнительные узлы.
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Отказаться от HTTPS при создании проекта

Включить шаблоны ASP.NET Core 2.1 или более поздней версии веб-приложения (из Visual Studio или командной строки dotnet) [перенаправления HTTPS](#require) и [HSTS](#hsts). Для развертываний, которые не требуют HTTPS вы можете отказаться от протокола HTTPS. Например некоторые службы серверной части, где HTTPS обрабатывается извне на границе, с помощью протокола HTTPS на каждом узле не требуется.

Чтобы отказаться от HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Снимите флажок **настроить для использования протокола HTTPS** флажок.

![Схема сущностей](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli) 

Использовать параметр `--no-https`. Пример

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Как настроить сертификат разработчика для Docker

См. в разделе [проблема GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
