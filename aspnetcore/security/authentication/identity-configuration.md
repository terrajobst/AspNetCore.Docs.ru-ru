---
title: Настройка удостоверения ASP.NET Core
author: AdrienTorris
description: Понять значения по умолчанию ASP.NET Core Identity и узнайте, как настроить свойства удостоверения для использования пользовательских значений.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 823182bed2cb953e07f9374d135868aeb2be9c60
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210123"
---
# <a name="configure-aspnet-core-identity"></a>Настройка удостоверения ASP.NET Core

Удостоверение ASP.NET Core использует значения по умолчанию для параметров, таких как политики паролей, блокировки и куки-файл конфигурации. Эти параметры можно переопределить в `Startup` класса.

## <a name="identity-options"></a>Параметры удостоверений

[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) класс представляет параметры, которые могут использоваться для настройки системы удостоверений. `IdentityOptions` необходимо задать **после** вызова `AddIdentity` или `AddDefaultIdentity`.

### <a name="claims-identity"></a>Идентификатор утверждений

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) указывает [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) с помощью свойства, показанные в следующей таблице.

| Свойство | Описание | Значение по умолчанию |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Возвращает или задает тип утверждения, используемый для утверждения роли. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Возвращает или задает тип утверждения, используемый для отметки утверждения безопасности. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Возвращает или задает тип утверждения, используемый для утверждение идентификатора пользователя. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Возвращает или задает тип утверждения, используемый для утверждения с именем пользователя. | [Содержащееся в ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Блокировки

Блокировки задается в [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) метод:

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

Приведенный выше код основан на `Login` удостоверение шаблона. 

Задаются параметры блокировки `StartUp.ConfigureServices`:

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

Указанный выше код задает [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) со значениями по умолчанию.

Сбрасывает счетчик попыток неудачные попытки доступа успешной проверки подлинности и сбрасывает часы.

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) указывает [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) со свойствами, приведенных в таблице.

| Свойство | Описание | Значение по умолчанию |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Определяет, если новый пользователь может быть заблокирован. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | Количество времени, пользователь будет заблокирован при возникновении блокировки. | 5 минут |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Число неудачных попыток доступа до пользователя будет заблокирована, если включена блокировка. | 5 |

### <a name="password"></a>Пароль

По умолчанию удостоверение требует, что пароли содержат символ верхнего регистра, строчную букву, цифру и специальный символ. Пароли должны быть по крайней мере шесть символов. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) можно задать в `Startup.ConfigureServices`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) указывает [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) со свойствами, приведенных в таблице.

::: moniker range=">= aspnetcore-2.0"

| Свойство | Описание | Значение по умолчанию |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Требуется число в диапазоне от 0 до 9, в пароле. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Минимальная длина пароля. | 6 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Должен стоять символ нижнего регистра в пароле. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Требуется не буквенно-цифровых символов в пароле. | `true` |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Применяется только к ASP.NET Core 2.0 или более поздней версии.<br><br> Необходимо число уникальных символов в пароле. | 1 |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Требуется символ верхнего регистра в пароле. | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Свойство | Описание | Значение по умолчанию |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Требуется число в диапазоне от 0 до 9, в пароле. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Минимальная длина пароля. | 6 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Должен стоять символ нижнего регистра в пароле. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Требуется не буквенно-цифровых символов в пароле. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Требуется символ верхнего регистра в пароле. | `true` |

::: moniker-end

### <a name="sign-in"></a>Вход

В следующем коде наборы `SignIn` (по умолчанию значения параметров):

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) указывает [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) со свойствами, приведенных в таблице.

| Свойство | Описание | Значение по умолчанию |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Требуется подтверждена электронной почты для входа. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Требуется номер подтверждена телефона для входа. | `false` |

### <a name="tokens"></a>Токены

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) указывает [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) со свойствами, приведенных в таблице.

|                                                        Свойство                                                         |                                                                                      Описание                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       Возвращает или задает `AuthenticatorTokenProvider` используется для проверки двухфакторной входы в систему с помощью средства проверки подлинности.                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     Возвращает или задает `ChangeEmailTokenProvider` используется для создания токенов, используемых в сообщениях электронной почты подтверждением изменение электронной почты.                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      Возвращает или задает `ChangePhoneNumberTokenProvider` использовать для создания маркеров, используемых для изменения номера телефонов.                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             Возвращает или задает поставщик маркеров, используемый для создания токенов, используемых в сообщениях электронной почты для подтверждения учетной записи.                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | Возвращает или задает [IUserTwoFactorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) используется для создания токенов, используемых в сообщениях электронной почты для сброса пароля. |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                Используется для конструирования [Поставщик маркера пользователя](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) с ключом, используемым в качестве имени поставщика.                 |

### <a name="user"></a>Пользователь

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) указывает [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) со свойствами, приведенных в таблице.

| Свойство | Описание | Значение по умолчанию |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Допустимые символы в имени пользователя. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-.\_@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Требуется уникальный адрес электронной почты для каждого пользователя. | `false` |

### <a name="cookie-settings"></a>Параметры файлов cookie

Настройка приложения в файле cookie `Startup.ConfigureServices`. [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) должен вызываться **после** вызова `AddIdentity` или `AddDefaultIdentity`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

Дополнительные сведения см. в разделе [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).

## <a name="password-hasher-options"></a>Модуль хэширования паролей

<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> Возвращает и задает параметры для хэширования пароля.

| Параметр | Описание: |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | Режим совместимости, используемый при хэшировании новые пароли. По умолчанию — <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>. Первый байт хэшированный пароль, который называется *формат маркера*, указывает версию алгоритм хэширования, используемый для хэширования пароля. При проверке пароля с хэшем, <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> метод Выбор правильного алгоритма, в зависимости от первого байта. Клиент способен для проверки подлинности независимо от того, из которых версией алгоритма, использованный для хэширования пароля. Настройка режима совместимости влияет на хэширования *новые пароли*. |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | Число итераций, используемых при хэшировании пароли с помощью PBKDF2. Это значение используется, только если <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> присваивается <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>. Значение должно быть целым положительным числом, значение по умолчанию — `10000`. |

В следующем примере <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> присваивается `12000` в `Startup.ConfigureServices`:

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
