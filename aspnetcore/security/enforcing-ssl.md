---
title: Принудительное использование HTTPS в ASP.NET Core
author: rick-anderson
description: Узнайте, как требовать HTTPS/TLS в веб-приложения ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b4c058d3b4f00276043d9520d756e62ed8cac5d9
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325605"
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

<a name="require"></a>

## <a name="require-https"></a>Требование HTTPS

::: moniker range=">= aspnetcore-2.1"

Мы рекомендуем все производство ASP.NET Core, веб-приложений вызова:

* По промежуточного слоя переадресации HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) для перенаправления всех запросов HTTP на HTTPS.
* [UseHsts](#hsts), безопасность строгой транспортный протокол HTTP (HSTS).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

Следующий код вызывает `UseHttpsRedirection` в `Startup` класса:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Выделенный выше код:

* Использует значение по умолчанию [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).
* Использует значение по умолчанию [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), если не переопределено `ASPNETCORE_HTTPS_PORT` переменной среды или [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

Мы рекомендуем использовать временный перенаправления, а не постоянное перенаправление, как кэширование ссылок может привести к нестабильной работе поведению в средах разработки. Мы рекомендуем использовать [HSTS](#hsts) сигнал для клиентов, которые только защитить ресурс запросы следует отправлять в приложение (только в рабочей среде).

> [!WARNING]
> Порт должен быть доступен для по промежуточного слоя для перенаправления на HTTPS. Если порт не доступен, не произойдет перенаправление на HTTPS. Порт HTTPS можно задать с помощью любого из следующих подходов:
>
> * Задайте `HttpsRedirectionOptions.HttpsPort`.
> * Задайте переменную среды `ASPNETCORE_HTTPS_PORT`.
> * В разработке, задать URL-адрес HTTPS в *launchsettings.json*.
> * Настройка конечной точки URL-адрес HTTPS [Kestrel](xref:fundamentals/servers/kestrel) или [HTTP.sys](xref:fundamentals/servers/httpsys).
>
> Если Kestrel и HTTP.sys используется в качестве общедоступных пограничного сервера, Kestrel и HTTP.sys должны быть настроены на прослушивание оба:
>
> * Безопасного порта, где перенаправляется клиент (как правило, 443 в рабочей среде и 5001 в разработке).
> * Небезопасный порт (как правило, 80 в рабочей среде) до 5000 при разработке.
>
> Небезопасный порт должен быть доступны клиенту в порядке для приложения для получения небезопасный запрос и перенаправить его безопасного порта.
>
> Любой брандмауэра между клиентом и сервером также должны быть открыты для трафика порты.
>
> Дополнительные сведения см. в разделе [конфигурации конечной точки Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) или <xref:fundamentals/servers/httpsys>.

Следующий выделенный код вызывает метод [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) настроить параметры по промежуточного слоя:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Вызов `AddHttpsRedirection` необходимо изменить значения `HttpsPort` или `RedirectStatusCode`.

Выделенный выше код:

* Наборы [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) для [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), который является значением по умолчанию. Используйте поля [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) класс для назначений `RedirectStatusCode`.
* Задать HTTPS-порт на 5001. Значение по умолчанию — 443.

Следующие механизмы автоматически задать порт:

* По промежуточного слоя можно обнаружить порты, через [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) когда применяются следующие условия:

  * Kestrel и HTTP.sys используется непосредственно с помощью конечных точек HTTPS (также относится к выполняющемуся приложению с помощью отладчика Visual Studio Code).
  * Только **один порт HTTPS** используется приложением.

* Visual Studio используется:

  * IIS Express поддерживает протокол HTTPS.
  * *launchSettings.json* задает `sslPort` для IIS Express.

> [!NOTE]
> При запуске приложения позади обратного прокси-сервера (например, IIS, IIS Express), `IServerAddressesFeature` недоступна. Необходимо вручную настроить порт. Если порт не задан, не перенаправляет запросы.

Порт можно настроить, задав [параметр конфигурации веб-узел https_port](xref:fundamentals/host/web-host#https-port):

**Ключ**: https_port  
**Тип**: *string*  
**По умолчанию**: не задано значение по умолчанию.  
**Задается с помощью**: `UseSetting`  
**Переменная среды**: `<PREFIX_>HTTPS_PORT` (используется префикс `ASPNETCORE_` при использовании веб-узла.)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> Порт можно настроить, задав URL-адрес с косвенно `ASPNETCORE_URLS` переменной среды. Настраивает сервер в переменной среды, а затем по промежуточного слоя косвенно за HTTPS-порт, через `IServerAddressesFeature`.

Если порт не задан:

* Не перенаправлять запросы.
* По промежуточного слоя записывает предупреждение «Не удалось определить https-порт для перенаправления.»

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

Для рабочих сред, реализация HTTPS в первый раз задайте начальное значение HSTS небольшое значение. Задайте значение от часов не больше, чем в один день в случае необходимости использования инфраструктуры HTTPS-HTTP. После вы уверены в устойчивости конфигурации HTTPS, увеличьте значение max-age HSTS; часто используемые значение — один год. 

В приведенном ниже коде

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Задает параметр предварительной загрузки заголовок Strict-Transport-Security. Предварительной загрузки не входит в состав [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается веб-браузеры для предварительной загрузки HSTS узлов на установку обновления. Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).
* Позволяет [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS для размещения таких поддоменов.
* Явно задает параметр max-age заголовок Strict-Transport-Security 60 дней. Если не установлено значение по умолчанию — 30 дней. См. в разделе [директива максимального](https://tools.ietf.org/html/rfc6797#section-6.1.1) Дополнительные сведения.
* Добавляет `example.com` в список узлов для исключения.

`UseHsts` исключает следующими узлами замыкания на себя:

* `localhost` : IPv4-адрес замыкания на себя.
* `127.0.0.1` : IPv4-адрес замыкания на себя.
* `[::1]` : IPv6-адрес замыкания на себя.

В предыдущем примере показано, как добавить дополнительные узлы.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>

## <a name="opt-out-of-httpshsts-on-project-creation"></a>Отказаться от HTTPS/HSTS при создании проекта

В некоторых сценариях службы серверной части, где безопасность подключения обрабатывается на границе общедоступные сети Настройка безопасности подключения на каждом узле не требуется. Веб-приложений, созданных из шаблонов в Visual Studio или из [команды dotnet new](/dotnet/core/tools/dotnet-new) команду enable [перенаправления HTTPS](#require) и [HSTS](#hsts). Для развертываний, которые не требуют этих сценариев вы можете отказаться от HTTPS/HSTS создания приложения на основе шаблона.

Чтобы отказаться от HTTPS/HSTS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Снимите флажок **настроить для использования протокола HTTPS** флажок.

![Схема сущностей](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli) 

Использовать параметр `--no-https`. Пример

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Как настроить сертификат разработчика для Docker

См. в разделе [проблема GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end

## <a name="additional-information"></a>Дополнительные сведения

* [Поддержка браузеров OWASP HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
