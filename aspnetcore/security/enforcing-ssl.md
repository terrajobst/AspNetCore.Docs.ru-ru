---
title: Применять HTTPS в ASP.NET Core
author: rick-anderson
description: Показано, как требуется HTTPS/TLS в ASP.NET Core веб-приложения.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: f49a7846149385125390285e2f1332d8e40642c0
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725940"
---
# <a name="enforce-https-in-aspnet-core"></a>Применять HTTPS в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом документе показано, как:

* Требуйте использования протокола HTTPS для всех запросов.
* Перенаправление всех запросов HTTP на HTTPS.

> [!WARNING]
> Сделать **не** использовать [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) на веб-API, получать конфиденциальные сведения. `RequireHttpsAttribute` использует коды состояния HTTP для перенаправления обозревателей с HTTP на HTTPS. Клиенты API не может понять или подчиняются перенаправлений с HTTP на HTTPS. Такие клиенты могут отправлять сведения по протоколу HTTP. Веб-API должен:
>
> * Прослушивает HTTP.
> * Закрывает соединение с кодом состояния 400 (неправильный запрос) и обслуживает запрос.

<a name="require"></a>
## <a name="require-https"></a>Требовать использования протокола HTTPS

::: moniker range=">= aspnetcore-2.1"

Рекомендуется, чтобы все веб-приложения ASP.NET Core вызова HTTPS перенаправление по промежуточного слоя ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) для перенаправления всех запросов HTTP на HTTPS.

Следующий код вызывает `UseHttpsRedirection` в `Startup` класса:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Следующий код вызывает [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) для настройки параметры по промежуточного слоя:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Предыдущий выделенный код:

* Наборы [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) для `Status307TemporaryRedirect`, который является значением по умолчанию. Приложения в рабочей среде следует вызывать [UseHsts](#hsts).
* Задает 5001 HTTPS-порт. Значение по умолчанию — 443.

Следующие механизмы автоматически задать порт:

* По промежуточного слоя может обнаруживать портов через [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) при наличии следующих условий:
  - Непосредственно с конечные точки HTTPS используется kestrel или HTTP.sys (также применяется для запуска приложения с помощью отладчика Visual Studio Code).
  - Только **один порт HTTPS** используется приложением.
* Visual Studio используется:
  - IIS Express включает взаимодействие по HTTPS.
  - *launchSettings.json* задает `sslPort` для IIS Express.

> [!NOTE]
> При запуске приложения за обратного прокси-сервера (например, службы IIS, IIS Express) `IServerAddressesFeature` недоступна. Необходимо вручную настроить порт. Если порт не настроен, запросы на перенаправление не выполнено.

Порт может быть установлен:

* Переменная среды `ASPNETCORE_HTTPS_PORT`.
* `http_port` ключ конфигурации узла (например, через *hostsettings.json* или аргумент командной строки).
* [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport). См. предыдущий пример, демонстрирующий значение 5001 порта.

> [!NOTE]
> Порт можно настроить, задав URL-адрес с косвенно `ASPNETCORE_URLS` переменной среды. Настраивает сервер в переменной среды, а затем по промежуточного слоя косвенно обнаруживает порт HTTPS через `IServerAddressesFeature`.

Если порт не задан:

* Запросы на перенаправление не выполнено.
* По промежуточного слоя заносит в журнал предупреждение.

> [!NOTE]
> Вместо использования по промежуточного слоя перенаправления HTTPS (`UseHttpsRedirection`) является использование по промежуточного слоя перезаписи URL-адрес (`AddRedirectToHttps`). `AddRedirectToHttps` Можно также задать код состояния и порта при выполнении перенаправления. Дополнительные сведения см. в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting).
>
> При перенаправлении HTTPS не требует дополнительных перенаправления правила, рекомендуется использовать HTTPS перенаправление по промежуточного слоя (`UseHttpsRedirection`) описанные в этом разделе.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется для обязательного использования протокола HTTPS. `[RequireHttpsAttribute]` можно дополнить контроллеров или методы или могут быть применены глобально. Чтобы применить атрибут глобально, добавьте следующий код в `ConfigureServices` в `Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Предыдущий выделенный код требует использовать все запросы `HTTPS`; таким образом, HTTP-запросы учитываются. Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Дополнительные сведения см. в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting). По промежуточного слоя также позволяет приложению задать код состояния или код состояния и порт, когда выполняется перенаправление.

Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности. Применение `[RequireHttps]` атрибут ко всем страницам контроллеры и Razor не считается защищено настолько, насколько требования HTTPS глобально. Не может гарантировать `[RequireHttps]` атрибут при добавлении новых контроллеров и страниц Razor.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>Безопасность Strict транспортный протокол HTTP (HSTS)

На [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [строгой безопасности транспорта (HSTS) HTTP](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) — расширение согласиться на использование безопасности, определяемый веб-приложения с помощью специальных заголовка. Когда этот заголовок получает поддерживаемого браузера браузера позволит обмен данными с передачей по протоколу HTTP к указанному домену и вместо этого будет отправлять все связи по протоколу HTTPS. Он также препятствует щелкните HTTPS через запросы в браузерах.

ASP.NET Core 2.1 или более поздней реализует HSTS с `UseHsts` метода расширения. Следующий код вызывает `UseHsts` при это приложение не в [режим разработки](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` не рекомендуется в разработке, так как заголовок HSTS высокой кэшируемое в браузерах. По умолчанию `UseHsts` исключает локальный петлевой адрес.

В приведенном ниже коде

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Задает параметр предварительной загрузки заголовка безопасности Strict-транспорта. Предварительная загрузка не частью [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается для веб-браузера для предварительной загрузки HSTS узлов на установку. Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).
* Включает [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS дочерних узлов. 
* Явно задает для параметра max-age заголовка безопасности для транспорта Strict до 60 дней. Если свойство не задано, по умолчанию используется значение 30 дней. В разделе [директива max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) для получения дополнительной информации.
* Добавляет `example.com` в список узлов для исключения.

`UseHsts` исключает замыкания на себя следующие узлы:

* `localhost` : IPv4-адрес замыкания на себя.
* `127.0.0.1` : IPv4-адрес замыкания на себя.
* `[::1]` : Петлевой адрес IPv6.

Предыдущий пример демонстрирует добавление дополнительных узлов.
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Отказаться от HTTPS на создания проекта

Включить шаблоны 2.1 или более поздней версии веб-приложений ASP.NET Core (из Visual Studio или в командной строке dotnet) [перенаправления HTTPS](#require) и [HSTS](#hsts). Для развертываний, не требующие HTTPS вы можете отказаться от HTTPS. Например некоторые серверных служб, где HTTPS обрабатывается извне на границе с помощью протокола HTTPS на каждом узле не требуется.

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

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Как можно настроить сертификат разработчика для Docker

В разделе [этой проблемы GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
