---
title: Авторизация в нужной раскладки в ASP.NET Core
author: rick-anderson
description: В этой статье объясняется, как ограничить удостоверение для нужной раскладки при работе с несколькими методами проверки подлинности.
ms.author: riande
ms.date: 10/12/2017
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 231c664006ee7ff91f471aa8d16c1fd18dcbabb1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278204"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>Авторизация в нужной раскладки в ASP.NET Core

В некоторых сценариях, например приложений на одной странице (SPAs) обычно используется несколько методов проверки подлинности. Например приложение может использовать проверку подлинности на основе файлов cookie для входа и проверки подлинности носителя JWT для запросов JavaScript. В некоторых случаях приложение может иметь несколько экземпляров обработчик проверки подлинности. Например два обработчика куки-файл, где один содержит основные идентификаторов и один создается при инициирована многофакторная проверка подлинности (MFA). Могут быть предприняты многофакторной проверки Подлинности, поскольку пользователь запросил операцию, которая требует дополнительной безопасности.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Имя схемы проверки подлинности — при настройке службы проверки подлинности во время проверки подлинности. Пример:

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

В приведенном выше коде были добавлены два обработчики проверки подлинности: один для файлов cookie и один для носителя.

>[!NOTE]
>Указание схемы по умолчанию приводит к `HttpContext.User` свойства, задаваемого этому удостоверению. Если такое поведение не требуется, отключите его с вызова без параметров форме `AddAuthentication`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Схемы проверки подлинности-это именованные middlewares проверки подлинности настраиваются во время проверки подлинности. Пример:

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

В приведенном выше коде были добавлены два middlewares проверки подлинности: один для файлов cookie и один для носителя.

>[!NOTE]
>Указание схемы по умолчанию приводит к `HttpContext.User` свойства, задаваемого этому удостоверению. Если такое поведение не требуется, отключить, задав `AuthenticationOptions.AutomaticAuthenticate` свойства `false`.

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>При выборе схемы с атрибутом авторизовать

Во время авторизации приложение сообщает, что обработчик для использования. Выберите обработчик, с помощью которого приложение будет авторизации, передавая список с разделителями запятыми схем проверки подлинности для `[Authorize]`. `[Authorize]` Атрибут указывает схему проверки подлинности или схем, которые будут использовать независимо от того, настроена ли значение по умолчанию. Пример:

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

В предыдущем примере носителя и файл cookie обработчики запуска и иметь возможность создания и добавления удостоверение для текущего пользователя. Указав только одну схему, выполняется соответствующий обработчик.

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

В приведенном выше коде выполняется обработчик, со схемой «Bearer». Всех удостоверений на основе файлов cookie учитываются.

## <a name="selecting-the-scheme-with-policies"></a>При выборе схемы с помощью политик

Чтобы задать нужный схем в [политики](xref:security/authorization/policies), можно задать `AuthenticationSchemes` коллекции при добавлении политики:

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

В предыдущем примере политики «Over18» выполняется только для идентификаторов, созданные с помощью обработчика «Bearer». Используйте политику, задав `[Authorize]` атрибута `Policy` свойство:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
