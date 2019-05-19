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
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="3af71-103">Сохранять дополнительные утверждения и маркеры от внешних поставщиков в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3af71-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="3af71-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="3af71-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3af71-105">Приложения ASP.NET Core можно установить дополнительные утверждения и маркеры от внешних поставщиков проверки подлинности, таких как Facebook, Google, Майкрософт и Twitter.</span><span class="sxs-lookup"><span data-stu-id="3af71-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="3af71-106">Каждый поставщик раскрывает различные сведения о пользователях на платформе, но шаблон для получения и преобразования данных пользователя в дополнительные утверждения одинаков.</span><span class="sxs-lookup"><span data-stu-id="3af71-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="3af71-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3af71-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3af71-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3af71-108">Prerequisites</span></span>

<span data-ttu-id="3af71-109">Решите, какие поставщики внешней проверки подлинности для поддержки в приложении.</span><span class="sxs-lookup"><span data-stu-id="3af71-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="3af71-110">Для каждого поставщика Регистрация приложения и получить идентификатор клиента и секрет клиента.</span><span class="sxs-lookup"><span data-stu-id="3af71-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="3af71-111">Дополнительные сведения см. в разделе <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="3af71-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="3af71-112">Пример приложения использует [поставщик проверки подлинности Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="3af71-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="3af71-113">Задание идентификатора и секрета клиента</span><span class="sxs-lookup"><span data-stu-id="3af71-113">Set the client ID and client secret</span></span>

<span data-ttu-id="3af71-114">Поставщик проверки подлинности OAuth устанавливает отношение доверия с помощью приложения с помощью идентификатора клиента и секрет клиента.</span><span class="sxs-lookup"><span data-stu-id="3af71-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="3af71-115">Идентификатор клиента и секрет клиента, создаваемые для приложения поставщика внешней проверки подлинности при регистрации приложения с поставщиком.</span><span class="sxs-lookup"><span data-stu-id="3af71-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="3af71-116">Каждый внешний поставщик, приложение использует должны быть настроены независимо друг от друга идентификатор клиента и секрет клиента поставщика.</span><span class="sxs-lookup"><span data-stu-id="3af71-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="3af71-117">Дополнительные сведения см. в разделах поставщика внешней проверки подлинности, которые применяются к вашему сценарию:</span><span class="sxs-lookup"><span data-stu-id="3af71-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="3af71-118">Проверка подлинности Facebook</span><span class="sxs-lookup"><span data-stu-id="3af71-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="3af71-119">Проверка подлинности Google</span><span class="sxs-lookup"><span data-stu-id="3af71-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="3af71-120">Проверка подлинности Майкрософт</span><span class="sxs-lookup"><span data-stu-id="3af71-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="3af71-121">Проверка подлинности Twitter</span><span class="sxs-lookup"><span data-stu-id="3af71-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="3af71-122">Другие поставщики проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="3af71-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="3af71-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="3af71-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="3af71-124">Пример приложения настраивает поставщик проверки подлинности Google с Идентификатором клиента и секрет клиента, предоставляемый Google:</span><span class="sxs-lookup"><span data-stu-id="3af71-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="3af71-125">Установить область проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="3af71-125">Establish the authentication scope</span></span>

<span data-ttu-id="3af71-126">Укажите список разрешений для получения от поставщика, путем указания <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="3af71-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="3af71-127">В следующей таблице отображаются области проверки подлинности для распространенных внешних поставщиков.</span><span class="sxs-lookup"><span data-stu-id="3af71-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="3af71-128">Поставщик</span><span class="sxs-lookup"><span data-stu-id="3af71-128">Provider</span></span>  | <span data-ttu-id="3af71-129">Область</span><span class="sxs-lookup"><span data-stu-id="3af71-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="3af71-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="3af71-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="3af71-131">Google</span><span class="sxs-lookup"><span data-stu-id="3af71-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="3af71-132">Майкрософт</span><span class="sxs-lookup"><span data-stu-id="3af71-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="3af71-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="3af71-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="3af71-134">В примере приложения, Google `userinfo.profile` область автоматически добавляется платформой при <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> вызывается <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="3af71-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="3af71-135">Если приложению требуется дополнительные области, их необходимо добавьте в параметры.</span><span class="sxs-lookup"><span data-stu-id="3af71-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="3af71-136">В следующем примере, Google `https://www.googleapis.com/auth/user.birthday.read` область добавляется для получения день рождения пользователя:</span><span class="sxs-lookup"><span data-stu-id="3af71-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="3af71-137">Сопоставить ключи данных пользователя и создать утверждения</span><span class="sxs-lookup"><span data-stu-id="3af71-137">Map user data keys and create claims</span></span>

<span data-ttu-id="3af71-138">В параметрах поставщика, укажите <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> или <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> каждый ключ/подраздел в внешнего поставщика JSON пользовательские данные для удостоверения приложения для чтения при входе.</span><span class="sxs-lookup"><span data-stu-id="3af71-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="3af71-139">Дополнительные сведения о типах утверждений, см. в разделе <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="3af71-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="3af71-140">Пример приложения создает языкового стандарта (`urn:google:locale`) и рисунка (`urn:google:picture`) утверждений от `locale` и `picture` ключи в пользовательских данных Google:</span><span class="sxs-lookup"><span data-stu-id="3af71-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="3af71-141">В <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) осуществил вход в приложение с помощью <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="3af71-141">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="3af71-142">Во время входа в систему <xref:Microsoft.AspNetCore.Identity.UserManager%601> можно хранить `ApplicationUser` утверждений для пользовательских данных, доступных из <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="3af71-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="3af71-143">В примере приложения `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) устанавливает языковой стандарт (`urn:google:locale`) и рисунка (`urn:google:picture`) утверждения для со знаком в `ApplicationUser`, включая утверждение для <xref:System.Security.Claims.ClaimTypes.GivenName> :</span><span class="sxs-lookup"><span data-stu-id="3af71-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="3af71-144">По умолчанию утверждения пользователя хранятся в файле cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3af71-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="3af71-145">Если файл cookie проверки подлинности слишком велико, это может привести приложение сбоем, так как:</span><span class="sxs-lookup"><span data-stu-id="3af71-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="3af71-146">Браузер обнаруживает, что заголовок cookie слишком много времени.</span><span class="sxs-lookup"><span data-stu-id="3af71-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="3af71-147">Общий размер запроса слишком велик.</span><span class="sxs-lookup"><span data-stu-id="3af71-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="3af71-148">Если большой объем сведений о пользователях является обязательным для обработки запросов пользователя:</span><span class="sxs-lookup"><span data-stu-id="3af71-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="3af71-149">Ограничьте количество и размер утверждения пользователей для только что приложение требует обработки запроса.</span><span class="sxs-lookup"><span data-stu-id="3af71-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="3af71-150">Использовать пользовательский <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> для по промежуточного слоя проверки подлинности <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> для хранения идентификаторов всех запросов.</span><span class="sxs-lookup"><span data-stu-id="3af71-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="3af71-151">Сохранить большие объемы информации идентификации на сервере при отправке только небольшой сеансовый ключ идентификатора клиента.</span><span class="sxs-lookup"><span data-stu-id="3af71-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="3af71-152">Сохранить маркер доступа</span><span class="sxs-lookup"><span data-stu-id="3af71-152">Save the access token</span></span>

<span data-ttu-id="3af71-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> Определяет, следует ли хранить маркеров доступа и обновления в <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> после успешной авторизации.</span><span class="sxs-lookup"><span data-stu-id="3af71-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="3af71-154">`SaveTokens` имеет значение `false` по умолчанию, чтобы уменьшить размер файла cookie окончательной проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3af71-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="3af71-155">В примере приложения задает значение `SaveTokens` для `true` в <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="3af71-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="3af71-156">Когда `OnPostConfirmationAsync` выполняет, сохранить маркер доступа ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) из внешнего поставщика в `ApplicationUser`в `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="3af71-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="3af71-157">Это пример приложения сохраняет маркер доступа в `OnPostConfirmationAsync` (Регистрация нового пользователя) и `OnGetCallbackAsync` (ранее зарегистрированного пользователя) в *Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="3af71-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="3af71-158">Как добавить дополнительные пользовательские маркеры</span><span class="sxs-lookup"><span data-stu-id="3af71-158">How to add additional custom tokens</span></span>

<span data-ttu-id="3af71-159">Чтобы продемонстрировать, как добавить пользовательский маркер, который хранится как часть `SaveTokens`, пример приложения добавляет <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> с текущим <xref:System.DateTime> для [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) из `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="3af71-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="3af71-160">Создание и добавление утверждений</span><span class="sxs-lookup"><span data-stu-id="3af71-160">Creating and adding claims</span></span>

<span data-ttu-id="3af71-161">Платформа предоставляет общие действия и методы расширения для создания и добавления утверждений в коллекцию.</span><span class="sxs-lookup"><span data-stu-id="3af71-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="3af71-162">Дополнительные сведения см. в разделах <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> и <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="3af71-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="3af71-163">Пользователи могут определять настраиваемые действия путем наследования от <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> и реализация абстрактного <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> метод.</span><span class="sxs-lookup"><span data-stu-id="3af71-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="3af71-164">Дополнительные сведения см. в разделе <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="3af71-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="3af71-165">Удаление Утверждение действий и утверждений</span><span class="sxs-lookup"><span data-stu-id="3af71-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="3af71-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) удаляет все утверждения действия для заданного <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> из коллекции.</span><span class="sxs-lookup"><span data-stu-id="3af71-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="3af71-167">[ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) удаляет утверждение заданного <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> из удостоверения.</span><span class="sxs-lookup"><span data-stu-id="3af71-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="3af71-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> Чаще всего используется со [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) удалить сформированные протокола утверждений.</span><span class="sxs-lookup"><span data-stu-id="3af71-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="3af71-169">Образец выходных данных приложения</span><span class="sxs-lookup"><span data-stu-id="3af71-169">Sample app output</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="3af71-170">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3af71-170">Additional resources</span></span>

* <span data-ttu-id="3af71-171">[engineering SocialSample приложение ASPNET/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; связанного примера приложения находится на [репозиторий GitHub aspnet/AspNetCore](https://github.com/aspnet/AspNetCore) `master` engineering ветви.</span><span class="sxs-lookup"><span data-stu-id="3af71-171">[aspnet/AspNetCore engineering SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [aspnet/AspNetCore GitHub repo's](https://github.com/aspnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="3af71-172">`master` Ветвь содержит код в активной разработке для следующего выпуска ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3af71-172">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="3af71-173">Чтобы просмотреть версию примера приложения для новой версии ASP.NET Core, используйте **ветви** раскрывающийся список для выбора ветви выпуска (например `release/2.2`).</span><span class="sxs-lookup"><span data-stu-id="3af71-173">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/2.2`).</span></span>
