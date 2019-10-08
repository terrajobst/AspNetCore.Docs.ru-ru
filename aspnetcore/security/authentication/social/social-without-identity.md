---
title: Аутентификация Facebook, Google и внешнего поставщика без удостоверения ASP.NET Core
author: rick-anderson
description: Объяснение использования проверки подлинности пользователей Facebook, Google, Twitter и т. д. без ASP.NET Core удостоверения.
ms.author: riande
ms.date: 09/25/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 54dd93a13b2f7ed09c2c305f529d5f4610567184
ms.sourcegitcommit: 6d26ab647ede4f8e57465e29b03be5cb130fc872
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2019
ms.locfileid: "71999895"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="d30b5-103">Использовать проверку подлинности поставщика социальных сетей без удостоверения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d30b5-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d30b5-104"><xref:security/authentication/social/index> описывает, как разрешить пользователям входить в систему с помощью OAuth 2,0 с учетными данными от внешних поставщиков проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d30b5-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="d30b5-105">Подход, описанный в этом разделе, включает ASP.NET Core удостоверение в качестве поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d30b5-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="d30b5-106">В этом примере демонстрируется использование внешнего поставщика проверки подлинности **без** ASP.NET Core удостоверения.</span><span class="sxs-lookup"><span data-stu-id="d30b5-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="d30b5-107">Это полезно для приложений, которые не занимают все функции ASP.NET Core удостоверения, но по-прежнему нуждаются в интеграции с доверенным внешним поставщиком проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d30b5-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="d30b5-108">В этом примере для проверки подлинности пользователей используется [Проверка подлинности Google](xref:security/authentication/google-logins) .</span><span class="sxs-lookup"><span data-stu-id="d30b5-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="d30b5-109">Использование аутентификации Google сдвигает многие сложности управления процессом входа в Google.</span><span class="sxs-lookup"><span data-stu-id="d30b5-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="d30b5-110">Чтобы выполнить интеграцию с другим внешним поставщиком проверки подлинности, см. следующие разделы:</span><span class="sxs-lookup"><span data-stu-id="d30b5-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="d30b5-111">Проверка подлинности Facebook</span><span class="sxs-lookup"><span data-stu-id="d30b5-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="d30b5-112">Проверка подлинности Майкрософт</span><span class="sxs-lookup"><span data-stu-id="d30b5-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="d30b5-113">Проверка подлинности Twitter</span><span class="sxs-lookup"><span data-stu-id="d30b5-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="d30b5-114">Другие поставщики</span><span class="sxs-lookup"><span data-stu-id="d30b5-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="d30b5-115">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="d30b5-115">Configuration</span></span>

<span data-ttu-id="d30b5-116">В методе `ConfigureServices` настройте схемы проверки подлинности приложения с помощью методов <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> и <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>:</span><span class="sxs-lookup"><span data-stu-id="d30b5-116">In the `ConfigureServices` method, configure the app's authentication schemes with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, and <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> methods:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet1)]

<span data-ttu-id="d30b5-117">Вызов <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> задает <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme> приложения.</span><span class="sxs-lookup"><span data-stu-id="d30b5-117">The call to <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> sets the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>.</span></span> <span data-ttu-id="d30b5-118">@No__t-0 — это схема по умолчанию, используемая следующими методами расширения проверки подлинности `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="d30b5-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="d30b5-119">Если задать для приложения `DefaultScheme` значение [кукиеаусентикатиондефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), приложение будет использовать файлы cookie в качестве схемы по умолчанию для этих методов расширения.</span><span class="sxs-lookup"><span data-stu-id="d30b5-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="d30b5-120">Если задать для приложения значение @no__t от 0 до [гугледефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), приложение будет использовать Google в качестве схемы по умолчанию для вызовов `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="d30b5-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="d30b5-121">`DefaultChallengeScheme` переопределяет `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="d30b5-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="d30b5-122">Дополнительные свойства, которые переопределяют `DefaultScheme` при установке, см. в разделе <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.</span><span class="sxs-lookup"><span data-stu-id="d30b5-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="d30b5-123">В `Startup.Configure` вызовите `UseAuthentication` и `UseAuthorization`, чтобы задать свойство `HttpContext.User` и запустить по промежуточного слоя авторизации для запросов.</span><span class="sxs-lookup"><span data-stu-id="d30b5-123">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` to set the `HttpContext.User` property and run Authorization Middleware for requests.</span></span> <span data-ttu-id="d30b5-124">Вызовите методы `UseAuthentication` и `UseAuthorization` перед вызовом `UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="d30b5-124">Call the `UseAuthentication` and `UseAuthorization` methods before calling `UseEndpoints`:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet2)]

<span data-ttu-id="d30b5-125">Дополнительные сведения о схемах проверки подлинности и проверке подлинности файлов cookie см. в разделе <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="d30b5-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="d30b5-126">Применить авторизацию</span><span class="sxs-lookup"><span data-stu-id="d30b5-126">Apply authorization</span></span>

<span data-ttu-id="d30b5-127">Проверьте конфигурацию проверки подлинности приложения, применив атрибут `AuthorizeAttribute` к контроллеру, действию или странице.</span><span class="sxs-lookup"><span data-stu-id="d30b5-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="d30b5-128">Следующий код ограничивает доступ к странице *конфиденциальности* для пользователей, которые прошли проверку подлинности:</span><span class="sxs-lookup"><span data-stu-id="d30b5-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="d30b5-129">Выйти</span><span class="sxs-lookup"><span data-stu-id="d30b5-129">Sign out</span></span>

<span data-ttu-id="d30b5-130">Чтобы выйти из текущего пользователя и удалить его файл cookie, вызовите [сигнаутасинк](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="d30b5-130">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="d30b5-131">Следующий код добавляет обработчик страницы `Logout` на страницу *индекса* :</span><span class="sxs-lookup"><span data-stu-id="d30b5-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Pages/Index.cshtml.cs?name=snippet&highlight=14-18)]

<span data-ttu-id="d30b5-132">Обратите внимание, что при вызове `SignOutAsync` не указана схема проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d30b5-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="d30b5-133">Значение @no__t (0) приложения `CookieAuthenticationDefaults.AuthenticationScheme` используется для возврата.</span><span class="sxs-lookup"><span data-stu-id="d30b5-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d30b5-134">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d30b5-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d30b5-135"><xref:security/authentication/social/index> описывает, как разрешить пользователям входить в систему с помощью OAuth 2,0 с учетными данными от внешних поставщиков проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d30b5-135"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="d30b5-136">Подход, описанный в этом разделе, включает ASP.NET Core удостоверение в качестве поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d30b5-136">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="d30b5-137">В этом примере демонстрируется использование внешнего поставщика проверки подлинности **без** ASP.NET Core удостоверения.</span><span class="sxs-lookup"><span data-stu-id="d30b5-137">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="d30b5-138">Это полезно для приложений, которые не занимают все функции ASP.NET Core удостоверения, но по-прежнему нуждаются в интеграции с доверенным внешним поставщиком проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d30b5-138">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="d30b5-139">В этом примере для проверки подлинности пользователей используется [Проверка подлинности Google](xref:security/authentication/google-logins) .</span><span class="sxs-lookup"><span data-stu-id="d30b5-139">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="d30b5-140">Использование аутентификации Google сдвигает многие сложности управления процессом входа в Google.</span><span class="sxs-lookup"><span data-stu-id="d30b5-140">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="d30b5-141">Чтобы выполнить интеграцию с другим внешним поставщиком проверки подлинности, см. следующие разделы:</span><span class="sxs-lookup"><span data-stu-id="d30b5-141">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="d30b5-142">Проверка подлинности Facebook</span><span class="sxs-lookup"><span data-stu-id="d30b5-142">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="d30b5-143">Проверка подлинности Майкрософт</span><span class="sxs-lookup"><span data-stu-id="d30b5-143">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="d30b5-144">Проверка подлинности Twitter</span><span class="sxs-lookup"><span data-stu-id="d30b5-144">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="d30b5-145">Другие поставщики</span><span class="sxs-lookup"><span data-stu-id="d30b5-145">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="d30b5-146">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="d30b5-146">Configuration</span></span>

<span data-ttu-id="d30b5-147">В методе `ConfigureServices` настройте схемы проверки подлинности приложения с помощью методов `AddAuthentication`, `AddCookie` и `AddGoogle`:</span><span class="sxs-lookup"><span data-stu-id="d30b5-147">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie`, and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="d30b5-148">Вызов [аддаусентикатион](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) задает [дефаултсчеме](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)приложения.</span><span class="sxs-lookup"><span data-stu-id="d30b5-148">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="d30b5-149">@No__t-0 — это схема по умолчанию, используемая следующими методами расширения проверки подлинности `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="d30b5-149">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="d30b5-150">Если задать для приложения `DefaultScheme` значение [кукиеаусентикатиондефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), приложение будет использовать файлы cookie в качестве схемы по умолчанию для этих методов расширения.</span><span class="sxs-lookup"><span data-stu-id="d30b5-150">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="d30b5-151">Если задать для приложения значение @no__t от 0 до [гугледефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), приложение будет использовать Google в качестве схемы по умолчанию для вызовов `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="d30b5-151">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="d30b5-152">`DefaultChallengeScheme` переопределяет `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="d30b5-152">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="d30b5-153">Дополнительные свойства, которые переопределяют `DefaultScheme` при установке, см. в разделе <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.</span><span class="sxs-lookup"><span data-stu-id="d30b5-153">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="d30b5-154">В методе `Configure` вызовите метод `UseAuthentication` для вызова по промежуточного слоя проверки подлинности, устанавливающего свойство `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="d30b5-154">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="d30b5-155">Вызовите метод `UseAuthentication` перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="d30b5-155">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="d30b5-156">Дополнительные сведения о схемах проверки подлинности и проверке подлинности файлов cookie см. в разделе <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="d30b5-156">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="d30b5-157">Применить авторизацию</span><span class="sxs-lookup"><span data-stu-id="d30b5-157">Apply authorization</span></span>

<span data-ttu-id="d30b5-158">Проверьте конфигурацию проверки подлинности приложения, применив атрибут `AuthorizeAttribute` к контроллеру, действию или странице.</span><span class="sxs-lookup"><span data-stu-id="d30b5-158">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="d30b5-159">Следующий код ограничивает доступ к странице *конфиденциальности* для пользователей, которые прошли проверку подлинности:</span><span class="sxs-lookup"><span data-stu-id="d30b5-159">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="d30b5-160">Выйти</span><span class="sxs-lookup"><span data-stu-id="d30b5-160">Sign out</span></span>

<span data-ttu-id="d30b5-161">Чтобы выйти из текущего пользователя и удалить его файл cookie, вызовите [сигнаутасинк](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="d30b5-161">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="d30b5-162">Следующий код добавляет обработчик страницы `Logout` на страницу *индекса* :</span><span class="sxs-lookup"><span data-stu-id="d30b5-162">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

<span data-ttu-id="d30b5-163">Обратите внимание, что при вызове `SignOutAsync` не указана схема проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d30b5-163">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="d30b5-164">Значение @no__t (0) приложения `CookieAuthenticationDefaults.AuthenticationScheme` используется для возврата.</span><span class="sxs-lookup"><span data-stu-id="d30b5-164">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d30b5-165">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d30b5-165">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
