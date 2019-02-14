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
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>Авторизация с определенной схемой в ASP.NET Core

В некоторых сценариях, таких как одностраничные приложения (SPA) довольно часто для использования нескольких методов проверки подлинности. Например приложение может использовать проверку подлинности на основе файлов cookie для входа и проверки подлинности носителя JWT для запросов JavaScript. В некоторых случаях приложение может иметь несколько экземпляров обработчика проверки подлинности. Например два обработчика файлов cookie, где один содержит базовой идентификации и один создается при активации многофакторной проверки подлинности (MFA). Могут быть предприняты многофакторной проверки Подлинности, поскольку пользователь запросил операцию, которая требует дополнительной безопасности.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

При настройке службы проверки подлинности во время проверки подлинности с именем схемы проверки подлинности. Пример:

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

В приведенном выше коде, были добавлены два обработчика проверки подлинности: один для файлов cookie и один для носителя.

>[!NOTE]
>Указывает схему по умолчанию приводит `HttpContext.User` свойство этому удостоверению. Если такое поведение не требуется, отключите его путем вызова без параметров вида `AddAuthentication`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Схемы проверки подлинности присваиваются в том случае, если по промежуточного слоя проверки подлинности настраиваются во время проверки подлинности. Пример:

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

В приведенном выше коде, были добавлены два компонента проверки подлинности: один для файлов cookie и один для носителя.

>[!NOTE]
>Указывает схему по умолчанию приводит `HttpContext.User` свойство этому удостоверению. Если такое поведение не требуется, отключить, задав `AuthenticationOptions.AutomaticAuthenticate` свойства `false`.

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Выбор схемы с помощью атрибута авторизации

При авторизации приложение указывает обработчик для использования. Выберите обработчик, с которым приложение будет авторизовать путем передачи разделенный запятыми список схем проверки подлинности `[Authorize]`. `[Authorize]` Атрибут указывает схему проверки подлинности или схемы, чтобы использовать независимо от того, настроена ли по умолчанию. Пример:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

В приведенном выше примере носителя и файл cookie обработчики запуска и иметь возможность создать и добавить удостоверение для текущего пользователя. Указав только единственную схему, выполняется соответствующий обработчик.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

В приведенном выше коде выполняется только обработчик с использованием схемы «Bearer». Удостоверения на основе файлов cookie учитываются.

## <a name="selecting-the-scheme-with-policies"></a>Выбор схемы с помощью политик

Если вы хотите задать нужный схем в [политики](xref:security/authorization/policies), можно задать `AuthenticationSchemes` коллекции при добавлении политики:

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

В приведенном выше примере «Over18» политики выполняется только от удостоверения, созданного с помощью обработчика «Bearer». Используйте политику, задав `[Authorize]` атрибута `Policy` свойство:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a>Использование нескольких схем проверки подлинности

Некоторые приложения может потребоваться поддержка нескольких типов проверки подлинности. Например приложение может проверять подлинность пользователей из Azure Active Directory и из базы данных пользователей. Другой пример — приложение, которое выполняет проверку подлинности пользователей служб федерации Active Directory и Azure Active Directory B2C. В этом случае приложение должно принимать токена носителя JWT от нескольких поставщиков.

Добавьте все схемы проверки подлинности, которые вы хотите принять. Например, следующий код в `Startup.ConfigureServices` добавляет две схемы проверки подлинности носителя JWT с помощью различных поставщиков:

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
> Схема проверки подлинности по умолчанию зарегистрирован только один проверки подлинности носителя JWT `JwtBearerDefaults.AuthenticationScheme`. Дополнительная проверка подлинности должен быть зарегистрирован с уникальной аутентификацией на основе схемы.

Следующим шагом является обновление политики авторизации по умолчанию для приема обе схемы проверки подлинности. Пример:

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

Как политика авторизации по умолчанию переопределяется, это можно использовать `[Authorize]` атрибутом в контроллерах. Затем контроллер принимает запросы с маркер JWT, выданный первого или второго поставщика.

::: moniker-end
