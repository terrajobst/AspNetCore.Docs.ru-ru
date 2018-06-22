---
title: Использование файла cookie проверки подлинности без ASP.NET Core Identity
author: rick-anderson
description: Описание использования файлов cookie проверки подлинности без ASP.NET Core Identity
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: 2064e4d6406ce3ca3ce28732f113e8c81447aace
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275610"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="d2253-103">Использование файла cookie проверки подлинности без ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="d2253-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="d2253-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Latham Люк](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d2253-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d2253-105">Как видно в предыдущих разделах проверки подлинности, [ASP.NET Core Identity](xref:security/authentication/identity) является полным, полнофункциональный проверки подлинности поставщиком для создания и обслуживания имена входа.</span><span class="sxs-lookup"><span data-stu-id="d2253-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="d2253-106">Тем не менее можно использовать свою собственную логику для нестандартной проверки подлинности с помощью файла cookie проверки подлинности на основе время от времени.</span><span class="sxs-lookup"><span data-stu-id="d2253-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="d2253-107">Как поставщик проверки подлинности автономного без ASP.NET Core Identity можно использовать проверку подлинности на основе файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="d2253-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d2253-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d2253-109">Для демонстрации в пример приложения учетной записи пользователя для гипотетического пользователя Rodriguez Мария встроен в приложение.</span><span class="sxs-lookup"><span data-stu-id="d2253-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="d2253-110">Используйте имя электронной почты "maria.rodriguez@contoso.com" и все пароли для входа пользователя.</span><span class="sxs-lookup"><span data-stu-id="d2253-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="d2253-111">Пользователь проходит проверку подлинности в `AuthenticateUser` метод в *Pages/Account/Login.cshtml.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="d2253-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="d2253-112">В реальном примере пользователь будет пройти проверку подлинности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d2253-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="d2253-113">Сведения о миграции проверки подлинности на основе файла cookie из ASP.NET Core 1.x 2.0, см. [перенести проверку подлинности и удостоверение для ASP.NET 2.0 основных раздела (файл Cookie проверки подлинности на основе)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="d2253-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="d2253-114">При использовании ASP.NET Core Identity см. [Общие сведения об идентификации](xref:security/authentication/identity) раздела.</span><span class="sxs-lookup"><span data-stu-id="d2253-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="d2253-115">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="d2253-115">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d2253-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2253-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="d2253-117">Если приложение не использует [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), создайте ссылку на пакет в файл проекта для [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета (версии 2.1.0 или более поздние).</span><span class="sxs-lookup"><span data-stu-id="d2253-117">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="d2253-118">В `ConfigureServices` метод, создайте службу проверки подлинности по промежуточного слоя с `AddAuthentication` и `AddCookie` методов:</span><span class="sxs-lookup"><span data-stu-id="d2253-118">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="d2253-119">`AuthenticationScheme` передаваемый `AddAuthentication` задает схему проверки подлинности по умолчанию для приложения.</span><span class="sxs-lookup"><span data-stu-id="d2253-119">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="d2253-120">`AuthenticationScheme` полезно при наличии нескольких экземпляров файла cookie проверки подлинности, и вы хотите [авторизации с нужной раскладки](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="d2253-120">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="d2253-121">Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы.</span><span class="sxs-lookup"><span data-stu-id="d2253-121">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="d2253-122">Можно указать любое строковое значение, отличающее схему.</span><span class="sxs-lookup"><span data-stu-id="d2253-122">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="d2253-123">В `Configure` используйте `UseAuthentication` метод, вызываемый по промежуточного слоя проверки подлинности, задает `HttpContext.User` свойства.</span><span class="sxs-lookup"><span data-stu-id="d2253-123">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="d2253-124">Вызовите `UseAuthentication` метод перед вызовом метода `UseMvcWithDefaultRoute` или `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="d2253-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="d2253-125">**Параметры AddCookie**</span><span class="sxs-lookup"><span data-stu-id="d2253-125">**AddCookie Options**</span></span>

<span data-ttu-id="d2253-126">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) класс используется для настройки параметров поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d2253-126">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="d2253-127">Параметр</span><span class="sxs-lookup"><span data-stu-id="d2253-127">Option</span></span> | <span data-ttu-id="d2253-128">Описание:</span><span class="sxs-lookup"><span data-stu-id="d2253-128">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="d2253-129">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="d2253-129">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="d2253-130">Предоставляет путь для предоставления с 302 Found (перенаправление URL-адрес) при срабатывании триггера `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="d2253-130">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="d2253-131">Значение по умолчанию — `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="d2253-131">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="d2253-132">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="d2253-132">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="d2253-133">Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство утверждений, созданный с помощью службы проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-133">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="d2253-134">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="d2253-134">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="d2253-135">Имя домена, где обслуживают куки-файл.</span><span class="sxs-lookup"><span data-stu-id="d2253-135">The domain name where the cookie is served.</span></span> <span data-ttu-id="d2253-136">По умолчанию это имя узла запроса.</span><span class="sxs-lookup"><span data-stu-id="d2253-136">By default, this is the host name of the request.</span></span> <span data-ttu-id="d2253-137">Браузер отправляет только куки-файл в запросах имени узла.</span><span class="sxs-lookup"><span data-stu-id="d2253-137">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="d2253-138">Вы можете настроить этот параметр для файлов cookie, доступных любому узлу в вашем домене.</span><span class="sxs-lookup"><span data-stu-id="d2253-138">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="d2253-139">Например, установите значение домена файла cookie `.contoso.com` делает ее доступной для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="d2253-139">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="d2253-140">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="d2253-140">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="d2253-141">Возвращает или задает время существования куки-файла.</span><span class="sxs-lookup"><span data-stu-id="d2253-141">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="d2253-142">В настоящее время это параметр Нет ops и станет устаревшим в ASP.NET Core 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="d2253-142">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="d2253-143">Используйте `ExpireTimeSpan` параметр, чтобы задать срок действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-143">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="d2253-144">Дополнительные сведения см. в разделе [уточняется поведение CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="d2253-144">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="d2253-145">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="d2253-145">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="d2253-146">Флаг, указывающий куки-файл должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="d2253-146">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="d2253-147">Изменение этого значения для `false` разрешает клиентские скрипты для доступа к куки-файл и может открыть приложение кража файлов cookie, должны иметь приложения [межсайтовых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость.</span><span class="sxs-lookup"><span data-stu-id="d2253-147">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="d2253-148">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="d2253-148">The default value is `true`.</span></span> |
| [<span data-ttu-id="d2253-149">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="d2253-149">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="d2253-150">Задает имя файла cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-150">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="d2253-151">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="d2253-151">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="d2253-152">Использовать для изоляции приложений, выполняющихся в то же имя узла.</span><span class="sxs-lookup"><span data-stu-id="d2253-152">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="d2253-153">Если у вас есть приложение, работающее на `/app1` и нужно ограничить файлы cookie для данного приложения, задайте `CookiePath` свойства `/app1`.</span><span class="sxs-lookup"><span data-stu-id="d2253-153">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="d2253-154">Таким образом, файл cookie доступна только для запросов к `/app1` и любое приложение, входящие в него.</span><span class="sxs-lookup"><span data-stu-id="d2253-154">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="d2253-155">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="d2253-155">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="d2253-156">Указывает, является ли браузер должен позволять куки-файл для подключения к веб-сайте запросы только (`SameSiteMode.Strict`) или запросы между сайтами, с использованием безопасного HTTP методы и запросы веб-сайте (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="d2253-156">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="d2253-157">Если задано значение `SameSiteMode.None`, не задано значение заголовка файла cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-157">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="d2253-158">Обратите внимание, что [по промежуточного слоя файлов Cookie политики](#cookie-policy-middleware) может перезаписать предоставленное значение.</span><span class="sxs-lookup"><span data-stu-id="d2253-158">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="d2253-159">Для поддержки проверки подлинности OAuth, значение по умолчанию — `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="d2253-159">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="d2253-160">Дополнительные сведения см. в разделе [потеряны из-за политики SameSite файла cookie проверки подлинности OAuth](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="d2253-160">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="d2253-161">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="d2253-161">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="d2253-162">Флаг, указывающий, следует ли созданный файл cookie ограничиваются HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и при запросе (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="d2253-162">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="d2253-163">Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="d2253-163">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="d2253-164">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="d2253-164">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="d2253-165">Наборы `DataProtectionProvider` , используемый для создания по умолчанию `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="d2253-165">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="d2253-166">Если `TicketDataFormat` свойство задано, `DataProtectionProvider` параметр не используется.</span><span class="sxs-lookup"><span data-stu-id="d2253-166">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="d2253-167">Если не указано, используется поставщик защиты данных приложения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d2253-167">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="d2253-168">События</span><span class="sxs-lookup"><span data-stu-id="d2253-168">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="d2253-169">Обработчик вызывает методы поставщика, позволяют контролировать приложения в определенные моменты обработки.</span><span class="sxs-lookup"><span data-stu-id="d2253-169">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="d2253-170">Если `Events` не указан, предоставляется экземпляр по умолчанию, не выполняет никаких действий, при вызове методов.</span><span class="sxs-lookup"><span data-stu-id="d2253-170">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="d2253-171">EventsType</span><span class="sxs-lookup"><span data-stu-id="d2253-171">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="d2253-172">Используется как тип службы для получения `Events` экземпляром, а не свойства.</span><span class="sxs-lookup"><span data-stu-id="d2253-172">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="d2253-173">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="d2253-173">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="d2253-174">`TimeSpan` После которого срок действия билета проверки подлинности, хранящийся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-174">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="d2253-175">`ExpireTimeSpan` добавляется на текущий момент времени, чтобы создать срок действия билета.</span><span class="sxs-lookup"><span data-stu-id="d2253-175">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="d2253-176">`ExpiredTimeSpan` Значение всегда переходит в зашифрованных AuthTicket, проверить сервер.</span><span class="sxs-lookup"><span data-stu-id="d2253-176">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="d2253-177">Он также может перейти в [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) заголовок, но только если `IsPersistent` имеет значение.</span><span class="sxs-lookup"><span data-stu-id="d2253-177">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="d2253-178">Чтобы задать `IsPersistent` для `true`, настройте [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) передаваемый `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="d2253-178">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="d2253-179">Значение по умолчанию `ExpireTimeSpan` — 14 дней.</span><span class="sxs-lookup"><span data-stu-id="d2253-179">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="d2253-180">LoginPath</span><span class="sxs-lookup"><span data-stu-id="d2253-180">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="d2253-181">Предоставляет путь для предоставления с 302 Found (перенаправление URL-адрес) при срабатывании триггера `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="d2253-181">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="d2253-182">Текущий URL-адрес, создавший код 401, добавляется в `LoginPath` как параметр строки запроса с именем `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="d2253-182">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="d2253-183">Запрос один раз на `LoginPath` предоставляет нового вход удостоверения, `ReturnUrlParameter` значение используется для перенаправления браузера обратно в URL-адрес, вызвавший исходный код состояния не санкционировано.</span><span class="sxs-lookup"><span data-stu-id="d2253-183">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="d2253-184">Значение по умолчанию — `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="d2253-184">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="d2253-185">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="d2253-185">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="d2253-186">Если `LogoutPath` предоставляется обработчику, а затем перенаправляет запрос по этому пути в зависимости от значения `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="d2253-186">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="d2253-187">Значение по умолчанию — `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="d2253-187">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="d2253-188">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="d2253-188">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="d2253-189">Определяет имя параметра строки запроса, добавленный для обработчика ответа 302 Found (перенаправление URL-адрес).</span><span class="sxs-lookup"><span data-stu-id="d2253-189">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="d2253-190">`ReturnUrlParameter` используется при получении запроса на `LoginPath` или `LogoutPath` для возврата в браузере исходный URL-адрес после выполнения действия имени входа или выхода из системы.</span><span class="sxs-lookup"><span data-stu-id="d2253-190">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="d2253-191">Значение по умолчанию — `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="d2253-191">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="d2253-192">SessionStore</span><span class="sxs-lookup"><span data-stu-id="d2253-192">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="d2253-193">Необязательный контейнер, используемый для хранения идентификаторов различных запросов.</span><span class="sxs-lookup"><span data-stu-id="d2253-193">An optional container used to store identity across requests.</span></span> <span data-ttu-id="d2253-194">При использовании только идентификатор сеанса отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="d2253-194">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="d2253-195">`SessionStore` можно использовать для устранения возможных проблем с большой удостоверения.</span><span class="sxs-lookup"><span data-stu-id="d2253-195">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="d2253-196">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="d2253-196">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="d2253-197">Флаг, указывающий, должен быть новый файл cookie, срок действия обновленного выдан динамически.</span><span class="sxs-lookup"><span data-stu-id="d2253-197">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="d2253-198">Это может произойти на любой запрос, где текущего срока действия файла cookie более чем на 50% срок действия истек.</span><span class="sxs-lookup"><span data-stu-id="d2253-198">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="d2253-199">Новую дату окончания срока действия перемещается вперед для текущей даты, а также `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="d2253-199">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="d2253-200">[Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` класса при вызове `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="d2253-200">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="d2253-201">Абсолютный срок действия может повысить безопасность приложения, ограничивая количество времени, допустимый файл cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d2253-201">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="d2253-202">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="d2253-202">The default value is `true`.</span></span> |
| [<span data-ttu-id="d2253-203">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="d2253-203">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="d2253-204">`TicketDataFormat` Используется для установки и снятия защиты identity и других свойств, хранящихся в значении cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-204">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="d2253-205">Если не указано, `TicketDataFormat` создается с помощью [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="d2253-205">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="d2253-206">Проверка</span><span class="sxs-lookup"><span data-stu-id="d2253-206">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="d2253-207">Метод, который проверяет, что параметры являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="d2253-207">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="d2253-208">Задать `CookieAuthenticationOptions` в конфигурации службы для проверки подлинности в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="d2253-208">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d2253-209">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d2253-209">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="d2253-210">Куки-файл использует 1.x ASP.NET Core [по промежуточного слоя](xref:fundamentals/middleware/index) , сериализует участника-пользователя в зашифрованном файле cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-210">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="d2253-211">При последующих запросах куки-файл проверяется, и основной сервер заново и назначенные `HttpContext.User` свойство.</span><span class="sxs-lookup"><span data-stu-id="d2253-211">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="d2253-212">Установка [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета NuGet в проекте.</span><span class="sxs-lookup"><span data-stu-id="d2253-212">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="d2253-213">Этот пакет содержит файл cookie по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="d2253-213">This package contains the cookie middleware.</span></span>

<span data-ttu-id="d2253-214">Используйте `UseCookieAuthentication` метод в `Configure` метод в вашей *файла Startup.cs* файла перед `UseMvc` или `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="d2253-214">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="d2253-215">**Параметры CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="d2253-215">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="d2253-216">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) класс используется для настройки параметров поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d2253-216">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="d2253-217">Параметр</span><span class="sxs-lookup"><span data-stu-id="d2253-217">Option</span></span> | <span data-ttu-id="d2253-218">Описание:</span><span class="sxs-lookup"><span data-stu-id="d2253-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="d2253-219">authenticationScheme</span><span class="sxs-lookup"><span data-stu-id="d2253-219">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="d2253-220">Задает схему проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d2253-220">Sets the authentication scheme.</span></span> <span data-ttu-id="d2253-221">`AuthenticationScheme` полезно, когда несколько экземпляров проверки подлинности, и вы хотите [авторизации с нужной раскладки](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="d2253-221">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="d2253-222">Установка `AuthenticationScheme` для `CookieAuthenticationDefaults.AuthenticationScheme` предоставляет значение файлов «cookie» для схемы.</span><span class="sxs-lookup"><span data-stu-id="d2253-222">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="d2253-223">Можно указать любое строковое значение, отличающее схему.</span><span class="sxs-lookup"><span data-stu-id="d2253-223">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="d2253-224">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="d2253-224">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="d2253-225">Задает значение, указывающее, что файл cookie проверки подлинности запустите при каждом запросе и попытка проверки и воссоздания любому участнику сериализованный она создана.</span><span class="sxs-lookup"><span data-stu-id="d2253-225">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="d2253-226">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="d2253-226">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="d2253-227">Значение true, если по промежуточного слоя проверки подлинности обрабатывает автоматического проблем.</span><span class="sxs-lookup"><span data-stu-id="d2253-227">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="d2253-228">Если значение равно false, проверка подлинности по промежуточного слоя лишь изменяет ответы, если они явным образом указано `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="d2253-228">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="d2253-229">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="d2253-229">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="d2253-230">Издатель, используемый для [издателя](/dotnet/api/system.security.claims.claim.issuer) свойство все утверждения, созданные по промежуточного слоя проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-230">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="d2253-231">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="d2253-231">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="d2253-232">Имя домена, где обслуживают куки-файл.</span><span class="sxs-lookup"><span data-stu-id="d2253-232">The domain name where the cookie is served.</span></span> <span data-ttu-id="d2253-233">По умолчанию это имя узла запроса.</span><span class="sxs-lookup"><span data-stu-id="d2253-233">By default, this is the host name of the request.</span></span> <span data-ttu-id="d2253-234">Браузер служит только файла cookie для сопоставления имени узла.</span><span class="sxs-lookup"><span data-stu-id="d2253-234">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="d2253-235">Вы можете настроить этот параметр для файлов cookie, доступных любому узлу в вашем домене.</span><span class="sxs-lookup"><span data-stu-id="d2253-235">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="d2253-236">Например, установите значение домена файла cookie `.contoso.com` делает ее доступной для `contoso.com`, `www.contoso.com`, и `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="d2253-236">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="d2253-237">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="d2253-237">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="d2253-238">Флаг, указывающий куки-файл должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="d2253-238">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="d2253-239">Изменение этого значения для `false` разрешает клиентские скрипты для доступа к куки-файл и может открыть приложение кража файлов cookie, должны иметь приложения [межсайтовых сценариев (XSS)](xref:security/cross-site-scripting) уязвимость.</span><span class="sxs-lookup"><span data-stu-id="d2253-239">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="d2253-240">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="d2253-240">The default value is `true`.</span></span> |
| [<span data-ttu-id="d2253-241">CookiePath</span><span class="sxs-lookup"><span data-stu-id="d2253-241">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="d2253-242">Использовать для изоляции приложений, выполняющихся в то же имя узла.</span><span class="sxs-lookup"><span data-stu-id="d2253-242">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="d2253-243">Если у вас есть приложение, работающее на `/app1` и нужно ограничить файлы cookie для данного приложения, задайте `CookiePath` свойства `/app1`.</span><span class="sxs-lookup"><span data-stu-id="d2253-243">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="d2253-244">Таким образом, файл cookie доступна только для запросов к `/app1` и любое приложение, входящие в него.</span><span class="sxs-lookup"><span data-stu-id="d2253-244">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="d2253-245">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="d2253-245">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="d2253-246">Флаг, указывающий, следует ли созданный файл cookie ограничиваются HTTPS (`CookieSecurePolicy.Always`), HTTP или HTTPS (`CookieSecurePolicy.None`), или тот же протокол, что и при запросе (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="d2253-246">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="d2253-247">Значение по умолчанию — `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="d2253-247">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="d2253-248">Описание</span><span class="sxs-lookup"><span data-stu-id="d2253-248">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="d2253-249">Дополнительные сведения о типе проверки подлинности доступны для приложения.</span><span class="sxs-lookup"><span data-stu-id="d2253-249">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="d2253-250">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="d2253-250">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="d2253-251">`TimeSpan` После которого срок действия билета проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d2253-251">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="d2253-252">Он добавляется в текущее время, чтобы создать срок действия билета.</span><span class="sxs-lookup"><span data-stu-id="d2253-252">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="d2253-253">Для использования `ExpireTimeSpan`, необходимо задать `IsPersistent` для `true` в `AuthenticationProperties` передаваемый `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="d2253-253">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="d2253-254">Значение по умолчанию — 14 дней.</span><span class="sxs-lookup"><span data-stu-id="d2253-254">The default value is 14 days.</span></span> |
| [<span data-ttu-id="d2253-255">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="d2253-255">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="d2253-256">Флаг, указывающий, сбрасывается ли дата окончания срока действия файла cookie при более половины `ExpireTimeSpan` истечет интервал.</span><span class="sxs-lookup"><span data-stu-id="d2253-256">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="d2253-257">Новое время exipiration перемещается вперед для текущей даты, а также `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="d2253-257">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="d2253-258">[Время истечения срока действия файла cookie абсолютный](xref:security/authentication/cookie#absolute-cookie-expiration) можно задать с помощью `AuthenticationProperties` класса при вызове `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="d2253-258">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="d2253-259">Абсолютный срок действия может повысить безопасность приложения, ограничивая количество времени, допустимый файл cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d2253-259">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="d2253-260">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="d2253-260">The default value is `true`.</span></span> |

<span data-ttu-id="d2253-261">Задать `CookieAuthenticationOptions` по промежуточного слоя проверки подлинности файла Cookie в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="d2253-261">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="d2253-262">Файл cookie политики по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="d2253-262">Cookie Policy Middleware</span></span>

<span data-ttu-id="d2253-263">[По промежуточного слоя файлов cookie политики](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) обеспечивает возможности политики куки-файл в приложении.</span><span class="sxs-lookup"><span data-stu-id="d2253-263">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="d2253-264">Добавление по промежуточного слоя в конвейере обработки приложения — порядок конфиденциальные; он влияет только на компоненты, зарегистрированные за ним в конвейере.</span><span class="sxs-lookup"><span data-stu-id="d2253-264">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="d2253-265">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) для по промежуточного слоя файлов Cookie политики позволяют управлять общих характеристик обработки файлов cookie и обработчик в обработчики обработки файла cookie при удалении или добавлены файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-265">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="d2253-266">Свойство.</span><span class="sxs-lookup"><span data-stu-id="d2253-266">Property</span></span> | <span data-ttu-id="d2253-267">Описание:</span><span class="sxs-lookup"><span data-stu-id="d2253-267">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="d2253-268">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="d2253-268">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="d2253-269">Влияет ли файлы cookie должен быть HttpOnly, который имеет флаг, указывающий, куки-файл должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="d2253-269">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="d2253-270">Значение по умолчанию — `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="d2253-270">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="d2253-271">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="d2253-271">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="d2253-272">Влияет на атрибут веб-сайте куки-файл (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="d2253-272">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="d2253-273">Значение по умолчанию — `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="d2253-273">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="d2253-274">Этот параметр доступен для основных компонентов ASP.NET 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="d2253-274">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="d2253-275">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="d2253-275">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="d2253-276">Вызывается при добавлении файла cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-276">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="d2253-277">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="d2253-277">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="d2253-278">Вызывается при удалении файла cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-278">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="d2253-279">Защита</span><span class="sxs-lookup"><span data-stu-id="d2253-279">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="d2253-280">Влияет ли файлы cookie должен быть Secure.</span><span class="sxs-lookup"><span data-stu-id="d2253-280">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="d2253-281">Значение по умолчанию — `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="d2253-281">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="d2253-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + только)</span><span class="sxs-lookup"><span data-stu-id="d2253-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="d2253-283">Значение по умолчанию `MinimumSameSitePolicy` значение `SameSiteMode.Lax` для разрешения проверки подлинности OAuth2.</span><span class="sxs-lookup"><span data-stu-id="d2253-283">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="d2253-284">Для строго принудительного применения политики веб-сайте `SameSiteMode.Strict`, задайте `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="d2253-284">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="d2253-285">Несмотря на то, что этот параметр сбрасывает OAuth2 и других схем проверки подлинности независимо от источника, он повышает уровень безопасности cookie для других типов приложений, которые не зависят от обработки запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="d2253-285">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="d2253-286">Параметр по промежуточного слоя куки-файл политики для `MinimumSameSitePolicy` может повлиять на ваш равным `Cookie.SameSite` в `CookieAuthenticationOptions` параметры в соответствии с Приведенная ниже таблица.</span><span class="sxs-lookup"><span data-stu-id="d2253-286">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="d2253-287">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="d2253-287">MinimumSameSitePolicy</span></span> | <span data-ttu-id="d2253-288">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="d2253-288">Cookie.SameSite</span></span> | <span data-ttu-id="d2253-289">Результирующий параметр Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="d2253-289">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="d2253-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="d2253-290">SameSiteMode.None</span></span>     | <span data-ttu-id="d2253-291">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="d2253-291">SameSiteMode.None</span></span><br><span data-ttu-id="d2253-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d2253-292">SameSiteMode.Lax</span></span><br><span data-ttu-id="d2253-293">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d2253-293">SameSiteMode.Strict</span></span> | <span data-ttu-id="d2253-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="d2253-294">SameSiteMode.None</span></span><br><span data-ttu-id="d2253-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d2253-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="d2253-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d2253-296">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="d2253-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d2253-297">SameSiteMode.Lax</span></span>      | <span data-ttu-id="d2253-298">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="d2253-298">SameSiteMode.None</span></span><br><span data-ttu-id="d2253-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d2253-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="d2253-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d2253-300">SameSiteMode.Strict</span></span> | <span data-ttu-id="d2253-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d2253-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="d2253-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d2253-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="d2253-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d2253-303">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="d2253-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d2253-304">SameSiteMode.Strict</span></span>   | <span data-ttu-id="d2253-305">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="d2253-305">SameSiteMode.None</span></span><br><span data-ttu-id="d2253-306">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d2253-306">SameSiteMode.Lax</span></span><br><span data-ttu-id="d2253-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d2253-307">SameSiteMode.Strict</span></span> | <span data-ttu-id="d2253-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d2253-308">SameSiteMode.Strict</span></span><br><span data-ttu-id="d2253-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d2253-309">SameSiteMode.Strict</span></span><br><span data-ttu-id="d2253-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d2253-310">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="d2253-311">Создание файла cookie проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="d2253-311">Create an authentication cookie</span></span>

<span data-ttu-id="d2253-312">Чтобы создать файл cookie, содержащий сведения о пользователе, следует создать [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="d2253-312">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="d2253-313">Сведения о пользователе сериализуется и сохраняется в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-313">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d2253-314">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2253-314">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="d2253-315">Создание [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) с все необходимые [утверждения](/dotnet/api/system.security.claims.claim)s и вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) входа пользователя:</span><span class="sxs-lookup"><span data-stu-id="d2253-315">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d2253-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d2253-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="d2253-317">Вызовите [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) входа пользователя:</span><span class="sxs-lookup"><span data-stu-id="d2253-317">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="d2253-318">`SignInAsync` создает зашифрованный файл cookie и добавляет его в текущий ответ.</span><span class="sxs-lookup"><span data-stu-id="d2253-318">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="d2253-319">Если не указать `AuthenticationScheme`, используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d2253-319">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="d2253-320">На самом деле шифрования, используемый — ASP.NET Core [защиты данных](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) системы.</span><span class="sxs-lookup"><span data-stu-id="d2253-320">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="d2253-321">Если вы используете приложение на нескольких компьютерах, балансировки нагрузки для приложений или с помощью веб-фермы, то необходимо [настроить защиту данных](xref:security/data-protection/configuration/overview) использовать же ключей и идентификатор приложения.</span><span class="sxs-lookup"><span data-stu-id="d2253-321">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="d2253-322">Выйти</span><span class="sxs-lookup"><span data-stu-id="d2253-322">Sign out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d2253-323">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2253-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="d2253-324">Чтобы выйти из системы текущего пользователя и удалить их куки-файл, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="d2253-324">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d2253-325">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d2253-325">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="d2253-326">Чтобы выйти из системы текущего пользователя и удалить их куки-файл, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="d2253-326">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="d2253-327">Если вы не используете `CookieAuthenticationDefaults.AuthenticationScheme` (или файлы «cookie») как схему (например, «ContosoCookie»), укажите схему, которая используется при настройке поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d2253-327">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="d2253-328">В противном случае используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d2253-328">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="d2253-329">В ответ на изменение серверной части</span><span class="sxs-lookup"><span data-stu-id="d2253-329">React to back-end changes</span></span>

<span data-ttu-id="d2253-330">После создания файла cookie, он становится единственным источником удостоверения.</span><span class="sxs-lookup"><span data-stu-id="d2253-330">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="d2253-331">Даже при отключении пользователя в серверной части системы системой проверки подлинности файла cookie не имеет сведений о это, а пользователь сохраняется вошедший в систему, при условии, что их файл cookie является допустимым.</span><span class="sxs-lookup"><span data-stu-id="d2253-331">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="d2253-332">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) событий в ASP.NET Core 2.x или [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) метод в ASP.NET Core 1.x можно использовать для перехвата и переопределение проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="d2253-332">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="d2253-333">Этот подход уменьшает риск отозванных пользователей, получающих доступ к приложению.</span><span class="sxs-lookup"><span data-stu-id="d2253-333">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="d2253-334">Одним из способов проверки cookie основан следить за изменением пользовательской базы данных.</span><span class="sxs-lookup"><span data-stu-id="d2253-334">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="d2253-335">Если базы данных не был изменен с момента выдачи объекта cookie пользователя, нет необходимости повторно пройти проверку подлинности пользователя при их куки-файл все еще действительна.</span><span class="sxs-lookup"><span data-stu-id="d2253-335">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="d2253-336">Чтобы реализовать этот сценарий базы данных, которая реализована в `IUserRepository` в этом примере хранит `LastChanged` значение.</span><span class="sxs-lookup"><span data-stu-id="d2253-336">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="d2253-337">При обновлении любого пользователя в базе данных, `LastChanged` имеет значение до текущего момента.</span><span class="sxs-lookup"><span data-stu-id="d2253-337">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="d2253-338">Чтобы сделать недействительными файла cookie при изменении базы данных на основе `LastChanged` значение, создайте файл cookie с `LastChanged` утверждение, содержащее текущий `LastChanged` значение из базы данных:</span><span class="sxs-lookup"><span data-stu-id="d2253-338">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d2253-339">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2253-339">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d2253-340">Чтобы реализовать переопределение для `ValidatePrincipal` событий записи метод со следующей сигнатурой в классе, унаследованном от [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="d2253-340">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="d2253-341">Пример выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d2253-341">An example looks like the following:</span></span>

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

<span data-ttu-id="d2253-342">Зарегистрируйте экземпляр события во время регистрации службы куки-файл в `ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="d2253-342">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="d2253-343">Регистрация службы с областью для предоставления вашей `CustomCookieAuthenticationEvents` класса:</span><span class="sxs-lookup"><span data-stu-id="d2253-343">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d2253-344">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d2253-344">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d2253-345">Чтобы реализовать переопределение для `ValidateAsync` событий записи метод со следующей сигнатурой:</span><span class="sxs-lookup"><span data-stu-id="d2253-345">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="d2253-346">ASP.NET Core Identity реализует эту проверку как часть его [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="d2253-346">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="d2253-347">Пример выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d2253-347">An example looks like the following:</span></span>

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

<span data-ttu-id="d2253-348">Регистрировать событие во время настройки файла cookie проверки подлинности в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="d2253-348">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

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

<span data-ttu-id="d2253-349">Рассмотрим ситуацию, в которой имя пользователя будет обновлено &mdash; решение, которое не влияет на безопасность системы никаким образом.</span><span class="sxs-lookup"><span data-stu-id="d2253-349">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="d2253-350">Безболезненно обновить участника-пользователя следует вызвать `context.ReplacePrincipal` и задайте `context.ShouldRenew` свойства `true`.</span><span class="sxs-lookup"><span data-stu-id="d2253-350">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="d2253-351">Подход, описанный здесь срабатывает при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="d2253-351">The approach described here is triggered on every request.</span></span> <span data-ttu-id="d2253-352">Это может привести к снижению больших производительности для приложения.</span><span class="sxs-lookup"><span data-stu-id="d2253-352">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="d2253-353">Сохраняемые файлы cookie</span><span class="sxs-lookup"><span data-stu-id="d2253-353">Persistent cookies</span></span>

<span data-ttu-id="d2253-354">Вы можете куки-файл для сохранения между сеансами браузера.</span><span class="sxs-lookup"><span data-stu-id="d2253-354">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="d2253-355">Это сохраняемости должны включаться только с согласия пользователя с помощью флажка «Запомнить мои» на имя входа или аналогичного механизма.</span><span class="sxs-lookup"><span data-stu-id="d2253-355">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="d2253-356">Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, нечувствительным к через браузер замыкания.</span><span class="sxs-lookup"><span data-stu-id="d2253-356">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="d2253-357">Параметры скользящего срока действия ранее настроенные соблюдаются.</span><span class="sxs-lookup"><span data-stu-id="d2253-357">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="d2253-358">Если истекает срок действия файла cookie, при закрытии, браузер очищает куки-файл после его перезапуска.</span><span class="sxs-lookup"><span data-stu-id="d2253-358">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d2253-359">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2253-359">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="d2253-360">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) класс находится в `Microsoft.AspNetCore.Authentication` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="d2253-360">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d2253-361">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d2253-361">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="d2253-362">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) класс находится в `Microsoft.AspNetCore.Http.Authentication` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="d2253-362">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="d2253-363">Срок действия файла cookie абсолютный</span><span class="sxs-lookup"><span data-stu-id="d2253-363">Absolute cookie expiration</span></span>

<span data-ttu-id="d2253-364">Можно задать абсолютный срок действия с `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="d2253-364">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="d2253-365">Необходимо также задать `IsPersistent`; в противном случае `ExpiresUtc` учитывается и создается файл cookie одного сеанса.</span><span class="sxs-lookup"><span data-stu-id="d2253-365">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="d2253-366">При `ExpiresUtc` устанавливается на `SignInAsync`, оно переопределяет значение `ExpireTimeSpan` параметр `CookieAuthenticationOptions`, если задать.</span><span class="sxs-lookup"><span data-stu-id="d2253-366">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="d2253-367">Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, срок действия составляет 20 минут.</span><span class="sxs-lookup"><span data-stu-id="d2253-367">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="d2253-368">Это не учитывает любые скользящий срок действия ранее настроенные.</span><span class="sxs-lookup"><span data-stu-id="d2253-368">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d2253-369">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2253-369">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d2253-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d2253-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="additional-resources"></a><span data-ttu-id="d2253-371">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d2253-371">Additional resources</span></span>

* [<span data-ttu-id="d2253-372">Изменения AUTH 2.0 или объявления миграции</span><span class="sxs-lookup"><span data-stu-id="d2253-372">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="d2253-373">Ограничение идентификаторов по схеме</span><span class="sxs-lookup"><span data-stu-id="d2253-373">Limit identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
* [<span data-ttu-id="d2253-374">Авторизация на основе утверждений</span><span class="sxs-lookup"><span data-stu-id="d2253-374">Claims-Based Authorization</span></span>](xref:security/authorization/claims)
* [<span data-ttu-id="d2253-375">Проверки роли на основе политик</span><span class="sxs-lookup"><span data-stu-id="d2253-375">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
