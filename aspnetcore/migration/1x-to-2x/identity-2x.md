---
title: Перенесите проверку подлинности и удостоверение в ASP.NET Core 2,0
author: scottaddie
description: В этой статье описаны наиболее распространенные действия по переносу проверки подлинности ASP.NET Core 1. x и идентификации в ASP.NET Core 2,0.
ms.author: scaddie
ms.date: 06/21/2019
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: af905f1127d504839f66d9e0e1ca1dfc27e32772
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654946"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="72fe3-103">Перенесите проверку подлинности и удостоверение в ASP.NET Core 2,0</span><span class="sxs-lookup"><span data-stu-id="72fe3-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="72fe3-104">[Скотт Эдди (](https://github.com/scottaddie) и [Хао кунг](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="72fe3-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="72fe3-105">ASP.NET Core 2,0 имеет новую модель для проверки подлинности и [идентификации](xref:security/authentication/identity) , которая упрощает настройку с помощью служб.</span><span class="sxs-lookup"><span data-stu-id="72fe3-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) that simplifies configuration by using services.</span></span> <span data-ttu-id="72fe3-106">ASP.NET Core 1. x приложения, использующие проверку подлинности или удостоверение, можно обновить для использования новой модели, как описано ниже.</span><span class="sxs-lookup"><span data-stu-id="72fe3-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

## <a name="update-namespaces"></a><span data-ttu-id="72fe3-107">Обновить пространства имен</span><span class="sxs-lookup"><span data-stu-id="72fe3-107">Update namespaces</span></span>

<span data-ttu-id="72fe3-108">В 1. x такие классы, как `IdentityRole` и `IdentityUser`, обнаружены в пространстве имен `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="72fe3-108">In 1.x, classes such `IdentityRole` and `IdentityUser` were found in the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` namespace.</span></span>

<span data-ttu-id="72fe3-109">В 2,0 пространство имен <xref:Microsoft.AspNetCore.Identity> стало новым доменом для нескольких таких классов.</span><span class="sxs-lookup"><span data-stu-id="72fe3-109">In 2.0, the <xref:Microsoft.AspNetCore.Identity> namespace became the new home for several of such classes.</span></span> <span data-ttu-id="72fe3-110">При использовании кода удостоверения по умолчанию затронутые классы включают `ApplicationUser` и `Startup`.</span><span class="sxs-lookup"><span data-stu-id="72fe3-110">With the default Identity code, affected classes include `ApplicationUser` and `Startup`.</span></span> <span data-ttu-id="72fe3-111">Настройте инструкции `using` для разрешения затронутых ссылок.</span><span class="sxs-lookup"><span data-stu-id="72fe3-111">Adjust your `using` statements to resolve the affected references.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="72fe3-112">Промежуточное по проверки подлинности и службы</span><span class="sxs-lookup"><span data-stu-id="72fe3-112">Authentication Middleware and services</span></span>

<span data-ttu-id="72fe3-113">В проектах 1. x Проверка подлинности настраивается через по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="72fe3-113">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="72fe3-114">Для каждой схемы проверки подлинности, которую вы хотите поддерживать, вызывается метод по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="72fe3-114">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="72fe3-115">В следующем примере 1. x настраивается проверка подлинности Facebook с удостоверением в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="72fe3-115">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="72fe3-116">В проектах 2,0 проверка подлинности настраивается с помощью служб.</span><span class="sxs-lookup"><span data-stu-id="72fe3-116">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="72fe3-117">Каждая схема проверки подлинности регистрируется в методе `ConfigureServices` *Startup.CS*.</span><span class="sxs-lookup"><span data-stu-id="72fe3-117">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="72fe3-118">Метод `UseIdentity` заменяется `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="72fe3-118">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="72fe3-119">Следующий пример 2,0 настраивает проверку подлинности Facebook с помощью Identity в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="72fe3-119">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="72fe3-120">Метод `UseAuthentication` добавляет один компонент промежуточного слоя проверки подлинности, отвечающий за автоматическую проверку подлинности и обработку запросов удаленной проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="72fe3-120">The `UseAuthentication` method adds a single authentication middleware component, which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="72fe3-121">Он заменяет все отдельные компоненты по промежуточного слоя одним общим компонентом по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="72fe3-121">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="72fe3-122">Ниже приведены инструкции по миграции для каждой основной схемы проверки подлинности 2,0.</span><span class="sxs-lookup"><span data-stu-id="72fe3-122">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="72fe3-123">Проверка подлинности на основе файлов cookie</span><span class="sxs-lookup"><span data-stu-id="72fe3-123">Cookie-based authentication</span></span>

<span data-ttu-id="72fe3-124">Выберите один из двух параметров ниже и внесите необходимые изменения в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="72fe3-124">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="72fe3-125">Использование файлов cookie с удостоверением</span><span class="sxs-lookup"><span data-stu-id="72fe3-125">Use cookies with Identity</span></span>
    - <span data-ttu-id="72fe3-126">Замените `UseIdentity` `UseAuthentication` в методе `Configure`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-126">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="72fe3-127">Вызовите метод `AddIdentity` в методе `ConfigureServices`, чтобы добавить службы проверки подлинности файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="72fe3-127">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="72fe3-128">При необходимости вызовите метод `ConfigureApplicationCookie` или `ConfigureExternalCookie` в методе `ConfigureServices`, чтобы настроить параметры cookie для удостоверений.</span><span class="sxs-lookup"><span data-stu-id="72fe3-128">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="72fe3-129">Использовать файлы cookie без удостоверений</span><span class="sxs-lookup"><span data-stu-id="72fe3-129">Use cookies without Identity</span></span>
    - <span data-ttu-id="72fe3-130">Замените вызов метода `UseCookieAuthentication` в методе `Configure` на `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-130">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="72fe3-131">Вызовите методы `AddAuthentication` и `AddCookie` в методе `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-131">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="72fe3-132">Проверка подлинности носителя JWT</span><span class="sxs-lookup"><span data-stu-id="72fe3-132">JWT Bearer Authentication</span></span>

<span data-ttu-id="72fe3-133">Внесите следующие изменения в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="72fe3-133">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="72fe3-134">Замените вызов метода `UseJwtBearerAuthentication` в методе `Configure` на `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-134">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="72fe3-135">Вызовите метод `AddJwtBearer` в методе `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-135">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="72fe3-136">Этот фрагмент кода не использует удостоверение, поэтому схему по умолчанию следует задать, передав `JwtBearerDefaults.AuthenticationScheme` методу `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="72fe3-136">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="72fe3-137">Проверка подлинности OpenID Connect Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="72fe3-137">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="72fe3-138">Внесите следующие изменения в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="72fe3-138">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="72fe3-139">Замените вызов метода `UseOpenIdConnectAuthentication` в методе `Configure` на `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-139">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="72fe3-140">Вызовите метод `AddOpenIdConnect` в методе `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-140">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

- <span data-ttu-id="72fe3-141">Замените свойство `PostLogoutRedirectUri` в действии `OpenIdConnectOptions` на `SignedOutRedirectUri`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-141">Replace the `PostLogoutRedirectUri` property in the `OpenIdConnectOptions` action with `SignedOutRedirectUri`:</span></span>

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a><span data-ttu-id="72fe3-142">Аутентификация Facebook</span><span class="sxs-lookup"><span data-stu-id="72fe3-142">Facebook authentication</span></span>

<span data-ttu-id="72fe3-143">Внесите следующие изменения в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="72fe3-143">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="72fe3-144">Замените вызов метода `UseFacebookAuthentication` в методе `Configure` на `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-144">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="72fe3-145">Вызовите метод `AddFacebook` в методе `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-145">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="72fe3-146">Аутентификация Google</span><span class="sxs-lookup"><span data-stu-id="72fe3-146">Google authentication</span></span>

<span data-ttu-id="72fe3-147">Внесите следующие изменения в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="72fe3-147">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="72fe3-148">Замените вызов метода `UseGoogleAuthentication` в методе `Configure` на `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-148">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="72fe3-149">Вызовите метод `AddGoogle` в методе `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-149">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="72fe3-150">Аутентификация учетной записи Майкрософт</span><span class="sxs-lookup"><span data-stu-id="72fe3-150">Microsoft Account authentication</span></span>

<span data-ttu-id="72fe3-151">Дополнительные сведения о учетная запись Майкрософт проверки подлинности см. в [этой статье о проблемах в GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/14455).</span><span class="sxs-lookup"><span data-stu-id="72fe3-151">For more information on Microsoft account authentication, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/14455).</span></span>

<span data-ttu-id="72fe3-152">Внесите следующие изменения в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="72fe3-152">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="72fe3-153">Замените вызов метода `UseMicrosoftAccountAuthentication` в методе `Configure` на `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-153">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="72fe3-154">Вызовите метод `AddMicrosoftAccount` в методе `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-154">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="72fe3-155">Аутентификация Twitter</span><span class="sxs-lookup"><span data-stu-id="72fe3-155">Twitter authentication</span></span>

<span data-ttu-id="72fe3-156">Внесите следующие изменения в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="72fe3-156">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="72fe3-157">Замените вызов метода `UseTwitterAuthentication` в методе `Configure` на `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-157">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="72fe3-158">Вызовите метод `AddTwitter` в методе `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-158">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="72fe3-159">Настройка схем проверки подлинности по умолчанию</span><span class="sxs-lookup"><span data-stu-id="72fe3-159">Setting default authentication schemes</span></span>

<span data-ttu-id="72fe3-160">В 1. x свойства `AutomaticAuthenticate` и `AutomaticChallenge` базового класса [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) предназначены для установки в одной схеме проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="72fe3-160">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="72fe3-161">Не существовало хорошего способа принудительного применения этого.</span><span class="sxs-lookup"><span data-stu-id="72fe3-161">There was no good way to enforce this.</span></span>

<span data-ttu-id="72fe3-162">В 2,0 эти два свойства были удалены как свойства в отдельном экземпляре `AuthenticationOptions`.</span><span class="sxs-lookup"><span data-stu-id="72fe3-162">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="72fe3-163">Их можно настроить в вызове метода `AddAuthentication` в методе `ConfigureServices` *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="72fe3-163">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="72fe3-164">В предыдущем фрагменте кода схема по умолчанию имеет значение `CookieAuthenticationDefaults.AuthenticationScheme` ("cookies").</span><span class="sxs-lookup"><span data-stu-id="72fe3-164">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="72fe3-165">Кроме того, можно использовать перегруженную версию метода `AddAuthentication`, чтобы задать более одного свойства.</span><span class="sxs-lookup"><span data-stu-id="72fe3-165">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="72fe3-166">В следующем примере перегруженного метода схема по умолчанию имеет значение `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="72fe3-166">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="72fe3-167">Схема проверки подлинности также может быть указана в индивидуальных `[Authorize]` атрибутов или политиках авторизации.</span><span class="sxs-lookup"><span data-stu-id="72fe3-167">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="72fe3-168">Определите схему по умолчанию в 2,0, если выполняется одно из следующих условий.</span><span class="sxs-lookup"><span data-stu-id="72fe3-168">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="72fe3-169">Вы хотите, чтобы пользователь автоматически вошел в учетную запись</span><span class="sxs-lookup"><span data-stu-id="72fe3-169">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="72fe3-170">Использование атрибута `[Authorize]` или политик авторизации без указания схем</span><span class="sxs-lookup"><span data-stu-id="72fe3-170">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="72fe3-171">Исключением из этого правила является метод `AddIdentity`.</span><span class="sxs-lookup"><span data-stu-id="72fe3-171">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="72fe3-172">Этот метод добавляет файлы cookie для вас и задает схемы проверки подлинности и вызова по умолчанию для файла cookie приложения `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="72fe3-172">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="72fe3-173">Кроме того, он задает схему входа по умолчанию для внешнего файла cookie `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="72fe3-173">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="72fe3-174">Использование расширений проверки подлинности HttpContext</span><span class="sxs-lookup"><span data-stu-id="72fe3-174">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="72fe3-175">Интерфейс `IAuthenticationManager` является основной точкой входа в систему проверки подлинности 1. x.</span><span class="sxs-lookup"><span data-stu-id="72fe3-175">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="72fe3-176">Он был заменен новым набором методов расширения `HttpContext` в пространстве имен `Microsoft.AspNetCore.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="72fe3-176">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="72fe3-177">Например, проекты 1. x ссылаются на свойство `Authentication`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-177">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="72fe3-178">В проектах 2,0 импортируйте `Microsoft.AspNetCore.Authentication` пространство имен и удалите ссылки на свойства `Authentication`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-178">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="72fe3-179">Проверка подлинности Windows (HTTP. sys/IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="72fe3-179">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="72fe3-180">Существует два варианта проверки подлинности Windows:</span><span class="sxs-lookup"><span data-stu-id="72fe3-180">There are two variations of Windows authentication:</span></span>

* <span data-ttu-id="72fe3-181">Узел допускает только пользователей, прошедших проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="72fe3-181">The host only allows authenticated users.</span></span> <span data-ttu-id="72fe3-182">На этот вариант не влияют изменения 2,0.</span><span class="sxs-lookup"><span data-stu-id="72fe3-182">This variation isn't affected by the 2.0 changes.</span></span>
* <span data-ttu-id="72fe3-183">Узел допускает анонимные и прошедшие проверку подлинности пользователи.</span><span class="sxs-lookup"><span data-stu-id="72fe3-183">The host allows both anonymous and authenticated users.</span></span> <span data-ttu-id="72fe3-184">На этот вариант влияют изменения 2,0.</span><span class="sxs-lookup"><span data-stu-id="72fe3-184">This variation is affected by the 2.0 changes.</span></span> <span data-ttu-id="72fe3-185">Например, приложение должно разрешать анонимных пользователей на уровне [IIS](xref:host-and-deploy/iis/index) или [http. sys](xref:fundamentals/servers/httpsys) , но авторизовать пользователей на уровнях контроллера.</span><span class="sxs-lookup"><span data-stu-id="72fe3-185">For example, the app should allow anonymous users at the [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorize users at the controller level.</span></span> <span data-ttu-id="72fe3-186">В этом сценарии задайте схему по умолчанию в методе `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="72fe3-186">In this scenario, set the default scheme in the `Startup.ConfigureServices` method.</span></span>

  <span data-ttu-id="72fe3-187">Для [Microsoft. AspNetCore. Server. IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)задайте для схемы по умолчанию значение `IISDefaults.AuthenticationScheme`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-187">For [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/), set the default scheme to `IISDefaults.AuthenticationScheme`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Server.IISIntegration;

  services.AddAuthentication(IISDefaults.AuthenticationScheme);
  ```

  <span data-ttu-id="72fe3-188">Для [Microsoft. AspNetCore. Server. HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)задайте для схемы по умолчанию значение `HttpSysDefaults.AuthenticationScheme`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-188">For [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/), set the default scheme to `HttpSysDefaults.AuthenticationScheme`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Server.HttpSys;

  services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
  ```

  <span data-ttu-id="72fe3-189">Если не задать схему по умолчанию, запрос авторизации (запрос) не будет работать со следующим исключением:</span><span class="sxs-lookup"><span data-stu-id="72fe3-189">Failure to set the default scheme prevents the authorize (challenge) request from working with the following exception:</span></span>

  > <span data-ttu-id="72fe3-190">`System.InvalidOperationException`: не указан Аусентикатионсчеме, и Дефаултчалленжесчеме не найден.</span><span class="sxs-lookup"><span data-stu-id="72fe3-190">`System.InvalidOperationException`: No authenticationScheme was specified, and there was no DefaultChallengeScheme found.</span></span>

<span data-ttu-id="72fe3-191">Дополнительные сведения см. в разделе <xref:security/authentication/windowsauth>.</span><span class="sxs-lookup"><span data-stu-id="72fe3-191">For more information, see <xref:security/authentication/windowsauth>.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="72fe3-192">Экземпляры Идентитикукиеоптионс</span><span class="sxs-lookup"><span data-stu-id="72fe3-192">IdentityCookieOptions instances</span></span>

<span data-ttu-id="72fe3-193">Побочным результатом изменений 2,0 является переключение на использование именованных параметров вместо экземпляров параметров файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="72fe3-193">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="72fe3-194">Возможность настройки имен для схемы cookie удостоверения удаляется.</span><span class="sxs-lookup"><span data-stu-id="72fe3-194">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="72fe3-195">Например, проекты 1. x используют [внедрение конструктора](xref:mvc/controllers/dependency-injection#constructor-injection) для передачи параметра `IdentityCookieOptions` в *AccountController.CS* и *ManageController.CS*.</span><span class="sxs-lookup"><span data-stu-id="72fe3-195">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs* and *ManageController.cs*.</span></span> <span data-ttu-id="72fe3-196">Доступ к схеме проверки подлинности внешних файлов cookie осуществляется из предоставленного экземпляра:</span><span class="sxs-lookup"><span data-stu-id="72fe3-196">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="72fe3-197">Вышеупомянутый внедренный конструктор не нужен в проектах 2,0, и поле `_externalCookieScheme` может быть удалено:</span><span class="sxs-lookup"><span data-stu-id="72fe3-197">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="72fe3-198">в проектах 1. x используется поле `_externalCookieScheme` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="72fe3-198">1.x projects used the `_externalCookieScheme` field as follows:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="72fe3-199">В проектах 2,0 замените приведенный выше код следующим.</span><span class="sxs-lookup"><span data-stu-id="72fe3-199">In 2.0 projects, replace the preceding code with the following.</span></span> <span data-ttu-id="72fe3-200">Константу `IdentityConstants.ExternalScheme` можно использовать напрямую.</span><span class="sxs-lookup"><span data-stu-id="72fe3-200">The `IdentityConstants.ExternalScheme` constant can be used directly.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="72fe3-201">Разрешите вновь добавленные `SignOutAsync` вызов, импортировав следующее пространство имен:</span><span class="sxs-lookup"><span data-stu-id="72fe3-201">Resolve the newly added `SignOutAsync` call by importing the following namespace:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="72fe3-202">Добавление свойств навигации Идентитюсер POCO</span><span class="sxs-lookup"><span data-stu-id="72fe3-202">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="72fe3-203">Свойства навигации ядра Entity Framework (EF) базового `IdentityUser` POCO (простой старый объект CLR) были удалены.</span><span class="sxs-lookup"><span data-stu-id="72fe3-203">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="72fe3-204">Если проект 1. x использовал эти свойства, вручную добавьте их обратно в проект 2,0:</span><span class="sxs-lookup"><span data-stu-id="72fe3-204">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="72fe3-205">Чтобы предотвратить дублирование внешних ключей при выполнении миграции EF Core, добавьте следующий объект в метод `OnModelCreating` класса `IdentityDbContext` (после вызова `base.OnModelCreating();`):</span><span class="sxs-lookup"><span data-stu-id="72fe3-205">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="72fe3-206">Заменить Жетекстерналаусентикатионсчемес</span><span class="sxs-lookup"><span data-stu-id="72fe3-206">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="72fe3-207">Синхронный метод `GetExternalAuthenticationSchemes` был удален в пользу асинхронной версии.</span><span class="sxs-lookup"><span data-stu-id="72fe3-207">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="72fe3-208">проекты 1. x имеют следующий код в *Controllers/манажеконтроллер. CS*:</span><span class="sxs-lookup"><span data-stu-id="72fe3-208">1.x projects have the following code in *Controllers/ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="72fe3-209">Этот метод отображается в *представлениях, учетной записи или имени входа. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="72fe3-209">This method appears in *Views/Account/Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

<span data-ttu-id="72fe3-210">В проектах 2,0 используйте метод <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*>.</span><span class="sxs-lookup"><span data-stu-id="72fe3-210">In 2.0 projects, use the <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> method.</span></span> <span data-ttu-id="72fe3-211">Изменение в *ManageController.CS* напоминает следующий код:</span><span class="sxs-lookup"><span data-stu-id="72fe3-211">The change in *ManageController.cs* resembles the following code:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="72fe3-212">В *Login. cshtml*свойство `AuthenticationScheme`, доступ к которому осуществляется в `foreach`ном цикле, изменяется на `Name`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-212">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="72fe3-213">Изменение свойства Манажелогинсвиевмодел</span><span class="sxs-lookup"><span data-stu-id="72fe3-213">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="72fe3-214">Объект `ManageLoginsViewModel` используется в `ManageLogins` действии *ManageController.CS*.</span><span class="sxs-lookup"><span data-stu-id="72fe3-214">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="72fe3-215">В проектах 1. x возвращаемым типом свойства `OtherLogins` объекта является `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="72fe3-215">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="72fe3-216">Этот тип возвращаемого значения требует импорта `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="72fe3-216">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="72fe3-217">В проектах 2,0 тип возвращаемого значения изменяется на `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="72fe3-217">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="72fe3-218">Этот новый тип возвращаемого значения требует замены `Microsoft.AspNetCore.Http.Authentication` импорта `Microsoft.AspNetCore.Authentication` импорта.</span><span class="sxs-lookup"><span data-stu-id="72fe3-218">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="72fe3-219">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="72fe3-219">Additional resources</span></span>

<span data-ttu-id="72fe3-220">Дополнительные сведения см. в [обсуждении проблемы с проверкой Подлинности 2,0](https://github.com/aspnet/Security/issues/1338) на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="72fe3-220">For more information, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
