---
title: Использовать проверку подлинности файлов cookie без ASP.NET Core Identity
author: rick-anderson
description: Объяснение того, с использованием проверки подлинности файла cookie без ASP.NET Core Identity
ms.author: riande
ms.date: 02/25/2019
uid: security/authentication/cookie
ms.openlocfilehash: c6cba588abb003ee677d8f1753b73a1ced1414b0
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209379"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>Использовать проверку подлинности файлов cookie без ASP.NET Core Identity

По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Люк Лэтем](https://github.com/guardrex)

Как вы уже видели в предыдущих разделах проверки подлинности, [удостоверения ASP.NET Core](xref:security/authentication/identity) — это поставщик проверки подлинности завершено, полнофункциональную для создания и обслуживания имена входа. Тем не менее может потребоваться использование собственной логики настраиваемой проверки подлинности на основе файлов cookie проверки подлинности в некоторых случаях. Проверка подлинности на основе файлов cookie можно использовать как автономного поставщика проверки подлинности без ASP.NET Core Identity.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Для демонстрационных целей в примере приложения учетной записи пользователя для гипотетической пользователя Родригез Мария встроен в приложение. Использовать имя пользователя по электронной почте "maria.rodriguez@contoso.com" и любой пароль для входа пользователя. Пользователь проходит проверку подлинности в `AuthenticateUser` метод в *Pages/Account/Login.cshtml.cs* файла. В реальный пример пользователь может пройти проверку подлинности в базе данных.

Сведения о миграции файл cookie проверки подлинности с помощью ASP.NET Core 1.x на 2.0, см. в разделе [перенести проверки подлинности и другие удостоверения в разделе ASP.NET Core 2.0 (проверка подлинности на основе файлов Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).

Чтобы использовать удостоверение ASP.NET Core, см. в разделе [Общие сведения об Identity](xref:security/authentication/identity) раздела.

## <a name="configuration"></a>Конфигурация

::: moniker range=">= aspnetcore-2.0"

Если приложение не использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), создайте ссылку на пакет в файле проекта для [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета (версии 2.1.0 или более поздние).

В `ConfigureServices` метод, создайте службу проверки подлинности по промежуточного слоя с `AddAuthentication` и `AddCookie` методов:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

`AuthenticationScheme` передаваемый `AddAuthentication` задает схему проверки подлинности по умолчанию для приложения. `AuthenticationScheme` полезно, если имеется несколько экземпляров файла cookie проверки подлинности, и вы хотите [авторизация с определенной схемой](xref:security/authorization/limitingidentitybyscheme). Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы. Вы можете указать любое строковое значение, являющийся отличительным признаком схемы.

Схема проверки подлинности приложения отличается от схемы проверки подлинности файла cookie для приложения. Когда схему проверки подлинности файла cookie не предоставляется для <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, он использует [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) («файлы cookie»).

Файл cookie проверки подлинности <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> свойству `true` по умолчанию. Файлы cookie проверки подлинности разрешены, если посетитель не означает согласие на сбор данных. Дополнительные сведения см. в разделе <xref:security/gdpr#essential-cookies>.

В `Configure` используйте `UseAuthentication` метод, вызываемый по промежуточного слоя проверки подлинности, который задает `HttpContext.User` свойства. Вызовите `UseAuthentication` метод перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

**Параметры AddCookie**

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) класс используется для настройки параметров поставщика проверки подлинности.

| Параметр | Описание |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | Предоставляет путь к предоставить "302 Found" (URL-адрес перенаправления) при активации `HttpContext.ForbidAsync`. Значение по умолчанию — `/Account/AccessDenied`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство на все утверждения, созданный с помощью службы проверки подлинности файла cookie. |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | Имя домена, где обслуживается куки-файл. По умолчанию это имя узла запроса. Браузер отправляет только куки-файл в запросах к соответствующим именем узла. Вы можете настроить этот параметр для файлов cookie, доступных на любом узле в вашем домене. Например, значение домена файла cookie `.contoso.com` сделает его доступным для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`. |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | Флаг, указывающий файл cookie должен быть доступен только на серверы. Это значение изменится на `false` разрешает клиентские скрипты, чтобы получить доступ к куки-файл и может открыть свое приложение для хищения файлов cookie, должны иметь приложения [межузловых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость. Значение по умолчанию — `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | Задает имя файла cookie. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | Использовать для изоляции приложений, выполняющихся в то же имя узла. Если у вас есть приложение, запущенное в `/app1` и нужно ограничить файлы cookie для этого приложения, задайте `CookiePath` свойства `/app1`. Таким образом, файл cookie доступна только для запросов к `/app1` и любого приложения под ним. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | Указывает, разрешено ли браузер файлов cookie, должны быть присоединены к только запросы веб-сайте (`SameSiteMode.Strict`) или запросов между сайтами, с использованием безопасного HTTP-методов и запросов веб-сайте (`SameSiteMode.Lax`). Если задано значение `SameSiteMode.None`, не задано значение заголовка файла cookie. Обратите внимание, что [промежуточного слоя политики](#cookie-policy-middleware) может перезаписать предоставленное значение. Для поддержки проверки подлинности OAuth, значение по умолчанию — `SameSiteMode.Lax`. Дополнительные сведения см. в разделе [нарушена из-за политики SameSite файла cookie проверки подлинности OAuth](https://github.com/aspnet/Security/issues/1231). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | Флаг, указывающий, если созданный файл cookie был ограничен HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и в запросе (`CookieSecurePolicy.SameAsRequest`). Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`. |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | Наборы `DataProtectionProvider` , используемый для создания по умолчанию `TicketDataFormat`. Если `TicketDataFormat` свойство задано, `DataProtectionProvider` параметр не используется. Если не указано, используется поставщик защиты данных приложения по умолчанию. |
| [События](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | Обработчик вызывает методы поставщика, который предоставить элемент управления приложения в определенные моменты обработки. Если `Events` не переданы, предоставляется экземпляр по умолчанию, не выполняет никаких действий при вызове методов. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | Используется как тип службы для получения `Events` экземпляром, а не свойства. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | `TimeSpan` После которого срока действия билета проверки подлинности, хранящийся в файл cookie. `ExpireTimeSpan` добавляется на текущий момент времени, чтобы создать срок действия билета. `ExpiredTimeSpan` Значение всегда переходит в зашифрованных AuthTicket, проверен сервером. Он также может перейти в [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) заголовок, но только если `IsPersistent` имеет значение. Чтобы задать `IsPersistent` для `true`, Настройка [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) передается `SignInAsync`. Значение по умолчанию `ExpireTimeSpan` — 14 дней. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | Предоставляет путь к предоставить "302 Found" (URL-адрес перенаправления) при активации `HttpContext.ChallengeAsync`. Добавляется текущий URL-адрес, вызвавший ответ 401 `LoginPath` как параметр строки запроса с именем, `ReturnUrlParameter`. Запрос один раз на `LoginPath` предоставляет новое входа удостоверение, `ReturnUrlParameter` значение используется для перенаправления браузера обратно в URL-адрес, вызвавший исходный код несанкционированного состояния. Значение по умолчанию — `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | Если `LogoutPath` предоставляется обработчику, а затем перенаправляет запрос по этому пути на основе значения из `ReturnUrlParameter`. Значение по умолчанию — `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | Определяет имя параметра строки запроса, которое добавляется с помощью обработчика для ответа 302 Found (перенаправление URL-адрес). `ReturnUrlParameter` используется при получении запроса на `LoginPath` или `LogoutPath` для возврата в браузере на исходный URL-адрес после выполнения действия имени входа или выхода. Значение по умолчанию — `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | Необязательный контейнер, используемый для хранения идентификаторов всех запросов. При использовании только идентификатор сеанса отправляется клиенту. `SessionStore` можно использовать для устранения потенциальных проблем с удостоверениями большой. |
| [slidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | Флаг, указывающий, если новый файл cookie с обновленной срок действия должен быть выдан динамически. Это может произойти на любой запрос, где текущий период истечения срока действия файла cookie более чем на 50% срок действия истек. Новый срок перемещается вперед быть текущая дата плюс `ExpireTimespan`. [Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` при вызове метода `SignInAsync`. Абсолютный срок действия можно повысить безопасность приложения, ограничивая количество времени, который действителен cookie проверки подлинности. Значение по умолчанию — `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | `TicketDataFormat` Используется для применения и снятия защиты identity и других свойств, которые хранятся в значении cookie. Если не указано, `TicketDataFormat` создается с помощью [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0). |
| [Проверка](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | Метод, который проверяет, что параметры являются допустимыми. |

Задайте `CookieAuthenticationOptions` в конфигурации службы для проверки подлинности в `ConfigureServices` метод:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

ASP.NET Core 1.x использует cookie [по промежуточного слоя](xref:fundamentals/middleware/index) , сериализует участника-пользователя в зашифрованном файле cookie. При последующих запросах куки-файл проверяется, а основной сервер заново и назначенные `HttpContext.User` свойство.

Установка [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета NuGet в проект. Этот пакет содержит по промежуточного слоя.

Используйте `UseCookieAuthentication` метод в `Configure` метод в вашей *Startup.cs* файл перед `UseMvc` или `UseMvcWithDefaultRoute`:

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

**CookieAuthenticationOptions Options**

[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) класс используется для настройки параметров поставщика проверки подлинности.

| Параметр | Описание: |
| ------ | ----------- |
| [Значение AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | Задает схему проверки подлинности. `AuthenticationScheme` полезно, если имеется несколько экземпляров проверки подлинности, и вы хотите [авторизация с определенной схемой](xref:security/authorization/limitingidentitybyscheme). Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы. Вы можете указать любое строковое значение, являющийся отличительным признаком схемы. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Задает значение, указывающее, что файл cookie проверки подлинности следует выполняется при каждом запросе и пытаются проверить и воссоздания любому участнику сериализованный созданные им. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | Если значение равно true, по промежуточного слоя проверки подлинности обрабатывает автоматического проблем. Если значение равно false, по промежуточного слоя проверки подлинности только изменяет ответы, если они явно не указано `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство на все утверждения, созданные по промежуточного слоя проверки подлинности. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | Имя домена, где обслуживается куки-файл. По умолчанию это имя узла запроса. Браузер обслуживает только файл cookie для сопоставления имени узла. Вы можете настроить этот параметр для файлов cookie, доступных на любом узле в вашем домене. Например, значение домена файла cookie `.contoso.com` сделает его доступным для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | Флаг, указывающий файл cookie должен быть доступен только на серверы. Это значение изменится на `false` разрешает клиентские скрипты, чтобы получить доступ к куки-файл и может открыть свое приложение для хищения файлов cookie, должны иметь приложения [межузловых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость. Значение по умолчанию — `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | Использовать для изоляции приложений, выполняющихся в то же имя узла. Если у вас есть приложение, запущенное в `/app1` и нужно ограничить файлы cookie для этого приложения, задайте `CookiePath` свойства `/app1`. Таким образом, файл cookie доступна только для запросов к `/app1` и любого приложения под ним. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | Флаг, указывающий, если созданный файл cookie был ограничен HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и в запросе (`CookieSecurePolicy.SameAsRequest`). Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`. |
| [Описание](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | Дополнительные сведения о типе проверки подлинности, который становится доступен в приложение. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | `TimeSpan` После которого срока действия билета проверки подлинности. Оно добавляется на текущий момент времени, чтобы создать срок действия билета. Чтобы использовать `ExpireTimeSpan`, необходимо задать `IsPersistent` для `true` в `AuthenticationProperties` передается `SignInAsync`. Значение по умолчанию — 14 дней. |
| [slidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | Флаг, указывающий, сбрасывается ли дату истечения срока действия файла cookie при более половины `ExpireTimeSpan` промежуток времени истек. Новый срок перемещается вперед быть текущая дата плюс `ExpireTimespan`. [Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` при вызове метода `SignInAsync`. Абсолютный срок действия можно повысить безопасность приложения, ограничивая количество времени, который действителен cookie проверки подлинности. Значение по умолчанию — `true`. |

Задайте `CookieAuthenticationOptions` для по промежуточного слоя проверки подлинности, в `Configure` метод:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a>По промежуточного слоя для файлов cookie политики

[По промежуточного слоя для файлов cookie политики](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) обеспечивает возможности политики файлов cookie в приложении. Добавлением по промежуточного слоя в конвейере обработки приложение является конфиденциальным; порядок он влияет только на компоненты, зарегистрированные за ним в конвейере.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) для по промежуточного слоя политики позволяют управлять общих характеристик обработки файлов cookie и обработчик в обработчики обработки файлов cookie, если файлы cookie добавляется или удаляется.

| Свойство | Описание: |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Влияет ли файлы cookie должен быть HttpOnly, который является флагом, показывающим куки-файл должен быть доступен только на серверы. Значение по умолчанию — `HttpOnlyPolicy.None`. |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Влияет на атрибут файла cookie веб-сайте (см. ниже). Значение по умолчанию — `SameSiteMode.Lax`. Этот параметр доступен для ASP.NET Core 2.0 +. |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Вызывается, когда файл cookie добавляется. |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Вызывается при удалении файла cookie. |
| [Защита](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | Влияет ли файлы cookie должен быть Secure. Значение по умолчанию — `CookieSecurePolicy.None`. |

**MinimumSameSitePolicy** (ASP.NET Core 2.0 + только)

Значение по умолчанию `MinimumSameSitePolicy` значение `SameSiteMode.Lax` для разрешения проверки подлинности OAuth2. Для строго ограничивает политику веб-сайте `SameSiteMode.Strict`, задайте `MinimumSameSitePolicy`. Несмотря на то, что этот параметр приводит к сбою OAuth2 и других схем проверки подлинности независимо от источника, оно повышает уровень безопасности cookie для других типов приложений, которые не полагаются на обработку независимо от источника запроса.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Параметр политики промежуточного слоя для `MinimumSameSitePolicy` может повлиять на ваши равным `Cookie.SameSite` в `CookieAuthenticationOptions` параметры в соответствии с в таблице ниже.

| MinimumSameSitePolicy | Cookie.SameSite | Результирующий параметр Cookie.SameSite |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Создайте файл cookie проверки подлинности

Чтобы создать файл cookie, содержащий сведения о пользователе, то необходимо создать [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal). Сведения о пользователе сериализуется и сохраняется в файле cookie. 

::: moniker range=">= aspnetcore-2.0"

Создание [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) с любыми обязательными [утверждения](/dotnet/api/system.security.claims.claim)s и вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) для входа пользователя:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) для входа пользователя:

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

`SignInAsync` создает зашифрованный файл cookie и добавляет его в текущий ответ. Если вы не укажете `AuthenticationScheme`, используется схема по умолчанию.

На самом деле, шифрования, используемых — ASP.NET Core [защиты данных](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) системы. Если вы размещаете приложения на нескольких компьютерах, балансировки нагрузки для приложений или с помощью веб-ферме, то необходимо [настроить защиту данных](xref:security/data-protection/configuration/overview) использование одного набора ключей и идентификатор приложения.

## <a name="sign-out"></a>Выйти

::: moniker range=">= aspnetcore-2.0"

Чтобы выйти из системы текущего пользователя и удалить их файл cookie, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Чтобы выйти из системы текущего пользователя и удалить их файл cookie, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

Если вы не используете `CookieAuthenticationDefaults.AuthenticationScheme` (или файлы «cookie») как схему (например, «ContosoCookie»), укажите схему, используемую при настройке поставщика проверки подлинности. В противном случае используется схема по умолчанию.

## <a name="react-to-back-end-changes"></a>Реагировать на изменения серверной части

Создав файл cookie, он становится единственным источником удостоверения. Даже при отключении пользователя в системах серверной системой проверки подлинности файла cookie не имеет сведений о это и пользователь остается в системе до тех пор, пока их файл cookie является допустимым.

[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) событий в ASP.NET Core 2.x или [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) метод в ASP.NET Core 1.x можно использовать для перехвата и переопределение проверки подлинности файла cookie. Этот подход уменьшает риск появления отозванных пользователей, обращающихся к приложению.

Один из способов проверки cookie основан на слежении за при изменении пользователя базы данных. Если базы данных не был изменен с момента выпуска файла cookie пользователя, нет необходимости пройти повторную аутентификацию пользователя, если их куки-файл все еще действителен. Чтобы реализовать этот сценарий базы данных, реализованное в `IUserRepository` в этом примере хранит `LastChanged` значение. При обновлении любого пользователя в базе данных, `LastChanged` имеет значение текущего времени.

Чтобы сделать недействительным файл cookie, когда база данных изменяется в зависимости от `LastChanged` создайте файл cookie с `LastChanged` утверждение, содержащее текущий `LastChanged` значение из базы данных:

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

::: moniker range=">= aspnetcore-2.0"

Для реализации переопределения для `ValidatePrincipal` события, написание метод со следующей сигнатурой в классе, унаследованном от [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

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

Зарегистрируйте экземпляр события во время регистрации службы файл cookie в `ConfigureServices` метод. Укажите регистрацию ограниченной службы для вашего `CustomCookieAuthenticationEvents` класса:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Для реализации переопределения для `ValidateAsync` события, написание метод со следующей сигнатурой:

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

Удостоверение ASP.NET Core реализует эту проверку в процессе его [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync). Пример выглядит следующим образом:

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

::: moniker-end

Рассмотрим ситуацию, в котором имя пользователя будет обновлено &mdash; решение, которое не влияет на безопасность любым способом. Если вы хотите безболезненно обновить участника-пользователя, вызовите `context.ReplacePrincipal` и задайте `context.ShouldRenew` свойства `true`.

> [!WARNING]
> Подход, описанный здесь активируется при каждом запросе. Это может привести к снижению производительности для приложения.

## <a name="persistent-cookies"></a>Постоянные файлы cookie

Требуется файл cookie для сохранения между сеансами браузера. Такое сохранение должны включаться только с согласия пользователя с помощью флажка «Запомнить мои» на имя входа или похожий механизм. 

Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, который выдерживает его через браузер замыкания. Учитываются параметры скользящего срока действия ранее была настроена. Если срок действия файла cookie, при закрытии браузера, браузер удаляет файл cookie после перезагрузки.

::: moniker range=">= aspnetcore-2.0"

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

::: moniker-end

## <a name="absolute-cookie-expiration"></a>Срок действия файла cookie абсолютный

Можно задать абсолютный срок действия с `ExpiresUtc`. Чтобы создать постоянный файл cookie, необходимо также задать `IsPersistent`; в противном случае файл cookie создается с временем жизни на основе сеансов и срок действия может истечь до или после проверки подлинности билета, она хранит. Когда `ExpiresUtc` устанавливается на `SignInAsync`, этот параметр переопределяет значение `ExpireTimeSpan` параметр `CookieAuthenticationOptions`, если задать.

Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, который выполняется в течение 20 минут. Это не учитывает параметры скользящего срока действия ранее была настроена.

::: moniker range=">= aspnetcore-2.0"

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

::: moniker-end

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Изменения AUTH 2.0 / объявления миграции](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [Проверок роли на основе политик](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
