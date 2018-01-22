---
title: "Авторизация в нужной раскладки - ASP.NET Core"
author: rick-anderson
description: "В этой статье объясняется, как ограничить удостоверение для нужной раскладки при работе с несколькими методами проверки подлинности."
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 099dba1a4235ef62ea298748645b99e2d6d12d44
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="authorize-with-a-specific-scheme"></a><span data-ttu-id="cfaf6-103">Авторизация в нужной раскладки</span><span class="sxs-lookup"><span data-stu-id="cfaf6-103">Authorize with a specific scheme</span></span>

<span data-ttu-id="cfaf6-104">В некоторых сценариях, например приложений на одной странице (SPAs) обычно используется несколько методов проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="cfaf6-105">Например приложение может использовать проверку подлинности на основе файлов cookie для входа и проверки подлинности носителя JWT для запросов JavaScript.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="cfaf6-106">В некоторых случаях приложение может иметь несколько экземпляров обработчик проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="cfaf6-107">Например два обработчика куки-файл, где один содержит основные идентификаторов и один создается при инициирована многофакторная проверка подлинности (MFA).</span><span class="sxs-lookup"><span data-stu-id="cfaf6-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="cfaf6-108">Могут быть предприняты многофакторной проверки Подлинности, поскольку пользователь запросил операцию, которая требует дополнительной безопасности.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cfaf6-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cfaf6-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="cfaf6-110">Имя схемы проверки подлинности — при настройке службы проверки подлинности во время проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="cfaf6-111">Пример:</span><span class="sxs-lookup"><span data-stu-id="cfaf6-111">For example:</span></span>

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

<span data-ttu-id="cfaf6-112">В приведенном выше коде были добавлены два обработчики проверки подлинности: один для файлов cookie и один для носителя.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="cfaf6-113">Указание схемы по умолчанию приводит к `HttpContext.User` свойства, задаваемого этому удостоверению.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="cfaf6-114">Если такое поведение не требуется, отключите его с вызова без параметров форме `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cfaf6-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cfaf6-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="cfaf6-116">Схемы проверки подлинности-это именованные middlewares проверки подлинности настраиваются во время проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="cfaf6-117">Пример:</span><span class="sxs-lookup"><span data-stu-id="cfaf6-117">For example:</span></span>

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

<span data-ttu-id="cfaf6-118">В приведенном выше коде были добавлены два middlewares проверки подлинности: один для файлов cookie и один для носителя.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="cfaf6-119">Указание схемы по умолчанию приводит к `HttpContext.User` свойства, задаваемого этому удостоверению.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="cfaf6-120">Если такое поведение не требуется, отключить, задав `AuthenticationOptions.AutomaticAuthenticate` свойства `false`.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="cfaf6-121">При выборе схемы с атрибутом авторизовать</span><span class="sxs-lookup"><span data-stu-id="cfaf6-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="cfaf6-122">Во время авторизации приложение сообщает, что обработчик для использования.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="cfaf6-123">Выберите обработчик, с помощью которого приложение будет авторизации, передавая список с разделителями запятыми схем проверки подлинности для `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="cfaf6-124">`[Authorize]` Атрибут указывает схему проверки подлинности или схем, которые будут использовать независимо от того, настроена ли значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="cfaf6-125">Пример:</span><span class="sxs-lookup"><span data-stu-id="cfaf6-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cfaf6-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cfaf6-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cfaf6-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cfaf6-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="cfaf6-128">В предыдущем примере носителя и файл cookie обработчики запуска и иметь возможность создания и добавления удостоверение для текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="cfaf6-129">Указав только одну схему, выполняется соответствующий обработчик.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cfaf6-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cfaf6-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cfaf6-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cfaf6-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="cfaf6-132">В приведенном выше коде выполняется обработчик, со схемой «Bearer».</span><span class="sxs-lookup"><span data-stu-id="cfaf6-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="cfaf6-133">Всех удостоверений на основе файлов cookie учитываются.</span><span class="sxs-lookup"><span data-stu-id="cfaf6-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="cfaf6-134">При выборе схемы с помощью политик</span><span class="sxs-lookup"><span data-stu-id="cfaf6-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="cfaf6-135">Чтобы задать нужный схем в [политики](xref:security/authorization/policies), можно задать `AuthenticationSchemes` коллекции при добавлении политики:</span><span class="sxs-lookup"><span data-stu-id="cfaf6-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="cfaf6-136">В предыдущем примере политики «Over18» выполняется только для идентификаторов, созданные с помощью обработчика «Bearer».</span><span class="sxs-lookup"><span data-stu-id="cfaf6-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="cfaf6-137">Используйте политику, задав `[Authorize]` атрибута `Policy` свойство:</span><span class="sxs-lookup"><span data-stu-id="cfaf6-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
