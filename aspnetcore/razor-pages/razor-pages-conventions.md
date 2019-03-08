---
title: Соглашения для маршрутов и приложений Razor Pages в ASP.NET Core
author: guardrex
description: Узнайте, как использовать соглашения поставщика модели маршрутов и приложений для управления маршрутизацией, обнаружением и обработкой страниц.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/07/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: c160d93e22fc5b3511ba4e5539cce8576346898b
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665553"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="1958c-103">Соглашения для маршрутов и приложений Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1958c-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="1958c-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="1958c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1958c-105">Узнайте, как использовать [соглашения поставщика модели маршрутов и приложений](xref:mvc/controllers/application-model#conventions) страниц для управления маршрутизацией, обнаружением и обработкой страниц в приложениях Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1958c-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="1958c-106">Когда вам необходимо настроить уникальные маршруты для отдельных страниц, используйте [соглашение AddPageRoute](#configure-a-page-route), описанное далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="1958c-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="1958c-107">Чтобы указать маршрута страницы, добавление сегментами маршрута или добавить параметры для маршрута, используйте страницы `@page` директива.</span><span class="sxs-lookup"><span data-stu-id="1958c-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="1958c-108">Дополнительные сведения см. в разделе [пользовательские маршруты](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="1958c-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="1958c-109">Существуют зарезервированные слова, которые нельзя использовать в качестве сегментами маршрута или имена параметров.</span><span class="sxs-lookup"><span data-stu-id="1958c-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="1958c-110">Дополнительные сведения см. в разделе [маршрутизации: Зарезервированные имена маршрутизации](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="1958c-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="1958c-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1958c-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="1958c-112">Сценарий</span><span class="sxs-lookup"><span data-stu-id="1958c-112">Scenario</span></span> | <span data-ttu-id="1958c-113">Что демонстрирует пример</span><span class="sxs-lookup"><span data-stu-id="1958c-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="1958c-114">Соглашения для моделей</span><span class="sxs-lookup"><span data-stu-id="1958c-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="1958c-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="1958c-115">Conventions.Add</span></span><ul><li><span data-ttu-id="1958c-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="1958c-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="1958c-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="1958c-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="1958c-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="1958c-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="1958c-119">Добавление шаблона маршрута и заголовка к страницам приложения.</span><span class="sxs-lookup"><span data-stu-id="1958c-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="1958c-120">Соглашения для действий с маршрутами страниц</span><span class="sxs-lookup"><span data-stu-id="1958c-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="1958c-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="1958c-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="1958c-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="1958c-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="1958c-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="1958c-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="1958c-124">Добавление шаблона маршрута к страницам в папке и к одной странице.</span><span class="sxs-lookup"><span data-stu-id="1958c-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="1958c-125">Соглашения для действий модели страниц</span><span class="sxs-lookup"><span data-stu-id="1958c-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="1958c-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="1958c-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="1958c-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="1958c-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="1958c-128">ConfigureFilter (класс фильтра, лямбда-выражение или фабрика фильтров)</span><span class="sxs-lookup"><span data-stu-id="1958c-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="1958c-129">Добавление заголовка к страницам в папке, добавление заголовка к одной странице и настройка [фабрики фильтров](xref:mvc/controllers/filters#ifilterfactory) для добавления заголовка к страницам приложения.</span><span class="sxs-lookup"><span data-stu-id="1958c-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="1958c-130">Соглашения о страницах Razor добавляются и настроить с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> метод расширения для <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> в сбор службы `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="1958c-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="1958c-131">Следующие примеры соглашений описаны далее в этом разделе:</span><span class="sxs-lookup"><span data-stu-id="1958c-131">The following convention examples are explained later in this topic:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a><span data-ttu-id="1958c-132">Порядок маршрута</span><span class="sxs-lookup"><span data-stu-id="1958c-132">Route order</span></span>

<span data-ttu-id="1958c-133">Маршруты указывают <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для обработки (сопоставление маршрутов).</span><span class="sxs-lookup"><span data-stu-id="1958c-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="1958c-134">Номер</span><span class="sxs-lookup"><span data-stu-id="1958c-134">Order</span></span>            | <span data-ttu-id="1958c-135">Поведение</span><span class="sxs-lookup"><span data-stu-id="1958c-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="1958c-136">-1</span><span class="sxs-lookup"><span data-stu-id="1958c-136">-1</span></span>               | <span data-ttu-id="1958c-137">Маршрут обрабатывается перед обработкой других маршрутов.</span><span class="sxs-lookup"><span data-stu-id="1958c-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="1958c-138">0</span><span class="sxs-lookup"><span data-stu-id="1958c-138">0</span></span>                | <span data-ttu-id="1958c-139">Порядок не указан (значение по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="1958c-139">Order isn't specified (default value).</span></span> <span data-ttu-id="1958c-140">Не назначая `Order` (`Order = null`) по умолчанию маршрут `Order` 0 (ноль) для обработки.</span><span class="sxs-lookup"><span data-stu-id="1958c-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="1958c-141">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="1958c-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="1958c-142">Указывает порядок обработки маршрута.</span><span class="sxs-lookup"><span data-stu-id="1958c-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="1958c-143">Обработка маршрутов устанавливается в соответствии с соглашением:</span><span class="sxs-lookup"><span data-stu-id="1958c-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="1958c-144">Маршруты обрабатываются в последовательном порядке (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="1958c-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="1958c-145">Когда маршруты имеют одинаковые `Order`, наиболее конкретный маршрут соответствует после менее определенных маршрутов.</span><span class="sxs-lookup"><span data-stu-id="1958c-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="1958c-146">Когда маршруты с тем же `Order` и URL-адрес запроса соответствует одинаковое число параметров, маршруты обрабатываются в порядке, в котором они добавляются к <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span><span class="sxs-lookup"><span data-stu-id="1958c-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="1958c-147">По возможности избегайте в зависимости от установленного маршрута обработки заказа.</span><span class="sxs-lookup"><span data-stu-id="1958c-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="1958c-148">Как правило Маршрутизация выбирает соответствующий маршрут с соответствующими URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="1958c-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="1958c-149">Если необходимо задать маршрут `Order` свойства для маршрутизации запросов правильно, маршрутизации схему приложения, вероятно, заблуждение клиентов и уязвимости для поддержания.</span><span class="sxs-lookup"><span data-stu-id="1958c-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="1958c-150">Поиск для упрощения маршрутизации схемы приложения.</span><span class="sxs-lookup"><span data-stu-id="1958c-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="1958c-151">Пример приложения требуется явный маршрут обработки заказа, чтобы продемонстрировать несколько сценариев маршрутизации, с помощью одного приложения.</span><span class="sxs-lookup"><span data-stu-id="1958c-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="1958c-152">Тем не менее, стоит пытаться избежать практика параметр маршрута `Order` в рабочих приложениях.</span><span class="sxs-lookup"><span data-stu-id="1958c-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="1958c-153">Средства маршрутизации в Razor Pages и контроллере MVC имеют общую реализацию.</span><span class="sxs-lookup"><span data-stu-id="1958c-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="1958c-154">Информация на порядок маршрута в разделах MVC доступна на [Маршрутизация к действиям контроллера: Упорядочение маршрутов на основе атрибутов](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="1958c-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="1958c-155">Соглашения для моделей</span><span class="sxs-lookup"><span data-stu-id="1958c-155">Model conventions</span></span>

<span data-ttu-id="1958c-156">Добавьте делегат для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> добавление [модели соглашения](xref:mvc/controllers/application-model#conventions) , применяемые к Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1958c-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="1958c-157">Добавление соглашения для модели маршрутов ко всем страницам</span><span class="sxs-lookup"><span data-stu-id="1958c-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="1958c-158">Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> в коллекцию <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> экземпляров, которые применяются при маршрута страницы модели конструкции.</span><span class="sxs-lookup"><span data-stu-id="1958c-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="1958c-159">В примере приложения шаблон маршрута `{globalTemplate?}` добавляется ко всем страницам приложения:</span><span class="sxs-lookup"><span data-stu-id="1958c-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="1958c-160">Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `1`.</span><span class="sxs-lookup"><span data-stu-id="1958c-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="1958c-161">Это гарантирует следующий маршрут, соответствующий поведения в примере приложения:</span><span class="sxs-lookup"><span data-stu-id="1958c-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="1958c-162">Шаблон маршрута для `TheContactPage/{text?}` добавляется в разделе ниже.</span><span class="sxs-lookup"><span data-stu-id="1958c-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="1958c-163">Обратитесь к странице маршрут имеет порядок по умолчанию `null` (`Order = 0`), чтобы он соответствовал перед `{globalTemplate?}` шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="1958c-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="1958c-164">`{aboutTemplate?}` Далее в этом разделе добавляется шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="1958c-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="1958c-165">Шаблону `{aboutTemplate?}` задано свойство `Order`, равное `2`.</span><span class="sxs-lookup"><span data-stu-id="1958c-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="1958c-166">При запросе страницы About по адресу `/About/RouteDataValue` значение RouteDataValue загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`), а не в `RouteData.Values["aboutTemplate"]` (`Order = 2`). Это происходит из-за значения свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="1958c-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="1958c-167">`{otherPagesTemplate?}` Далее в этом разделе добавляется шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="1958c-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="1958c-168">Шаблону `{otherPagesTemplate?}` задано свойство `Order`, равное `2`.</span><span class="sxs-lookup"><span data-stu-id="1958c-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="1958c-169">При любой страницы в *страниц/OtherPages* папку запрашивается с параметром маршрута (например, `/OtherPages/Page1/RouteDataValue`), «RouteDataValue» загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`) и не `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) из-за параметра `Order` свойство.</span><span class="sxs-lookup"><span data-stu-id="1958c-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="1958c-170">Везде, где это возможно, не устанавливайте `Order`, что приводит к `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="1958c-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="1958c-171">Используют маршрутов, чтобы выбрать правильный маршрут.</span><span class="sxs-lookup"><span data-stu-id="1958c-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="1958c-172">Параметры страницы Razor, такие как добавление <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, добавляются при добавлении в коллекцию служб в MVC `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1958c-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1958c-173">Пример см. в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="1958c-173">For an example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="1958c-174">Запросите страницу About по адресу `localhost:5000/About/GlobalRouteValue` из примера и проверьте результат:</span><span class="sxs-lookup"><span data-stu-id="1958c-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Запрос страницы About с сегментом маршрута GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="1958c-177">Добавить соглашение для модели приложений ко всем страницам</span><span class="sxs-lookup"><span data-stu-id="1958c-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="1958c-178">Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> в коллекцию <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> экземпляров, которые применяются при странице конструкции модели приложения.</span><span class="sxs-lookup"><span data-stu-id="1958c-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="1958c-179">Для демонстрации этих и других соглашений далее в этом разделе в пример приложения включен класс `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="1958c-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="1958c-180">Конструктор класса принимает строку `name` и массив строк `values`.</span><span class="sxs-lookup"><span data-stu-id="1958c-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="1958c-181">Эти значения используются в его методе `OnResultExecuting` для задания заголовка ответа.</span><span class="sxs-lookup"><span data-stu-id="1958c-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="1958c-182">Класс показан полностью в разделе [Соглашения для действий модели страниц](#page-model-action-conventions) далее.</span><span class="sxs-lookup"><span data-stu-id="1958c-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="1958c-183">Пример приложения использует класс `AddHeaderAttribute`, чтобы добавить заголовок `GlobalHeader` ко всем страницам в приложении:</span><span class="sxs-lookup"><span data-stu-id="1958c-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="1958c-184">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1958c-184">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="1958c-185">Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="1958c-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Заголовки ответа страницы About, показывающие, что добавлен заголовок GlobalHeader.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="1958c-187">Добавление обработчика модели соглашение ко всем страницам</span><span class="sxs-lookup"><span data-stu-id="1958c-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="1958c-188">Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> в коллекцию <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> экземпляров, которые применяются при странице обработчик построения модели.</span><span class="sxs-lookup"><span data-stu-id="1958c-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="1958c-189">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1958c-189">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="1958c-190">Соглашения для действий с маршрутами страниц</span><span class="sxs-lookup"><span data-stu-id="1958c-190">Page route action conventions</span></span>

<span data-ttu-id="1958c-191">Поставщик модели маршрутов по умолчанию, который является производным от <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> вызывает соглашения, которые предназначены для предоставления точек расширения для настройки маршрутов страниц.</span><span class="sxs-lookup"><span data-stu-id="1958c-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="1958c-192">Соглашение для модели маршрутов папки</span><span class="sxs-lookup"><span data-stu-id="1958c-192">Folder route model convention</span></span>

<span data-ttu-id="1958c-193">Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , вызывающий действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> для всех страниц в указанной папке.</span><span class="sxs-lookup"><span data-stu-id="1958c-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="1958c-194">Пример приложения использует <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> для добавления шаблона маршрута `{otherPagesTemplate?}` к страницам в папке *OtherPages*:</span><span class="sxs-lookup"><span data-stu-id="1958c-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="1958c-195">Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `2`.</span><span class="sxs-lookup"><span data-stu-id="1958c-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="1958c-196">Это гарантирует, что шаблон для `{globalTemplate?}` (ранее в этом разделе, чтобы задать `1`) имеет преимущество — для первого значение данных маршрута при указании одного значения маршрута предоставляется.</span><span class="sxs-lookup"><span data-stu-id="1958c-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="1958c-197">Если страница в *страниц/OtherPages* папку запрашивается со значением параметра маршрута (например, `/OtherPages/Page1/RouteDataValue`), «RouteDataValue» загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`) и не `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) из-за параметра `Order` свойство.</span><span class="sxs-lookup"><span data-stu-id="1958c-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="1958c-198">Везде, где это возможно, не устанавливайте `Order`, что приводит к `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="1958c-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="1958c-199">Используют маршрутов, чтобы выбрать правильный маршрут.</span><span class="sxs-lookup"><span data-stu-id="1958c-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="1958c-200">Запросите страницу Page1 по адресу `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` из примера и проверьте результат:</span><span class="sxs-lookup"><span data-stu-id="1958c-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Страница Page1 в папке OtherPages запрашивается с сегментом маршрута GlobalRouteValue и OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="1958c-203">Соглашение для модели маршрутов страницы</span><span class="sxs-lookup"><span data-stu-id="1958c-203">Page route model convention</span></span>

<span data-ttu-id="1958c-204">Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , вызывающий действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> для страницы с указанным именем.</span><span class="sxs-lookup"><span data-stu-id="1958c-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="1958c-205">Пример приложения использует `AddPageRouteModelConvention` для добавления шаблона маршрута `{aboutTemplate?}` к странице About:</span><span class="sxs-lookup"><span data-stu-id="1958c-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="1958c-206">Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `2`.</span><span class="sxs-lookup"><span data-stu-id="1958c-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="1958c-207">Это гарантирует, что шаблон для `{globalTemplate?}` (ранее в этом разделе, чтобы задать `1`) имеет преимущество — для первого значение данных маршрута при указании одного значения маршрута предоставляется.</span><span class="sxs-lookup"><span data-stu-id="1958c-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="1958c-208">При запросе страницы About со значением параметра маршрута в `/About/RouteDataValue`, «RouteDataValue» загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`) и не `RouteData.Values["aboutTemplate"]` (`Order = 2`) из-за параметра `Order` свойство.</span><span class="sxs-lookup"><span data-stu-id="1958c-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="1958c-209">Везде, где это возможно, не устанавливайте `Order`, что приводит к `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="1958c-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="1958c-210">Используют маршрутов, чтобы выбрать правильный маршрут.</span><span class="sxs-lookup"><span data-stu-id="1958c-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="1958c-211">Запросите страницу About по адресу `localhost:5000/About/GlobalRouteValue/AboutRouteValue` из примера и проверьте результат:</span><span class="sxs-lookup"><span data-stu-id="1958c-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![Страница About запрашивается с сегментами маршрута для GlobalRouteValue и AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="1958c-214">Использовать параметр преобразователя для настройки маршрутов страниц</span><span class="sxs-lookup"><span data-stu-id="1958c-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="1958c-215">Страница маршруты, созданный ASP.NET Core можно настроить, используя параметр преобразователя.</span><span class="sxs-lookup"><span data-stu-id="1958c-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="1958c-216">Преобразователь параметров реализует `IOutboundParameterTransformer` и преобразует значения параметров.</span><span class="sxs-lookup"><span data-stu-id="1958c-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="1958c-217">Например, пользовательский преобразователь параметра `SlugifyParameterTransformer` изменяет значение маршрута `SubscriptionManagement` на `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="1958c-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="1958c-218">`PageRouteTransformerConvention` Соглашение для модели маршрутов страницы применяется параметр преобразователя от сегментов имени файлов и папок автоматически созданные маршруты в приложении.</span><span class="sxs-lookup"><span data-stu-id="1958c-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="1958c-219">Например, файл страницы Razor Pages в */Pages/SubscriptionManagement/ViewAll.cshtml* бы маршрута из переписать `/SubscriptionManagement/ViewAll` для `/subscription-management/view-all`.</span><span class="sxs-lookup"><span data-stu-id="1958c-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="1958c-220">`PageRouteTransformerConvention` только преобразует автоматически созданный сегменты маршрута страницы, полученные из папки Razor Pages и имя файла.</span><span class="sxs-lookup"><span data-stu-id="1958c-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="1958c-221">Он не преобразует сегментами маршрута, добавленные с помощью `@page` директива.</span><span class="sxs-lookup"><span data-stu-id="1958c-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="1958c-222">Соглашение также не преобразует маршруты, добавленные с <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span><span class="sxs-lookup"><span data-stu-id="1958c-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="1958c-223">`PageRouteTransformerConvention` Зарегистрирован в качестве параметра в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1958c-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="1958c-224">Настройка маршрута страницы</span><span class="sxs-lookup"><span data-stu-id="1958c-224">Configure a page route</span></span>

<span data-ttu-id="1958c-225">Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> для настройки маршрута на страницу по указанному пути страницы.</span><span class="sxs-lookup"><span data-stu-id="1958c-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="1958c-226">Созданные ссылки на страницу будут использовать заданный вами маршрут.</span><span class="sxs-lookup"><span data-stu-id="1958c-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="1958c-227">Для установления маршрута метод `AddPageRoute` использует `AddPageRouteModelConvention`.</span><span class="sxs-lookup"><span data-stu-id="1958c-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="1958c-228">Пример приложения создает маршрут к `/TheContactPage` для файла *Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1958c-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="1958c-229">На страницу Contact также можно перейти с помощью `/Contact` через маршрут по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1958c-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="1958c-230">Настраиваемый маршрут на страницу Contact в примере приложения позволяет использовать необязательный сегмент маршрута `text` (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="1958c-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="1958c-231">Страница также включает этот необязательный сегмент в своей директиве `@page` на случай, если посетитель переходит на страницу по маршруту `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="1958c-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="1958c-232">Обратите внимание, что URL-адрес, созданный для ссылки **Contact** на отображенной странице, отражает обновленный маршрут:</span><span class="sxs-lookup"><span data-stu-id="1958c-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Ссылка Contact на панели навигации в примере приложения](razor-pages-conventions/_static/contact-link.png)

![В ссылке Contact в показанном HTML видно, что href имеет значение "/TheContactPage".](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="1958c-235">Перейдите на страницу Contact по ее обычному маршруту `/Contact` или по настроенному маршруту `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="1958c-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="1958c-236">Если вы укажете дополнительный сегмент маршрута `text`, страница отобразит этот сегмент в HTML-кодировке:</span><span class="sxs-lookup"><span data-stu-id="1958c-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Пример в браузере Microsoft Edge с указанием необязательного сегмента маршрута "text" со значением "TextValue" в URL-адресе.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="1958c-239">Соглашения для действий модели страниц</span><span class="sxs-lookup"><span data-stu-id="1958c-239">Page model action conventions</span></span>

<span data-ttu-id="1958c-240">Поставщик модели страниц по умолчанию, который реализует <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> вызывает соглашения, которые предназначены для предоставления точек расширения для настройки моделей страниц.</span><span class="sxs-lookup"><span data-stu-id="1958c-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="1958c-241">Эти соглашения полезны при создании и изменении для обнаружения и обработки страниц.</span><span class="sxs-lookup"><span data-stu-id="1958c-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="1958c-242">Примеры в этом разделе, пример приложения использует `AddHeaderAttribute` класс, являющийся <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, который применяет заголовок ответа:</span><span class="sxs-lookup"><span data-stu-id="1958c-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="1958c-243">Образец показывает использование соглашений для применения атрибута ко всем страницам в папке и к одной странице.</span><span class="sxs-lookup"><span data-stu-id="1958c-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="1958c-244">**Соглашение для модели приложений папки**</span><span class="sxs-lookup"><span data-stu-id="1958c-244">**Folder app model convention**</span></span>

<span data-ttu-id="1958c-245">Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , вызывающий действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> экземпляры для всех страниц в указанной папке.</span><span class="sxs-lookup"><span data-stu-id="1958c-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="1958c-246">Пример демонстрирует использование `AddFolderApplicationModelConvention` путем добавления заголовка `OtherPagesHeader` к страницам в папке *OtherPages* приложения:</span><span class="sxs-lookup"><span data-stu-id="1958c-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="1958c-247">Запросите страницу Page1 по адресу `localhost:5000/OtherPages/Page1` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="1958c-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Заголовки ответа страницы OtherPages/Page1, показывающие добавленный заголовок OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="1958c-249">**Соглашение для модели приложений страницы**</span><span class="sxs-lookup"><span data-stu-id="1958c-249">**Page app model convention**</span></span>

<span data-ttu-id="1958c-250">Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , вызывающий действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> для страницы с указанным именем.</span><span class="sxs-lookup"><span data-stu-id="1958c-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="1958c-251">Пример демонстрирует использование `AddPageApplicationModelConvention` путем добавления заголовка `AboutHeader` на страницу About:</span><span class="sxs-lookup"><span data-stu-id="1958c-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="1958c-252">Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="1958c-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Заголовки ответа страницы About, показывающие, что добавлен заголовок AboutHeader.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="1958c-254">**Настройка фильтра**</span><span class="sxs-lookup"><span data-stu-id="1958c-254">**Configure a filter**</span></span>

<span data-ttu-id="1958c-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Настраивает указанный фильтр для применения.</span><span class="sxs-lookup"><span data-stu-id="1958c-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="1958c-256">Вы можете создать класс фильтра, но пример приложения показывает реализацию фильтра в лямбда-выражении, которое реализовано "за кулисами" в качестве фабрики, возвращающей фильтр:</span><span class="sxs-lookup"><span data-stu-id="1958c-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="1958c-257">Модель приложений страницы используется для проверки относительного пути на наличие сегментов, ведущих к странице Page2 в папке *OtherPages*.</span><span class="sxs-lookup"><span data-stu-id="1958c-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="1958c-258">Если условие выполняется, добавляется заголовок.</span><span class="sxs-lookup"><span data-stu-id="1958c-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="1958c-259">Если нет, применяется `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="1958c-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="1958c-260">`EmptyFilter` является [фильтром действий](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="1958c-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="1958c-261">Поскольку в Razor Pages фильтры действий не учитываются, `EmptyFilter` является "холостой командой", как и задумано, если путь не содержит `OtherPages/Page2`.</span><span class="sxs-lookup"><span data-stu-id="1958c-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` no-ops as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="1958c-262">Запросите страницу Page2 по адресу `localhost:5000/OtherPages/Page2` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="1958c-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![В ответ для страницы Page2 добавлен заголовок OtherPagesPage2Header.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="1958c-264">**Настройка фабрики фильтров**</span><span class="sxs-lookup"><span data-stu-id="1958c-264">**Configure a filter factory**</span></span>

<span data-ttu-id="1958c-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Настраивает указанную фабрику на применение [фильтры](xref:mvc/controllers/filters) ко всем страницам Razor.</span><span class="sxs-lookup"><span data-stu-id="1958c-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="1958c-266">В примере приложения приведен образец использования [фабрики фильтров](xref:mvc/controllers/filters#ifilterfactory) путем добавления на страницы приложения заголовка `FilterFactoryHeader` с двумя значениями:</span><span class="sxs-lookup"><span data-stu-id="1958c-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="1958c-267">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="1958c-267">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="1958c-268">Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:</span><span class="sxs-lookup"><span data-stu-id="1958c-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Заголовки ответов на странице About показывают два добавленных заголовка FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="1958c-270">Фильтры MVC и фильтр страниц (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="1958c-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="1958c-271">[Фильтры действий](xref:mvc/controllers/filters#action-filters) MVC игнорируются в Razor Pages, так как Razor Pages использует методы обработчиков.</span><span class="sxs-lookup"><span data-stu-id="1958c-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="1958c-272">Для использования доступны другие типы фильтров MVC: [Авторизация](xref:mvc/controllers/filters#authorization-filters), [исключение](xref:mvc/controllers/filters#exception-filters), [ресурсов](xref:mvc/controllers/filters#resource-filters), и [результат](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="1958c-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="1958c-273">Дополнительные сведения см. в разделе [Фильтры](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="1958c-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="1958c-274">Фильтр страниц (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) представляет собой фильтр, применяемый для Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1958c-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="1958c-275">Дополнительные сведения см. в разделе [Методы фильтрации для Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="1958c-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1958c-276">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1958c-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
