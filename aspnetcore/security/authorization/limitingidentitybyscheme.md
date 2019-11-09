---
title: Авторизация с помощью определенной схемы в ASP.NET Core
author: rick-anderson
description: В этой статье объясняется, как ограничить идентификацию определенной схемой при работе с несколькими методами проверки подлинности.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 38da80519b9d5d097c24d38b5a37503174629fc4
ms.sourcegitcommit: 4818385c3cfe0805e15138a2c1785b62deeaab90
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896971"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>Авторизация с помощью определенной схемы в ASP.NET Core

В некоторых сценариях, таких как одностраничные приложения (одностраничные приложения), обычно используется несколько методов проверки подлинности. Например, приложение может использовать проверку подлинности на основе файлов cookie для входа и аутентификации JWT Bearer для запросов JavaScript. В некоторых случаях приложение может иметь несколько экземпляров обработчика проверки подлинности. Например, два обработчика файлов cookie, где один из них содержит базовое удостоверение и создается при активации многофакторной проверки подлинности (MFA). MFA может активироваться, так как пользователь запросил операцию, требующую дополнительной защиты.

Схема проверки подлинности называется, когда служба проверки подлинности настроена во время проверки подлинности. Пример:

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

В приведенном выше коде были добавлены два обработчика проверки подлинности: один для файлов cookie и один для носителя.

>[!NOTE]
>При указании схемы по умолчанию задается свойство `HttpContext.User`, для которого задано это удостоверение. Если такое поведение не требуется, отключите его, вызвав форму `AddAuthentication`без параметров.

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Выбор схемы с помощью атрибута авторизации

В точке авторизации приложение указывает, какой обработчик следует использовать. Выберите обработчик, с помощью которого приложение будет выполнять авторизацию, передав в `[Authorize]`список схем проверки подлинности с разделителями-запятыми. Атрибут `[Authorize]` указывает схему или схемы проверки подлинности, которые будут использоваться независимо от того, настроено ли значение по умолчанию. Пример:

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

В предыдущем примере выполняются обработчики файлов cookie и носителя, а также возможность создания и добавления удостоверения для текущего пользователя. Если указать только одну схему, выполняется соответствующий обработчик.

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

В приведенном выше коде выполняется только обработчик со схемой "Bearer". Все удостоверения на основе файлов cookie игнорируются.

## <a name="selecting-the-scheme-with-policies"></a>Выбор схемы с политиками

Если вы предпочитаете указывать нужные схемы в [политике](xref:security/authorization/policies), можно задать `AuthenticationSchemes` коллекцию при добавлении политики.

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

В предыдущем примере политика "Over18" выполняется только для удостоверения, созданного обработчиком "Bearer". Используйте политику, задав свойство `Policy` атрибута `[Authorize]`:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a>Использование нескольких схем проверки подлинности

Некоторым приложениям может потребоваться поддержка нескольких типов проверки подлинности. Например, приложение может выполнять проверку подлинности пользователей из Azure Active Directory и из базы данных пользователей. Другим примером является приложение, которое выполняет проверку подлинности пользователей как с службы федерации Active Directory (AD FS), так Azure Active Directory B2C. В этом случае приложение должно принять токен носителя JWT из нескольких издателей.

Добавьте все схемы проверки подлинности, которые вы хотите принять. Например, следующий код в `Startup.ConfigureServices` добавляет две схемы проверки подлинности носителя JWT с разными издателями:

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
> Только одна проверка подлинности носителя JWT регистрируется в схеме проверки подлинности по умолчанию `JwtBearerDefaults.AuthenticationScheme`. Дополнительную проверку подлинности необходимо зарегистрировать с помощью уникальной схемы проверки подлинности.

Следующим шагом является обновление политики авторизации по умолчанию для принятия обеих схем проверки подлинности. Пример:

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

По мере переопределения политики авторизации по умолчанию можно использовать атрибут `[Authorize]` в контроллерах. Затем контроллер принимает запросы с маркером JWT, выданным первым или вторым издателем.

::: moniker-end
