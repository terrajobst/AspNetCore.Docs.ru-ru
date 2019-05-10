---
title: Доступ к HttpContext в ASP.NET Core
author: coderandhiker
description: Сведения о получении доступа к HttpContext в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: 373c036e0839ce51259e23f8503fbe4691b48751
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64886519"
---
# <a name="access-httpcontext-in-aspnet-core"></a>Доступ к HttpContext в ASP.NET Core

Приложения ASP.NET Core получают доступ к `HttpContext` через интерфейс [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) и его реализацию по умолчанию — [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor). `IHttpContextAccessor` требуется использовать только при необходимости доступа к `HttpContext` внутри службы.

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

## <a name="use-httpcontext-from-a-razor-view"></a>Использование HttpContext из представления Razor

Представления Razor предоставляют `HttpContext` непосредственно через свойство [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context). В следующем примере имя текущего пользователя в приложении интрасети извлекается с использованием проверки подлинности Windows.

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

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

В следующем примере:

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

## <a name="httpcontext-access-from-a-background-thread"></a>Доступ к HttpContext из фонового потока

`HttpContext` не является потокобезопасным. Чтение или запись свойств `HttpContext` за пределами обработки запроса может привести к `NullReferenceException`.

> [!NOTE]
> Использование `HttpContext` за пределами обработки запроса часто приводит к `NullReferenceException`. Если приложение генерирует случайные `NullReferenceException`, просмотрите части кода, которые запускают фоновую обработку или продолжают обработку после выполнения запроса. Ищите ошибки, например определение метода контроллера в качестве `async void`.

Для безопасного выполнения фоновой работы с данными `HttpContext` сделайте следующее.

* Скопируйте необходимые данные во время обработки запроса.
* Передайте скопированные данные в фоновую задачу.

Чтобы избежать небезопасного кода, никогда не передавайте `HttpContext` в метод, который выполняет фоновую работу. Вместо этого передайте нужные данные.

```csharp
public class EmailController
{
    public ActionResult SendEmail(string email)
    {
        var correlationId = HttpContext.Request.Headers["x-correlation-id"].ToString();

        // Starts sending an email, but doesn't wait for it to complete
        _ = SendEmailCore(correlationId);
        return View();
    }

    private async Task SendEmailCore(string correlationId)
    {
        // send the email
    }
}
