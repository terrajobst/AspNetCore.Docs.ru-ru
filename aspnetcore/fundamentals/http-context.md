---
title: Доступ к HttpContext в ASP.NET Core
author: coderandhiker
description: Сведения о получении доступа к HttpContext в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: b1ff80943db1788b465accd51c70a3c3a3462d5c
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202714"
---
# <a name="access-httpcontext-in-aspnet-core"></a>Доступ к HttpContext в ASP.NET Core

Приложения ASP.NET Core получают доступ к `HttpContext` через интерфейс [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) и его реализацию по умолчанию — [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a>Использование HttpContext через Razor Pages

Класс [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) в Razor Pages предоставляет свойство [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext):

```csharp
public class AboutModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = HttpContext.Request.PathBase;
    }
}
```

::: moniker-end

## <a name="use-httpcontext-from-a-controller"></a>Использование HttpContext через контроллер

Контроллеры предоставляют свойство [ControllerBase.HttpContex](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext):

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;
        // Do something with the PathBase.

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a>Использование HttpContext через ПО промежуточного слоя

При работе с компонентами пользовательского ПО промежуточного слоя свойство `HttpContext` передается в метод `Invoke` или `InvokeAsync` и доступ к нему может осуществляться при настройке ПО промежуточного слоя:

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a>Использование HttpContext через пользовательские компоненты

Для других компонентов платформы и пользовательских компонентов, которым требуется доступ к `HttpContext`, рекомендуется зарегистрировать зависимость с помощью встроенного контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection). Контейнер внедрения зависимостей предоставляет свойство `IHttpContextAccessor` для всех классов, которые объявляют его как зависимость в своих конструкторах.

::: moniker range=">= aspnetcore-2.1"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc();
     services.AddSingleton<IHttpContextAccessor, HttpContextAccessor>();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

В предшествующем примере:

* `UserRepository` объявляет зависимость от `IHttpContextAccessor`.
* Зависимость предоставляется, если внедрение зависимостей разрешает цепочку зависимостей и создает экземпляр класса `UserRepository`.

```csharp
public class UserRepository : IUserRepository
{
    private readonly IHttpContextAccessor _httpContextAccessor;

    public UserRepository(IHttpContextAccessor httpContextAccessor)
    {
        _httpContextAccessor = httpContextAccessor;
    }

    public void LogCurrentUser()
    {
        var username = _httpContextAccessor.HttpContext.User.Identity.Name;
        service.LogAccessRequest(username);
    }
}
```
