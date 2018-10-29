---
title: Активация ПО промежуточного слоя на основе фабрики в ASP.NET Core
author: guardrex
description: В этой статье приводятся сведения о том, как использовать строго типизированное ПО промежуточного слоя с реализацией активации на основе фабрики в ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2018
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 566a5c5f642a3f55e72a8e070c69d2bfddaee3a1
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207203"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="4da9d-103">Активация ПО промежуточного слоя на основе фабрики в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4da9d-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="4da9d-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="4da9d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4da9d-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) — это точка расширяемости для активации [ПО промежуточного слоя](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="4da9d-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="4da9d-106">Методы расширения `UseMiddleware` проверяют, реализует ли зарегистрированный тип ПО промежуточного слоя `IMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="4da9d-106">`UseMiddleware` extension methods check if a middleware's registered type implements `IMiddleware`.</span></span> <span data-ttu-id="4da9d-107">Если да, то экземпляр `IMiddlewareFactory`, зарегистрированный в контейнере, используется для разрешения реализации `IMiddleware` вместо стандартной логики активации ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="4da9d-107">If it does, the `IMiddlewareFactory` instance registered in the container is used to resolve the `IMiddleware` implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="4da9d-108">ПО промежуточного слоя регистрируется как ограниченная или временная служба в контейнере служб приложения.</span><span class="sxs-lookup"><span data-stu-id="4da9d-108">The middleware is registered as a scoped or transient service in the app's service container.</span></span>

<span data-ttu-id="4da9d-109">Преимущества:</span><span class="sxs-lookup"><span data-stu-id="4da9d-109">Benefits:</span></span>

* <span data-ttu-id="4da9d-110">Активация по запросу (внедрение ограниченных служб)</span><span class="sxs-lookup"><span data-stu-id="4da9d-110">Activation per request (injection of scoped services)</span></span>
* <span data-ttu-id="4da9d-111">Строгая типизация ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="4da9d-111">Strong typing of middleware</span></span>

<span data-ttu-id="4da9d-112">`IMiddleware` активируется по запросу, благодаря чему ограниченные (scoped) службы могут внедряться в конструктор ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="4da9d-112">`IMiddleware` is activated per request, so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="4da9d-113">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4da9d-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4da9d-114">В примере приложения показана активация ПО промежуточного слоя следующими способами:</span><span class="sxs-lookup"><span data-stu-id="4da9d-114">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="4da9d-115">Стандартный.</span><span class="sxs-lookup"><span data-stu-id="4da9d-115">Convention.</span></span> <span data-ttu-id="4da9d-116">Дополнительные сведения о стандартной активации ПО промежуточного слоя см. [здесь](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="4da9d-116">For more information on conventional middleware activation, see the [Middleware](xref:fundamentals/middleware/index) topic.</span></span>
* <span data-ttu-id="4da9d-117">Реализация [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware).</span><span class="sxs-lookup"><span data-stu-id="4da9d-117">An [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="4da9d-118">По умолчанию ПО промежуточного слоя активируется [классом MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory).</span><span class="sxs-lookup"><span data-stu-id="4da9d-118">The default [MiddlewareFactory class](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) activates the middleware.</span></span>

<span data-ttu-id="4da9d-119">Реализации ПО промежуточного слоя функционируют идентичным образом и записывают значение, предоставленное в параметре строки запроса (`key`).</span><span class="sxs-lookup"><span data-stu-id="4da9d-119">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="4da9d-120">ПО промежуточного слоя использует внедренный контекст базы данных (ограниченная служба) для записи значения строки запроса в базу данных, выполняющуюся в памяти.</span><span class="sxs-lookup"><span data-stu-id="4da9d-120">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

## <a name="imiddleware"></a><span data-ttu-id="4da9d-121">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="4da9d-121">IMiddleware</span></span>

<span data-ttu-id="4da9d-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) определяет ПО промежуточного слоя для конвейера запросов приложения.</span><span class="sxs-lookup"><span data-stu-id="4da9d-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="4da9d-123">Метод [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) обрабатывает запрос и возвращает объект `Task`, который представляет выполнение ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="4da9d-123">The [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) method handles requests and returns a `Task` that represents the execution of the middleware.</span></span>

<span data-ttu-id="4da9d-124">Стандартная активация ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="4da9d-124">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="4da9d-125">Активация ПО промежуточного слоя с помощью `MiddlewareFactory`:</span><span class="sxs-lookup"><span data-stu-id="4da9d-125">Middleware activated by `MiddlewareFactory`:</span></span>

[!code-csharp[](extensibility/sample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="4da9d-126">Расширения, создаваемые для ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="4da9d-126">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="4da9d-127">Невозможна передача объектов в ПО промежуточного слоя, активируемое на основе фабрики с помощью `UseMiddleware`:</span><span class="sxs-lookup"><span data-stu-id="4da9d-127">It isn't possible to pass objects to the factory-activated middleware with `UseMiddleware`:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="4da9d-128">Активируемое на основе фабрики ПО промежуточного слоя добавляется во встроенный контейнер в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4da9d-128">The factory-activated middleware is added to the built-in container in *Startup.cs*:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

<span data-ttu-id="4da9d-129">Оба ПО промежуточного слоя регистрируются в конвейере обработки запросов в `Configure`:</span><span class="sxs-lookup"><span data-stu-id="4da9d-129">Both middlewares are registered in the request processing pipeline in `Configure`:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=14-15)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="4da9d-130">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="4da9d-130">IMiddlewareFactory</span></span>

<span data-ttu-id="4da9d-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) предоставляет методы для создания ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="4da9d-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span> <span data-ttu-id="4da9d-132">Реализация ПО промежуточного слоя на основе фабрики регистрируется в контейнере в виде ограниченной (scoped) службы.</span><span class="sxs-lookup"><span data-stu-id="4da9d-132">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="4da9d-133">Реализация `IMiddlewareFactory` по умолчанию, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), находится в пакете [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/).</span><span class="sxs-lookup"><span data-stu-id="4da9d-133">The default `IMiddlewareFactory` implementation, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4da9d-134">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4da9d-134">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
