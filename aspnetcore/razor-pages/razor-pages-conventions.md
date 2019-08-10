---
title: Соглашения для маршрутов и приложений Razor Pages в ASP.NET Core
author: guardrex
description: Узнайте, как использовать соглашения поставщика модели маршрутов и приложений для управления маршрутизацией, обнаружением и обработкой страниц.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/08/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: c93f169c422d260f738faba4812861521f383e51
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914973"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="0f141-103">Соглашения для маршрутов и приложений Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f141-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="0f141-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="0f141-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0f141-105">Узнайте, как использовать [соглашения поставщика модели маршрутов и приложений](xref:mvc/controllers/application-model#conventions) страниц для управления маршрутизацией, обнаружением и обработкой страниц в приложениях Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0f141-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="0f141-106">Когда вам необходимо настроить уникальные маршруты для отдельных страниц, используйте [соглашение AddPageRoute](#configure-a-page-route), описанное далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="0f141-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="0f141-107">Чтобы указать маршрут страницы, добавить сегменты маршрута или добавить параметры в маршрут, используйте `@page` директиву страницы.</span><span class="sxs-lookup"><span data-stu-id="0f141-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="0f141-108">Дополнительные сведения см. в разделе [Custom Routes](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="0f141-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="0f141-109">Существуют зарезервированные слова, которые нельзя использовать в качестве сегментов маршрутов или имен параметров.</span><span class="sxs-lookup"><span data-stu-id="0f141-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="0f141-110">Дополнительные сведения см. в [разделе Маршрутизация: Зарезервированные](xref:fundamentals/routing#reserved-routing-names)имена маршрутов.</span><span class="sxs-lookup"><span data-stu-id="0f141-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="0f141-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0f141-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="0f141-112">Сценарий</span><span class="sxs-lookup"><span data-stu-id="0f141-112">Scenario</span></span> | <span data-ttu-id="0f141-113">Что демонстрирует пример</span><span class="sxs-lookup"><span data-stu-id="0f141-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="0f141-114">Соглашения для моделей</span><span class="sxs-lookup"><span data-stu-id="0f141-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="0f141-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="0f141-115">Conventions.Add</span></span><ul><li><span data-ttu-id="0f141-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="0f141-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="0f141-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="0f141-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="0f141-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="0f141-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="0f141-119">Добавление шаблона маршрута и заголовка к страницам приложения.</span><span class="sxs-lookup"><span data-stu-id="0f141-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="0f141-120">Соглашения для действий с маршрутами страниц</span><span class="sxs-lookup"><span data-stu-id="0f141-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="0f141-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="0f141-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="0f141-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="0f141-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="0f141-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="0f141-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="0f141-124">Добавление шаблона маршрута к страницам в папке и к одной странице.</span><span class="sxs-lookup"><span data-stu-id="0f141-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="0f141-125">Соглашения для действий модели страниц</span><span class="sxs-lookup"><span data-stu-id="0f141-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="0f141-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="0f141-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="0f141-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="0f141-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="0f141-128">ConfigureFilter (класс фильтра, лямбда-выражение или фабрика фильтров)</span><span class="sxs-lookup"><span data-stu-id="0f141-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="0f141-129">Добавление заголовка к страницам в папке, добавление заголовка к одной странице и настройка [фабрики фильтров](xref:mvc/controllers/filters#ifilterfactory) для добавления заголовка к страницам приложения.</span><span class="sxs-lookup"><span data-stu-id="0f141-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="0f141-130">Razor Pages соглашения добавляются и настраиваются с <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> помощью <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> метода расширения в коллекции служб в `Startup` классе.</span><span class="sxs-lookup"><span data-stu-id="0f141-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="0f141-131">Следующие примеры соглашений описаны далее в этом разделе:</span><span class="sxs-lookup"><span data-stu-id="0f141-131">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="0f141-132">Порядок маршрутов</span><span class="sxs-lookup"><span data-stu-id="0f141-132">Route order</span></span>

<span data-ttu-id="0f141-133">Маршруты указывают <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для обработки (сопоставление маршрутов).</span><span class="sxs-lookup"><span data-stu-id="0f141-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="0f141-134">Номер</span><span class="sxs-lookup"><span data-stu-id="0f141-134">Order</span></span>            | <span data-ttu-id="0f141-135">Поведение</span><span class="sxs-lookup"><span data-stu-id="0f141-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="0f141-136">-1</span><span class="sxs-lookup"><span data-stu-id="0f141-136">-1</span></span>               | <span data-ttu-id="0f141-137">Маршрут обрабатывается до обработки других маршрутов.</span><span class="sxs-lookup"><span data-stu-id="0f141-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="0f141-138">0</span><span class="sxs-lookup"><span data-stu-id="0f141-138">0</span></span>                | <span data-ttu-id="0f141-139">Порядок не указан (значение по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="0f141-139">Order isn't specified (default value).</span></span> <span data-ttu-id="0f141-140">Не назначение `Order` (`Order = null`) по умолчанию присваивает `Order` маршруту значение 0 (нуль) для обработки.</span><span class="sxs-lookup"><span data-stu-id="0f141-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="0f141-141">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="0f141-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="0f141-142">Указывает порядок обработки маршрутов.</span><span class="sxs-lookup"><span data-stu-id="0f141-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="0f141-143">Обработка маршрутов устанавливается по соглашению:</span><span class="sxs-lookup"><span data-stu-id="0f141-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="0f141-144">Маршруты обрабатываются в последовательном порядке (-1, 0, 1 &hellip; , 2, n).</span><span class="sxs-lookup"><span data-stu-id="0f141-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="0f141-145">Если маршруты `Order`совпадают, сначала сопоставляются наиболее конкретный маршрут, за которым следуют менее конкретные маршруты.</span><span class="sxs-lookup"><span data-stu-id="0f141-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="0f141-146">Если маршруты с `Order` одинаковым и одинаковым числом параметров соответствуют URL-адресу запроса, маршруты обрабатываются в том порядке, в котором <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>они добавляются в.</span><span class="sxs-lookup"><span data-stu-id="0f141-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="0f141-147">По возможности старайтесь не зависеть от установленного порядка обработки маршрутов.</span><span class="sxs-lookup"><span data-stu-id="0f141-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="0f141-148">Как правило, маршрутизация выбирает правильный маршрут с сопоставлением URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="0f141-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="0f141-149">Если необходимо задать свойства маршрута `Order` для правильной маршрутизации запросов, схема маршрутизации приложения, скорее всего, выпутать клиентов и не будет поддерживать поддержку.</span><span class="sxs-lookup"><span data-stu-id="0f141-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="0f141-150">Поиск упрощает схему маршрутизации приложения.</span><span class="sxs-lookup"><span data-stu-id="0f141-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="0f141-151">Для примера приложения требуется явный порядок обработки маршрута, чтобы продемонстрировать несколько сценариев маршрутизации, использующих одно приложение.</span><span class="sxs-lookup"><span data-stu-id="0f141-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="0f141-152">Однако следует попытаться избежать практической ситуации при настройке маршрута `Order` в рабочих приложениях.</span><span class="sxs-lookup"><span data-stu-id="0f141-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="0f141-153">Средства маршрутизации в Razor Pages и контроллере MVC имеют общую реализацию.</span><span class="sxs-lookup"><span data-stu-id="0f141-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="0f141-154">Сведения о порядке маршрутов см. в разделе [маршрутизация к действиям контроллера: Упорядочивание маршрутов](xref:mvc/controllers/routing#ordering-attribute-routes)атрибутов.</span><span class="sxs-lookup"><span data-stu-id="0f141-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="0f141-155">Соглашения для моделей</span><span class="sxs-lookup"><span data-stu-id="0f141-155">Model conventions</span></span>

<span data-ttu-id="0f141-156">Добавьте делегат для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> , чтобы добавить [соглашения модели](xref:mvc/controllers/application-model#conventions) , применяемые к Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0f141-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="0f141-157">Добавление соглашения о модели маршрута ко всем страницам</span><span class="sxs-lookup"><span data-stu-id="0f141-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="0f141-158">Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> добавления в коллекцию <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> экземпляров, которые применяются во время построения модели маршрутизации страниц.</span><span class="sxs-lookup"><span data-stu-id="0f141-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="0f141-159">В примере приложения шаблон маршрута `{globalTemplate?}` добавляется ко всем страницам приложения:</span><span class="sxs-lookup"><span data-stu-id="0f141-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0f141-160">Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `1`.</span><span class="sxs-lookup"><span data-stu-id="0f141-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="0f141-161">Это обеспечивает следующее поведение сопоставления маршрутов в примере приложения:</span><span class="sxs-lookup"><span data-stu-id="0f141-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="0f141-162">Шаблон маршрута для `TheContactPage/{text?}` добавляется далее в разделе.</span><span class="sxs-lookup"><span data-stu-id="0f141-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="0f141-163">Маршрут страницы контакта имеет порядок `null` по умолчанию (`Order = 0`), `{globalTemplate?}` поэтому он соответствует шаблону маршрута.</span><span class="sxs-lookup"><span data-stu-id="0f141-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="0f141-164">Шаблон `{aboutTemplate?}` маршрута добавляется далее в разделе.</span><span class="sxs-lookup"><span data-stu-id="0f141-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="0f141-165">Шаблону `{aboutTemplate?}` задано свойство `Order`, равное `2`.</span><span class="sxs-lookup"><span data-stu-id="0f141-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="0f141-166">При запросе страницы About по адресу `/About/RouteDataValue` значение RouteDataValue загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`), а не в `RouteData.Values["aboutTemplate"]` (`Order = 2`). Это происходит из-за значения свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="0f141-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="0f141-167">Шаблон `{otherPagesTemplate?}` маршрута добавляется далее в разделе.</span><span class="sxs-lookup"><span data-stu-id="0f141-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="0f141-168">Шаблону `{otherPagesTemplate?}` задано свойство `Order`, равное `2`.</span><span class="sxs-lookup"><span data-stu-id="0f141-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="0f141-169">Когда любая страница в папке *pages/OtherPages* запрашивается с помощью параметра Route (например, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" загружается `RouteData.Values["globalTemplate"]` в`Order = 1`() и `RouteData.Values["otherPagesTemplate"]` не`Order = 2`() из-за установки свойства `Order` свойство.</span><span class="sxs-lookup"><span data-stu-id="0f141-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="0f141-170">Везде, где это возможно, `Order`не устанавливайте, `Order = 0`что приводит к.</span><span class="sxs-lookup"><span data-stu-id="0f141-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="0f141-171">Чтобы выбрать правильный маршрут, используйте маршрутизацию.</span><span class="sxs-lookup"><span data-stu-id="0f141-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="0f141-172">Параметры Razor Pages, такие как добавление <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, добавляются при добавлении MVC в коллекцию служб в. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="0f141-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0f141-173">Пример см. в [образце приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="0f141-173">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0f141-174">Запросите страницу About по адресу `localhost:5000/About/GlobalRouteValue` из примера и проверьте результат:</span><span class="sxs-lookup"><span data-stu-id="0f141-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Запрос страницы About с сегментом маршрута GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="0f141-177">Добавление соглашения о модели приложений ко всем страницам</span><span class="sxs-lookup"><span data-stu-id="0f141-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="0f141-178">Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> добавления в коллекцию <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> экземпляров, которые применяются во время построения модели приложения страницы.</span><span class="sxs-lookup"><span data-stu-id="0f141-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="0f141-179">Для демонстрации этих и других соглашений далее в этом разделе в пример приложения включен класс `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="0f141-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="0f141-180">Конструктор класса принимает строку `name` и массив строк `values`.</span><span class="sxs-lookup"><span data-stu-id="0f141-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="0f141-181">Эти значения используются в его методе `OnResultExecuting` для задания заголовка ответа.</span><span class="sxs-lookup"><span data-stu-id="0f141-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="0f141-182">Класс показан полностью в разделе [Соглашения для действий модели страниц](#page-model-action-conventions) далее.</span><span class="sxs-lookup"><span data-stu-id="0f141-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="0f141-183">Пример приложения использует класс `AddHeaderAttribute`, чтобы добавить заголовок `GlobalHeader` ко всем страницам в приложении:</span><span class="sxs-lookup"><span data-stu-id="0f141-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0f141-184">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0f141-184">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="0f141-185">Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="0f141-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Заголовки ответа страницы About, показывающие, что добавлен заголовок GlobalHeader.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="0f141-187">Добавление соглашения модели обработчиков ко всем страницам</span><span class="sxs-lookup"><span data-stu-id="0f141-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="0f141-188">Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> добавления в коллекцию <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> экземпляров, которые применяются во время построения модели обработчика страницы.</span><span class="sxs-lookup"><span data-stu-id="0f141-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0f141-189">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0f141-189">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

## <a name="page-route-action-conventions"></a><span data-ttu-id="0f141-190">Соглашения для действий с маршрутами страниц</span><span class="sxs-lookup"><span data-stu-id="0f141-190">Page route action conventions</span></span>

<span data-ttu-id="0f141-191">Поставщик модели маршрута по умолчанию, производный <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> от, вызывает соглашения, которые предназначены для предоставления точек расширения для настройки маршрутов страниц.</span><span class="sxs-lookup"><span data-stu-id="0f141-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="0f141-192">Соглашение о модели маршрута папки</span><span class="sxs-lookup"><span data-stu-id="0f141-192">Folder route model convention</span></span>

<span data-ttu-id="0f141-193">Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> для создания и добавления объекта <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , который вызывает действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> для всех страниц в указанной папке.</span><span class="sxs-lookup"><span data-stu-id="0f141-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="0f141-194">Пример приложения использует <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> для добавления шаблона маршрута `{otherPagesTemplate?}` к страницам в папке *OtherPages*:</span><span class="sxs-lookup"><span data-stu-id="0f141-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="0f141-195">Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `2`.</span><span class="sxs-lookup"><span data-stu-id="0f141-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="0f141-196">Это гарантирует, что шаблону `{globalTemplate?}` (установленному ранее в `1`разделе) присваивается приоритет для первой координаты значения данных маршрута, если указано одно значение маршрута.</span><span class="sxs-lookup"><span data-stu-id="0f141-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="0f141-197">Если страница в папке *pages/OtherPages* запрашивается с помощью значения параметра маршрута (например `/OtherPages/Page1/RouteDataValue`,), то "RouteDataValue" загружается в`Order = 1` `RouteData.Values["globalTemplate"]` () и `RouteData.Values["otherPagesTemplate"]` не`Order = 2`() из-за установки свойства `Order` свойство.</span><span class="sxs-lookup"><span data-stu-id="0f141-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="0f141-198">Везде, где это возможно, `Order`не устанавливайте, `Order = 0`что приводит к.</span><span class="sxs-lookup"><span data-stu-id="0f141-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="0f141-199">Чтобы выбрать правильный маршрут, используйте маршрутизацию.</span><span class="sxs-lookup"><span data-stu-id="0f141-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="0f141-200">Запросите страницу Page1 по адресу `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` из примера и проверьте результат:</span><span class="sxs-lookup"><span data-stu-id="0f141-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Страница Page1 в папке OtherPages запрашивается с сегментом маршрута GlobalRouteValue и OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="0f141-203">Соглашение о модели маршрута страницы</span><span class="sxs-lookup"><span data-stu-id="0f141-203">Page route model convention</span></span>

<span data-ttu-id="0f141-204">Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> для создания и добавления объекта <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , который вызывает действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> в для страницы с указанным именем.</span><span class="sxs-lookup"><span data-stu-id="0f141-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="0f141-205">Пример приложения использует `AddPageRouteModelConvention` для добавления шаблона маршрута `{aboutTemplate?}` к странице About:</span><span class="sxs-lookup"><span data-stu-id="0f141-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

<span data-ttu-id="0f141-206">Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `2`.</span><span class="sxs-lookup"><span data-stu-id="0f141-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="0f141-207">Это гарантирует, что шаблону `{globalTemplate?}` (установленному ранее в `1`разделе) присваивается приоритет для первой координаты значения данных маршрута, если указано одно значение маршрута.</span><span class="sxs-lookup"><span data-stu-id="0f141-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="0f141-208">Если страница `/About/RouteDataValue`about запрашивается со значением параметра маршрута, то "RouteDataValue" загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`) `Order` и не `RouteData.Values["aboutTemplate"]` (`Order = 2`) из-за задания свойства.</span><span class="sxs-lookup"><span data-stu-id="0f141-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="0f141-209">Везде, где это возможно, `Order`не устанавливайте, `Order = 0`что приводит к.</span><span class="sxs-lookup"><span data-stu-id="0f141-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="0f141-210">Чтобы выбрать правильный маршрут, используйте маршрутизацию.</span><span class="sxs-lookup"><span data-stu-id="0f141-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="0f141-211">Запросите страницу About по адресу `localhost:5000/About/GlobalRouteValue/AboutRouteValue` из примера и проверьте результат:</span><span class="sxs-lookup"><span data-stu-id="0f141-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![Страница About запрашивается с сегментами маршрута для GlobalRouteValue и AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="0f141-214">Использование преобразователя параметров для настройки маршрутов страниц</span><span class="sxs-lookup"><span data-stu-id="0f141-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="0f141-215">Маршруты страниц, созданные ASP.NET Core, можно настроить с помощью преобразователя параметров.</span><span class="sxs-lookup"><span data-stu-id="0f141-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="0f141-216">Преобразователь параметров реализует `IOutboundParameterTransformer` и преобразует значения параметров.</span><span class="sxs-lookup"><span data-stu-id="0f141-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="0f141-217">Например, пользовательский преобразователь параметра `SlugifyParameterTransformer` изменяет значение маршрута `SubscriptionManagement` на `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="0f141-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="0f141-218">Соглашение о модели маршрута страницыприменяетпреобразовательпараметровкименампапокиименфайловавтоматическисоздаваемыхмаршрутовстраницвприложении.`PageRouteTransformerConvention`</span><span class="sxs-lookup"><span data-stu-id="0f141-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="0f141-219">Например, файл Razor Pages в */Пажес/субскриптионманажемент/виевалл.кштмл* будет иметь перезапись маршрута из `/SubscriptionManagement/ViewAll` в. `/subscription-management/view-all`</span><span class="sxs-lookup"><span data-stu-id="0f141-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="0f141-220">`PageRouteTransformerConvention`преобразует только автоматически создаваемые сегменты маршрута страницы, полученные из папки Razor Pages и имени файла.</span><span class="sxs-lookup"><span data-stu-id="0f141-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="0f141-221">Он не преобразует сегменты маршрута, добавленные `@page` с помощью директивы.</span><span class="sxs-lookup"><span data-stu-id="0f141-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="0f141-222">Это соглашение также не преобразует маршруты, <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>добавленные.</span><span class="sxs-lookup"><span data-stu-id="0f141-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="0f141-223">Регистрируется в качестве параметра в `Startup.ConfigureServices`: `PageRouteTransformerConvention`</span><span class="sxs-lookup"><span data-stu-id="0f141-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="0f141-224">Настройка маршрута страницы</span><span class="sxs-lookup"><span data-stu-id="0f141-224">Configure a page route</span></span>

<span data-ttu-id="0f141-225">Используется <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> для настройки маршрута к странице по указанному пути страницы.</span><span class="sxs-lookup"><span data-stu-id="0f141-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="0f141-226">Созданные ссылки на страницу будут использовать заданный вами маршрут.</span><span class="sxs-lookup"><span data-stu-id="0f141-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="0f141-227">Для установления маршрута метод `AddPageRoute` использует `AddPageRouteModelConvention`.</span><span class="sxs-lookup"><span data-stu-id="0f141-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="0f141-228">Пример приложения создает маршрут к `/TheContactPage` для файла *Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0f141-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

<span data-ttu-id="0f141-229">На страницу Contact также можно перейти с помощью `/Contact` через маршрут по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0f141-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="0f141-230">Настраиваемый маршрут на страницу Contact в примере приложения позволяет использовать необязательный сегмент маршрута `text` (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="0f141-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="0f141-231">Страница также включает этот необязательный сегмент в своей директиве `@page` на случай, если посетитель переходит на страницу по маршруту `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="0f141-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

<span data-ttu-id="0f141-232">Обратите внимание, что URL-адрес, созданный для ссылки **Contact** на отображенной странице, отражает обновленный маршрут:</span><span class="sxs-lookup"><span data-stu-id="0f141-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Ссылка Contact на панели навигации в примере приложения](razor-pages-conventions/_static/contact-link.png)

![В ссылке Contact в показанном HTML видно, что href имеет значение "/TheContactPage".](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="0f141-235">Перейдите на страницу Contact по ее обычному маршруту `/Contact` или по настроенному маршруту `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="0f141-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="0f141-236">Если вы укажете дополнительный сегмент маршрута `text`, страница отобразит этот сегмент в HTML-кодировке:</span><span class="sxs-lookup"><span data-stu-id="0f141-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Пример в браузере Microsoft Edge с указанием необязательного сегмента маршрута "text" со значением "TextValue" в URL-адресе.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="0f141-239">Соглашения для действий модели страниц</span><span class="sxs-lookup"><span data-stu-id="0f141-239">Page model action conventions</span></span>

<span data-ttu-id="0f141-240">Поставщик модели страницы по умолчанию, <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> реализующий соглашения о вызовах, которые предназначены для предоставления точек расширения для настройки моделей страниц.</span><span class="sxs-lookup"><span data-stu-id="0f141-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="0f141-241">Эти соглашения полезны при создании и изменении для обнаружения и обработки страниц.</span><span class="sxs-lookup"><span data-stu-id="0f141-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="0f141-242">В примерах, приведенных в этом разделе, в примере `AddHeaderAttribute` приложения используется класс <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, который применяет заголовок ответа:</span><span class="sxs-lookup"><span data-stu-id="0f141-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0f141-243">Образец показывает использование соглашений для применения атрибута ко всем страницам в папке и к одной странице.</span><span class="sxs-lookup"><span data-stu-id="0f141-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="0f141-244">**Соглашение для модели приложений папки**</span><span class="sxs-lookup"><span data-stu-id="0f141-244">**Folder app model convention**</span></span>

<span data-ttu-id="0f141-245">Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> для создания и добавления объекта <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , который <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> вызывает действие для экземпляров для всех страниц в указанной папке.</span><span class="sxs-lookup"><span data-stu-id="0f141-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="0f141-246">Пример демонстрирует использование `AddFolderApplicationModelConvention` путем добавления заголовка `OtherPagesHeader` к страницам в папке *OtherPages* приложения:</span><span class="sxs-lookup"><span data-stu-id="0f141-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

<span data-ttu-id="0f141-247">Запросите страницу Page1 по адресу `localhost:5000/OtherPages/Page1` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="0f141-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Заголовки ответа страницы OtherPages/Page1, показывающие добавленный заголовок OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="0f141-249">**Соглашение для модели приложений страницы**</span><span class="sxs-lookup"><span data-stu-id="0f141-249">**Page app model convention**</span></span>

<span data-ttu-id="0f141-250">Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> для создания и добавления объекта <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , который вызывает действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> в для страницы с указанным именем.</span><span class="sxs-lookup"><span data-stu-id="0f141-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="0f141-251">Пример демонстрирует использование `AddPageApplicationModelConvention` путем добавления заголовка `AboutHeader` на страницу About:</span><span class="sxs-lookup"><span data-stu-id="0f141-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

<span data-ttu-id="0f141-252">Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="0f141-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Заголовки ответа страницы About, показывающие, что добавлен заголовок AboutHeader.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="0f141-254">**Настройка фильтра**</span><span class="sxs-lookup"><span data-stu-id="0f141-254">**Configure a filter**</span></span>

<span data-ttu-id="0f141-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>настраивает указанный фильтр для применения.</span><span class="sxs-lookup"><span data-stu-id="0f141-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="0f141-256">Вы можете создать класс фильтра, но пример приложения показывает реализацию фильтра в лямбда-выражении, которое реализовано "за кулисами" в качестве фабрики, возвращающей фильтр:</span><span class="sxs-lookup"><span data-stu-id="0f141-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

<span data-ttu-id="0f141-257">Модель приложений страницы используется для проверки относительного пути на наличие сегментов, ведущих к странице Page2 в папке *OtherPages*.</span><span class="sxs-lookup"><span data-stu-id="0f141-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="0f141-258">Если условие выполняется, добавляется заголовок.</span><span class="sxs-lookup"><span data-stu-id="0f141-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="0f141-259">Если нет, применяется `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="0f141-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="0f141-260">`EmptyFilter` является [фильтром действий](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="0f141-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="0f141-261">Так как фильтры действий игнорируются Razor Pages, параметр `EmptyFilter` не действует так, как предполагалось, если `OtherPages/Page2`путь не содержит.</span><span class="sxs-lookup"><span data-stu-id="0f141-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="0f141-262">Запросите страницу Page2 по адресу `localhost:5000/OtherPages/Page2` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="0f141-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![В ответ для страницы Page2 добавлен заголовок OtherPagesPage2Header.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="0f141-264">**Настройка фабрики фильтров**</span><span class="sxs-lookup"><span data-stu-id="0f141-264">**Configure a filter factory**</span></span>

<span data-ttu-id="0f141-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>настраивает указанную фабрику для применения [фильтров](xref:mvc/controllers/filters) ко всем Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0f141-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="0f141-266">В примере приложения приведен образец использования [фабрики фильтров](xref:mvc/controllers/filters#ifilterfactory) путем добавления на страницы приложения заголовка `FilterFactoryHeader` с двумя значениями:</span><span class="sxs-lookup"><span data-stu-id="0f141-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

<span data-ttu-id="0f141-267">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="0f141-267">*AddHeaderWithFactory.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0f141-268">Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="0f141-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Заголовки ответов на странице About показывают два добавленных заголовка FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="0f141-270">Фильтры MVC и фильтр страниц (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="0f141-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="0f141-271">[Фильтры действий](xref:mvc/controllers/filters#action-filters) MVC игнорируются в Razor Pages, так как Razor Pages использует методы обработчиков.</span><span class="sxs-lookup"><span data-stu-id="0f141-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="0f141-272">Другие типы фильтров MVC доступны для использования: [Авторизация](xref:mvc/controllers/filters#authorization-filters), [исключение](xref:mvc/controllers/filters#exception-filters), [ресурс](xref:mvc/controllers/filters#resource-filters)и [результат](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="0f141-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="0f141-273">Дополнительные сведения см. в разделе [Фильтры](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="0f141-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="0f141-274">Фильтр страницы (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) — это фильтр, который применяется к Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0f141-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="0f141-275">Дополнительные сведения см. в разделе [Методы фильтрации для Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="0f141-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0f141-276">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0f141-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
