---
title: Сохранять дополнительные утверждения и маркеры от внешних поставщиков в ASP.NET Core
author: guardrex
description: Узнайте, как установить дополнительные утверждения и маркеры от внешних поставщиков.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: e18287e5a4171b3f7a6daa122111448b8447c1da
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874842"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>Сохранять дополнительные утверждения и маркеры от внешних поставщиков в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Приложения ASP.NET Core можно установить дополнительные утверждения и маркеры от внешних поставщиков проверки подлинности, таких как Facebook, Google, Майкрософт и Twitter. Каждый поставщик раскрывает различные сведения о пользователях на платформе, но шаблон для получения и преобразования данных пользователя в дополнительные утверждения одинаков.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Предварительные требования

Решите, какие поставщики внешней проверки подлинности для поддержки в приложении. Для каждого поставщика Регистрация приложения и получить идентификатор клиента и секрет клиента. Дополнительные сведения см. в разделе <xref:security/authentication/social/index>. Пример приложения использует [поставщик проверки подлинности Google](xref:security/authentication/google-logins).

## <a name="set-the-client-id-and-client-secret"></a>Задание идентификатора и секрета клиента

Поставщик проверки подлинности OAuth устанавливает отношение доверия с помощью приложения с помощью идентификатора клиента и секрет клиента. Идентификатор клиента и секрет клиента, создаваемые для приложения поставщика внешней проверки подлинности при регистрации приложения с поставщиком. Каждый внешний поставщик, приложение использует должны быть настроены независимо друг от друга идентификатор клиента и секрет клиента поставщика. Дополнительные сведения см. в разделах поставщика внешней проверки подлинности, которые применяются к вашему сценарию:

* [Проверка подлинности Facebook](xref:security/authentication/facebook-logins)
* [Проверка подлинности Google](xref:security/authentication/google-logins)
* [Проверка подлинности Майкрософт](xref:security/authentication/microsoft-logins)
* [Проверка подлинности Twitter](xref:security/authentication/twitter-logins)
* [Другие поставщики проверки подлинности](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

Пример приложения настраивает поставщик проверки подлинности Google с Идентификатором клиента и секрет клиента, предоставляемый Google:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>Установить область проверки подлинности

Укажите список разрешений для получения от поставщика, путем указания <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. В следующей таблице отображаются области проверки подлинности для распространенных внешних поставщиков.

| Поставщик  | Область                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Майкрософт | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

В примере приложения, Google `userinfo.profile` область автоматически добавляется платформой при <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> вызывается <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>. Если приложению требуется дополнительные области, их необходимо добавьте в параметры. В следующем примере, Google `https://www.googleapis.com/auth/user.birthday.read` область добавляется для получения день рождения пользователя:

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>Сопоставить ключи данных пользователя и создать утверждения

В параметрах поставщика, укажите <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> или <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> каждый ключ/подраздел в внешнего поставщика JSON пользовательские данные для удостоверения приложения для чтения при входе. Дополнительные сведения о типах утверждений, см. в разделе <xref:System.Security.Claims.ClaimTypes>.

Пример приложения создает языкового стандарта (`urn:google:locale`) и рисунка (`urn:google:picture`) утверждений от `locale` и `picture` ключи в пользовательских данных Google:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

В <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) осуществил вход в приложение с помощью <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>. Во время входа в систему <xref:Microsoft.AspNetCore.Identity.UserManager%601> можно хранить `ApplicationUser` утверждений для пользовательских данных, доступных из <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

В примере приложения `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) устанавливает языковой стандарт (`urn:google:locale`) и рисунка (`urn:google:picture`) утверждения для со знаком в `ApplicationUser`, включая утверждение для <xref:System.Security.Claims.ClaimTypes.GivenName> :

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

По умолчанию утверждения пользователя хранятся в файле cookie проверки подлинности. Если файл cookie проверки подлинности слишком велико, это может привести приложение сбоем, так как:

* Браузер обнаруживает, что заголовок cookie слишком много времени.
* Общий размер запроса слишком велик.

Если большой объем сведений о пользователях является обязательным для обработки запросов пользователя:

* Ограничьте количество и размер утверждения пользователей для только что приложение требует обработки запроса.
* Использовать пользовательский <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> для по промежуточного слоя проверки подлинности <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> для хранения идентификаторов всех запросов. Сохранить большие объемы информации идентификации на сервере при отправке только небольшой сеансовый ключ идентификатора клиента.

## <a name="save-the-access-token"></a>Сохранить маркер доступа

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> Определяет, следует ли хранить маркеров доступа и обновления в <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> после успешной авторизации. `SaveTokens` имеет значение `false` по умолчанию, чтобы уменьшить размер файла cookie окончательной проверки подлинности.

В примере приложения задает значение `SaveTokens` для `true` в <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

Когда `OnPostConfirmationAsync` выполняет, сохранить маркер доступа ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) из внешнего поставщика в `ApplicationUser`в `AuthenticationProperties`.

Это пример приложения сохраняет маркер доступа в `OnPostConfirmationAsync` (Регистрация нового пользователя) и `OnGetCallbackAsync` (ранее зарегистрированного пользователя) в *Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>Как добавить дополнительные пользовательские маркеры

Чтобы продемонстрировать, как добавить пользовательский маркер, который хранится как часть `SaveTokens`, пример приложения добавляет <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> с текущим <xref:System.DateTime> для [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) из `TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a>Создание и добавление утверждений

Платформа предоставляет общие действия и методы расширения для создания и добавления утверждений в коллекцию. Дополнительные сведения см. в разделах <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> и <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.

Пользователи могут определять настраиваемые действия путем наследования от <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> и реализация абстрактного <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> метод.

Дополнительные сведения см. в разделе <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.

## <a name="removal-of-claim-actions-and-claims"></a>Удаление Утверждение действий и утверждений

[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) удаляет все утверждения действия для заданного <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> из коллекции. [ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) удаляет утверждение заданного <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> из удостоверения. <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> Чаще всего используется со [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) удалить сформированные протокола утверждений.

## <a name="sample-app-output"></a>Образец выходных данных приложения

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

## <a name="additional-resources"></a>Дополнительные ресурсы

* [engineering SocialSample приложение ASPNET/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; связанного примера приложения находится на [репозиторий GitHub aspnet/AspNetCore](https://github.com/aspnet/AspNetCore) `master` engineering ветви. `master` Ветвь содержит код в активной разработке для следующего выпуска ASP.NET Core. Чтобы просмотреть версию примера приложения для новой версии ASP.NET Core, используйте **ветви** раскрывающийся список для выбора ветви выпуска (например `release/2.2`).
