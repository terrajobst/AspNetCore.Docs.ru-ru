---
title: Миграция проверки подлинности и удостоверения в ASP.NET Core 2.0
author: scottaddie
description: В этой статье описаны наиболее распространенные действия при миграции проверка подлинности ASP.NET Core 1.x и удостоверение ASP.NET Core 2.0.
ms.author: scaddie
ms.date: 10/26/2017
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 6d457d42ad29ca579ba74e3b097d143bd6531b72
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838327"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="3f662-103">Миграция проверки подлинности и удостоверения в ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="3f662-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="3f662-104">По [Scott Addie](https://github.com/scottaddie) и [поздравить Хао](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="3f662-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="3f662-105">ASP.NET Core 2.0 реализована новая модель для проверки подлинности и [удостоверений](xref:security/authentication/identity) с помощью служб, который упрощает настройку.</span><span class="sxs-lookup"><span data-stu-id="3f662-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="3f662-106">Приложения ASP.NET Core 1.x, использующие проверку подлинности или удостоверение можно обновить для использования новой модели, как описано ниже.</span><span class="sxs-lookup"><span data-stu-id="3f662-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="3f662-107">По промежуточного слоя проверки подлинности и службы</span><span class="sxs-lookup"><span data-stu-id="3f662-107">Authentication Middleware and services</span></span>
<span data-ttu-id="3f662-108">В проектах версии 1.x проверки подлинности настраивается с помощью по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="3f662-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="3f662-109">Для каждой схемы проверки подлинности, которые требуется поддерживать вызывается метод по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="3f662-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="3f662-110">В следующем примере 1.x настраивает проверку подлинности Facebook с использованием удостоверения *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f662-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions { 
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
} 
```

<span data-ttu-id="3f662-111">В проектах 2.0 настроена проверка подлинности через службы.</span><span class="sxs-lookup"><span data-stu-id="3f662-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="3f662-112">Каждая схема проверки подлинности регистрируется в `ConfigureServices` метод *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="3f662-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="3f662-113">`UseIdentity` Метод заменяется `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="3f662-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="3f662-114">Приведенный ниже 2.0 настраивает проверку подлинности Facebook с использованием удостоверения *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f662-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

<span data-ttu-id="3f662-115">`UseAuthentication` Метод добавляет компонент по промежуточного слоя проверки подлинности, который отвечает за автоматической проверки подлинности, а также обработка запросов удаленной проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3f662-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="3f662-116">Он заменяет все компоненты по промежуточного слоя для отдельных единого компонент по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="3f662-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="3f662-117">Ниже приведены 2.0 инструкции по миграции для каждой схемы основной проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3f662-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="3f662-118">Проверка подлинности на основе файлов cookie</span><span class="sxs-lookup"><span data-stu-id="3f662-118">Cookie-based authentication</span></span>
<span data-ttu-id="3f662-119">Выберите один из приведенных ниже параметров и внесите необходимые изменения в *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f662-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="3f662-120">Использование файлов cookie с удостоверением</span><span class="sxs-lookup"><span data-stu-id="3f662-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="3f662-121">Замените `UseIdentity` с `UseAuthentication` в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="3f662-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="3f662-122">Вызвать `AddIdentity` метод в `ConfigureServices` метод для добавления службы проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="3f662-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="3f662-123">При необходимости вызывать `ConfigureApplicationCookie` или `ConfigureExternalCookie` метод в `ConfigureServices` метод, чтобы настроить параметры файла cookie удостоверений.</span><span class="sxs-lookup"><span data-stu-id="3f662-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="3f662-124">Использование файлов cookie без Identity</span><span class="sxs-lookup"><span data-stu-id="3f662-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="3f662-125">Замените `UseCookieAuthentication` вызов метода `Configure` метод с `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3f662-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="3f662-126">Вызвать `AddAuthentication` и `AddCookie` методы в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="3f662-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User, 
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options => 
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="3f662-127">Проверки подлинности носителя JWT</span><span class="sxs-lookup"><span data-stu-id="3f662-127">JWT Bearer Authentication</span></span>
<span data-ttu-id="3f662-128">Внесите следующие изменения в *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f662-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="3f662-129">Замените `UseJwtBearerAuthentication` вызов метода `Configure` метод с `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3f662-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="3f662-130">Вызвать `AddJwtBearer` метод в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="3f662-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="3f662-131">В этом фрагменте кода не использует удостоверение, поэтому схема по умолчанию задается путем передачи `JwtBearerDefaults.AuthenticationScheme` для `AddAuthentication` метод.</span><span class="sxs-lookup"><span data-stu-id="3f662-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="3f662-132">Проверки подлинности OpenID Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="3f662-132">OpenID Connect (OIDC) authentication</span></span>
<span data-ttu-id="3f662-133">Внесите следующие изменения в *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f662-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="3f662-134">Замените `UseOpenIdConnectAuthentication` вызов метода `Configure` метод с `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3f662-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="3f662-135">Вызвать `AddOpenIdConnect` метод в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="3f662-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(options => 
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options => 
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a><span data-ttu-id="3f662-136">Проверка подлинности Facebook</span><span class="sxs-lookup"><span data-stu-id="3f662-136">Facebook authentication</span></span>
<span data-ttu-id="3f662-137">Внесите следующие изменения в *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f662-137">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="3f662-138">Замените `UseFacebookAuthentication` вызов метода `Configure` метод с `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3f662-138">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="3f662-139">Вызвать `AddFacebook` метод в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="3f662-139">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="3f662-140">Проверка подлинности Google</span><span class="sxs-lookup"><span data-stu-id="3f662-140">Google authentication</span></span>
<span data-ttu-id="3f662-141">Внесите следующие изменения в *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f662-141">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="3f662-142">Замените `UseGoogleAuthentication` вызов метода `Configure` метод с `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3f662-142">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="3f662-143">Вызвать `AddGoogle` метод в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="3f662-143">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="3f662-144">Проверка подлинности учетной записи Майкрософт</span><span class="sxs-lookup"><span data-stu-id="3f662-144">Microsoft Account authentication</span></span>
<span data-ttu-id="3f662-145">Внесите следующие изменения в *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f662-145">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="3f662-146">Замените `UseMicrosoftAccountAuthentication` вызов метода `Configure` метод с `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3f662-146">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="3f662-147">Вызвать `AddMicrosoftAccount` метод в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="3f662-147">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="3f662-148">Проверка подлинности Twitter</span><span class="sxs-lookup"><span data-stu-id="3f662-148">Twitter authentication</span></span>
<span data-ttu-id="3f662-149">Внесите следующие изменения в *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f662-149">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="3f662-150">Замените `UseTwitterAuthentication` вызов метода `Configure` метод с `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3f662-150">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="3f662-151">Вызвать `AddTwitter` метод в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="3f662-151">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="3f662-152">Параметр схемы проверки подлинности по умолчанию</span><span class="sxs-lookup"><span data-stu-id="3f662-152">Setting default authentication schemes</span></span>
<span data-ttu-id="3f662-153">В версии 1.x `AutomaticAuthenticate` и `AutomaticChallenge` свойства [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) базового класса должны устанавливаться на основе схемы проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3f662-153">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="3f662-154">Невозможно было хорошо это обеспечить.</span><span class="sxs-lookup"><span data-stu-id="3f662-154">There was no good way to enforce this.</span></span>

<span data-ttu-id="3f662-155">В версии 2.0, эти свойства будут удалены как свойства в отдельных `AuthenticationOptions` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="3f662-155">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="3f662-156">Они могут быть настроены в `AddAuthentication` вызова метода внутри `ConfigureServices` метод *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f662-156">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="3f662-157">В предыдущем фрагменте кода, схемой по умолчанию имеет значение `CookieAuthenticationDefaults.AuthenticationScheme` («файлы cookie»).</span><span class="sxs-lookup"><span data-stu-id="3f662-157">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="3f662-158">Кроме того, используйте перегруженную версию `AddAuthentication` метод, чтобы задать более одного свойства.</span><span class="sxs-lookup"><span data-stu-id="3f662-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="3f662-159">В следующем примере перегруженный метод, схемой по умолчанию имеет значение `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="3f662-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="3f662-160">Схема проверки подлинности в качестве альтернативы можно составить в индивидуальную `[Authorize]` атрибутов или политик авторизации.</span><span class="sxs-lookup"><span data-stu-id="3f662-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="3f662-161">Определите схемой по умолчанию в версии 2.0, если выполняется одно из следующих условий:</span><span class="sxs-lookup"><span data-stu-id="3f662-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="3f662-162">Требуется, чтобы пользователь мог автоматически входить в</span><span class="sxs-lookup"><span data-stu-id="3f662-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="3f662-163">Использовании `[Authorize]` атрибута или авторизации политики без указания схемы</span><span class="sxs-lookup"><span data-stu-id="3f662-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="3f662-164">Исключением из этого правила является `AddIdentity` метод.</span><span class="sxs-lookup"><span data-stu-id="3f662-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="3f662-165">Этот метод добавляет файлы cookie для вас и наборы по умолчанию для проверки подлинности и бросьте схемы для файла cookie приложения `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="3f662-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="3f662-166">Кроме того, он задает схему по умолчанию вход внешний файл cookie `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="3f662-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="3f662-167">Использовать модули проверки подлинности HttpContext</span><span class="sxs-lookup"><span data-stu-id="3f662-167">Use HttpContext authentication extensions</span></span>
<span data-ttu-id="3f662-168">`IAuthenticationManager` Интерфейс является главную точку входа в систему проверки подлинности 1.x.</span><span class="sxs-lookup"><span data-stu-id="3f662-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="3f662-169">Он был заменен новым набором `HttpContext` методы расширения в `Microsoft.AspNetCore.Authentication` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="3f662-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="3f662-170">Например, 1.x проекты ссылки `Authentication` свойство:</span><span class="sxs-lookup"><span data-stu-id="3f662-170">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="3f662-171">В проектах 2.0 импортировать `Microsoft.AspNetCore.Authentication` пространства имен и удалите `Authentication` ссылок на свойства:</span><span class="sxs-lookup"><span data-stu-id="3f662-171">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="3f662-172">Проверка подлинности Windows (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="3f662-172">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="3f662-173">Существует два варианта проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="3f662-173">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="3f662-174">Узел разрешает только пользователям, прошедшим проверку подлинности</span><span class="sxs-lookup"><span data-stu-id="3f662-174">The host only allows authenticated users</span></span>
2. <span data-ttu-id="3f662-175">Узел позволяет использовать объекты анонимных и прошедшие проверку пользователи</span><span class="sxs-lookup"><span data-stu-id="3f662-175">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="3f662-176">Первый вариант, описанных выше 2.0 изменения не влияют.</span><span class="sxs-lookup"><span data-stu-id="3f662-176">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="3f662-177">Второй вариант, описанных выше зависит от изменения версии 2.0.</span><span class="sxs-lookup"><span data-stu-id="3f662-177">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="3f662-178">Например, вы может разрешение анонимных пользователей в приложение в IIS или [HTTP.sys](xref:fundamentals/servers/weblistener) слоя но авторизацию пользователей на уровне контроллера.</span><span class="sxs-lookup"><span data-stu-id="3f662-178">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="3f662-179">В этом случае значение схемы по умолчанию `IISDefaults.AuthenticationScheme` в `ConfigureServices` метод *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f662-179">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="3f662-180">Не удалось установить схему по умолчанию соответствующим образом предотвращает запроса authorize бросить вызов работу.</span><span class="sxs-lookup"><span data-stu-id="3f662-180">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="3f662-181">Экземпляры IdentityCookieOptions</span><span class="sxs-lookup"><span data-stu-id="3f662-181">IdentityCookieOptions instances</span></span>
<span data-ttu-id="3f662-182">Побочный эффект изменения 2.0 заключается в переходе на использование именованных параметров, а не к экземпляру параметры файла cookie.</span><span class="sxs-lookup"><span data-stu-id="3f662-182">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="3f662-183">Возможность настраивать имена схем идентификации файл cookie удаляется.</span><span class="sxs-lookup"><span data-stu-id="3f662-183">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="3f662-184">Например, 1.x проекты использующие [внедрение через конструктор](xref:mvc/controllers/dependency-injection#constructor-injection) для передачи `IdentityCookieOptions` параметр в *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="3f662-184">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="3f662-185">Схема проверки подлинности внешних файлов cookie осуществляется из предоставленного экземпляра.</span><span class="sxs-lookup"><span data-stu-id="3f662-185">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="3f662-186">Упомянутые выше конструктор injection становится ненужным в проектах 2.0 и `_externalCookieScheme` поле можно удалить:</span><span class="sxs-lookup"><span data-stu-id="3f662-186">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="3f662-187">`IdentityConstants.ExternalScheme` Константа может быть использована напрямую:</span><span class="sxs-lookup"><span data-stu-id="3f662-187">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="3f662-188">Добавление IdentityUser POCO свойства навигации</span><span class="sxs-lookup"><span data-stu-id="3f662-188">Add IdentityUser POCO navigation properties</span></span>
<span data-ttu-id="3f662-189">Свойства навигации Entity Framework (EF) Core базового `IdentityUser` POCO (обычные старые объекты среды CLR) будут удалены.</span><span class="sxs-lookup"><span data-stu-id="3f662-189">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="3f662-190">При использовании этих свойств проекта 1.x вручную добавьте их к проекту для версии 2.0:</span><span class="sxs-lookup"><span data-stu-id="3f662-190">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

<span data-ttu-id="3f662-191">Чтобы избежать повторяющихся внешние ключи, при выполнении миграций EF Core, добавьте следующий код в вашей `IdentityDbContext` класса `OnModelCreating` метод (после `base.OnModelCreating();` вызов):</span><span class="sxs-lookup"><span data-stu-id="3f662-191">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Core Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Core Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="3f662-192">Замените GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="3f662-192">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="3f662-193">Синхронный метод `GetExternalAuthenticationSchemes` был удален, а асинхронная версия.</span><span class="sxs-lookup"><span data-stu-id="3f662-193">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="3f662-194">проекты 1.x имеется следующий код *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f662-194">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="3f662-195">Этот метод представлен в *Login.cshtml* слишком:</span><span class="sxs-lookup"><span data-stu-id="3f662-195">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="3f662-196">В проектах 2.0 используйте `GetExternalAuthenticationSchemesAsync` метод:</span><span class="sxs-lookup"><span data-stu-id="3f662-196">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="3f662-197">В *Login.cshtml*, `AuthenticationScheme` свойство с доступом в `foreach` примет цикл `Name`:</span><span class="sxs-lookup"><span data-stu-id="3f662-197">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="3f662-198">Изменение свойства ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="3f662-198">ManageLoginsViewModel property change</span></span>
<span data-ttu-id="3f662-199">Объект `ManageLoginsViewModel` объект используется в `ManageLogins` действие *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="3f662-199">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="3f662-200">В проектах версии 1.x, объект элемента `OtherLogins` свойства возвращаемого типа `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="3f662-200">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="3f662-201">Тип возвращаемого значения требует импорта `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="3f662-201">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="3f662-202">В проектах 2.0, возвращаемый тип изменения в `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="3f662-202">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="3f662-203">Этот новый тип возвращаемого значения требуется замена `Microsoft.AspNetCore.Http.Authentication` импорта с `Microsoft.AspNetCore.Authentication` импорта.</span><span class="sxs-lookup"><span data-stu-id="3f662-203">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="3f662-204">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3f662-204">Additional resources</span></span>
<span data-ttu-id="3f662-205">Дополнительные сведения и обсуждение, см. в разделе [обсуждение Auth 2.0](https://github.com/aspnet/Security/issues/1338) проблемы на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="3f662-205">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
