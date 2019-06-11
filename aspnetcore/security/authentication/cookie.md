---
title: Использовать проверку подлинности файлов cookie без ASP.NET Core Identity
author: rick-anderson
description: Объяснение того, с использованием проверки подлинности файла cookie без ASP.NET Core Identity
ms.author: riande
ms.date: 02/25/2019
uid: security/authentication/cookie
ms.openlocfilehash: f3e02b357a83cf5fc4b9fcdc79b2fbe80da98507
ms.sourcegitcommit: 9691b742134563b662948b0ed63f54ef7186801e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2019
ms.locfileid: "66824757"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="a29c8-103">Использовать проверку подлинности файлов cookie без ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="a29c8-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="a29c8-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Люк Лэтем](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a29c8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a29c8-105">Как вы уже видели в предыдущих разделах проверки подлинности, [удостоверения ASP.NET Core](xref:security/authentication/identity) — это поставщик проверки подлинности завершено, полнофункциональную для создания и обслуживания имена входа.</span><span class="sxs-lookup"><span data-stu-id="a29c8-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="a29c8-106">Тем не менее может потребоваться использование собственной логики настраиваемой проверки подлинности на основе файлов cookie проверки подлинности в некоторых случаях.</span><span class="sxs-lookup"><span data-stu-id="a29c8-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="a29c8-107">Проверка подлинности на основе файлов cookie можно использовать как автономного поставщика проверки подлинности без ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="a29c8-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="a29c8-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a29c8-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a29c8-109">Для демонстрационных целей в примере приложения учетной записи пользователя для гипотетической пользователя Родригез Мария встроен в приложение.</span><span class="sxs-lookup"><span data-stu-id="a29c8-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="a29c8-110">Использовать имя пользователя по электронной почте "maria.rodriguez@contoso.com" и любой пароль для входа пользователя.</span><span class="sxs-lookup"><span data-stu-id="a29c8-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="a29c8-111">Пользователь проходит проверку подлинности в `AuthenticateUser` метод в *Pages/Account/Login.cshtml.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="a29c8-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="a29c8-112">В реальный пример пользователь может пройти проверку подлинности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a29c8-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="a29c8-113">Сведения о миграции файл cookie проверки подлинности с помощью ASP.NET Core 1.x на 2.0, см. в разделе [перенести проверки подлинности и другие удостоверения в разделе ASP.NET Core 2.0 (проверка подлинности на основе файлов Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="a29c8-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="a29c8-114">Чтобы использовать удостоверение ASP.NET Core, см. в разделе [Общие сведения об Identity](xref:security/authentication/identity) раздела.</span><span class="sxs-lookup"><span data-stu-id="a29c8-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="a29c8-115">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="a29c8-115">Configuration</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a29c8-116">Если приложение не использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), создайте ссылку на пакет в файле проекта для [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета (версии 2.1.0 или более поздние).</span><span class="sxs-lookup"><span data-stu-id="a29c8-116">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="a29c8-117">В `ConfigureServices` метод, создайте службу проверки подлинности по промежуточного слоя с `AddAuthentication` и `AddCookie` методов:</span><span class="sxs-lookup"><span data-stu-id="a29c8-117">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="a29c8-118">`AuthenticationScheme` передаваемый `AddAuthentication` задает схему проверки подлинности по умолчанию для приложения.</span><span class="sxs-lookup"><span data-stu-id="a29c8-118">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="a29c8-119">`AuthenticationScheme` полезно, если имеется несколько экземпляров файла cookie проверки подлинности, и вы хотите [авторизация с определенной схемой](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="a29c8-119">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="a29c8-120">Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы.</span><span class="sxs-lookup"><span data-stu-id="a29c8-120">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="a29c8-121">Вы можете указать любое строковое значение, являющийся отличительным признаком схемы.</span><span class="sxs-lookup"><span data-stu-id="a29c8-121">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="a29c8-122">Схема проверки подлинности приложения отличается от схемы проверки подлинности файла cookie для приложения.</span><span class="sxs-lookup"><span data-stu-id="a29c8-122">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="a29c8-123">Когда схему проверки подлинности файла cookie не предоставляется для <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, он использует [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) («файлы cookie»).</span><span class="sxs-lookup"><span data-stu-id="a29c8-123">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies").</span></span>

<span data-ttu-id="a29c8-124">Файл cookie проверки подлинности <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> свойству `true` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a29c8-124">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="a29c8-125">Файлы cookie проверки подлинности разрешены, если посетитель не означает согласие на сбор данных.</span><span class="sxs-lookup"><span data-stu-id="a29c8-125">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="a29c8-126">Дополнительные сведения см. в разделе <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="a29c8-126">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="a29c8-127">В `Configure` используйте `UseAuthentication` метод, вызываемый по промежуточного слоя проверки подлинности, который задает `HttpContext.User` свойства.</span><span class="sxs-lookup"><span data-stu-id="a29c8-127">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="a29c8-128">Вызовите `UseAuthentication` метод перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="a29c8-128">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="a29c8-129">**Параметры AddCookie**</span><span class="sxs-lookup"><span data-stu-id="a29c8-129">**AddCookie Options**</span></span>

<span data-ttu-id="a29c8-130">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) класс используется для настройки параметров поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a29c8-130">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="a29c8-131">Параметр</span><span class="sxs-lookup"><span data-stu-id="a29c8-131">Option</span></span> | <span data-ttu-id="a29c8-132">Описание</span><span class="sxs-lookup"><span data-stu-id="a29c8-132">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="a29c8-133">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="a29c8-133">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-134">Предоставляет путь к предоставить "302 Found" (URL-адрес перенаправления) при активации `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-134">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="a29c8-135">Значение по умолчанию — `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-135">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="a29c8-136">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="a29c8-136">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-137">Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство на все утверждения, созданный с помощью службы проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="a29c8-137">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="a29c8-138">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="a29c8-138">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-139">Имя домена, где обслуживается куки-файл.</span><span class="sxs-lookup"><span data-stu-id="a29c8-139">The domain name where the cookie is served.</span></span> <span data-ttu-id="a29c8-140">По умолчанию это имя узла запроса.</span><span class="sxs-lookup"><span data-stu-id="a29c8-140">By default, this is the host name of the request.</span></span> <span data-ttu-id="a29c8-141">Браузер отправляет только куки-файл в запросах к соответствующим именем узла.</span><span class="sxs-lookup"><span data-stu-id="a29c8-141">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="a29c8-142">Вы можете настроить этот параметр для файлов cookie, доступных на любом узле в вашем домене.</span><span class="sxs-lookup"><span data-stu-id="a29c8-142">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="a29c8-143">Например, значение домена файла cookie `.contoso.com` сделает его доступным для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-143">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="a29c8-144">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="a29c8-144">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-145">Флаг, указывающий файл cookie должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="a29c8-145">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="a29c8-146">Это значение изменится на `false` разрешает клиентские скрипты, чтобы получить доступ к куки-файл и может открыть свое приложение для хищения файлов cookie, должны иметь приложения [межузловых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость.</span><span class="sxs-lookup"><span data-stu-id="a29c8-146">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="a29c8-147">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-147">The default value is `true`.</span></span> |
| [<span data-ttu-id="a29c8-148">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="a29c8-148">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-149">Задает имя файла cookie.</span><span class="sxs-lookup"><span data-stu-id="a29c8-149">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="a29c8-150">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="a29c8-150">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-151">Использовать для изоляции приложений, выполняющихся в то же имя узла.</span><span class="sxs-lookup"><span data-stu-id="a29c8-151">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="a29c8-152">Если у вас есть приложение, запущенное в `/app1` и нужно ограничить файлы cookie для этого приложения, задайте `CookiePath` свойства `/app1`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-152">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="a29c8-153">Таким образом, файл cookie доступна только для запросов к `/app1` и любого приложения под ним.</span><span class="sxs-lookup"><span data-stu-id="a29c8-153">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="a29c8-154">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="a29c8-154">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-155">Указывает, разрешено ли браузер файлов cookie, должны быть присоединены к только запросы веб-сайте (`SameSiteMode.Strict`) или запросов между сайтами, с использованием безопасного HTTP-методов и запросов веб-сайте (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="a29c8-155">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="a29c8-156">Если задано значение `SameSiteMode.None`, не задано значение заголовка файла cookie.</span><span class="sxs-lookup"><span data-stu-id="a29c8-156">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="a29c8-157">Обратите внимание, что [промежуточного слоя политики](#cookie-policy-middleware) может перезаписать предоставленное значение.</span><span class="sxs-lookup"><span data-stu-id="a29c8-157">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="a29c8-158">Для поддержки проверки подлинности OAuth, значение по умолчанию — `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-158">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="a29c8-159">Дополнительные сведения см. в разделе [нарушена из-за политики SameSite файла cookie проверки подлинности OAuth](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="a29c8-159">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="a29c8-160">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="a29c8-160">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-161">Флаг, указывающий, если созданный файл cookie был ограничен HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и в запросе (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="a29c8-161">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="a29c8-162">Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-162">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="a29c8-163">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="a29c8-163">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-164">Наборы `DataProtectionProvider` , используемый для создания по умолчанию `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-164">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="a29c8-165">Если `TicketDataFormat` свойство задано, `DataProtectionProvider` параметр не используется.</span><span class="sxs-lookup"><span data-stu-id="a29c8-165">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="a29c8-166">Если не указано, используется поставщик защиты данных приложения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a29c8-166">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="a29c8-167">События</span><span class="sxs-lookup"><span data-stu-id="a29c8-167">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-168">Обработчик вызывает методы поставщика, который предоставить элемент управления приложения в определенные моменты обработки.</span><span class="sxs-lookup"><span data-stu-id="a29c8-168">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="a29c8-169">Если `Events` не переданы, предоставляется экземпляр по умолчанию, не выполняет никаких действий при вызове методов.</span><span class="sxs-lookup"><span data-stu-id="a29c8-169">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="a29c8-170">EventsType</span><span class="sxs-lookup"><span data-stu-id="a29c8-170">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-171">Используется как тип службы для получения `Events` экземпляром, а не свойства.</span><span class="sxs-lookup"><span data-stu-id="a29c8-171">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="a29c8-172">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="a29c8-172">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-173">`TimeSpan` После которого срока действия билета проверки подлинности, хранящийся в файл cookie.</span><span class="sxs-lookup"><span data-stu-id="a29c8-173">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="a29c8-174">`ExpireTimeSpan` добавляется на текущий момент времени, чтобы создать срок действия билета.</span><span class="sxs-lookup"><span data-stu-id="a29c8-174">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="a29c8-175">`ExpiredTimeSpan` Значение всегда переходит в зашифрованных AuthTicket, проверен сервером.</span><span class="sxs-lookup"><span data-stu-id="a29c8-175">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="a29c8-176">Он также может перейти в [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) заголовок, но только если `IsPersistent` имеет значение.</span><span class="sxs-lookup"><span data-stu-id="a29c8-176">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="a29c8-177">Чтобы задать `IsPersistent` для `true`, Настройка [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) передается `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-177">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="a29c8-178">Значение по умолчанию `ExpireTimeSpan` — 14 дней.</span><span class="sxs-lookup"><span data-stu-id="a29c8-178">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="a29c8-179">LoginPath</span><span class="sxs-lookup"><span data-stu-id="a29c8-179">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-180">Предоставляет путь к предоставить "302 Found" (URL-адрес перенаправления) при активации `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-180">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="a29c8-181">Добавляется текущий URL-адрес, вызвавший ответ 401 `LoginPath` как параметр строки запроса с именем, `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-181">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="a29c8-182">Запрос один раз на `LoginPath` предоставляет новое входа удостоверение, `ReturnUrlParameter` значение используется для перенаправления браузера обратно в URL-адрес, вызвавший исходный код несанкционированного состояния.</span><span class="sxs-lookup"><span data-stu-id="a29c8-182">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="a29c8-183">Значение по умолчанию — `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-183">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="a29c8-184">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="a29c8-184">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-185">Если `LogoutPath` предоставляется обработчику, а затем перенаправляет запрос по этому пути на основе значения из `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-185">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="a29c8-186">Значение по умолчанию — `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-186">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="a29c8-187">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="a29c8-187">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-188">Определяет имя параметра строки запроса, которое добавляется с помощью обработчика для ответа 302 Found (перенаправление URL-адрес).</span><span class="sxs-lookup"><span data-stu-id="a29c8-188">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="a29c8-189">`ReturnUrlParameter` используется при получении запроса на `LoginPath` или `LogoutPath` для возврата в браузере на исходный URL-адрес после выполнения действия имени входа или выхода.</span><span class="sxs-lookup"><span data-stu-id="a29c8-189">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="a29c8-190">Значение по умолчанию — `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-190">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="a29c8-191">SessionStore</span><span class="sxs-lookup"><span data-stu-id="a29c8-191">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-192">Необязательный контейнер, используемый для хранения идентификаторов всех запросов.</span><span class="sxs-lookup"><span data-stu-id="a29c8-192">An optional container used to store identity across requests.</span></span> <span data-ttu-id="a29c8-193">При использовании только идентификатор сеанса отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="a29c8-193">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="a29c8-194">`SessionStore` можно использовать для устранения потенциальных проблем с удостоверениями большой.</span><span class="sxs-lookup"><span data-stu-id="a29c8-194">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="a29c8-195">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="a29c8-195">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-196">Флаг, указывающий, если новый файл cookie с обновленной срок действия должен быть выдан динамически.</span><span class="sxs-lookup"><span data-stu-id="a29c8-196">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="a29c8-197">Это может произойти на любой запрос, где текущий период истечения срока действия файла cookie более чем на 50% срок действия истек.</span><span class="sxs-lookup"><span data-stu-id="a29c8-197">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="a29c8-198">Новый срок перемещается вперед быть текущая дата плюс `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-198">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="a29c8-199">[Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` при вызове метода `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-199">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="a29c8-200">Абсолютный срок действия можно повысить безопасность приложения, ограничивая количество времени, который действителен cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a29c8-200">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="a29c8-201">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-201">The default value is `true`.</span></span> |
| [<span data-ttu-id="a29c8-202">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="a29c8-202">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-203">`TicketDataFormat` Используется для применения и снятия защиты identity и других свойств, которые хранятся в значении cookie.</span><span class="sxs-lookup"><span data-stu-id="a29c8-203">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="a29c8-204">Если не указано, `TicketDataFormat` создается с помощью [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="a29c8-204">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="a29c8-205">Проверка</span><span class="sxs-lookup"><span data-stu-id="a29c8-205">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="a29c8-206">Метод, который проверяет, что параметры являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="a29c8-206">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="a29c8-207">Задайте `CookieAuthenticationOptions` в конфигурации службы для проверки подлинности в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="a29c8-207">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a29c8-208">ASP.NET Core 1.x использует cookie [по промежуточного слоя](xref:fundamentals/middleware/index) , сериализует участника-пользователя в зашифрованном файле cookie.</span><span class="sxs-lookup"><span data-stu-id="a29c8-208">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="a29c8-209">При последующих запросах куки-файл проверяется, а основной сервер заново и назначенные `HttpContext.User` свойство.</span><span class="sxs-lookup"><span data-stu-id="a29c8-209">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="a29c8-210">Установка [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="a29c8-210">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="a29c8-211">Этот пакет содержит по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="a29c8-211">This package contains the cookie middleware.</span></span>

<span data-ttu-id="a29c8-212">Используйте `UseCookieAuthentication` метод в `Configure` метод в вашей *Startup.cs* файл перед `UseMvc` или `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="a29c8-212">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="a29c8-213">**CookieAuthenticationOptions Options**</span><span class="sxs-lookup"><span data-stu-id="a29c8-213">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="a29c8-214">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) класс используется для настройки параметров поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a29c8-214">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="a29c8-215">Параметр</span><span class="sxs-lookup"><span data-stu-id="a29c8-215">Option</span></span> | <span data-ttu-id="a29c8-216">Описание</span><span class="sxs-lookup"><span data-stu-id="a29c8-216">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="a29c8-217">Значение AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="a29c8-217">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="a29c8-218">Задает схему проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a29c8-218">Sets the authentication scheme.</span></span> <span data-ttu-id="a29c8-219">`AuthenticationScheme` полезно, если имеется несколько экземпляров проверки подлинности, и вы хотите [авторизация с определенной схемой](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="a29c8-219">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="a29c8-220">Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы.</span><span class="sxs-lookup"><span data-stu-id="a29c8-220">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="a29c8-221">Вы можете указать любое строковое значение, являющийся отличительным признаком схемы.</span><span class="sxs-lookup"><span data-stu-id="a29c8-221">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="a29c8-222">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="a29c8-222">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="a29c8-223">Задает значение, указывающее, что файл cookie проверки подлинности следует выполняется при каждом запросе и пытаются проверить и воссоздания любому участнику сериализованный созданные им.</span><span class="sxs-lookup"><span data-stu-id="a29c8-223">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="a29c8-224">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="a29c8-224">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="a29c8-225">Если значение равно true, по промежуточного слоя проверки подлинности обрабатывает автоматического проблем.</span><span class="sxs-lookup"><span data-stu-id="a29c8-225">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="a29c8-226">Если значение равно false, по промежуточного слоя проверки подлинности только изменяет ответы, если они явно не указано `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-226">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="a29c8-227">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="a29c8-227">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="a29c8-228">Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство на все утверждения, созданные по промежуточного слоя проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a29c8-228">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="a29c8-229">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="a29c8-229">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="a29c8-230">Имя домена, где обслуживается куки-файл.</span><span class="sxs-lookup"><span data-stu-id="a29c8-230">The domain name where the cookie is served.</span></span> <span data-ttu-id="a29c8-231">По умолчанию это имя узла запроса.</span><span class="sxs-lookup"><span data-stu-id="a29c8-231">By default, this is the host name of the request.</span></span> <span data-ttu-id="a29c8-232">Браузер обслуживает только файл cookie для сопоставления имени узла.</span><span class="sxs-lookup"><span data-stu-id="a29c8-232">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="a29c8-233">Вы можете настроить этот параметр для файлов cookie, доступных на любом узле в вашем домене.</span><span class="sxs-lookup"><span data-stu-id="a29c8-233">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="a29c8-234">Например, значение домена файла cookie `.contoso.com` сделает его доступным для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-234">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="a29c8-235">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="a29c8-235">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="a29c8-236">Флаг, указывающий файл cookie должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="a29c8-236">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="a29c8-237">Это значение изменится на `false` разрешает клиентские скрипты, чтобы получить доступ к куки-файл и может открыть свое приложение для хищения файлов cookie, должны иметь приложения [межузловых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость.</span><span class="sxs-lookup"><span data-stu-id="a29c8-237">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="a29c8-238">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-238">The default value is `true`.</span></span> |
| [<span data-ttu-id="a29c8-239">CookiePath</span><span class="sxs-lookup"><span data-stu-id="a29c8-239">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="a29c8-240">Использовать для изоляции приложений, выполняющихся в то же имя узла.</span><span class="sxs-lookup"><span data-stu-id="a29c8-240">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="a29c8-241">Если у вас есть приложение, запущенное в `/app1` и нужно ограничить файлы cookie для этого приложения, задайте `CookiePath` свойства `/app1`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-241">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="a29c8-242">Таким образом, файл cookie доступна только для запросов к `/app1` и любого приложения под ним.</span><span class="sxs-lookup"><span data-stu-id="a29c8-242">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="a29c8-243">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="a29c8-243">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="a29c8-244">Флаг, указывающий, если созданный файл cookie был ограничен HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и в запросе (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="a29c8-244">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="a29c8-245">Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-245">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="a29c8-246">Описание</span><span class="sxs-lookup"><span data-stu-id="a29c8-246">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="a29c8-247">Дополнительные сведения о типе проверки подлинности, который становится доступен в приложение.</span><span class="sxs-lookup"><span data-stu-id="a29c8-247">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="a29c8-248">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="a29c8-248">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="a29c8-249">`TimeSpan` После которого срока действия билета проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a29c8-249">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="a29c8-250">Оно добавляется на текущий момент времени, чтобы создать срок действия билета.</span><span class="sxs-lookup"><span data-stu-id="a29c8-250">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="a29c8-251">Чтобы использовать `ExpireTimeSpan`, необходимо задать `IsPersistent` для `true` в `AuthenticationProperties` передается `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-251">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="a29c8-252">Значение по умолчанию — 14 дней.</span><span class="sxs-lookup"><span data-stu-id="a29c8-252">The default value is 14 days.</span></span> |
| [<span data-ttu-id="a29c8-253">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="a29c8-253">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="a29c8-254">Флаг, указывающий, сбрасывается ли дату истечения срока действия файла cookie при более половины `ExpireTimeSpan` промежуток времени истек.</span><span class="sxs-lookup"><span data-stu-id="a29c8-254">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="a29c8-255">Новый срок перемещается вперед быть текущая дата плюс `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-255">The new expiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="a29c8-256">[Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` при вызове метода `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-256">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="a29c8-257">Абсолютный срок действия можно повысить безопасность приложения, ограничивая количество времени, который действителен cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a29c8-257">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="a29c8-258">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-258">The default value is `true`.</span></span> |

<span data-ttu-id="a29c8-259">Задайте `CookieAuthenticationOptions` для по промежуточного слоя проверки подлинности, в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="a29c8-259">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a><span data-ttu-id="a29c8-260">По промежуточного слоя для файлов cookie политики</span><span class="sxs-lookup"><span data-stu-id="a29c8-260">Cookie Policy Middleware</span></span>

<span data-ttu-id="a29c8-261">[По промежуточного слоя для файлов cookie политики](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) обеспечивает возможности политики файлов cookie в приложении.</span><span class="sxs-lookup"><span data-stu-id="a29c8-261">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="a29c8-262">Добавлением по промежуточного слоя в конвейере обработки приложение является конфиденциальным; порядок он влияет только на компоненты, зарегистрированные за ним в конвейере.</span><span class="sxs-lookup"><span data-stu-id="a29c8-262">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="a29c8-263">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) для по промежуточного слоя политики позволяют управлять общих характеристик обработки файлов cookie и обработчик в обработчики обработки файлов cookie, если файлы cookie добавляется или удаляется.</span><span class="sxs-lookup"><span data-stu-id="a29c8-263">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="a29c8-264">Свойство</span><span class="sxs-lookup"><span data-stu-id="a29c8-264">Property</span></span> | <span data-ttu-id="a29c8-265">Описание</span><span class="sxs-lookup"><span data-stu-id="a29c8-265">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="a29c8-266">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="a29c8-266">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="a29c8-267">Влияет ли файлы cookie должен быть HttpOnly, который является флагом, показывающим куки-файл должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="a29c8-267">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="a29c8-268">Значение по умолчанию — `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-268">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="a29c8-269">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="a29c8-269">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="a29c8-270">Влияет на атрибут файла cookie веб-сайте (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="a29c8-270">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="a29c8-271">Значение по умолчанию — `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-271">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="a29c8-272">Этот параметр доступен для ASP.NET Core 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="a29c8-272">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="a29c8-273">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="a29c8-273">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="a29c8-274">Вызывается, когда файл cookie добавляется.</span><span class="sxs-lookup"><span data-stu-id="a29c8-274">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="a29c8-275">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="a29c8-275">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="a29c8-276">Вызывается при удалении файла cookie.</span><span class="sxs-lookup"><span data-stu-id="a29c8-276">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="a29c8-277">Защита</span><span class="sxs-lookup"><span data-stu-id="a29c8-277">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="a29c8-278">Влияет ли файлы cookie должен быть Secure.</span><span class="sxs-lookup"><span data-stu-id="a29c8-278">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="a29c8-279">Значение по умолчанию — `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-279">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="a29c8-280">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + только)</span><span class="sxs-lookup"><span data-stu-id="a29c8-280">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="a29c8-281">Значение по умолчанию `MinimumSameSitePolicy` значение `SameSiteMode.Lax` для разрешения проверки подлинности OAuth2.</span><span class="sxs-lookup"><span data-stu-id="a29c8-281">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="a29c8-282">Для строго ограничивает политику веб-сайте `SameSiteMode.Strict`, задайте `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-282">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="a29c8-283">Несмотря на то, что этот параметр приводит к сбою OAuth2 и других схем проверки подлинности независимо от источника, оно повышает уровень безопасности cookie для других типов приложений, которые не полагаются на обработку независимо от источника запроса.</span><span class="sxs-lookup"><span data-stu-id="a29c8-283">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="a29c8-284">Параметр политики промежуточного слоя для `MinimumSameSitePolicy` может повлиять на ваши равным `Cookie.SameSite` в `CookieAuthenticationOptions` параметры в соответствии с в таблице ниже.</span><span class="sxs-lookup"><span data-stu-id="a29c8-284">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="a29c8-285">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="a29c8-285">MinimumSameSitePolicy</span></span> | <span data-ttu-id="a29c8-286">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="a29c8-286">Cookie.SameSite</span></span> | <span data-ttu-id="a29c8-287">Результирующий параметр Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="a29c8-287">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="a29c8-288">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="a29c8-288">SameSiteMode.None</span></span>     | <span data-ttu-id="a29c8-289">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="a29c8-289">SameSiteMode.None</span></span><br><span data-ttu-id="a29c8-290">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="a29c8-290">SameSiteMode.Lax</span></span><br><span data-ttu-id="a29c8-291">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="a29c8-291">SameSiteMode.Strict</span></span> | <span data-ttu-id="a29c8-292">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="a29c8-292">SameSiteMode.None</span></span><br><span data-ttu-id="a29c8-293">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="a29c8-293">SameSiteMode.Lax</span></span><br><span data-ttu-id="a29c8-294">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="a29c8-294">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="a29c8-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="a29c8-295">SameSiteMode.Lax</span></span>      | <span data-ttu-id="a29c8-296">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="a29c8-296">SameSiteMode.None</span></span><br><span data-ttu-id="a29c8-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="a29c8-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="a29c8-298">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="a29c8-298">SameSiteMode.Strict</span></span> | <span data-ttu-id="a29c8-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="a29c8-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="a29c8-300">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="a29c8-300">SameSiteMode.Lax</span></span><br><span data-ttu-id="a29c8-301">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="a29c8-301">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="a29c8-302">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="a29c8-302">SameSiteMode.Strict</span></span>   | <span data-ttu-id="a29c8-303">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="a29c8-303">SameSiteMode.None</span></span><br><span data-ttu-id="a29c8-304">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="a29c8-304">SameSiteMode.Lax</span></span><br><span data-ttu-id="a29c8-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="a29c8-305">SameSiteMode.Strict</span></span> | <span data-ttu-id="a29c8-306">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="a29c8-306">SameSiteMode.Strict</span></span><br><span data-ttu-id="a29c8-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="a29c8-307">SameSiteMode.Strict</span></span><br><span data-ttu-id="a29c8-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="a29c8-308">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="a29c8-309">Создайте файл cookie проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="a29c8-309">Create an authentication cookie</span></span>

<span data-ttu-id="a29c8-310">Чтобы создать файл cookie, содержащий сведения о пользователе, то необходимо создать [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="a29c8-310">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="a29c8-311">Сведения о пользователе сериализуется и сохраняется в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="a29c8-311">The user information is serialized and stored in the cookie.</span></span> 

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a29c8-312">Создание [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) с любыми обязательными [утверждения](/dotnet/api/system.security.claims.claim)s и вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) для входа пользователя:</span><span class="sxs-lookup"><span data-stu-id="a29c8-312">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a29c8-313">Вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) для входа пользователя:</span><span class="sxs-lookup"><span data-stu-id="a29c8-313">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

<span data-ttu-id="a29c8-314">`SignInAsync` создает зашифрованный файл cookie и добавляет его в текущий ответ.</span><span class="sxs-lookup"><span data-stu-id="a29c8-314">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="a29c8-315">Если вы не укажете `AuthenticationScheme`, используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a29c8-315">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="a29c8-316">На самом деле, шифрования, используемых — ASP.NET Core [защиты данных](xref:security/data-protection/using-data-protection) системы.</span><span class="sxs-lookup"><span data-stu-id="a29c8-316">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system.</span></span> <span data-ttu-id="a29c8-317">Если вы размещаете приложения на нескольких компьютерах, балансировки нагрузки для приложений или с помощью веб-ферме, то необходимо [настроить защиту данных](xref:security/data-protection/configuration/overview) использование одного набора ключей и идентификатор приложения.</span><span class="sxs-lookup"><span data-stu-id="a29c8-317">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="a29c8-318">Выйти</span><span class="sxs-lookup"><span data-stu-id="a29c8-318">Sign out</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a29c8-319">Чтобы выйти из системы текущего пользователя и удалить их файл cookie, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="a29c8-319">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a29c8-320">Чтобы выйти из системы текущего пользователя и удалить их файл cookie, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="a29c8-320">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

<span data-ttu-id="a29c8-321">Если вы не используете `CookieAuthenticationDefaults.AuthenticationScheme` (или файлы «cookie») как схему (например, «ContosoCookie»), укажите схему, используемую при настройке поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a29c8-321">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="a29c8-322">В противном случае используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a29c8-322">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="a29c8-323">Реагировать на изменения серверной части</span><span class="sxs-lookup"><span data-stu-id="a29c8-323">React to back-end changes</span></span>

<span data-ttu-id="a29c8-324">Создав файл cookie, он становится единственным источником удостоверения.</span><span class="sxs-lookup"><span data-stu-id="a29c8-324">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="a29c8-325">Даже при отключении пользователя в системах серверной системой проверки подлинности файла cookie не имеет сведений о это и пользователь остается в системе до тех пор, пока их файл cookie является допустимым.</span><span class="sxs-lookup"><span data-stu-id="a29c8-325">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="a29c8-326">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) событий в ASP.NET Core 2.x или [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) метод в ASP.NET Core 1.x можно использовать для перехвата и переопределение проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="a29c8-326">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="a29c8-327">Этот подход уменьшает риск появления отозванных пользователей, обращающихся к приложению.</span><span class="sxs-lookup"><span data-stu-id="a29c8-327">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="a29c8-328">Один из способов проверки cookie основан на слежении за при изменении пользователя базы данных.</span><span class="sxs-lookup"><span data-stu-id="a29c8-328">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="a29c8-329">Если базы данных не был изменен с момента выпуска файла cookie пользователя, нет необходимости пройти повторную аутентификацию пользователя, если их куки-файл все еще действителен.</span><span class="sxs-lookup"><span data-stu-id="a29c8-329">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="a29c8-330">Чтобы реализовать этот сценарий базы данных, реализованное в `IUserRepository` в этом примере хранит `LastChanged` значение.</span><span class="sxs-lookup"><span data-stu-id="a29c8-330">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="a29c8-331">При обновлении любого пользователя в базе данных, `LastChanged` имеет значение текущего времени.</span><span class="sxs-lookup"><span data-stu-id="a29c8-331">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="a29c8-332">Чтобы сделать недействительным файл cookie, когда база данных изменяется в зависимости от `LastChanged` создайте файл cookie с `LastChanged` утверждение, содержащее текущий `LastChanged` значение из базы данных:</span><span class="sxs-lookup"><span data-stu-id="a29c8-332">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="a29c8-333">Для реализации переопределения для `ValidatePrincipal` события, написание метод со следующей сигнатурой в классе, унаследованном от [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="a29c8-333">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="a29c8-334">Пример выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a29c8-334">An example looks like the following:</span></span>

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

<span data-ttu-id="a29c8-335">Зарегистрируйте экземпляр события во время регистрации службы файл cookie в `ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="a29c8-335">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="a29c8-336">Укажите регистрацию ограниченной службы для вашего `CustomCookieAuthenticationEvents` класса:</span><span class="sxs-lookup"><span data-stu-id="a29c8-336">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

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

<span data-ttu-id="a29c8-337">Для реализации переопределения для `ValidateAsync` события, написание метод со следующей сигнатурой:</span><span class="sxs-lookup"><span data-stu-id="a29c8-337">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="a29c8-338">Удостоверение ASP.NET Core реализует эту проверку в процессе его [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="a29c8-338">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="a29c8-339">Пример выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a29c8-339">An example looks like the following:</span></span>

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

<span data-ttu-id="a29c8-340">Регистрировать событие во время настройки файла cookie проверки подлинности в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="a29c8-340">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

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

<span data-ttu-id="a29c8-341">Рассмотрим ситуацию, в котором имя пользователя будет обновлено &mdash; решение, которое не влияет на безопасность любым способом.</span><span class="sxs-lookup"><span data-stu-id="a29c8-341">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="a29c8-342">Если вы хотите безболезненно обновить участника-пользователя, вызовите `context.ReplacePrincipal` и задайте `context.ShouldRenew` свойства `true`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-342">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="a29c8-343">Подход, описанный здесь активируется при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="a29c8-343">The approach described here is triggered on every request.</span></span> <span data-ttu-id="a29c8-344">Это может привести к снижению производительности для приложения.</span><span class="sxs-lookup"><span data-stu-id="a29c8-344">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="a29c8-345">Постоянные файлы cookie</span><span class="sxs-lookup"><span data-stu-id="a29c8-345">Persistent cookies</span></span>

<span data-ttu-id="a29c8-346">Требуется файл cookie для сохранения между сеансами браузера.</span><span class="sxs-lookup"><span data-stu-id="a29c8-346">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="a29c8-347">Такое сохранение должны включаться только с согласия пользователя с помощью флажка «Запомнить мои» на имя входа или похожий механизм.</span><span class="sxs-lookup"><span data-stu-id="a29c8-347">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on login or a similar mechanism.</span></span> 

<span data-ttu-id="a29c8-348">Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, который выдерживает его через браузер замыкания.</span><span class="sxs-lookup"><span data-stu-id="a29c8-348">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="a29c8-349">Учитываются параметры скользящего срока действия ранее была настроена.</span><span class="sxs-lookup"><span data-stu-id="a29c8-349">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="a29c8-350">Если срок действия файла cookie, при закрытии браузера, браузер удаляет файл cookie после перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="a29c8-350">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

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

<span data-ttu-id="a29c8-351">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) класс находится в `Microsoft.AspNetCore.Authentication` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="a29c8-351">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

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

<span data-ttu-id="a29c8-352">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) класс находится в `Microsoft.AspNetCore.Http.Authentication` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="a29c8-352">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

::: moniker-end

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="a29c8-353">Срок действия файла cookie абсолютный</span><span class="sxs-lookup"><span data-stu-id="a29c8-353">Absolute cookie expiration</span></span>

<span data-ttu-id="a29c8-354">Можно задать абсолютный срок действия с `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="a29c8-354">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="a29c8-355">Чтобы создать постоянный файл cookie, необходимо также задать `IsPersistent`; в противном случае файл cookie создается с временем жизни на основе сеансов и срок действия может истечь до или после проверки подлинности билета, она хранит.</span><span class="sxs-lookup"><span data-stu-id="a29c8-355">To create a persistent cookie, you must also set `IsPersistent`; otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="a29c8-356">Когда `ExpiresUtc` устанавливается на `SignInAsync`, этот параметр переопределяет значение `ExpireTimeSpan` параметр `CookieAuthenticationOptions`, если задать.</span><span class="sxs-lookup"><span data-stu-id="a29c8-356">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="a29c8-357">Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, который выполняется в течение 20 минут.</span><span class="sxs-lookup"><span data-stu-id="a29c8-357">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="a29c8-358">Это не учитывает параметры скользящего срока действия ранее была настроена.</span><span class="sxs-lookup"><span data-stu-id="a29c8-358">This ignores any sliding expiration settings previously configured.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="a29c8-359">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a29c8-359">Additional resources</span></span>

* [<span data-ttu-id="a29c8-360">Изменения AUTH 2.0 / объявления миграции</span><span class="sxs-lookup"><span data-stu-id="a29c8-360">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="a29c8-361">Проверок роли на основе политик</span><span class="sxs-lookup"><span data-stu-id="a29c8-361">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
