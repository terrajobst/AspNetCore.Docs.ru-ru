---
title: Аутентификация Facebook, Google и внешнего поставщика без удостоверения ASP.NET Core
author: rick-anderson
description: Объяснение использования проверки подлинности пользователей Facebook, Google, Twitter и т. д. без ASP.NET Core удостоверения.
ms.author: riande
ms.date: 11/19/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 680ea091dcc5ed7f94879b5d277e8be7e5abeb7b
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289116"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="3c1da-103">Использовать проверку подлинности поставщика социальных сетей без удостоверения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c1da-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3c1da-104"><xref:security/authentication/social/index> описано, как разрешить пользователям входить в систему с помощью OAuth 2,0 с учетными данными от внешних поставщиков проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3c1da-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="3c1da-105">Подход, описанный в этом разделе, включает ASP.NET Core удостоверение в качестве поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3c1da-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="3c1da-106">В этом примере демонстрируется использование внешнего поставщика проверки подлинности **без** ASP.NET Core удостоверения.</span><span class="sxs-lookup"><span data-stu-id="3c1da-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="3c1da-107">Это полезно для приложений, которые не занимают все функции ASP.NET Core удостоверения, но по-прежнему нуждаются в интеграции с доверенным внешним поставщиком проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3c1da-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="3c1da-108">В этом примере для проверки подлинности пользователей используется [Проверка подлинности Google](xref:security/authentication/google-logins) .</span><span class="sxs-lookup"><span data-stu-id="3c1da-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="3c1da-109">Использование аутентификации Google сдвигает многие сложности управления процессом входа в Google.</span><span class="sxs-lookup"><span data-stu-id="3c1da-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="3c1da-110">Чтобы выполнить интеграцию с другим внешним поставщиком проверки подлинности, см. следующие разделы:</span><span class="sxs-lookup"><span data-stu-id="3c1da-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="3c1da-111">Проверка подлинности Facebook</span><span class="sxs-lookup"><span data-stu-id="3c1da-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="3c1da-112">Проверка подлинности Майкрософт</span><span class="sxs-lookup"><span data-stu-id="3c1da-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="3c1da-113">Проверка подлинности Twitter</span><span class="sxs-lookup"><span data-stu-id="3c1da-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="3c1da-114">Другие поставщики</span><span class="sxs-lookup"><span data-stu-id="3c1da-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="3c1da-115">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="3c1da-115">Configuration</span></span>

<span data-ttu-id="3c1da-116">В методе `ConfigureServices` настройте схемы проверки подлинности приложения с помощью методов <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>и <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>.</span><span class="sxs-lookup"><span data-stu-id="3c1da-116">In the `ConfigureServices` method, configure the app's authentication schemes with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, and <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> methods:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet1)]

<span data-ttu-id="3c1da-117">Вызов <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> задает <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>приложения.</span><span class="sxs-lookup"><span data-stu-id="3c1da-117">The call to <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> sets the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>.</span></span> <span data-ttu-id="3c1da-118">`DefaultScheme` является схемой по умолчанию, используемой следующими методами расширения проверки подлинности `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="3c1da-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="3c1da-119">Настройка `DefaultScheme` приложения в [кукиеаусентикатиондефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies") настраивает приложение на использование файлов cookie в качестве схемы по умолчанию для этих методов расширения.</span><span class="sxs-lookup"><span data-stu-id="3c1da-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="3c1da-120">Настройка <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> приложения в [гугледефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") настраивает приложение на использование Google в качестве схемы по умолчанию для вызовов `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="3c1da-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="3c1da-121">`DefaultChallengeScheme` переопределяет `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="3c1da-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="3c1da-122">Дополнительные свойства, переопределяющие `DefaultScheme` при установке, см. в разделе <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.</span><span class="sxs-lookup"><span data-stu-id="3c1da-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="3c1da-123">В `Startup.Configure`вызывайте `UseAuthentication` и `UseAuthorization` между вызовами `UseRouting` и `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="3c1da-123">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` between calling `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="3c1da-124">При этом задается свойство `HttpContext.User` и выполняется по промежуточного слоя авторизации для запросов:</span><span class="sxs-lookup"><span data-stu-id="3c1da-124">This sets the `HttpContext.User` property and runs the Authorization Middleware for requests:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet2&highlight=3-4)]

<span data-ttu-id="3c1da-125">Дополнительные сведения о схемах проверки подлинности и проверке подлинности файлов cookie см. в разделе <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="3c1da-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="3c1da-126">Применить авторизацию</span><span class="sxs-lookup"><span data-stu-id="3c1da-126">Apply authorization</span></span>

<span data-ttu-id="3c1da-127">Проверьте конфигурацию проверки подлинности приложения, применив атрибут `AuthorizeAttribute` к контроллеру, действию или странице.</span><span class="sxs-lookup"><span data-stu-id="3c1da-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="3c1da-128">Следующий код ограничивает доступ к странице *конфиденциальности* для пользователей, которые прошли проверку подлинности:</span><span class="sxs-lookup"><span data-stu-id="3c1da-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="3c1da-129">Выйти</span><span class="sxs-lookup"><span data-stu-id="3c1da-129">Sign out</span></span>

<span data-ttu-id="3c1da-130">Чтобы выйти из текущего пользователя и удалить его файл cookie, вызовите [сигнаутасинк](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="3c1da-130">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="3c1da-131">Следующий код добавляет обработчик `Logout` страницы на страницу *индекса* :</span><span class="sxs-lookup"><span data-stu-id="3c1da-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

<span data-ttu-id="3c1da-132">Обратите внимание, что в вызове `SignOutAsync` не указана схема проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3c1da-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="3c1da-133">`DefaultScheme` приложения `CookieAuthenticationDefaults.AuthenticationScheme` используется для возврата.</span><span class="sxs-lookup"><span data-stu-id="3c1da-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c1da-134">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3c1da-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3c1da-135"><xref:security/authentication/social/index> описано, как разрешить пользователям входить в систему с помощью OAuth 2,0 с учетными данными от внешних поставщиков проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3c1da-135"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="3c1da-136">Подход, описанный в этом разделе, включает ASP.NET Core удостоверение в качестве поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3c1da-136">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="3c1da-137">В этом примере демонстрируется использование внешнего поставщика проверки подлинности **без** ASP.NET Core удостоверения.</span><span class="sxs-lookup"><span data-stu-id="3c1da-137">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="3c1da-138">Это полезно для приложений, которые не занимают все функции ASP.NET Core удостоверения, но по-прежнему нуждаются в интеграции с доверенным внешним поставщиком проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3c1da-138">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="3c1da-139">В этом примере для проверки подлинности пользователей используется [Проверка подлинности Google](xref:security/authentication/google-logins) .</span><span class="sxs-lookup"><span data-stu-id="3c1da-139">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="3c1da-140">Использование аутентификации Google сдвигает многие сложности управления процессом входа в Google.</span><span class="sxs-lookup"><span data-stu-id="3c1da-140">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="3c1da-141">Чтобы выполнить интеграцию с другим внешним поставщиком проверки подлинности, см. следующие разделы:</span><span class="sxs-lookup"><span data-stu-id="3c1da-141">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="3c1da-142">Проверка подлинности Facebook</span><span class="sxs-lookup"><span data-stu-id="3c1da-142">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="3c1da-143">Проверка подлинности Майкрософт</span><span class="sxs-lookup"><span data-stu-id="3c1da-143">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="3c1da-144">Проверка подлинности Twitter</span><span class="sxs-lookup"><span data-stu-id="3c1da-144">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="3c1da-145">Другие поставщики</span><span class="sxs-lookup"><span data-stu-id="3c1da-145">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="3c1da-146">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="3c1da-146">Configuration</span></span>

<span data-ttu-id="3c1da-147">В методе `ConfigureServices` настройте схемы проверки подлинности приложения с помощью методов `AddAuthentication`, `AddCookie`и `AddGoogle`.</span><span class="sxs-lookup"><span data-stu-id="3c1da-147">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie`, and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet1)]

<span data-ttu-id="3c1da-148">Вызов [аддаусентикатион](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) задает [дефаултсчеме](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)приложения.</span><span class="sxs-lookup"><span data-stu-id="3c1da-148">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="3c1da-149">`DefaultScheme` является схемой по умолчанию, используемой следующими методами расширения проверки подлинности `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="3c1da-149">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="3c1da-150">Настройка `DefaultScheme` приложения в [кукиеаусентикатиондефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies") настраивает приложение на использование файлов cookie в качестве схемы по умолчанию для этих методов расширения.</span><span class="sxs-lookup"><span data-stu-id="3c1da-150">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="3c1da-151">Настройка <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> приложения в [гугледефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") настраивает приложение на использование Google в качестве схемы по умолчанию для вызовов `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="3c1da-151">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="3c1da-152">`DefaultChallengeScheme` переопределяет `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="3c1da-152">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="3c1da-153">Дополнительные свойства, переопределяющие `DefaultScheme` при установке, см. в разделе <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.</span><span class="sxs-lookup"><span data-stu-id="3c1da-153">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="3c1da-154">В методе `Configure` вызовите метод `UseAuthentication` для вызова по промежуточного слоя проверки подлинности, устанавливающего свойство `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="3c1da-154">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="3c1da-155">Вызовите метод `UseAuthentication` перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="3c1da-155">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet2)]

<span data-ttu-id="3c1da-156">Дополнительные сведения о схемах проверки подлинности и проверке подлинности файлов cookie см. в разделе <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="3c1da-156">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="3c1da-157">Применить авторизацию</span><span class="sxs-lookup"><span data-stu-id="3c1da-157">Apply authorization</span></span>

<span data-ttu-id="3c1da-158">Проверьте конфигурацию проверки подлинности приложения, применив атрибут `AuthorizeAttribute` к контроллеру, действию или странице.</span><span class="sxs-lookup"><span data-stu-id="3c1da-158">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="3c1da-159">Следующий код ограничивает доступ к странице *конфиденциальности* для пользователей, которые прошли проверку подлинности:</span><span class="sxs-lookup"><span data-stu-id="3c1da-159">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="3c1da-160">Выйти</span><span class="sxs-lookup"><span data-stu-id="3c1da-160">Sign out</span></span>

<span data-ttu-id="3c1da-161">Чтобы выйти из текущего пользователя и удалить его файл cookie, вызовите [сигнаутасинк](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="3c1da-161">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="3c1da-162">Следующий код добавляет обработчик `Logout` страницы на страницу *индекса* :</span><span class="sxs-lookup"><span data-stu-id="3c1da-162">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

<span data-ttu-id="3c1da-163">Обратите внимание, что в вызове `SignOutAsync` не указана схема проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3c1da-163">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="3c1da-164">`DefaultScheme` приложения `CookieAuthenticationDefaults.AuthenticationScheme` используется для возврата.</span><span class="sxs-lookup"><span data-stu-id="3c1da-164">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c1da-165">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3c1da-165">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
