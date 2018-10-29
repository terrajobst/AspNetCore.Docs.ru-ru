---
title: Использовать проверку подлинности файлов cookie без ASP.NET Core Identity
author: rick-anderson
description: Объяснение того, с использованием проверки подлинности файла cookie без ASP.NET Core Identity
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: 8add7559557d505397c3be8d8a48aa2e9d9e45e8
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207424"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="5b43c-103">Использовать проверку подлинности файлов cookie без ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="5b43c-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="5b43c-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Люк Лэтем](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5b43c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5b43c-105">Как вы уже видели в предыдущих разделах проверки подлинности, [удостоверения ASP.NET Core](xref:security/authentication/identity) — это поставщик проверки подлинности завершено, полнофункциональную для создания и обслуживания имена входа.</span><span class="sxs-lookup"><span data-stu-id="5b43c-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="5b43c-106">Тем не менее может потребоваться использование собственной логики настраиваемой проверки подлинности на основе файлов cookie проверки подлинности в некоторых случаях.</span><span class="sxs-lookup"><span data-stu-id="5b43c-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="5b43c-107">Проверка подлинности на основе файлов cookie можно использовать как автономного поставщика проверки подлинности без ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="5b43c-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="5b43c-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5b43c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5b43c-109">Для демонстрационных целей в примере приложения учетной записи пользователя для гипотетической пользователя Родригез Мария встроен в приложение.</span><span class="sxs-lookup"><span data-stu-id="5b43c-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="5b43c-110">Использовать имя пользователя по электронной почте "maria.rodriguez@contoso.com" и любой пароль для входа пользователя.</span><span class="sxs-lookup"><span data-stu-id="5b43c-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="5b43c-111">Пользователь проходит проверку подлинности в `AuthenticateUser` метод в *Pages/Account/Login.cshtml.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="5b43c-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="5b43c-112">В реальный пример пользователь может пройти проверку подлинности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5b43c-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="5b43c-113">Сведения о миграции файл cookie проверки подлинности с помощью ASP.NET Core 1.x на 2.0, см. в разделе [перенести проверки подлинности и другие удостоверения в разделе ASP.NET Core 2.0 (проверка подлинности на основе файлов Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="5b43c-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="5b43c-114">Чтобы использовать удостоверение ASP.NET Core, см. в разделе [Общие сведения об Identity](xref:security/authentication/identity) раздела.</span><span class="sxs-lookup"><span data-stu-id="5b43c-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="5b43c-115">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="5b43c-115">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5b43c-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5b43c-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="5b43c-117">Если приложение не использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), создайте ссылку на пакет в файле проекта для [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета (версии 2.1.0 или более поздние).</span><span class="sxs-lookup"><span data-stu-id="5b43c-117">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="5b43c-118">В `ConfigureServices` метод, создайте службу проверки подлинности по промежуточного слоя с `AddAuthentication` и `AddCookie` методов:</span><span class="sxs-lookup"><span data-stu-id="5b43c-118">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="5b43c-119">`AuthenticationScheme` передаваемый `AddAuthentication` задает схему проверки подлинности по умолчанию для приложения.</span><span class="sxs-lookup"><span data-stu-id="5b43c-119">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="5b43c-120">`AuthenticationScheme` полезно, если имеется несколько экземпляров файла cookie проверки подлинности, и вы хотите [авторизация с определенной схемой](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="5b43c-120">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="5b43c-121">Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы.</span><span class="sxs-lookup"><span data-stu-id="5b43c-121">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="5b43c-122">Вы можете указать любое строковое значение, являющийся отличительным признаком схемы.</span><span class="sxs-lookup"><span data-stu-id="5b43c-122">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="5b43c-123">Схема проверки подлинности приложения отличается от схемы проверки подлинности файла cookie для приложения.</span><span class="sxs-lookup"><span data-stu-id="5b43c-123">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="5b43c-124">Когда схему проверки подлинности файла cookie не предоставляется для <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, он использует [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) («файлы cookie»).</span><span class="sxs-lookup"><span data-stu-id="5b43c-124">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies").</span></span>

<span data-ttu-id="5b43c-125">В `Configure` используйте `UseAuthentication` метод, вызываемый по промежуточного слоя проверки подлинности, который задает `HttpContext.User` свойства.</span><span class="sxs-lookup"><span data-stu-id="5b43c-125">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="5b43c-126">Вызовите `UseAuthentication` метод перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="5b43c-126">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="5b43c-127">**Параметры AddCookie**</span><span class="sxs-lookup"><span data-stu-id="5b43c-127">**AddCookie Options**</span></span>

<span data-ttu-id="5b43c-128">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) класс используется для настройки параметров поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5b43c-128">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="5b43c-129">Параметр</span><span class="sxs-lookup"><span data-stu-id="5b43c-129">Option</span></span> | <span data-ttu-id="5b43c-130">Описание</span><span class="sxs-lookup"><span data-stu-id="5b43c-130">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="5b43c-131">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="5b43c-131">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-132">Предоставляет путь к предоставить "302 Found" (URL-адрес перенаправления) при активации `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-132">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="5b43c-133">Значение по умолчанию — `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-133">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="5b43c-134">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="5b43c-134">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-135">Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство на все утверждения, созданный с помощью службы проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="5b43c-135">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="5b43c-136">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="5b43c-136">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-137">Имя домена, где обслуживается куки-файл.</span><span class="sxs-lookup"><span data-stu-id="5b43c-137">The domain name where the cookie is served.</span></span> <span data-ttu-id="5b43c-138">По умолчанию это имя узла запроса.</span><span class="sxs-lookup"><span data-stu-id="5b43c-138">By default, this is the host name of the request.</span></span> <span data-ttu-id="5b43c-139">Браузер отправляет только куки-файл в запросах к соответствующим именем узла.</span><span class="sxs-lookup"><span data-stu-id="5b43c-139">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="5b43c-140">Вы можете настроить этот параметр для файлов cookie, доступных на любом узле в вашем домене.</span><span class="sxs-lookup"><span data-stu-id="5b43c-140">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="5b43c-141">Например, значение домена файла cookie `.contoso.com` сделает его доступным для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-141">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="5b43c-142">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="5b43c-142">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-143">Возвращает или задает срок действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="5b43c-143">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="5b43c-144">В настоящее время это параметр холостой командой и станут устаревшими в ASP.NET Core 2.1 или более поздней.</span><span class="sxs-lookup"><span data-stu-id="5b43c-144">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="5b43c-145">Используйте `ExpireTimeSpan` параметр, чтобы задать срок действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="5b43c-145">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="5b43c-146">Дополнительные сведения см. в разделе [уточнить поведение CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="5b43c-146">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="5b43c-147">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="5b43c-147">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-148">Флаг, указывающий файл cookie должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="5b43c-148">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="5b43c-149">Это значение изменится на `false` разрешает клиентские скрипты, чтобы получить доступ к куки-файл и может открыть свое приложение для хищения файлов cookie, должны иметь приложения [межузловых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость.</span><span class="sxs-lookup"><span data-stu-id="5b43c-149">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="5b43c-150">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-150">The default value is `true`.</span></span> |
| [<span data-ttu-id="5b43c-151">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="5b43c-151">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-152">Задает имя файла cookie.</span><span class="sxs-lookup"><span data-stu-id="5b43c-152">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="5b43c-153">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="5b43c-153">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-154">Использовать для изоляции приложений, выполняющихся в то же имя узла.</span><span class="sxs-lookup"><span data-stu-id="5b43c-154">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="5b43c-155">Если у вас есть приложение, запущенное в `/app1` и нужно ограничить файлы cookie для этого приложения, задайте `CookiePath` свойства `/app1`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-155">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="5b43c-156">Таким образом, файл cookie доступна только для запросов к `/app1` и любого приложения под ним.</span><span class="sxs-lookup"><span data-stu-id="5b43c-156">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="5b43c-157">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="5b43c-157">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-158">Указывает, разрешено ли браузер файлов cookie, должны быть присоединены к только запросы веб-сайте (`SameSiteMode.Strict`) или запросов между сайтами, с использованием безопасного HTTP-методов и запросов веб-сайте (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="5b43c-158">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="5b43c-159">Если задано значение `SameSiteMode.None`, не задано значение заголовка файла cookie.</span><span class="sxs-lookup"><span data-stu-id="5b43c-159">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="5b43c-160">Обратите внимание, что [промежуточного слоя политики](#cookie-policy-middleware) может перезаписать предоставленное значение.</span><span class="sxs-lookup"><span data-stu-id="5b43c-160">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="5b43c-161">Для поддержки проверки подлинности OAuth, значение по умолчанию — `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-161">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="5b43c-162">Дополнительные сведения см. в разделе [нарушена из-за политики SameSite файла cookie проверки подлинности OAuth](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="5b43c-162">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="5b43c-163">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="5b43c-163">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-164">Флаг, указывающий, если созданный файл cookie был ограничен HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и в запросе (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="5b43c-164">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="5b43c-165">Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-165">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="5b43c-166">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="5b43c-166">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-167">Наборы `DataProtectionProvider` , используемый для создания по умолчанию `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-167">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="5b43c-168">Если `TicketDataFormat` свойство задано, `DataProtectionProvider` параметр не используется.</span><span class="sxs-lookup"><span data-stu-id="5b43c-168">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="5b43c-169">Если не указано, используется поставщик защиты данных приложения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5b43c-169">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="5b43c-170">События</span><span class="sxs-lookup"><span data-stu-id="5b43c-170">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-171">Обработчик вызывает методы поставщика, который предоставить элемент управления приложения в определенные моменты обработки.</span><span class="sxs-lookup"><span data-stu-id="5b43c-171">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="5b43c-172">Если `Events` не переданы, предоставляется экземпляр по умолчанию, не выполняет никаких действий при вызове методов.</span><span class="sxs-lookup"><span data-stu-id="5b43c-172">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="5b43c-173">EventsType</span><span class="sxs-lookup"><span data-stu-id="5b43c-173">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-174">Используется как тип службы для получения `Events` экземпляром, а не свойства.</span><span class="sxs-lookup"><span data-stu-id="5b43c-174">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="5b43c-175">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="5b43c-175">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-176">`TimeSpan` После которого срока действия билета проверки подлинности, хранящийся в файл cookie.</span><span class="sxs-lookup"><span data-stu-id="5b43c-176">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="5b43c-177">`ExpireTimeSpan` добавляется на текущий момент времени, чтобы создать срок действия билета.</span><span class="sxs-lookup"><span data-stu-id="5b43c-177">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="5b43c-178">`ExpiredTimeSpan` Значение всегда переходит в зашифрованных AuthTicket, проверен сервером.</span><span class="sxs-lookup"><span data-stu-id="5b43c-178">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="5b43c-179">Он также может перейти в [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) заголовок, но только если `IsPersistent` имеет значение.</span><span class="sxs-lookup"><span data-stu-id="5b43c-179">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="5b43c-180">Чтобы задать `IsPersistent` для `true`, Настройка [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) передается `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-180">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="5b43c-181">Значение по умолчанию `ExpireTimeSpan` — 14 дней.</span><span class="sxs-lookup"><span data-stu-id="5b43c-181">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="5b43c-182">LoginPath</span><span class="sxs-lookup"><span data-stu-id="5b43c-182">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-183">Предоставляет путь к предоставить "302 Found" (URL-адрес перенаправления) при активации `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-183">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="5b43c-184">Добавляется текущий URL-адрес, вызвавший ответ 401 `LoginPath` как параметр строки запроса с именем, `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-184">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="5b43c-185">Запрос один раз на `LoginPath` предоставляет новое входа удостоверение, `ReturnUrlParameter` значение используется для перенаправления браузера обратно в URL-адрес, вызвавший исходный код несанкционированного состояния.</span><span class="sxs-lookup"><span data-stu-id="5b43c-185">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="5b43c-186">Значение по умолчанию — `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-186">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="5b43c-187">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="5b43c-187">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-188">Если `LogoutPath` предоставляется обработчику, а затем перенаправляет запрос по этому пути на основе значения из `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-188">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="5b43c-189">Значение по умолчанию — `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-189">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="5b43c-190">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="5b43c-190">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-191">Определяет имя параметра строки запроса, которое добавляется с помощью обработчика для ответа 302 Found (перенаправление URL-адрес).</span><span class="sxs-lookup"><span data-stu-id="5b43c-191">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="5b43c-192">`ReturnUrlParameter` используется при получении запроса на `LoginPath` или `LogoutPath` для возврата в браузере на исходный URL-адрес после выполнения действия имени входа или выхода.</span><span class="sxs-lookup"><span data-stu-id="5b43c-192">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="5b43c-193">Значение по умолчанию — `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-193">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="5b43c-194">SessionStore</span><span class="sxs-lookup"><span data-stu-id="5b43c-194">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-195">Необязательный контейнер, используемый для хранения идентификаторов всех запросов.</span><span class="sxs-lookup"><span data-stu-id="5b43c-195">An optional container used to store identity across requests.</span></span> <span data-ttu-id="5b43c-196">При использовании только идентификатор сеанса отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="5b43c-196">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="5b43c-197">`SessionStore` можно использовать для устранения потенциальных проблем с удостоверениями большой.</span><span class="sxs-lookup"><span data-stu-id="5b43c-197">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="5b43c-198">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="5b43c-198">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-199">Флаг, указывающий, если новый файл cookie с обновленной срок действия должен быть выдан динамически.</span><span class="sxs-lookup"><span data-stu-id="5b43c-199">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="5b43c-200">Это может произойти на любой запрос, где текущий период истечения срока действия файла cookie более чем на 50% срок действия истек.</span><span class="sxs-lookup"><span data-stu-id="5b43c-200">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="5b43c-201">Новый срок перемещается вперед быть текущая дата плюс `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-201">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="5b43c-202">[Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` при вызове метода `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-202">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="5b43c-203">Абсолютный срок действия можно повысить безопасность приложения, ограничивая количество времени, который действителен cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5b43c-203">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="5b43c-204">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-204">The default value is `true`.</span></span> |
| [<span data-ttu-id="5b43c-205">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="5b43c-205">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-206">`TicketDataFormat` Используется для применения и снятия защиты identity и других свойств, которые хранятся в значении cookie.</span><span class="sxs-lookup"><span data-stu-id="5b43c-206">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="5b43c-207">Если не указано, `TicketDataFormat` создается с помощью [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="5b43c-207">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="5b43c-208">Проверка</span><span class="sxs-lookup"><span data-stu-id="5b43c-208">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="5b43c-209">Метод, который проверяет, что параметры являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="5b43c-209">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="5b43c-210">Задайте `CookieAuthenticationOptions` в конфигурации службы для проверки подлинности в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="5b43c-210">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5b43c-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5b43c-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="5b43c-212">ASP.NET Core 1.x использует cookie [по промежуточного слоя](xref:fundamentals/middleware/index) , сериализует участника-пользователя в зашифрованном файле cookie.</span><span class="sxs-lookup"><span data-stu-id="5b43c-212">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="5b43c-213">При последующих запросах куки-файл проверяется, а основной сервер заново и назначенные `HttpContext.User` свойство.</span><span class="sxs-lookup"><span data-stu-id="5b43c-213">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="5b43c-214">Установка [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="5b43c-214">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="5b43c-215">Этот пакет содержит по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="5b43c-215">This package contains the cookie middleware.</span></span>

<span data-ttu-id="5b43c-216">Используйте `UseCookieAuthentication` метод в `Configure` метод в вашей *Startup.cs* файл перед `UseMvc` или `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="5b43c-216">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="5b43c-217">**Параметры CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="5b43c-217">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="5b43c-218">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) класс используется для настройки параметров поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5b43c-218">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="5b43c-219">Параметр</span><span class="sxs-lookup"><span data-stu-id="5b43c-219">Option</span></span> | <span data-ttu-id="5b43c-220">Описание</span><span class="sxs-lookup"><span data-stu-id="5b43c-220">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="5b43c-221">Значение AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="5b43c-221">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="5b43c-222">Задает схему проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5b43c-222">Sets the authentication scheme.</span></span> <span data-ttu-id="5b43c-223">`AuthenticationScheme` полезно, если имеется несколько экземпляров проверки подлинности, и вы хотите [авторизация с определенной схемой](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="5b43c-223">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="5b43c-224">Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы.</span><span class="sxs-lookup"><span data-stu-id="5b43c-224">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="5b43c-225">Вы можете указать любое строковое значение, являющийся отличительным признаком схемы.</span><span class="sxs-lookup"><span data-stu-id="5b43c-225">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="5b43c-226">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="5b43c-226">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="5b43c-227">Задает значение, указывающее, что файл cookie проверки подлинности следует выполняется при каждом запросе и пытаются проверить и воссоздания любому участнику сериализованный созданные им.</span><span class="sxs-lookup"><span data-stu-id="5b43c-227">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="5b43c-228">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="5b43c-228">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="5b43c-229">Если значение равно true, по промежуточного слоя проверки подлинности обрабатывает автоматического проблем.</span><span class="sxs-lookup"><span data-stu-id="5b43c-229">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="5b43c-230">Если значение равно false, по промежуточного слоя проверки подлинности только изменяет ответы, если они явно не указано `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-230">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="5b43c-231">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="5b43c-231">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="5b43c-232">Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство на все утверждения, созданные по промежуточного слоя проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5b43c-232">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="5b43c-233">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="5b43c-233">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="5b43c-234">Имя домена, где обслуживается куки-файл.</span><span class="sxs-lookup"><span data-stu-id="5b43c-234">The domain name where the cookie is served.</span></span> <span data-ttu-id="5b43c-235">По умолчанию это имя узла запроса.</span><span class="sxs-lookup"><span data-stu-id="5b43c-235">By default, this is the host name of the request.</span></span> <span data-ttu-id="5b43c-236">Браузер обслуживает только файл cookie для сопоставления имени узла.</span><span class="sxs-lookup"><span data-stu-id="5b43c-236">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="5b43c-237">Вы можете настроить этот параметр для файлов cookie, доступных на любом узле в вашем домене.</span><span class="sxs-lookup"><span data-stu-id="5b43c-237">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="5b43c-238">Например, значение домена файла cookie `.contoso.com` сделает его доступным для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-238">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="5b43c-239">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="5b43c-239">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="5b43c-240">Флаг, указывающий файл cookie должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="5b43c-240">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="5b43c-241">Это значение изменится на `false` разрешает клиентские скрипты, чтобы получить доступ к куки-файл и может открыть свое приложение для хищения файлов cookie, должны иметь приложения [межузловых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость.</span><span class="sxs-lookup"><span data-stu-id="5b43c-241">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="5b43c-242">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-242">The default value is `true`.</span></span> |
| [<span data-ttu-id="5b43c-243">CookiePath</span><span class="sxs-lookup"><span data-stu-id="5b43c-243">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="5b43c-244">Использовать для изоляции приложений, выполняющихся в то же имя узла.</span><span class="sxs-lookup"><span data-stu-id="5b43c-244">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="5b43c-245">Если у вас есть приложение, запущенное в `/app1` и нужно ограничить файлы cookie для этого приложения, задайте `CookiePath` свойства `/app1`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-245">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="5b43c-246">Таким образом, файл cookie доступна только для запросов к `/app1` и любого приложения под ним.</span><span class="sxs-lookup"><span data-stu-id="5b43c-246">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="5b43c-247">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="5b43c-247">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="5b43c-248">Флаг, указывающий, если созданный файл cookie был ограничен HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и в запросе (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="5b43c-248">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="5b43c-249">Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-249">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="5b43c-250">Описание</span><span class="sxs-lookup"><span data-stu-id="5b43c-250">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="5b43c-251">Дополнительные сведения о типе проверки подлинности, который становится доступен в приложение.</span><span class="sxs-lookup"><span data-stu-id="5b43c-251">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="5b43c-252">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="5b43c-252">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="5b43c-253">`TimeSpan` После которого срока действия билета проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5b43c-253">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="5b43c-254">Оно добавляется на текущий момент времени, чтобы создать срок действия билета.</span><span class="sxs-lookup"><span data-stu-id="5b43c-254">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="5b43c-255">Чтобы использовать `ExpireTimeSpan`, необходимо задать `IsPersistent` для `true` в `AuthenticationProperties` передается `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-255">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="5b43c-256">Значение по умолчанию — 14 дней.</span><span class="sxs-lookup"><span data-stu-id="5b43c-256">The default value is 14 days.</span></span> |
| [<span data-ttu-id="5b43c-257">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="5b43c-257">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="5b43c-258">Флаг, указывающий, сбрасывается ли дату истечения срока действия файла cookie при более половины `ExpireTimeSpan` промежуток времени истек.</span><span class="sxs-lookup"><span data-stu-id="5b43c-258">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="5b43c-259">Новое время exipiration перемещается вперед быть текущая дата плюс `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-259">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="5b43c-260">[Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` при вызове метода `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-260">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="5b43c-261">Абсолютный срок действия можно повысить безопасность приложения, ограничивая количество времени, который действителен cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5b43c-261">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="5b43c-262">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-262">The default value is `true`.</span></span> |

<span data-ttu-id="5b43c-263">Задайте `CookieAuthenticationOptions` для по промежуточного слоя проверки подлинности, в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="5b43c-263">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="5b43c-264">По промежуточного слоя для файлов cookie политики</span><span class="sxs-lookup"><span data-stu-id="5b43c-264">Cookie Policy Middleware</span></span>

<span data-ttu-id="5b43c-265">[По промежуточного слоя для файлов cookie политики](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) обеспечивает возможности политики файлов cookie в приложении.</span><span class="sxs-lookup"><span data-stu-id="5b43c-265">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="5b43c-266">Добавлением по промежуточного слоя в конвейере обработки приложение является конфиденциальным; порядок он влияет только на компоненты, зарегистрированные за ним в конвейере.</span><span class="sxs-lookup"><span data-stu-id="5b43c-266">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="5b43c-267">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) для по промежуточного слоя политики позволяют управлять общих характеристик обработки файлов cookie и обработчик в обработчики обработки файлов cookie, если файлы cookie добавляется или удаляется.</span><span class="sxs-lookup"><span data-stu-id="5b43c-267">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="5b43c-268">Свойство.</span><span class="sxs-lookup"><span data-stu-id="5b43c-268">Property</span></span> | <span data-ttu-id="5b43c-269">Описание</span><span class="sxs-lookup"><span data-stu-id="5b43c-269">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="5b43c-270">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="5b43c-270">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="5b43c-271">Влияет ли файлы cookie должен быть HttpOnly, который является флагом, показывающим куки-файл должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="5b43c-271">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="5b43c-272">Значение по умолчанию — `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-272">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="5b43c-273">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="5b43c-273">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="5b43c-274">Влияет на атрибут файла cookie веб-сайте (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="5b43c-274">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="5b43c-275">Значение по умолчанию — `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-275">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="5b43c-276">Этот параметр доступен для ASP.NET Core 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="5b43c-276">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="5b43c-277">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="5b43c-277">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="5b43c-278">Вызывается, когда файл cookie добавляется.</span><span class="sxs-lookup"><span data-stu-id="5b43c-278">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="5b43c-279">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="5b43c-279">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="5b43c-280">Вызывается при удалении файла cookie.</span><span class="sxs-lookup"><span data-stu-id="5b43c-280">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="5b43c-281">Защита</span><span class="sxs-lookup"><span data-stu-id="5b43c-281">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="5b43c-282">Влияет ли файлы cookie должен быть Secure.</span><span class="sxs-lookup"><span data-stu-id="5b43c-282">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="5b43c-283">Значение по умолчанию — `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-283">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="5b43c-284">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + только)</span><span class="sxs-lookup"><span data-stu-id="5b43c-284">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="5b43c-285">Значение по умолчанию `MinimumSameSitePolicy` значение `SameSiteMode.Lax` для разрешения проверки подлинности OAuth2.</span><span class="sxs-lookup"><span data-stu-id="5b43c-285">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="5b43c-286">Для строго ограничивает политику веб-сайте `SameSiteMode.Strict`, задайте `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-286">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="5b43c-287">Несмотря на то, что этот параметр приводит к сбою OAuth2 и других схем проверки подлинности независимо от источника, оно повышает уровень безопасности cookie для других типов приложений, которые не полагаются на обработку независимо от источника запроса.</span><span class="sxs-lookup"><span data-stu-id="5b43c-287">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="5b43c-288">Параметр политики промежуточного слоя для `MinimumSameSitePolicy` может повлиять на ваши равным `Cookie.SameSite` в `CookieAuthenticationOptions` параметры в соответствии с в таблице ниже.</span><span class="sxs-lookup"><span data-stu-id="5b43c-288">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="5b43c-289">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="5b43c-289">MinimumSameSitePolicy</span></span> | <span data-ttu-id="5b43c-290">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="5b43c-290">Cookie.SameSite</span></span> | <span data-ttu-id="5b43c-291">Результирующий параметр Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="5b43c-291">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="5b43c-292">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="5b43c-292">SameSiteMode.None</span></span>     | <span data-ttu-id="5b43c-293">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="5b43c-293">SameSiteMode.None</span></span><br><span data-ttu-id="5b43c-294">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5b43c-294">SameSiteMode.Lax</span></span><br><span data-ttu-id="5b43c-295">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5b43c-295">SameSiteMode.Strict</span></span> | <span data-ttu-id="5b43c-296">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="5b43c-296">SameSiteMode.None</span></span><br><span data-ttu-id="5b43c-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5b43c-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="5b43c-298">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5b43c-298">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="5b43c-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5b43c-299">SameSiteMode.Lax</span></span>      | <span data-ttu-id="5b43c-300">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="5b43c-300">SameSiteMode.None</span></span><br><span data-ttu-id="5b43c-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5b43c-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="5b43c-302">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5b43c-302">SameSiteMode.Strict</span></span> | <span data-ttu-id="5b43c-303">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5b43c-303">SameSiteMode.Lax</span></span><br><span data-ttu-id="5b43c-304">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5b43c-304">SameSiteMode.Lax</span></span><br><span data-ttu-id="5b43c-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5b43c-305">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="5b43c-306">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5b43c-306">SameSiteMode.Strict</span></span>   | <span data-ttu-id="5b43c-307">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="5b43c-307">SameSiteMode.None</span></span><br><span data-ttu-id="5b43c-308">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5b43c-308">SameSiteMode.Lax</span></span><br><span data-ttu-id="5b43c-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5b43c-309">SameSiteMode.Strict</span></span> | <span data-ttu-id="5b43c-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5b43c-310">SameSiteMode.Strict</span></span><br><span data-ttu-id="5b43c-311">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5b43c-311">SameSiteMode.Strict</span></span><br><span data-ttu-id="5b43c-312">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5b43c-312">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="5b43c-313">Создайте файл cookie проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="5b43c-313">Create an authentication cookie</span></span>

<span data-ttu-id="5b43c-314">Чтобы создать файл cookie, содержащий сведения о пользователе, то необходимо создать [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="5b43c-314">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="5b43c-315">Сведения о пользователе сериализуется и сохраняется в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="5b43c-315">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5b43c-316">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5b43c-316">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="5b43c-317">Создание [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) с любыми обязательными [утверждения](/dotnet/api/system.security.claims.claim)s и вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) для входа пользователя:</span><span class="sxs-lookup"><span data-stu-id="5b43c-317">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5b43c-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5b43c-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="5b43c-319">Вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) для входа пользователя:</span><span class="sxs-lookup"><span data-stu-id="5b43c-319">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="5b43c-320">`SignInAsync` создает зашифрованный файл cookie и добавляет его в текущий ответ.</span><span class="sxs-lookup"><span data-stu-id="5b43c-320">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="5b43c-321">Если вы не укажете `AuthenticationScheme`, используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5b43c-321">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="5b43c-322">На самом деле, шифрования, используемых — ASP.NET Core [защиты данных](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) системы.</span><span class="sxs-lookup"><span data-stu-id="5b43c-322">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="5b43c-323">Если вы размещаете приложения на нескольких компьютерах, балансировки нагрузки для приложений или с помощью веб-ферме, то необходимо [настроить защиту данных](xref:security/data-protection/configuration/overview) использование одного набора ключей и идентификатор приложения.</span><span class="sxs-lookup"><span data-stu-id="5b43c-323">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="5b43c-324">Выйти</span><span class="sxs-lookup"><span data-stu-id="5b43c-324">Sign out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5b43c-325">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5b43c-325">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="5b43c-326">Чтобы выйти из системы текущего пользователя и удалить их файл cookie, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="5b43c-326">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5b43c-327">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5b43c-327">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="5b43c-328">Чтобы выйти из системы текущего пользователя и удалить их файл cookie, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="5b43c-328">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="5b43c-329">Если вы не используете `CookieAuthenticationDefaults.AuthenticationScheme` (или файлы «cookie») как схему (например, «ContosoCookie»), укажите схему, используемую при настройке поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5b43c-329">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="5b43c-330">В противном случае используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5b43c-330">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="5b43c-331">Реагировать на изменения серверной части</span><span class="sxs-lookup"><span data-stu-id="5b43c-331">React to back-end changes</span></span>

<span data-ttu-id="5b43c-332">Создав файл cookie, он становится единственным источником удостоверения.</span><span class="sxs-lookup"><span data-stu-id="5b43c-332">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="5b43c-333">Даже при отключении пользователя в системах серверной системой проверки подлинности файла cookie не имеет сведений о это и пользователь остается в системе до тех пор, пока их файл cookie является допустимым.</span><span class="sxs-lookup"><span data-stu-id="5b43c-333">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="5b43c-334">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) событий в ASP.NET Core 2.x или [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) метод в ASP.NET Core 1.x можно использовать для перехвата и переопределение проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="5b43c-334">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="5b43c-335">Этот подход уменьшает риск появления отозванных пользователей, обращающихся к приложению.</span><span class="sxs-lookup"><span data-stu-id="5b43c-335">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="5b43c-336">Один из способов проверки cookie основан на слежении за при изменении пользователя базы данных.</span><span class="sxs-lookup"><span data-stu-id="5b43c-336">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="5b43c-337">Если базы данных не был изменен с момента выпуска файла cookie пользователя, нет необходимости пройти повторную аутентификацию пользователя, если их куки-файл все еще действителен.</span><span class="sxs-lookup"><span data-stu-id="5b43c-337">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="5b43c-338">Чтобы реализовать этот сценарий базы данных, реализованное в `IUserRepository` в этом примере хранит `LastChanged` значение.</span><span class="sxs-lookup"><span data-stu-id="5b43c-338">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="5b43c-339">При обновлении любого пользователя в базе данных, `LastChanged` имеет значение текущего времени.</span><span class="sxs-lookup"><span data-stu-id="5b43c-339">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="5b43c-340">Чтобы сделать недействительным файл cookie, когда база данных изменяется в зависимости от `LastChanged` создайте файл cookie с `LastChanged` утверждение, содержащее текущий `LastChanged` значение из базы данных:</span><span class="sxs-lookup"><span data-stu-id="5b43c-340">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5b43c-341">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5b43c-341">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5b43c-342">Для реализации переопределения для `ValidatePrincipal` события, написание метод со следующей сигнатурой в классе, унаследованном от [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="5b43c-342">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="5b43c-343">Пример выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5b43c-343">An example looks like the following:</span></span>

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

<span data-ttu-id="5b43c-344">Зарегистрируйте экземпляр события во время регистрации службы файл cookie в `ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="5b43c-344">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="5b43c-345">Укажите регистрацию ограниченной службы для вашего `CustomCookieAuthenticationEvents` класса:</span><span class="sxs-lookup"><span data-stu-id="5b43c-345">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5b43c-346">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5b43c-346">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5b43c-347">Для реализации переопределения для `ValidateAsync` события, написание метод со следующей сигнатурой:</span><span class="sxs-lookup"><span data-stu-id="5b43c-347">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="5b43c-348">Удостоверение ASP.NET Core реализует эту проверку в процессе его [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="5b43c-348">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="5b43c-349">Пример выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5b43c-349">An example looks like the following:</span></span>

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

<span data-ttu-id="5b43c-350">Регистрировать событие во время настройки файла cookie проверки подлинности в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="5b43c-350">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="5b43c-351">Рассмотрим ситуацию, в котором имя пользователя будет обновлено &mdash; решение, которое не влияет на безопасность любым способом.</span><span class="sxs-lookup"><span data-stu-id="5b43c-351">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="5b43c-352">Если вы хотите безболезненно обновить участника-пользователя, вызовите `context.ReplacePrincipal` и задайте `context.ShouldRenew` свойства `true`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-352">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="5b43c-353">Подход, описанный здесь активируется при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="5b43c-353">The approach described here is triggered on every request.</span></span> <span data-ttu-id="5b43c-354">Это может привести к снижению производительности для приложения.</span><span class="sxs-lookup"><span data-stu-id="5b43c-354">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="5b43c-355">Постоянные файлы cookie</span><span class="sxs-lookup"><span data-stu-id="5b43c-355">Persistent cookies</span></span>

<span data-ttu-id="5b43c-356">Требуется файл cookie для сохранения между сеансами браузера.</span><span class="sxs-lookup"><span data-stu-id="5b43c-356">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="5b43c-357">Такое сохранение должны включаться только с согласия пользователя с помощью флажка «Запомнить мои» на имя входа или похожий механизм.</span><span class="sxs-lookup"><span data-stu-id="5b43c-357">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="5b43c-358">Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, который выдерживает его через браузер замыкания.</span><span class="sxs-lookup"><span data-stu-id="5b43c-358">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="5b43c-359">Учитываются параметры скользящего срока действия ранее была настроена.</span><span class="sxs-lookup"><span data-stu-id="5b43c-359">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="5b43c-360">Если срок действия файла cookie, при закрытии браузера, браузер удаляет файл cookie после перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="5b43c-360">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5b43c-361">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5b43c-361">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="5b43c-362">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) класс находится в `Microsoft.AspNetCore.Authentication` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="5b43c-362">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5b43c-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5b43c-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="5b43c-364">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) класс находится в `Microsoft.AspNetCore.Http.Authentication` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="5b43c-364">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="5b43c-365">Срок действия файла cookie абсолютный</span><span class="sxs-lookup"><span data-stu-id="5b43c-365">Absolute cookie expiration</span></span>

<span data-ttu-id="5b43c-366">Можно задать абсолютный срок действия с `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="5b43c-366">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="5b43c-367">Необходимо также задать `IsPersistent`; в противном случае `ExpiresUtc` учитывается и создается файл cookie одного сеанса.</span><span class="sxs-lookup"><span data-stu-id="5b43c-367">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="5b43c-368">Когда `ExpiresUtc` устанавливается на `SignInAsync`, этот параметр переопределяет значение `ExpireTimeSpan` параметр `CookieAuthenticationOptions`, если задать.</span><span class="sxs-lookup"><span data-stu-id="5b43c-368">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="5b43c-369">Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, который выполняется в течение 20 минут.</span><span class="sxs-lookup"><span data-stu-id="5b43c-369">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="5b43c-370">Это не учитывает параметры скользящего срока действия ранее была настроена.</span><span class="sxs-lookup"><span data-stu-id="5b43c-370">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5b43c-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5b43c-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5b43c-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5b43c-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

---

## <a name="additional-resources"></a><span data-ttu-id="5b43c-373">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5b43c-373">Additional resources</span></span>

* [<span data-ttu-id="5b43c-374">Изменения AUTH 2.0 / объявления миграции</span><span class="sxs-lookup"><span data-stu-id="5b43c-374">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="5b43c-375">Проверок роли на основе политик</span><span class="sxs-lookup"><span data-stu-id="5b43c-375">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
