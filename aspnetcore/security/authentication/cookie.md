---
title: "С помощью файла Cookie проверки подлинности без удостоверения ASP.NET Core"
author: rick-anderson
description: "Описание использования файлов cookie проверки подлинности без ASP.NET Core Identity"
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: 1f20a2f7cab123e5a41ee1d232271da9de4c9058
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>С помощью файла Cookie проверки подлинности без удостоверения ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Latham Люк](https://github.com/guardrex)

Как видно в предыдущих разделах проверки подлинности, [ASP.NET Core Identity](xref:security/authentication/identity) является полным, полнофункциональный проверки подлинности поставщиком для создания и обслуживания имена входа. Тем не менее можно использовать свою собственную логику для нестандартной проверки подлинности с помощью файла cookie проверки подлинности на основе время от времени. Как поставщик проверки подлинности автономного без ASP.NET Core Identity можно использовать проверку подлинности на основе файлов cookie.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Сведения о миграции проверки подлинности на основе файла cookie из ASP.NET Core 1.x 2.0, см. [миграции проверку подлинности и удостоверение для ASP.NET 2.0 основных раздела (файл Cookie проверки подлинности на основе)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).

## <a name="configuration"></a>Конфигурация

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Если вы не используете [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), установите версию 2.0 + [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакет NuGet.

В `ConfigureServices` метод, создайте службу проверки подлинности по промежуточного слоя с `AddAuthentication` и `AddCookie` методов:

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet1)]

`AuthenticationScheme`передаваемый `AddAuthentication` задает схему проверки подлинности по умолчанию для приложения. `AuthenticationScheme`полезно при наличии нескольких экземпляров файла cookie проверки подлинности, и вы хотите [авторизации с нужной раскладки](xref:security/authorization/limitingidentitybyscheme). Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы. Можно указать любое строковое значение, отличающее схему.

В `Configure` используйте `UseAuthentication` метод, вызываемый по промежуточного слоя проверки подлинности, задает `HttpContext.User` свойства. Вызовите `UseAuthentication` метод перед вызовом метода `UseMvcWithDefaultRoute` или `UseMvc`:

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet2)]

**Параметры AddCookie**

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) класс используется для настройки параметров поставщика проверки подлинности.

| Параметр | Описание: |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | Предоставляет путь для предоставления с 302 Found (перенаправление URL-адрес) при срабатывании триггера `HttpContext.ForbidAsync`. Значение по умолчанию — `/Account/AccessDenied`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство утверждений, созданный с помощью службы проверки подлинности файла cookie. |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | Имя домена, где обслуживают куки-файл. По умолчанию это имя узла запроса. Браузер отправляет только куки-файл в запросах имени узла. Вы можете настроить этот параметр для файлов cookie, доступных любому узлу в вашем домене. Например, установите значение домена файла cookie `.contoso.com` делает ее доступной для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`. |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | Возвращает или задает время существования куки-файла. В настоящее время это параметр Нет ops и станет устаревшим в ASP.NET Core 2.1 +. Используйте `ExpireTimeSpan` параметр, чтобы задать срок действия файла cookie. Дополнительные сведения см. в разделе [уточняется поведение CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293). |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | Флаг, указывающий куки-файл должен быть доступен только на серверы. Изменение этого значения для `false` разрешает клиентские скрипты для доступа к куки-файл и может открыть приложение кража файлов cookie, должны иметь приложения [межсайтовых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость. Значение по умолчанию — `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | Задает имя файла cookie. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | Использовать для изоляции приложений, выполняющихся в то же имя узла. Если у вас есть приложение, работающее на `/app1` и нужно ограничить файлы cookie для данного приложения, задайте `CookiePath` свойства `/app1`. Таким образом, файл cookie доступна только для запросов к `/app1` и любое приложение, входящие в него. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | Указывает, является ли браузер должен позволять куки-файл для подключения к веб-сайте запросы только (`SameSiteMode.Strict`) или запросы между сайтами, с использованием безопасного HTTP методы и запросы веб-сайте (`SameSiteMode.Lax`). Если задано значение `SameSiteMode.None`, не задано значение заголовка файла cookie. Обратите внимание, что [по промежуточного слоя файлов Cookie политики](#cookie-policy-middleware) может перезаписать предоставленное значение. Для поддержки проверки подлинности OAuth, значение по умолчанию — `SameSiteMode.Lax`. Дополнительные сведения см. в разделе [потеряны из-за политики SameSite файла cookie проверки подлинности OAuth](https://github.com/aspnet/Security/issues/1231). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | Флаг, указывающий, следует ли созданный файл cookie ограничиваются HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и при запросе (`CookieSecurePolicy.SameAsRequest`). Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`. |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | Наборы `DataProtectionProvider` , используемый для создания по умолчанию `TicketDataFormat`. Если `TicketDataFormat` свойство задано, `DataProtectionProvider` параметр не используется. Если не указано, используется поставщик защиты данных приложения по умолчанию. |
| [События](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | Обработчик вызывает методы поставщика, позволяют контролировать приложения в определенные моменты обработки. Если `Events` не указан, предоставляется экземпляр по умолчанию, не выполняет никаких действий, при вызове методов. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | Используется как тип службы для получения `Events` экземпляром, а не свойства. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | `TimeSpan` После которого срок действия билета проверки подлинности, хранящийся в файле cookie. `ExpireTimeSpan`добавляется на текущий момент времени, чтобы создать срок действия билета. `ExpiredTimeSpan` Значение всегда переходит в зашифрованных AuthTicket, проверить сервер. Он также может перейти в [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) заголовок, но только если `IsPersistent` имеет значение. Чтобы задать `IsPersistent` для `true`, настройте [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) передаваемый `SignInAsync`. Значение по умолчанию `ExpireTimeSpan` — 14 дней. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | Предоставляет путь для предоставления с 302 Found (перенаправление URL-адрес) при срабатывании триггера `HttpContext.ChallengeAsync`. Текущий URL-адрес, создавший код 401, добавляется в `LoginPath` как параметр строки запроса с именем `ReturnUrlParameter`. Запрос один раз на `LoginPath` предоставляет нового вход удостоверения, `ReturnUrlParameter` значение используется для перенаправления браузера обратно в URL-адрес, вызвавший исходный код состояния не санкционировано. Значение по умолчанию — `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | Если `LogoutPath` предоставляется обработчику, а затем перенаправляет запрос по этому пути в зависимости от значения `ReturnUrlParameter`. Значение по умолчанию — `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | Определяет имя параметра строки запроса, добавленный для обработчика ответа 302 Found (перенаправление URL-адрес). `ReturnUrlParameter`используется при получении запроса на `LoginPath` или `LogoutPath` для возврата в браузере исходный URL-адрес после выполнения действия имени входа или выхода из системы. Значение по умолчанию — `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | Необязательный контейнер, используемый для хранения идентификаторов различных запросов. При использовании только идентификатор сеанса отправляется клиенту. `SessionStore`можно использовать для устранения возможных проблем с большой удостоверения. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | Флаг, указывающий, должен быть новый файл cookie, срок действия обновленного выдан динамически. Это может произойти на любой запрос, где текущего срока действия файла cookie более чем на 50% срок действия истек. Новую дату окончания срока действия перемещается вперед для текущей даты, а также `ExpireTimespan`. [Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` класса при вызове `SignInAsync`. Абсолютный срок действия может повысить безопасность приложения, ограничивая количество времени, допустимый файл cookie проверки подлинности. Значение по умолчанию — `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | `TicketDataFormat` Используется для установки и снятия защиты identity и других свойств, хранящихся в значении cookie. Если не указано, `TicketDataFormat` создается с помощью [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0). |
| [Validate](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | Метод, который проверяет, что параметры являются допустимыми. |

Задать `CookieAuthenticationOptions` в конфигурации службы для проверки подлинности в `ConfigureServices` метод:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Куки-файл использует 1.x ASP.NET Core [по промежуточного слоя](xref:fundamentals/middleware/index) , сериализует участника-пользователя в зашифрованном файле cookie. При последующих запросах куки-файл проверяется, и основной сервер заново и назначенные `HttpContext.User` свойство.

Установка [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета NuGet в проекте. Этот пакет содержит файл cookie по промежуточного слоя.

Используйте `UseCookieAuthentication` метод в `Configure` метод в вашей *файла Startup.cs* файла перед `UseMvc` или `UseMvcWithDefaultRoute`:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

**Параметры CookieAuthenticationOptions**

[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) класс используется для настройки параметров поставщика проверки подлинности.

| Параметр | Описание: |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | Задает схему проверки подлинности. `AuthenticationScheme`полезно, когда несколько экземпляров проверки подлинности, и вы хотите [авторизации с нужной раскладки](xref:security/authorization/limitingidentitybyscheme). Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы. Можно указать любое строковое значение, отличающее схему. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Задает значение, указывающее, что файл cookie проверки подлинности запустите при каждом запросе и попытка проверки и воссоздания любому участнику сериализованный она создана. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | Значение true, если по промежуточного слоя проверки подлинности обрабатывает автоматического проблем. Если значение равно false, проверка подлинности по промежуточного слоя лишь изменяет ответы, если они явным образом указано `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство все утверждения, созданные по промежуточного слоя проверки подлинности файла cookie. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | Имя домена, где обслуживают куки-файл. По умолчанию это имя узла запроса. Браузер служит только файла cookie для сопоставления имени узла. Вы можете настроить этот параметр для файлов cookie, доступных любому узлу в вашем домене. Например, установите значение домена файла cookie `.contoso.com` делает ее доступной для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | Флаг, указывающий куки-файл должен быть доступен только на серверы. Изменение этого значения для `false` разрешает клиентские скрипты для доступа к куки-файл и может открыть приложение кража файлов cookie, должны иметь приложения [межсайтовых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость. Значение по умолчанию — `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | Использовать для изоляции приложений, выполняющихся в то же имя узла. Если у вас есть приложение, работающее на `/app1` и нужно ограничить файлы cookie для данного приложения, задайте `CookiePath` свойства `/app1`. Таким образом, файл cookie доступна только для запросов к `/app1` и любое приложение, входящие в него. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | Флаг, указывающий, следует ли созданный файл cookie ограничиваются HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и при запросе (`CookieSecurePolicy.SameAsRequest`). Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`. |
| [Описание](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | Дополнительные сведения о типе проверки подлинности доступны для приложения. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | `TimeSpan` После которого срок действия билета проверки подлинности. Он добавляется в текущее время, чтобы создать срок действия билета. Для использования `ExpireTimeSpan`, необходимо задать `IsPersistent` для `true` в `AuthenticationProperties` передаваемый `SignInAsync`. Значение по умолчанию — 14 дней. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | Флаг, указывающий, сбрасывается ли дата окончания срока действия файла cookie при более половины `ExpireTimeSpan` истечет интервал. Новое время exipiration перемещается вперед для текущей даты, а также `ExpireTimespan`. [Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` класса при вызове `SignInAsync`. Абсолютный срок действия может повысить безопасность приложения, ограничивая количество времени, допустимый файл cookie проверки подлинности. Значение по умолчанию — `true`. |

Задать `CookieAuthenticationOptions` по промежуточного слоя проверки подлинности файла Cookie в `Configure` метод:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a>Файл cookie политики по промежуточного слоя

[По промежуточного слоя файлов cookie политики](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) обеспечивает возможности политики куки-файл в приложении. Добавление по промежуточного слоя в конвейере обработки приложения — порядок конфиденциальные; он влияет только на компоненты, зарегистрированные за ним в конвейере.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) для по промежуточного слоя файлов Cookie политики позволяют управлять общих характеристик обработки файлов cookie и обработчик в обработчики обработки файла cookie при удалении или добавлены файлы cookie.

| Свойство. | Описание: |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Влияет ли файлы cookie должен быть HttpOnly, который имеет флаг, указывающий, куки-файл должен быть доступен только на серверы. Значение по умолчанию — `HttpOnlyPolicy.None`. |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Влияет на атрибут веб-сайте куки-файл (см. ниже). Значение по умолчанию — `SameSiteMode.Lax`. Этот параметр доступен для основных компонентов ASP.NET 2.0 +. |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Вызывается при добавлении файла cookie. |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Вызывается при удалении файла cookie. |
| [Secure](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | Влияет ли файлы cookie должен быть Secure. Значение по умолчанию — `CookieSecurePolicy.None`. |

**MinimumSameSitePolicy** (ASP.NET Core 2.0 + только)

Значение по умолчанию `MinimumSameSitePolicy` значение `SameSiteMode.Lax` для разрешения проверки подлинности OAuth2. Для строго принудительного применения политики веб-сайте `SameSiteMode.Strict`, задайте `MinimumSameSitePolicy`. Несмотря на то, что этот параметр сбрасывает OAuth2 и других схем проверки подлинности независимо от источника, он повышает уровень безопасности cookie для других типов приложений, которые не зависят от обработки запроса независимо от источника.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Параметр по промежуточного слоя куки-файл политики для `MinimumSameSitePolicy` может повлиять на ваш равным `Cookie.SameSite` в `CookieAuthenticationOptions` параметры в соответствии с Приведенная ниже таблица.

| MinimumSameSitePolicy | Cookie.SameSite | Результирующий параметр Cookie.SameSite |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="creating-an-authentication-cookie"></a>Создание файла cookie проверки подлинности

Чтобы создать файл cookie, содержащий сведения о пользователе, следует создать [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal). Сведения о пользователе сериализуется и сохраняется в файле cookie. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Создание [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) с все необходимые [утверждения](/dotnet/api/system.security.claims.claim)s и вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) входа пользователя:

[!code-csharp[Main](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) входа пользователя:

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

`SignInAsync`создает зашифрованный файл cookie и добавляет его в текущий ответ. Если не указать `AuthenticationScheme`, используется схема по умолчанию.

На самом деле шифрования, используемый — ASP.NET Core [защиты данных](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) системы. Если вы используете приложение на нескольких компьютерах, балансировки нагрузки для приложений или с помощью веб-фермы, то необходимо [настроить защиту данных](xref:security/data-protection/configuration/overview) использовать же ключей и идентификатор приложения.

## <a name="signing-out"></a>Выход

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Чтобы выйти из системы текущего пользователя и удалить их куки-файл, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[Main](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Чтобы выйти из системы текущего пользователя и удалить их куки-файл, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

Если вы не используете `CookieAuthenticationDefaults.AuthenticationScheme` (или файлы «cookie») как схему (например, «ContosoCookie»), укажите схему, которая используется при настройке поставщика проверки подлинности. В противном случае используется схема по умолчанию.

## <a name="reacting-to-back-end-changes"></a>Отклик на изменения в серверной части

После создания файла cookie, он становится единственным источником удостоверения. Даже при отключении пользователя в серверной части системы системой проверки подлинности файла cookie не имеет сведений о это, а пользователь сохраняется вошедший в систему, при условии, что их файл cookie является допустимым.

[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) событий в ASP.NET Core 2.x или [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) метод в ASP.NET Core 1.x можно использовать для перехвата и переопределение проверки подлинности файла cookie. Этот подход уменьшает риск отозванных пользователей, получающих доступ к приложению.

Одним из способов проверки cookie основан следить за изменением пользовательской базы данных. Если базы данных не был изменен с момента выдачи объекта cookie пользователя, нет необходимости повторно пройти проверку подлинности пользователя при их куки-файл все еще действительна. Чтобы реализовать этот сценарий базы данных, которая реализована в `IUserRepository` в этом примере хранит `LastChanged` значение. При обновлении любого пользователя в базе данных, `LastChanged` имеет значение до текущего момента.

Чтобы сделать недействительными файла cookie при изменении базы данных на основе `LastChanged` значение, создайте файл cookie с `LastChanged` утверждение, содержащее текущий `LastChanged` значение из базы данных:

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Чтобы реализовать переопределение для `ValidatePrincipal` событий записи метод со следующей сигнатурой в классе, унаследованном от [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Пример выглядит следующим образом:

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Зарегистрируйте экземпляр события во время регистрации службы куки-файл в `ConfigureServices` метод. Регистрация службы с областью для предоставления вашей `CustomCookieAuthenticationEvents` класса:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Чтобы реализовать переопределение для `ValidateAsync` событий записи метод со следующей сигнатурой:

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET Core Identity реализует эту проверку как часть его [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync). Пример выглядит следующим образом:

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Регистрировать событие во время настройки файла cookie проверки подлинности в `Configure` метод:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

Рассмотрим ситуацию, в которой имя пользователя будет обновлено &mdash; решение, которое не влияет на безопасность системы никаким образом. Безболезненно обновить участника-пользователя следует вызвать `context.ReplacePrincipal` и задайте `context.ShouldRenew` свойства `true`.

> [!WARNING]
> Подход, описанный здесь срабатывает при каждом запросе. Это может привести к снижению больших производительности для приложения.

## <a name="persistent-cookies"></a>Сохраняемые файлы cookie

Вы можете куки-файл для сохранения между сеансами браузера. Это сохраняемости должны включаться только с согласия пользователя с помощью флажка «Запомнить мои» на имя входа или аналогичного механизма. 

Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, нечувствительным к через браузер замыкания. Параметры скользящего срока действия ранее настроенные соблюдаются. Если истекает срок действия файла cookie, при закрытии, браузер очищает куки-файл после его перезапуска.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) класс находится в `Microsoft.AspNetCore.Authentication` пространства имен.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) класс находится в `Microsoft.AspNetCore.Http.Authentication` пространства имен.

---

## <a name="absolute-cookie-expiration"></a>Срок действия файла cookie абсолютный

Можно задать абсолютный срок действия с `ExpiresUtc`. Необходимо также задать `IsPersistent`; в противном случае `ExpiresUtc` учитывается и создается файл cookie одного сеанса. При `ExpiresUtc` устанавливается на `SignInAsync`, оно переопределяет значение `ExpireTimeSpan` параметр `CookieAuthenticationOptions`, если задать.

Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, срок действия составляет 20 минут. Это не учитывает любые скользящий срок действия ранее настроенные.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="see-also"></a>См. также

* [Изменения AUTH 2.0 или объявления миграции](https://github.com/aspnet/Announcements/issues/262)
* [Ограничение идентификаторов по схеме](xref:security/authorization/limitingidentitybyscheme)
