---
title: Сохранять дополнительные утверждения и маркеры от внешних поставщиков в ASP.NET Core
author: guardrex
description: Узнайте, как установить дополнительные утверждения и маркеры от внешних поставщиков.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 6386dd5f0bd901be01cf56081a6b9ca7f408f9f9
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336160"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>Сохранять дополнительные утверждения и маркеры от внешних поставщиков в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Приложения ASP.NET Core можно установить дополнительные утверждения и маркеры от внешних поставщиков проверки подлинности, таких как Facebook, Google, Майкрософт и Twitter. Каждый поставщик раскрывает различные сведения о пользователях на платформе, но шаблон для получения и преобразования данных пользователя в дополнительные утверждения одинаков.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisite"></a>Предварительные требования

Решите, какие поставщики внешней проверки подлинности для поддержки в приложении. Для каждого поставщика Регистрация приложения и получить идентификатор клиента и секрет клиента. Дополнительные сведения см. в разделе <xref:security/authentication/social/index>. [Пример приложения](#sample-app-instructions) использует [поставщик проверки подлинности Google](xref:security/authentication/google-logins).

## <a name="authentication-provider-configuration"></a>Конфигурация поставщика проверки подлинности

### <a name="set-the-client-id-and-client-secret"></a>Задание идентификатора и секрета клиента

Поставщик проверки подлинности OAuth устанавливает отношение доверия с помощью приложения с помощью идентификатора клиента и секрет клиента. Идентификатор клиента и секрет клиента, создаваемые для приложения поставщика внешней проверки подлинности при регистрации приложения с поставщиком. Каждый внешний поставщик, приложение использует должны быть настроены независимо друг от друга идентификатор клиента и секрет клиента поставщика. Дополнительные сведения см. в разделах поставщика внешней проверки подлинности, которые применяются к вашему сценарию:

* [Проверка подлинности Facebook](xref:security/authentication/facebook-logins)
* [Проверка подлинности Google](xref:security/authentication/google-logins)
* [Проверка подлинности Майкрософт](xref:security/authentication/microsoft-logins)
* [Проверка подлинности Twitter](xref:security/authentication/twitter-logins)
* [Другие поставщики проверки подлинности](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

Пример приложения настраивает поставщик проверки подлинности Google с Идентификатором клиента и секрет клиента, предоставляемый Google:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

### <a name="establish-the-authentication-scope"></a>Установить область проверки подлинности

Укажите список разрешений для получения от поставщика, путем указания <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. В следующей таблице отображаются области проверки подлинности для распространенных внешних поставщиков.

| Поставщик  | Область                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/plus.login`                     |
| Майкрософт | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

Пример приложения добавляет Google `plus.login` область для запроса входа Google + в разрешениях:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

### <a name="map-user-data-keys-and-create-claims"></a>Сопоставить ключи данных пользователя и создать утверждения

В параметрах поставщика, укажите <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> для каждого ключа в внешнего поставщика JSON пользовательские данные для удостоверения приложения для чтения при входе. Дополнительные сведения о типах утверждений, см. в разделе <xref:System.Security.Claims.ClaimTypes>.

Пример приложения создает <xref:System.Security.Claims.ClaimTypes.Gender> утверждение от `gender` ключа в данных пользователя Google:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

В <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) осуществил вход в приложение с помощью <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>. Во время входа в систему <xref:Microsoft.AspNetCore.Identity.UserManager`1> можно хранить `ApplicationUser` утверждения для пользовательских данных, доступных из <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

В примере приложения `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) устанавливает <xref:System.Security.Claims.ClaimTypes.Gender> утверждения для со знаком в `ApplicationUser`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

### <a name="save-the-access-token"></a>Сохранить маркер доступа

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> Определяет, следует ли хранить маркеров доступа и обновления в <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> после успешной авторизации. `SaveTokens` имеет значение `false` по умолчанию, чтобы уменьшить размер файла cookie окончательной проверки подлинности.

В примере приложения задает значение `SaveTokens` для `true` в <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

Когда `OnPostConfirmationAsync` выполняет, сохранить маркер доступа ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) из внешнего поставщика в `ApplicationUser`в `AuthenticationProperties`.

Это пример приложения сохраняет маркер доступа в:

* `OnPostConfirmationAsync` &ndash; Выполняет для регистрация нового пользователя.
* `OnGetCallbackAsync` &ndash; Выполняется, если ранее зарегистрированный пользователь входит в приложение.

*Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

### <a name="how-to-add-additional-custom-tokens"></a>Как добавить дополнительные пользовательские маркеры

Чтобы продемонстрировать, как добавить пользовательский маркер, который хранится как часть `SaveTokens`, пример приложения добавляет <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> с текущим <xref:System.DateTime> для [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) из `TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a>Примеры инструкций по назначению приложения

Этот пример демонстрирует способы:

* Получите Пол пользователя от Google и сохраняет Пол утверждение со значением.
* Store маркер доступа Google в пользователя `AuthenticationProperties`.

Чтобы использовать пример приложения:

1. Регистрация приложения и получить допустимый идентификатор клиента и секрет клиента для проверки подлинности Google. Дополнительные сведения см. в разделе <xref:security/authentication/google-logins>.
1. Указание идентификатора клиента и секрет клиента для приложения в <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> из `Startup.ConfigureServices`.
1. Запустите приложение и запрос страницы Мои утверждения. Если пользователь не вошел в систему, которую перенаправляет пользователя приложение в Google. Войдите с помощью Google. Google перенаправляет пользователя обратно к приложению (`/Home/MyClaims`). Пользователь проходит проверку подлинности и загрузке страницы Мои утверждения. Пол утверждения находится в каталоге **утверждения пользователей** со значением, полученный из Google. Маркер доступа появится в **свойства проверки подлинности**.

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```
