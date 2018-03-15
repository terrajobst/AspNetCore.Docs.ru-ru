---
title: "Настройка удостоверения ASP.NET Core"
author: AdrienTorris
description: "Обзор ASP.NET Core Identity значения по умолчанию и подробные сведения о настройке свойства удостоверения для использования пользовательских значений."
manager: wpickett
ms.author: scaddie
ms.date: 03/06/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: f8be8a555454a99a3e75b5cd3d42c11e1d7b2b7e
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="configure-identity"></a>Настройка удостоверения

Удостоверение ASP.NET Core использует конфигурацию по умолчанию для параметров, таких как политика паролей, время блокировки и параметры файлов cookie. Эти параметры можно переопределить в приложении `Startup` класса.

## <a name="identity-options"></a>Параметры удостоверений

[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) класс представляет параметры, которые можно использовать для настройки системы удостоверений.

### <a name="claims-identity"></a>Идентификатор утверждений

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) указывает [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) с свойства, показанные в таблице.

| Свойство. | Описание | Значение по умолчанию |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Возвращает или задает тип утверждения, используемый для утверждения роли. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Возвращает или задает тип утверждения, используемый для отметки утверждения безопасности. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Возвращает или задает тип утверждения, используемый для утверждения идентификатора пользователя. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Возвращает или задает тип утверждения, используемый для утверждения имени пользователя. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Блокировки

Блокирует работу пользователя в течение заданного времени после заданного числа неудачных попыток доступа (по умолчанию: 5-минутного блокировки после 5 неудачных попыток доступа). Сбрасывает счетчик неудачных доступов попыток прошли проверку подлинности и сбрасывает часы.

В следующем примере значения по умолчанию:

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

Убедитесь, что [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) задает `lockoutOnFailure` для `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) указывает [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) с свойства, показанные в таблице.

| Свойство. | Описание | Значение по умолчанию |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Определяет, если новый пользователь может быть заблокирован. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | Количество времени, пользователь будет заблокирован при возникновении блокировки. | 5 минут |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Число неудачных попыток доступа до пользователя будет заблокирована, если включена блокировка. | 5 |

### <a name="password"></a>Пароль

По умолчанию удостоверение требует, что пароли содержат символ верхнего регистра, строчные буквы, цифры и не алфавитно-цифровой символ. Пароли должны быть по крайней мере шесть символов. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) можно изменить в `Startup.ConfigureServices`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 добавлен [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) свойство. В противном случае параметры аналогичны ASP.NET Core 1.x.

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) указывает [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) с свойства, показанные в таблице.

| Свойство. | Описание | Значение по умолчанию |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Необходимо указать число от 0-9 и пароль. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Минимальная длина пароля. | 6 |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Применяется только на основных ASP.NET 2.0 или более поздней версии.<br><br> Требует количества уникальных символов в пароле. | 1 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Требуется нижний регистр символов в пароле. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Требуется не буквенно-цифровых символов в пароле. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Требуется символ верхнего регистра в пароле. | `true` |

### <a name="sign-in"></a>вход

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) указывает [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) с свойства, показанные в таблице.

| Свойство. | Описание | Значение по умолчанию |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Требует подтверждения электронной почты для входа. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Требуется номер телефона, подтвержденной для входа. | `false` |

### <a name="tokens"></a>лексемы

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) указывает [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) с свойства, показанные в таблице.

| Свойство. | Описание: |
| -------- | ----------- |
| [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider) | Возвращает или задает `AuthenticatorTokenProvider` используется для проверки двухфакторной входа в систему с помощью средства проверки подлинности. |
| [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider) | Возвращает или задает `ChangeEmailTokenProvider` используется для создания токенов, используемых в сообщениях электронной почты Подтверждение изменений по электронной почте. |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) | Возвращает или задает `ChangePhoneNumberTokenProvider` используется для создания токенов, используемых при изменении номера телефонов. |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) | Возвращает или задает поставщик маркеров, используемый для создания токенов, используемых в сообщениях электронной почты для подтверждения учетной записи. |
| [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider) | Возвращает или задает [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) используется для создания токенов, используемых в сообщениях электронной почты для сброса пароля. |
| [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap) | Используется для создания [пользовательского поставщика маркеров](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) с ключом, который используется в качестве имени поставщика. |

### <a name="user"></a>Пользовательская

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) указывает [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) с свойства, показанные в таблице.

| Свойство. | Описание | Значение по умолчанию |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Допустимые символы в имени пользователя. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Требуется иметь уникальный адрес электронной почты пользователя. | `false` |

## <a name="cookie-settings"></a>Параметры файлов cookie

Настройка приложения в файле cookie `Startup.ConfigureServices`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions) имеет следующие свойства:

| Свойство. | Описание: |
| -------- | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath) | Уведомляет обработчик, что следует изменить исходящий *403 Запрещено* код состояния в *перенаправление 302* на заданный путь.<br><br>Значение по умолчанию — `/Account/AccessDenied`. |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme) | Применяется только к ASP.NET Core 1.x.<br><br> Логическое имя для проверки подлинности конкретной схемы. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate) | Применяется только к ASP.NET Core 1.x.<br><br> Если значение равно true, файл cookie проверки подлинности запустите при каждом запросе и попытка проверки и воссоздания любому участнику сериализованный она создана. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge) | Применяется только к ASP.NET Core 1.x.<br><br> Значение true, если по промежуточного слоя проверки подлинности обрабатывает автоматического проблем. Если значение равно false, проверка подлинности по промежуточного слоя лишь изменяет ответы, если они явным образом указано `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer) | Возвращает или задает поставщик, который должен использоваться для любые утверждения, которые создаются (наследуется от [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)). |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) | Домен, связанный с cookie. |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration) | Возвращает или задает время существования файлов cookie HTTP (не файл cookie проверки подлинности). Это свойство переопределено с [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan). Не должны использоваться в контексте CookieAuthentication. |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) | Указывает, доступен ли cookie клиентским сценариям.<br><br>Значение по умолчанию — `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) | Имя файла cookie.<br><br>Значение по умолчанию — `.AspNetCore.Cookies`. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) | Путь к файлу cookie. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) | `SameSite` Атрибут файла cookie.<br><br>Значение по умолчанию — [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy) | [CookieSecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) конфигурации.<br><br>Значение по умолчанию — [CookieSecurePolicy.SameAsRequest](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy). |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain) | Применяется только к ASP.NET Core 1.x.<br><br> Имя домена, где обслуживают куки-файл. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly) | Применяется только к ASP.NET Core 1.x.<br><br> Флаг, указывающий куки-файл должен быть доступен только на серверы.<br><br>Значение по умолчанию — `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath) | Применяется только к ASP.NET Core 1.x.<br><br> Использовать для изоляции приложений, выполняющихся в то же имя узла. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure) | Применяется только к ASP.NET Core 1.x.<br><br> Флаг, указывающий, следует ли созданный файл cookie ограничиваются HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и при запросе (`CookieSecurePolicy.SameAsRequest`).<br><br>Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`. |
| [CookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.cookiemanager) | Компонент, используемый для получения файлов cookie из запроса или задать их в ответе. | [ChunkingCookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.chunkingcookiemanager) |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider) | Если задано, поставщик используемые [CookieAuthenticationHandler](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationhandler) для защиты данных. |
| [Описание](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description) | Применяется только к ASP.NET Core 1.x.<br><br> Дополнительные сведения о типе проверки подлинности доступны для приложения. |
| [События](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events) | Обработчик вызывает методы в поставщике, который обеспечивает элементы управления приложения в определенных точках, в котором происходит обработка. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype) | Если задано, службы тип, получаемый `Events` экземпляра вместо свойства (наследуется от [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)). |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan) | Элементы управления процессорное время билета проверки подлинности, хранящихся в файл cookie остается действительным с момента его создания.<br><br>Значение по умолчанию — 14 дней. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath) | Если пользователь не авторизован, он будет перенаправлен на этот путь для имени входа.<br><br>Значение по умолчанию — `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath) | При выходе пользователя, он будет перенаправлен на этот путь.<br><br>Значение по умолчанию — `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter) | Определяет имя параметра строки запроса, который добавляется по промежуточного слоя при *проверки подлинности 401* код состояния изменяется на *перенаправление 302* пути входа.<br><br>Значение по умолчанию — `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore) | Необязательный контейнер, в котором хранится удостоверение для различных запросов. |
| [slidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration) | Если значение равно true, новый файл cookie выдается с новым окончанием срока действия текущего файла cookie при более половины срока.<br><br>Значение по умолчанию — `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat) | `TicketDataFormat` Используется для установки и снятия защиты identity и других свойств, хранящихся в значении файла cookie. |
