---
title: Методы фильтрации для Razor Pages в ASP.NET Core
author: Rick-Anderson
description: Описание способов создания методов фильтрации Razor Pages в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/05/2018
uid: razor-pages/filter
ms.openlocfilehash: 5b233d95c9fbab09c64072377b85b40b127df7b7
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205942"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a><span data-ttu-id="b5e8a-103">Методы фильтрации для Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5e8a-103">Filter methods for Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="b5e8a-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="b5e8a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b5e8a-105">Фильтры Razor Pages [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) и [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) разрешают Razor Pages выполнять код до и после запуска обработчика Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-105">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="b5e8a-106">Фильтры страницы Razor похожи на [фильтры действий MVC ASP.NET Core](xref:mvc/controllers/filters#action-filters), но их нельзя применять к методам обработчика отдельной страницы.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-106">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span> 

<span data-ttu-id="b5e8a-107">Фильтры страницы Razor:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-107">Razor Page filters:</span></span>

* <span data-ttu-id="b5e8a-108">Запускают код после выбора метода обработчика, но до привязки модели.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-108">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="b5e8a-109">Запускают код до выполнения метода обработчика, но после привязки модели.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-109">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="b5e8a-110">Запускают код после выполнения метода обработчика.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-110">Run code after the handler method executes.</span></span>
* <span data-ttu-id="b5e8a-111">Могут реализовываться глобально или на странице.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-111">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="b5e8a-112">Не могут применяться к методам обработчика для конкретной страницы.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-112">Cannot be applied to specific page handler methods.</span></span>

<span data-ttu-id="b5e8a-113">Код может быть запущен до выполнения метода обработчика с помощью конструктора страницы или ПО промежуточного слоя, но только фильтры страницы Razor имеют доступ к [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span><span class="sxs-lookup"><span data-stu-id="b5e8a-113">Code can be run before a handler method executes using the page constructor or middleware, but only Razor Page filters have access to [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span></span> <span data-ttu-id="b5e8a-114">Фильтры имеют производный параметр [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0), который предоставляет доступ к `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-114">Filters have a [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="b5e8a-115">Например, образец [Применение атрибута фильтра](#ifa) добавляет заголовок к ответу. Это невозможно сделать с помощью конструкторов или ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-115">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="b5e8a-116">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b5e8a-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b5e8a-117">Фильтры страницы Razor предоставляют следующие методы, которые могут применяться глобально или на уровне страницы:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-117">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="b5e8a-118">Синхронные методы:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-118">Synchronous methods:</span></span>

    * <span data-ttu-id="b5e8a-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0). Вызывается после выбора метода обработчика, но до привязки модели.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
    * <span data-ttu-id="b5e8a-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0). Вызывается до выполнения метода обработчика, но после привязки модели.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
    * <span data-ttu-id="b5e8a-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0). Вызывается после выполнения метода обработчика, но до результата действия.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="b5e8a-122">Асинхронные методы:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-122">Asynchronous methods:</span></span>

    * <span data-ttu-id="b5e8a-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0). Вызывается асинхронно после выбора метода обработчика, но до привязки модели.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
    * <span data-ttu-id="b5e8a-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0). Вызывается асинхронно до вызова метода обработчика, но после привязки модели.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

> [!NOTE]
> <span data-ttu-id="b5e8a-125">Реализуйте **либо** синхронный, либо асинхронный вариант интерфейса фильтра, но не оба варианта.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-125">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="b5e8a-126">Платформа сначала проверяет, реализует ли фильтр асинхронный интерфейс. Если да, вызывается он.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-126">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="b5e8a-127">В противном случае вызываются методы синхронного интерфейса.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-127">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="b5e8a-128">Если реализуются оба интерфейса, вызываются только асинхронные методы.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-128">If both interfaces are implemented, only the async methods are be called.</span></span> <span data-ttu-id="b5e8a-129">Это же правило применяется к переопределению на страницах — реализуйте синхронную или асинхронную версию переопределения, но не обе сразу.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-129">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="b5e8a-130">Реализации фильтров страницы Razor глобально</span><span class="sxs-lookup"><span data-stu-id="b5e8a-130">Implement Razor Page filters globally</span></span>

<span data-ttu-id="b5e8a-131">В следующем коде реализуется фильтр `IAsyncPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-131">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="b5e8a-132">В приведенном выше коде [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) не является обязательным.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-132">In the preceding code, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) is not required.</span></span> <span data-ttu-id="b5e8a-133">Он используется в образце для предоставления сведений о трассировке для приложения.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-133">It's used in the sample to provide trace information for the application.</span></span>

<span data-ttu-id="b5e8a-134">В следующем коде включается `SampleAsyncPageFilter` в классе `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-134">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

<span data-ttu-id="b5e8a-135">В следующем коде демонстрируется полный класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-135">The following code shows the complete `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

<span data-ttu-id="b5e8a-136">Следующий код вызывает `AddFolderApplicationModelConvention` для применения `SampleAsyncPageFilter` только к страницам в */subFolder*:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-136">The following code calls `AddFolderApplicationModelConvention` to apply the `SampleAsyncPageFilter` to only pages in */subFolder*:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="b5e8a-137">В следующем коде реализуется синхронный метод `IPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-137">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="b5e8a-138">В следующем коде включается `SamplePageFilter`:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-138">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="b5e8a-139">Реализация фильтров страницы Razor путем переопределения методов фильтра</span><span class="sxs-lookup"><span data-stu-id="b5e8a-139">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="b5e8a-140">В следующем коде переопределяются синхронные фильтры страницы Razor:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-140">The following code overrides the synchronous Razor Page filters:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

::: moniker-end

<a name="ifa"></a>
## <a name="implement-a-filter-attribute"></a><span data-ttu-id="b5e8a-141">Реализация атрибута фильтра</span><span class="sxs-lookup"><span data-stu-id="b5e8a-141">Implement a filter attribute</span></span>

<span data-ttu-id="b5e8a-142">Встроенный фильтр на основе атрибутов [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) может быть разделен на подклассы.</span><span class="sxs-lookup"><span data-stu-id="b5e8a-142">The built-in attribute-based filter [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filter can be subclassed.</span></span> <span data-ttu-id="b5e8a-143">Следующий фильтр добавляет заголовок к ответу:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-143">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="b5e8a-144">В следующем коде применяется атрибут `AddHeader`:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-144">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

<span data-ttu-id="b5e8a-145">Инструкции по переопределению порядка см. в разделе [Переопределение порядка по умолчанию](xref:mvc/controllers/filters#overriding-the-default-order).</span><span class="sxs-lookup"><span data-stu-id="b5e8a-145">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="b5e8a-146">Инструкции по замыканию конвейера фильтров из фильтра см. в разделе [Отмена и замыкание](xref:mvc/controllers/filters#cancellation-and-short-circuiting).</span><span class="sxs-lookup"><span data-stu-id="b5e8a-146">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span> 

<a name="auth"></a>
## <a name="authorize-filter-attribute"></a><span data-ttu-id="b5e8a-147">Атрибут фильтра Authorize</span><span class="sxs-lookup"><span data-stu-id="b5e8a-147">Authorize filter attribute</span></span>

<span data-ttu-id="b5e8a-148">Атрибут [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) можно применить к `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="b5e8a-148">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
