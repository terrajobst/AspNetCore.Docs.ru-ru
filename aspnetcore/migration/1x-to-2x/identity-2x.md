---
title: Перенести проверку подлинности и удостоверение в ASP.NET Core 2.0
author: scottaddie
description: В этой статье описаны наиболее распространенные действия для переноса 1.x ASP.NET Core проверку подлинности и удостоверение по Core ASP.NET 2.0.
manager: wpickett
ms.author: scaddie
ms.date: 10/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 0653906996f9f37d436ebefc6a738d2603788d53
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Перенести проверку подлинности и удостоверение в ASP.NET Core 2.0

По [Scott Addie](https://github.com/scottaddie) и [поздравить Hao](https://github.com/HaoK)

Ядро ASP.NET 2.0 имеет новую модель для проверки подлинности и [удостоверение](xref:security/authentication/identity) с помощью служб, который упрощает настройку. Приложения ASP.NET Core 1.x, использующие проверку подлинности или идентификаторов можно обновить для использования новой модели, как показано ниже.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>По промежуточного слоя проверки подлинности и службы
В проектах 1.x настроена проверка подлинности через по промежуточного слоя. Для каждой схемы проверки подлинности, которые требуется поддерживать вызывается метод по промежуточного слоя.

В следующем примере 1.x настраивает проверку подлинности Facebook с удостоверения *файла Startup.cs*:

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

В проектах на 2.0 через службы настроена проверка подлинности. Каждая схема проверки подлинности, зарегистрированный в `ConfigureServices` метод *файла Startup.cs*. `UseIdentity` Заменяется метод `UseAuthentication`.

Пример 2.0 настройки проверки подлинности Facebook с удостоверения *файла Startup.cs*:

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

`UseAuthentication` Метод добавляет компонент по промежуточного слоя одной проверки подлинности, который отвечает за автоматической проверки подлинности и обработки запросов на удаленную проверку подлинности. Он заменяет все компоненты отдельных по промежуточного слоя единого компонент по промежуточного слоя.

Ниже приведены 2.0 инструкции по миграции для каждой схемы основной проверки подлинности.

### <a name="cookie-based-authentication"></a>Проверка подлинности на основе файлов cookie
Выберите один из приведенных ниже параметров и внесите необходимые изменения в *файла Startup.cs*:

1. Использование файлов cookie с удостоверением
    - Замените `UseIdentity` с `UseAuthentication` в `Configure` метод:

        ```csharp
        app.UseAuthentication();
        ```

    - Вызвать `AddIdentity` метод в `ConfigureServices` метод, чтобы добавить службы проверки подлинности файла cookie.
    - При необходимости вызывать `ConfigureApplicationCookie` или `ConfigureExternalCookie` метод в `ConfigureServices` метод, чтобы настроить параметры файлов cookie удостоверений.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Использование файлов cookie без идентификатора
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
Внесите следующие изменения в *файла Startup.cs*:
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

    Этот фрагмент кода не использует удостоверение, чтобы задать схему по умолчанию, передав `JwtBearerDefaults.AuthenticationScheme` для `AddAuthentication` метод.

### <a name="openid-connect-oidc-authentication"></a>Проверка подлинности подключения OpenID (OIDC)
Внесите следующие изменения в *файла Startup.cs*:

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
Внесите следующие изменения в *файла Startup.cs*:
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
Внесите следующие изменения в *файла Startup.cs*:
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
Внесите следующие изменения в *файла Startup.cs*:
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
Внесите следующие изменения в *файла Startup.cs*:
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
В 1.x `AutomaticAuthenticate` и `AutomaticChallenge` свойства [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) базового класса должны устанавливаться на одна схема проверки подлинности. Произошла хороший способ контроля.

В версии 2.0, эти свойства будут удалены как свойства в отдельных `AuthenticationOptions` экземпляра. Они могут быть настроены в `AddAuthentication` вызова метода внутри `ConfigureServices` метод *файла Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

В предыдущем фрагменте кода схемы по умолчанию имеет значение `CookieAuthenticationDefaults.AuthenticationScheme` («файлы cookie»).

Кроме того, используйте перегруженную версию `AddAuthentication` метод, чтобы задать более одного свойства. В следующем примере перегруженный метод схемы по умолчанию выбрана `CookieAuthenticationDefaults.AuthenticationScheme`. Схема проверки подлинности можно также может быть указан в пределах индивидуальными `[Authorize]` атрибутов или политики авторизации.

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Определите схема по умолчанию в версии 2.0, если выполняется одно из следующих условий:
- Пользователю необходимо автоматически войти в
- Вы используете `[Authorize]` атрибута или авторизации политики без указания схемы

Исключением из этого правила является `AddIdentity` метод. Этот метод добавляет файлы cookie для вас и задает значение по умолчанию проверки подлинности и проверку схемы в файл cookie приложения `IdentityConstants.ApplicationScheme`. Кроме того, он задает схему по умолчанию вход внешний файл cookie `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Используйте HttpContext модули проверки подлинности
`IAuthenticationManager` Интерфейс — Главная точка входа в систему проверки подлинности 1.x. Он был заменен с новым набором `HttpContext` методы расширения в `Microsoft.AspNetCore.Authentication` пространства имен.

Например, 1.x проекты ссылки `Authentication` свойство:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

В проектах на 2.0 импортируйте `Microsoft.AspNetCore.Authentication` пространства имен и удалите `Authentication` ссылок на свойства:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Проверка подлинности Windows (HTTP.sys / IISIntegration)
Существует два типа проверки подлинности Windows.
1. Узел позволяет только прошедшие проверку
2. Узел позволяет как анонимные, а проверка подлинности пользователей

Первый вариант, описанных выше 2.0 изменения не влияют.

Второй вариант, описанных выше влияют изменения 2.0. Например, вам может разрешение анонимных пользователей в приложение на IIS или [HTTP.sys](xref:fundamentals/servers/weblistener) слоя но авторизацию пользователей на уровне контроллера. В этом случае значение схемы по умолчанию `IISDefaults.AuthenticationScheme` в `ConfigureServices` метод *файла Startup.cs*:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

Не удалось задать схему по умолчанию соответствующим образом предотвращает запроса authorize проверить работу.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>Экземпляры IdentityCookieOptions
Побочный эффект изменений 2.0 является параметром для использование именованных параметров вместо экземпляров параметры файлов cookie. Позволяет настраивать имена схем идентификатора файла cookie удаляется.

Например, 1.x проекты использующие [внедрение конструктора](xref:mvc/controllers/dependency-injection#constructor-injection) для передачи `IdentityCookieOptions` параметр в *AccountController.cs*. Схему проверки подлинности внешних файлов cookie осуществляется из предоставленного экземпляра.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

Внедрение упомянутой выше конструктора необязательной 2.0 проектах и `_externalCookieScheme` удаление поля:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

`IdentityConstants.ExternalScheme` Константа может быть использована напрямую:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Добавить IdentityUser POCO свойства навигации
Entity Framework (EF) основные свойства навигации базового `IdentityUser` POCO (Plain объект среды CLR) были удалены. При использовании этих свойств проекта 1.x вручную добавьте их к проекту для версии 2.0:

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

Для предотвращения повторяющихся внешних ключей при выполнении миграции Core EF, добавьте следующий код в вашей `IdentityDbContext` класса `OnModelCreating` метод (после `base.OnModelCreating();` вызова):

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Identity table names and more.
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
Синхронный метод `GetExternalAuthenticationSchemes` был удален в пользу асинхронную версию. проекты 1.x имеется следующий код *ManageController.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Этот метод представлен в *Login.cshtml* слишком:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

В проектах 2.0, используйте `GetExternalAuthenticationSchemesAsync` метод:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

В *Login.cshtml*, `AuthenticationScheme` свойство с доступом в `foreach` примет цикла `Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Изменение свойства ManageLoginsViewModel
Объект `ManageLoginsViewModel` объект используется в `ManageLogins` действие *ManageController.cs*. 1.x проектах объект элемента `OtherLogins` свойство возвращаемого типа `IList<AuthenticationDescription>`. Этот тип возвращаемого значения требуется импорте `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

В проектах 2.0, возвращаемый тип изменения в `IList<AuthenticationScheme>`. Этот новый тип возврата требуется замена `Microsoft.AspNetCore.Http.Authentication` импорта с `Microsoft.AspNetCore.Authentication` импорта.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Дополнительные ресурсы
Дополнительные сведения и описание см. в разделе [обсуждение для проверки подлинности 2.0](https://github.com/aspnet/Security/issues/1338) проблемы на сайте GitHub.
