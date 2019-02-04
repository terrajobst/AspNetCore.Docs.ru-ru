---
title: Доступ к HttpContext в ASP.NET Core
author: coderandhiker
description: Сведения о получении доступа к HttpContext в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: babc637cdec8590ac14f7924c17e862e5b2f6a81
ms.sourcegitcommit: d22b3c23c45a076c4f394a70b1c8df2fbcdf656d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2019
ms.locfileid: "55428490"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="5a41b-103">Доступ к HttpContext в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5a41b-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="5a41b-104">Приложения ASP.NET Core получают доступ к `HttpContext` через интерфейс [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) и его реализацию по умолчанию — [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="5a41b-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span> <span data-ttu-id="5a41b-105">`IHttpContextAccessor` требуется использовать только при необходимости доступа к `HttpContext` внутри службы.</span><span class="sxs-lookup"><span data-stu-id="5a41b-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="5a41b-106">Использование HttpContext через Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5a41b-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="5a41b-107">Класс [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) в Razor Pages предоставляет свойство [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext):</span><span class="sxs-lookup"><span data-stu-id="5a41b-107">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="5a41b-108">Использование HttpContext из представления Razor</span><span class="sxs-lookup"><span data-stu-id="5a41b-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="5a41b-109">Представления Razor предоставляют `HttpContext` непосредственно через свойство [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context).</span><span class="sxs-lookup"><span data-stu-id="5a41b-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) property on the view.</span></span> <span data-ttu-id="5a41b-110">В следующем примере имя текущего пользователя в приложении интрасети извлекается с использованием проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="5a41b-110">The following example retrieves the current username in an Intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="5a41b-111">Использование HttpContext через контроллер</span><span class="sxs-lookup"><span data-stu-id="5a41b-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="5a41b-112">Контроллеры предоставляют свойство [ControllerBase.HttpContex](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext):</span><span class="sxs-lookup"><span data-stu-id="5a41b-112">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="5a41b-113">Использование HttpContext через ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="5a41b-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="5a41b-114">При работе с компонентами пользовательского ПО промежуточного слоя свойство `HttpContext` передается в метод `Invoke` или `InvokeAsync` и доступ к нему может осуществляться при настройке ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="5a41b-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="5a41b-115">Использование HttpContext через пользовательские компоненты</span><span class="sxs-lookup"><span data-stu-id="5a41b-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="5a41b-116">Для других компонентов платформы и пользовательских компонентов, которым требуется доступ к `HttpContext`, рекомендуется зарегистрировать зависимость с помощью встроенного контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5a41b-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="5a41b-117">Контейнер внедрения зависимостей предоставляет свойство `IHttpContextAccessor` для всех классов, которые объявляют его как зависимость в своих конструкторах.</span><span class="sxs-lookup"><span data-stu-id="5a41b-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

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

<span data-ttu-id="5a41b-118">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="5a41b-118">In the following example:</span></span>

* <span data-ttu-id="5a41b-119">`UserRepository` объявляет зависимость от `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="5a41b-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="5a41b-120">Зависимость предоставляется, если внедрение зависимостей разрешает цепочку зависимостей и создает экземпляр класса `UserRepository`.</span><span class="sxs-lookup"><span data-stu-id="5a41b-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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
