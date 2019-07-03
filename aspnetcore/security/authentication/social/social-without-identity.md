---
title: Facebook, Google и внешних поставщиков проверки подлинности без ASP.NET Core Identity
author: rick-anderson
description: Объяснение с помощью Facebook, Google, Twitter, и т.д. учетная запись проверки подлинности пользователя без ASP.NET Core Identity.
ms.author: riande
ms.date: 07/04/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: e67da513fef1ce453110c465b08e9c7965e71df5
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67557653"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="b231f-103">Использовать проверку подлинности поставщиков социальных сетей вход без ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="b231f-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="b231f-104"><xref:security/authentication/social/index> Описывает, как разрешить пользователям входить с помощью OAuth 2.0 с учетными данными внешних поставщиков проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b231f-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="b231f-105">Подход, описанный в этом разделе содержит удостоверение ASP.NET Core в качестве поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b231f-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="b231f-106">В этом примере показано, как использовать внешний поставщик аутентификации **без** удостоверения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b231f-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="b231f-107">Это полезно для приложений, которые не требуются все возможности удостоверения ASP.NET Core, но по-прежнему требуется интеграция с доверенного внешнего поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b231f-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="b231f-108">В этом примере используется [проверки подлинности Google](xref:security/authentication/google-logins) для проверки подлинности пользователей.</span><span class="sxs-lookup"><span data-stu-id="b231f-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="b231f-109">С помощью Google проверки подлинности сдвигает многие из сложностей в управлении процесс входа в Google.</span><span class="sxs-lookup"><span data-stu-id="b231f-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="b231f-110">Чтобы интегрировать с разных внешнего поставщика проверки подлинности, см. в разделах:</span><span class="sxs-lookup"><span data-stu-id="b231f-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="b231f-111">Проверка подлинности Facebook</span><span class="sxs-lookup"><span data-stu-id="b231f-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="b231f-112">Проверка подлинности Майкрософт</span><span class="sxs-lookup"><span data-stu-id="b231f-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="b231f-113">Проверка подлинности Twitter</span><span class="sxs-lookup"><span data-stu-id="b231f-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="b231f-114">Другие поставщики</span><span class="sxs-lookup"><span data-stu-id="b231f-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="b231f-115">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="b231f-115">Configuration</span></span>

<span data-ttu-id="b231f-116">В `ConfigureServices` метод, настройте схемы проверки подлинности приложения с `AddAuthentication`, `AddCookie` и `AddGoogle` методов:</span><span class="sxs-lookup"><span data-stu-id="b231f-116">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie` and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="b231f-117">Вызов [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) задает приложения [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span><span class="sxs-lookup"><span data-stu-id="b231f-117">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="b231f-118">`DefaultScheme` Представляет собой схему по умолчанию, используемые следующие `HttpContext` методы расширения для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="b231f-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="b231f-119">Установка приложения `DefaultScheme` для [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) («файлы cookie») настраивает приложение для использования файлов cookie в качестве схемы по умолчанию для этих методов расширения.</span><span class="sxs-lookup"><span data-stu-id="b231f-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="b231f-120">Установка приложения <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> для [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) («Google») настраивает приложение для использования Google в качестве схемы по умолчанию для вызовов `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="b231f-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="b231f-121">`DefaultChallengeScheme` переопределяет `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="b231f-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="b231f-122">См. в разделе <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> дополнительных свойств, которые переопределяют `DefaultScheme` при установке.</span><span class="sxs-lookup"><span data-stu-id="b231f-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="b231f-123">В `Configure` мы вызываем метод `UseAuthentication` метод, вызываемый по промежуточного слоя проверки подлинности, который задает `HttpContext.User` свойства.</span><span class="sxs-lookup"><span data-stu-id="b231f-123">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="b231f-124">Вызовите `UseAuthentication` метод перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="b231f-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="b231f-125">Дополнительные сведения о схемы проверки подлинности и проверки подлинности файла cookie, см. в разделе <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="b231f-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="applying-basic-authorization"></a><span data-ttu-id="b231f-126">Применение базовой авторизации</span><span class="sxs-lookup"><span data-stu-id="b231f-126">Applying basic authorization</span></span>

<span data-ttu-id="b231f-127">Проверить конфигурацию проверки подлинности приложения, применяя `AuthorizeAttribute` атрибут контроллера, действия или страницы.</span><span class="sxs-lookup"><span data-stu-id="b231f-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="b231f-128">Следующий код ограничивает доступ к *конфиденциальности* страницы для пользователей, которые прошли проверку подлинности:</span><span class="sxs-lookup"><span data-stu-id="b231f-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="b231f-129">Выйти</span><span class="sxs-lookup"><span data-stu-id="b231f-129">Sign out</span></span>

<span data-ttu-id="b231f-130">Чтобы выйти из системы текущего пользователя и удалить их файл cookie, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="b231f-130">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0).</span></span> <span data-ttu-id="b231f-131">Следующий код добавляет `Logout` обработчик страниц для *индекс* страницы:</span><span class="sxs-lookup"><span data-stu-id="b231f-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

<span data-ttu-id="b231f-132">Обратите внимание, что вызов `SignOutAsync` не указывает схему проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b231f-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="b231f-133">Приложения `DefaultScheme` из `CookieAuthenticationDefaults.AuthenticationScheme` используется на случай возможного отката обратно.</span><span class="sxs-lookup"><span data-stu-id="b231f-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b231f-134">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b231f-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
