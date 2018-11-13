---
title: Использовать проверку подлинности файлов cookie без ASP.NET Core Identity
author: rick-anderson
description: Объяснение того, с использованием проверки подлинности файла cookie без ASP.NET Core Identity
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: f55b36cf3fc3b60e9d592348625f58ebaba90da7
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570117"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="2170f-103">Использовать проверку подлинности файлов cookie без ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="2170f-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="2170f-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Люк Лэтем](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2170f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2170f-105">Как вы уже видели в предыдущих разделах проверки подлинности, [удостоверения ASP.NET Core](xref:security/authentication/identity) — это поставщик проверки подлинности завершено, полнофункциональную для создания и обслуживания имена входа.</span><span class="sxs-lookup"><span data-stu-id="2170f-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="2170f-106">Тем не менее может потребоваться использование собственной логики настраиваемой проверки подлинности на основе файлов cookie проверки подлинности в некоторых случаях.</span><span class="sxs-lookup"><span data-stu-id="2170f-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="2170f-107">Проверка подлинности на основе файлов cookie можно использовать как автономного поставщика проверки подлинности без ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="2170f-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="2170f-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2170f-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2170f-109">Для демонстрационных целей в примере приложения учетной записи пользователя для гипотетической пользователя Родригез Мария встроен в приложение.</span><span class="sxs-lookup"><span data-stu-id="2170f-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="2170f-110">Использовать имя пользователя по электронной почте "maria.rodriguez@contoso.com" и любой пароль для входа пользователя.</span><span class="sxs-lookup"><span data-stu-id="2170f-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="2170f-111">Пользователь проходит проверку подлинности в `AuthenticateUser` метод в *Pages/Account/Login.cshtml.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="2170f-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="2170f-112">В реальный пример пользователь может пройти проверку подлинности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2170f-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="2170f-113">Сведения о миграции файл cookie проверки подлинности с помощью ASP.NET Core 1.x на 2.0, см. в разделе [перенести проверки подлинности и другие удостоверения в разделе ASP.NET Core 2.0 (проверка подлинности на основе файлов Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="2170f-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="2170f-114">Чтобы использовать удостоверение ASP.NET Core, см. в разделе [Общие сведения об Identity](xref:security/authentication/identity) раздела.</span><span class="sxs-lookup"><span data-stu-id="2170f-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="2170f-115">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="2170f-115">Configuration</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2170f-116">Если приложение не использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), создайте ссылку на пакет в файле проекта для [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета (версии 2.1.0 или более поздние).</span><span class="sxs-lookup"><span data-stu-id="2170f-116">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="2170f-117">В `ConfigureServices` метод, создайте службу проверки подлинности по промежуточного слоя с `AddAuthentication` и `AddCookie` методов:</span><span class="sxs-lookup"><span data-stu-id="2170f-117">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="2170f-118">`AuthenticationScheme` передаваемый `AddAuthentication` задает схему проверки подлинности по умолчанию для приложения.</span><span class="sxs-lookup"><span data-stu-id="2170f-118">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="2170f-119">`AuthenticationScheme` полезно, если имеется несколько экземпляров файла cookie проверки подлинности, и вы хотите [авторизация с определенной схемой](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="2170f-119">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="2170f-120">Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы.</span><span class="sxs-lookup"><span data-stu-id="2170f-120">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="2170f-121">Вы можете указать любое строковое значение, являющийся отличительным признаком схемы.</span><span class="sxs-lookup"><span data-stu-id="2170f-121">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="2170f-122">Схема проверки подлинности приложения отличается от схемы проверки подлинности файла cookie для приложения.</span><span class="sxs-lookup"><span data-stu-id="2170f-122">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="2170f-123">Когда схему проверки подлинности файла cookie не предоставляется для <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, он использует [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) («файлы cookie»).</span><span class="sxs-lookup"><span data-stu-id="2170f-123">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies").</span></span>

<span data-ttu-id="2170f-124">В `Configure` используйте `UseAuthentication` метод, вызываемый по промежуточного слоя проверки подлинности, который задает `HttpContext.User` свойства.</span><span class="sxs-lookup"><span data-stu-id="2170f-124">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="2170f-125">Вызовите `UseAuthentication` метод перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="2170f-125">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="2170f-126">**Параметры AddCookie**</span><span class="sxs-lookup"><span data-stu-id="2170f-126">**AddCookie Options**</span></span>

<span data-ttu-id="2170f-127">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) класс используется для настройки параметров поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2170f-127">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="2170f-128">Параметр</span><span class="sxs-lookup"><span data-stu-id="2170f-128">Option</span></span> | <span data-ttu-id="2170f-129">Описание</span><span class="sxs-lookup"><span data-stu-id="2170f-129">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="2170f-130">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="2170f-130">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="2170f-131">Предоставляет путь к предоставить "302 Found" (URL-адрес перенаправления) при активации `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="2170f-131">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="2170f-132">Значение по умолчанию — `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="2170f-132">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="2170f-133">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="2170f-133">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="2170f-134">Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство на все утверждения, созданный с помощью службы проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="2170f-134">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="2170f-135">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="2170f-135">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="2170f-136">Имя домена, где обслуживается куки-файл.</span><span class="sxs-lookup"><span data-stu-id="2170f-136">The domain name where the cookie is served.</span></span> <span data-ttu-id="2170f-137">По умолчанию это имя узла запроса.</span><span class="sxs-lookup"><span data-stu-id="2170f-137">By default, this is the host name of the request.</span></span> <span data-ttu-id="2170f-138">Браузер отправляет только куки-файл в запросах к соответствующим именем узла.</span><span class="sxs-lookup"><span data-stu-id="2170f-138">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="2170f-139">Вы можете настроить этот параметр для файлов cookie, доступных на любом узле в вашем домене.</span><span class="sxs-lookup"><span data-stu-id="2170f-139">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="2170f-140">Например, значение домена файла cookie `.contoso.com` сделает его доступным для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="2170f-140">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="2170f-141">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="2170f-141">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="2170f-142">Возвращает или задает срок действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="2170f-142">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="2170f-143">В настоящее время это параметр холостой командой и станут устаревшими в ASP.NET Core 2.1 или более поздней.</span><span class="sxs-lookup"><span data-stu-id="2170f-143">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="2170f-144">Используйте `ExpireTimeSpan` параметр, чтобы задать срок действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="2170f-144">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="2170f-145">Дополнительные сведения см. в разделе [уточнить поведение CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="2170f-145">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="2170f-146">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="2170f-146">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="2170f-147">Флаг, указывающий файл cookie должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="2170f-147">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="2170f-148">Это значение изменится на `false` разрешает клиентские скрипты, чтобы получить доступ к куки-файл и может открыть свое приложение для хищения файлов cookie, должны иметь приложения [межузловых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость.</span><span class="sxs-lookup"><span data-stu-id="2170f-148">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="2170f-149">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="2170f-149">The default value is `true`.</span></span> |
| [<span data-ttu-id="2170f-150">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="2170f-150">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="2170f-151">Задает имя файла cookie.</span><span class="sxs-lookup"><span data-stu-id="2170f-151">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="2170f-152">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="2170f-152">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="2170f-153">Использовать для изоляции приложений, выполняющихся в то же имя узла.</span><span class="sxs-lookup"><span data-stu-id="2170f-153">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="2170f-154">Если у вас есть приложение, запущенное в `/app1` и нужно ограничить файлы cookie для этого приложения, задайте `CookiePath` свойства `/app1`.</span><span class="sxs-lookup"><span data-stu-id="2170f-154">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="2170f-155">Таким образом, файл cookie доступна только для запросов к `/app1` и любого приложения под ним.</span><span class="sxs-lookup"><span data-stu-id="2170f-155">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="2170f-156">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="2170f-156">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="2170f-157">Указывает, разрешено ли браузер файлов cookie, должны быть присоединены к только запросы веб-сайте (`SameSiteMode.Strict`) или запросов между сайтами, с использованием безопасного HTTP-методов и запросов веб-сайте (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="2170f-157">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="2170f-158">Если задано значение `SameSiteMode.None`, не задано значение заголовка файла cookie.</span><span class="sxs-lookup"><span data-stu-id="2170f-158">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="2170f-159">Обратите внимание, что [промежуточного слоя политики](#cookie-policy-middleware) может перезаписать предоставленное значение.</span><span class="sxs-lookup"><span data-stu-id="2170f-159">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="2170f-160">Для поддержки проверки подлинности OAuth, значение по умолчанию — `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="2170f-160">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="2170f-161">Дополнительные сведения см. в разделе [нарушена из-за политики SameSite файла cookie проверки подлинности OAuth](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="2170f-161">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="2170f-162">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="2170f-162">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="2170f-163">Флаг, указывающий, если созданный файл cookie был ограничен HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и в запросе (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="2170f-163">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="2170f-164">Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="2170f-164">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="2170f-165">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="2170f-165">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="2170f-166">Наборы `DataProtectionProvider` , используемый для создания по умолчанию `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="2170f-166">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="2170f-167">Если `TicketDataFormat` свойство задано, `DataProtectionProvider` параметр не используется.</span><span class="sxs-lookup"><span data-stu-id="2170f-167">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="2170f-168">Если не указано, используется поставщик защиты данных приложения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2170f-168">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="2170f-169">События</span><span class="sxs-lookup"><span data-stu-id="2170f-169">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="2170f-170">Обработчик вызывает методы поставщика, который предоставить элемент управления приложения в определенные моменты обработки.</span><span class="sxs-lookup"><span data-stu-id="2170f-170">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="2170f-171">Если `Events` не переданы, предоставляется экземпляр по умолчанию, не выполняет никаких действий при вызове методов.</span><span class="sxs-lookup"><span data-stu-id="2170f-171">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="2170f-172">EventsType</span><span class="sxs-lookup"><span data-stu-id="2170f-172">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="2170f-173">Используется как тип службы для получения `Events` экземпляром, а не свойства.</span><span class="sxs-lookup"><span data-stu-id="2170f-173">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="2170f-174">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="2170f-174">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="2170f-175">`TimeSpan` После которого срока действия билета проверки подлинности, хранящийся в файл cookie.</span><span class="sxs-lookup"><span data-stu-id="2170f-175">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="2170f-176">`ExpireTimeSpan` добавляется на текущий момент времени, чтобы создать срок действия билета.</span><span class="sxs-lookup"><span data-stu-id="2170f-176">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="2170f-177">`ExpiredTimeSpan` Значение всегда переходит в зашифрованных AuthTicket, проверен сервером.</span><span class="sxs-lookup"><span data-stu-id="2170f-177">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="2170f-178">Он также может перейти в [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) заголовок, но только если `IsPersistent` имеет значение.</span><span class="sxs-lookup"><span data-stu-id="2170f-178">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="2170f-179">Чтобы задать `IsPersistent` для `true`, Настройка [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) передается `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="2170f-179">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="2170f-180">Значение по умолчанию `ExpireTimeSpan` — 14 дней.</span><span class="sxs-lookup"><span data-stu-id="2170f-180">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="2170f-181">LoginPath</span><span class="sxs-lookup"><span data-stu-id="2170f-181">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="2170f-182">Предоставляет путь к предоставить "302 Found" (URL-адрес перенаправления) при активации `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="2170f-182">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="2170f-183">Добавляется текущий URL-адрес, вызвавший ответ 401 `LoginPath` как параметр строки запроса с именем, `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="2170f-183">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="2170f-184">Запрос один раз на `LoginPath` предоставляет новое входа удостоверение, `ReturnUrlParameter` значение используется для перенаправления браузера обратно в URL-адрес, вызвавший исходный код несанкционированного состояния.</span><span class="sxs-lookup"><span data-stu-id="2170f-184">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="2170f-185">Значение по умолчанию — `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="2170f-185">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="2170f-186">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="2170f-186">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="2170f-187">Если `LogoutPath` предоставляется обработчику, а затем перенаправляет запрос по этому пути на основе значения из `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="2170f-187">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="2170f-188">Значение по умолчанию — `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="2170f-188">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="2170f-189">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="2170f-189">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="2170f-190">Определяет имя параметра строки запроса, которое добавляется с помощью обработчика для ответа 302 Found (перенаправление URL-адрес).</span><span class="sxs-lookup"><span data-stu-id="2170f-190">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="2170f-191">`ReturnUrlParameter` используется при получении запроса на `LoginPath` или `LogoutPath` для возврата в браузере на исходный URL-адрес после выполнения действия имени входа или выхода.</span><span class="sxs-lookup"><span data-stu-id="2170f-191">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="2170f-192">Значение по умолчанию — `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="2170f-192">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="2170f-193">SessionStore</span><span class="sxs-lookup"><span data-stu-id="2170f-193">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="2170f-194">Необязательный контейнер, используемый для хранения идентификаторов всех запросов.</span><span class="sxs-lookup"><span data-stu-id="2170f-194">An optional container used to store identity across requests.</span></span> <span data-ttu-id="2170f-195">При использовании только идентификатор сеанса отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="2170f-195">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="2170f-196">`SessionStore` можно использовать для устранения потенциальных проблем с удостоверениями большой.</span><span class="sxs-lookup"><span data-stu-id="2170f-196">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="2170f-197">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="2170f-197">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="2170f-198">Флаг, указывающий, если новый файл cookie с обновленной срок действия должен быть выдан динамически.</span><span class="sxs-lookup"><span data-stu-id="2170f-198">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="2170f-199">Это может произойти на любой запрос, где текущий период истечения срока действия файла cookie более чем на 50% срок действия истек.</span><span class="sxs-lookup"><span data-stu-id="2170f-199">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="2170f-200">Новый срок перемещается вперед быть текущая дата плюс `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="2170f-200">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="2170f-201">[Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` при вызове метода `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="2170f-201">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="2170f-202">Абсолютный срок действия можно повысить безопасность приложения, ограничивая количество времени, который действителен cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2170f-202">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="2170f-203">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="2170f-203">The default value is `true`.</span></span> |
| [<span data-ttu-id="2170f-204">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="2170f-204">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="2170f-205">`TicketDataFormat` Используется для применения и снятия защиты identity и других свойств, которые хранятся в значении cookie.</span><span class="sxs-lookup"><span data-stu-id="2170f-205">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="2170f-206">Если не указано, `TicketDataFormat` создается с помощью [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="2170f-206">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="2170f-207">Проверка</span><span class="sxs-lookup"><span data-stu-id="2170f-207">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="2170f-208">Метод, который проверяет, что параметры являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="2170f-208">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="2170f-209">Задайте `CookieAuthenticationOptions` в конфигурации службы для проверки подлинности в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="2170f-209">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2170f-210">ASP.NET Core 1.x использует cookie [по промежуточного слоя](xref:fundamentals/middleware/index) , сериализует участника-пользователя в зашифрованном файле cookie.</span><span class="sxs-lookup"><span data-stu-id="2170f-210">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="2170f-211">При последующих запросах куки-файл проверяется, а основной сервер заново и назначенные `HttpContext.User` свойство.</span><span class="sxs-lookup"><span data-stu-id="2170f-211">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="2170f-212">Установка [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="2170f-212">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="2170f-213">Этот пакет содержит по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="2170f-213">This package contains the cookie middleware.</span></span>

<span data-ttu-id="2170f-214">Используйте `UseCookieAuthentication` метод в `Configure` метод в вашей *Startup.cs* файл перед `UseMvc` или `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="2170f-214">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="2170f-215">**Параметры CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="2170f-215">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="2170f-216">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) класс используется для настройки параметров поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2170f-216">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="2170f-217">Параметр</span><span class="sxs-lookup"><span data-stu-id="2170f-217">Option</span></span> | <span data-ttu-id="2170f-218">Описание</span><span class="sxs-lookup"><span data-stu-id="2170f-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="2170f-219">Значение AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="2170f-219">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="2170f-220">Задает схему проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2170f-220">Sets the authentication scheme.</span></span> <span data-ttu-id="2170f-221">`AuthenticationScheme` полезно, если имеется несколько экземпляров проверки подлинности, и вы хотите [авторизация с определенной схемой](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="2170f-221">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="2170f-222">Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы.</span><span class="sxs-lookup"><span data-stu-id="2170f-222">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="2170f-223">Вы можете указать любое строковое значение, являющийся отличительным признаком схемы.</span><span class="sxs-lookup"><span data-stu-id="2170f-223">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="2170f-224">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="2170f-224">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="2170f-225">Задает значение, указывающее, что файл cookie проверки подлинности следует выполняется при каждом запросе и пытаются проверить и воссоздания любому участнику сериализованный созданные им.</span><span class="sxs-lookup"><span data-stu-id="2170f-225">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="2170f-226">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="2170f-226">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="2170f-227">Если значение равно true, по промежуточного слоя проверки подлинности обрабатывает автоматического проблем.</span><span class="sxs-lookup"><span data-stu-id="2170f-227">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="2170f-228">Если значение равно false, по промежуточного слоя проверки подлинности только изменяет ответы, если они явно не указано `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="2170f-228">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="2170f-229">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="2170f-229">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="2170f-230">Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство на все утверждения, созданные по промежуточного слоя проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2170f-230">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="2170f-231">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="2170f-231">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="2170f-232">Имя домена, где обслуживается куки-файл.</span><span class="sxs-lookup"><span data-stu-id="2170f-232">The domain name where the cookie is served.</span></span> <span data-ttu-id="2170f-233">По умолчанию это имя узла запроса.</span><span class="sxs-lookup"><span data-stu-id="2170f-233">By default, this is the host name of the request.</span></span> <span data-ttu-id="2170f-234">Браузер обслуживает только файл cookie для сопоставления имени узла.</span><span class="sxs-lookup"><span data-stu-id="2170f-234">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="2170f-235">Вы можете настроить этот параметр для файлов cookie, доступных на любом узле в вашем домене.</span><span class="sxs-lookup"><span data-stu-id="2170f-235">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="2170f-236">Например, значение домена файла cookie `.contoso.com` сделает его доступным для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="2170f-236">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="2170f-237">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="2170f-237">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="2170f-238">Флаг, указывающий файл cookie должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="2170f-238">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="2170f-239">Это значение изменится на `false` разрешает клиентские скрипты, чтобы получить доступ к куки-файл и может открыть свое приложение для хищения файлов cookie, должны иметь приложения [межузловых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость.</span><span class="sxs-lookup"><span data-stu-id="2170f-239">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="2170f-240">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="2170f-240">The default value is `true`.</span></span> |
| [<span data-ttu-id="2170f-241">CookiePath</span><span class="sxs-lookup"><span data-stu-id="2170f-241">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="2170f-242">Использовать для изоляции приложений, выполняющихся в то же имя узла.</span><span class="sxs-lookup"><span data-stu-id="2170f-242">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="2170f-243">Если у вас есть приложение, запущенное в `/app1` и нужно ограничить файлы cookie для этого приложения, задайте `CookiePath` свойства `/app1`.</span><span class="sxs-lookup"><span data-stu-id="2170f-243">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="2170f-244">Таким образом, файл cookie доступна только для запросов к `/app1` и любого приложения под ним.</span><span class="sxs-lookup"><span data-stu-id="2170f-244">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="2170f-245">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="2170f-245">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="2170f-246">Флаг, указывающий, если созданный файл cookie был ограничен HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и в запросе (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="2170f-246">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="2170f-247">Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="2170f-247">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="2170f-248">Описание</span><span class="sxs-lookup"><span data-stu-id="2170f-248">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="2170f-249">Дополнительные сведения о типе проверки подлинности, который становится доступен в приложение.</span><span class="sxs-lookup"><span data-stu-id="2170f-249">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="2170f-250">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="2170f-250">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="2170f-251">`TimeSpan` После которого срока действия билета проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2170f-251">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="2170f-252">Оно добавляется на текущий момент времени, чтобы создать срок действия билета.</span><span class="sxs-lookup"><span data-stu-id="2170f-252">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="2170f-253">Чтобы использовать `ExpireTimeSpan`, необходимо задать `IsPersistent` для `true` в `AuthenticationProperties` передается `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="2170f-253">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="2170f-254">Значение по умолчанию — 14 дней.</span><span class="sxs-lookup"><span data-stu-id="2170f-254">The default value is 14 days.</span></span> |
| [<span data-ttu-id="2170f-255">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="2170f-255">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="2170f-256">Флаг, указывающий, сбрасывается ли дату истечения срока действия файла cookie при более половины `ExpireTimeSpan` промежуток времени истек.</span><span class="sxs-lookup"><span data-stu-id="2170f-256">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="2170f-257">Новое время exipiration перемещается вперед быть текущая дата плюс `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="2170f-257">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="2170f-258">[Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` при вызове метода `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="2170f-258">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="2170f-259">Абсолютный срок действия можно повысить безопасность приложения, ограничивая количество времени, который действителен cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2170f-259">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="2170f-260">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="2170f-260">The default value is `true`.</span></span> |

<span data-ttu-id="2170f-261">Задайте `CookieAuthenticationOptions` для по промежуточного слоя проверки подлинности, в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="2170f-261">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a><span data-ttu-id="2170f-262">По промежуточного слоя для файлов cookie политики</span><span class="sxs-lookup"><span data-stu-id="2170f-262">Cookie Policy Middleware</span></span>

<span data-ttu-id="2170f-263">[По промежуточного слоя для файлов cookie политики](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) обеспечивает возможности политики файлов cookie в приложении.</span><span class="sxs-lookup"><span data-stu-id="2170f-263">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="2170f-264">Добавлением по промежуточного слоя в конвейере обработки приложение является конфиденциальным; порядок он влияет только на компоненты, зарегистрированные за ним в конвейере.</span><span class="sxs-lookup"><span data-stu-id="2170f-264">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="2170f-265">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) для по промежуточного слоя политики позволяют управлять общих характеристик обработки файлов cookie и обработчик в обработчики обработки файлов cookie, если файлы cookie добавляется или удаляется.</span><span class="sxs-lookup"><span data-stu-id="2170f-265">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="2170f-266">Свойство.</span><span class="sxs-lookup"><span data-stu-id="2170f-266">Property</span></span> | <span data-ttu-id="2170f-267">Описание</span><span class="sxs-lookup"><span data-stu-id="2170f-267">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="2170f-268">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="2170f-268">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="2170f-269">Влияет ли файлы cookie должен быть HttpOnly, который является флагом, показывающим куки-файл должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="2170f-269">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="2170f-270">Значение по умолчанию — `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="2170f-270">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="2170f-271">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="2170f-271">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="2170f-272">Влияет на атрибут файла cookie веб-сайте (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="2170f-272">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="2170f-273">Значение по умолчанию — `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="2170f-273">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="2170f-274">Этот параметр доступен для ASP.NET Core 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="2170f-274">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="2170f-275">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="2170f-275">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="2170f-276">Вызывается, когда файл cookie добавляется.</span><span class="sxs-lookup"><span data-stu-id="2170f-276">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="2170f-277">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="2170f-277">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="2170f-278">Вызывается при удалении файла cookie.</span><span class="sxs-lookup"><span data-stu-id="2170f-278">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="2170f-279">Защита</span><span class="sxs-lookup"><span data-stu-id="2170f-279">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="2170f-280">Влияет ли файлы cookie должен быть Secure.</span><span class="sxs-lookup"><span data-stu-id="2170f-280">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="2170f-281">Значение по умолчанию — `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="2170f-281">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="2170f-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + только)</span><span class="sxs-lookup"><span data-stu-id="2170f-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="2170f-283">Значение по умолчанию `MinimumSameSitePolicy` значение `SameSiteMode.Lax` для разрешения проверки подлинности OAuth2.</span><span class="sxs-lookup"><span data-stu-id="2170f-283">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="2170f-284">Для строго ограничивает политику веб-сайте `SameSiteMode.Strict`, задайте `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="2170f-284">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="2170f-285">Несмотря на то, что этот параметр приводит к сбою OAuth2 и других схем проверки подлинности независимо от источника, оно повышает уровень безопасности cookie для других типов приложений, которые не полагаются на обработку независимо от источника запроса.</span><span class="sxs-lookup"><span data-stu-id="2170f-285">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="2170f-286">Параметр политики промежуточного слоя для `MinimumSameSitePolicy` может повлиять на ваши равным `Cookie.SameSite` в `CookieAuthenticationOptions` параметры в соответствии с в таблице ниже.</span><span class="sxs-lookup"><span data-stu-id="2170f-286">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="2170f-287">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="2170f-287">MinimumSameSitePolicy</span></span> | <span data-ttu-id="2170f-288">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="2170f-288">Cookie.SameSite</span></span> | <span data-ttu-id="2170f-289">Результирующий параметр Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="2170f-289">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="2170f-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="2170f-290">SameSiteMode.None</span></span>     | <span data-ttu-id="2170f-291">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="2170f-291">SameSiteMode.None</span></span><br><span data-ttu-id="2170f-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2170f-292">SameSiteMode.Lax</span></span><br><span data-ttu-id="2170f-293">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2170f-293">SameSiteMode.Strict</span></span> | <span data-ttu-id="2170f-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="2170f-294">SameSiteMode.None</span></span><br><span data-ttu-id="2170f-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2170f-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="2170f-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2170f-296">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="2170f-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2170f-297">SameSiteMode.Lax</span></span>      | <span data-ttu-id="2170f-298">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="2170f-298">SameSiteMode.None</span></span><br><span data-ttu-id="2170f-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2170f-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="2170f-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2170f-300">SameSiteMode.Strict</span></span> | <span data-ttu-id="2170f-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2170f-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="2170f-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2170f-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="2170f-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2170f-303">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="2170f-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2170f-304">SameSiteMode.Strict</span></span>   | <span data-ttu-id="2170f-305">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="2170f-305">SameSiteMode.None</span></span><br><span data-ttu-id="2170f-306">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2170f-306">SameSiteMode.Lax</span></span><br><span data-ttu-id="2170f-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2170f-307">SameSiteMode.Strict</span></span> | <span data-ttu-id="2170f-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2170f-308">SameSiteMode.Strict</span></span><br><span data-ttu-id="2170f-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2170f-309">SameSiteMode.Strict</span></span><br><span data-ttu-id="2170f-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2170f-310">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="2170f-311">Создайте файл cookie проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="2170f-311">Create an authentication cookie</span></span>

<span data-ttu-id="2170f-312">Чтобы создать файл cookie, содержащий сведения о пользователе, то необходимо создать [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="2170f-312">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="2170f-313">Сведения о пользователе сериализуется и сохраняется в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="2170f-313">The user information is serialized and stored in the cookie.</span></span> 

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2170f-314">Создание [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) с любыми обязательными [утверждения](/dotnet/api/system.security.claims.claim)s и вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) для входа пользователя:</span><span class="sxs-lookup"><span data-stu-id="2170f-314">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2170f-315">Вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) для входа пользователя:</span><span class="sxs-lookup"><span data-stu-id="2170f-315">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

<span data-ttu-id="2170f-316">`SignInAsync` создает зашифрованный файл cookie и добавляет его в текущий ответ.</span><span class="sxs-lookup"><span data-stu-id="2170f-316">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="2170f-317">Если вы не укажете `AuthenticationScheme`, используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2170f-317">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="2170f-318">На самом деле, шифрования, используемых — ASP.NET Core [защиты данных](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) системы.</span><span class="sxs-lookup"><span data-stu-id="2170f-318">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="2170f-319">Если вы размещаете приложения на нескольких компьютерах, балансировки нагрузки для приложений или с помощью веб-ферме, то необходимо [настроить защиту данных](xref:security/data-protection/configuration/overview) использование одного набора ключей и идентификатор приложения.</span><span class="sxs-lookup"><span data-stu-id="2170f-319">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="2170f-320">Выйти</span><span class="sxs-lookup"><span data-stu-id="2170f-320">Sign out</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2170f-321">Чтобы выйти из системы текущего пользователя и удалить их файл cookie, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="2170f-321">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2170f-322">Чтобы выйти из системы текущего пользователя и удалить их файл cookie, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="2170f-322">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

<span data-ttu-id="2170f-323">Если вы не используете `CookieAuthenticationDefaults.AuthenticationScheme` (или файлы «cookie») как схему (например, «ContosoCookie»), укажите схему, используемую при настройке поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2170f-323">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="2170f-324">В противном случае используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2170f-324">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="2170f-325">Реагировать на изменения серверной части</span><span class="sxs-lookup"><span data-stu-id="2170f-325">React to back-end changes</span></span>

<span data-ttu-id="2170f-326">Создав файл cookie, он становится единственным источником удостоверения.</span><span class="sxs-lookup"><span data-stu-id="2170f-326">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="2170f-327">Даже при отключении пользователя в системах серверной системой проверки подлинности файла cookie не имеет сведений о это и пользователь остается в системе до тех пор, пока их файл cookie является допустимым.</span><span class="sxs-lookup"><span data-stu-id="2170f-327">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="2170f-328">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) событий в ASP.NET Core 2.x или [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) метод в ASP.NET Core 1.x можно использовать для перехвата и переопределение проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="2170f-328">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="2170f-329">Этот подход уменьшает риск появления отозванных пользователей, обращающихся к приложению.</span><span class="sxs-lookup"><span data-stu-id="2170f-329">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="2170f-330">Один из способов проверки cookie основан на слежении за при изменении пользователя базы данных.</span><span class="sxs-lookup"><span data-stu-id="2170f-330">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="2170f-331">Если базы данных не был изменен с момента выпуска файла cookie пользователя, нет необходимости пройти повторную аутентификацию пользователя, если их куки-файл все еще действителен.</span><span class="sxs-lookup"><span data-stu-id="2170f-331">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="2170f-332">Чтобы реализовать этот сценарий базы данных, реализованное в `IUserRepository` в этом примере хранит `LastChanged` значение.</span><span class="sxs-lookup"><span data-stu-id="2170f-332">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="2170f-333">При обновлении любого пользователя в базе данных, `LastChanged` имеет значение текущего времени.</span><span class="sxs-lookup"><span data-stu-id="2170f-333">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="2170f-334">Чтобы сделать недействительным файл cookie, когда база данных изменяется в зависимости от `LastChanged` создайте файл cookie с `LastChanged` утверждение, содержащее текущий `LastChanged` значение из базы данных:</span><span class="sxs-lookup"><span data-stu-id="2170f-334">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="2170f-335">Для реализации переопределения для `ValidatePrincipal` события, написание метод со следующей сигнатурой в классе, унаследованном от [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="2170f-335">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="2170f-336">Пример выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2170f-336">An example looks like the following:</span></span>

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

<span data-ttu-id="2170f-337">Зарегистрируйте экземпляр события во время регистрации службы файл cookie в `ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="2170f-337">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="2170f-338">Укажите регистрацию ограниченной службы для вашего `CustomCookieAuthenticationEvents` класса:</span><span class="sxs-lookup"><span data-stu-id="2170f-338">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

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

<span data-ttu-id="2170f-339">Для реализации переопределения для `ValidateAsync` события, написание метод со следующей сигнатурой:</span><span class="sxs-lookup"><span data-stu-id="2170f-339">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="2170f-340">Удостоверение ASP.NET Core реализует эту проверку в процессе его [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="2170f-340">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="2170f-341">Пример выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2170f-341">An example looks like the following:</span></span>

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

<span data-ttu-id="2170f-342">Регистрировать событие во время настройки файла cookie проверки подлинности в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="2170f-342">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

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

<span data-ttu-id="2170f-343">Рассмотрим ситуацию, в котором имя пользователя будет обновлено &mdash; решение, которое не влияет на безопасность любым способом.</span><span class="sxs-lookup"><span data-stu-id="2170f-343">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="2170f-344">Если вы хотите безболезненно обновить участника-пользователя, вызовите `context.ReplacePrincipal` и задайте `context.ShouldRenew` свойства `true`.</span><span class="sxs-lookup"><span data-stu-id="2170f-344">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="2170f-345">Подход, описанный здесь активируется при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="2170f-345">The approach described here is triggered on every request.</span></span> <span data-ttu-id="2170f-346">Это может привести к снижению производительности для приложения.</span><span class="sxs-lookup"><span data-stu-id="2170f-346">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="2170f-347">Постоянные файлы cookie</span><span class="sxs-lookup"><span data-stu-id="2170f-347">Persistent cookies</span></span>

<span data-ttu-id="2170f-348">Требуется файл cookie для сохранения между сеансами браузера.</span><span class="sxs-lookup"><span data-stu-id="2170f-348">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="2170f-349">Такое сохранение должны включаться только с согласия пользователя с помощью флажка «Запомнить мои» на имя входа или похожий механизм.</span><span class="sxs-lookup"><span data-stu-id="2170f-349">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on login or a similar mechanism.</span></span> 

<span data-ttu-id="2170f-350">Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, который выдерживает его через браузер замыкания.</span><span class="sxs-lookup"><span data-stu-id="2170f-350">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="2170f-351">Учитываются параметры скользящего срока действия ранее была настроена.</span><span class="sxs-lookup"><span data-stu-id="2170f-351">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="2170f-352">Если срок действия файла cookie, при закрытии браузера, браузер удаляет файл cookie после перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="2170f-352">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

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

<span data-ttu-id="2170f-353">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) класс находится в `Microsoft.AspNetCore.Authentication` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="2170f-353">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

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

<span data-ttu-id="2170f-354">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) класс находится в `Microsoft.AspNetCore.Http.Authentication` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="2170f-354">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

::: moniker-end

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="2170f-355">Срок действия файла cookie абсолютный</span><span class="sxs-lookup"><span data-stu-id="2170f-355">Absolute cookie expiration</span></span>

<span data-ttu-id="2170f-356">Можно задать абсолютный срок действия с `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="2170f-356">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="2170f-357">Необходимо также задать `IsPersistent`; в противном случае `ExpiresUtc` учитывается и создается файл cookie одного сеанса.</span><span class="sxs-lookup"><span data-stu-id="2170f-357">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="2170f-358">Когда `ExpiresUtc` устанавливается на `SignInAsync`, этот параметр переопределяет значение `ExpireTimeSpan` параметр `CookieAuthenticationOptions`, если задать.</span><span class="sxs-lookup"><span data-stu-id="2170f-358">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="2170f-359">Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, который выполняется в течение 20 минут.</span><span class="sxs-lookup"><span data-stu-id="2170f-359">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="2170f-360">Это не учитывает параметры скользящего срока действия ранее была настроена.</span><span class="sxs-lookup"><span data-stu-id="2170f-360">This ignores any sliding expiration settings previously configured.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="2170f-361">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2170f-361">Additional resources</span></span>

* [<span data-ttu-id="2170f-362">Изменения AUTH 2.0 / объявления миграции</span><span class="sxs-lookup"><span data-stu-id="2170f-362">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="2170f-363">Проверок роли на основе политик</span><span class="sxs-lookup"><span data-stu-id="2170f-363">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
