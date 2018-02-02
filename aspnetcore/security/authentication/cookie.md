---
title: "С помощью файла Cookie проверки подлинности без удостоверения ASP.NET Core"
author: rick-anderson
description: "Описание использования файлов cookie проверки подлинности без ASP.NET Core Identity"
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: 1f20a2f7cab123e5a41ee1d232271da9de4c9058
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="f688e-103">С помощью файла Cookie проверки подлинности без удостоверения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f688e-103">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="f688e-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Latham Люк](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f688e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f688e-105">Как видно в предыдущих разделах проверки подлинности, [ASP.NET Core Identity](xref:security/authentication/identity) является полным, полнофункциональный проверки подлинности поставщиком для создания и обслуживания имена входа.</span><span class="sxs-lookup"><span data-stu-id="f688e-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="f688e-106">Тем не менее можно использовать свою собственную логику для нестандартной проверки подлинности с помощью файла cookie проверки подлинности на основе время от времени.</span><span class="sxs-lookup"><span data-stu-id="f688e-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="f688e-107">Как поставщик проверки подлинности автономного без ASP.NET Core Identity можно использовать проверку подлинности на основе файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="f688e-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f688e-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f688e-109">Сведения о миграции проверки подлинности на основе файла cookie из ASP.NET Core 1.x 2.0, см. [миграции проверку подлинности и удостоверение для ASP.NET 2.0 основных раздела (файл Cookie проверки подлинности на основе)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="f688e-109">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrating Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

## <a name="configuration"></a><span data-ttu-id="f688e-110">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="f688e-110">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f688e-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f688e-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f688e-112">Если вы не используете [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), установите версию 2.0 + [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="f688e-112">If you aren't using the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), install version 2.0+ of the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package.</span></span>

<span data-ttu-id="f688e-113">В `ConfigureServices` метод, создайте службу проверки подлинности по промежуточного слоя с `AddAuthentication` и `AddCookie` методов:</span><span class="sxs-lookup"><span data-stu-id="f688e-113">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="f688e-114">`AuthenticationScheme`передаваемый `AddAuthentication` задает схему проверки подлинности по умолчанию для приложения.</span><span class="sxs-lookup"><span data-stu-id="f688e-114">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="f688e-115">`AuthenticationScheme`полезно при наличии нескольких экземпляров файла cookie проверки подлинности, и вы хотите [авторизации с нужной раскладки](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="f688e-115">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="f688e-116">Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы.</span><span class="sxs-lookup"><span data-stu-id="f688e-116">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="f688e-117">Можно указать любое строковое значение, отличающее схему.</span><span class="sxs-lookup"><span data-stu-id="f688e-117">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="f688e-118">В `Configure` используйте `UseAuthentication` метод, вызываемый по промежуточного слоя проверки подлинности, задает `HttpContext.User` свойства.</span><span class="sxs-lookup"><span data-stu-id="f688e-118">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="f688e-119">Вызовите `UseAuthentication` метод перед вызовом метода `UseMvcWithDefaultRoute` или `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="f688e-119">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="f688e-120">**Параметры AddCookie**</span><span class="sxs-lookup"><span data-stu-id="f688e-120">**AddCookie Options**</span></span>

<span data-ttu-id="f688e-121">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) класс используется для настройки параметров поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f688e-121">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="f688e-122">Параметр</span><span class="sxs-lookup"><span data-stu-id="f688e-122">Option</span></span> | <span data-ttu-id="f688e-123">Описание:</span><span class="sxs-lookup"><span data-stu-id="f688e-123">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="f688e-124">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="f688e-124">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="f688e-125">Предоставляет путь для предоставления с 302 Found (перенаправление URL-адрес) при срабатывании триггера `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="f688e-125">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="f688e-126">Значение по умолчанию — `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="f688e-126">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="f688e-127">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="f688e-127">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="f688e-128">Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство утверждений, созданный с помощью службы проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-128">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="f688e-129">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="f688e-129">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="f688e-130">Имя домена, где обслуживают куки-файл.</span><span class="sxs-lookup"><span data-stu-id="f688e-130">The domain name where the cookie is served.</span></span> <span data-ttu-id="f688e-131">По умолчанию это имя узла запроса.</span><span class="sxs-lookup"><span data-stu-id="f688e-131">By default, this is the host name of the request.</span></span> <span data-ttu-id="f688e-132">Браузер отправляет только куки-файл в запросах имени узла.</span><span class="sxs-lookup"><span data-stu-id="f688e-132">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="f688e-133">Вы можете настроить этот параметр для файлов cookie, доступных любому узлу в вашем домене.</span><span class="sxs-lookup"><span data-stu-id="f688e-133">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="f688e-134">Например, установите значение домена файла cookie `.contoso.com` делает ее доступной для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="f688e-134">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="f688e-135">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="f688e-135">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="f688e-136">Возвращает или задает время существования куки-файла.</span><span class="sxs-lookup"><span data-stu-id="f688e-136">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="f688e-137">В настоящее время это параметр Нет ops и станет устаревшим в ASP.NET Core 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="f688e-137">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="f688e-138">Используйте `ExpireTimeSpan` параметр, чтобы задать срок действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-138">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="f688e-139">Дополнительные сведения см. в разделе [уточняется поведение CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="f688e-139">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="f688e-140">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="f688e-140">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="f688e-141">Флаг, указывающий куки-файл должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="f688e-141">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="f688e-142">Изменение этого значения для `false` разрешает клиентские скрипты для доступа к куки-файл и может открыть приложение кража файлов cookie, должны иметь приложения [межсайтовых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость.</span><span class="sxs-lookup"><span data-stu-id="f688e-142">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="f688e-143">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="f688e-143">The default value is `true`.</span></span> |
| [<span data-ttu-id="f688e-144">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="f688e-144">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="f688e-145">Задает имя файла cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-145">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="f688e-146">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="f688e-146">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="f688e-147">Использовать для изоляции приложений, выполняющихся в то же имя узла.</span><span class="sxs-lookup"><span data-stu-id="f688e-147">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="f688e-148">Если у вас есть приложение, работающее на `/app1` и нужно ограничить файлы cookie для данного приложения, задайте `CookiePath` свойства `/app1`.</span><span class="sxs-lookup"><span data-stu-id="f688e-148">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="f688e-149">Таким образом, файл cookie доступна только для запросов к `/app1` и любое приложение, входящие в него.</span><span class="sxs-lookup"><span data-stu-id="f688e-149">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="f688e-150">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="f688e-150">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="f688e-151">Указывает, является ли браузер должен позволять куки-файл для подключения к веб-сайте запросы только (`SameSiteMode.Strict`) или запросы между сайтами, с использованием безопасного HTTP методы и запросы веб-сайте (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="f688e-151">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="f688e-152">Если задано значение `SameSiteMode.None`, не задано значение заголовка файла cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-152">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="f688e-153">Обратите внимание, что [по промежуточного слоя файлов Cookie политики](#cookie-policy-middleware) может перезаписать предоставленное значение.</span><span class="sxs-lookup"><span data-stu-id="f688e-153">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="f688e-154">Для поддержки проверки подлинности OAuth, значение по умолчанию — `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="f688e-154">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="f688e-155">Дополнительные сведения см. в разделе [потеряны из-за политики SameSite файла cookie проверки подлинности OAuth](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="f688e-155">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="f688e-156">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="f688e-156">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="f688e-157">Флаг, указывающий, следует ли созданный файл cookie ограничиваются HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и при запросе (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="f688e-157">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="f688e-158">Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="f688e-158">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="f688e-159">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="f688e-159">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="f688e-160">Наборы `DataProtectionProvider` , используемый для создания по умолчанию `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="f688e-160">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="f688e-161">Если `TicketDataFormat` свойство задано, `DataProtectionProvider` параметр не используется.</span><span class="sxs-lookup"><span data-stu-id="f688e-161">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="f688e-162">Если не указано, используется поставщик защиты данных приложения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f688e-162">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="f688e-163">События</span><span class="sxs-lookup"><span data-stu-id="f688e-163">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="f688e-164">Обработчик вызывает методы поставщика, позволяют контролировать приложения в определенные моменты обработки.</span><span class="sxs-lookup"><span data-stu-id="f688e-164">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="f688e-165">Если `Events` не указан, предоставляется экземпляр по умолчанию, не выполняет никаких действий, при вызове методов.</span><span class="sxs-lookup"><span data-stu-id="f688e-165">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="f688e-166">EventsType</span><span class="sxs-lookup"><span data-stu-id="f688e-166">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="f688e-167">Используется как тип службы для получения `Events` экземпляром, а не свойства.</span><span class="sxs-lookup"><span data-stu-id="f688e-167">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="f688e-168">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="f688e-168">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="f688e-169">`TimeSpan` После которого срок действия билета проверки подлинности, хранящийся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-169">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="f688e-170">`ExpireTimeSpan`добавляется на текущий момент времени, чтобы создать срок действия билета.</span><span class="sxs-lookup"><span data-stu-id="f688e-170">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="f688e-171">`ExpiredTimeSpan` Значение всегда переходит в зашифрованных AuthTicket, проверить сервер.</span><span class="sxs-lookup"><span data-stu-id="f688e-171">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="f688e-172">Он также может перейти в [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) заголовок, но только если `IsPersistent` имеет значение.</span><span class="sxs-lookup"><span data-stu-id="f688e-172">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="f688e-173">Чтобы задать `IsPersistent` для `true`, настройте [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) передаваемый `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="f688e-173">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="f688e-174">Значение по умолчанию `ExpireTimeSpan` — 14 дней.</span><span class="sxs-lookup"><span data-stu-id="f688e-174">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="f688e-175">LoginPath</span><span class="sxs-lookup"><span data-stu-id="f688e-175">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="f688e-176">Предоставляет путь для предоставления с 302 Found (перенаправление URL-адрес) при срабатывании триггера `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="f688e-176">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="f688e-177">Текущий URL-адрес, создавший код 401, добавляется в `LoginPath` как параметр строки запроса с именем `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="f688e-177">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="f688e-178">Запрос один раз на `LoginPath` предоставляет нового вход удостоверения, `ReturnUrlParameter` значение используется для перенаправления браузера обратно в URL-адрес, вызвавший исходный код состояния не санкционировано.</span><span class="sxs-lookup"><span data-stu-id="f688e-178">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="f688e-179">Значение по умолчанию — `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="f688e-179">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="f688e-180">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="f688e-180">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="f688e-181">Если `LogoutPath` предоставляется обработчику, а затем перенаправляет запрос по этому пути в зависимости от значения `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="f688e-181">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="f688e-182">Значение по умолчанию — `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="f688e-182">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="f688e-183">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="f688e-183">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="f688e-184">Определяет имя параметра строки запроса, добавленный для обработчика ответа 302 Found (перенаправление URL-адрес).</span><span class="sxs-lookup"><span data-stu-id="f688e-184">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="f688e-185">`ReturnUrlParameter`используется при получении запроса на `LoginPath` или `LogoutPath` для возврата в браузере исходный URL-адрес после выполнения действия имени входа или выхода из системы.</span><span class="sxs-lookup"><span data-stu-id="f688e-185">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="f688e-186">Значение по умолчанию — `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="f688e-186">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="f688e-187">SessionStore</span><span class="sxs-lookup"><span data-stu-id="f688e-187">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="f688e-188">Необязательный контейнер, используемый для хранения идентификаторов различных запросов.</span><span class="sxs-lookup"><span data-stu-id="f688e-188">An optional container used to store identity across requests.</span></span> <span data-ttu-id="f688e-189">При использовании только идентификатор сеанса отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="f688e-189">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="f688e-190">`SessionStore`можно использовать для устранения возможных проблем с большой удостоверения.</span><span class="sxs-lookup"><span data-stu-id="f688e-190">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="f688e-191">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="f688e-191">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="f688e-192">Флаг, указывающий, должен быть новый файл cookie, срок действия обновленного выдан динамически.</span><span class="sxs-lookup"><span data-stu-id="f688e-192">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="f688e-193">Это может произойти на любой запрос, где текущего срока действия файла cookie более чем на 50% срок действия истек.</span><span class="sxs-lookup"><span data-stu-id="f688e-193">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="f688e-194">Новую дату окончания срока действия перемещается вперед для текущей даты, а также `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="f688e-194">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="f688e-195">[Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` класса при вызове `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="f688e-195">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="f688e-196">Абсолютный срок действия может повысить безопасность приложения, ограничивая количество времени, допустимый файл cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f688e-196">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="f688e-197">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="f688e-197">The default value is `true`.</span></span> |
| [<span data-ttu-id="f688e-198">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="f688e-198">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="f688e-199">`TicketDataFormat` Используется для установки и снятия защиты identity и других свойств, хранящихся в значении cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-199">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="f688e-200">Если не указано, `TicketDataFormat` создается с помощью [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="f688e-200">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="f688e-201">Validate</span><span class="sxs-lookup"><span data-stu-id="f688e-201">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="f688e-202">Метод, который проверяет, что параметры являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="f688e-202">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="f688e-203">Задать `CookieAuthenticationOptions` в конфигурации службы для проверки подлинности в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="f688e-203">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f688e-204">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f688e-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f688e-205">Куки-файл использует 1.x ASP.NET Core [по промежуточного слоя](xref:fundamentals/middleware/index) , сериализует участника-пользователя в зашифрованном файле cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-205">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="f688e-206">При последующих запросах куки-файл проверяется, и основной сервер заново и назначенные `HttpContext.User` свойство.</span><span class="sxs-lookup"><span data-stu-id="f688e-206">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="f688e-207">Установка [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета NuGet в проекте.</span><span class="sxs-lookup"><span data-stu-id="f688e-207">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="f688e-208">Этот пакет содержит файл cookie по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="f688e-208">This package contains the cookie middleware.</span></span>

<span data-ttu-id="f688e-209">Используйте `UseCookieAuthentication` метод в `Configure` метод в вашей *файла Startup.cs* файла перед `UseMvc` или `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="f688e-209">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="f688e-210">**Параметры CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="f688e-210">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="f688e-211">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) класс используется для настройки параметров поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f688e-211">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="f688e-212">Параметр</span><span class="sxs-lookup"><span data-stu-id="f688e-212">Option</span></span> | <span data-ttu-id="f688e-213">Описание:</span><span class="sxs-lookup"><span data-stu-id="f688e-213">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="f688e-214">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="f688e-214">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="f688e-215">Задает схему проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f688e-215">Sets the authentication scheme.</span></span> <span data-ttu-id="f688e-216">`AuthenticationScheme`полезно, когда несколько экземпляров проверки подлинности, и вы хотите [авторизации с нужной раскладки](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="f688e-216">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="f688e-217">Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы.</span><span class="sxs-lookup"><span data-stu-id="f688e-217">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="f688e-218">Можно указать любое строковое значение, отличающее схему.</span><span class="sxs-lookup"><span data-stu-id="f688e-218">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="f688e-219">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="f688e-219">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="f688e-220">Задает значение, указывающее, что файл cookie проверки подлинности запустите при каждом запросе и попытка проверки и воссоздания любому участнику сериализованный она создана.</span><span class="sxs-lookup"><span data-stu-id="f688e-220">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="f688e-221">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="f688e-221">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="f688e-222">Значение true, если по промежуточного слоя проверки подлинности обрабатывает автоматического проблем.</span><span class="sxs-lookup"><span data-stu-id="f688e-222">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="f688e-223">Если значение равно false, проверка подлинности по промежуточного слоя лишь изменяет ответы, если они явным образом указано `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="f688e-223">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="f688e-224">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="f688e-224">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="f688e-225">Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство все утверждения, созданные по промежуточного слоя проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-225">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="f688e-226">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="f688e-226">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="f688e-227">Имя домена, где обслуживают куки-файл.</span><span class="sxs-lookup"><span data-stu-id="f688e-227">The domain name where the cookie is served.</span></span> <span data-ttu-id="f688e-228">По умолчанию это имя узла запроса.</span><span class="sxs-lookup"><span data-stu-id="f688e-228">By default, this is the host name of the request.</span></span> <span data-ttu-id="f688e-229">Браузер служит только файла cookie для сопоставления имени узла.</span><span class="sxs-lookup"><span data-stu-id="f688e-229">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="f688e-230">Вы можете настроить этот параметр для файлов cookie, доступных любому узлу в вашем домене.</span><span class="sxs-lookup"><span data-stu-id="f688e-230">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="f688e-231">Например, установите значение домена файла cookie `.contoso.com` делает ее доступной для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="f688e-231">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="f688e-232">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="f688e-232">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="f688e-233">Флаг, указывающий куки-файл должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="f688e-233">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="f688e-234">Изменение этого значения для `false` разрешает клиентские скрипты для доступа к куки-файл и может открыть приложение кража файлов cookie, должны иметь приложения [межсайтовых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость.</span><span class="sxs-lookup"><span data-stu-id="f688e-234">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="f688e-235">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="f688e-235">The default value is `true`.</span></span> |
| [<span data-ttu-id="f688e-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="f688e-236">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="f688e-237">Использовать для изоляции приложений, выполняющихся в то же имя узла.</span><span class="sxs-lookup"><span data-stu-id="f688e-237">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="f688e-238">Если у вас есть приложение, работающее на `/app1` и нужно ограничить файлы cookie для данного приложения, задайте `CookiePath` свойства `/app1`.</span><span class="sxs-lookup"><span data-stu-id="f688e-238">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="f688e-239">Таким образом, файл cookie доступна только для запросов к `/app1` и любое приложение, входящие в него.</span><span class="sxs-lookup"><span data-stu-id="f688e-239">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="f688e-240">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="f688e-240">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="f688e-241">Флаг, указывающий, следует ли созданный файл cookie ограничиваются HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и при запросе (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="f688e-241">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="f688e-242">Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="f688e-242">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="f688e-243">Описание</span><span class="sxs-lookup"><span data-stu-id="f688e-243">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="f688e-244">Дополнительные сведения о типе проверки подлинности доступны для приложения.</span><span class="sxs-lookup"><span data-stu-id="f688e-244">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="f688e-245">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="f688e-245">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="f688e-246">`TimeSpan` После которого срок действия билета проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f688e-246">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="f688e-247">Он добавляется в текущее время, чтобы создать срок действия билета.</span><span class="sxs-lookup"><span data-stu-id="f688e-247">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="f688e-248">Для использования `ExpireTimeSpan`, необходимо задать `IsPersistent` для `true` в `AuthenticationProperties` передаваемый `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="f688e-248">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="f688e-249">Значение по умолчанию — 14 дней.</span><span class="sxs-lookup"><span data-stu-id="f688e-249">The default value is 14 days.</span></span> |
| [<span data-ttu-id="f688e-250">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="f688e-250">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="f688e-251">Флаг, указывающий, сбрасывается ли дата окончания срока действия файла cookie при более половины `ExpireTimeSpan` истечет интервал.</span><span class="sxs-lookup"><span data-stu-id="f688e-251">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="f688e-252">Новое время exipiration перемещается вперед для текущей даты, а также `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="f688e-252">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="f688e-253">[Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` класса при вызове `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="f688e-253">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="f688e-254">Абсолютный срок действия может повысить безопасность приложения, ограничивая количество времени, допустимый файл cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f688e-254">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="f688e-255">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="f688e-255">The default value is `true`.</span></span> |

<span data-ttu-id="f688e-256">Задать `CookieAuthenticationOptions` по промежуточного слоя проверки подлинности файла Cookie в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="f688e-256">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="f688e-257">Файл cookie политики по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="f688e-257">Cookie Policy Middleware</span></span>

<span data-ttu-id="f688e-258">[По промежуточного слоя файлов cookie политики](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) обеспечивает возможности политики куки-файл в приложении.</span><span class="sxs-lookup"><span data-stu-id="f688e-258">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="f688e-259">Добавление по промежуточного слоя в конвейере обработки приложения — порядок конфиденциальные; он влияет только на компоненты, зарегистрированные за ним в конвейере.</span><span class="sxs-lookup"><span data-stu-id="f688e-259">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="f688e-260">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) для по промежуточного слоя файлов Cookie политики позволяют управлять общих характеристик обработки файлов cookie и обработчик в обработчики обработки файла cookie при удалении или добавлены файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-260">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="f688e-261">Свойство.</span><span class="sxs-lookup"><span data-stu-id="f688e-261">Property</span></span> | <span data-ttu-id="f688e-262">Описание:</span><span class="sxs-lookup"><span data-stu-id="f688e-262">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="f688e-263">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="f688e-263">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="f688e-264">Влияет ли файлы cookie должен быть HttpOnly, который имеет флаг, указывающий, куки-файл должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="f688e-264">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="f688e-265">Значение по умолчанию — `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="f688e-265">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="f688e-266">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="f688e-266">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="f688e-267">Влияет на атрибут веб-сайте куки-файл (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="f688e-267">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="f688e-268">Значение по умолчанию — `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="f688e-268">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="f688e-269">Этот параметр доступен для основных компонентов ASP.NET 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="f688e-269">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="f688e-270">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="f688e-270">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="f688e-271">Вызывается при добавлении файла cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-271">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="f688e-272">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="f688e-272">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="f688e-273">Вызывается при удалении файла cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-273">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="f688e-274">Secure</span><span class="sxs-lookup"><span data-stu-id="f688e-274">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="f688e-275">Влияет ли файлы cookie должен быть Secure.</span><span class="sxs-lookup"><span data-stu-id="f688e-275">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="f688e-276">Значение по умолчанию — `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="f688e-276">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="f688e-277">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + только)</span><span class="sxs-lookup"><span data-stu-id="f688e-277">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="f688e-278">Значение по умолчанию `MinimumSameSitePolicy` значение `SameSiteMode.Lax` для разрешения проверки подлинности OAuth2.</span><span class="sxs-lookup"><span data-stu-id="f688e-278">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="f688e-279">Для строго принудительного применения политики веб-сайте `SameSiteMode.Strict`, задайте `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="f688e-279">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="f688e-280">Несмотря на то, что этот параметр сбрасывает OAuth2 и других схем проверки подлинности независимо от источника, он повышает уровень безопасности cookie для других типов приложений, которые не зависят от обработки запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="f688e-280">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="f688e-281">Параметр по промежуточного слоя куки-файл политики для `MinimumSameSitePolicy` может повлиять на ваш равным `Cookie.SameSite` в `CookieAuthenticationOptions` параметры в соответствии с Приведенная ниже таблица.</span><span class="sxs-lookup"><span data-stu-id="f688e-281">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="f688e-282">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="f688e-282">MinimumSameSitePolicy</span></span> | <span data-ttu-id="f688e-283">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="f688e-283">Cookie.SameSite</span></span> | <span data-ttu-id="f688e-284">Результирующий параметр Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="f688e-284">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="f688e-285">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f688e-285">SameSiteMode.None</span></span>     | <span data-ttu-id="f688e-286">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f688e-286">SameSiteMode.None</span></span><br><span data-ttu-id="f688e-287">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f688e-287">SameSiteMode.Lax</span></span><br><span data-ttu-id="f688e-288">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f688e-288">SameSiteMode.Strict</span></span> | <span data-ttu-id="f688e-289">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f688e-289">SameSiteMode.None</span></span><br><span data-ttu-id="f688e-290">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f688e-290">SameSiteMode.Lax</span></span><br><span data-ttu-id="f688e-291">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f688e-291">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="f688e-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f688e-292">SameSiteMode.Lax</span></span>      | <span data-ttu-id="f688e-293">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f688e-293">SameSiteMode.None</span></span><br><span data-ttu-id="f688e-294">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f688e-294">SameSiteMode.Lax</span></span><br><span data-ttu-id="f688e-295">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f688e-295">SameSiteMode.Strict</span></span> | <span data-ttu-id="f688e-296">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f688e-296">SameSiteMode.Lax</span></span><br><span data-ttu-id="f688e-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f688e-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="f688e-298">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f688e-298">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="f688e-299">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f688e-299">SameSiteMode.Strict</span></span>   | <span data-ttu-id="f688e-300">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f688e-300">SameSiteMode.None</span></span><br><span data-ttu-id="f688e-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f688e-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="f688e-302">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f688e-302">SameSiteMode.Strict</span></span> | <span data-ttu-id="f688e-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f688e-303">SameSiteMode.Strict</span></span><br><span data-ttu-id="f688e-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f688e-304">SameSiteMode.Strict</span></span><br><span data-ttu-id="f688e-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f688e-305">SameSiteMode.Strict</span></span> |

## <a name="creating-an-authentication-cookie"></a><span data-ttu-id="f688e-306">Создание файла cookie проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="f688e-306">Creating an authentication cookie</span></span>

<span data-ttu-id="f688e-307">Чтобы создать файл cookie, содержащий сведения о пользователе, следует создать [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="f688e-307">To create a cookie holding user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="f688e-308">Сведения о пользователе сериализуется и сохраняется в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-308">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f688e-309">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f688e-309">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f688e-310">Создание [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) с все необходимые [утверждения](/dotnet/api/system.security.claims.claim)s и вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) входа пользователя:</span><span class="sxs-lookup"><span data-stu-id="f688e-310">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[Main](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f688e-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f688e-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f688e-312">Вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) входа пользователя:</span><span class="sxs-lookup"><span data-stu-id="f688e-312">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="f688e-313">`SignInAsync`создает зашифрованный файл cookie и добавляет его в текущий ответ.</span><span class="sxs-lookup"><span data-stu-id="f688e-313">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="f688e-314">Если не указать `AuthenticationScheme`, используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f688e-314">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="f688e-315">На самом деле шифрования, используемый — ASP.NET Core [защиты данных](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) системы.</span><span class="sxs-lookup"><span data-stu-id="f688e-315">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="f688e-316">Если вы используете приложение на нескольких компьютерах, балансировки нагрузки для приложений или с помощью веб-фермы, то необходимо [настроить защиту данных](xref:security/data-protection/configuration/overview) использовать же ключей и идентификатор приложения.</span><span class="sxs-lookup"><span data-stu-id="f688e-316">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="f688e-317">Выход</span><span class="sxs-lookup"><span data-stu-id="f688e-317">Signing out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f688e-318">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f688e-318">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f688e-319">Чтобы выйти из системы текущего пользователя и удалить их куки-файл, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="f688e-319">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[Main](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f688e-320">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f688e-320">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f688e-321">Чтобы выйти из системы текущего пользователя и удалить их куки-файл, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="f688e-321">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="f688e-322">Если вы не используете `CookieAuthenticationDefaults.AuthenticationScheme` (или файлы «cookie») как схему (например, «ContosoCookie»), укажите схему, которая используется при настройке поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f688e-322">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="f688e-323">В противном случае используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f688e-323">Otherwise, the default scheme is used.</span></span>

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="f688e-324">Отклик на изменения в серверной части</span><span class="sxs-lookup"><span data-stu-id="f688e-324">Reacting to back-end changes</span></span>

<span data-ttu-id="f688e-325">После создания файла cookie, он становится единственным источником удостоверения.</span><span class="sxs-lookup"><span data-stu-id="f688e-325">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="f688e-326">Даже при отключении пользователя в серверной части системы системой проверки подлинности файла cookie не имеет сведений о это, а пользователь сохраняется вошедший в систему, при условии, что их файл cookie является допустимым.</span><span class="sxs-lookup"><span data-stu-id="f688e-326">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="f688e-327">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) событий в ASP.NET Core 2.x или [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) метод в ASP.NET Core 1.x можно использовать для перехвата и переопределение проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="f688e-327">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="f688e-328">Этот подход уменьшает риск отозванных пользователей, получающих доступ к приложению.</span><span class="sxs-lookup"><span data-stu-id="f688e-328">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="f688e-329">Одним из способов проверки cookie основан следить за изменением пользовательской базы данных.</span><span class="sxs-lookup"><span data-stu-id="f688e-329">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="f688e-330">Если базы данных не был изменен с момента выдачи объекта cookie пользователя, нет необходимости повторно пройти проверку подлинности пользователя при их куки-файл все еще действительна.</span><span class="sxs-lookup"><span data-stu-id="f688e-330">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="f688e-331">Чтобы реализовать этот сценарий базы данных, которая реализована в `IUserRepository` в этом примере хранит `LastChanged` значение.</span><span class="sxs-lookup"><span data-stu-id="f688e-331">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="f688e-332">При обновлении любого пользователя в базе данных, `LastChanged` имеет значение до текущего момента.</span><span class="sxs-lookup"><span data-stu-id="f688e-332">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="f688e-333">Чтобы сделать недействительными файла cookie при изменении базы данных на основе `LastChanged` значение, создайте файл cookie с `LastChanged` утверждение, содержащее текущий `LastChanged` значение из базы данных:</span><span class="sxs-lookup"><span data-stu-id="f688e-333">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f688e-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f688e-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f688e-335">Чтобы реализовать переопределение для `ValidatePrincipal` событий записи метод со следующей сигнатурой в классе, унаследованном от [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="f688e-335">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="f688e-336">Пример выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f688e-336">An example looks like the following:</span></span>

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

<span data-ttu-id="f688e-337">Зарегистрируйте экземпляр события во время регистрации службы куки-файл в `ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="f688e-337">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="f688e-338">Регистрация службы с областью для предоставления вашей `CustomCookieAuthenticationEvents` класса:</span><span class="sxs-lookup"><span data-stu-id="f688e-338">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f688e-339">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f688e-339">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f688e-340">Чтобы реализовать переопределение для `ValidateAsync` событий записи метод со следующей сигнатурой:</span><span class="sxs-lookup"><span data-stu-id="f688e-340">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="f688e-341">ASP.NET Core Identity реализует эту проверку как часть его [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="f688e-341">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="f688e-342">Пример выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f688e-342">An example looks like the following:</span></span>

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

<span data-ttu-id="f688e-343">Регистрировать событие во время настройки файла cookie проверки подлинности в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="f688e-343">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

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

<span data-ttu-id="f688e-344">Рассмотрим ситуацию, в которой имя пользователя будет обновлено &mdash; решение, которое не влияет на безопасность системы никаким образом.</span><span class="sxs-lookup"><span data-stu-id="f688e-344">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="f688e-345">Безболезненно обновить участника-пользователя следует вызвать `context.ReplacePrincipal` и задайте `context.ShouldRenew` свойства `true`.</span><span class="sxs-lookup"><span data-stu-id="f688e-345">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="f688e-346">Подход, описанный здесь срабатывает при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="f688e-346">The approach described here is triggered on every request.</span></span> <span data-ttu-id="f688e-347">Это может привести к снижению больших производительности для приложения.</span><span class="sxs-lookup"><span data-stu-id="f688e-347">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="f688e-348">Сохраняемые файлы cookie</span><span class="sxs-lookup"><span data-stu-id="f688e-348">Persistent cookies</span></span>

<span data-ttu-id="f688e-349">Вы можете куки-файл для сохранения между сеансами браузера.</span><span class="sxs-lookup"><span data-stu-id="f688e-349">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="f688e-350">Это сохраняемости должны включаться только с согласия пользователя с помощью флажка «Запомнить мои» на имя входа или аналогичного механизма.</span><span class="sxs-lookup"><span data-stu-id="f688e-350">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="f688e-351">Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, нечувствительным к через браузер замыкания.</span><span class="sxs-lookup"><span data-stu-id="f688e-351">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="f688e-352">Параметры скользящего срока действия ранее настроенные соблюдаются.</span><span class="sxs-lookup"><span data-stu-id="f688e-352">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="f688e-353">Если истекает срок действия файла cookie, при закрытии, браузер очищает куки-файл после его перезапуска.</span><span class="sxs-lookup"><span data-stu-id="f688e-353">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f688e-354">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f688e-354">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="f688e-355">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) класс находится в `Microsoft.AspNetCore.Authentication` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="f688e-355">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f688e-356">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f688e-356">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="f688e-357">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) класс находится в `Microsoft.AspNetCore.Http.Authentication` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="f688e-357">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="f688e-358">Срок действия файла cookie абсолютный</span><span class="sxs-lookup"><span data-stu-id="f688e-358">Absolute cookie expiration</span></span>

<span data-ttu-id="f688e-359">Можно задать абсолютный срок действия с `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="f688e-359">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="f688e-360">Необходимо также задать `IsPersistent`; в противном случае `ExpiresUtc` учитывается и создается файл cookie одного сеанса.</span><span class="sxs-lookup"><span data-stu-id="f688e-360">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="f688e-361">При `ExpiresUtc` устанавливается на `SignInAsync`, оно переопределяет значение `ExpireTimeSpan` параметр `CookieAuthenticationOptions`, если задать.</span><span class="sxs-lookup"><span data-stu-id="f688e-361">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="f688e-362">Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, срок действия составляет 20 минут.</span><span class="sxs-lookup"><span data-stu-id="f688e-362">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="f688e-363">Это не учитывает любые скользящий срок действия ранее настроенные.</span><span class="sxs-lookup"><span data-stu-id="f688e-363">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f688e-364">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f688e-364">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f688e-365">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f688e-365">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="see-also"></a><span data-ttu-id="f688e-366">См. также</span><span class="sxs-lookup"><span data-stu-id="f688e-366">See also</span></span>

* [<span data-ttu-id="f688e-367">Изменения AUTH 2.0 или объявления миграции</span><span class="sxs-lookup"><span data-stu-id="f688e-367">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="f688e-368">Ограничение идентификаторов по схеме</span><span class="sxs-lookup"><span data-stu-id="f688e-368">Limiting identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
