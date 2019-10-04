---
title: Сохранение дополнительных утверждений и маркеров от внешних поставщиков в ASP.NET Core
author: guardrex
description: Узнайте, как устанавливать дополнительные утверждения и токены от внешних поставщиков.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/01/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: cdf263df8d1aa17ea3820a16ecbd10abce9d683d
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925156"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>Сохранение дополнительных утверждений и маркеров от внешних поставщиков в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

::: moniker range=">= aspnetcore-3.0"

Приложение ASP.NET Core может устанавливать дополнительные утверждения и маркеры от внешних поставщиков проверки подлинности, таких как Facebook, Google, Microsoft и Twitter. Каждый поставщик раскрывает разные сведения о пользователях на своей платформе, но шаблон для получения и преобразования пользовательских данных в дополнительные утверждения одинаковы.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Предварительные требования

Решите, какие внешние поставщики проверки подлинности поддерживаются в приложении. Для каждого поставщика Зарегистрируйте приложение и получите идентификатор клиента и секрет клиента. Дополнительные сведения см. в разделе <xref:security/authentication/social/index>. В примере приложения используется [поставщик проверки подлинности Google](xref:security/authentication/google-logins).

## <a name="set-the-client-id-and-client-secret"></a>Задание идентификатора клиента и секрета клиента

Поставщик проверки подлинности OAuth устанавливает отношение доверия с приложением, используя идентификатор клиента и секрет клиента. Идентификатор клиента и значения секрета клиента создаются для приложения внешним поставщиком проверки подлинности при регистрации приложения в поставщике. Каждый внешний поставщик, используемый приложением, должен быть настроен независимо с ИДЕНТИФИКАТОРом клиента и секретом клиента поставщика. Дополнительные сведения см. в разделах, посвященных внешнему поставщику проверки подлинности, которые относятся к вашему сценарию.

* [Проверка подлинности Facebook](xref:security/authentication/facebook-logins)
* [Проверка подлинности Google](xref:security/authentication/google-logins)
* [Проверка подлинности Майкрософт](xref:security/authentication/microsoft-logins)
* [Проверка подлинности Twitter](xref:security/authentication/twitter-logins)
* [Другие поставщики проверки подлинности](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

Пример приложения настраивает поставщик проверки подлинности Google с ИДЕНТИФИКАТОРом клиента и секретом клиента, предоставленным Google:

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>Определение области проверки подлинности

Укажите список разрешений для получения от поставщика, указав <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. В следующей таблице приведены области проверки подлинности для общих внешних поставщиков.

| Поставщик  | `Scope`                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Майкрософт | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

В примере приложения платформа Google `userinfo.profile` автоматически добавляется платформой при вызове <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> на <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>. Если приложению требуются дополнительные области, добавьте их в параметры. В следующем примере область Google `https://www.googleapis.com/auth/user.birthday.read` добавляется для получения дня рождения пользователя:

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>Сопоставьте ключи данных пользователя и создайте утверждения

В параметрах поставщика укажите <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> или <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> для каждого ключа или подраздела в данных пользователя JSON внешнего поставщика, чтобы удостоверение приложения было прочитано при входе. Дополнительные сведения о типах утверждений см. в разделе <xref:System.Security.Claims.ClaimTypes>.

Пример приложения создает утверждения (`urn:google:locale`) и Picture (`urn:google:picture`) из ключей `locale` и `picture` в данных пользователя Google:

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

В <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*> <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) подписывается в приложение с <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>. В процессе входа <xref:Microsoft.AspNetCore.Identity.UserManager%601> может хранить `ApplicationUser` утверждений для пользовательских данных, доступных из <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

В примере приложения `OnPostConfirmationAsync` (*Account/екстерналлогин. cshtml. CS*) устанавливает утверждения языков (`urn:google:locale`) и picture (`urn:google:picture`) для входа в `ApplicationUser`, включая утверждение для <xref:System.Security.Claims.ClaimTypes.GivenName>:

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

По умолчанию утверждения пользователя хранятся в файле cookie проверки подлинности. Если файл cookie для проверки подлинности слишком большой, это может привести к сбою приложения по следующим причинам:

* Браузер обнаруживает, что заголовок файла cookie слишком длинный.
* Общий размер запроса слишком велик.

Если для обработки запросов пользователей требуется большой объем пользовательских данных:

* Ограничьте количество и размер утверждений пользователей для обработки запроса только тем, что требуется приложению.
* Используйте пользовательский <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> для промежуточного слоя проверки подлинности "cookie" <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> для хранения удостоверений между запросами. Сохраняйте большие объемы информации об удостоверениях на сервере, при этом клиенту отправляется только небольшой ключ идентификатора сеанса.

## <a name="save-the-access-token"></a>Сохранение маркера доступа

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> определяет, должны ли маркеры доступа и обновления храниться в <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> после успешной авторизации. `SaveTokens` по умолчанию имеет значение `false`, чтобы уменьшить размер файла cookie окончательной проверки подлинности.

Пример приложения устанавливает значение `SaveTokens` в `true` в <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

При выполнении `OnPostConfirmationAsync` Сохраните маркер доступа ([екстерналлогининфо. аусентикатионтокенс](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) из внешнего поставщика в `ApplicationUser` `AuthenticationProperties`.

Пример приложения сохраняет маркер доступа в `OnPostConfirmationAsync` (Новая регистрация пользователя) и `OnGetCallbackAsync` (ранее зарегистрированный пользователь) в *Account/екстерналлогин. cshtml. CS*:

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>Добавление дополнительных настраиваемых токенов

Чтобы продемонстрировать, как добавить пользовательский маркер, который хранится в составе `SaveTokens`, пример приложения добавляет <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> с текущим <xref:System.DateTime> для [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) из `TicketCreated`:

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a>Создание и добавление утверждений

Платформа предоставляет общие действия и методы расширения для создания и добавления заявок в коллекцию. Дополнительные сведения см. в разделах <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> и <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.

Пользователи могут определять пользовательские действия, производные от <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> и реализуя абстрактный метод <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>.

Дополнительные сведения см. в разделе <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.

## <a name="removal-of-claim-actions-and-claims"></a>Удаление действий утверждений и утверждений

[Клаимактионколлектион. Remove (String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) удаляет все действия утверждения для заданного <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> из коллекции. [Клаимактионколлектионмапекстенсионс. делетеклаим (клаимактионколлектион, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) удаляет утверждение заданного <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> по идентификатору. <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> в основном используется с [OpenID Connect Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) для удаления сформированных протоколом утверждений.

## <a name="sample-app-output"></a>Выходные данные примера приложения

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Приложение ASP.NET Core может устанавливать дополнительные утверждения и маркеры от внешних поставщиков проверки подлинности, таких как Facebook, Google, Microsoft и Twitter. Каждый поставщик раскрывает разные сведения о пользователях на своей платформе, но шаблон для получения и преобразования пользовательских данных в дополнительные утверждения одинаковы.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Предварительные требования

Решите, какие внешние поставщики проверки подлинности поддерживаются в приложении. Для каждого поставщика Зарегистрируйте приложение и получите идентификатор клиента и секрет клиента. Дополнительные сведения см. в разделе <xref:security/authentication/social/index>. В примере приложения используется [поставщик проверки подлинности Google](xref:security/authentication/google-logins).

## <a name="set-the-client-id-and-client-secret"></a>Задание идентификатора клиента и секрета клиента

Поставщик проверки подлинности OAuth устанавливает отношение доверия с приложением, используя идентификатор клиента и секрет клиента. Идентификатор клиента и значения секрета клиента создаются для приложения внешним поставщиком проверки подлинности при регистрации приложения в поставщике. Каждый внешний поставщик, используемый приложением, должен быть настроен независимо с ИДЕНТИФИКАТОРом клиента и секретом клиента поставщика. Дополнительные сведения см. в разделах, посвященных внешнему поставщику проверки подлинности, которые относятся к вашему сценарию.

* [Проверка подлинности Facebook](xref:security/authentication/facebook-logins)
* [Проверка подлинности Google](xref:security/authentication/google-logins)
* [Проверка подлинности Майкрософт](xref:security/authentication/microsoft-logins)
* [Проверка подлинности Twitter](xref:security/authentication/twitter-logins)
* [Другие поставщики проверки подлинности](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

Пример приложения настраивает поставщик проверки подлинности Google с ИДЕНТИФИКАТОРом клиента и секретом клиента, предоставленным Google:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>Определение области проверки подлинности

Укажите список разрешений для получения от поставщика, указав <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. В следующей таблице приведены области проверки подлинности для общих внешних поставщиков.

| Поставщик  | `Scope`                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Майкрософт | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

В примере приложения платформа Google `userinfo.profile` автоматически добавляется платформой при вызове <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> на <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>. Если приложению требуются дополнительные области, добавьте их в параметры. В следующем примере область Google `https://www.googleapis.com/auth/user.birthday.read` добавляется для получения дня рождения пользователя:

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>Сопоставьте ключи данных пользователя и создайте утверждения

В параметрах поставщика укажите <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> или <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> для каждого ключа или подраздела в данных пользователя JSON внешнего поставщика, чтобы удостоверение приложения было прочитано при входе. Дополнительные сведения о типах утверждений см. в разделе <xref:System.Security.Claims.ClaimTypes>.

Пример приложения создает утверждения (`urn:google:locale`) и Picture (`urn:google:picture`) из ключей `locale` и `picture` в данных пользователя Google:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

В <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*> <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) подписывается в приложение с <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>. В процессе входа <xref:Microsoft.AspNetCore.Identity.UserManager%601> может хранить `ApplicationUser` утверждений для пользовательских данных, доступных из <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

В примере приложения `OnPostConfirmationAsync` (*Account/екстерналлогин. cshtml. CS*) устанавливает утверждения языков (`urn:google:locale`) и picture (`urn:google:picture`) для входа в `ApplicationUser`, включая утверждение для <xref:System.Security.Claims.ClaimTypes.GivenName>:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

По умолчанию утверждения пользователя хранятся в файле cookie проверки подлинности. Если файл cookie для проверки подлинности слишком большой, это может привести к сбою приложения по следующим причинам:

* Браузер обнаруживает, что заголовок файла cookie слишком длинный.
* Общий размер запроса слишком велик.

Если для обработки запросов пользователей требуется большой объем пользовательских данных:

* Ограничьте количество и размер утверждений пользователей для обработки запроса только тем, что требуется приложению.
* Используйте пользовательский <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> для промежуточного слоя проверки подлинности "cookie" <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> для хранения удостоверений между запросами. Сохраняйте большие объемы информации об удостоверениях на сервере, при этом клиенту отправляется только небольшой ключ идентификатора сеанса.

## <a name="save-the-access-token"></a>Сохранение маркера доступа

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> определяет, должны ли маркеры доступа и обновления храниться в <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> после успешной авторизации. `SaveTokens` по умолчанию имеет значение `false`, чтобы уменьшить размер файла cookie окончательной проверки подлинности.

Пример приложения устанавливает значение `SaveTokens` в `true` в <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

При выполнении `OnPostConfirmationAsync` Сохраните маркер доступа ([екстерналлогининфо. аусентикатионтокенс](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) из внешнего поставщика в `ApplicationUser` `AuthenticationProperties`.

Пример приложения сохраняет маркер доступа в `OnPostConfirmationAsync` (Новая регистрация пользователя) и `OnGetCallbackAsync` (ранее зарегистрированный пользователь) в *Account/екстерналлогин. cshtml. CS*:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>Добавление дополнительных настраиваемых токенов

Чтобы продемонстрировать, как добавить пользовательский маркер, который хранится в составе `SaveTokens`, пример приложения добавляет <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> с текущим <xref:System.DateTime> для [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) из `TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a>Создание и добавление утверждений

Платформа предоставляет общие действия и методы расширения для создания и добавления заявок в коллекцию. Дополнительные сведения см. в разделах <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> и <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.

Пользователи могут определять пользовательские действия, производные от <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> и реализуя абстрактный метод <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>.

Дополнительные сведения см. в разделе <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.

## <a name="removal-of-claim-actions-and-claims"></a>Удаление действий утверждений и утверждений

[Клаимактионколлектион. Remove (String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) удаляет все действия утверждения для заданного <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> из коллекции. [Клаимактионколлектионмапекстенсионс. делетеклаим (клаимактионколлектион, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) удаляет утверждение заданного <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> по идентификатору. <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> в основном используется с [OpenID Connect Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) для удаления сформированных протоколом утверждений.

## <a name="sample-app-output"></a>Выходные данные примера приложения

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

::: moniker-end

## <a name="additional-resources"></a>Дополнительные ресурсы

* AspNetCore [соЦиалсампле @no__t приложение AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) -1. связанный пример приложения находится в ветви [ASPNET/в репозитории GitHub](https://github.com/aspnet/AspNetCore) `master`. Ветвь `master` содержит код в разделе активная разработка для следующего выпуска ASP.NET Core. Чтобы просмотреть версию примера приложения для выпущенной версии ASP.NET Core, используйте раскрывающийся список **ветвь** для выбора ветви выпуска (например `release/{X.Y}`).
