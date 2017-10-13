---
title: "Авторизация в нужной раскладки - ASP.NET Core"
author: rick-anderson
description: "В этой статье объясняется, как ограничить удостоверение для нужной раскладки при работе с несколькими методами проверки подлинности."
keywords: "ASP.NET Core удостоверения, схема проверки подлинности"
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: cf3259f206b8d970cc6f2b0b9e52e233c30d6df3
ms.sourcegitcommit: e3b1726cc04e80dc28464c35259edbd3bc39a438
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/12/2017
---
# <a name="authorize-with-a-specific-scheme"></a><span data-ttu-id="5c69a-104">Авторизация в нужной раскладки</span><span class="sxs-lookup"><span data-stu-id="5c69a-104">Authorize with a specific scheme</span></span>

<span data-ttu-id="5c69a-105">В некоторых сценариях, например приложений на одной странице (SPAs) обычно используется несколько методов проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5c69a-105">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="5c69a-106">Например приложение может использовать проверку подлинности на основе файлов cookie для входа и проверки подлинности носителя JWT для запросов JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5c69a-106">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="5c69a-107">В некоторых случаях приложение может иметь несколько экземпляров обработчик проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5c69a-107">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="5c69a-108">Например два обработчика куки-файл, где один содержит основные идентификаторов и один создается при инициирована многофакторная проверка подлинности (MFA).</span><span class="sxs-lookup"><span data-stu-id="5c69a-108">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="5c69a-109">Могут быть предприняты многофакторной проверки Подлинности, поскольку пользователь запросил операцию, которая требует дополнительной безопасности.</span><span class="sxs-lookup"><span data-stu-id="5c69a-109">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c69a-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c69a-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5c69a-111">Имя схемы проверки подлинности — при настройке службы проверки подлинности во время проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5c69a-111">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="5c69a-112">Пример:</span><span class="sxs-lookup"><span data-stu-id="5c69a-112">For example:</span></span>

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

<span data-ttu-id="5c69a-113">В приведенном выше коде были добавлены два обработчики проверки подлинности: один для файлов cookie и один для носителя.</span><span class="sxs-lookup"><span data-stu-id="5c69a-113">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="5c69a-114">Указание схемы по умолчанию приводит к `HttpContext.User` свойства, задаваемого этому удостоверению.</span><span class="sxs-lookup"><span data-stu-id="5c69a-114">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="5c69a-115">Если такое поведение не требуется, отключите его с вызова без параметров форме `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="5c69a-115">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c69a-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c69a-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5c69a-117">Схемы проверки подлинности-это именованные middlewares проверки подлинности настраиваются во время проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5c69a-117">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="5c69a-118">Пример:</span><span class="sxs-lookup"><span data-stu-id="5c69a-118">For example:</span></span>

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

<span data-ttu-id="5c69a-119">В приведенном выше коде были добавлены два middlewares проверки подлинности: один для файлов cookie и один для носителя.</span><span class="sxs-lookup"><span data-stu-id="5c69a-119">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="5c69a-120">Указание схемы по умолчанию приводит к `HttpContext.User` свойства, задаваемого этому удостоверению.</span><span class="sxs-lookup"><span data-stu-id="5c69a-120">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="5c69a-121">Если такое поведение не требуется, отключить, задав `AuthenticationOptions.AutomaticAuthenticate` свойства `false`.</span><span class="sxs-lookup"><span data-stu-id="5c69a-121">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="5c69a-122">При выборе схемы с атрибутом авторизовать</span><span class="sxs-lookup"><span data-stu-id="5c69a-122">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="5c69a-123">Во время авторизации приложение сообщает, что обработчик для использования.</span><span class="sxs-lookup"><span data-stu-id="5c69a-123">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="5c69a-124">Выберите обработчик, с помощью которого приложение будет авторизации, передавая список с разделителями запятыми схем проверки подлинности для `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="5c69a-124">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="5c69a-125">`[Authorize]` Атрибут указывает схему проверки подлинности или схем, которые будут использовать независимо от того, настроена ли значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5c69a-125">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="5c69a-126">Пример:</span><span class="sxs-lookup"><span data-stu-id="5c69a-126">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c69a-127">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c69a-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c69a-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c69a-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

---

<span data-ttu-id="5c69a-129">В предыдущем примере носителя и файл cookie обработчики запуска и иметь возможность создания и добавления удостоверение для текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="5c69a-129">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="5c69a-130">Указав только одну схему, выполняется соответствующий обработчик.</span><span class="sxs-lookup"><span data-stu-id="5c69a-130">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c69a-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c69a-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = "Bearer")]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c69a-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c69a-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
public class MixedController : Controller
```

---

<span data-ttu-id="5c69a-133">В приведенном выше коде выполняется обработчик, со схемой «Bearer».</span><span class="sxs-lookup"><span data-stu-id="5c69a-133">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="5c69a-134">Всех удостоверений на основе файлов cookie учитываются.</span><span class="sxs-lookup"><span data-stu-id="5c69a-134">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="5c69a-135">При выборе схемы с помощью политик</span><span class="sxs-lookup"><span data-stu-id="5c69a-135">Selecting the scheme with policies</span></span>

<span data-ttu-id="5c69a-136">Чтобы задать нужный схем в [политики](xref:security/authorization/policies#security-authorization-policies-based), можно задать `AuthenticationSchemes` коллекции при добавлении политики:</span><span class="sxs-lookup"><span data-stu-id="5c69a-136">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies#security-authorization-policies-based), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add("Bearer");
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

<span data-ttu-id="5c69a-137">В предыдущем примере политики «Over18» выполняется только для идентификаторов, созданные с помощью обработчика «Bearer».</span><span class="sxs-lookup"><span data-stu-id="5c69a-137">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="5c69a-138">Используйте политику, задав `[Authorize]` атрибута `Policy` свойство:</span><span class="sxs-lookup"><span data-stu-id="5c69a-138">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
