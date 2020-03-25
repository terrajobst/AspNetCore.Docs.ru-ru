---
title: Маршрутизация к действиям контроллера в ASP.NET Core
author: rick-anderson
description: Узнайте, как в MVC ASP.NET Core используется ПО промежуточного слоя маршрутизации для сопоставления URL-адресов входящих запросов с действиями.
ms.author: riande
ms.date: 3/25/2020
uid: mvc/controllers/routing
ms.openlocfilehash: be7da9eeaf64c2f52c095b5179ccc22db43d57c3
ms.sourcegitcommit: 99e71ae03319ab386baf2ebde956fc2d511df8b8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2020
ms.locfileid: "80242583"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="e43a5-103">Маршрутизация к действиям контроллера в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e43a5-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="e43a5-104">[Райан Nowak)](https://github.com/rynowak), [Kirk Ларкин](https://twitter.com/serpent5)и [Рик Андерсон (](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e43a5-104">By [Ryan Nowak](https://github.com/rynowak), [Kirk Larkin](https://twitter.com/serpent5), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e43a5-105">Контроллеры ASP.NET Core используют по [промежуточного слоя](xref:fundamentals/middleware/index) маршрутизации для сопоставления URL-адресов входящих запросов и сопоставления их с [действиями](#action).</span><span class="sxs-lookup"><span data-stu-id="e43a5-105">ASP.NET Core controllers use the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to [actions](#action).</span></span>  <span data-ttu-id="e43a5-106">Шаблоны маршрутов:</span><span class="sxs-lookup"><span data-stu-id="e43a5-106">Routes templates:</span></span>

* <span data-ttu-id="e43a5-107">Определяются в коде запуска или атрибутах.</span><span class="sxs-lookup"><span data-stu-id="e43a5-107">Are defined in startup code or attributes.</span></span>
* <span data-ttu-id="e43a5-108">Описание способов сопоставления путей URL-адресов с [действиями](#action).</span><span class="sxs-lookup"><span data-stu-id="e43a5-108">Describe how URL paths are matched to [actions](#action).</span></span>
* <span data-ttu-id="e43a5-109">Используются для создания URL-адресов для ссылок.</span><span class="sxs-lookup"><span data-stu-id="e43a5-109">Are used to generate URLs for links.</span></span> <span data-ttu-id="e43a5-110">Созданные ссылки обычно возвращаются в ответах.</span><span class="sxs-lookup"><span data-stu-id="e43a5-110">The generated links are typically returned in responses.</span></span>

<span data-ttu-id="e43a5-111">Действия либо [направляются по соглашению](#cr) , либо [маршрутизируются по атрибуту](#ar).</span><span class="sxs-lookup"><span data-stu-id="e43a5-111">Actions are either [conventionally-routed](#cr) or [attribute-routed](#ar).</span></span> <span data-ttu-id="e43a5-112">При размещении маршрута на контроллере или [действии](#action) он пересылается по атрибуту.</span><span class="sxs-lookup"><span data-stu-id="e43a5-112">Placing a route on the controller or [action](#action) makes it attribute-routed.</span></span> <span data-ttu-id="e43a5-113">Дополнительные сведения см. в разделе [Смешанная маршрутизация](#routing-mixed-ref-label).</span><span class="sxs-lookup"><span data-stu-id="e43a5-113">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="e43a5-114">Этот документ:</span><span class="sxs-lookup"><span data-stu-id="e43a5-114">This document:</span></span>

* <span data-ttu-id="e43a5-115">Объясняет взаимодействие между MVC и маршрутизацией.</span><span class="sxs-lookup"><span data-stu-id="e43a5-115">Explains the interactions between MVC and routing:</span></span>
  * <span data-ttu-id="e43a5-116">Как обычные приложения MVC используют функции маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-116">How typical MVC apps make use of routing features.</span></span>
  * <span data-ttu-id="e43a5-117">Рассматриваются оба:</span><span class="sxs-lookup"><span data-stu-id="e43a5-117">Covers both:</span></span>
    * <span data-ttu-id="e43a5-118">[Соглашение о маршрутизации](#cr) обычно используется с контроллерами и представлениями.</span><span class="sxs-lookup"><span data-stu-id="e43a5-118">[Conventionally routing](#cr) typically used with controllers and views.</span></span>
    * <span data-ttu-id="e43a5-119">*Маршрутизация атрибутов* , используемая с API-интерфейсами RESTful.</span><span class="sxs-lookup"><span data-stu-id="e43a5-119">*Attribute routing* used with REST APIs.</span></span> <span data-ttu-id="e43a5-120">Если вы в основном заинтересованы в маршрутизации для API-интерфейсов RESTFUL, перейдите к разделу [Маршрутизация атрибутов для интерфейсов API для остальных](#ar) компонентов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-120">If you're primarily interested in routing for REST APIs, jump to the [Attribute routing for REST APIs](#ar) section.</span></span>
  * <span data-ttu-id="e43a5-121">Дополнительные сведения о маршрутизации см. в разделе [Маршрутизация](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="e43a5-121">See [Routing](xref:fundamentals/routing) for advanced routing details.</span></span>
* <span data-ttu-id="e43a5-122">Относится к системе маршрутизации по умолчанию, добавленной в ASP.NET Core 3,0, называемую маршрутизацией конечных точек.</span><span class="sxs-lookup"><span data-stu-id="e43a5-122">Refers to the default routing system added in ASP.NET Core 3.0, called endpoint routing.</span></span> <span data-ttu-id="e43a5-123">В целях совместимости можно использовать контроллеры с предыдущей версией маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-123">It's possible to use controllers with the previous version of routing for compatibility purposes.</span></span> <span data-ttu-id="e43a5-124">Инструкции см. в [руководству по миграции 2.2-3.0](xref:migration/22-to-30) .</span><span class="sxs-lookup"><span data-stu-id="e43a5-124">See the [2.2-3.0 migration guide](xref:migration/22-to-30) for instructions.</span></span> <span data-ttu-id="e43a5-125">Справочные материалы по устаревшей системе маршрутизации см. в [версии 2,2 этого документа](xref:mvc/controllers/routing?view=aspnetcore-2.2) .</span><span class="sxs-lookup"><span data-stu-id="e43a5-125">Refer to the [2.2 version of this document](xref:mvc/controllers/routing?view=aspnetcore-2.2) for reference material on the legacy routing system.</span></span>

<a name="cr"></a>

## <a name="set-up-conventional-route"></a><span data-ttu-id="e43a5-126">Настройка обычного маршрута</span><span class="sxs-lookup"><span data-stu-id="e43a5-126">Set up conventional route</span></span>

<span data-ttu-id="e43a5-127">При использовании [обычной маршрутизации](#crd)`Startup.Configure` обычно имеет код, аналогичный приведенному ниже.</span><span class="sxs-lookup"><span data-stu-id="e43a5-127">`Startup.Configure` typically has code similar to the following when using [conventional routing](#crd):</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet)]

<span data-ttu-id="e43a5-128">В вызове <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> используется для создания одного маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-128">Inside the call to <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> is used to create a single route.</span></span> <span data-ttu-id="e43a5-129">Один маршрут называется `default`ным маршрутом.</span><span class="sxs-lookup"><span data-stu-id="e43a5-129">The single route is named `default` route.</span></span> <span data-ttu-id="e43a5-130">Большинство приложений с контроллерами и представлениями используют шаблон маршрута, аналогичный маршруту `default`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-130">Most apps with controllers and views use a route template similar to the `default` route.</span></span> <span data-ttu-id="e43a5-131">Интерфейсы API-интерфейсов для интерфейса остальных должны использовать [маршрутизацию атрибутов](#ar).</span><span class="sxs-lookup"><span data-stu-id="e43a5-131">REST APIs should use [attribute routing](#ar).</span></span>

<span data-ttu-id="e43a5-132">`"{controller=Home}/{action=Index}/{id?}"`шаблона маршрута:</span><span class="sxs-lookup"><span data-stu-id="e43a5-132">The route template `"{controller=Home}/{action=Index}/{id?}"`:</span></span>

* <span data-ttu-id="e43a5-133">Соответствует URL-пути, например `/Products/Details/5`</span><span class="sxs-lookup"><span data-stu-id="e43a5-133">Matches a URL path like `/Products/Details/5`</span></span>
* <span data-ttu-id="e43a5-134">Извлекает значения маршрута `{ controller = Products, action = Details, id = 5 }` путем маркировки пути.</span><span class="sxs-lookup"><span data-stu-id="e43a5-134">Extracts the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="e43a5-135">Извлечение значений маршрута приводит к совпадению, если у приложения есть контроллер с именем `ProductsController` и действие `Details`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-135">The extraction of route values results in a match if the app has a controller named `ProductsController` and a `Details` action:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippetA)]

 <span data-ttu-id="e43a5-136">Метод [мидисплайраутеинфо](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) включен в [Пример загрузки](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) и используется для вывода сведений о маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-136">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>

  * <span data-ttu-id="e43a5-137">`/Products/Details/5` модель привязывает значение `id = 5`, чтобы задать для `5`параметра `id`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-137">`/Products/Details/5` model binds the value of `id = 5` to set the `id` parameter to `5`.</span></span> <span data-ttu-id="e43a5-138">Дополнительные сведения см. в разделе [Привязка модели](xref:mvc/models/model-binding) .</span><span class="sxs-lookup"><span data-stu-id="e43a5-138">See [Model Binding](xref:mvc/models/model-binding) for more details.</span></span>
* <span data-ttu-id="e43a5-139">`{controller=Home}` определяет `Home` как `controller`по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-139">`{controller=Home}` defines `Home` as the default `controller`.</span></span>
* <span data-ttu-id="e43a5-140">`{action=Index}` определяет `Index` как `action`по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-140">`{action=Index}` defines `Index` as the default `action`.</span></span>
*  <span data-ttu-id="e43a5-141">`?` символ в `{id?}` определяет `id` как необязательный.</span><span class="sxs-lookup"><span data-stu-id="e43a5-141">The `?` character in `{id?}` defines `id` as optional.</span></span>
  * <span data-ttu-id="e43a5-142">Параметры маршрута по умолчанию и необязательные параметры необязательно должны присутствовать в пути URL-адреса для сопоставления.</span><span class="sxs-lookup"><span data-stu-id="e43a5-142">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="e43a5-143">Подробное описание синтаксиса шаблона маршрута см. в разделе [Справочник по шаблону маршрута](xref:fundamentals/routing#route-template-reference).</span><span class="sxs-lookup"><span data-stu-id="e43a5-143">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>
* <span data-ttu-id="e43a5-144">Соответствует URL-пути `/`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-144">Matches the URL path `/`.</span></span>
* <span data-ttu-id="e43a5-145">Создает значения маршрута `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-145">Produces the route values `{ controller = Home, action = Index }`.</span></span>
* <span data-ttu-id="e43a5-146">Метод [мидисплайраутеинфо](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) включен в [Пример загрузки](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) и используется для вывода сведений о маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-146">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>

<span data-ttu-id="e43a5-147">Значения для `controller` и `action` используют значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-147">The values for `controller` and `action` make use of the default values.</span></span> <span data-ttu-id="e43a5-148">`id` не создает значение, так как в URL-пути нет соответствующего сегмента.</span><span class="sxs-lookup"><span data-stu-id="e43a5-148">`id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="e43a5-149">`/` соответствует, только если существует `HomeController` и `Index` действие:</span><span class="sxs-lookup"><span data-stu-id="e43a5-149">`/` only matches if there exists a `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="e43a5-150">Используя предыдущее определение контроллера и шаблон маршрута, `HomeController.Index` действие выполняется для следующих URL-путей:</span><span class="sxs-lookup"><span data-stu-id="e43a5-150">Using the preceding controller definition and route template, the `HomeController.Index` action is run for the following URL paths:</span></span>

* `/Home/Index/17`
* `/Home/Index`
* `/Home`
* `/`

<span data-ttu-id="e43a5-151">URL-путь `/` использует контроллеры `Home` и `Index` действие шаблона маршрута по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-151">The URL path `/` uses the route template default `Home` controllers and `Index` action.</span></span> <span data-ttu-id="e43a5-152">`/Home` URL-пути использует действие `Index` шаблона маршрута по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-152">The URL path `/Home` uses the route template default `Index` action.</span></span>

<span data-ttu-id="e43a5-153">Универсальный метод <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>:</span><span class="sxs-lookup"><span data-stu-id="e43a5-153">The convenience method <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>:</span></span>

```csharp
endpoints.MapDefaultControllerRoute();
```

<span data-ttu-id="e43a5-154">Меняющей</span><span class="sxs-lookup"><span data-stu-id="e43a5-154">Replaces:</span></span>

```csharp
endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="e43a5-155">Маршрутизация настраивается с использованием <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> и <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="e43a5-155">Routing is configured using the <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> middleware.</span></span> <span data-ttu-id="e43a5-156">Использование контроллеров:</span><span class="sxs-lookup"><span data-stu-id="e43a5-156">To use controllers:</span></span>

* <span data-ttu-id="e43a5-157">Вызовите <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> внутри `UseEndpoints`, чтобы сопоставлять [перенаправляемые](#ar) контроллеры.</span><span class="sxs-lookup"><span data-stu-id="e43a5-157">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> inside `UseEndpoints` to map [attribute routed](#ar) controllers.</span></span>
* <span data-ttu-id="e43a5-158">Вызовите <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> или <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>, чтобы [сослать направляемые по соглашениям](#cr) контроллеры.</span><span class="sxs-lookup"><span data-stu-id="e43a5-158">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> or <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>, to map [conventionally routed](#cr) controllers.</span></span>

<a name="routing-conventional-ref-label"></a>
<a name="crd"></a>

## <a name="conventional-routing"></a><span data-ttu-id="e43a5-159">Маршрутизация на основе соглашений</span><span class="sxs-lookup"><span data-stu-id="e43a5-159">Conventional routing</span></span>

<span data-ttu-id="e43a5-160">Стандартная маршрутизация используется с контроллерами и представлениями.</span><span class="sxs-lookup"><span data-stu-id="e43a5-160">Conventional routing is used with controllers and views.</span></span> <span data-ttu-id="e43a5-161">Маршрут `default`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-161">The `default` route:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet2)]

<span data-ttu-id="e43a5-162">является примером *маршрутизации на основе соглашений*.</span><span class="sxs-lookup"><span data-stu-id="e43a5-162">is an example of a *conventional routing*.</span></span> <span data-ttu-id="e43a5-163">Он называется *обычной маршрутизацией* , так как он устанавливает *соглашение* для URL-путей:</span><span class="sxs-lookup"><span data-stu-id="e43a5-163">It's called *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="e43a5-164">Первый сегмент пути, `{controller=Home}`, сопоставляется с именем контроллера.</span><span class="sxs-lookup"><span data-stu-id="e43a5-164">The first path segment, `{controller=Home}`, maps to the controller name.</span></span>
* <span data-ttu-id="e43a5-165">Второй сегмент, `{action=Index}`, сопоставляется с именем [действия](#action) .</span><span class="sxs-lookup"><span data-stu-id="e43a5-165">The second segment, `{action=Index}`, maps to the [action](#action) name.</span></span>
* <span data-ttu-id="e43a5-166">Третий сегмент, `{id?}` используется для необязательного `id`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-166">The third segment, `{id?}` is used for an optional `id`.</span></span> <span data-ttu-id="e43a5-167">`?` в `{id?}` делает его необязательным.</span><span class="sxs-lookup"><span data-stu-id="e43a5-167">The `?` in `{id?}` makes it optional.</span></span> <span data-ttu-id="e43a5-168">`id` используется для соотнесения с сущностью модели.</span><span class="sxs-lookup"><span data-stu-id="e43a5-168">`id` is used to map to a model entity.</span></span>

<span data-ttu-id="e43a5-169">Используя этот `default` маршрут, путь URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="e43a5-169">Using this `default` route, the URL path:</span></span>

* <span data-ttu-id="e43a5-170">`/Products/List` сопоставляется `ProductsController.List` действию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-170">`/Products/List` maps to the `ProductsController.List` action.</span></span>
* <span data-ttu-id="e43a5-171">`/Blog/Article/17` сопоставляется с `BlogController.Article` и модель, как правило, привязывает параметр `id` к 17.</span><span class="sxs-lookup"><span data-stu-id="e43a5-171">`/Blog/Article/17` maps to `BlogController.Article` and typically model binds the `id` parameter to 17.</span></span>

<span data-ttu-id="e43a5-172">Это сопоставление:</span><span class="sxs-lookup"><span data-stu-id="e43a5-172">This mapping:</span></span>

* <span data-ttu-id="e43a5-173">Основаны **только**на именах контроллера и [действий](#action) .</span><span class="sxs-lookup"><span data-stu-id="e43a5-173">Is based on the controller and [action](#action) names **only**.</span></span>
* <span data-ttu-id="e43a5-174">Не основано на пространствах имен, расположениях исходных файлов или параметров метода.</span><span class="sxs-lookup"><span data-stu-id="e43a5-174">Isn't based on namespaces, source file locations, or method parameters.</span></span>

<span data-ttu-id="e43a5-175">Использование обычной маршрутизации с маршрутом по умолчанию позволяет создавать приложения без необходимости создания нового шаблона URL-адреса для каждого действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-175">Using conventional routing with the default route allows creating the app without having to come up with a new URL pattern for each action.</span></span> <span data-ttu-id="e43a5-176">Для приложения с действиями в стиле [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) с согласованностью URL-адресов на контроллерах:</span><span class="sxs-lookup"><span data-stu-id="e43a5-176">For an app with [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) style actions, having consistency for the URLs across controllers:</span></span>

* <span data-ttu-id="e43a5-177">Помогает упростить код.</span><span class="sxs-lookup"><span data-stu-id="e43a5-177">Helps simplify the code.</span></span>
* <span data-ttu-id="e43a5-178">Делает пользовательский интерфейс более предсказуемым.</span><span class="sxs-lookup"><span data-stu-id="e43a5-178">Makes the UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="e43a5-179">`id` в приведенном выше коде определяется шаблоном маршрута как необязательный.</span><span class="sxs-lookup"><span data-stu-id="e43a5-179">The `id` in the preceding code is defined as optional by the route template.</span></span> <span data-ttu-id="e43a5-180">Действия могут выполняться без дополнительного идентификатора, предоставленного в качестве части URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-180">Actions can execute without the optional ID provided as part of the URL.</span></span> <span data-ttu-id="e43a5-181">Обычно при пропуске`id` из URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="e43a5-181">Generally, when`id` is omitted from the URL:</span></span>
>
> * <span data-ttu-id="e43a5-182">`id` устанавливается в `0` с помощью привязки модели.</span><span class="sxs-lookup"><span data-stu-id="e43a5-182">`id` is set to `0` by model binding.</span></span>
> * <span data-ttu-id="e43a5-183">Сущность не найдена в базе данных, соответствующей `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-183">No entity is found in the database matching `id == 0`.</span></span>
>
> <span data-ttu-id="e43a5-184">[Маршрутизация атрибутов](#ar) обеспечивает детальный контроль, чтобы сделать идентификатор необходимым для некоторых действий, а не для других.</span><span class="sxs-lookup"><span data-stu-id="e43a5-184">[Attribute routing](#ar) provides fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="e43a5-185">По соглашению документация включает необязательные параметры, такие как `id`, когда они, скорее всего, будут отображаться в правильном использовании.</span><span class="sxs-lookup"><span data-stu-id="e43a5-185">By convention, the documentation includes optional parameters like `id` when they're likely to appear in correct usage.</span></span>

<span data-ttu-id="e43a5-186">Для большинства приложений следует выбрать базовую описательную схему маршрутизации таким образом, чтобы URL-адреса были удобочитаемыми и осмысленными.</span><span class="sxs-lookup"><span data-stu-id="e43a5-186">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="e43a5-187">Традиционный маршрут по умолчанию `{controller=Home}/{action=Index}/{id?}`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-187">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="e43a5-188">Поддерживает основную и описательную схемы маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-188">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="e43a5-189">Является отправной точкой для приложений на базе пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-189">Is a useful starting point for UI-based apps.</span></span>
* <span data-ttu-id="e43a5-190">— Единственный шаблон маршрута, необходимый для многих приложений пользовательского веб-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-190">Is the only route template needed for many web UI apps.</span></span> <span data-ttu-id="e43a5-191">Для больших веб-приложений пользовательского интерфейса другой маршрут, использующий [области](#areas) , если это необходимо.</span><span class="sxs-lookup"><span data-stu-id="e43a5-191">For larger web UI apps, another route using [Areas](#areas) if frequently all that's needed.</span></span>

<span data-ttu-id="e43a5-192"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> и <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*>:</span><span class="sxs-lookup"><span data-stu-id="e43a5-192"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> :</span></span>

* <span data-ttu-id="e43a5-193">Автоматически назначить значение **заказа** своим конечным точкам в соответствии с порядком их вызова.</span><span class="sxs-lookup"><span data-stu-id="e43a5-193">Automatically assign an **order** value to their endpoints based on the order they are invoked.</span></span>

<span data-ttu-id="e43a5-194">Маршрутизация конечных точек в ASP.NET Core 3,0 и более поздних версий:</span><span class="sxs-lookup"><span data-stu-id="e43a5-194">Endpoint routing in ASP.NET Core 3.0 and later:</span></span>

* <span data-ttu-id="e43a5-195">Не имеет концепции маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-195">Doesn't have a concept of routes.</span></span>
* <span data-ttu-id="e43a5-196">Не предоставляет гарантий упорядочения для выполнения расширяемости, все конечные точки обрабатываются одновременно.</span><span class="sxs-lookup"><span data-stu-id="e43a5-196">Doesn't provide ordering guarantees for the execution of extensibility,  all endpoints are processed at once.</span></span>

<span data-ttu-id="e43a5-197">Чтобы увидеть, как встроенные реализации маршрутизации, такие как [, сопоставляются с запросами, включите ](xref:fundamentals/logging/index)ведение журнала<xref:Microsoft.AspNetCore.Routing.Route>.</span><span class="sxs-lookup"><span data-stu-id="e43a5-197">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

<span data-ttu-id="e43a5-198">[Маршрутизация атрибутов](#ar) объясняется далее в этом документе.</span><span class="sxs-lookup"><span data-stu-id="e43a5-198">[Attribute routing](#ar) is explained later in this document.</span></span>

<a name="mr"></a>

### <a name="multiple-conventional-routes"></a><span data-ttu-id="e43a5-199">Несколько обычных маршрутов</span><span class="sxs-lookup"><span data-stu-id="e43a5-199">Multiple conventional routes</span></span>

<span data-ttu-id="e43a5-200">В `UseEndpoints` можно добавить несколько [обычных маршрутов](#cr) , добавив дополнительные вызовы <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> и <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>.</span><span class="sxs-lookup"><span data-stu-id="e43a5-200">Multiple [conventional routes](#cr) can be added inside `UseEndpoints` by adding more calls to <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>.</span></span> <span data-ttu-id="e43a5-201">Это позволяет определить несколько соглашений или добавить традиционные маршруты, предназначенные для конкретного [действия](#action), например:</span><span class="sxs-lookup"><span data-stu-id="e43a5-201">Doing so allows defining multiple conventions, or to adding conventional routes that are dedicated to a specific [action](#action), such as:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<a name="dcr"></a>

<span data-ttu-id="e43a5-202">`blog` маршрут в приведенном выше коде является **выделенным обычным маршрутом**.</span><span class="sxs-lookup"><span data-stu-id="e43a5-202">The `blog` route in the preceding code is a **dedicated conventional route**.</span></span> <span data-ttu-id="e43a5-203">Он называется выделенным обычным маршрутом по следующим причинам.</span><span class="sxs-lookup"><span data-stu-id="e43a5-203">It's called a dedicated conventional route because:</span></span>

* <span data-ttu-id="e43a5-204">В нем используется [Обычная маршрутизация](#cr).</span><span class="sxs-lookup"><span data-stu-id="e43a5-204">It uses [conventional routing](#cr).</span></span>
* <span data-ttu-id="e43a5-205">Он предназначен для конкретного [действия](#action).</span><span class="sxs-lookup"><span data-stu-id="e43a5-205">It's dedicated to a specific [action](#action).</span></span>

<span data-ttu-id="e43a5-206">Так как `controller` и `action` не отображаются в шаблоне маршрута `"blog/{*article}"` в качестве параметров:</span><span class="sxs-lookup"><span data-stu-id="e43a5-206">Because `controller` and `action` don't appear in the route template `"blog/{*article}"` as parameters:</span></span>

* <span data-ttu-id="e43a5-207">Они могут иметь только значения по умолчанию `{ controller = "Blog", action = "Article" }`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-207">They can only have the default values `{ controller = "Blog", action = "Article" }`.</span></span>
* <span data-ttu-id="e43a5-208">Этот маршрут всегда сопоставляется с действием `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-208">This route always maps to the action `BlogController.Article`.</span></span>

<span data-ttu-id="e43a5-209">`/Blog`, `/Blog/Article`и `/Blog/{any-string}` являются единственными URL-путями, соответствующими маршруту блога.</span><span class="sxs-lookup"><span data-stu-id="e43a5-209">`/Blog`, `/Blog/Article`, and `/Blog/{any-string}` are the only URL paths that match the blog route.</span></span>

<span data-ttu-id="e43a5-210">В предыдущем примере:</span><span class="sxs-lookup"><span data-stu-id="e43a5-210">The preceding example:</span></span>

* <span data-ttu-id="e43a5-211">`blog` маршрут имеет более высокий приоритет для совпадений, чем маршрут `default`, так как он добавляется первым.</span><span class="sxs-lookup"><span data-stu-id="e43a5-211">`blog` route has a higher priority for matches than the `default` route because it is added first.</span></span>
* <span data-ttu-id="e43a5-212">— И пример маршрутизации [по стилю](https://developer.mozilla.org/docs/Glossary/Slug) , в которой в качестве части URL-адреса обычно используется имя статьи.</span><span class="sxs-lookup"><span data-stu-id="e43a5-212">Is and example of [Slug](https://developer.mozilla.org/docs/Glossary/Slug) style routing where it's typical to have an article name as part of the URL.</span></span>

> [!WARNING]
> <span data-ttu-id="e43a5-213">В ASP.NET Core 3,0 и более поздних версиях маршрутизация не выполняет:</span><span class="sxs-lookup"><span data-stu-id="e43a5-213">In ASP.NET Core 3.0 and later, routing doesn't:</span></span>
> * <span data-ttu-id="e43a5-214">Определите концепцию, называемую *маршрутом*.</span><span class="sxs-lookup"><span data-stu-id="e43a5-214">Define a concept called a *route*.</span></span> <span data-ttu-id="e43a5-215">`UseRouting` добавляет сопоставление маршрутов в конвейер по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="e43a5-215">`UseRouting` adds route matching to the middleware pipeline.</span></span> <span data-ttu-id="e43a5-216">По промежуточного слоя `UseRouting` просматривает набор конечных точек, определенных в приложении, и выбирает наилучшее совпадение конечных точек на основе запроса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-216">The `UseRouting` middleware looks at the set of endpoints defined in the app, and selects the best endpoint match based on the request.</span></span>
> * <span data-ttu-id="e43a5-217">Предоставьте гарантии порядка выполнения расширяемости, например <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> или <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>.</span><span class="sxs-lookup"><span data-stu-id="e43a5-217">Provide guarantees about the execution order of extensibility like <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> or <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>.</span></span>
>
><span data-ttu-id="e43a5-218">См. раздел о [маршрутизации](xref:fundamentals/routing) для справочных материалов по маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-218">See [Routing](xref:fundamentals/routing) for reference material on routing.</span></span>

<a name="cro"></a>

### <a name="conventional-routing-order"></a><span data-ttu-id="e43a5-219">Стандартный порядок маршрутизации</span><span class="sxs-lookup"><span data-stu-id="e43a5-219">Conventional routing order</span></span>

<span data-ttu-id="e43a5-220">Обычная маршрутизация соответствует только комбинации действий и контроллера, определенных приложением.</span><span class="sxs-lookup"><span data-stu-id="e43a5-220">Conventional routing only matches a combination of action and controller that are defined by the app.</span></span> <span data-ttu-id="e43a5-221">Это предназначено для упрощения случаев, когда обычные маршруты перекрываются.</span><span class="sxs-lookup"><span data-stu-id="e43a5-221">This is intended to simplify cases where conventional routes overlap.</span></span>
<span data-ttu-id="e43a5-222">Добавление маршрутов с помощью <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>и <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> автоматически присваивает значение порядка их конечным точкам в соответствии с порядком их вызова.</span><span class="sxs-lookup"><span data-stu-id="e43a5-222">Adding routes using <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>, and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> automatically assign an order value to their endpoints based on the order they are invoked.</span></span> <span data-ttu-id="e43a5-223">Совпадает с более высоким приоритетом на маршруте, который отображается ранее.</span><span class="sxs-lookup"><span data-stu-id="e43a5-223">Matches from a route that appears earlier have a higher priority.</span></span> <span data-ttu-id="e43a5-224">При маршрутизации на основе соглашений учитывается порядок.</span><span class="sxs-lookup"><span data-stu-id="e43a5-224">Conventional routing is order-dependent.</span></span> <span data-ttu-id="e43a5-225">Как правило, маршруты с областями следует размещать раньше, так как они более специфичны, чем маршруты без области.</span><span class="sxs-lookup"><span data-stu-id="e43a5-225">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span> <span data-ttu-id="e43a5-226">[Выделенные обычные маршруты](#dcr) с перехватом всех параметров маршрута, таких как `{*article}`, могут сделать маршрут слишком [жадным](xref:fundamentals/routing#greedy), то есть он будет соответствовать URL-адресам, которые должны сопоставляться другими маршрутами.</span><span class="sxs-lookup"><span data-stu-id="e43a5-226">[Dedicated conventional routes](#dcr) with catch all route parameters like `{*article}` can make a route too [greedy](xref:fundamentals/routing#greedy), meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="e43a5-227">Помещайте жадные маршруты позже в таблице маршрутов, чтобы предотвратить жадные соответствия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-227">Put the greedy routes later in the route table to prevent greedy matches.</span></span>

<a name="best"></a>

### <a name="resolving-ambiguous-actions"></a><span data-ttu-id="e43a5-228">Разрешение неоднозначных действий</span><span class="sxs-lookup"><span data-stu-id="e43a5-228">Resolving ambiguous actions</span></span>

<span data-ttu-id="e43a5-229">При совпадении двух конечных точек через маршрутизацию маршрутизация должна выполнить одно из следующих действий:</span><span class="sxs-lookup"><span data-stu-id="e43a5-229">When two endpoints match through routing, routing must do one of the following:</span></span>

* <span data-ttu-id="e43a5-230">Выберите лучший кандидат.</span><span class="sxs-lookup"><span data-stu-id="e43a5-230">Choose the best candidate.</span></span>
* <span data-ttu-id="e43a5-231">Создание исключения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-231">Throw an exception.</span></span>

<span data-ttu-id="e43a5-232">Пример:</span><span class="sxs-lookup"><span data-stu-id="e43a5-232">For example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet9)]

<span data-ttu-id="e43a5-233">Предыдущий контроллер определяет два соответствующих действия:</span><span class="sxs-lookup"><span data-stu-id="e43a5-233">The preceding controller defines two actions that match:</span></span>

* <span data-ttu-id="e43a5-234">URL-путь `/Products33/Edit/17`</span><span class="sxs-lookup"><span data-stu-id="e43a5-234">The URL path `/Products33/Edit/17`</span></span>
* <span data-ttu-id="e43a5-235">`{ controller = Products33, action = Edit, id = 17 }`данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-235">Route data `{ controller = Products33, action = Edit, id = 17 }`.</span></span>

<span data-ttu-id="e43a5-236">Это типичный шаблон для контроллеров MVC:</span><span class="sxs-lookup"><span data-stu-id="e43a5-236">This is a typical pattern for MVC controllers:</span></span>

* <span data-ttu-id="e43a5-237">`Edit(int)` отображает форму для изменения продукта.</span><span class="sxs-lookup"><span data-stu-id="e43a5-237">`Edit(int)` displays a form to edit a product.</span></span>
* <span data-ttu-id="e43a5-238">`Edit(int, Product)` обрабатывает опубликованную форму.</span><span class="sxs-lookup"><span data-stu-id="e43a5-238">`Edit(int, Product)` processes  the posted form.</span></span>

<span data-ttu-id="e43a5-239">Чтобы разрешить правильный маршрут, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-239">To resolve the correct route:</span></span>

* <span data-ttu-id="e43a5-240">`Edit(int, Product)` выбирается, когда запрос является HTTP-`POST`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-240">`Edit(int, Product)` is selected when the request is an HTTP `POST`.</span></span>
* <span data-ttu-id="e43a5-241">`Edit(int)` выбирается, когда [команда HTTP](#verb) является любым другим.</span><span class="sxs-lookup"><span data-stu-id="e43a5-241">`Edit(int)` is selected when the [HTTP verb](#verb) is anything else.</span></span> <span data-ttu-id="e43a5-242">`Edit(int)` обычно вызывается с помощью `GET`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-242">`Edit(int)` is generally called via `GET`.</span></span>

<span data-ttu-id="e43a5-243"><xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]`, предоставляется для маршрутизации, чтобы ее можно было выбрать в зависимости от метода HTTP запроса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-243">The <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]`, is provided to routing so that it can choose based on the HTTP method of the request.</span></span> <span data-ttu-id="e43a5-244">`HttpPostAttribute` делает `Edit(int, Product)` более подходящие, чем `Edit(int)`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-244">The `HttpPostAttribute` makes `Edit(int, Product)` a better match than `Edit(int)`.</span></span>

<span data-ttu-id="e43a5-245">Важно понимать роль таких атрибутов, как `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-245">It's important to understand the role of attributes like `HttpPostAttribute`.</span></span> <span data-ttu-id="e43a5-246">Аналогичные атрибуты определяются для других [HTTP-команд](#verb).</span><span class="sxs-lookup"><span data-stu-id="e43a5-246">Similar attributes are defined for other [HTTP verbs](#verb).</span></span> <span data-ttu-id="e43a5-247">В [обычной маршрутизации](#cr)для действий используется одно и то же имя действия, когда они входят в форму отображения, Рабочий процесс отправки формы.</span><span class="sxs-lookup"><span data-stu-id="e43a5-247">In [conventional routing](#cr), it's common for actions to use the same action name when they're part of a show form, submit form workflow.</span></span> <span data-ttu-id="e43a5-248">Например, см. раздел [изучение двух методов действия Edit](xref:tutorials/first-mvc-app/controller-methods-views#get-post).</span><span class="sxs-lookup"><span data-stu-id="e43a5-248">For example, see [Examine the two Edit action methods](xref:tutorials/first-mvc-app/controller-methods-views#get-post).</span></span>

<span data-ttu-id="e43a5-249">Если службе маршрутизации не удается выбрать лучший вариант, выдается <xref:System.Reflection.AmbiguousMatchException>, где перечисляются несколько совпадающих конечных точек.</span><span class="sxs-lookup"><span data-stu-id="e43a5-249">If routing can't choose a best candidate, an <xref:System.Reflection.AmbiguousMatchException> is thrown, listing the multiple matched endpoints.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="conventional-route-names"></a><span data-ttu-id="e43a5-250">Имена обычных маршрутов</span><span class="sxs-lookup"><span data-stu-id="e43a5-250">Conventional route names</span></span>

<span data-ttu-id="e43a5-251">Строки `"blog"` и `"default"` в следующих примерах являются стандартными именами маршрутов:</span><span class="sxs-lookup"><span data-stu-id="e43a5-251">The strings  `"blog"` and `"default"` in the following examples are conventional route names:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="e43a5-252">Имена маршрутов придают маршруту логическое имя.</span><span class="sxs-lookup"><span data-stu-id="e43a5-252">The route names give the route a logical name.</span></span> <span data-ttu-id="e43a5-253">Именованный маршрут можно использовать для создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-253">The named route can be used for URL generation.</span></span> <span data-ttu-id="e43a5-254">Использование именованного маршрута упрощает создание URL-адресов, когда порядок маршрутов может усложнить создание URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-254">Using a named route simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="e43a5-255">Имена маршрутов должны быть уникальными для всего приложения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-255">Route names must be unique application wide.</span></span>

<span data-ttu-id="e43a5-256">Имена маршрутов:</span><span class="sxs-lookup"><span data-stu-id="e43a5-256">Route names:</span></span>

* <span data-ttu-id="e43a5-257">Не влияют на сопоставление URL-адресов или обработку запросов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-257">Have no impact on URL matching or handling of requests.</span></span>
* <span data-ttu-id="e43a5-258">Используются только для создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-258">Are used only for URL generation.</span></span>

<span data-ttu-id="e43a5-259">Концепция имени маршрута представлена в маршрутизации как [иендпоинтнамеметадата](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata).</span><span class="sxs-lookup"><span data-stu-id="e43a5-259">The route name concept is represented in routing as [IEndpointNameMetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata).</span></span> <span data-ttu-id="e43a5-260">Термины **имя маршрута** и **имя конечной точки**:</span><span class="sxs-lookup"><span data-stu-id="e43a5-260">The terms **route name** and **endpoint name**:</span></span>

* <span data-ttu-id="e43a5-261">Являются взаимозаменяемыми.</span><span class="sxs-lookup"><span data-stu-id="e43a5-261">Are interchangeable.</span></span>
* <span data-ttu-id="e43a5-262">То, какой из них используется в документации и коде, зависит от описываемого API.</span><span class="sxs-lookup"><span data-stu-id="e43a5-262">Which one is used in documentation and code depends on the API being described.</span></span>

<a name="attribute-routing-ref-label"></a>
<a name="ar"></a>

## <a name="attribute-routing-for-rest-apis"></a><span data-ttu-id="e43a5-263">Маршрутизация атрибутов для интерфейсов API RESTFUL</span><span class="sxs-lookup"><span data-stu-id="e43a5-263">Attribute routing for REST APIs</span></span>

<span data-ttu-id="e43a5-264">Для моделирования функциональных возможностей приложения в качестве набора ресурсов, в которых операции представлены [HTTP-командами](#verb), интерфейсы API-интерфейса должны использовать маршрутизацию атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-264">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by [HTTP verbs](#verb).</span></span>

<span data-ttu-id="e43a5-265">При маршрутизации с помощью атрибутов используется набор атрибутов для сопоставления действий непосредственно с шаблонами маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-265">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="e43a5-266">Следующий `StartUp.Configure` код типичен для REST API и используется в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e43a5-266">The following `StartUp.Configure` code is typical for a REST API and is used in the next sample:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupApi.cs?name=snippet)]

<span data-ttu-id="e43a5-267">В приведенном выше коде <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> вызывается в `UseEndpoints` для подключения к перенаправленным контроллерам атрибута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-267">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> is called inside `UseEndpoints` to map attribute routed controllers.</span></span>

<span data-ttu-id="e43a5-268">Рассмотрим следующий пример:</span><span class="sxs-lookup"><span data-stu-id="e43a5-268">In the following example:</span></span>

* <span data-ttu-id="e43a5-269">Используется предыдущий метод `Configure`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-269">The preceding `Configure` method is used.</span></span>
* <span data-ttu-id="e43a5-270">`HomeController` соответствует набору URL-адресов, аналогично тому, который `{controller=Home}/{action=Index}/{id?}` совпадает с обычным маршрутом по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-270">`HomeController` matches a set of URLs similar to what the default conventional route `{controller=Home}/{action=Index}/{id?}` matches.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

<span data-ttu-id="e43a5-271">`HomeController.Index` действие выполняется для любого из URL-путей `/`, `/Home`, `/Home/Index`или `/Home/Index/3`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-271">The `HomeController.Index` action is run for any of the URL paths `/`, `/Home`, `/Home/Index`, or `/Home/Index/3`.</span></span>

<span data-ttu-id="e43a5-272">В этом примере демонстрируется ключевое различие в программировании между маршрутизацией атрибутов и [обычной маршрутизацией](#cr).</span><span class="sxs-lookup"><span data-stu-id="e43a5-272">This example highlights a key programming difference between attribute routing and [conventional routing](#cr).</span></span> <span data-ttu-id="e43a5-273">Маршрутизация атрибутов требует больше входных данных для указания маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-273">Attribute routing requires more input to specify a route.</span></span> <span data-ttu-id="e43a5-274">Стандартный маршрут по умолчанию обрабатывает маршруты более кратко.</span><span class="sxs-lookup"><span data-stu-id="e43a5-274">The conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="e43a5-275">Однако маршрутизация атрибутов разрешает и требует точного контроля над тем, какие шаблоны маршрутов применяются к каждому [действию](#action).</span><span class="sxs-lookup"><span data-stu-id="e43a5-275">However, attribute routing allows and requires precise control of which route templates apply to each [action](#action).</span></span>

<span data-ttu-id="e43a5-276">При использовании маршрутизации атрибутов имя контроллера и имена действий **не** воспроизводят роль, в которой сопоставляется действие.</span><span class="sxs-lookup"><span data-stu-id="e43a5-276">With attribute routing, the controller name and action names play **no** role in which action is matched.</span></span> <span data-ttu-id="e43a5-277">В следующем примере совпадают те же URL-адреса, что и в предыдущем примере:</span><span class="sxs-lookup"><span data-stu-id="e43a5-277">The following example matches the same URLs as the previous example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="e43a5-278">В следующем коде используется замена маркера для `action` и `controller`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-278">The following code uses token replacement for `action` and `controller`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet22)]

<span data-ttu-id="e43a5-279">Следующий код применяется `[Route("[controller]/[action]")]` к контроллеру:</span><span class="sxs-lookup"><span data-stu-id="e43a5-279">The following code applies `[Route("[controller]/[action]")]` to the controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

<span data-ttu-id="e43a5-280">В приведенном выше коде шаблоны методов `Index` должны в начале `/` или `~/` шаблоны маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-280">In the preceding code, the `Index` method templates must prepend `/` or `~/` to the route templates.</span></span> <span data-ttu-id="e43a5-281">Шаблоны маршрутов, применяемые к действию, которое начинается с символа `/` или `~/`, не объединяются с шаблонами маршрутов, применяемыми к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="e43a5-281">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span>

<span data-ttu-id="e43a5-282">Сведения о выборе шаблона маршрутов см. в разделе [приоритет шаблона маршрута](xref:fundamentals/routing#rtp) .</span><span class="sxs-lookup"><span data-stu-id="e43a5-282">See [Route template precedence](xref:fundamentals/routing#rtp) for information on route template selection.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="e43a5-283">Зарезервированные имена маршрутизации</span><span class="sxs-lookup"><span data-stu-id="e43a5-283">Reserved routing names</span></span>

<span data-ttu-id="e43a5-284">При использовании контроллеров или Razor Pages следующие ключевые слова являются зарезервированными именами параметров маршрута:</span><span class="sxs-lookup"><span data-stu-id="e43a5-284">The following keywords are reserved route parameter names when using Controllers or Razor Pages:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

<span data-ttu-id="e43a5-285">Использование `page` в качестве параметра маршрута с маршрутизацией атрибутов является распространенной ошибкой.</span><span class="sxs-lookup"><span data-stu-id="e43a5-285">Using `page` as a route parameter with attribute routing is a common error.</span></span> <span data-ttu-id="e43a5-286">Это приводит к несогласованности и путанице при формировании URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-286">Doing that results in inconsistent and confusing behavior with URL generation.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo2Controller.cs?name=snippet)]

<span data-ttu-id="e43a5-287">Специальные имена параметров используются при формировании URL-адресов для определения того, относится ли операция формирования URL-адреса к странице Razor или к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="e43a5-287">The special parameter names are used by the URL generation to determine if a URL generation operation refers to a Razor Page or to a Controller.</span></span>

<a name="verb"></a>

## <a name="http-verb-templates"></a><span data-ttu-id="e43a5-288">Шаблоны HTTP-команд</span><span class="sxs-lookup"><span data-stu-id="e43a5-288">HTTP verb templates</span></span>

<span data-ttu-id="e43a5-289">ASP.NET Core содержит следующие шаблоны HTTP-команд:</span><span class="sxs-lookup"><span data-stu-id="e43a5-289">ASP.NET Core has the following HTTP verb templates:</span></span>

* <span data-ttu-id="e43a5-290">[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span><span class="sxs-lookup"><span data-stu-id="e43a5-290">[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span></span>
* <span data-ttu-id="e43a5-291">[HttpPost](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span><span class="sxs-lookup"><span data-stu-id="e43a5-291">[[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span></span>
* <span data-ttu-id="e43a5-292">[[Хттппут]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span><span class="sxs-lookup"><span data-stu-id="e43a5-292">[[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span></span>
* <span data-ttu-id="e43a5-293">[[Хттпделете]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span><span class="sxs-lookup"><span data-stu-id="e43a5-293">[[HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span></span>
* <span data-ttu-id="e43a5-294">[[Хттфеад]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span><span class="sxs-lookup"><span data-stu-id="e43a5-294">[[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span></span>
* <span data-ttu-id="e43a5-295">[[Хттппатч]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span><span class="sxs-lookup"><span data-stu-id="e43a5-295">[[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span></span>

<a name="rt"></a>

### <a name="route-templates"></a><span data-ttu-id="e43a5-296">Шаблоны маршрутов</span><span class="sxs-lookup"><span data-stu-id="e43a5-296">Route templates</span></span>

<span data-ttu-id="e43a5-297">ASP.NET Core имеет следующие шаблоны маршрутов:</span><span class="sxs-lookup"><span data-stu-id="e43a5-297">ASP.NET Core has the following route templates:</span></span>

* <span data-ttu-id="e43a5-298">Все [шаблоны HTTP-команд](#verb) являются шаблонами маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-298">All the [HTTP verb templates](#verb) are route templates.</span></span>
* <span data-ttu-id="e43a5-299">[[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="e43a5-299">[[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span></span>

<a name="arx"></a>

### <a name="attribute-routing-with-http-verb-attributes"></a><span data-ttu-id="e43a5-300">Маршрутизация атрибутов с помощью атрибутов глагола HTTP</span><span class="sxs-lookup"><span data-stu-id="e43a5-300">Attribute routing with Http verb attributes</span></span>

<span data-ttu-id="e43a5-301">Рассмотрим следующий контроллер:</span><span class="sxs-lookup"><span data-stu-id="e43a5-301">Consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="e43a5-302">В приведенном выше коде:</span><span class="sxs-lookup"><span data-stu-id="e43a5-302">In the preceding code:</span></span>

* <span data-ttu-id="e43a5-303">Каждое действие содержит атрибут `[HttpGet]`, который ограничивает сопоставление только запросами HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="e43a5-303">Each action contains the `[HttpGet]` attribute, which constrains matching to HTTP GET requests only.</span></span>
* <span data-ttu-id="e43a5-304">`GetProduct` действие включает шаблон `"{id}"`, поэтому `id` добавляется к шаблону `"api/[controller]"` на контроллере.</span><span class="sxs-lookup"><span data-stu-id="e43a5-304">The `GetProduct` action includes the `"{id}"` template, therefore `id` is appended to the `"api/[controller]"` template on the controller.</span></span> <span data-ttu-id="e43a5-305">Шаблон методов `"api/[controller]/"{id}""`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-305">The methods template is `"api/[controller]/"{id}""`.</span></span> <span data-ttu-id="e43a5-306">Поэтому это действие соответствует только запросам GET для формы `/api/test2/xyz`,`/api/test2/123`,`/api/test2/{any string}`и т. д.</span><span class="sxs-lookup"><span data-stu-id="e43a5-306">Therefore this action only matches GET requests of for the form `/api/test2/xyz`,`/api/test2/123`,`/api/test2/{any string}`, etc.</span></span>
  [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet2)]
* <span data-ttu-id="e43a5-307">Действие `GetIntProduct` содержит шаблон `"int/{id:int}")`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-307">The `GetIntProduct` action contains the `"int/{id:int}")` template.</span></span> <span data-ttu-id="e43a5-308">`:int` часть шаблона ограничивает `id` значения маршрута строками, которые можно преобразовать в целое число.</span><span class="sxs-lookup"><span data-stu-id="e43a5-308">The `:int` portion of the template constrains the `id` route values to strings that can be converted to an integer.</span></span> <span data-ttu-id="e43a5-309">Запрос GET к `/api/test2/int/abc`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-309">A GET request to `/api/test2/int/abc`:</span></span>
  * <span data-ttu-id="e43a5-310">Не соответствует этому действию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-310">Doesn't match this action.</span></span>
  * <span data-ttu-id="e43a5-311">Возвращает ошибку [404 не найден](https://developer.mozilla.org/docs/Web/HTTP/Status/404) .</span><span class="sxs-lookup"><span data-stu-id="e43a5-311">Returns a [404 Not Found](https://developer.mozilla.org/docs/Web/HTTP/Status/404) error.</span></span>
    [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet3)]
* <span data-ttu-id="e43a5-312">Действие `GetInt2Product` содержит `{id}` в шаблоне, но не ограничивает `id` значениями, которые можно преобразовать в целое число.</span><span class="sxs-lookup"><span data-stu-id="e43a5-312">The `GetInt2Product` action contains `{id}` in the template, but doesn't constrain `id` to values that can be converted to an integer.</span></span> <span data-ttu-id="e43a5-313">Запрос GET к `/api/test2/int2/abc`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-313">A GET request to `/api/test2/int2/abc`:</span></span>
  * <span data-ttu-id="e43a5-314">Соответствует этому маршруту.</span><span class="sxs-lookup"><span data-stu-id="e43a5-314">Matches this route.</span></span>
  * <span data-ttu-id="e43a5-315">Привязке модели не удается преобразовать `abc` в целое число.</span><span class="sxs-lookup"><span data-stu-id="e43a5-315">Model binding fails to convert `abc` to an integer.</span></span> <span data-ttu-id="e43a5-316">Параметр `id` метода имеет тип Integer.</span><span class="sxs-lookup"><span data-stu-id="e43a5-316">The `id` parameter of the method is integer.</span></span>
  * <span data-ttu-id="e43a5-317">Возвращает [400 Неверный запрос](https://developer.mozilla.org/docs/Web/HTTP/Status/400) , так как привязка модели не смогла преобразовать`abc` в целое число.</span><span class="sxs-lookup"><span data-stu-id="e43a5-317">Returns a [400 Bad Request](https://developer.mozilla.org/docs/Web/HTTP/Status/400) because model binding failed to convert`abc` to an integer.</span></span>
      [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet4)]

<span data-ttu-id="e43a5-318">Маршрутизация атрибутов может использовать <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute> атрибуты, такие как <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>и <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>.</span><span class="sxs-lookup"><span data-stu-id="e43a5-318">Attribute routing can use <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute> attributes such as <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>, and <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>.</span></span> <span data-ttu-id="e43a5-319">Все атрибуты [HTTP-команды](#verb) принимают шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-319">All of the [HTTP verb](#verb) attributes accept a route template.</span></span> <span data-ttu-id="e43a5-320">В следующем примере показаны два действия, которые соответствуют одному шаблону маршрута:</span><span class="sxs-lookup"><span data-stu-id="e43a5-320">The following example shows two actions that match the same route template:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyProductsController.cs?name=snippet1)]

<span data-ttu-id="e43a5-321">Использование URL-пути `/products3`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-321">Using the URL path `/products3`:</span></span>

* <span data-ttu-id="e43a5-322">Действие `MyProductsController.ListProducts` выполняется, когда [HTTP-команда](#verb) `GET`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-322">The `MyProductsController.ListProducts` action runs when the [HTTP verb](#verb) is `GET`.</span></span>
* <span data-ttu-id="e43a5-323">Действие `MyProductsController.CreateProduct` выполняется, когда [HTTP-команда](#verb) `POST`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-323">The `MyProductsController.CreateProduct` action runs when the [HTTP verb](#verb) is `POST`.</span></span>

<span data-ttu-id="e43a5-324">При создании REST API в редких случаях необходимо использовать `[Route(...)]` в методе действия, поскольку действие принимает все методы HTTP.</span><span class="sxs-lookup"><span data-stu-id="e43a5-324">When building a REST API, it's rare that you'll need to use `[Route(...)]` on an action method because the action accepts all HTTP methods.</span></span> <span data-ttu-id="e43a5-325">Лучше использовать более конкретный [атрибут HTTP-команды](#verb) , чтобы точно определить, что поддерживает API.</span><span class="sxs-lookup"><span data-stu-id="e43a5-325">It's better to use the more specific [HTTP verb attribute](#verb) to be precise about what your API supports.</span></span> <span data-ttu-id="e43a5-326">Клиенты интерфейсов REST API должны знать, какие пути и HTTP-команды сопоставляются с определенными логическими операциями.</span><span class="sxs-lookup"><span data-stu-id="e43a5-326">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="e43a5-327">Для моделирования функциональных возможностей приложения в качестве набора ресурсов, в которых операции представлены HTTP-командами, интерфейсы API-интерфейса должны использовать маршрутизацию атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-327">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="e43a5-328">Это означает, что многие операции, например GET и POST для одного и того же логического ресурса, используют один и тот же URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="e43a5-328">This means that many operations, for example, GET and POST on the same logical resource use the same URL.</span></span> <span data-ttu-id="e43a5-329">Маршрутизация с помощью атрибутов обеспечивает необходимый уровень контроля, позволяющий тщательно разработать схему общедоступных конечных точек API-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-329">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="e43a5-330">Так как маршрут на основе атрибутов применяется к определенному действию, можно легко сделать параметры обязательными в рамках определения шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-330">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="e43a5-331">В следующем примере `id` требуется в качестве части URL-пути:</span><span class="sxs-lookup"><span data-stu-id="e43a5-331">In the following example, `id` is required as part of the URL path:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="e43a5-332">Действие `Products2ApiController.GetProduct(int)`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-332">The `Products2ApiController.GetProduct(int)` action:</span></span>

* <span data-ttu-id="e43a5-333">Выполняется с URL-путем, например `/products2/3`</span><span class="sxs-lookup"><span data-stu-id="e43a5-333">Is run with URL path like `/products2/3`</span></span>
* <span data-ttu-id="e43a5-334">Не выполняется с URL-адресом `/products2`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-334">Isn't run with the URL path `/products2`.</span></span>

<span data-ttu-id="e43a5-335">Атрибут [[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) позволяет выполнять действие для ограничения поддерживаемых типов содержимого запросов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-335">The [[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) attribute allows an action to limit the supported request content types.</span></span> <span data-ttu-id="e43a5-336">Дополнительные сведения см. [в разделе Определение поддерживаемых типов содержимого запросов с помощью атрибута использования](xref:web-api/index#consumes).</span><span class="sxs-lookup"><span data-stu-id="e43a5-336">For more information, see [Define supported request content types with the Consumes attribute](xref:web-api/index#consumes).</span></span>

 <span data-ttu-id="e43a5-337">Полное описание шаблонов маршрутов и связанных параметров см. в статье [Маршрутизация](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="e43a5-337">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

<span data-ttu-id="e43a5-338">Дополнительные сведения о `[ApiController]`см. в разделе [атрибут ApiController](xref:web-api/index##apicontroller-attribute).</span><span class="sxs-lookup"><span data-stu-id="e43a5-338">For more information on `[ApiController]`, see [ApiController attribute](xref:web-api/index##apicontroller-attribute).</span></span>

## <a name="route-name"></a><span data-ttu-id="e43a5-339">Имя маршрута</span><span class="sxs-lookup"><span data-stu-id="e43a5-339">Route name</span></span>

<span data-ttu-id="e43a5-340">Следующий код определяет имя маршрута `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-340">The following code  defines a route name of `Products_List`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="e43a5-341">Имена маршрутов могут использоваться для формирования URL-адреса на основе определенного маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-341">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="e43a5-342">Имена маршрутов:</span><span class="sxs-lookup"><span data-stu-id="e43a5-342">Route names:</span></span>

* <span data-ttu-id="e43a5-343">Не влияют на поведение маршрутизации в соответствии с URL-адресом.</span><span class="sxs-lookup"><span data-stu-id="e43a5-343">Have no impact on the URL matching behavior of routing.</span></span>
* <span data-ttu-id="e43a5-344">Используются только для создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-344">Are only used for URL generation.</span></span>

<span data-ttu-id="e43a5-345">Имена маршрутов должны быть уникальными в пределах приложения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-345">Route names must be unique application-wide.</span></span>

<span data-ttu-id="e43a5-346">Сравните предыдущий код с обычным маршрутом по умолчанию, который определяет `id` параметр как необязательный (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="e43a5-346">Contrast the preceding code with the conventional default route, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="e43a5-347">Возможность точного указания интерфейсов API имеет свои преимущества, такие как разрешение `/products` и `/products/5` отправляются в различные действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-347">The ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

## <a name="combining-attribute-routes"></a><span data-ttu-id="e43a5-348">Объединение маршрутов атрибутов</span><span class="sxs-lookup"><span data-stu-id="e43a5-348">Combining attribute routes</span></span>

<span data-ttu-id="e43a5-349">Чтобы избежать лишних повторов при маршрутизации с помощью атрибутов, атрибуты маршрута для контроллера объединяются с атрибутами маршрута для отдельных действий.</span><span class="sxs-lookup"><span data-stu-id="e43a5-349">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="e43a5-350">Все шаблоны маршрутов, определенные в контроллере, добавляются перед шаблонами маршрутов для действий.</span><span class="sxs-lookup"><span data-stu-id="e43a5-350">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="e43a5-351">В результате добавления атрибута маршрута для контроллера **все** его действия будут использовать маршрутизацию с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-351">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet)]

<span data-ttu-id="e43a5-352">В предшествующем примере:</span><span class="sxs-lookup"><span data-stu-id="e43a5-352">In the preceding example:</span></span>

* <span data-ttu-id="e43a5-353">URL-путь `/products` может соответствовать `ProductsApi.ListProducts`</span><span class="sxs-lookup"><span data-stu-id="e43a5-353">The URL path `/products` can match `ProductsApi.ListProducts`</span></span>
* <span data-ttu-id="e43a5-354">URL-путь `/products/5` может соответствовать `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-354">The URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span>

<span data-ttu-id="e43a5-355">Оба эти действия соответствуют только HTTP-`GET`, так как они помечены атрибутом `[HttpGet]`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-355">Both of these actions only match HTTP `GET` because they're marked with the `[HttpGet]` attribute.</span></span>

<span data-ttu-id="e43a5-356">Шаблоны маршрутов, применяемые к действию, которое начинается с символа `/` или `~/`, не объединяются с шаблонами маршрутов, применяемыми к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="e43a5-356">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="e43a5-357">Следующий пример соответствует набору URL-путей, аналогичному маршруту по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-357">The following example matches a set of URL paths similar to the default route.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet)]

<span data-ttu-id="e43a5-358">В следующей таблице описаны атрибуты `[Route]` в приведенном выше коде.</span><span class="sxs-lookup"><span data-stu-id="e43a5-358">The following table explains the `[Route]` attributes in the preceding code:</span></span>

| <span data-ttu-id="e43a5-359">attribute</span><span class="sxs-lookup"><span data-stu-id="e43a5-359">Attribute</span></span>               | <span data-ttu-id="e43a5-360">Объединяет с `[Route("Home")]`</span><span class="sxs-lookup"><span data-stu-id="e43a5-360">Combines with `[Route("Home")]`</span></span> | <span data-ttu-id="e43a5-361">Определение шаблона маршрута</span><span class="sxs-lookup"><span data-stu-id="e43a5-361">Defines route template</span></span> |
| ----------------- | ------------ | --------- |
| `[Route("")]` | <span data-ttu-id="e43a5-362">Да</span><span class="sxs-lookup"><span data-stu-id="e43a5-362">Yes</span></span> | `"Home"` |
| `[Route("Index")]` | <span data-ttu-id="e43a5-363">Да</span><span class="sxs-lookup"><span data-stu-id="e43a5-363">Yes</span></span> | `"Home/Index"` |
| `[Route("/")]` | <span data-ttu-id="e43a5-364">**Нет**</span><span class="sxs-lookup"><span data-stu-id="e43a5-364">**No**</span></span> | `""` |
| `[Route("About")]` | <span data-ttu-id="e43a5-365">Да</span><span class="sxs-lookup"><span data-stu-id="e43a5-365">Yes</span></span> | `"Home/About"` |

<a name="routing-ordering-ref-label"></a>
<a name="oar"></a>

### <a name="attribute-route-order"></a><span data-ttu-id="e43a5-366">Порядок маршрута атрибута</span><span class="sxs-lookup"><span data-stu-id="e43a5-366">Attribute route order</span></span>

<span data-ttu-id="e43a5-367">Маршрутизация создает дерево и сопоставляет все конечные точки одновременно:</span><span class="sxs-lookup"><span data-stu-id="e43a5-367">Routing builds a tree and matches all endpoints simultaneously:</span></span>

* <span data-ttu-id="e43a5-368">Записи маршрутов ведут себя так, как если бы они были размещены в идеальном порядке.</span><span class="sxs-lookup"><span data-stu-id="e43a5-368">The route entries behave as if placed in an ideal ordering.</span></span>
* <span data-ttu-id="e43a5-369">Наиболее конкретные маршруты могут выполняться до более общих маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-369">The most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="e43a5-370">Например, маршрут атрибута, подобный `blog/search/{topic}`, более специфичен, чем маршрут атрибута, такой как `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-370">For example, an attribute route like `blog/search/{topic}` is more specific than an attribute route like `blog/{*article}`.</span></span> <span data-ttu-id="e43a5-371">По умолчанию маршрут `blog/search/{topic}` имеет более высокий приоритет, так как он является более конкретным.</span><span class="sxs-lookup"><span data-stu-id="e43a5-371">The `blog/search/{topic}` route has higher priority, by default, because it's more specific.</span></span> <span data-ttu-id="e43a5-372">При использовании [обычной маршрутизации](#cr)разработчик несет ответственность за размещение маршрутов в нужном порядке.</span><span class="sxs-lookup"><span data-stu-id="e43a5-372">Using [conventional routing](#cr), the developer is responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="e43a5-373">Маршруты атрибутов могут настраивать порядок с помощью свойства <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order>.</span><span class="sxs-lookup"><span data-stu-id="e43a5-373">Attribute routes can configure an order using the <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order> property.</span></span> <span data-ttu-id="e43a5-374">Все указанные в платформе [атрибуты маршрута](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) включают `Order`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-374">All of the framework provided [route attributes](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) include `Order` .</span></span> <span data-ttu-id="e43a5-375">Маршруты обрабатываются в порядке возрастания значения свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-375">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="e43a5-376">Порядок по умолчанию — `0`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-376">The default order is `0`.</span></span> <span data-ttu-id="e43a5-377">Настройка маршрута с помощью `Order = -1` выполняется перед маршрутами, которые не задают порядок.</span><span class="sxs-lookup"><span data-stu-id="e43a5-377">Setting a route using `Order = -1` runs before routes that don't set an order.</span></span> <span data-ttu-id="e43a5-378">Настройка маршрута с помощью `Order = 1` выполняется после упорядочения маршрутов по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-378">Setting a route using `Order = 1` runs after default route ordering.</span></span>

<span data-ttu-id="e43a5-379">**Избегайте** в зависимости от `Order`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-379">**Avoid** depending on `Order`.</span></span> <span data-ttu-id="e43a5-380">Если для правильной маршрутизации URL-адресу приложения требуются явные значения порядка, скорее всего, это приведет к путанице с клиентами.</span><span class="sxs-lookup"><span data-stu-id="e43a5-380">If an app's URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="e43a5-381">Как правило, маршрутизация атрибутов выбирает правильный маршрут с сопоставлением URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-381">In general, attribute routing selects the correct route with URL matching.</span></span> <span data-ttu-id="e43a5-382">Если порядок по умолчанию, используемый для создания URL-адреса, не работает, использование имени маршрута в качестве переопределения обычно проще, чем применение свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-382">If the default order used for URL generation isn't working, using a route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="e43a5-383">Рассмотрим следующие два контроллера, которые определяют `/home`сопоставления маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-383">Consider the following two controllers which both define the route matching `/home`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="e43a5-384">При запросе `/home` с помощью приведенного выше кода создается исключение, аналогичное следующему:</span><span class="sxs-lookup"><span data-stu-id="e43a5-384">Requesting `/home` with the preceding code throws an exception similar to the following:</span></span>

```text
AmbiguousMatchException: The request matched multiple endpoints. Matches:

 WebMvcRouting.Controllers.HomeController.Index
 WebMvcRouting.Controllers.MyDemoController.MyIndex
```

<span data-ttu-id="e43a5-385">Добавление `Order` к одному из атрибутов маршрута разрешает неоднозначность:</span><span class="sxs-lookup"><span data-stu-id="e43a5-385">Adding `Order` to one of the route attributes resolves the ambiguity:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo3Controller.cs?name=snippet3& highlight=2)]

<span data-ttu-id="e43a5-386">В приведенном выше коде `/home` запускает конечную точку `HomeController.Index`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-386">With the preceding code, `/home` runs the `HomeController.Index` endpoint.</span></span> <span data-ttu-id="e43a5-387">Чтобы получить `MyDemoController.MyIndex`, запросите `/home/MyIndex`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-387">To get to the `MyDemoController.MyIndex`, request `/home/MyIndex`.</span></span> <span data-ttu-id="e43a5-388">**Примечание.**</span><span class="sxs-lookup"><span data-stu-id="e43a5-388">**Note**:</span></span>

* <span data-ttu-id="e43a5-389">Приведенный выше код представляет собой пример или низкую структуру маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-389">The preceding code is an example or poor routing design.</span></span> <span data-ttu-id="e43a5-390">Он использовался для иллюстрации свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-390">It was used to illustrate the `Order` property.</span></span>
* <span data-ttu-id="e43a5-391">Свойство `Order` разрешает неоднозначность, но этот шаблон не может быть сопоставлен.</span><span class="sxs-lookup"><span data-stu-id="e43a5-391">The `Order` property only resolves the ambiguity, that template cannot be matched.</span></span> <span data-ttu-id="e43a5-392">Лучше удалить шаблон `[Route("Home")]`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-392">It would be better to remove the `[Route("Home")]` template.</span></span>

<span data-ttu-id="e43a5-393">См. раздел [соглашения о Razor Pages маршрутах и приложениях. порядок](xref:razor-pages/razor-pages-conventions#route-order) маршрута для получения сведений о порядке маршрутов с Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e43a5-393">See [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order) for information on route order with Razor Pages.</span></span>

<span data-ttu-id="e43a5-394">В некоторых случаях возвращается ошибка HTTP 500 с неоднозначными маршрутами.</span><span class="sxs-lookup"><span data-stu-id="e43a5-394">In some cases, an HTTP 500 error is returned with ambiguous routes.</span></span> <span data-ttu-id="e43a5-395">Используйте [ведение журнала](xref:fundamentals/logging/index) , чтобы узнать, какие конечные точки привели к возникновению `AmbiguousMatchException`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-395">Use [logging](xref:fundamentals/logging/index) to see which endpoints caused the `AmbiguousMatchException`.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="e43a5-396">Замена токенов в шаблонах маршрутов [контроллер], [действие], [область]</span><span class="sxs-lookup"><span data-stu-id="e43a5-396">Token replacement in route templates [controller], [action], [area]</span></span>

<span data-ttu-id="e43a5-397">Для удобства маршруты к атрибутам поддерживают замену маркера для зарезервированных параметров маршрута путем заключения маркера в один из следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="e43a5-397">For convenience, attribute routes support token replacement for reserved route parameters by enclosing a token in one of the following:</span></span>

* <span data-ttu-id="e43a5-398">Квадратные скобки: `[]`</span><span class="sxs-lookup"><span data-stu-id="e43a5-398">Square braces: `[]`</span></span>
* <span data-ttu-id="e43a5-399">Фигурные скобки: `{}`</span><span class="sxs-lookup"><span data-stu-id="e43a5-399">Curly braces: `{}`</span></span>

<span data-ttu-id="e43a5-400">Маркеры `[action]`, `[area]`и `[controller]` заменяются значениями имени действия, имени области и имени контроллера из действия, в котором определен маршрут:</span><span class="sxs-lookup"><span data-stu-id="e43a5-400">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="e43a5-401">В приведенном выше коде:</span><span class="sxs-lookup"><span data-stu-id="e43a5-401">In the preceding code:</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet10)]

  * <span data-ttu-id="e43a5-402">Соответствует `/Products0/List`</span><span class="sxs-lookup"><span data-stu-id="e43a5-402">Matches `/Products0/List`</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet11)]

  * <span data-ttu-id="e43a5-403">Соответствует `/Products0/Edit/{id}`</span><span class="sxs-lookup"><span data-stu-id="e43a5-403">Matches `/Products0/Edit/{id}`</span></span>

<span data-ttu-id="e43a5-404">Замена токенов происходит на последнем этапе создания маршрутов на основе атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-404">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="e43a5-405">Предыдущий пример ведет себя так же, как и следующий код:</span><span class="sxs-lookup"><span data-stu-id="e43a5-405">The preceding example behaves the same as the following code:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet20)]

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="e43a5-406">Маршруты на основе атрибутов могут также сочетаться с наследованием.</span><span class="sxs-lookup"><span data-stu-id="e43a5-406">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="e43a5-407">Это мощное сочетание с заменой токенов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-407">This is powerful combined with token replacement.</span></span> <span data-ttu-id="e43a5-408">Замена токенов также применяется к именам маршрутов, определенным в маршрутах на основе атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-408">Token replacement also applies to route names defined by attribute routes.</span></span>
<span data-ttu-id="e43a5-409">`[Route("[controller]/[action]", Name="[controller]_[action]")]`создает уникальное имя маршрута для каждого действия:</span><span class="sxs-lookup"><span data-stu-id="e43a5-409">`[Route("[controller]/[action]", Name="[controller]_[action]")]`generates a unique route name for each action:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet5)]

<span data-ttu-id="e43a5-410">Замена токенов также применяется к именам маршрутов, определенным в маршрутах на основе атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-410">Token replacement also applies to route names defined by attribute routes.</span></span>
`[Route("[controller]/[action]", Name="[controller]_[action]")]`
<span data-ttu-id="e43a5-411">формирует уникальное имя маршрута для каждого действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-411">generates a unique route name for each action.</span></span>

<span data-ttu-id="e43a5-412">Для сопоставления с литеральным разделителем замены токенов `[` или `]` его следует экранировать путем повтора символа (`[[` или `]]`).</span><span class="sxs-lookup"><span data-stu-id="e43a5-412">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="e43a5-413">Использование преобразователя параметров для настройки замены токенов</span><span class="sxs-lookup"><span data-stu-id="e43a5-413">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="e43a5-414">Замену токенов можно настроить, используя преобразователь параметров.</span><span class="sxs-lookup"><span data-stu-id="e43a5-414">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="e43a5-415">Преобразователь параметров реализует <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> и преобразует значения параметров.</span><span class="sxs-lookup"><span data-stu-id="e43a5-415">A parameter transformer implements <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> and transforms the value of parameters.</span></span> <span data-ttu-id="e43a5-416">Например, настраиваемый `SlugifyParameterTransformer` параметра Transformer изменяет значение `SubscriptionManagement` маршрута на `subscription-management`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-416">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet2)]

<span data-ttu-id="e43a5-417"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> является соглашением для модели приложения, которое:</span><span class="sxs-lookup"><span data-stu-id="e43a5-417">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> is an application model convention that:</span></span>

* <span data-ttu-id="e43a5-418">Применяет преобразователь параметров ко всем маршрутам атрибута в приложении.</span><span class="sxs-lookup"><span data-stu-id="e43a5-418">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="e43a5-419">Настраивает значения токена маршрут атрибута при замене.</span><span class="sxs-lookup"><span data-stu-id="e43a5-419">Customizes the attribute route token values as they are replaced.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/SubscriptionManagementController.cs?name=snippet)]

<span data-ttu-id="e43a5-420">Предыдущий метод `ListAll` соответствует `/subscription-management/list-all`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-420">The preceding `ListAll` method matches `/subscription-management/list-all`.</span></span>

<span data-ttu-id="e43a5-421">`RouteTokenTransformerConvention` регистрируется в качестве параметра в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-421">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet)]

<span data-ttu-id="e43a5-422">Определение служебной версии см. в [веб-документах MDN в служебной системе](https://developer.mozilla.org/docs/Glossary/Slug) .</span><span class="sxs-lookup"><span data-stu-id="e43a5-422">See [MDN web docs on Slug](https://developer.mozilla.org/docs/Glossary/Slug) for the definition of Slug.</span></span>

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-attribute-routes"></a><span data-ttu-id="e43a5-423">Несколько маршрутов атрибутов</span><span class="sxs-lookup"><span data-stu-id="e43a5-423">Multiple attribute routes</span></span>

<span data-ttu-id="e43a5-424">Маршрутизация с помощью атрибутов поддерживает определение нескольких маршрутов к одному и тому же действию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-424">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="e43a5-425">Наиболее распространенным применением этого способа является имитация поведения стандартного маршрута по умолчанию, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e43a5-425">The most common usage of this is to mimic the behavior of the default conventional route as shown in the following example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6x)]

<span data-ttu-id="e43a5-426">Размещение нескольких атрибутов маршрута на контроллере означает, что каждый из них объединяется с каждым атрибутом маршрута в методах действия:</span><span class="sxs-lookup"><span data-stu-id="e43a5-426">Putting multiple route attributes on the controller means that each one combines with each of the route attributes on the action methods:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6)]

<span data-ttu-id="e43a5-427">Все ограничения маршрута [HTTP-команд](#verb) реализуют `IActionConstraint`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-427">All the [HTTP verb](#verb) route constraints implement `IActionConstraint`.</span></span>

<span data-ttu-id="e43a5-428">Если в действие помещаются несколько атрибутов маршрута, реализующих <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>:</span><span class="sxs-lookup"><span data-stu-id="e43a5-428">When multiple route attributes that implement <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> are placed on an action:</span></span>

* <span data-ttu-id="e43a5-429">Каждое ограничение действия объединяется с шаблоном маршрута, примененным к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="e43a5-429">Each action constraint combines with the route template applied to the controller.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet7)]

<span data-ttu-id="e43a5-430">Использование нескольких маршрутов в действиях может показаться полезным и мощным, поэтому лучше обеспечить базовое и четкое определение пространства URL-адресов вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-430">Using multiple routes on actions might seem useful and powerful, it's better to keep your app's URL space basic and well defined.</span></span> <span data-ttu-id="e43a5-431">Используйте несколько маршрутов **в действиях** , если это необходимо, например, для поддержки существующих клиентов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-431">Use multiple routes on actions **only** where needed, for example, to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="e43a5-432">Задание необязательных параметров, значений по умолчанию и ограничений для маршрутов на основе атрибутов</span><span class="sxs-lookup"><span data-stu-id="e43a5-432">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="e43a5-433">Маршруты на основе атрибутов поддерживают тот же синтаксис для указания необязательных параметров, значений по умолчанию и ограничений, что и маршруты на основе соглашений.</span><span class="sxs-lookup"><span data-stu-id="e43a5-433">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet8&highlight=3)]

<span data-ttu-id="e43a5-434">В приведенном выше коде `[HttpPost("product/{id:int}")]` применяет ограничение маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-434">In the preceding code, `[HttpPost("product/{id:int}")]` applies a route constraint.</span></span> <span data-ttu-id="e43a5-435">`ProductsController.ShowProduct` действие сопоставляется только с URL-путями, такими как `/product/3`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-435">The `ProductsController.ShowProduct` action is matched only by URL paths like `/product/3`.</span></span> <span data-ttu-id="e43a5-436">Часть шаблона маршрута `{id:int}` ограничивает этот сегмент только целыми числами.</span><span class="sxs-lookup"><span data-stu-id="e43a5-436">The route template portion `{id:int}` constrains that segment to only integers.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

<span data-ttu-id="e43a5-437">Подробное описание синтаксиса шаблона маршрута см. в разделе [Справочник по шаблону маршрута](xref:fundamentals/routing#route-template-reference).</span><span class="sxs-lookup"><span data-stu-id="e43a5-437">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="e43a5-438">Настраиваемые атрибуты маршрута с помощью Ираутетемплатепровидер</span><span class="sxs-lookup"><span data-stu-id="e43a5-438">Custom route attributes using IRouteTemplateProvider</span></span>

<span data-ttu-id="e43a5-439">Все [атрибуты маршрута](#rt) реализуют <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider>.</span><span class="sxs-lookup"><span data-stu-id="e43a5-439">All of the [route attributes](#rt) implement <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider>.</span></span> <span data-ttu-id="e43a5-440">Среда выполнения ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e43a5-440">The ASP.NET Core runtime:</span></span>

* <span data-ttu-id="e43a5-441">Ищет атрибуты классов контроллеров и методов действий при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-441">Looks for attributes on controller classes and action methods when the app starts.</span></span>
* <span data-ttu-id="e43a5-442">Использует атрибуты, реализующие `IRouteTemplateProvider`, для построения начального набора маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-442">Uses the attributes that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="e43a5-443">Реализуйте `IRouteTemplateProvider` для определения пользовательских атрибутов маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-443">Implement `IRouteTemplateProvider` to define custom route attributes.</span></span> <span data-ttu-id="e43a5-444">Каждая реализация `IRouteTemplateProvider` позволяет определить один маршрут с пользовательским шаблоном маршрута, порядком и именем.</span><span class="sxs-lookup"><span data-stu-id="e43a5-444">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyTestApiController.cs?name=snippet&highlight=1-10)]

<span data-ttu-id="e43a5-445">Предыдущий метод `Get` возвращает `Order = 2, Template = api/MyTestApi`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-445">The preceding `Get` method returns `Order = 2, Template = api/MyTestApi`.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="use-application-model-to-customize-attribute-routes"></a><span data-ttu-id="e43a5-446">Использование модели приложения для настройки маршрутов к атрибутам</span><span class="sxs-lookup"><span data-stu-id="e43a5-446">Use application model to customize attribute routes</span></span>

<span data-ttu-id="e43a5-447">Модель приложения:</span><span class="sxs-lookup"><span data-stu-id="e43a5-447">The application model:</span></span>

* <span data-ttu-id="e43a5-448">Объектная модель, создаваемая при запуске.</span><span class="sxs-lookup"><span data-stu-id="e43a5-448">Is an object model created at startup.</span></span>
* <span data-ttu-id="e43a5-449">Содержит все метаданные, используемые ASP.NET Core для маршрутизации и выполнения действий в приложении.</span><span class="sxs-lookup"><span data-stu-id="e43a5-449">Contains all of the metadata used by ASP.NET Core to route and execute the actions in an app.</span></span>

<span data-ttu-id="e43a5-450">Модель приложения включает все данные, собранные из атрибутов маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-450">The application model includes all of the data gathered from route attributes.</span></span> <span data-ttu-id="e43a5-451">Данные из атрибутов маршрута предоставляются реализацией `IRouteTemplateProvider`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-451">The data from route attributes is provided by the `IRouteTemplateProvider` implementation.</span></span> <span data-ttu-id="e43a5-452">Именован</span><span class="sxs-lookup"><span data-stu-id="e43a5-452">Conventions:</span></span>

* <span data-ttu-id="e43a5-453">Можно написать, чтобы изменить модель приложения, чтобы настроить маршрутизацию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-453">Can be written to modify the application model to customize how routing behaves.</span></span>
* <span data-ttu-id="e43a5-454">Считываются при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-454">Are read at app startup.</span></span>

<span data-ttu-id="e43a5-455">В этом разделе показан простой пример настройки маршрутизации с помощью модели приложения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-455">This section shows a basic example of customizing routing using application model.</span></span> <span data-ttu-id="e43a5-456">Следующий код делает маршруты примерно в соответствии со структурой папок проекта.</span><span class="sxs-lookup"><span data-stu-id="e43a5-456">The following code makes routes roughly line up with the folder structure of the project.</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet)]

<span data-ttu-id="e43a5-457">Следующий код предотвращает применение `namespace`ного соглашения к контроллерам, которые направляются по атрибуту:</span><span class="sxs-lookup"><span data-stu-id="e43a5-457">The following code prevents the `namespace` convention from being applied to controllers that are attribute routed:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet2)]

<span data-ttu-id="e43a5-458">Например, следующий контроллер не использует `NamespaceRoutingConvention`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-458">For example, the following controller doesn't use `NamespaceRoutingConvention`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/ManagersController.cs?name=snippet&highlight=1)]

<span data-ttu-id="e43a5-459">Метод `NamespaceRoutingConvention.Apply`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-459">The `NamespaceRoutingConvention.Apply` method:</span></span>

* <span data-ttu-id="e43a5-460">Не выполняет никаких действий, если контроллер является перенаправляемым атрибутом.</span><span class="sxs-lookup"><span data-stu-id="e43a5-460">Does nothing if the controller is attribute routed.</span></span>
* <span data-ttu-id="e43a5-461">Задает шаблон контроллеров на основе `namespace`с удалением базового `namespace`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-461">Sets the controllers template based on the `namespace`, with the base `namespace` removed.</span></span>

<span data-ttu-id="e43a5-462">`NamespaceRoutingConvention` можно применить в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-462">The `NamespaceRoutingConvention` can be applied in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Startup.cs?name=snippet&highlight=1,14-18)]

<span data-ttu-id="e43a5-463">Например, рассмотрим следующий контроллер:</span><span class="sxs-lookup"><span data-stu-id="e43a5-463">For example, consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/UsersController.cs)]

<span data-ttu-id="e43a5-464">В приведенном выше коде:</span><span class="sxs-lookup"><span data-stu-id="e43a5-464">In the preceding code:</span></span>

* <span data-ttu-id="e43a5-465">Базовый `namespace` `My.Application`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-465">The base `namespace` is `My.Application`.</span></span>
* <span data-ttu-id="e43a5-466">Полное имя предыдущего контроллера — `My.Application.Admin.Controllers.UsersController`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-466">The full name of the preceding controller is `My.Application.Admin.Controllers.UsersController`.</span></span>
* <span data-ttu-id="e43a5-467">`NamespaceRoutingConvention` задает для шаблона контроллеров значение `Admin/Controllers/Users/[action]/{id?`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-467">The `NamespaceRoutingConvention` sets the controllers template to `Admin/Controllers/Users/[action]/{id?`.</span></span>

<span data-ttu-id="e43a5-468">`NamespaceRoutingConvention` также можно применить в качестве атрибута в контроллере:</span><span class="sxs-lookup"><span data-stu-id="e43a5-468">The `NamespaceRoutingConvention` can also be applied as an attribute on a controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/TestController.cs?name=snippet&highlight=1)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="e43a5-469">Смешанная маршрутизация с помощью атрибутов и на основе соглашений</span><span class="sxs-lookup"><span data-stu-id="e43a5-469">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="e43a5-470">ASP.NET Core приложения могут сочетать использование обычной маршрутизации и маршрутизации атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-470">ASP.NET Core apps can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="e43a5-471">Маршруты на основе соглашений часто применяются для контроллеров, предоставляющих HTML-страницы для браузеров, а маршруты на основе атрибутов — для контроллеров, предоставляющих интерфейсы REST API.</span><span class="sxs-lookup"><span data-stu-id="e43a5-471">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="e43a5-472">Действия маршрутизируются либо на основе соглашений, либо с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-472">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="e43a5-473">При добавлении маршрута к контроллеру или действию они становятся маршрутизируемыми с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-473">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="e43a5-474">Действия, определяющие маршруты на основе атрибутов, недоступны по маршрутам на основе соглашений, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="e43a5-474">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="e43a5-475">**Любой** атрибут маршрута на контроллере делает **все** действия в атрибуте Controller перенаправлены.</span><span class="sxs-lookup"><span data-stu-id="e43a5-475">**Any** route attribute on the controller makes **all** actions in the controller attribute routed.</span></span>

<span data-ttu-id="e43a5-476">Маршрутизация атрибутов и Обычная маршрутизация используют один и тот же механизм маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-476">Attribute routing and conventional routing use the same routing engine.</span></span>

<a name="routing-url-gen-ref-label"></a>
<a name="ambient"></a>

## <a name="url-generation-and-ambient-values"></a><span data-ttu-id="e43a5-477">Создание URL-адресов и значения окружения</span><span class="sxs-lookup"><span data-stu-id="e43a5-477">URL Generation and ambient values</span></span>

<span data-ttu-id="e43a5-478">Приложения могут использовать функции создания URL-адресов маршрутизации для создания URL-ссылок на действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-478">Apps can use routing URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="e43a5-479">Создание URL-адресов устраняет прописано URL-адреса, делая код более надежным и сопровождаемым.</span><span class="sxs-lookup"><span data-stu-id="e43a5-479">Generating URLs eliminates hardcoding URLs, making code more robust and maintainable.</span></span> <span data-ttu-id="e43a5-480">В этом разделе рассматриваются функции создания URL-адресов, предоставляемые MVC, и описываются только основные принципы работы формирования URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-480">This section focuses on the URL generation features provided by MVC and only cover basics of how URL generation works.</span></span> <span data-ttu-id="e43a5-481">Подробное описание формирования URL-адреса см. в статье [Маршрутизация](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="e43a5-481">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="e43a5-482">Интерфейс <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> является базовым элементом инфраструктуры между MVC и маршрутизацией для создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-482">The <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> interface is the underlying element of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="e43a5-483">Экземпляр `IUrlHelper` доступен через свойство `Url` в контроллерах, представлениях и компонентах представления.</span><span class="sxs-lookup"><span data-stu-id="e43a5-483">An instance of `IUrlHelper` is available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="e43a5-484">В следующем примере интерфейс `IUrlHelper` используется с помощью свойства `Controller.Url` для создания URL-адреса другого действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-484">In the following example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="e43a5-485">Если приложение использует стандартный маршрут по умолчанию, значением переменной `url` является строка URL-пути `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-485">If the app is using the default conventional route, the value of the `url` variable is the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="e43a5-486">Этот путь URL-адреса создается при маршрутизации путем объединения:</span><span class="sxs-lookup"><span data-stu-id="e43a5-486">This URL path is created by routing by combining:</span></span>

* <span data-ttu-id="e43a5-487">Значения маршрута из текущего запроса, которые называются **внешними значениями**.</span><span class="sxs-lookup"><span data-stu-id="e43a5-487">The route values from the current request, which are called **ambient values**.</span></span>
* <span data-ttu-id="e43a5-488">Значения, передаваемые в `Url.Action` и подставив эти значения в шаблон маршрута:</span><span class="sxs-lookup"><span data-stu-id="e43a5-488">The values passed to `Url.Action` and substituting those values into the route template:</span></span>

``` text
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="e43a5-489">Значение каждого параметра маршрута в шаблоне маршрута заменяется соответствующими именами со значениями и значениями окружения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-489">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="e43a5-490">Параметр маршрута, который не имеет значения, может:</span><span class="sxs-lookup"><span data-stu-id="e43a5-490">A route parameter that doesn't have a value can:</span></span>

* <span data-ttu-id="e43a5-491">Используйте значение по умолчанию, если таковое имеется.</span><span class="sxs-lookup"><span data-stu-id="e43a5-491">Use a default value if it has one.</span></span>
* <span data-ttu-id="e43a5-492">Пропускается, если он необязателен.</span><span class="sxs-lookup"><span data-stu-id="e43a5-492">Be skipped if it's optional.</span></span> <span data-ttu-id="e43a5-493">Например, `id` из шаблона маршрута `{controller}/{action}/{id?}`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-493">For example, the `id` from the  route template `{controller}/{action}/{id?}`.</span></span>

<span data-ttu-id="e43a5-494">Создание URL-адреса завершается ошибкой, если ни один из обязательных параметров маршрута не имеет соответствующего значения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-494">URL generation fails if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="e43a5-495">Если для маршрута не удалось сформировать URL-адрес, проверяется следующий маршрут, пока не будут проверены все маршруты или не будет найдено соответствие.</span><span class="sxs-lookup"><span data-stu-id="e43a5-495">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="e43a5-496">В предыдущем примере `Url.Action` предполагается [Обычная маршрутизация](#cr).</span><span class="sxs-lookup"><span data-stu-id="e43a5-496">The preceding example of `Url.Action` assumes [conventional routing](#cr).</span></span> <span data-ttu-id="e43a5-497">Создание URL-адреса работает аналогично [маршрутизации атрибутов](#ar), хотя понятия различны.</span><span class="sxs-lookup"><span data-stu-id="e43a5-497">URL generation works similarly with [attribute routing](#ar), though the concepts are different.</span></span> <span data-ttu-id="e43a5-498">С обычной маршрутизацией:</span><span class="sxs-lookup"><span data-stu-id="e43a5-498">With conventional routing:</span></span>

* <span data-ttu-id="e43a5-499">Значения маршрута используются для расширения шаблона.</span><span class="sxs-lookup"><span data-stu-id="e43a5-499">The route values are used to expand a template.</span></span>
* <span data-ttu-id="e43a5-500">Значения маршрута для `controller` и `action` обычно отображаются в этом шаблоне.</span><span class="sxs-lookup"><span data-stu-id="e43a5-500">The route values for `controller` and `action` usually appear in that template.</span></span> <span data-ttu-id="e43a5-501">Это работает потому, что URL-адреса, соответствующие маршрутизации, соответствуют соглашению.</span><span class="sxs-lookup"><span data-stu-id="e43a5-501">This works because the URLs matched by routing adhere to a convention.</span></span>

<span data-ttu-id="e43a5-502">В следующем примере используется маршрутизация атрибутов:</span><span class="sxs-lookup"><span data-stu-id="e43a5-502">The following example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationAttrController.cs?name=snippet_1)]

<span data-ttu-id="e43a5-503">`Source` действие в предыдущем коде создает `custom/url/to/destination`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-503">The `Source` action in the preceding code generates `custom/url/to/destination`.</span></span>

<span data-ttu-id="e43a5-504"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> был добавлен в ASP.NET Core 3,0 в качестве альтернативы `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-504"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> was added in ASP.NET Core 3.0 as an alternative to `IUrlHelper`.</span></span> <span data-ttu-id="e43a5-505">`LinkGenerator` предлагает аналогичные, но более гибкие функции.</span><span class="sxs-lookup"><span data-stu-id="e43a5-505">`LinkGenerator` offers similar but more flexible functionality.</span></span> <span data-ttu-id="e43a5-506">Все остальные методы в `IUrlHelper` также имеют соответствующее семейство методов для `LinkGenerator`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-506">Each other the methods on `IUrlHelper` has a corresponding family of methods on `LinkGenerator` as well.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="e43a5-507">Формирование URL-адресов по имени действия</span><span class="sxs-lookup"><span data-stu-id="e43a5-507">Generating URLs by action name</span></span>

<span data-ttu-id="e43a5-508">[URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [линкженератор. жетпасбяктион](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*)и все связанные перегрузки предназначены для создания целевой конечной точки путем указания имени контроллера и имени действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-508">[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [LinkGenerator.GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*), and all related overloads all are designed to generate the target endpoint by specifying a controller name and action name.</span></span>

<span data-ttu-id="e43a5-509">При использовании `Url.Action`текущие значения маршрута для `controller` и `action` предоставляются средой выполнения:</span><span class="sxs-lookup"><span data-stu-id="e43a5-509">When using `Url.Action`, the current route values for `controller` and `action` are provided by the runtime:</span></span>

* <span data-ttu-id="e43a5-510">Значение `controller` и `action` являются частью как [значений окружающих](#ambient) элементов, так и значений.</span><span class="sxs-lookup"><span data-stu-id="e43a5-510">The value of `controller` and `action` are part of both [ambient values](#ambient) and values.</span></span> <span data-ttu-id="e43a5-511">Метод `Url.Action` всегда использует текущие значения `action` и `controller` и создает URL-путь, который направляет в текущее действие.</span><span class="sxs-lookup"><span data-stu-id="e43a5-511">The method `Url.Action` always uses the current values of `action` and `controller` and generates a URL path that routes to the current action.</span></span>

<span data-ttu-id="e43a5-512">Маршрутизация пытается использовать значения во внешних значениях для заполнения сведений, которые не были предоставлены при формировании URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-512">Routing attempts to use the values in ambient values to fill in information that wasn't provided when generating a URL.</span></span> <span data-ttu-id="e43a5-513">Рассмотрим такой маршрут, как `{a}/{b}/{c}/{d}` со значениями окружения `{ a = Alice, b = Bob, c = Carol, d = David }`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-513">Consider a route like `{a}/{b}/{c}/{d}` with ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`:</span></span>

* <span data-ttu-id="e43a5-514">Маршрутизация содержит достаточно информации для создания URL-адреса без дополнительных значений.</span><span class="sxs-lookup"><span data-stu-id="e43a5-514">Routing has enough information to generate a URL without any additional values.</span></span>
* <span data-ttu-id="e43a5-515">Маршрутизация содержит достаточно информации, так как все параметры маршрута имеют значение.</span><span class="sxs-lookup"><span data-stu-id="e43a5-515">Routing has enough information because all route parameters have a value.</span></span>

<span data-ttu-id="e43a5-516">Если добавляется значение `{ d = Donovan }`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-516">If the value `{ d = Donovan }` is added:</span></span>

* <span data-ttu-id="e43a5-517">Значение `{ d = David }` игнорируется.</span><span class="sxs-lookup"><span data-stu-id="e43a5-517">The value `{ d = David }` is ignored.</span></span>
* <span data-ttu-id="e43a5-518">Путь к созданному URL-адресу — `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-518">The generated URL path is `Alice/Bob/Carol/Donovan`.</span></span>

<span data-ttu-id="e43a5-519">**Предупреждение**: URL-пути являются иерархическими.</span><span class="sxs-lookup"><span data-stu-id="e43a5-519">**Warning**: URL paths are hierarchical.</span></span> <span data-ttu-id="e43a5-520">В предыдущем примере, если добавляется значение `{ c = Cheryl }`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-520">In the preceding example, if the value `{ c = Cheryl }` is added:</span></span>

* <span data-ttu-id="e43a5-521">Оба значения `{ c = Carol, d = David }` игнорируются.</span><span class="sxs-lookup"><span data-stu-id="e43a5-521">Both of the values `{ c = Carol, d = David }` are ignored.</span></span>
* <span data-ttu-id="e43a5-522">Больше нет значения для `d` и создание URL-адреса завершается неудачей.</span><span class="sxs-lookup"><span data-stu-id="e43a5-522">There is no longer a value for `d` and URL generation fails.</span></span>
* <span data-ttu-id="e43a5-523">Для создания URL-адреса необходимо указать требуемые значения `c` и `d`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-523">The desired values of `c` and `d` must be specified to generate a URL.</span></span>  

<span data-ttu-id="e43a5-524">Возможно, вы намерены столкнуться с этой проблемой с `{controller}/{action}/{id?}`маршрута по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-524">You might expect to hit this problem with the default route `{controller}/{action}/{id?}`.</span></span> <span data-ttu-id="e43a5-525">Эта проблема возникает редко, поскольку `Url.Action` всегда явно указывает `controller` и `action` значение.</span><span class="sxs-lookup"><span data-stu-id="e43a5-525">This problem is rare in practice because `Url.Action` always explicitly specifies a `controller` and `action` value.</span></span>

<span data-ttu-id="e43a5-526">Несколько перегрузок [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) принимают объект значений маршрута, чтобы предоставить значения для параметров маршрута, отличных от `controller` и `action`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-526">Several overloads of [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) take a route values object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="e43a5-527">Объект значений маршрута часто используется с `id`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-527">The route values object is frequently used with `id`.</span></span> <span data-ttu-id="e43a5-528">Например, `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-528">For example, `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="e43a5-529">Объект значений маршрута:</span><span class="sxs-lookup"><span data-stu-id="e43a5-529">The route values object:</span></span>

* <span data-ttu-id="e43a5-530">По соглашению обычно является объектом анонимного типа.</span><span class="sxs-lookup"><span data-stu-id="e43a5-530">By convention is usually an object of anonymous type.</span></span>
* <span data-ttu-id="e43a5-531">Может быть `IDictionary<>` или [POCO](https://wikipedia.org/wiki/Plain_old_CLR_object)).</span><span class="sxs-lookup"><span data-stu-id="e43a5-531">Can be an `IDictionary<>` or a [POCO](https://wikipedia.org/wiki/Plain_old_CLR_object)).</span></span>

<span data-ttu-id="e43a5-532">Остальные значения маршрута, которые не соответствуют параметрам маршрута, помещаются в строку запроса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-532">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="e43a5-533">Приведенный выше код создает `/Products/Buy/17?color=red`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-533">The preceding code generates `/Products/Buy/17?color=red`.</span></span>

<span data-ttu-id="e43a5-534">Следующий код создает абсолютный URL-адрес:</span><span class="sxs-lookup"><span data-stu-id="e43a5-534">The following code generates an absolute URL:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet2)]

<span data-ttu-id="e43a5-535">Чтобы создать абсолютный URL-адрес, используйте один из следующих способов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-535">To create an absolute URL, use one of the following:</span></span>

* <span data-ttu-id="e43a5-536">Перегрузка, принимающая `protocol`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-536">An overload that accepts a `protocol`.</span></span> <span data-ttu-id="e43a5-537">Например, приведенный выше код.</span><span class="sxs-lookup"><span data-stu-id="e43a5-537">For example, the preceding code.</span></span>
* <span data-ttu-id="e43a5-538">[Линкженератор. жетурибяктион](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*), который по умолчанию создает абсолютные URI.</span><span class="sxs-lookup"><span data-stu-id="e43a5-538">[LinkGenerator.GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*), which generates absolute URIs by default.</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generate-urls-by-route"></a><span data-ttu-id="e43a5-539">Создание URL-адресов по маршруту</span><span class="sxs-lookup"><span data-stu-id="e43a5-539">Generate URLs by route</span></span>

<span data-ttu-id="e43a5-540">Приведенный выше код демонстрирует создание URL-адреса путем передачи контроллера и имени действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-540">The preceding code demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="e43a5-541">`IUrlHelper` также предоставляет семейство методов [URL. RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*) .</span><span class="sxs-lookup"><span data-stu-id="e43a5-541">`IUrlHelper` also provides the [Url.RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*) family of methods.</span></span> <span data-ttu-id="e43a5-542">Эти методы похожи на [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), но не копируют текущие значения `action` и `controller` в значения маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-542">These methods are similar to [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="e43a5-543">Наиболее распространенное использование `Url.RouteUrl`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-543">The most common usage of `Url.RouteUrl`:</span></span>

* <span data-ttu-id="e43a5-544">Указывает имя маршрута для создания URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-544">Specifies a route name to generate the URL.</span></span>
* <span data-ttu-id="e43a5-545">Обычно не указывает имя контроллера или действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-545">Generally doesn't specify a controller or action name.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGeneration2Controller.cs?name=snippet_1)]

<span data-ttu-id="e43a5-546">Следующий файл Razor создает ссылку HTML на `Destination_Route`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-546">The following Razor file generates an HTML link to the `Destination_Route`:</span></span>

[!code-cshtml[](routing/samples/3.x/main/Views/Shared/MyLink.cshtml)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generate-urls-in-html-and-razor"></a><span data-ttu-id="e43a5-547">Создание URL-адресов в HTML и Razor</span><span class="sxs-lookup"><span data-stu-id="e43a5-547">Generate URLs in HTML and Razor</span></span>

<span data-ttu-id="e43a5-548"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper> предоставляет <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> методов [HTML. бегинформ](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) и [HTML. ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) для создания элементов `<form>` и `<a>` соответственно.</span><span class="sxs-lookup"><span data-stu-id="e43a5-548"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper> provides the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> methods [Html.BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) and [Html.ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="e43a5-549">Эти методы используют метод [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) для создания URL-адреса и принимают аналогичные аргументы.</span><span class="sxs-lookup"><span data-stu-id="e43a5-549">These methods use the [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="e43a5-550">Эквивалентами методов `Url.RouteUrl` для `HtmlHelper` являются методы `Html.BeginRouteForm` и `Html.RouteLink`, которые имеют схожие функции.</span><span class="sxs-lookup"><span data-stu-id="e43a5-550">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="e43a5-551">Для формирования URL-адресов используются вспомогательные функции тегов `form` и `<a>`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-551">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="e43a5-552">Обе они реализуются с помощью интерфейса `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-552">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="e43a5-553">Дополнительные сведения см. [в разделе вспомогательные функции тегов в формах](xref:mvc/views/working-with-forms) .</span><span class="sxs-lookup"><span data-stu-id="e43a5-553">See [Tag Helpers in forms](xref:mvc/views/working-with-forms) for more information.</span></span>

<span data-ttu-id="e43a5-554">Внутри представлений интерфейс `IUrlHelper` доступен посредством свойства `Url` для особых случаев формирования URL-адресов, помимо описанных выше.</span><span class="sxs-lookup"><span data-stu-id="e43a5-554">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="url-generation-in-action-results"></a><span data-ttu-id="e43a5-555">Создание URL-адресов в результатах действий</span><span class="sxs-lookup"><span data-stu-id="e43a5-555">URL generation in Action Results</span></span>

<span data-ttu-id="e43a5-556">В предыдущих примерах показано использование `IUrlHelper` в контроллере.</span><span class="sxs-lookup"><span data-stu-id="e43a5-556">The preceding examples showed using `IUrlHelper` in a controller.</span></span> <span data-ttu-id="e43a5-557">Наиболее распространенным использованием контроллера является создание URL-адреса как части результата действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-557">The most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="e43a5-558">Базовые классы <xref:Microsoft.AspNetCore.Mvc.ControllerBase> и <xref:Microsoft.AspNetCore.Mvc.Controller> предоставляют удобные методы для результатов действий, ссылающихся на другое действие.</span><span class="sxs-lookup"><span data-stu-id="e43a5-558">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase> and <xref:Microsoft.AspNetCore.Mvc.Controller> base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="e43a5-559">Один из типичных способов использования — перенаправление после приема входных данных пользователя:</span><span class="sxs-lookup"><span data-stu-id="e43a5-559">One typical usage is to redirect after accepting user input:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/CustomerController.cs?name=snippet)]

<span data-ttu-id="e43a5-560">Такие методы фабрики результатов действий, как <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*> и <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>, соответствуют методам в `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-560">The action results factory methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="e43a5-561">Выделенные маршруты на основе соглашений</span><span class="sxs-lookup"><span data-stu-id="e43a5-561">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="e43a5-562">В [обычной маршрутизации](#cr) может использоваться особый тип определения маршрута, называемый [выделенным обычным маршрутом](#dcr).</span><span class="sxs-lookup"><span data-stu-id="e43a5-562">[Conventional routing](#cr) can use a special kind of route definition called a [dedicated conventional route](#dcr).</span></span> <span data-ttu-id="e43a5-563">В следующем примере маршрут с именем `blog` является выделенным обычным маршрутом:</span><span class="sxs-lookup"><span data-stu-id="e43a5-563">In the following example, the route named `blog` is a dedicated conventional route:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="e43a5-564">Используя предыдущие определения маршрутов, `Url.Action("Index", "Home")` создает URL-путь `/` используя `default` маршрут, но почему?</span><span class="sxs-lookup"><span data-stu-id="e43a5-564">Using the preceding route definitions, `Url.Action("Index", "Home")` generates the URL path `/` using the `default` route, but why?</span></span> <span data-ttu-id="e43a5-565">Можно было бы предположить, что значений маршрута `{ controller = Home, action = Index }` было бы достаточно для формирования URL-адреса с помощью `blog` и результатом было бы `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-565">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="e43a5-566">[Выделенные традиционные маршруты](#dcr) полагаются на особое поведение значений по умолчанию, которые не имеют соответствующего параметра маршрута, который предотвращает слишком [жадную](xref:fundamentals/routing#greedy) маршрутизацию при формировании URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-566">[Dedicated conventional routes](#dcr) rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being too [greedy](xref:fundamentals/routing#greedy) with URL generation.</span></span> <span data-ttu-id="e43a5-567">В этом случае значения по умолчанию — `{ controller = Blog, action = Article }`, но параметров маршрута `controller` и `action` нет.</span><span class="sxs-lookup"><span data-stu-id="e43a5-567">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="e43a5-568">Когда система маршрутизации производит формирование URL-адреса, предоставленные значения должны соответствовать значениям по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-568">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="e43a5-569">Создание URL-адреса с помощью `blog` завершается сбоем, так как значения `{ controller = Home, action = Index }` не совпадают `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-569">URL generation using `blog` fails because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="e43a5-570">После этого система маршрутизации выполнит попытку использовать маршрут `default`, которая завершится успешно.</span><span class="sxs-lookup"><span data-stu-id="e43a5-570">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="e43a5-571">Зоны</span><span class="sxs-lookup"><span data-stu-id="e43a5-571">Areas</span></span>

<span data-ttu-id="e43a5-572">[Области](xref:mvc/controllers/areas) — это функция MVC, используемая для упорядочивания связанных функций в группе в виде отдельных:</span><span class="sxs-lookup"><span data-stu-id="e43a5-572">[Areas](xref:mvc/controllers/areas) are an MVC feature used to organize related functionality into a group as a separate:</span></span>

* <span data-ttu-id="e43a5-573">Пространство имен маршрутизации для действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="e43a5-573">Routing namespace for controller actions.</span></span>
* <span data-ttu-id="e43a5-574">Структура папок для представлений.</span><span class="sxs-lookup"><span data-stu-id="e43a5-574">Folder structure for views.</span></span>

<span data-ttu-id="e43a5-575">Использование областей позволяет приложению иметь несколько контроллеров с одинаковым именем, если они имеют разные области.</span><span class="sxs-lookup"><span data-stu-id="e43a5-575">Using areas allows an app to have multiple controllers with the same name, as long as they have different areas.</span></span> <span data-ttu-id="e43a5-576">При использовании областей создается иерархия в целях маршрутизации. Для этого к `area` и `controller` добавляется еще один параметр маршрута, `action`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-576">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="e43a5-577">В этом разделе обсуждается взаимодействие маршрутизации с областями.</span><span class="sxs-lookup"><span data-stu-id="e43a5-577">This section discusses how routing interacts with areas.</span></span> <span data-ttu-id="e43a5-578">Сведения об использовании областей с представлениями см. в разделе [области](xref:mvc/controllers/areas) .</span><span class="sxs-lookup"><span data-stu-id="e43a5-578">See [Areas](xref:mvc/controllers/areas) for details about how areas are used with views.</span></span>

<span data-ttu-id="e43a5-579">В следующем примере MVC настраивается для использования стандартного маршрута по умолчанию и маршрута `area` для `area` с именем `Blog`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-579">The following example configures MVC to use the default conventional route and an `area` route for an `area` named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="e43a5-580">В приведенном выше коде вызывается <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> для создания `"blog_route"`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-580">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> is called to create the `"blog_route"`.</span></span> <span data-ttu-id="e43a5-581">Вторым параметром, `"Blog"`, является имя области.</span><span class="sxs-lookup"><span data-stu-id="e43a5-581">The second parameter, `"Blog"`, is the area name.</span></span>

<span data-ttu-id="e43a5-582">При сопоставлении URL-пути, например `/Manage/Users/AddUser`, `"blog_route"` маршрут создает значения маршрута `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-582">When matching a URL path like `/Manage/Users/AddUser`, the `"blog_route"` route generates the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="e43a5-583">`area` значение маршрута создается значением по умолчанию для `area`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-583">The `area` route value is produced by a default value for `area`.</span></span> <span data-ttu-id="e43a5-584">Маршрут, созданный `MapAreaControllerRoute`, эквивалентен следующему:</span><span class="sxs-lookup"><span data-stu-id="e43a5-584">The route created by `MapAreaControllerRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup2.cs?name=snippet2)]

<span data-ttu-id="e43a5-585">Метод `MapAreaControllerRoute` создает маршрут с помощью значения по умолчанию и ограничения для `area` с использованием предоставленного имени маршрута (в данном случае `Blog`).</span><span class="sxs-lookup"><span data-stu-id="e43a5-585">`MapAreaControllerRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="e43a5-586">Значение по умолчанию гарантирует, что маршрут всегда создает значение `{ area = Blog, ... }`. Ограничение требует значения `{ area = Blog, ... }` для формирования URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-586">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

<span data-ttu-id="e43a5-587">При маршрутизации на основе соглашений учитывается порядок.</span><span class="sxs-lookup"><span data-stu-id="e43a5-587">Conventional routing is order-dependent.</span></span> <span data-ttu-id="e43a5-588">Как правило, маршруты с областями следует размещать раньше, так как они более специфичны, чем маршруты без области.</span><span class="sxs-lookup"><span data-stu-id="e43a5-588">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span>

<span data-ttu-id="e43a5-589">Используя приведенный выше пример, значения маршрута `{ area = Blog, controller = Users, action = AddUser }` соответствовать следующему действию:</span><span class="sxs-lookup"><span data-stu-id="e43a5-589">Using the preceding example, the route values `{ area = Blog, controller = Users, action = AddUser }` match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="e43a5-590">Атрибут [[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) указывает, что контроллер является частью области.</span><span class="sxs-lookup"><span data-stu-id="e43a5-590">The [[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute is what denotes a controller as part of an area.</span></span> <span data-ttu-id="e43a5-591">Этот контроллер находится в области `Blog`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-591">This controller is in the `Blog` area.</span></span> <span data-ttu-id="e43a5-592">Контроллеры без атрибута `[Area]` не являются членами какой-либо области и не **соответствуют,** если значение маршрута `area` предоставляется службой маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-592">Controllers without an `[Area]` attribute are not members of any area, and do **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="e43a5-593">В приведенном ниже примере только первый контроллер может соответствовать значениям маршрута `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-593">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

<span data-ttu-id="e43a5-594">Пространство имен каждого контроллера показано здесь для полноты.</span><span class="sxs-lookup"><span data-stu-id="e43a5-594">The namespace of each controller is shown here for completeness.</span></span> <span data-ttu-id="e43a5-595">Если предыдущие контроллеры используют одно и то же пространство имен, создается ошибка компилятора.</span><span class="sxs-lookup"><span data-stu-id="e43a5-595">If the preceding controllers uses the same namespace, a compiler error would be generated.</span></span> <span data-ttu-id="e43a5-596">Имена пространств классов не влияют на маршрутизацию в MVC.</span><span class="sxs-lookup"><span data-stu-id="e43a5-596">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="e43a5-597">Первые два контроллера входят в области и будут соответствовать запросу, только если соответствующее имя области предоставлено значением маршрута `area`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-597">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="e43a5-598">Третий контроллер не входит ни в одну область и может соответствовать запросу, только если значение `area` не предоставлено системой маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-598">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

<a name="aa"></a>

<span data-ttu-id="e43a5-599">В плане сопоставления *отсутствующих значений* отсутствие значения `area` равносильно тому, как если значением `area` было бы NULL или пустая строка.</span><span class="sxs-lookup"><span data-stu-id="e43a5-599">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="e43a5-600">При выполнении действия в области значение маршрута для `area` доступно в качестве [внешнего значения](#ambient) для маршрутизации, используемой для формирования URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-600">When executing an action inside an area, the route value for `area` is available as an [ambient value](#ambient) for routing to use for URL generation.</span></span> <span data-ttu-id="e43a5-601">Это означает, что по умолчанию области являются *фиксированными* при формировании URL-адресов, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="e43a5-601">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup3.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<span data-ttu-id="e43a5-602">Следующий код создает URL-адрес для `/Zebra/Users/AddUser`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-602">The following code generates a URL to `/Zebra/Users/AddUser`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/HomeController.cs?name=snippet)]

<a name="action"></a>

## <a name="action-definition"></a><span data-ttu-id="e43a5-603">Определение действия</span><span class="sxs-lookup"><span data-stu-id="e43a5-603">Action definition</span></span>

<span data-ttu-id="e43a5-604">Открытые методы на контроллере, за исключением тех, которые имеют атрибут [недействия](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) , являются действиями.</span><span class="sxs-lookup"><span data-stu-id="e43a5-604">Public methods on a controller, except those with the [NonAction](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) attribute, are actions.</span></span>

## <a name="sample-code"></a><span data-ttu-id="e43a5-605">Образец кода</span><span class="sxs-lookup"><span data-stu-id="e43a5-605">Sample code</span></span>

 * <span data-ttu-id="e43a5-606">Метод [мидисплайраутеинфо](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) включен в [Пример загрузки](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) и используется для вывода сведений о маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-606">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>
* <span data-ttu-id="e43a5-607">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e43a5-607">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e43a5-608">В ASP.NET Core MVC используется [ПО промежуточного слоя](xref:fundamentals/middleware/index) маршрутизации для сопоставления URL-адресов входящих запросов с действиями.</span><span class="sxs-lookup"><span data-stu-id="e43a5-608">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="e43a5-609">Маршруты определяются в коде запуска или атрибутах.</span><span class="sxs-lookup"><span data-stu-id="e43a5-609">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="e43a5-610">Они описывают то, как пути URL-адресов должны сопоставляться с действиями.</span><span class="sxs-lookup"><span data-stu-id="e43a5-610">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="e43a5-611">С помощью маршрутов также формируются URL-адреса (для ссылок), отправляемые в ответах.</span><span class="sxs-lookup"><span data-stu-id="e43a5-611">Routes are also used to generate URLs (for links) sent out in responses.</span></span>

<span data-ttu-id="e43a5-612">Действия маршрутизируются либо на основе соглашений, либо с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-612">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="e43a5-613">При добавлении маршрута к контроллеру или действию они становятся маршрутизируемыми с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-613">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="e43a5-614">Дополнительные сведения см. в разделе [Смешанная маршрутизация](#routing-mixed-ref-label).</span><span class="sxs-lookup"><span data-stu-id="e43a5-614">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="e43a5-615">В этом документе описывается взаимодействие между MVC и маршрутизацией и использование возможностей маршрутизации в типичных приложениях MVC.</span><span class="sxs-lookup"><span data-stu-id="e43a5-615">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="e43a5-616">Подробные сведения о расширенной маршрутизации см. в статье [Маршрутизация](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="e43a5-616">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="e43a5-617">Настройка ПО промежуточного слоя маршрутизации</span><span class="sxs-lookup"><span data-stu-id="e43a5-617">Setting up Routing Middleware</span></span>

<span data-ttu-id="e43a5-618">Метод *Configure* содержит код, который выглядит примерно так:</span><span class="sxs-lookup"><span data-stu-id="e43a5-618">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="e43a5-619">В вызове `UseMvc` метод `MapRoute` используется для создания одного маршрута, который называется маршрутом `default`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-619">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="e43a5-620">В большинстве приложений MVC используется маршрут с шаблоном, сходным с маршрутом `default`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-620">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="e43a5-621">Шаблон маршрута `"{controller=Home}/{action=Index}/{id?}"` может сопоставлять путь URL-адреса, например `/Products/Details/5`, и извлекает значения маршрута `{ controller = Products, action = Details, id = 5 }` путем разбивки пути на лексемы.</span><span class="sxs-lookup"><span data-stu-id="e43a5-621">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="e43a5-622">MVC попытается найти контроллер с именем `ProductsController` и выполнить действие `Details`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-622">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="e43a5-623">Обратите внимание на то, что в этом примере привязка модели будет использовать значение `id = 5`, чтобы присвоить параметру `id` значение `5` при вызове действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-623">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="e43a5-624">Дополнительные сведения см. в разделе [Привязка модели](../models/model-binding.md).</span><span class="sxs-lookup"><span data-stu-id="e43a5-624">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="e43a5-625">Использование маршрута `default`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-625">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="e43a5-626">Шаблон маршрута:</span><span class="sxs-lookup"><span data-stu-id="e43a5-626">The route template:</span></span>

* <span data-ttu-id="e43a5-627">`{controller=Home}` определяет `Home` в качестве объекта `controller` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-627">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="e43a5-628">`{action=Index}` определяет `Index` в качестве объекта `action` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-628">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="e43a5-629">`{id?}` определяет необязательный параметр `id`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-629">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="e43a5-630">Параметры маршрута по умолчанию и необязательные параметры необязательно должны присутствовать в пути URL-адреса для сопоставления.</span><span class="sxs-lookup"><span data-stu-id="e43a5-630">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="e43a5-631">Подробное описание синтаксиса шаблона маршрута см. в разделе [Справочник по шаблону маршрута](xref:fundamentals/routing#route-template-reference).</span><span class="sxs-lookup"><span data-stu-id="e43a5-631">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="e43a5-632">`"{controller=Home}/{action=Index}/{id?}"` может сопоставляться с путем URL-адреса `/` и выдает значения маршрута `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-632">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="e43a5-633">Для объектов `controller` и `action` используются значения по умолчанию. `id` не имеет значения, так как в пути URL-адреса нет соответствующего сегмента.</span><span class="sxs-lookup"><span data-stu-id="e43a5-633">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="e43a5-634">MVC будет использовать эти значения маршрута для выбора действия `HomeController` и `Index`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-634">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="e43a5-635">При использовании этого определения контроллера и шаблона маршрута действие `HomeController.Index` будет выполняться для любых из следующих путей URL-адресов:</span><span class="sxs-lookup"><span data-stu-id="e43a5-635">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="e43a5-636">Универсальный метод `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-636">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="e43a5-637">Можно использовать вместо следующего метода:</span><span class="sxs-lookup"><span data-stu-id="e43a5-637">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="e43a5-638">`UseMvc` и `UseMvcWithDefaultRoute` добавляют экземпляр `RouterMiddleware` в конвейер ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="e43a5-638">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="e43a5-639">MVC не взаимодействует с ПО промежуточного слоя напрямую, а использует маршрутизацию для обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-639">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="e43a5-640">MVC подключается к маршрутам посредством экземпляра `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-640">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="e43a5-641">Код метода `UseMvc` имеет примерно следующий вид:</span><span class="sxs-lookup"><span data-stu-id="e43a5-641">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="e43a5-642">Метод `UseMvc` не определяет маршруты напрямую, а добавляет заполнитель в коллекцию маршрутов для маршрута `attribute`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-642">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="e43a5-643">Перегрузка `UseMvc(Action<IRouteBuilder>)` позволяет добавлять собственные маршруты, а также поддерживает маршрутизацию с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-643">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="e43a5-644">Метод `UseMvc` и все его варианты добавляют заполнитель для маршрута на основе атрибутов. Маршрутизация с помощью атрибутов доступна всегда вне зависимости от того, как настроен метод `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-644">`UseMvc` and all of its variations add a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="e43a5-645">Метод `UseMvcWithDefaultRoute` определяет маршрут по умолчанию и поддерживает маршрутизацию с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-645">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="e43a5-646">В разделе [Маршрутизация с помощью атрибутов](#attribute-routing-ref-label) приводятся более подробные сведения о маршрутизации с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-646">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="e43a5-647">Маршрутизация на основе соглашений</span><span class="sxs-lookup"><span data-stu-id="e43a5-647">Conventional routing</span></span>

<span data-ttu-id="e43a5-648">Маршрут `default`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-648">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="e43a5-649">Приведенный выше код является примером обычной маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-649">The preceding code is an example of a conventional routing.</span></span> <span data-ttu-id="e43a5-650">Этот стиль называется обычной маршрутизацией, так как он устанавливает *соглашение* для URL-путей:</span><span class="sxs-lookup"><span data-stu-id="e43a5-650">This style is called conventional routing because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="e43a5-651">Первый сегмент пути сопоставляется с именем контроллера.</span><span class="sxs-lookup"><span data-stu-id="e43a5-651">The first path segment maps to the controller name.</span></span>
* <span data-ttu-id="e43a5-652">Второй сопоставляется с именем действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-652">The second maps to the action name.</span></span>
* <span data-ttu-id="e43a5-653">Третий сегмент используется для необязательных `id`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-653">The third segment is used for an optional `id`.</span></span> <span data-ttu-id="e43a5-654">`id` сопоставляется с сущностью модели.</span><span class="sxs-lookup"><span data-stu-id="e43a5-654">`id` maps to a model entity.</span></span>

<span data-ttu-id="e43a5-655">При использовании этого маршрута `default` путь URL-адреса `/Products/List` сопоставляется с действием `ProductsController.List`, а путь `/Blog/Article/17` сопоставляется с `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-655">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="e43a5-656">Такое сопоставление основывается **только** на именах контроллера и действия, но не на пространствах имен, расположениях исходных файлов или параметрах метода.</span><span class="sxs-lookup"><span data-stu-id="e43a5-656">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="e43a5-657">Использование маршрутизации на основе соглашений с маршрутом по умолчанию позволяет быстро создавать приложение, не придумывая новый шаблон URL-адреса для каждого определяемого действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-657">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="e43a5-658">В случае с приложением с действиями в стиле CRUD единообразие URL-адресов для всех контроллеров позволяет упростить код и сделать пользовательский интерфейс более предсказуемым.</span><span class="sxs-lookup"><span data-stu-id="e43a5-658">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="e43a5-659">Параметр `id` определяется в шаблоне маршрута как необязательный. Это означает, что действия могут выполняться, даже если идентификатор не указан в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="e43a5-659">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="e43a5-660">Как правило, если параметр `id` отсутствует в URL-адресе, привязка модели присваивает ему значение `0`, и в результате в базе данных не будет найдена сущность, соответствующая `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-660">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="e43a5-661">Маршрутизация с помощью атрибутов обеспечивает детальный контроль, позволяя настраивать идентификатор как обязательный лишь для некоторых действий.</span><span class="sxs-lookup"><span data-stu-id="e43a5-661">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="e43a5-662">В документации необязательные параметры, такие как `id`, будут включаться, только если они, скорее всего, могут использоваться в соответствующей ситуации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-662">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="e43a5-663">Несколько маршрутов</span><span class="sxs-lookup"><span data-stu-id="e43a5-663">Multiple routes</span></span>

<span data-ttu-id="e43a5-664">В метод `UseMvc` можно добавить несколько маршрутов, добавив дополнительные вызовы `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-664">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="e43a5-665">Таким образом можно определить несколько соглашений или добавить маршруты на основе соглашений, предназначенные для определенного действия, например:</span><span class="sxs-lookup"><span data-stu-id="e43a5-665">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="e43a5-666">Маршрут `blog` здесь — это *выделенный маршрут на основе соглашения*. Это означает, что он использует систему маршрутизации на основе соглашений, но предназначен для определенного действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-666">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="e43a5-667">Так как параметры `controller` и `action` отсутствуют в шаблоне маршрута, они могут иметь только значения по умолчанию, поэтому этот маршрут всегда будет сопоставляться с действием `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-667">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="e43a5-668">Маршруты в коллекции маршрутов упорядочены и обрабатываются в порядке добавления.</span><span class="sxs-lookup"><span data-stu-id="e43a5-668">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="e43a5-669">Поэтому в этом примере маршрут `blog` будет проверяться перед маршрутом `default`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-669">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="e43a5-670">*Выделенные традиционные маршруты* часто используют **перекрестные** параметры маршрута, такие как `{*article}` для записи оставшейся части пути URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-670">*Dedicated conventional routes* often use **catch-all** route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="e43a5-671">Из-за этого маршрут может оказаться слишком универсальным, то есть он будет соответствовать URL-адресам, которые должны сопоставляться с другими маршрутами.</span><span class="sxs-lookup"><span data-stu-id="e43a5-671">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="e43a5-672">Чтобы избежать этой проблемы, такие универсальные маршруты следует помещать в конце таблицы маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-672">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="e43a5-673">Откат</span><span class="sxs-lookup"><span data-stu-id="e43a5-673">Fallback</span></span>

<span data-ttu-id="e43a5-674">В ходе обработки запроса MVC проверяет, можно ли найти контроллер и действие в приложении с помощью предоставленных значений маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-674">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="e43a5-675">Если нет действия, соответствующего значениям, маршрут считается не сопоставленным, и проверяется следующий маршрут.</span><span class="sxs-lookup"><span data-stu-id="e43a5-675">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="e43a5-676">Этот процесс называется *откатом* и призван упростить обработку случаев, когда маршруты на основе соглашений перекрываются.</span><span class="sxs-lookup"><span data-stu-id="e43a5-676">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="e43a5-677">Разрешение неоднозначности действий</span><span class="sxs-lookup"><span data-stu-id="e43a5-677">Disambiguating actions</span></span>

<span data-ttu-id="e43a5-678">Если при маршрутизации найдены два соответствующих действия, платформа MVC должна устранить неоднозначность, выбрав наиболее подходящее из них, или создать исключение.</span><span class="sxs-lookup"><span data-stu-id="e43a5-678">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="e43a5-679">Пример:</span><span class="sxs-lookup"><span data-stu-id="e43a5-679">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="e43a5-680">Этот контроллер определяет два действия, которые соответствуют пути URL-адреса `/Products/Edit/17` и маршрутизируют данные `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-680">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="e43a5-681">Это типичный шаблон для контроллеров MVC, в котором метод `Edit(int)` отображает форму для изменения сведений о продукте, а метод `Edit(int, Product)` обрабатывает отправленную форму.</span><span class="sxs-lookup"><span data-stu-id="e43a5-681">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="e43a5-682">Чтобы это было возможным, платформа MVC должна выбрать `Edit(int, Product)` для HTTP-запроса `POST` и `Edit(int)` для любой другой HTTP-команды.</span><span class="sxs-lookup"><span data-stu-id="e43a5-682">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="e43a5-683">`HttpPostAttribute` (`[HttpPost]`) — это реализация интерфейса `IActionConstraint`, которая позволяет выбрать действие, только если HTTP-команда — `POST`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-683">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="e43a5-684">Наличие интерфейса `IActionConstraint` делает метод `Edit(int, Product)` более подходящим вариантом, чем `Edit(int)`, поэтому `Edit(int, Product)` будет проверяться первым.</span><span class="sxs-lookup"><span data-stu-id="e43a5-684">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="e43a5-685">Пользовательские реализации `IActionConstraint` потребуется создавать только в особых ситуациях, однако важно понимать роль таких атрибутов, как `HttpPostAttribute`, — аналогичные атрибуты определены для других HTTP-команд.</span><span class="sxs-lookup"><span data-stu-id="e43a5-685">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="e43a5-686">При маршрутизации на основе соглашений для действий часто используются одинаковые имена в рамках рабочего процесса `show form -> submit form`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-686">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="e43a5-687">Удобство такого шаблона станет очевидным после ознакомления с разделом [Сведения об интерфейсе IActionConstraint](#understanding-iactionconstraint).</span><span class="sxs-lookup"><span data-stu-id="e43a5-687">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="e43a5-688">Если найдено несколько совпадений и MVC не может определить наиболее подходящий маршрут, создается исключение `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-688">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="e43a5-689">Имена маршрутов</span><span class="sxs-lookup"><span data-stu-id="e43a5-689">Route names</span></span>

<span data-ttu-id="e43a5-690">Строки `"blog"` и `"default"` в следующих примерах представляют собой имена маршрутов:</span><span class="sxs-lookup"><span data-stu-id="e43a5-690">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="e43a5-691">Имя маршрута — это логическое имя, которое позволяет использовать именованный маршрут для формирования URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-691">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="e43a5-692">Имена значительно упрощают создание URL-адресов, если оно представляет сложность из-за порядка маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-692">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="e43a5-693">Имена маршрутов должны быть уникальными в пределах приложения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-693">Route names must be unique application-wide.</span></span>

<span data-ttu-id="e43a5-694">Имена маршрутов не влияют на сопоставление URL-адресов или обработку запросов; они служат только для формирования URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-694">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="e43a5-695">В статье [Маршрутизация](xref:fundamentals/routing) приводятся более подробные сведения о формировании URL-адресов, в том числе во вспомогательных объектах MVC.</span><span class="sxs-lookup"><span data-stu-id="e43a5-695">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="e43a5-696">Маршрутизация с помощью атрибутов</span><span class="sxs-lookup"><span data-stu-id="e43a5-696">Attribute routing</span></span>

<span data-ttu-id="e43a5-697">При маршрутизации с помощью атрибутов используется набор атрибутов для сопоставления действий непосредственно с шаблонами маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-697">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="e43a5-698">В приведенном ниже примере в методе `app.UseMvc();` используется `Configure`, и маршрут не передается.</span><span class="sxs-lookup"><span data-stu-id="e43a5-698">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="e43a5-699">`HomeController` будет соответствовать набору URL-адресов, аналогичных тем, которым соответствует маршрут по умолчанию `{controller=Home}/{action=Index}/{id?}`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-699">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

<span data-ttu-id="e43a5-700">Действие `HomeController.Index()` будет выполняться для любого из путей URL-адресов `/`, `/Home` или `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-700">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="e43a5-701">В этом примере показано ключевое различие между маршрутизацией с помощью атрибутов и маршрутизацией на основе соглашений.</span><span class="sxs-lookup"><span data-stu-id="e43a5-701">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="e43a5-702">При использовании маршрутизации с помощью атрибутов для указания маршрута требуется больше входных данных; маршрут по умолчанию на основе соглашения более лаконичен.</span><span class="sxs-lookup"><span data-stu-id="e43a5-702">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="e43a5-703">Однако маршрутизация с помощью атрибутов обеспечивает более точный контроль над тем, какие шаблоны маршрутов применяются к каждому действию, и требует такого контроля.</span><span class="sxs-lookup"><span data-stu-id="e43a5-703">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="e43a5-704">В случае с маршрутизацией с помощью атрибутов имя контроллера и имена действий **не** имеют значения при выборе действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-704">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="e43a5-705">В этом примере сопоставление будет производиться с теми же URL-адресами, что и в предыдущем.</span><span class="sxs-lookup"><span data-stu-id="e43a5-705">This example will match the same URLs as the previous example.</span></span>

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> <span data-ttu-id="e43a5-706">Приведенные выше шаблоны маршрутов не определяют параметры маршрутов для `action`, `area` и `controller`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-706">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="e43a5-707">По сути, эти параметры маршрутов не разрешены в маршрутах на основе атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-707">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="e43a5-708">Так как шаблон маршрута уже связан с действием, не имеет смысла анализировать имя действия в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="e43a5-708">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="e43a5-709">Маршрутизация с помощью атрибутов Http[Verb]</span><span class="sxs-lookup"><span data-stu-id="e43a5-709">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="e43a5-710">При маршрутизации с помощью атрибутов также могут использоваться атрибуты `Http[Verb]`, такие как `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-710">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="e43a5-711">Все эти атрибуты могут принимать шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-711">All of these attributes can accept a route template.</span></span> <span data-ttu-id="e43a5-712">В этом примере показаны два действия, которые совпадают с одним шаблоном маршрута:</span><span class="sxs-lookup"><span data-stu-id="e43a5-712">This example shows two actions that match the same route template:</span></span>

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

<span data-ttu-id="e43a5-713">Для такого пути URL-адреса, как `/products`, действие `ProductsApi.ListProducts` будет выполнено, если HTTP-командой является `GET`, а действие `ProductsApi.CreateProduct` будет выполнено, если HTTP-командой является `POST`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-713">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="e43a5-714">При маршрутизации с помощью атрибутов URL-адрес сначала сопоставляется с набором шаблоном маршрутов, определяемых атрибутами маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-714">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="e43a5-715">После того как найден соответствующий шаблон маршрута, применяются ограничения `IActionConstraint` для определения действий, которые могут быть выполнены.</span><span class="sxs-lookup"><span data-stu-id="e43a5-715">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="e43a5-716">При разработке REST API редко приходится использовать `[Route(...)]` для метода действия, так как действие принимает все методы HTTP.</span><span class="sxs-lookup"><span data-stu-id="e43a5-716">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method as the action will accept all HTTP methods.</span></span> <span data-ttu-id="e43a5-717">Предпочтительнее применять более конкретные атрибуты `Http*Verb*Attributes`, чтобы точно определить поддерживаемые API возможности.</span><span class="sxs-lookup"><span data-stu-id="e43a5-717">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="e43a5-718">Клиенты интерфейсов REST API должны знать, какие пути и HTTP-команды сопоставляются с определенными логическими операциями.</span><span class="sxs-lookup"><span data-stu-id="e43a5-718">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="e43a5-719">Так как маршрут на основе атрибутов применяется к определенному действию, можно легко сделать параметры обязательными в рамках определения шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-719">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="e43a5-720">В этом примере параметр `id` является обязательным в пути URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-720">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="e43a5-721">Действие `ProductsApi.GetProduct(int)` будет выполнено для такого пути URL-адреса, как `/products/3`, но не для такого пути URL-адреса, как `/products`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-721">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="e43a5-722">Полное описание шаблонов маршрутов и связанных параметров см. в статье [Маршрутизация](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="e43a5-722">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="e43a5-723">Имя маршрута</span><span class="sxs-lookup"><span data-stu-id="e43a5-723">Route Name</span></span>

<span data-ttu-id="e43a5-724">В следующем коде определяется *имя маршрута*`Products_List`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-724">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="e43a5-725">Имена маршрутов могут использоваться для формирования URL-адреса на основе определенного маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-725">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="e43a5-726">Они не влияют на то, как производится сопоставление URL-адресов при маршрутизации, и служат только для формирования URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-726">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="e43a5-727">Имена маршрутов должны быть уникальными в пределах приложения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-727">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="e43a5-728">Сравните это поведение с *маршрутом по умолчанию* на основе соглашения, в котором параметр `id` определяется как необязательный (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="e43a5-728">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="e43a5-729">Такая возможность точно указывать интерфейсы API имеет преимущества. Например, она позволяет направлять `/products` и `/products/5` в разные действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-729">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="e43a5-730">Объединение маршрутов</span><span class="sxs-lookup"><span data-stu-id="e43a5-730">Combining routes</span></span>

<span data-ttu-id="e43a5-731">Чтобы избежать лишних повторов при маршрутизации с помощью атрибутов, атрибуты маршрута для контроллера объединяются с атрибутами маршрута для отдельных действий.</span><span class="sxs-lookup"><span data-stu-id="e43a5-731">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="e43a5-732">Все шаблоны маршрутов, определенные в контроллере, добавляются перед шаблонами маршрутов для действий.</span><span class="sxs-lookup"><span data-stu-id="e43a5-732">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="e43a5-733">В результате добавления атрибута маршрута для контроллера **все** его действия будут использовать маршрутизацию с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-733">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="e43a5-734">В этом примере путь URL-адреса `/products` может соответствовать `ProductsApi.ListProducts`, а путь URL-адреса `/products/5` — `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-734">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="e43a5-735">Оба эти действия соответствуют только HTTP-запросу `GET`, так как они помечены атрибутом `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-735">Both of these actions only match HTTP `GET` because they're marked with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="e43a5-736">Шаблоны маршрутов, применяемые к действию, которое начинается с символа `/` или `~/`, не объединяются с шаблонами маршрутов, применяемыми к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="e43a5-736">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="e43a5-737">Этот пример соответствует такому же набору путей URL-адресов, что и *маршрут по умолчанию*.</span><span class="sxs-lookup"><span data-stu-id="e43a5-737">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a><span data-ttu-id="e43a5-738">Упорядочение маршрутов на основе атрибутов</span><span class="sxs-lookup"><span data-stu-id="e43a5-738">Ordering attribute routes</span></span>

<span data-ttu-id="e43a5-739">В отличие от обычных маршрутов, которые выполняются в определенном порядке, маршрутизация атрибутов создает дерево и сопоставляет все маршруты одновременно.</span><span class="sxs-lookup"><span data-stu-id="e43a5-739">In contrast to conventional routes, which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="e43a5-740">Это равносильно тому, как если бы записи маршрутов находились в идеальном порядке: наиболее конкретные маршруты имеют возможность выполнения перед более общими.</span><span class="sxs-lookup"><span data-stu-id="e43a5-740">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="e43a5-741">Например, такой маршрут, как `blog/search/{topic}`, является более конкретным по сравнению с `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-741">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="e43a5-742">С логической точки зрения, маршрут `blog/search/{topic}` по умолчанию выполняется первым, так как это единственная разумная очередность.</span><span class="sxs-lookup"><span data-stu-id="e43a5-742">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="e43a5-743">При использовании маршрутизации на основе соглашений разработчик отвечает за расположение маршрутов в нужном порядке.</span><span class="sxs-lookup"><span data-stu-id="e43a5-743">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="e43a5-744">При маршрутизации с помощью атрибутов порядок может настраиваться с помощью свойства `Order` всех атрибутов маршрутов, предоставляемых платформой.</span><span class="sxs-lookup"><span data-stu-id="e43a5-744">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="e43a5-745">Маршруты обрабатываются в порядке возрастания значения свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-745">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="e43a5-746">Порядок по умолчанию — `0`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-746">The default order is `0`.</span></span> <span data-ttu-id="e43a5-747">Маршрут, для которого задано значение `Order = -1`, будет выполняться перед маршрутами, для которых порядок не задан.</span><span class="sxs-lookup"><span data-stu-id="e43a5-747">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="e43a5-748">Маршрут, для которого задано значение `Order = 1`, будет выполняться после маршрутов с порядком по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-748">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="e43a5-749">Старайтесь не использовать свойство `Order`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-749">Avoid depending on `Order`.</span></span> <span data-ttu-id="e43a5-750">Если для правильной маршрутизации в пространстве URL-адресов требуются явно заданные значения порядка, скорее всего, это будет вызывать путаницу и в среде клиентов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-750">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="e43a5-751">Как правило, при маршрутизации с помощью атрибутов правильный маршрут выбирается посредством сопоставления URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-751">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="e43a5-752">Если порядок по умолчанию для формирования URL-адресов не работает, использовать имя маршрута в качестве переопределения, как правило, проще, чем применять свойство `Order`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-752">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="e43a5-753">Средства маршрутизации в Razor Pages и контроллере MVC имеют общую реализацию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-753">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="e43a5-754">Сведения о порядке маршрутизации в Razor Pages см. в статье [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order) (Маршрутизация и соглашения в приложении Razor Pages: порядок маршрутизации).</span><span class="sxs-lookup"><span data-stu-id="e43a5-754">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="e43a5-755">Замена токенов в шаблонах маршрутов ([controller], [action], [area])</span><span class="sxs-lookup"><span data-stu-id="e43a5-755">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="e43a5-756">Для удобства маршрута на основе атрибутов поддерживают *замену токенов* путем заключения токена в квадратные скобки (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="e43a5-756">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="e43a5-757">Токены `[action]`, `[area]` и `[controller]` заменяются значениями имени действия, имени области и имени контроллера из действия, в котором определен маршрут.</span><span class="sxs-lookup"><span data-stu-id="e43a5-757">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="e43a5-758">В следующем примере действия могут соответствовать путям URL-адресов, как описано в комментариях:</span><span class="sxs-lookup"><span data-stu-id="e43a5-758">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="e43a5-759">Замена токенов происходит на последнем этапе создания маршрутов на основе атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-759">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="e43a5-760">Приведенный выше пример будет работать так же, как следующий код:</span><span class="sxs-lookup"><span data-stu-id="e43a5-760">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="e43a5-761">Маршруты на основе атрибутов могут также сочетаться с наследованием.</span><span class="sxs-lookup"><span data-stu-id="e43a5-761">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="e43a5-762">Эта возможность особенно эффективна при использовании вместе с заменой токенов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-762">This is particularly powerful combined with token replacement.</span></span>

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPut("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

<span data-ttu-id="e43a5-763">Замена токенов также применяется к именам маршрутов, определенным в маршрутах на основе атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-763">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="e43a5-764">`[Route("[controller]/[action]", Name="[controller]_[action]")]` создает уникальное имя маршрута для каждого действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-764">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="e43a5-765">Для сопоставления с литеральным разделителем замены токенов `[` или `]` его следует экранировать путем повтора символа (`[[` или `]]`).</span><span class="sxs-lookup"><span data-stu-id="e43a5-765">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="e43a5-766">Использование преобразователя параметров для настройки замены токенов</span><span class="sxs-lookup"><span data-stu-id="e43a5-766">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="e43a5-767">Замену токенов можно настроить, используя преобразователь параметров.</span><span class="sxs-lookup"><span data-stu-id="e43a5-767">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="e43a5-768">Преобразователь параметров реализует `IOutboundParameterTransformer` и преобразует значения параметров.</span><span class="sxs-lookup"><span data-stu-id="e43a5-768">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="e43a5-769">Например, пользовательский преобразователь параметра `SlugifyParameterTransformer` изменяет значение маршрута `SubscriptionManagement` на `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-769">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="e43a5-770">`RouteTokenTransformerConvention` является соглашением для модели приложения, которое:</span><span class="sxs-lookup"><span data-stu-id="e43a5-770">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="e43a5-771">Применяет преобразователь параметров ко всем маршрутам атрибута в приложении.</span><span class="sxs-lookup"><span data-stu-id="e43a5-771">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="e43a5-772">Настраивает значения токена маршрут атрибута при замене.</span><span class="sxs-lookup"><span data-stu-id="e43a5-772">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="e43a5-773">`RouteTokenTransformerConvention` регистрируется в качестве параметра в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-773">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options =>
    {
        options.Conventions.Add(new RouteTokenTransformerConvention(
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


::: moniker range="< aspnetcore-3.0"
<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="e43a5-774">Несколько маршрутов</span><span class="sxs-lookup"><span data-stu-id="e43a5-774">Multiple Routes</span></span>

<span data-ttu-id="e43a5-775">Маршрутизация с помощью атрибутов поддерживает определение нескольких маршрутов к одному и тому же действию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-775">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="e43a5-776">Наиболее распространенный случай использования этой возможности — имитация поведения *маршрута по умолчанию на основе соглашения*, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e43a5-776">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="e43a5-777">Добавление нескольких атрибутов маршрута для контроллера означает, что каждый из них будет объединяться с каждым из атрибутов маршрута, определенных для методов действий.</span><span class="sxs-lookup"><span data-stu-id="e43a5-777">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

<span data-ttu-id="e43a5-778">Если несколько атрибутов маршрута (реализующих интерфейс `IActionConstraint`) добавлены для действия, каждое ограничение действия объединяется с шаблоном маршрута из атрибута, в котором оно определено.</span><span class="sxs-lookup"><span data-stu-id="e43a5-778">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> <span data-ttu-id="e43a5-779">Хотя использование нескольких маршрутов к действиям может показаться очень эффективной возможностью, лучше, чтобы пространство URL-адресов приложения оставалось простым и четко организованным.</span><span class="sxs-lookup"><span data-stu-id="e43a5-779">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="e43a5-780">Используйте несколько маршрутов к действиям только там, где это необходимо, например для поддержки существующих клиентов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-780">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="e43a5-781">Задание необязательных параметров, значений по умолчанию и ограничений для маршрутов на основе атрибутов</span><span class="sxs-lookup"><span data-stu-id="e43a5-781">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="e43a5-782">Маршруты на основе атрибутов поддерживают тот же синтаксис для указания необязательных параметров, значений по умолчанию и ограничений, что и маршруты на основе соглашений.</span><span class="sxs-lookup"><span data-stu-id="e43a5-782">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="e43a5-783">Подробное описание синтаксиса шаблона маршрута см. в разделе [Справочник по шаблону маршрута](xref:fundamentals/routing#route-template-reference).</span><span class="sxs-lookup"><span data-stu-id="e43a5-783">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="e43a5-784">Пользовательские атрибуты маршрута с использованием `IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="e43a5-784">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="e43a5-785">Все атрибуты маршрутов, предоставляемые платформой ( `[Route(...)]`, `[HttpGet(...)]` и т. д.), реализуют интерфейс `IRouteTemplateProvider`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-785">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="e43a5-786">MVC ищет атрибуты в классах контроллеров и методах действий при запуске приложения и использует те из них, которые реализуют интерфейс `IRouteTemplateProvider`, для формирования начального набора маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-786">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="e43a5-787">Вы можете реализовать интерфейс `IRouteTemplateProvider` для определения собственных атрибутов маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-787">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="e43a5-788">Каждая реализация `IRouteTemplateProvider` позволяет определить один маршрут с пользовательским шаблоном маршрута, порядком и именем.</span><span class="sxs-lookup"><span data-stu-id="e43a5-788">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="e43a5-789">Атрибут в приведенном выше примере автоматически присваивает шаблону `Template` значение `"api/[controller]"` при применении `[MyApiController]`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-789">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="e43a5-790">Настройка маршрутов на основе атрибутов с помощью модели приложения</span><span class="sxs-lookup"><span data-stu-id="e43a5-790">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="e43a5-791">*Модель приложения* — это объектная модель, которая создается при запуске со всеми метаданными, используемыми платформой MVC для маршрутизации и выполнения действий.</span><span class="sxs-lookup"><span data-stu-id="e43a5-791">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="e43a5-792">*Модель приложения* включает в себя все данные, собранные из атрибутов маршрутов (посредством интерфейса `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="e43a5-792">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="e43a5-793">Вы можете создать *соглашения*, чтобы изменить модель приложения при запуске с целью настройки маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-793">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="e43a5-794">В этом разделе приводится простой пример настройки маршрутизации с помощью модели приложения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-794">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/samples/2.x/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="e43a5-795">Смешанная маршрутизация с помощью атрибутов и на основе соглашений</span><span class="sxs-lookup"><span data-stu-id="e43a5-795">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="e43a5-796">В приложениях MVC маршрутизация с помощью атрибутов и маршрутизация на основе соглашений могут использоваться вместе.</span><span class="sxs-lookup"><span data-stu-id="e43a5-796">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="e43a5-797">Маршруты на основе соглашений часто применяются для контроллеров, предоставляющих HTML-страницы для браузеров, а маршруты на основе атрибутов — для контроллеров, предоставляющих интерфейсы REST API.</span><span class="sxs-lookup"><span data-stu-id="e43a5-797">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="e43a5-798">Действия маршрутизируются либо на основе соглашений, либо с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-798">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="e43a5-799">При добавлении маршрута к контроллеру или действию они становятся маршрутизируемыми с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-799">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="e43a5-800">Действия, определяющие маршруты на основе атрибутов, недоступны по маршрутам на основе соглашений, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="e43a5-800">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="e43a5-801">**Любой** атрибут маршрута контроллера делает все действия в атрибуте контроллера маршрутизируемыми.</span><span class="sxs-lookup"><span data-stu-id="e43a5-801">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="e43a5-802">Два типа систем маршрутизации отличает процесс, выполняемый после нахождения URL-адреса, соответствующего шаблону маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-802">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="e43a5-803">При маршрутизации на основе соглашений значения маршрута из соответствия используются для выбора действия и контроллера из таблицы подстановки, содержащей все действия с маршрутизацией на основе соглашений.</span><span class="sxs-lookup"><span data-stu-id="e43a5-803">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="e43a5-804">При маршрутизации с помощью атрибутов каждый шаблон уже связан с действием, и дальнейшая подстановка не требуется.</span><span class="sxs-lookup"><span data-stu-id="e43a5-804">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="e43a5-805">Сложные сегменты</span><span class="sxs-lookup"><span data-stu-id="e43a5-805">Complex segments</span></span>

<span data-ttu-id="e43a5-806">Сложные сегменты (например, `[Route("/dog{token}cat")]`) обрабатываются путем "нежадного" сопоставления литералов справа налево.</span><span class="sxs-lookup"><span data-stu-id="e43a5-806">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="e43a5-807">Описание см. в [исходном коде](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296).</span><span class="sxs-lookup"><span data-stu-id="e43a5-807">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="e43a5-808">Дополнительные сведения см. в [этой проблеме](https://github.com/dotnet/AspNetCore.Docs/issues/8197).</span><span class="sxs-lookup"><span data-stu-id="e43a5-808">For more information, see [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="e43a5-809">Формирование URL-адреса</span><span class="sxs-lookup"><span data-stu-id="e43a5-809">URL Generation</span></span>

<span data-ttu-id="e43a5-810">Приложения MVC могут использовать функции формирования URL-адреса, предоставляемые системой маршрутизации, для создания URL-ссылок на действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-810">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="e43a5-811">Формирование URL-адресов устраняет необходимость в их жестком задании, что делает код надежнее и проще в обслуживании.</span><span class="sxs-lookup"><span data-stu-id="e43a5-811">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="e43a5-812">В этом разделе рассматриваются функции формирования URL-адреса, предоставляемые платформой MVC, и описываются лишь базовые принципы их работы.</span><span class="sxs-lookup"><span data-stu-id="e43a5-812">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="e43a5-813">Подробное описание формирования URL-адреса см. в статье [Маршрутизация](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="e43a5-813">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="e43a5-814">Интерфейс `IUrlHelper` — это базовый компонент инфраструктуры, обеспечивающий взаимодействие между MVC и системой маршрутизации для формирования URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-814">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="e43a5-815">Доступ к экземпляру `IUrlHelper` в контроллерах, представлениях и компонентах представлений можно получить посредством свойства `Url`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-815">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="e43a5-816">В этом примере интерфейс `IUrlHelper` используется посредством свойства `Controller.Url` для формирования URL-адреса другого действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-816">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="e43a5-817">Если в приложении применяется маршрут по умолчанию на основе соглашения, значением переменной `url` будет строка с путем URL-адреса `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-817">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="e43a5-818">Этот путь URL-адреса создается системой маршрутизации путем объединения значений маршрута из текущего запроса (значения окружения) со значениями, переданными в `Url.Action`, и подстановки этих значений в шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-818">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="e43a5-819">Значение каждого параметра маршрута в шаблоне маршрута заменяется соответствующими именами со значениями и значениями окружения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-819">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="e43a5-820">Параметр маршрута, у которого нет значения, может принимать значение по умолчанию, если оно имеется, или пропускаться, если он является необязательным (как в случае с параметром `id` в этом примере).</span><span class="sxs-lookup"><span data-stu-id="e43a5-820">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="e43a5-821">Сформировать URL-адрес не удастся, если у любого из обязательных параметров маршрута не будет соответствующего значения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-821">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="e43a5-822">Если для маршрута не удалось сформировать URL-адрес, проверяется следующий маршрут, пока не будут проверены все маршруты или не будет найдено соответствие.</span><span class="sxs-lookup"><span data-stu-id="e43a5-822">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="e43a5-823">В примере `Url.Action` выше предполагается использование маршрутизации на основе соглашений, однако формирование URL-адреса происходит аналогичным образом и в случае с маршрутизацией с помощью атрибутов, хотя принципы иные.</span><span class="sxs-lookup"><span data-stu-id="e43a5-823">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="e43a5-824">При применении маршрутизации на основе соглашений шаблон расширяется с помощью значений маршрута, и значения маршрута для `controller` и `action` обычно находятся в этом шаблоне. Это возможно по той причине, что URL-адреса, сопоставленные системой маршрутизации, следуют *соглашению*.</span><span class="sxs-lookup"><span data-stu-id="e43a5-824">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="e43a5-825">При маршрутизации с помощью атрибутов значения маршрута для `controller` и `action` не могут находиться в шаблоне. Вместо этого они применяются для поиска нужного шаблона.</span><span class="sxs-lookup"><span data-stu-id="e43a5-825">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="e43a5-826">В этом примере используется маршрутизация с помощью атрибутов:</span><span class="sxs-lookup"><span data-stu-id="e43a5-826">This example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/2.x/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="e43a5-827">MVC создает таблицу подстановки, содержащую все действия с маршрутизацией с помощью атрибутов, и сопоставляет значения `controller` и `action` для выбора шаблона маршрута, с помощью которого должен формироваться URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="e43a5-827">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="e43a5-828">В приведенном выше примере создается URL-адрес `custom/url/to/destination`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-828">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="e43a5-829">Формирование URL-адресов по имени действия</span><span class="sxs-lookup"><span data-stu-id="e43a5-829">Generating URLs by action name</span></span>

<span data-ttu-id="e43a5-830">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="e43a5-830">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="e43a5-831">`Action`) и все связанные перегрузки предполагают необходимость указания целевого объекта путем задания имени контроллера и имени действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-831">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="e43a5-832">При использовании метода `Url.Action` текущие значения маршрута для `controller` и `action` уже определены: значения `controller` и `action` включены как в *значения окружения*, так и в **обычные** *значения*.</span><span class="sxs-lookup"><span data-stu-id="e43a5-832">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="e43a5-833">Метод `Url.Action` всегда использует текущие значения `action` и `controller` и формирует путь URL-адреса, ведущий к текущему действию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-833">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="e43a5-834">При формировании URL-адреса система маршрутизации пытается использовать значения окружения для заполнения сведений, которые вы не предоставили.</span><span class="sxs-lookup"><span data-stu-id="e43a5-834">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="e43a5-835">При использовании такого маршрута, как `{a}/{b}/{c}/{d}`, и значений окружения `{ a = Alice, b = Bob, c = Carol, d = David }` система маршрутизации имеет достаточно информации для формирования URL-адреса без дополнительных значений, так как все параметры маршрута имеют значения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-835">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="e43a5-836">Если добавить значение `{ d = Donovan }`, значение `{ d = David }` не будет учитываться, и будет сформирован путь URL-адреса `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-836">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="e43a5-837">Пути URL-адресов являются иерархическими.</span><span class="sxs-lookup"><span data-stu-id="e43a5-837">URL paths are hierarchical.</span></span> <span data-ttu-id="e43a5-838">Если в приведенном выше примере добавить значение `{ c = Cheryl }`, пропущены будут оба значения `{ c = Carol, d = David }`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-838">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="e43a5-839">В этом случае параметр `d` больше не имеет значения и сформировать URL-адрес не удастся.</span><span class="sxs-lookup"><span data-stu-id="e43a5-839">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="e43a5-840">Потребуется указать нужные значения для параметров `c` и `d`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-840">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="e43a5-841">Эта проблема может возникнуть с маршрутом по умолчанию (`{controller}/{action}/{id?}`), но на практике она встречается редко, так как `Url.Action` всегда явным образом задает значения `controller` и `action`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-841">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="e43a5-842">Более длинные перегрузки `Url.Action` также принимают дополнительный объект *значений маршрута* для предоставления значений параметров маршрута, отличных от `controller` и `action`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-842">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="e43a5-843">Чаще всего он применяется с параметром `id`, например `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-843">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="e43a5-844">В соответствии с соглашением объект *значений маршрута* обычно имеет анонимный тип, но это может быть также экземпляр `IDictionary<>` или *обычный объект .NET*.</span><span class="sxs-lookup"><span data-stu-id="e43a5-844">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="e43a5-845">Остальные значения маршрута, которые не соответствуют параметрам маршрута, помещаются в строку запроса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-845">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="e43a5-846">Чтобы создать абсолютный URL-адрес, используйте перегрузку, принимающую `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="e43a5-846">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="e43a5-847">Формирование URL-адресов по маршруту</span><span class="sxs-lookup"><span data-stu-id="e43a5-847">Generating URLs by route</span></span>

<span data-ttu-id="e43a5-848">В приведенном выше коде демонстрировалось формирование URL-адреса путем передачи имен контроллера и действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-848">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="e43a5-849">Интерфейс `IUrlHelper` также предоставляет семейство методов `Url.RouteUrl`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-849">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="e43a5-850">Эти методы похожи на `Url.Action`, но они не копируют текущие значения `action` и `controller` в значения маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-850">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="e43a5-851">Наиболее распространенный способ их применения — указание имени определенного маршрута, который должен использоваться для формирования URL-адреса, как правило, *без* указания имени контроллера или действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-851">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="e43a5-852">Формирование URL-адресов в HTML</span><span class="sxs-lookup"><span data-stu-id="e43a5-852">Generating URLs in HTML</span></span>

<span data-ttu-id="e43a5-853">Интерфейс `IHtmlHelper` предоставляет методы `HtmlHelper``Html.BeginForm` и `Html.ActionLink` для создания элементов `<form>` и `<a>` соответственно.</span><span class="sxs-lookup"><span data-stu-id="e43a5-853">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="e43a5-854">Эти методы используют метод `Url.Action` для формирования URL-адреса и принимают одинаковые аргументы.</span><span class="sxs-lookup"><span data-stu-id="e43a5-854">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="e43a5-855">Эквивалентами методов `Url.RouteUrl` для `HtmlHelper` являются методы `Html.BeginRouteForm` и `Html.RouteLink`, которые имеют схожие функции.</span><span class="sxs-lookup"><span data-stu-id="e43a5-855">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="e43a5-856">Для формирования URL-адресов используются вспомогательные функции тегов `form` и `<a>`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-856">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="e43a5-857">Обе они реализуются с помощью интерфейса `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-857">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="e43a5-858">Дополнительные сведения см. в статье [Работа с формами](../views/working-with-forms.md).</span><span class="sxs-lookup"><span data-stu-id="e43a5-858">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="e43a5-859">Внутри представлений интерфейс `IUrlHelper` доступен посредством свойства `Url` для особых случаев формирования URL-адресов, помимо описанных выше.</span><span class="sxs-lookup"><span data-stu-id="e43a5-859">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="e43a5-860">Формирование URL-адресов в результатах действий</span><span class="sxs-lookup"><span data-stu-id="e43a5-860">Generating URLS in Action Results</span></span>

<span data-ttu-id="e43a5-861">В предыдущих примерах было продемонстрировано использование интерфейса `IUrlHelper` в контроллере, хотя чаще всего URL-адреса формируются в контроллерах в рамках результата действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-861">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="e43a5-862">Базовые классы `ControllerBase` и `Controller` предоставляют удобные методы для результатов действий, ссылающихся на другое действие.</span><span class="sxs-lookup"><span data-stu-id="e43a5-862">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="e43a5-863">Одним из типичных способов их применения является перенаправление после принятия входных данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="e43a5-863">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public IActionResult Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
    return View(customer);
}
```

<span data-ttu-id="e43a5-864">Фабричные методы результатов действий по своей структуре похожи на методы интерфейса `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-864">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="e43a5-865">Выделенные маршруты на основе соглашений</span><span class="sxs-lookup"><span data-stu-id="e43a5-865">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="e43a5-866">При маршрутизации на основе соглашений может использоваться особый тип определения маршрута — *выделенный маршрут на основе соглашения*.</span><span class="sxs-lookup"><span data-stu-id="e43a5-866">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="e43a5-867">В приведенном ниже примере маршрут с именем `blog` является выделенным маршрутом на основе соглашения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-867">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="e43a5-868">При использовании таких определений маршрутов метод `Url.Action("Index", "Home")` создаст путь URL-адреса `/` с маршрутом `default`. В чем же причина?</span><span class="sxs-lookup"><span data-stu-id="e43a5-868">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="e43a5-869">Можно было бы предположить, что значений маршрута `{ controller = Home, action = Index }` было бы достаточно для формирования URL-адреса с помощью `blog` и результатом было бы `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-869">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="e43a5-870">В случае с выделенными маршрутами на основе соглашений используется особое поведение значений по умолчанию, для которых нет соответствующих параметров маршрута. Благодаря ему маршрут не может быть слишком универсальным при формировании URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-870">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="e43a5-871">В этом случае значения по умолчанию — `{ controller = Blog, action = Article }`, но параметров маршрута `controller` и `action` нет.</span><span class="sxs-lookup"><span data-stu-id="e43a5-871">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="e43a5-872">Когда система маршрутизации производит формирование URL-адреса, предоставленные значения должны соответствовать значениям по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e43a5-872">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="e43a5-873">Сформировать URL-адрес с помощью `blog` не удастся, так как значения `{ controller = Home, action = Index }` не соответствуют `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-873">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="e43a5-874">После этого система маршрутизации выполнит попытку использовать маршрут `default`, которая завершится успешно.</span><span class="sxs-lookup"><span data-stu-id="e43a5-874">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="e43a5-875">Зоны</span><span class="sxs-lookup"><span data-stu-id="e43a5-875">Areas</span></span>

<span data-ttu-id="e43a5-876">[Области](areas.md) — это возможность MVC, которая служит для объединения связанных функций в группу в виде отдельного пространства имен маршрутизации (для действий контроллеров) и структуры папок (для представлений).</span><span class="sxs-lookup"><span data-stu-id="e43a5-876">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="e43a5-877">Благодаря областям приложение может иметь несколько контроллеров с одинаковыми именами, если у них будут разные *области*.</span><span class="sxs-lookup"><span data-stu-id="e43a5-877">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="e43a5-878">При использовании областей создается иерархия в целях маршрутизации. Для этого к `area` и `controller` добавляется еще один параметр маршрута, `action`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-878">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="e43a5-879">В этом разделе рассматривается взаимодействие системы маршрутизации с областями. Подробные сведения об использовании областей с представлениями см. в статье [Области](areas.md).</span><span class="sxs-lookup"><span data-stu-id="e43a5-879">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="e43a5-880">В следующем примере в MVC настраивается использование маршрута на основе соглашения по умолчанию и *маршрута области* для области с именем `Blog`:</span><span class="sxs-lookup"><span data-stu-id="e43a5-880">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="e43a5-881">Результатом сопоставления первого маршрута с таким путем URL-адреса, как `/Manage/Users/AddUser`, будут значения маршрута `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-881">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="e43a5-882">Значение маршрута `area` получается на основе значения по умолчанию для параметра `area`. По сути, маршрут, создаваемый методом `MapAreaRoute`, эквивалентен следующему:</span><span class="sxs-lookup"><span data-stu-id="e43a5-882">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="e43a5-883">Метод `MapAreaRoute` создает маршрут с помощью значения по умолчанию и ограничения для `area` с использованием предоставленного имени маршрута (в данном случае `Blog`).</span><span class="sxs-lookup"><span data-stu-id="e43a5-883">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="e43a5-884">Значение по умолчанию гарантирует, что маршрут всегда создает значение `{ area = Blog, ... }`. Ограничение требует значения `{ area = Blog, ... }` для формирования URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-884">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="e43a5-885">При маршрутизации на основе соглашений учитывается порядок.</span><span class="sxs-lookup"><span data-stu-id="e43a5-885">Conventional routing is order-dependent.</span></span> <span data-ttu-id="e43a5-886">Как правило, маршруты с областями должны находиться раньше в таблице маршрутов, так как они более точные, чем маршруты без областей.</span><span class="sxs-lookup"><span data-stu-id="e43a5-886">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="e43a5-887">В предыдущем примере значения маршрута будут соответствовать следующему действию:</span><span class="sxs-lookup"><span data-stu-id="e43a5-887">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="e43a5-888">`AreaAttribute` обозначает контроллер в рамках области. В таком случае говорят, что контроллер находится в области `Blog`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-888">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="e43a5-889">Контроллеры без атрибута `[Area]` не входят ни в одну область и **не** будут соответствовать маршруту, если система маршрутизации предоставляет значение маршрута `area`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-889">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="e43a5-890">В приведенном ниже примере только первый контроллер может соответствовать значениям маршрута `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-890">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="e43a5-891">Пространство имен каждого контроллера приведено здесь для полноты. В противном случае между контроллерами возник бы конфликт именования, и произошла бы ошибка компилятора.</span><span class="sxs-lookup"><span data-stu-id="e43a5-891">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="e43a5-892">Имена пространств классов не влияют на маршрутизацию в MVC.</span><span class="sxs-lookup"><span data-stu-id="e43a5-892">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="e43a5-893">Первые два контроллера входят в области и будут соответствовать запросу, только если соответствующее имя области предоставлено значением маршрута `area`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-893">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="e43a5-894">Третий контроллер не входит ни в одну область и может соответствовать запросу, только если значение `area` не предоставлено системой маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e43a5-894">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="e43a5-895">В плане сопоставления *отсутствующих значений* отсутствие значения `area` равносильно тому, как если значением `area` было бы NULL или пустая строка.</span><span class="sxs-lookup"><span data-stu-id="e43a5-895">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="e43a5-896">При выполнении действия внутри области значение маршрута для `area` будет доступно как *значение окружения*, которое система маршрутизации использует для формирования URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e43a5-896">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="e43a5-897">Это означает, что по умолчанию области являются *фиксированными* при формировании URL-адресов, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="e43a5-897">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>
[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="e43a5-898">Основные сведения об интерфейсе IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="e43a5-898">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="e43a5-899">В этом разделе описываются внутренние принципы работы платформы и то, как MVC выбирает действие, которое необходимо выполнить.</span><span class="sxs-lookup"><span data-stu-id="e43a5-899">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="e43a5-900">Типичному приложению пользовательская реализация `IActionConstraint` не требуется.</span><span class="sxs-lookup"><span data-stu-id="e43a5-900">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="e43a5-901">Скорее всего, вы уже пользовались интерфейсом `IActionConstraint`, даже если не знакомы с ним.</span><span class="sxs-lookup"><span data-stu-id="e43a5-901">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="e43a5-902">Атрибут `[HttpGet]` и сходные атрибуты `[Http-VERB]` реализуют интерфейс `IActionConstraint`, чтобы ограничить выполнение метода действия.</span><span class="sxs-lookup"><span data-stu-id="e43a5-902">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="e43a5-903">В случае с маршрутом по умолчанию на основе соглашения путь URL-адреса `/Products/Edit` даст значения `{ controller = Products, action = Edit }`, которые будут соответствовать **обоим** приведенным здесь действиям.</span><span class="sxs-lookup"><span data-stu-id="e43a5-903">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="e43a5-904">В терминологии `IActionConstraint` можно сказать, что оба действия считаются кандидатами, так как они оба соответствуют данным маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-904">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="e43a5-905">Когда метод `HttpGetAttribute` выполняется, он сообщает, что *Edit()* является соответствием для запроса *GET*, но не является соответствием для каких-либо иных HTTP-команд.</span><span class="sxs-lookup"><span data-stu-id="e43a5-905">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="e43a5-906">Для действия `Edit(...)` ограничения не определены, поэтому оно соответствует любой HTTP-команде.</span><span class="sxs-lookup"><span data-stu-id="e43a5-906">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="e43a5-907">Поэтому если рассматривать запрос `POST`, соответствием будет только `Edit(...)`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-907">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="e43a5-908">Однако в случае с запросом `GET` соответствовать могут оба действия, хотя действие с `IActionConstraint` всегда считается *имеющим приоритет*.</span><span class="sxs-lookup"><span data-stu-id="e43a5-908">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="e43a5-909">Таким образом, поскольку действие `Edit()` имеет атрибут `[HttpGet]`, оно считается более конкретным и будет выбрано в случае соответствия обоих действий.</span><span class="sxs-lookup"><span data-stu-id="e43a5-909">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="e43a5-910">По сути, интерфейс `IActionConstraint` представляет собой своего рода *перегрузку*, но вместо перегрузки методов с одинаковыми именами он перегружает действия, соответствующие одному и тому же URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="e43a5-910">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="e43a5-911">При маршрутизации с помощью атрибутов также применяется интерфейс `IActionConstraint`, в результате чего кандидатами могут считаться действия из разных контроллеров.</span><span class="sxs-lookup"><span data-stu-id="e43a5-911">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="e43a5-912">Реализация IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="e43a5-912">Implementing IActionConstraint</span></span>

<span data-ttu-id="e43a5-913">Самый простой способ реализовать интерфейс `IActionConstraint` — создать класс, производный от `System.Attribute`, и добавить его в действия и контроллеры.</span><span class="sxs-lookup"><span data-stu-id="e43a5-913">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="e43a5-914">MVC автоматически обнаруживает экземпляры `IActionConstraint`, применяемые как атрибуты.</span><span class="sxs-lookup"><span data-stu-id="e43a5-914">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="e43a5-915">Вы можете использовать модель приложения для применения ограничений, и это, пожалуй, самый гибкий подход, так как он позволяет метапрограммировать способ их применения.</span><span class="sxs-lookup"><span data-stu-id="e43a5-915">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="e43a5-916">В следующем примере ограничение выбирает действие на основе *кода страны* из данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="e43a5-916">In the following example, a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="e43a5-917">[Полный пример можно найти в GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="e43a5-917">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

<span data-ttu-id="e43a5-918">Вы отвечаете за реализацию метода `Accept` и определение очередности применения ограничений.</span><span class="sxs-lookup"><span data-stu-id="e43a5-918">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="e43a5-919">В этом случае метод `Accept` возвращает значение `true`, означающее, что действие является соответствующим при соответствии значения маршрута `country`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-919">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="e43a5-920">Таким образом, он позволяет возвращаться к действию без атрибута, чем отличается от метода `RouteValueAttribute`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-920">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="e43a5-921">В примере показано, что если определено действие `en-US`, то для кода страны `fr-FR` будет выбран более общий контроллер, к которому не применяется ограничение `[CountrySpecific(...)]`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-921">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="e43a5-922">Свойство `Order` определяет, к какому *этапу* относится ограничение.</span><span class="sxs-lookup"><span data-stu-id="e43a5-922">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="e43a5-923">Ограничения действий применяются группами в соответствии со значением `Order`.</span><span class="sxs-lookup"><span data-stu-id="e43a5-923">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="e43a5-924">Например, все предоставляемые платформой атрибуты HTTP-методов имеют одинаковое значение `Order`, благодаря чему они выполняются на одном этапе.</span><span class="sxs-lookup"><span data-stu-id="e43a5-924">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="e43a5-925">Чтобы реализовать требуемые политики, можно использовать любое количество этапов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-925">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="e43a5-926">Чтобы выбрать значение `Order`, подумайте, должно ли ограничение применяться перед выполнением HTTP-методов.</span><span class="sxs-lookup"><span data-stu-id="e43a5-926">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="e43a5-927">Чем меньше значение, тем раньше применяется ограничение.</span><span class="sxs-lookup"><span data-stu-id="e43a5-927">Lower numbers run first.</span></span>

::: moniker-end
