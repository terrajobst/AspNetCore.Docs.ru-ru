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
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Миграция проверки подлинности и удостоверения в ASP.NET Core 2.0

По [Scott Addie](https://github.com/scottaddie) и [поздравить Хао](https://github.com/HaoK)

ASP.NET Core 2.0 реализована новая модель для проверки подлинности и [удостоверений](xref:security/authentication/identity) с помощью служб, который упрощает настройку. Приложения ASP.NET Core 1.x, использующие проверку подлинности или удостоверение можно обновить для использования новой модели, как описано ниже.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>По промежуточного слоя проверки подлинности и службы
В проектах версии 1.x проверки подлинности настраивается с помощью по промежуточного слоя. Для каждой схемы проверки подлинности, которые требуется поддерживать вызывается метод по промежуточного слоя.

В следующем примере 1.x настраивает проверку подлинности Facebook с использованием удостоверения *Startup.cs*:

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

В проектах 2.0 настроена проверка подлинности через службы. Каждая схема проверки подлинности регистрируется в `ConfigureServices` метод *Startup.cs*. `UseIdentity` Метод заменяется `UseAuthentication`.

Приведенный ниже 2.0 настраивает проверку подлинности Facebook с использованием удостоверения *Startup.cs*:

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

`UseAuthentication` Метод добавляет компонент по промежуточного слоя проверки подлинности, который отвечает за автоматической проверки подлинности, а также обработка запросов удаленной проверки подлинности. Он заменяет все компоненты по промежуточного слоя для отдельных единого компонент по промежуточного слоя.

Ниже приведены 2.0 инструкции по миграции для каждой схемы основной проверки подлинности.

### <a name="cookie-based-authentication"></a>Проверка подлинности на основе файлов cookie
Выберите один из приведенных ниже параметров и внесите необходимые изменения в *Startup.cs*:

1. Использование файлов cookie с удостоверением
    - Замените `UseIdentity` с `UseAuthentication` в `Configure` метод:

        ```csharp
        app.UseAuthentication();
        ```

    - Вызвать `AddIdentity` метод в `ConfigureServices` метод для добавления службы проверки подлинности файла cookie.
    - При необходимости вызывать `ConfigureApplicationCookie` или `ConfigureExternalCookie` метод в `ConfigureServices` метод, чтобы настроить параметры файла cookie удостоверений.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Использование файлов cookie без Identity
    - Замените `UseCookieAuthentication` вызов метода `Configure` метод с `UseAuthentication`:
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - Вызвать `AddAuthentication` и `AddCookie` методы в `ConfigureServices` метод:

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

### <a name="jwt-bearer-authentication"></a>Проверки подлинности носителя JWT
Внесите следующие изменения в *Startup.cs*:
- Замените `UseJwtBearerAuthentication` вызов метода `Configure` метод с `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Вызвать `AddJwtBearer` метод в `ConfigureServices` метод:

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    В этом фрагменте кода не использует удостоверение, поэтому схема по умолчанию задается путем передачи `JwtBearerDefaults.AuthenticationScheme` для `AddAuthentication` метод.

### <a name="openid-connect-oidc-authentication"></a>Проверки подлинности OpenID Connect (OIDC)
Внесите следующие изменения в *Startup.cs*:

- Замените `UseOpenIdConnectAuthentication` вызов метода `Configure` метод с `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Вызвать `AddOpenIdConnect` метод в `ConfigureServices` метод:

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

### <a name="facebook-authentication"></a>Проверка подлинности Facebook
Внесите следующие изменения в *Startup.cs*:
- Замените `UseFacebookAuthentication` вызов метода `Configure` метод с `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Вызвать `AddFacebook` метод в `ConfigureServices` метод:
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Проверка подлинности Google
Внесите следующие изменения в *Startup.cs*:
- Замените `UseGoogleAuthentication` вызов метода `Configure` метод с `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Вызвать `AddGoogle` метод в `ConfigureServices` метод:

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a>Проверка подлинности учетной записи Майкрософт
Внесите следующие изменения в *Startup.cs*:
- Замените `UseMicrosoftAccountAuthentication` вызов метода `Configure` метод с `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Вызвать `AddMicrosoftAccount` метод в `ConfigureServices` метод:

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a>Проверка подлинности Twitter
Внесите следующие изменения в *Startup.cs*:
- Замените `UseTwitterAuthentication` вызов метода `Configure` метод с `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Вызвать `AddTwitter` метод в `ConfigureServices` метод:

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Параметр схемы проверки подлинности по умолчанию
В версии 1.x `AutomaticAuthenticate` и `AutomaticChallenge` свойства [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) базового класса должны устанавливаться на основе схемы проверки подлинности. Невозможно было хорошо это обеспечить.

В версии 2.0, эти свойства будут удалены как свойства в отдельных `AuthenticationOptions` экземпляра. Они могут быть настроены в `AddAuthentication` вызова метода внутри `ConfigureServices` метод *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

В предыдущем фрагменте кода, схемой по умолчанию имеет значение `CookieAuthenticationDefaults.AuthenticationScheme` («файлы cookie»).

Кроме того, используйте перегруженную версию `AddAuthentication` метод, чтобы задать более одного свойства. В следующем примере перегруженный метод, схемой по умолчанию имеет значение `CookieAuthenticationDefaults.AuthenticationScheme`. Схема проверки подлинности в качестве альтернативы можно составить в индивидуальную `[Authorize]` атрибутов или политик авторизации.

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Определите схемой по умолчанию в версии 2.0, если выполняется одно из следующих условий:
- Требуется, чтобы пользователь мог автоматически входить в
- Использовании `[Authorize]` атрибута или авторизации политики без указания схемы

Исключением из этого правила является `AddIdentity` метод. Этот метод добавляет файлы cookie для вас и наборы по умолчанию для проверки подлинности и бросьте схемы для файла cookie приложения `IdentityConstants.ApplicationScheme`. Кроме того, он задает схему по умолчанию вход внешний файл cookie `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Использовать модули проверки подлинности HttpContext
`IAuthenticationManager` Интерфейс является главную точку входа в систему проверки подлинности 1.x. Он был заменен новым набором `HttpContext` методы расширения в `Microsoft.AspNetCore.Authentication` пространства имен.

Например, 1.x проекты ссылки `Authentication` свойство:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

В проектах 2.0 импортировать `Microsoft.AspNetCore.Authentication` пространства имен и удалите `Authentication` ссылок на свойства:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Проверка подлинности Windows (HTTP.sys / IISIntegration)
Существует два варианта проверки подлинности Windows.
1. Узел разрешает только пользователям, прошедшим проверку подлинности
2. Узел позволяет использовать объекты анонимных и прошедшие проверку пользователи

Первый вариант, описанных выше 2.0 изменения не влияют.

Второй вариант, описанных выше зависит от изменения версии 2.0. Например, вы может разрешение анонимных пользователей в приложение в IIS или [HTTP.sys](xref:fundamentals/servers/weblistener) слоя но авторизацию пользователей на уровне контроллера. В этом случае значение схемы по умолчанию `IISDefaults.AuthenticationScheme` в `ConfigureServices` метод *Startup.cs*:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

Не удалось установить схему по умолчанию соответствующим образом предотвращает запроса authorize бросить вызов работу.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>Экземпляры IdentityCookieOptions
Побочный эффект изменения 2.0 заключается в переходе на использование именованных параметров, а не к экземпляру параметры файла cookie. Возможность настраивать имена схем идентификации файл cookie удаляется.

Например, 1.x проекты использующие [внедрение через конструктор](xref:mvc/controllers/dependency-injection#constructor-injection) для передачи `IdentityCookieOptions` параметр в *AccountController.cs*. Схема проверки подлинности внешних файлов cookie осуществляется из предоставленного экземпляра.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

Упомянутые выше конструктор injection становится ненужным в проектах 2.0 и `_externalCookieScheme` поле можно удалить:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

`IdentityConstants.ExternalScheme` Константа может быть использована напрямую:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Добавление IdentityUser POCO свойства навигации
Свойства навигации Entity Framework (EF) Core базового `IdentityUser` POCO (обычные старые объекты среды CLR) будут удалены. При использовании этих свойств проекта 1.x вручную добавьте их к проекту для версии 2.0:

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

Чтобы избежать повторяющихся внешние ключи, при выполнении миграций EF Core, добавьте следующий код в вашей `IdentityDbContext` класса `OnModelCreating` метод (после `base.OnModelCreating();` вызов):

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

## <a name="replace-getexternalauthenticationschemes"></a>Замените GetExternalAuthenticationSchemes
Синхронный метод `GetExternalAuthenticationSchemes` был удален, а асинхронная версия. проекты 1.x имеется следующий код *ManageController.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Этот метод представлен в *Login.cshtml* слишком:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

В проектах 2.0 используйте `GetExternalAuthenticationSchemesAsync` метод:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

В *Login.cshtml*, `AuthenticationScheme` свойство с доступом в `foreach` примет цикл `Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Изменение свойства ManageLoginsViewModel
Объект `ManageLoginsViewModel` объект используется в `ManageLogins` действие *ManageController.cs*. В проектах версии 1.x, объект элемента `OtherLogins` свойства возвращаемого типа `IList<AuthenticationDescription>`. Тип возвращаемого значения требует импорта `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

В проектах 2.0, возвращаемый тип изменения в `IList<AuthenticationScheme>`. Этот новый тип возвращаемого значения требуется замена `Microsoft.AspNetCore.Http.Authentication` импорта с `Microsoft.AspNetCore.Authentication` импорта.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Дополнительные ресурсы
Дополнительные сведения и обсуждение, см. в разделе [обсуждение Auth 2.0](https://github.com/aspnet/Security/issues/1338) проблемы на сайте GitHub.
