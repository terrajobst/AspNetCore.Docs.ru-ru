---
title: Авторизация с определенной схемой в ASP.NET Core
author: rick-anderson
description: В этой статье объясняется, как для ограничения удостоверений для определенной схемы, при работе с нескольких методов проверки подлинности.
ms.author: riande
ms.date: 10/22/2018
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 778bb61f472ab2e76f85da5999d3c79238188f19
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248203"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="0af9e-103">Авторизация с определенной схемой в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0af9e-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="0af9e-104">В некоторых сценариях, таких как одностраничные приложения (SPA) довольно часто для использования нескольких методов проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="0af9e-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="0af9e-105">Например приложение может использовать проверку подлинности на основе файлов cookie для входа и проверки подлинности носителя JWT для запросов JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0af9e-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="0af9e-106">В некоторых случаях приложение может иметь несколько экземпляров обработчика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="0af9e-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="0af9e-107">Например два обработчика файлов cookie, где один содержит базовой идентификации и один создается при активации многофакторной проверки подлинности (MFA).</span><span class="sxs-lookup"><span data-stu-id="0af9e-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="0af9e-108">Могут быть предприняты многофакторной проверки Подлинности, поскольку пользователь запросил операцию, которая требует дополнительной безопасности.</span><span class="sxs-lookup"><span data-stu-id="0af9e-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0af9e-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0af9e-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0af9e-110">При настройке службы проверки подлинности во время проверки подлинности с именем схемы проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="0af9e-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="0af9e-111">Пример:</span><span class="sxs-lookup"><span data-stu-id="0af9e-111">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

<span data-ttu-id="0af9e-112">В приведенном выше коде, были добавлены два обработчика проверки подлинности: один для файлов cookie и один для носителя.</span><span class="sxs-lookup"><span data-stu-id="0af9e-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="0af9e-113">Указывает схему по умолчанию приводит `HttpContext.User` свойство этому удостоверению.</span><span class="sxs-lookup"><span data-stu-id="0af9e-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="0af9e-114">Если такое поведение не требуется, отключите его путем вызова без параметров вида `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="0af9e-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0af9e-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0af9e-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0af9e-116">Схемы проверки подлинности присваиваются в том случае, если по промежуточного слоя проверки подлинности настраиваются во время проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="0af9e-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="0af9e-117">Пример:</span><span class="sxs-lookup"><span data-stu-id="0af9e-117">For example:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

<span data-ttu-id="0af9e-118">В приведенном выше коде, были добавлены два компонента проверки подлинности: один для файлов cookie и один для носителя.</span><span class="sxs-lookup"><span data-stu-id="0af9e-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="0af9e-119">Указывает схему по умолчанию приводит `HttpContext.User` свойство этому удостоверению.</span><span class="sxs-lookup"><span data-stu-id="0af9e-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="0af9e-120">Если такое поведение не требуется, отключить, задав `AuthenticationOptions.AutomaticAuthenticate` свойства `false`.</span><span class="sxs-lookup"><span data-stu-id="0af9e-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="0af9e-121">Выбор схемы с помощью атрибута авторизации</span><span class="sxs-lookup"><span data-stu-id="0af9e-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="0af9e-122">При авторизации приложение указывает обработчик для использования.</span><span class="sxs-lookup"><span data-stu-id="0af9e-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="0af9e-123">Выберите обработчик, с которым приложение будет авторизовать путем передачи разделенный запятыми список схем проверки подлинности `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="0af9e-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="0af9e-124">`[Authorize]` Атрибут указывает схему проверки подлинности или схемы, чтобы использовать независимо от того, настроена ли по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0af9e-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="0af9e-125">Пример:</span><span class="sxs-lookup"><span data-stu-id="0af9e-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0af9e-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0af9e-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0af9e-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0af9e-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

<span data-ttu-id="0af9e-128">В приведенном выше примере носителя и файл cookie обработчики запуска и иметь возможность создать и добавить удостоверение для текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="0af9e-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="0af9e-129">Указав только единственную схему, выполняется соответствующий обработчик.</span><span class="sxs-lookup"><span data-stu-id="0af9e-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0af9e-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0af9e-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0af9e-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0af9e-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="0af9e-132">В приведенном выше коде выполняется только обработчик с использованием схемы «Bearer».</span><span class="sxs-lookup"><span data-stu-id="0af9e-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="0af9e-133">Удостоверения на основе файлов cookie учитываются.</span><span class="sxs-lookup"><span data-stu-id="0af9e-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="0af9e-134">Выбор схемы с помощью политик</span><span class="sxs-lookup"><span data-stu-id="0af9e-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="0af9e-135">Если вы хотите задать нужный схем в [политики](xref:security/authorization/policies), можно задать `AuthenticationSchemes` коллекции при добавлении политики:</span><span class="sxs-lookup"><span data-stu-id="0af9e-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

<span data-ttu-id="0af9e-136">В приведенном выше примере «Over18» политики выполняется только от удостоверения, созданного с помощью обработчика «Bearer».</span><span class="sxs-lookup"><span data-stu-id="0af9e-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="0af9e-137">Используйте политику, задав `[Authorize]` атрибута `Policy` свойство:</span><span class="sxs-lookup"><span data-stu-id="0af9e-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a><span data-ttu-id="0af9e-138">Использование нескольких схем проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="0af9e-138">Use multiple authentication schemes</span></span>

<span data-ttu-id="0af9e-139">Некоторые приложения может потребоваться поддержка нескольких типов проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="0af9e-139">Some apps may need to support multiple types of authentication.</span></span> <span data-ttu-id="0af9e-140">Например приложение может проверять подлинность пользователей из Azure Active Directory и из базы данных пользователей.</span><span class="sxs-lookup"><span data-stu-id="0af9e-140">For example, your app might authenticate users from Azure Active Directory and from a users database.</span></span> <span data-ttu-id="0af9e-141">Другой пример — приложение, которое выполняет проверку подлинности пользователей служб федерации Active Directory и Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="0af9e-141">Another example is an app that authenticates users from both Active Directory Federation Services and Azure Active Directory B2C.</span></span> <span data-ttu-id="0af9e-142">В этом случае приложение должно принимать токена носителя JWT от нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="0af9e-142">In this case, the app should accept a JWT bearer token from several issuers.</span></span>

<span data-ttu-id="0af9e-143">Добавьте все схемы проверки подлинности, которые вы хотите принять.</span><span class="sxs-lookup"><span data-stu-id="0af9e-143">Add all authentication schemes you'd like to accept.</span></span> <span data-ttu-id="0af9e-144">Например, следующий код в `Startup.ConfigureServices` добавляет две схемы проверки подлинности носителя JWT с помощью различных поставщиков:</span><span class="sxs-lookup"><span data-stu-id="0af9e-144">For example, the following code in `Startup.ConfigureServices` adds two JWT bearer authentication schemes with different issuers:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://localhost:5000/identity/";
        })
        .AddJwtBearer("AzureAD", options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://login.microsoftonline.com/eb971100-6f99-4bdc-8611-1bc8edd7f436/";
        });
}
```

> [!NOTE]
> <span data-ttu-id="0af9e-145">Схема проверки подлинности по умолчанию зарегистрирован только один проверки подлинности носителя JWT `JwtBearerDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="0af9e-145">Only one JWT bearer authentication is registered with the default authentication scheme `JwtBearerDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="0af9e-146">Дополнительная проверка подлинности должен быть зарегистрирован с уникальной аутентификацией на основе схемы.</span><span class="sxs-lookup"><span data-stu-id="0af9e-146">Additional authentication has to be registered with a unique authentication scheme.</span></span>

<span data-ttu-id="0af9e-147">Следующим шагом является обновление политики авторизации по умолчанию для приема обе схемы проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="0af9e-147">The next step is to update the default authorization policy to accept both authentication schemes.</span></span> <span data-ttu-id="0af9e-148">Пример:</span><span class="sxs-lookup"><span data-stu-id="0af9e-148">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthorization(options =>
    {
        var defaultAuthorizationPolicyBuilder = new AuthorizationPolicyBuilder(
            JwtBearerDefaults.AuthenticationScheme,
            "AzureAD");
        defaultAuthorizationPolicyBuilder = 
            defaultAuthorizationPolicyBuilder.RequireAuthenticatedUser();
        options.DefaultPolicy = defaultAuthorizationPolicyBuilder.Build();
    });
}
```

<span data-ttu-id="0af9e-149">Как политика авторизации по умолчанию переопределяется, это можно использовать `[Authorize]` атрибутом в контроллерах.</span><span class="sxs-lookup"><span data-stu-id="0af9e-149">As the default authorization policy is overridden, it's possible to use the `[Authorize]` attribute in controllers.</span></span> <span data-ttu-id="0af9e-150">Затем контроллер принимает запросы с маркер JWT, выданный первого или второго поставщика.</span><span class="sxs-lookup"><span data-stu-id="0af9e-150">The controller then accepts requests with JWT issued by the first or second issuer.</span></span>

::: moniker-end
