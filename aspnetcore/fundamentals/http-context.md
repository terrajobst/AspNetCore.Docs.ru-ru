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
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="17efa-103">Доступ к HttpContext в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="17efa-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="17efa-104">Приложения ASP.NET Core получают доступ к `HttpContext` через интерфейс [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) и его реализацию по умолчанию — [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="17efa-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="17efa-105">Использование HttpContext через Razor Pages</span><span class="sxs-lookup"><span data-stu-id="17efa-105">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="17efa-106">Класс [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) в Razor Pages предоставляет свойство [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext):</span><span class="sxs-lookup"><span data-stu-id="17efa-106">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="17efa-107">Использование HttpContext через контроллер</span><span class="sxs-lookup"><span data-stu-id="17efa-107">Use HttpContext from a controller</span></span>

<span data-ttu-id="17efa-108">Контроллеры предоставляют свойство [ControllerBase.HttpContex](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext):</span><span class="sxs-lookup"><span data-stu-id="17efa-108">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="17efa-109">Использование HttpContext через ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="17efa-109">Use HttpContext from middleware</span></span>

<span data-ttu-id="17efa-110">При работе с компонентами пользовательского ПО промежуточного слоя свойство `HttpContext` передается в метод `Invoke` или `InvokeAsync` и доступ к нему может осуществляться при настройке ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="17efa-110">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="17efa-111">Использование HttpContext через пользовательские компоненты</span><span class="sxs-lookup"><span data-stu-id="17efa-111">Use HttpContext from custom components</span></span>

<span data-ttu-id="17efa-112">Для других компонентов платформы и пользовательских компонентов, которым требуется доступ к `HttpContext`, рекомендуется зарегистрировать зависимость с помощью встроенного контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="17efa-112">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="17efa-113">Контейнер внедрения зависимостей предоставляет свойство `IHttpContextAccessor` для всех классов, которые объявляют его как зависимость в своих конструкторах.</span><span class="sxs-lookup"><span data-stu-id="17efa-113">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

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

<span data-ttu-id="17efa-114">В предшествующем примере:</span><span class="sxs-lookup"><span data-stu-id="17efa-114">In the preceding example:</span></span>

* <span data-ttu-id="17efa-115">`UserRepository` объявляет зависимость от `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="17efa-115">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="17efa-116">Зависимость предоставляется, если внедрение зависимостей разрешает цепочку зависимостей и создает экземпляр класса `UserRepository`.</span><span class="sxs-lookup"><span data-stu-id="17efa-116">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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
