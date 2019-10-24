---
title: Соглашения для маршрутов и приложений Razor Pages в ASP.NET Core
author: guardrex
description: Узнайте, как использовать соглашения поставщика модели маршрутов и приложений для управления маршрутизацией, обнаружением и обработкой страниц.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/22/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: a0a1eda69da34d1865fd11ef464c3697bcd01eff
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779215"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="c5089-103">Соглашения для маршрутов и приложений Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5089-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="c5089-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="c5089-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c5089-105">Узнайте, как использовать [соглашения поставщика модели маршрутов и приложений](xref:mvc/controllers/application-model#conventions) страниц для управления маршрутизацией, обнаружением и обработкой страниц в приложениях Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c5089-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="c5089-106">Когда вам необходимо настроить уникальные маршруты для отдельных страниц, используйте [соглашение AddPageRoute](#configure-a-page-route), описанное далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="c5089-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="c5089-107">Чтобы указать маршрут страницы, добавить сегменты маршрута или добавить параметры в маршрут, используйте директиву `@page` страницы.</span><span class="sxs-lookup"><span data-stu-id="c5089-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="c5089-108">Дополнительные сведения см. в разделе [Custom Routes](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="c5089-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="c5089-109">Существуют зарезервированные слова, которые нельзя использовать в качестве сегментов маршрутов или имен параметров.</span><span class="sxs-lookup"><span data-stu-id="c5089-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="c5089-110">Дополнительные сведения см. в разделе [Маршрутизация: зарезервированные имена маршрутов](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="c5089-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="c5089-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c5089-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="c5089-112">Сценарий</span><span class="sxs-lookup"><span data-stu-id="c5089-112">Scenario</span></span> | <span data-ttu-id="c5089-113">Что демонстрирует пример</span><span class="sxs-lookup"><span data-stu-id="c5089-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="c5089-114">Соглашения для моделей</span><span class="sxs-lookup"><span data-stu-id="c5089-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="c5089-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="c5089-115">Conventions.Add</span></span><ul><li><span data-ttu-id="c5089-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="c5089-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="c5089-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="c5089-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="c5089-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="c5089-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="c5089-119">Добавление шаблона маршрута и заголовка к страницам приложения.</span><span class="sxs-lookup"><span data-stu-id="c5089-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="c5089-120">Соглашения для действий с маршрутами страниц</span><span class="sxs-lookup"><span data-stu-id="c5089-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="c5089-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="c5089-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="c5089-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="c5089-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="c5089-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="c5089-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="c5089-124">Добавление шаблона маршрута к страницам в папке и к одной странице.</span><span class="sxs-lookup"><span data-stu-id="c5089-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="c5089-125">Соглашения для действий модели страниц</span><span class="sxs-lookup"><span data-stu-id="c5089-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="c5089-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="c5089-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="c5089-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="c5089-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="c5089-128">ConfigureFilter (класс фильтра, лямбда-выражение или фабрика фильтров)</span><span class="sxs-lookup"><span data-stu-id="c5089-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="c5089-129">Добавление заголовка к страницам в папке, добавление заголовка к одной странице и настройка [фабрики фильтров](xref:mvc/controllers/filters#ifilterfactory) для добавления заголовка к страницам приложения.</span><span class="sxs-lookup"><span data-stu-id="c5089-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="c5089-130">Razor Pages соглашения добавляются и настраиваются с помощью метода расширения <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> для <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> коллекции служб в классе `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c5089-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="c5089-131">Следующие примеры соглашений описаны далее в этом разделе:</span><span class="sxs-lookup"><span data-stu-id="c5089-131">The following convention examples are explained later in this topic:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

## <a name="route-order"></a><span data-ttu-id="c5089-132">Порядок маршрутов</span><span class="sxs-lookup"><span data-stu-id="c5089-132">Route order</span></span>

<span data-ttu-id="c5089-133">Маршруты указывают <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для обработки (сопоставление маршрутов).</span><span class="sxs-lookup"><span data-stu-id="c5089-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="c5089-134">Номер</span><span class="sxs-lookup"><span data-stu-id="c5089-134">Order</span></span>            | <span data-ttu-id="c5089-135">Поведение</span><span class="sxs-lookup"><span data-stu-id="c5089-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="c5089-136">-1</span><span class="sxs-lookup"><span data-stu-id="c5089-136">-1</span></span>               | <span data-ttu-id="c5089-137">Маршрут обрабатывается до обработки других маршрутов.</span><span class="sxs-lookup"><span data-stu-id="c5089-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="c5089-138">0</span><span class="sxs-lookup"><span data-stu-id="c5089-138">0</span></span>                | <span data-ttu-id="c5089-139">Порядок не указан (значение по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="c5089-139">Order isn't specified (default value).</span></span> <span data-ttu-id="c5089-140">Не назначайте `Order` (`Order = null`) по умолчанию маршрут `Order` значение 0 (ноль) для обработки.</span><span class="sxs-lookup"><span data-stu-id="c5089-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="c5089-141">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="c5089-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="c5089-142">Указывает порядок обработки маршрутов.</span><span class="sxs-lookup"><span data-stu-id="c5089-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="c5089-143">Обработка маршрутов устанавливается по соглашению:</span><span class="sxs-lookup"><span data-stu-id="c5089-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="c5089-144">Маршруты обрабатываются в последовательном порядке (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="c5089-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="c5089-145">Если маршруты имеют одинаковые `Order`, сначала сопоставляются наиболее конкретный маршрут, за которым следуют менее определенные маршруты.</span><span class="sxs-lookup"><span data-stu-id="c5089-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="c5089-146">Если маршруты с одинаковым `Order` и количеством параметров совпадают с URL-адресом запроса, маршруты обрабатываются в том порядке, в котором они добавляются в <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span><span class="sxs-lookup"><span data-stu-id="c5089-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="c5089-147">По возможности старайтесь не зависеть от установленного порядка обработки маршрутов.</span><span class="sxs-lookup"><span data-stu-id="c5089-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="c5089-148">Как правило, маршрутизация выбирает правильный маршрут с сопоставлением URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="c5089-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="c5089-149">Если для правильной маршрутизации запросов необходимо задать свойства маршрута `Order`, схема маршрутизации приложения, скорее всего, запутать клиентов и не будет поддерживаться.</span><span class="sxs-lookup"><span data-stu-id="c5089-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="c5089-150">Поиск упрощает схему маршрутизации приложения.</span><span class="sxs-lookup"><span data-stu-id="c5089-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="c5089-151">Для примера приложения требуется явный порядок обработки маршрута, чтобы продемонстрировать несколько сценариев маршрутизации, использующих одно приложение.</span><span class="sxs-lookup"><span data-stu-id="c5089-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="c5089-152">Тем не менее, следует попытаться избежать появления рекомендаций по настройке `Order` маршрутов в рабочих приложениях.</span><span class="sxs-lookup"><span data-stu-id="c5089-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="c5089-153">Средства маршрутизации в Razor Pages и контроллере MVC имеют общую реализацию.</span><span class="sxs-lookup"><span data-stu-id="c5089-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="c5089-154">Сведения о порядке маршрутов см. в разделе [Маршрутизация к действиям контроллера: упорядочение маршрутов атрибутов](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="c5089-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="c5089-155">Соглашения для моделей</span><span class="sxs-lookup"><span data-stu-id="c5089-155">Model conventions</span></span>

<span data-ttu-id="c5089-156">Добавьте делегат для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, чтобы добавить [соглашения модели](xref:mvc/controllers/application-model#conventions) , применяемые к Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c5089-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="c5089-157">Добавление соглашения о модели маршрута ко всем страницам</span><span class="sxs-lookup"><span data-stu-id="c5089-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="c5089-158">Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> в коллекцию экземпляров <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, которые применяются во время построения модели маршрутизации страниц.</span><span class="sxs-lookup"><span data-stu-id="c5089-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="c5089-159">В примере приложения шаблон маршрута `{globalTemplate?}` добавляется ко всем страницам приложения:</span><span class="sxs-lookup"><span data-stu-id="c5089-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c5089-160">Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `1`.</span><span class="sxs-lookup"><span data-stu-id="c5089-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="c5089-161">Это обеспечивает следующее поведение сопоставления маршрутов в примере приложения:</span><span class="sxs-lookup"><span data-stu-id="c5089-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="c5089-162">Шаблон маршрута для `TheContactPage/{text?}` добавляется далее в разделе.</span><span class="sxs-lookup"><span data-stu-id="c5089-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="c5089-163">Маршрут страницы контакта по умолчанию имеет порядок `null` (`Order = 0`), поэтому он соответствует шаблону `{globalTemplate?}` маршрута.</span><span class="sxs-lookup"><span data-stu-id="c5089-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="c5089-164">Шаблон `{aboutTemplate?}`ного маршрута добавляется далее в разделе.</span><span class="sxs-lookup"><span data-stu-id="c5089-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="c5089-165">Шаблону `{aboutTemplate?}` задано свойство `Order`, равное `2`.</span><span class="sxs-lookup"><span data-stu-id="c5089-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="c5089-166">При запросе страницы About по адресу `/About/RouteDataValue` значение RouteDataValue загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`), а не в `RouteData.Values["aboutTemplate"]` (`Order = 2`). Это происходит из-за значения свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="c5089-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="c5089-167">Шаблон `{otherPagesTemplate?}`ного маршрута добавляется далее в разделе.</span><span class="sxs-lookup"><span data-stu-id="c5089-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="c5089-168">Шаблону `{otherPagesTemplate?}` задано свойство `Order`, равное `2`.</span><span class="sxs-lookup"><span data-stu-id="c5089-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="c5089-169">Если любая страница в папке *pages/OtherPages* запрашивается с помощью параметра Route (например, `/OtherPages/Page1/RouteDataValue`), то "RouteDataValue" загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`), а не `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) из-за задания свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="c5089-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="c5089-170">Везде, где это возможно, не следует задавать `Order`, что приводит к `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="c5089-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="c5089-171">Чтобы выбрать правильный маршрут, используйте маршрутизацию.</span><span class="sxs-lookup"><span data-stu-id="c5089-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="c5089-172">Razor Pages параметры, такие как добавление <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, добавляются при добавлении MVC в коллекцию служб в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c5089-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c5089-173">Пример см. в [образце приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="c5089-173">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c5089-174">Запросите страницу About по адресу `localhost:5000/About/GlobalRouteValue` из примера и проверьте результат:</span><span class="sxs-lookup"><span data-stu-id="c5089-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Запрос страницы About с сегментом маршрута GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="c5089-177">Добавление соглашения о модели приложений ко всем страницам</span><span class="sxs-lookup"><span data-stu-id="c5089-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="c5089-178">Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> в коллекцию экземпляров <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, которые применяются во время построения модели приложения страницы.</span><span class="sxs-lookup"><span data-stu-id="c5089-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="c5089-179">Для демонстрации этих и других соглашений далее в этом разделе в пример приложения включен класс `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="c5089-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="c5089-180">Конструктор класса принимает строку `name` и массив строк `values`.</span><span class="sxs-lookup"><span data-stu-id="c5089-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="c5089-181">Эти значения используются в его методе `OnResultExecuting` для задания заголовка ответа.</span><span class="sxs-lookup"><span data-stu-id="c5089-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="c5089-182">Класс показан полностью в разделе [Соглашения для действий модели страниц](#page-model-action-conventions) далее.</span><span class="sxs-lookup"><span data-stu-id="c5089-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="c5089-183">Пример приложения использует класс `AddHeaderAttribute`, чтобы добавить заголовок `GlobalHeader` ко всем страницам в приложении:</span><span class="sxs-lookup"><span data-stu-id="c5089-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c5089-184">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c5089-184">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="c5089-185">Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="c5089-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Заголовки ответа страницы About, показывающие, что добавлен заголовок GlobalHeader.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="c5089-187">Добавление соглашения модели обработчиков ко всем страницам</span><span class="sxs-lookup"><span data-stu-id="c5089-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="c5089-188">Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> в коллекцию экземпляров <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, которые применяются во время построения модели обработчика страницы.</span><span class="sxs-lookup"><span data-stu-id="c5089-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c5089-189">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c5089-189">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

## <a name="page-route-action-conventions"></a><span data-ttu-id="c5089-190">Соглашения для действий с маршрутами страниц</span><span class="sxs-lookup"><span data-stu-id="c5089-190">Page route action conventions</span></span>

<span data-ttu-id="c5089-191">Поставщик модели маршрута по умолчанию, производный от <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider>, вызывает соглашения, которые предназначены для предоставления точек расширения для настройки маршрутов страниц.</span><span class="sxs-lookup"><span data-stu-id="c5089-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="c5089-192">Соглашение о модели маршрута папки</span><span class="sxs-lookup"><span data-stu-id="c5089-192">Folder route model convention</span></span>

<span data-ttu-id="c5089-193">Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>, чтобы создать и добавить <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, который вызывает действие в <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> для всех страниц в указанной папке.</span><span class="sxs-lookup"><span data-stu-id="c5089-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="c5089-194">Пример приложения использует <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> для добавления шаблона маршрута `{otherPagesTemplate?}` к страницам в папке *OtherPages*:</span><span class="sxs-lookup"><span data-stu-id="c5089-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="c5089-195">Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `2`.</span><span class="sxs-lookup"><span data-stu-id="c5089-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="c5089-196">Это гарантирует, что шаблон для `{globalTemplate?}` (заданный ранее в разделе `1`) получает приоритет для первой координаты значения данных маршрута, если указано одно значение маршрута.</span><span class="sxs-lookup"><span data-stu-id="c5089-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="c5089-197">Если страница в папке *pages/OtherPages* запрашивается с помощью значения параметра маршрута (например, `/OtherPages/Page1/RouteDataValue`), то "RouteDataValue" загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`), а не `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) из-за задания свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="c5089-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="c5089-198">Везде, где это возможно, не следует задавать `Order`, что приводит к `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="c5089-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="c5089-199">Чтобы выбрать правильный маршрут, используйте маршрутизацию.</span><span class="sxs-lookup"><span data-stu-id="c5089-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="c5089-200">Запросите страницу Page1 по адресу `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` из примера и проверьте результат:</span><span class="sxs-lookup"><span data-stu-id="c5089-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Страница Page1 в папке OtherPages запрашивается с сегментом маршрута GlobalRouteValue и OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="c5089-203">Соглашение о модели маршрута страницы</span><span class="sxs-lookup"><span data-stu-id="c5089-203">Page route model convention</span></span>

<span data-ttu-id="c5089-204">Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>, чтобы создать и добавить <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, который вызывает действие в <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> для страницы с указанным именем.</span><span class="sxs-lookup"><span data-stu-id="c5089-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="c5089-205">Пример приложения использует `AddPageRouteModelConvention` для добавления шаблона маршрута `{aboutTemplate?}` к странице About:</span><span class="sxs-lookup"><span data-stu-id="c5089-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

<span data-ttu-id="c5089-206">Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `2`.</span><span class="sxs-lookup"><span data-stu-id="c5089-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="c5089-207">Это гарантирует, что шаблон для `{globalTemplate?}` (заданный ранее в разделе `1`) получает приоритет для первой координаты значения данных маршрута, если указано одно значение маршрута.</span><span class="sxs-lookup"><span data-stu-id="c5089-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="c5089-208">Если страница about запрашивается со значением параметра маршрута в `/About/RouteDataValue`, то "RouteDataValue" загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`), а не `RouteData.Values["aboutTemplate"]` (`Order = 2`) из-за задания свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="c5089-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="c5089-209">Везде, где это возможно, не следует задавать `Order`, что приводит к `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="c5089-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="c5089-210">Чтобы выбрать правильный маршрут, используйте маршрутизацию.</span><span class="sxs-lookup"><span data-stu-id="c5089-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="c5089-211">Запросите страницу About по адресу `localhost:5000/About/GlobalRouteValue/AboutRouteValue` из примера и проверьте результат:</span><span class="sxs-lookup"><span data-stu-id="c5089-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![Страница About запрашивается с сегментами маршрута для GlobalRouteValue и AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="c5089-214">Использование преобразователя параметров для настройки маршрутов страниц</span><span class="sxs-lookup"><span data-stu-id="c5089-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="c5089-215">Маршруты страниц, созданные ASP.NET Core, можно настроить с помощью преобразователя параметров.</span><span class="sxs-lookup"><span data-stu-id="c5089-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="c5089-216">Преобразователь параметров реализует `IOutboundParameterTransformer` и преобразует значения параметров.</span><span class="sxs-lookup"><span data-stu-id="c5089-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="c5089-217">Например, пользовательский преобразователь параметра `SlugifyParameterTransformer` изменяет значение маршрута `SubscriptionManagement` на `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="c5089-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="c5089-218">В соответствии с соглашением о модели `PageRouteTransformerConvention` страниц преобразователь параметров применяется к сегментам папок и имен файлов автоматически создаваемых маршрутов страниц в приложении.</span><span class="sxs-lookup"><span data-stu-id="c5089-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="c5089-219">Например, файл Razor Pages в */Пажес/субскриптионманажемент/виевалл.кштмл* будет иметь перезапись маршрута из `/SubscriptionManagement/ViewAll` в `/subscription-management/view-all`.</span><span class="sxs-lookup"><span data-stu-id="c5089-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="c5089-220">`PageRouteTransformerConvention` преобразует автоматически создаваемые сегменты маршрута страницы, полученные из папки Razor Pages и имени файла.</span><span class="sxs-lookup"><span data-stu-id="c5089-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="c5089-221">Он не преобразует сегменты маршрута, добавленные с помощью директивы `@page`.</span><span class="sxs-lookup"><span data-stu-id="c5089-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="c5089-222">Это соглашение также не преобразует маршруты, добавленные <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span><span class="sxs-lookup"><span data-stu-id="c5089-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="c5089-223">@No__t_0 регистрируется как вариант в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c5089-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
                    new SlugifyParameterTransformer()));
        });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
                    new SlugifyParameterTransformer()));
        });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

## <a name="configure-a-page-route"></a><span data-ttu-id="c5089-224">Настройка маршрута страницы</span><span class="sxs-lookup"><span data-stu-id="c5089-224">Configure a page route</span></span>

<span data-ttu-id="c5089-225">Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>, чтобы настроить маршрут к странице по указанному пути к странице.</span><span class="sxs-lookup"><span data-stu-id="c5089-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="c5089-226">Созданные ссылки на страницу будут использовать заданный вами маршрут.</span><span class="sxs-lookup"><span data-stu-id="c5089-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="c5089-227">Для установления маршрута метод `AddPageRoute` использует `AddPageRouteModelConvention`.</span><span class="sxs-lookup"><span data-stu-id="c5089-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="c5089-228">Пример приложения создает маршрут к `/TheContactPage` для файла *Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c5089-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

<span data-ttu-id="c5089-229">На страницу Contact также можно перейти с помощью `/Contact` через маршрут по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c5089-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="c5089-230">Настраиваемый маршрут на страницу Contact в примере приложения позволяет использовать необязательный сегмент маршрута `text` (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="c5089-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="c5089-231">Страница также включает этот необязательный сегмент в своей директиве `@page` на случай, если посетитель переходит на страницу по маршруту `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="c5089-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

<span data-ttu-id="c5089-232">Обратите внимание, что URL-адрес, созданный для ссылки **Contact** на отображенной странице, отражает обновленный маршрут:</span><span class="sxs-lookup"><span data-stu-id="c5089-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Ссылка Contact на панели навигации в примере приложения](razor-pages-conventions/_static/contact-link.png)

![В ссылке Contact в показанном HTML видно, что href имеет значение "/TheContactPage".](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="c5089-235">Перейдите на страницу Contact по ее обычному маршруту `/Contact` или по настроенному маршруту `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="c5089-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="c5089-236">Если вы укажете дополнительный сегмент маршрута `text`, страница отобразит этот сегмент в HTML-кодировке:</span><span class="sxs-lookup"><span data-stu-id="c5089-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Пример в браузере Microsoft Edge с указанием необязательного сегмента маршрута "text" со значением "TextValue" в URL-адресе.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="c5089-239">Соглашения для действий модели страниц</span><span class="sxs-lookup"><span data-stu-id="c5089-239">Page model action conventions</span></span>

<span data-ttu-id="c5089-240">Поставщик модели страницы по умолчанию, реализующий <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider>, вызывает соглашения, которые предназначены для предоставления точек расширения для настройки моделей страниц.</span><span class="sxs-lookup"><span data-stu-id="c5089-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="c5089-241">Эти соглашения полезны при создании и изменении для обнаружения и обработки страниц.</span><span class="sxs-lookup"><span data-stu-id="c5089-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="c5089-242">В примерах этого раздела Пример приложения использует класс `AddHeaderAttribute`, который является <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, который применяет заголовок ответа:</span><span class="sxs-lookup"><span data-stu-id="c5089-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c5089-243">Образец показывает использование соглашений для применения атрибута ко всем страницам в папке и к одной странице.</span><span class="sxs-lookup"><span data-stu-id="c5089-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="c5089-244">**Соглашение для модели приложений папки**</span><span class="sxs-lookup"><span data-stu-id="c5089-244">**Folder app model convention**</span></span>

<span data-ttu-id="c5089-245">Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>, чтобы создать и добавить <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, который вызывает действие в экземплярах <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> для всех страниц в указанной папке.</span><span class="sxs-lookup"><span data-stu-id="c5089-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="c5089-246">Пример демонстрирует использование `AddFolderApplicationModelConvention` путем добавления заголовка `OtherPagesHeader` к страницам в папке *OtherPages* приложения:</span><span class="sxs-lookup"><span data-stu-id="c5089-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

<span data-ttu-id="c5089-247">Запросите страницу Page1 по адресу `localhost:5000/OtherPages/Page1` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="c5089-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Заголовки ответа страницы OtherPages/Page1, показывающие добавленный заголовок OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="c5089-249">**Соглашение для модели приложений страницы**</span><span class="sxs-lookup"><span data-stu-id="c5089-249">**Page app model convention**</span></span>

<span data-ttu-id="c5089-250">Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>, чтобы создать и добавить <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, который вызывает действие в <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> для страницы с указанным именем.</span><span class="sxs-lookup"><span data-stu-id="c5089-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="c5089-251">Пример демонстрирует использование `AddPageApplicationModelConvention` путем добавления заголовка `AboutHeader` на страницу About:</span><span class="sxs-lookup"><span data-stu-id="c5089-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

<span data-ttu-id="c5089-252">Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="c5089-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Заголовки ответа страницы About, показывающие, что добавлен заголовок AboutHeader.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="c5089-254">**Настройка фильтра**</span><span class="sxs-lookup"><span data-stu-id="c5089-254">**Configure a filter**</span></span>

<span data-ttu-id="c5089-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> настраивает указанный фильтр для применения.</span><span class="sxs-lookup"><span data-stu-id="c5089-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="c5089-256">Вы можете создать класс фильтра, но пример приложения показывает реализацию фильтра в лямбда-выражении, которое реализовано "за кулисами" в качестве фабрики, возвращающей фильтр:</span><span class="sxs-lookup"><span data-stu-id="c5089-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

<span data-ttu-id="c5089-257">Модель приложений страницы используется для проверки относительного пути на наличие сегментов, ведущих к странице Page2 в папке *OtherPages*.</span><span class="sxs-lookup"><span data-stu-id="c5089-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="c5089-258">Если условие выполняется, добавляется заголовок.</span><span class="sxs-lookup"><span data-stu-id="c5089-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="c5089-259">Если нет, применяется `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="c5089-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="c5089-260">`EmptyFilter` является [фильтром действий](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="c5089-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="c5089-261">Так как фильтры действий игнорируются Razor Pages, `EmptyFilter` не действует, как предполагалось, если путь не содержит `OtherPages/Page2`.</span><span class="sxs-lookup"><span data-stu-id="c5089-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="c5089-262">Запросите страницу Page2 по адресу `localhost:5000/OtherPages/Page2` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="c5089-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![В ответ для страницы Page2 добавлен заголовок OtherPagesPage2Header.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="c5089-264">**Настройка фабрики фильтров**</span><span class="sxs-lookup"><span data-stu-id="c5089-264">**Configure a filter factory**</span></span>

<span data-ttu-id="c5089-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> настраивает указанную фабрику для применения [фильтров](xref:mvc/controllers/filters) ко всем Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c5089-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="c5089-266">В примере приложения приведен образец использования [фабрики фильтров](xref:mvc/controllers/filters#ifilterfactory) путем добавления на страницы приложения заголовка `FilterFactoryHeader` с двумя значениями:</span><span class="sxs-lookup"><span data-stu-id="c5089-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

<span data-ttu-id="c5089-267">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="c5089-267">*AddHeaderWithFactory.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c5089-268">Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="c5089-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Заголовки ответов на странице About показывают два добавленных заголовка FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="c5089-270">Фильтры MVC и фильтр страниц (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="c5089-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="c5089-271">[Фильтры действий](xref:mvc/controllers/filters#action-filters) MVC игнорируются в Razor Pages, так как Razor Pages использует методы обработчиков.</span><span class="sxs-lookup"><span data-stu-id="c5089-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="c5089-272">Доступны для использования другие типы фильтров MVC: фильтры [авторизации](xref:mvc/controllers/filters#authorization-filters), [исключений](xref:mvc/controllers/filters#exception-filters), [ресурсов](xref:mvc/controllers/filters#resource-filters) и [результатов](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="c5089-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="c5089-273">Дополнительные сведения см. в разделе [Фильтры](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="c5089-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="c5089-274">Фильтр страницы (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) — это фильтр, который применяется к Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c5089-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="c5089-275">Дополнительные сведения см. в разделе [Методы фильтрации для Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="c5089-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c5089-276">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c5089-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
