---
title: "Маршрутизация в действиях контроллера"
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: 497ce47fa567f163cb7b1eb891408f0100d15b8a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="routing-to-controller-actions"></a><span data-ttu-id="a96e4-102">Маршрутизация в действиях контроллера</span><span class="sxs-lookup"><span data-stu-id="a96e4-102">Routing to Controller Actions</span></span>

<span data-ttu-id="a96e4-103">По [Nowak Райана](https://github.com/rynowak) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a96e4-103">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a96e4-104">ASP.NET Core MVC использует маршрутизации [по промежуточного слоя](../../fundamentals/middleware.md) для сопоставления URL-адреса входящих запросов и сопоставить их действия.</span><span class="sxs-lookup"><span data-stu-id="a96e4-104">ASP.NET Core MVC uses the Routing [middleware](../../fundamentals/middleware.md) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="a96e4-105">Маршруты, определяются в код запуска или атрибуты.</span><span class="sxs-lookup"><span data-stu-id="a96e4-105">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="a96e4-106">Маршруты описаны сопоставления URL-пути к действиям.</span><span class="sxs-lookup"><span data-stu-id="a96e4-106">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="a96e4-107">Маршруты, также используются для создания URL-адреса (для ссылок) отправки в ответах.</span><span class="sxs-lookup"><span data-stu-id="a96e4-107">Routes are also used to generate URLs (for links) sent out in responses.</span></span> 

<span data-ttu-id="a96e4-108">Традиционно либо направляются действия или атрибут маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-108">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="a96e4-109">Размещение маршрут на контроллер или действие упрощает атрибут маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-109">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="a96e4-110">В разделе [смешанный маршрутизации](#routing-mixed-ref-label) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-110">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="a96e4-111">В этом документе объясняется взаимодействие между MVC и маршрутизации, а также как типичные Создание приложения MVC функции маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-111">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="a96e4-112">В разделе [маршрутизации](xref:fundamentals/routing) сведения о расширенной маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-112">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="a96e4-113">Настройка маршрутизации по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="a96e4-113">Setting up Routing Middleware</span></span>

<span data-ttu-id="a96e4-114">В вашей *Настройка* метода может появиться код, аналогичный:</span><span class="sxs-lookup"><span data-stu-id="a96e4-114">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a96e4-115">Внутри вызова `UseMvc`, `MapRoute` используется для создания одного маршрута, мы будем называть `default` маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-115">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="a96e4-116">Маршрут будет использоваться для большинства приложений MVC с помощью шаблона аналогично `default` маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-116">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="a96e4-117">Шаблон маршрута `"{controller=Home}/{action=Index}/{id?}"` может соответствовать пути URL-адрес, например `/Products/Details/5` , который извлечет значения маршрута `{ controller = Products, action = Details, id = 5 }` по маркирование путь.</span><span class="sxs-lookup"><span data-stu-id="a96e4-117">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="a96e4-118">MVC попытается найти контроллер с именем `ProductsController` и выполните действие `Details`:</span><span class="sxs-lookup"><span data-stu-id="a96e4-118">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="a96e4-119">Обратите внимание, что в этом примере привязки модели будет использовать значение `id = 5` для задания `id` параметра `5` при вызове этого действия.</span><span class="sxs-lookup"><span data-stu-id="a96e4-119">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="a96e4-120">В разделе [привязки модели](../models/model-binding.md) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="a96e4-120">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="a96e4-121">С помощью `default` маршрута:</span><span class="sxs-lookup"><span data-stu-id="a96e4-121">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="a96e4-122">Шаблон маршрута:</span><span class="sxs-lookup"><span data-stu-id="a96e4-122">The route template:</span></span>

* <span data-ttu-id="a96e4-123">`{controller=Home}`Определяет `Home` по умолчанию`controller`</span><span class="sxs-lookup"><span data-stu-id="a96e4-123">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="a96e4-124">`{action=Index}`Определяет `Index` по умолчанию`action`</span><span class="sxs-lookup"><span data-stu-id="a96e4-124">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="a96e4-125">`{id?}`Определяет `id` как необязательные</span><span class="sxs-lookup"><span data-stu-id="a96e4-125">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="a96e4-126">По умолчанию и параметры необязательно маршрута необязательно должны присутствовать в URL-адрес для сопоставления.</span><span class="sxs-lookup"><span data-stu-id="a96e4-126">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="a96e4-127">В разделе [ссылка на шаблон маршрута](../../fundamentals/routing.md#route-template-reference) подробное описание синтаксиса шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-127">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="a96e4-128">`"{controller=Home}/{action=Index}/{id?}"`можно сопоставить URL-адрес `/` и создает значения маршрута `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-128">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="a96e4-129">Значения для `controller` и `action` сделать использование значений по умолчанию `id` не создает значение, поскольку отсутствует соответствующий сегмент URL-пути.</span><span class="sxs-lookup"><span data-stu-id="a96e4-129">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="a96e4-130">MVC будет использовать эти значения маршрута для выбора `HomeController` и `Index` действия:</span><span class="sxs-lookup"><span data-stu-id="a96e4-130">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="a96e4-131">С помощью данного определения контроллера и шаблон маршрута, `HomeController.Index` действие будет выполняться для любого из следующих путей URL-адрес:</span><span class="sxs-lookup"><span data-stu-id="a96e4-131">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="a96e4-132">Удобный метод `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="a96e4-132">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="a96e4-133">Можно использовать для замены:</span><span class="sxs-lookup"><span data-stu-id="a96e4-133">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a96e4-134">`UseMvc`и `UseMvcWithDefaultRoute` добавить экземпляр `RouterMiddleware` для конвейера по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="a96e4-134">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="a96e4-135">MVC не взаимодействует непосредственно с по промежуточного слоя и использует маршрутизацию для обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-135">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="a96e4-136">MVC подключен ко всем маршрутам через экземпляр `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-136">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="a96e4-137">Код внутри `UseMvc` — примерно следующего содержания:</span><span class="sxs-lookup"><span data-stu-id="a96e4-137">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="a96e4-138">`UseMvc`все маршруты, непосредственно не определяет заполнитель он добавляет в коллекцию маршрутов для `attribute` маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-138">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="a96e4-139">Перегрузка `UseMvc(Action<IRouteBuilder>)` позволяет добавлять собственные объекты, а также поддерживает маршрутизацию атрибута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-139">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="a96e4-140">`UseMvc`и все его вариантов добавляется заполнитель для атрибута маршрута — атрибут маршрутизации доступна всегда независимо от того, как настроить `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-140">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="a96e4-141">`UseMvcWithDefaultRoute`определяет маршрут по умолчанию и поддерживает маршрутизацию атрибута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-141">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="a96e4-142">[Маршрутизацией атрибутов](#attribute-routing-ref-label) раздел содержит дополнительные сведения о маршрутизации атрибута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-142">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="a96e4-143">Обычная маршрутизация</span><span class="sxs-lookup"><span data-stu-id="a96e4-143">Conventional routing</span></span>

<span data-ttu-id="a96e4-144">`default` Маршрута:</span><span class="sxs-lookup"><span data-stu-id="a96e4-144">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="a96e4-145">Примером может служить *Обычная маршрутизация*.</span><span class="sxs-lookup"><span data-stu-id="a96e4-145">is an example of a *conventional routing*.</span></span> <span data-ttu-id="a96e4-146">Мы называем этот стиль *Обычная маршрутизация* , так как он устанавливает *соглашение* для пути URL-адресов:</span><span class="sxs-lookup"><span data-stu-id="a96e4-146">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="a96e4-147">Первый сегмент пути сопоставляется с именем контроллера</span><span class="sxs-lookup"><span data-stu-id="a96e4-147">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="a96e4-148">второй сопоставляется с именем действия.</span><span class="sxs-lookup"><span data-stu-id="a96e4-148">the second maps to the action name.</span></span>

* <span data-ttu-id="a96e4-149">Третий сегмента используется для необязательного `id` используется для сопоставления с сущностью модели</span><span class="sxs-lookup"><span data-stu-id="a96e4-149">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="a96e4-150">С помощью этого `default` маршрут, URL-адрес `/Products/List` сопоставляется `ProductsController.List` действия, и `/Blog/Article/17` сопоставляется `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-150">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="a96e4-151">Это сопоставление основана на именах контроллера и действия **только** и не зависят от пространства имен расположений файлов исходного кода и параметры метода.</span><span class="sxs-lookup"><span data-stu-id="a96e4-151">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="a96e4-152">С помощью стандартной маршрутизации с маршрут по умолчанию позволяет быстро создавать приложения без необходимости придумать новый шаблон URL-адреса для каждого из действий, определенных пользователем.</span><span class="sxs-lookup"><span data-stu-id="a96e4-152">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="a96e4-153">Для приложения с действиями CRUD стиля имеющие согласованности для URL-адреса между контроллерами помогают упростить код и делает более предсказуемым пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="a96e4-153">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="a96e4-154">`id` Определяется как необязательный шаблон маршрута, это означает, что ваши действия могут выполняться без кода, входящее в состав URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="a96e4-154">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="a96e4-155">Обычно что произойдет, если `id` исключается из URL-адрес — что он будет присвоено `0` привязкой модели и в результате не сущности будут находиться в соответствующей базе данных `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-155">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="a96e4-156">Атрибут маршрутизации может предоставить точного управления, чтобы сделать код, необходимый для некоторых действий, но не других.</span><span class="sxs-lookup"><span data-stu-id="a96e4-156">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="a96e4-157">По соглашению, документация будет содержать необязательные параметры, например `id` когда они вероятнее всего, появятся в правильный тип использования.</span><span class="sxs-lookup"><span data-stu-id="a96e4-157">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="a96e4-158">Несколько маршрутов</span><span class="sxs-lookup"><span data-stu-id="a96e4-158">Multiple routes</span></span>

<span data-ttu-id="a96e4-159">Можно добавить несколько маршрутов внутри `UseMvc` , добавив дополнительные вызовы `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-159">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="a96e4-160">Это позволит вам определить несколько соглашения или добавить обычной маршруты, которые выделены для определенного действия, такие как:</span><span class="sxs-lookup"><span data-stu-id="a96e4-160">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a96e4-161">`blog` Маршрут здесь *выделенной обычной маршрута*, это означает, что он использует традиционных систем маршрутизации, но предназначенный для определенных действий.</span><span class="sxs-lookup"><span data-stu-id="a96e4-161">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="a96e4-162">Поскольку `controller` и `action` не отображаются в шаблоне маршрута как параметры, они могут иметь только значения по умолчанию и таким образом этот маршрут всегда сопоставлять действие `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-162">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="a96e4-163">Маршруты в коллекции маршрутов упорядочены и обрабатываются в порядке, в котором они добавляются.</span><span class="sxs-lookup"><span data-stu-id="a96e4-163">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="a96e4-164">Поэтому в этом примере `blog` маршрута будет предпринята перед `default` маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-164">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="a96e4-165">*Выделенные обычной маршруты* часто используют параметры универсальным маршрут, например `{*article}` для захвата оставшуюся часть URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="a96e4-165">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="a96e4-166">Это можно сделать маршрута «слишком жадное» это означает, что он совпадает с URL-адреса, которые предназначены для сопоставления с других маршрутов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-166">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="a96e4-167">Поместите маршруты «жадного» ниже в таблице маршрутов, чтобы устранить эту проблему.</span><span class="sxs-lookup"><span data-stu-id="a96e4-167">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="a96e4-168">Откат</span><span class="sxs-lookup"><span data-stu-id="a96e4-168">Fallback</span></span>

<span data-ttu-id="a96e4-169">В ходе обработки запроса MVC проверит, что значения маршрута может использоваться для поиска контроллера и действия в приложении.</span><span class="sxs-lookup"><span data-stu-id="a96e4-169">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="a96e4-170">Если значения маршрута не соответствует действию затем маршрута не рассматриваются как совпадающие, и следующий маршрут будет предпринята.</span><span class="sxs-lookup"><span data-stu-id="a96e4-170">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="a96e4-171">Это называется *резервной*, и он предназначен для упрощения случаях перекрытия обычной маршруты.</span><span class="sxs-lookup"><span data-stu-id="a96e4-171">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="a96e4-172">Неоднозначными действия</span><span class="sxs-lookup"><span data-stu-id="a96e4-172">Disambiguating actions</span></span>

<span data-ttu-id="a96e4-173">Если два действия совпадают по маршрутизация, MVC должны устранить неоднозначность в выберите кандидата «рекомендации», либо исключение.</span><span class="sxs-lookup"><span data-stu-id="a96e4-173">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="a96e4-174">Пример:</span><span class="sxs-lookup"><span data-stu-id="a96e4-174">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="a96e4-175">Этот контроллер определяет два действия, которые будет соответствовать URL-адрес `/Products/Edit/17` и маршрутизации данных `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-175">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="a96e4-176">Это типичный шаблон для контроллеров MVC где `Edit(int)` отображает форму для изменения продукта, и `Edit(int, Product)` обрабатывает отправленной формы.</span><span class="sxs-lookup"><span data-stu-id="a96e4-176">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="a96e4-177">Для этого потребуется MVC выбрать `Edit(int, Product)` Если запрашивается HTTP `POST` и `Edit(int)` при команду HTTP-либо еще.</span><span class="sxs-lookup"><span data-stu-id="a96e4-177">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="a96e4-178">`HttpPostAttribute` ( `[HttpPost]` ) — Это реализация `IActionConstraint` , разрешает только действие выделяется при HTTP-команда `POST`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-178">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="a96e4-179">Наличие `IActionConstraint` делает `Edit(int, Product)` соответствует лучше, чем `Edit(int)`, поэтому `Edit(int, Product)` будут применяться первыми.</span><span class="sxs-lookup"><span data-stu-id="a96e4-179">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="a96e4-180">Необходимо только создавать пользовательские `IActionConstraint` реализации в специализированных сценариев, но важно понимать роль атрибутов, например `HttpPostAttribute` -похожие атрибуты определяются для других команд HTTP.</span><span class="sxs-lookup"><span data-stu-id="a96e4-180">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="a96e4-181">В обычной маршрутизации являются общими для действий использовать то же имя действия в том случае, если они являются частью `show form -> submit form` рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="a96e4-181">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="a96e4-182">Удобство этот шаблон будет становятся более очевидными, просмотрев [IActionConstraint основные сведения о](#understanding-iactionconstraint) раздела.</span><span class="sxs-lookup"><span data-stu-id="a96e4-182">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="a96e4-183">Если соответствует несколько маршрутов, и MVC не удается найти маршрут «рекомендации», он будет вызывать `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-183">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="a96e4-184">Имена маршрутов</span><span class="sxs-lookup"><span data-stu-id="a96e4-184">Route names</span></span>

<span data-ttu-id="a96e4-185">Строки `"blog"` и `"default"` в следующих примерах, имена маршрутов:</span><span class="sxs-lookup"><span data-stu-id="a96e4-185">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a96e4-186">Имена маршрутов помогут маршрута логическое имя именованный маршрут может использоваться для создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-186">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="a96e4-187">Это значительно упрощает создание URL-адрес при упорядочение маршрутов может стать сложным создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-187">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="a96e4-188">Имена маршрутов должны быть уникальными на уровне приложения.</span><span class="sxs-lookup"><span data-stu-id="a96e4-188">Route names must be unique application-wide.</span></span>

<span data-ttu-id="a96e4-189">Имена маршрутов не оказывают влияния на URL-адрес, соответствующий или обработка запросов; они используются только для создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-189">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="a96e4-190">[Маршрутизация](xref:fundamentals/routing) содержатся более подробные сведения о создании URL-адреса, включая создания URL-адресов в MVC конкретных вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-190">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="a96e4-191">Атрибут маршрутизации</span><span class="sxs-lookup"><span data-stu-id="a96e4-191">Attribute routing</span></span>

<span data-ttu-id="a96e4-192">Атрибут маршрутизации использует набор атрибутов для сопоставления действия шаблонов маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-192">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="a96e4-193">В следующем примере `app.UseMvc();` используется в `Configure` метод и ни один маршрут не передается.</span><span class="sxs-lookup"><span data-stu-id="a96e4-193">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="a96e4-194">`HomeController` Будут соответствовать набору аналогично маршрут по умолчанию URL-адреса `{controller=Home}/{action=Index}/{id?}` будет соответствовать:</span><span class="sxs-lookup"><span data-stu-id="a96e4-194">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="a96e4-195">`HomeController.Index()` Действие будет выполняться для всех путей URL-адрес `/`, `/Home`, или `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-195">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="a96e4-196">В этом примере представлены программирования основное отличие между атрибутом и обычной маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-196">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="a96e4-197">Маршрутизация атрибут требует дальнейшего ввода данных для определения маршрута; маршрут по умолчанию обычной более четко обрабатывает маршрутов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-197">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="a96e4-198">Тем не менее маршрутизацией атрибутов позволяет (и требует) точный контроль, из которых шаблонов маршрута применяются к каждому действию.</span><span class="sxs-lookup"><span data-stu-id="a96e4-198">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="a96e4-199">С атрибутом маршрутизации имя контроллера и имена действий воспроизведение **не** роли, в которой выбрано действие.</span><span class="sxs-lookup"><span data-stu-id="a96e4-199">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="a96e4-200">В этом примере будет соответствовать же URL-адреса в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="a96e4-200">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="a96e4-201">Выше шаблонов маршрута не определяют параметры маршрута для `action`, `area`, и `controller`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-201">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="a96e4-202">На самом деле эти параметры маршрута в атрибут маршрутов не разрешены.</span><span class="sxs-lookup"><span data-stu-id="a96e4-202">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="a96e4-203">Поскольку шаблон маршрута уже связан с действием, не имеет смысла для синтаксического анализа имя действия по URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="a96e4-203">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="a96e4-204">Атрибут маршрутизации с атрибутами Http [команда]</span><span class="sxs-lookup"><span data-stu-id="a96e4-204">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="a96e4-205">Атрибут маршрутизации можно также сделать использование `Http[Verb]` атрибуты, такие как `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-205">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="a96e4-206">Все эти атрибуты могут принимать шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-206">All of these attributes can accept a route template.</span></span> <span data-ttu-id="a96e4-207">В этом примере показано два действий, которые соответствуют один и тот же шаблон маршрута:</span><span class="sxs-lookup"><span data-stu-id="a96e4-207">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="a96e4-208">Для пути URL-адрес, например `/products` `ProductsApi.ListProducts` действие будет выполняться при HTTP-команда `GET` и `ProductsApi.CreateProduct` будет выполняться при HTTP-команда `POST`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-208">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="a96e4-209">Сначала маршрутизации атрибут соответствует URL-адрес с набором шаблонов маршрута, определенные атрибутами маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-209">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="a96e4-210">После соответствует шаблон маршрута, `IActionConstraint` ограничения применяются, чтобы определить, какие действия могут быть выполнены.</span><span class="sxs-lookup"><span data-stu-id="a96e4-210">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="a96e4-211">При построении REST API, довольно редко, что вы хотите использовать `[Route(...)]` на метод действия.</span><span class="sxs-lookup"><span data-stu-id="a96e4-211">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="a96e4-212">Рекомендуется использовать более конкретного `Http*Verb*Attributes` необходимо точно определить, поддерживает API.</span><span class="sxs-lookup"><span data-stu-id="a96e4-212">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="a96e4-213">Клиенты REST API-интерфейсов, должны знать, что пути и HTTP-команды сопоставить определенные логические операции.</span><span class="sxs-lookup"><span data-stu-id="a96e4-213">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="a96e4-214">Поскольку маршрут атрибута применяется к определенное действие, легко изменять параметры, необходимые в рамках определения шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-214">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="a96e4-215">В этом примере `id` является неотъемлемой частью URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="a96e4-215">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="a96e4-216">`ProductsApi.GetProduct(int)` Действие будет выполнено для пути URL-адрес, например `/products/3` , но не для пути URL-адрес, например `/products`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-216">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="a96e4-217">В разделе [маршрутизации](../../fundamentals/routing.md) полное описание шаблонов маршрута и связанные параметры.</span><span class="sxs-lookup"><span data-stu-id="a96e4-217">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="a96e4-218">Имя маршрута</span><span class="sxs-lookup"><span data-stu-id="a96e4-218">Route Name</span></span>

<span data-ttu-id="a96e4-219">В следующем коде определяется *имя маршрута* из `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="a96e4-219">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="a96e4-220">Имена маршрутов можно использовать для создания URL-адрес, на основании конкретного маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-220">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="a96e4-221">Имена маршрутов не оказывают влияния на поведение маршрутизации сопоставления URL-адрес и используются только для создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-221">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="a96e4-222">Имена маршрутов должны быть уникальными на уровне приложения.</span><span class="sxs-lookup"><span data-stu-id="a96e4-222">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="a96e4-223">Сравните это с обычной *маршрут по умолчанию*, который определяет `id` параметр как необязательные (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="a96e4-223">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="a96e4-224">Эта возможность точного указания API-интерфейсы имеет преимущества, например `/products` и `/products/5` для отправки для различных действий.</span><span class="sxs-lookup"><span data-stu-id="a96e4-224">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="a96e4-225">Объединение маршрутов</span><span class="sxs-lookup"><span data-stu-id="a96e4-225">Combining routes</span></span>

<span data-ttu-id="a96e4-226">Чтобы сделать атрибут маршрутизации менее повторяющихся, атрибуты маршрута на контроллере, объединяются с атрибуты маршрута в отдельных операциях.</span><span class="sxs-lookup"><span data-stu-id="a96e4-226">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="a96e4-227">Какой-либо шаблон маршрута на контроллере добавляются к шаблонов маршрута для действия.</span><span class="sxs-lookup"><span data-stu-id="a96e4-227">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="a96e4-228">Делает помещения атрибута маршрута для контроллера **все** действий в контроллере маршрутизацию атрибута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-228">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="a96e4-229">В этом примере URL-адрес `/products` можно сопоставить `ProductsApi.ListProducts`и URL-адрес `/products/5` может соответствовать `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-229">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="a96e4-230">Оба эти действия только соответствовать HTTP `GET` так, как они декорированных `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-230">Both of these actions only match HTTP `GET` because they're decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="a96e4-231">Направления примененных действий, которые начинаются с шаблонов `/` не получить в сочетании с шаблоны маршрута, примененные к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="a96e4-231">Route templates applied to an action that begin with a `/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="a96e4-232">В этом примере соответствует набору URL-адрес путей, похожих на *маршрут по умолчанию*.</span><span class="sxs-lookup"><span data-stu-id="a96e4-232">This example matches a set of URL paths similar to the *default route*.</span></span>

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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="a96e4-233">Упорядочение атрибутов маршрутов</span><span class="sxs-lookup"><span data-stu-id="a96e4-233">Ordering attribute routes</span></span>

<span data-ttu-id="a96e4-234">В отличие от обычных маршруты, которые выполняются в определенном порядке маршрутизацией атрибутов строит дерево и сопоставляет все маршруты одновременно.</span><span class="sxs-lookup"><span data-stu-id="a96e4-234">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="a96e4-235">Это аналогично тому как-если записи маршрута были помещены в идеальный порядок; Наиболее конкретные пути имеют возможность выполнения более общих маршрутов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-235">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="a96e4-236">Например, такая как маршрут `blog/search/{topic}` является более точным определением, чем маршрут как `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-236">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="a96e4-237">Говоря логически `blog/search/{topic}` маршрута «выполняется» во-первых, по умолчанию, поскольку это только разумно упорядочение.</span><span class="sxs-lookup"><span data-stu-id="a96e4-237">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="a96e4-238">С помощью стандартной маршрутизации, разработчик отвечает за размещение маршруты в требуемом порядке.</span><span class="sxs-lookup"><span data-stu-id="a96e4-238">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="a96e4-239">Маршруты атрибута можно настроить порядок с помощью `Order` всех атрибутов маршрутов платформы.</span><span class="sxs-lookup"><span data-stu-id="a96e4-239">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="a96e4-240">Маршруты обрабатываются в соответствии с возрастающем сортировки `Order` свойство.</span><span class="sxs-lookup"><span data-stu-id="a96e4-240">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="a96e4-241">Порядок по умолчанию `0`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-241">The default order is `0`.</span></span> <span data-ttu-id="a96e4-242">Настройка маршрута с помощью `Order = -1` выполняется раньше, чем маршруты, в которых не задано заказа.</span><span class="sxs-lookup"><span data-stu-id="a96e4-242">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="a96e4-243">Настройка маршрута с помощью `Order = 1` будет выполняться после упорядочения маршрута по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a96e4-243">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="a96e4-244">Избегайте в зависимости от `Order`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-244">Avoid depending on `Order`.</span></span> <span data-ttu-id="a96e4-245">Если пространстве URL-адреса требует явного порядка значений правильно направить, он скорее всего путаницу также клиентам.</span><span class="sxs-lookup"><span data-stu-id="a96e4-245">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="a96e4-246">В целом маршрутизацией атрибутов будет выбирать правильный маршрут с соответствующим URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="a96e4-246">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="a96e4-247">Если порядок по умолчанию, используемый для создания URL-адресов не работает, используя имя маршрута, как переопределения обычно проще, чем применение `Order` свойство.</span><span class="sxs-lookup"><span data-stu-id="a96e4-247">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="a96e4-248">Токен замены в шаблонах маршрутов ([контроллер] [действие] [область])</span><span class="sxs-lookup"><span data-stu-id="a96e4-248">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="a96e4-249">Для удобства поддержки маршруты атрибута *заменить токен* путем заключения маркер в квадратные скобки (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="a96e4-249">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="a96e4-250">Токены `[action]`, `[area]`, и `[controller]` будут заменены значениями имя действия, имя области и имя контроллера, из действия, в котором определен маршрут.</span><span class="sxs-lookup"><span data-stu-id="a96e4-250">The tokens `[action]`, `[area]`, and `[controller]` will be replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="a96e4-251">В этом примере действий может соответствовать пути URL-адресов, как описано в комментариях:</span><span class="sxs-lookup"><span data-stu-id="a96e4-251">In this example the actions can match URL paths as described in the comments:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="a96e4-252">На последнем этапе построения маршруты атрибутов происходит замена токенов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-252">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="a96e4-253">Приведенный выше пример будет функционировать так же, как следующий код:</span><span class="sxs-lookup"><span data-stu-id="a96e4-253">The above example will behave the same as the following code:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="a96e4-254">Маршруты атрибутов может также быть совмещено с наследованием.</span><span class="sxs-lookup"><span data-stu-id="a96e4-254">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="a96e4-255">Это очень мощная в сочетании с замещения токена.</span><span class="sxs-lookup"><span data-stu-id="a96e4-255">This is particularly powerful combined with token replacement.</span></span>

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

<span data-ttu-id="a96e4-256">Замена токенов также применяется к имена маршрутов, определяемый маршрутов с атрибутами.</span><span class="sxs-lookup"><span data-stu-id="a96e4-256">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="a96e4-257">`[Route("[controller]/[action]", Name="[controller]_[action]")]`Создает имя маршрута, уникальное для каждого действия.</span><span class="sxs-lookup"><span data-stu-id="a96e4-257">`[Route("[controller]/[action]", Name="[controller]_[action]")]` will generate a unique route name for each action.</span></span>

<span data-ttu-id="a96e4-258">Для сопоставления разделитель литерала замещения токена `[` или `]`, экранировать его повторяя символ (`[[` или `]]`).</span><span class="sxs-lookup"><span data-stu-id="a96e4-258">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="a96e4-259">Несколько маршрутов</span><span class="sxs-lookup"><span data-stu-id="a96e4-259">Multiple Routes</span></span>

<span data-ttu-id="a96e4-260">Атрибут маршрутизации поддерживает определение нескольких маршрутов достижения те же действия.</span><span class="sxs-lookup"><span data-stu-id="a96e4-260">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="a96e4-261">Наиболее распространенное использование является имитируют поведение *обычной маршрут по умолчанию* как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="a96e4-261">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="a96e4-262">Размещение различных атрибутов маршрутов на контроллере означает каждый из них объединит с каждым из атрибутов маршрутов на методы действий.</span><span class="sxs-lookup"><span data-stu-id="a96e4-262">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="a96e4-263">При различных атрибутов маршрутов (, реализующие `IActionConstraint`) помещаются в действие, а затем объединяет каждое действие ограничение с помощью шаблона маршрута из атрибута, который она определена.</span><span class="sxs-lookup"><span data-stu-id="a96e4-263">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="a96e4-264">С помощью нескольких маршрутов на действия может показаться мощные, лучше хранить пространстве URL-адреса приложения простой и четко определенных.</span><span class="sxs-lookup"><span data-stu-id="a96e4-264">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="a96e4-265">Используйте несколько маршрутов на действия только в том случае, при необходимости, например для поддержки существующих клиентов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-265">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="a96e4-266">Указание атрибута маршрута необязательные параметры, значения по умолчанию и ограничений</span><span class="sxs-lookup"><span data-stu-id="a96e4-266">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="a96e4-267">Маршруты атрибута поддерживают один и тот же синтаксис встроенного как обычной маршруты, чтобы указать необязательные параметры, значения по умолчанию и ограничения.</span><span class="sxs-lookup"><span data-stu-id="a96e4-267">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="a96e4-268">В разделе [ссылка на шаблон маршрута](../../fundamentals/routing.md#route-template-reference) подробное описание синтаксиса шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-268">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="a96e4-269">Настраиваемый маршрут атрибутов с помощью`IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="a96e4-269">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="a96e4-270">Все атрибуты маршрута, доступные в framework ( `[Route(...)]`, `[HttpGet(...)]` , т. д.) реализуют `IRouteTemplateProvider` интерфейса.</span><span class="sxs-lookup"><span data-stu-id="a96e4-270">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="a96e4-271">MVC выполняет поиск атрибутов в контроллер классы и методы действий при запуске и использует те, которые реализуют приложения `IRouteTemplateProvider` для создания начального набора маршрутов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-271">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="a96e4-272">Можно реализовать `IRouteTemplateProvider` определение собственных атрибутов маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-272">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="a96e4-273">Каждый `IRouteTemplateProvider` можно задать один маршрут с помощью шаблона настраиваемый маршрут порядок и имя:</span><span class="sxs-lookup"><span data-stu-id="a96e4-273">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="a96e4-274">В приведенном выше примере атрибут автоматически задает `Template` для `"api/[controller]"` при `[MyApiController]` применяется.</span><span class="sxs-lookup"><span data-stu-id="a96e4-274">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="a96e4-275">С помощью модели приложений для настройки маршрутов с атрибутами</span><span class="sxs-lookup"><span data-stu-id="a96e4-275">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="a96e4-276">*Модель приложения* — это объектная модель, созданными при запуске все метаданные, используемые платформой MVC для маршрутизации и выполнения действий.</span><span class="sxs-lookup"><span data-stu-id="a96e4-276">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="a96e4-277">*Модель приложения* включает в себя все данные, собранные из атрибутов маршрутов (через `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="a96e4-277">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="a96e4-278">Можно написать *соглашения* для изменения модели приложения во время запуска для настройки поведения маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-278">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="a96e4-279">В этом разделе показан простой пример настройки маршрутизации с помощью модели приложения.</span><span class="sxs-lookup"><span data-stu-id="a96e4-279">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="a96e4-280">Смешанный маршрутизации: атрибут vs обычной маршрутизацию</span><span class="sxs-lookup"><span data-stu-id="a96e4-280">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="a96e4-281">MVC-приложений можно смешивать использование обычных и атрибут маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-281">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="a96e4-282">Он является типичным обычной маршруты для контроллеров, обслуживающий HTML-страниц для браузеров, а атрибут маршрутизации для контроллеров, обслуживающий API-интерфейсов REST.</span><span class="sxs-lookup"><span data-stu-id="a96e4-282">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="a96e4-283">Традиционно либо направляются действия или атрибут маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-283">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="a96e4-284">Размещение маршрут на контроллер или действие упрощает атрибут маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-284">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="a96e4-285">Действия, которые определяют маршруты атрибутов не удается подключиться через обычные маршруты и наоборот.</span><span class="sxs-lookup"><span data-stu-id="a96e4-285">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="a96e4-286">**Любой** атрибут маршрута на контроллере делает все действия в атрибуте контроллера маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-286">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="a96e4-287">Что отличает два типа маршрутизации систем — это процесс, который применяется после URL-адрес соответствует шаблону маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-287">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="a96e4-288">В Обычная маршрутизация значения маршрута при поиске используются для выбора из таблицы подстановок всех обычных действий перенаправленное действия и контроллера.</span><span class="sxs-lookup"><span data-stu-id="a96e4-288">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="a96e4-289">Атрибут маршрутизации, каждый шаблон уже связан с действием, и требуется без дальнейшего просмотра.</span><span class="sxs-lookup"><span data-stu-id="a96e4-289">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="a96e4-290">Создание URL-адреса</span><span class="sxs-lookup"><span data-stu-id="a96e4-290">URL Generation</span></span>

<span data-ttu-id="a96e4-291">Для создания URL-адрес ссылки на действия MVC-приложений можно использовать функции маршрутизации для создания URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="a96e4-291">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="a96e4-292">Создание URL-адреса позволяет исключить URL-адреса жесткого задания, что делает код более надежным и простым в обслуживании.</span><span class="sxs-lookup"><span data-stu-id="a96e4-292">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="a96e4-293">В этом разделе основное внимание уделяется возможности создания URL-адрес, предоставленные MVC и рассматриваются только основные сведения о работе создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-293">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="a96e4-294">В разделе [маршрутизации](../../fundamentals/routing.md) подробное описание создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-294">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="a96e4-295">`IUrlHelper` Интерфейс является базовой инфраструктуры между MVC и маршрутизации для создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-295">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="a96e4-296">Вы найдете экземпляр `IUrlHelper` через `Url` свойство контроллеров, представлений и просмотр компонентов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-296">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="a96e4-297">В этом примере `IUrlHelper` через используется интерфейс `Controller.Url` свойства для создания URL-адрес другого действия.</span><span class="sxs-lookup"><span data-stu-id="a96e4-297">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="a96e4-298">Если приложение использует значение по умолчанию обычной маршрутизации, значение `url` переменная будет пути строки URL-адреса `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-298">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="a96e4-299">Этот URL-адрес создается по Маршрутизация путем объединения значения маршрута из текущего запроса (окружения значений), с помощью значений, переданных `Url.Action` и подстановка эти значения в шаблоне маршрута:</span><span class="sxs-lookup"><span data-stu-id="a96e4-299">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="a96e4-300">Каждый параметр маршрута в шаблоне маршрута имеет его значение заменяют соответствующие имена со значениями и окружения значения.</span><span class="sxs-lookup"><span data-stu-id="a96e4-300">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="a96e4-301">Параметр маршрута, который не имеет значения можно использовать значение по умолчанию, если он имеется, или можно пропустить, если он является необязательным (как в случае `id` в этом примере).</span><span class="sxs-lookup"><span data-stu-id="a96e4-301">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="a96e4-302">Создании URL-адреса завершится ошибкой, если какой-либо параметр необходимые маршрута не имеет соответствующее значение.</span><span class="sxs-lookup"><span data-stu-id="a96e4-302">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="a96e4-303">В случае создания URL-адреса для маршрута следующий маршрут, предпринимается попытка до попытки все маршруты, или найдено совпадение.</span><span class="sxs-lookup"><span data-stu-id="a96e4-303">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="a96e4-304">Пример `Url.Action` выше предполагается Обычная маршрутизация, но URL-адрес поколения работает точно так же при маршрутизации атрибута, однако концепции отличаются.</span><span class="sxs-lookup"><span data-stu-id="a96e4-304">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="a96e4-305">При обычной маршрутизации значения маршрута используются для расширения шаблон и значения маршрута для `controller` и `action` обычно отображаются в этом шаблоне - это работает, поскольку соответствуют соответствовавшего маршрутизации URL-адреса *соглашение*.</span><span class="sxs-lookup"><span data-stu-id="a96e4-305">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="a96e4-306">В маршрутизации атрибутов, значений маршрута для `controller` и `action` не могут отображаться в шаблоне - вместо этого они используются для поиска шаблон, который должен использовать.</span><span class="sxs-lookup"><span data-stu-id="a96e4-306">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="a96e4-307">В этом примере используется атрибут маршрутизации:</span><span class="sxs-lookup"><span data-stu-id="a96e4-307">This example uses attribute routing:</span></span>

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="a96e4-308">MVC создает таблицу подстановки всех действий по Маршрутизация атрибута и будет соответствовать `controller` и `action` значения для выбора маршрута шаблон, используемый для создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-308">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="a96e4-309">В примере выше `custom/url/to/destination` создается.</span><span class="sxs-lookup"><span data-stu-id="a96e4-309">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="a96e4-310">Создание URL-адреса, имя действия</span><span class="sxs-lookup"><span data-stu-id="a96e4-310">Generating URLs by action name</span></span>

<span data-ttu-id="a96e4-311">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="a96e4-311">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="a96e4-312">`Action`) и все связанные перегрузки, все они основаны на этой идеи, что вы хотите указать, что создании связи путем указания имени контроллера и действия.</span><span class="sxs-lookup"><span data-stu-id="a96e4-312">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="a96e4-313">При использовании `Url.Action`, значения текущего маршрута для `controller` и `action` задаются автоматически - значение `controller` и `action` являются частью обоих *окружения значения* **и** *значения*.</span><span class="sxs-lookup"><span data-stu-id="a96e4-313">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="a96e4-314">Метод `Url.Action`, всегда использует текущие значения `action` и `controller` и будет создавать URL-путь к текущему действию.</span><span class="sxs-lookup"><span data-stu-id="a96e4-314">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="a96e4-315">Маршрутизация пытается использовать значения в окружения значения для заполнения сведений, которые не предоставляют при формировании URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="a96e4-315">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="a96e4-316">С помощью маршрута как `{a}/{b}/{c}/{d}` и окружения значения `{ a = Alice, b = Bob, c = Carol, d = David }`, маршрутизации предоставляет достаточно сведений для создания URL-адреса без дополнительных значений — поскольку направлять все параметры имеют значения.</span><span class="sxs-lookup"><span data-stu-id="a96e4-316">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="a96e4-317">Если вы добавили значение `{ d = Donovan }`, значение `{ d = David }` будет проигнорировано, и созданный URL-адрес будет `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-317">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="a96e4-318">URL-пути являются иерархическими.</span><span class="sxs-lookup"><span data-stu-id="a96e4-318">URL paths are hierarchical.</span></span> <span data-ttu-id="a96e4-319">В примере выше, если вы добавили значение `{ c = Cheryl }`, оба значения `{ c = Carol, d = David }` будет проигнорировано.</span><span class="sxs-lookup"><span data-stu-id="a96e4-319">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="a96e4-320">В этом случае мы больше не задано значение `d` и создания URL-адресов не удастся.</span><span class="sxs-lookup"><span data-stu-id="a96e4-320">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="a96e4-321">Необходимо указать нужное значение `c` и `d`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-321">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="a96e4-322">Ожидается возникновение неполадок с маршрут по умолчанию (`{controller}/{action}/{id?}`)-это на практике, как возникает редко, но `Url.Action` будет всегда явно указывать `controller` и `action` значение.</span><span class="sxs-lookup"><span data-stu-id="a96e4-322">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="a96e4-323">Больше перегрузки `Url.Action` также принимают дополнительный *значения маршрута* объекта для предоставления значений для параметров маршрута, отличный от `controller` и `action`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-323">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="a96e4-324">Вы увидите наиболее часто используется с `id` как `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-324">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="a96e4-325">По соглашению *значения маршрута* объект обычно является анонимный тип объекта, но это также может быть `IDictionary<>` или *обычный объект .NET*.</span><span class="sxs-lookup"><span data-stu-id="a96e4-325">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="a96e4-326">Все значения дополнительных маршрута, не соответствующих параметров маршрута, помещаются в строку запроса.</span><span class="sxs-lookup"><span data-stu-id="a96e4-326">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="a96e4-327">Чтобы создать абсолютный URL-адрес, используйте перегрузку, которая принимает `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="a96e4-327">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="a96e4-328">Создание URL-адреса, маршрут</span><span class="sxs-lookup"><span data-stu-id="a96e4-328">Generating URLs by route</span></span>

<span data-ttu-id="a96e4-329">Приведенный выше код показано создание URL-адреса, передав имя контроллера и действия.</span><span class="sxs-lookup"><span data-stu-id="a96e4-329">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="a96e4-330">`IUrlHelper`также предоставляет `Url.RouteUrl` семейство методов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-330">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="a96e4-331">Эти методы похожи на `Url.Action`, но они не копирует текущие значения `action` и `controller` значения маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-331">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="a96e4-332">Укажите имя маршрута для использования конкретного маршрута для формирования URL-адрес, обычно является наиболее распространенное использование *без* указания имени контроллера или действия.</span><span class="sxs-lookup"><span data-stu-id="a96e4-332">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="a96e4-333">Создание URL-адреса в формате HTML</span><span class="sxs-lookup"><span data-stu-id="a96e4-333">Generating URLs in HTML</span></span>

<span data-ttu-id="a96e4-334">`IHtmlHelper`предоставляет `HtmlHelper` методы `Html.BeginForm` и `Html.ActionLink` для создания `<form>` и `<a>` элементы соответственно.</span><span class="sxs-lookup"><span data-stu-id="a96e4-334">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="a96e4-335">Эти методы используют `Url.Action` метод для создания URL-адреса и принятии аналогичные аргументов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-335">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="a96e4-336">`Url.RouteUrl` Companions для `HtmlHelper` , `Html.BeginRouteForm` и `Html.RouteLink` которого имеют одинаковые функции.</span><span class="sxs-lookup"><span data-stu-id="a96e4-336">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="a96e4-337">TagHelpers создания URL-адресов через `form` вспомогательной функции тегов и `<a>` вспомогательной функции тегов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-337">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="a96e4-338">Они используют `IUrlHelper` для их реализации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-338">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="a96e4-339">В разделе [работа с формами](../views/working-with-forms.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-339">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="a96e4-340">В представлениях `IUrlHelper` доступна через `Url` свойства для создания URL-адрес любого ad-hoc, не представленных выше.</span><span class="sxs-lookup"><span data-stu-id="a96e4-340">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="a96e4-341">Создание URL-адреса в результаты действий</span><span class="sxs-lookup"><span data-stu-id="a96e4-341">Generating URLS in Action Results</span></span>

<span data-ttu-id="a96e4-342">В вышеприведенных примерах показано использование `IUrlHelper` в контроллере, пока наиболее распространенное использование в контроллере для формирования URL-адрес как часть результата действия.</span><span class="sxs-lookup"><span data-stu-id="a96e4-342">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="a96e4-343">`ControllerBase` И `Controller` базовые классы предоставляют удобные методы для результатов действий, которые ссылаются на другое действие.</span><span class="sxs-lookup"><span data-stu-id="a96e4-343">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="a96e4-344">Один обычно используется для перенаправления после принимать входные данные пользователя.</span><span class="sxs-lookup"><span data-stu-id="a96e4-344">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

<span data-ttu-id="a96e4-345">Фабричные методы результаты действия следуют схеме, к методам `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-345">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="a96e4-346">Особым случаем для выделенного обычной маршрутов</span><span class="sxs-lookup"><span data-stu-id="a96e4-346">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="a96e4-347">Обычная маршрутизация можно использовать специальный вид определение маршрут с именем *выделенной обычной маршрута*.</span><span class="sxs-lookup"><span data-stu-id="a96e4-347">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="a96e4-348">В следующем примере маршрут с именем `blog` выделенной обычной маршрутом.</span><span class="sxs-lookup"><span data-stu-id="a96e4-348">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a96e4-349">С помощью этих определений маршрутов `Url.Action("Index", "Home")` создаст URL-адрес `/` с `default` маршрута, но и почему?</span><span class="sxs-lookup"><span data-stu-id="a96e4-349">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="a96e4-350">Догадаться значения маршрута `{ controller = Home, action = Index }` должен быть достаточно, чтобы создать URL-адрес с помощью `blog`, и результат будет `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-350">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="a96e4-351">Выделенный обычной маршруты полагаться на специальное поведение значений по умолчанию, у которых нет соответствующего параметра маршрута, который блокирует маршрута «слишком жадного» с создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-351">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="a96e4-352">В этом случае значения по умолчанию, `{ controller = Blog, action = Article }`и ни `controller` , ни `action` отображается в виде параметра маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-352">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="a96e4-353">При маршрутизации выполняет создания URL-адресов, предоставленных значений должны соответствовать значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a96e4-353">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="a96e4-354">URL-адрес с помощью создания `blog` завершится сбоем, поскольку значения `{ controller = Home, action = Index }` не соответствуют `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-354">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="a96e4-355">Маршрутизация затем возвращается к повторите `default`, которая завершается успешно.</span><span class="sxs-lookup"><span data-stu-id="a96e4-355">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="a96e4-356">Области</span><span class="sxs-lookup"><span data-stu-id="a96e4-356">Areas</span></span>

<span data-ttu-id="a96e4-357">[Области](areas.md) — это функция MVC, используемое для систематизации связанные функциональные возможности в группу как отдельное маршрутизации-пространство имен (для действий контроллеров) и структуру папок (для представления).</span><span class="sxs-lookup"><span data-stu-id="a96e4-357">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="a96e4-358">Использование областей позволяет приложению несколько контроллеров с тем же именем — при условии, что они имеют разные *областей*.</span><span class="sxs-lookup"><span data-stu-id="a96e4-358">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="a96e4-359">Использование областей создает иерархию с целью маршрутизации путем добавления другого параметра маршрута, `area` для `controller` и `action`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-359">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="a96e4-360">В этом разделе обсуждается, как маршрутизации взаимодействует с областями - разделе [областей](areas.md) подробные сведения об использовании области с представлениями.</span><span class="sxs-lookup"><span data-stu-id="a96e4-360">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="a96e4-361">В следующем примере настраивается MVC для использования обычной маршрута по умолчанию и *область маршрута* для области с именем `Blog`:</span><span class="sxs-lookup"><span data-stu-id="a96e4-361">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="a96e4-362">При сопоставлении URL-путь, например `/Manage/Users/AddUser`, первый маршрут сформирует значения маршрута `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-362">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="a96e4-363">`area` Маршрут значение создается путем значение по умолчанию для `area`, на самом деле маршрута, созданные `MapAreaRoute` эквивалентно следующему:</span><span class="sxs-lookup"><span data-stu-id="a96e4-363">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="a96e4-364">`MapAreaRoute`создает маршрут, используя значение по умолчанию и ограничения для `area` имени указанного область, в этом случае `Blog`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-364">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="a96e4-365">Значение по умолчанию обеспечивает всегда дает маршрута `{ area = Blog, ... }`, ограничение требует значение `{ area = Blog, ... }` для создания URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-365">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="a96e4-366">Обычная маршрутизация зависит от порядка сортировки.</span><span class="sxs-lookup"><span data-stu-id="a96e4-366">Conventional routing is order-dependent.</span></span> <span data-ttu-id="a96e4-367">В общем случае маршруты с областями следует разместить выше в таблице маршрутов, как они более точным определением, чем маршруты без области.</span><span class="sxs-lookup"><span data-stu-id="a96e4-367">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="a96e4-368">Используя приведенный выше пример, значения маршрута будет соответствовать следующее действие:</span><span class="sxs-lookup"><span data-stu-id="a96e4-368">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="a96e4-369">`AreaAttribute` Является то, что обозначает контроллере в рамках области, говорят, что этот контроллер находится в `Blog` области.</span><span class="sxs-lookup"><span data-stu-id="a96e4-369">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="a96e4-370">Контроллеры без `[Area]` атрибута не являются членами любую область и будет **не** соответствует при `area` маршрута значение предоставляется маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-370">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="a96e4-371">В следующем примере первый контроллер в списке можно сопоставить значения маршрута `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-371">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="a96e4-372">Пространство имен для каждого контроллера показано ниже для полноты - контроллеров должны иметь именования конфликт и выдают ошибку.</span><span class="sxs-lookup"><span data-stu-id="a96e4-372">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="a96e4-373">Класс пространства имен не оказывают влияния на маршрутизации в MVC.</span><span class="sxs-lookup"><span data-stu-id="a96e4-373">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="a96e4-374">Первые два контроллеры являются членами области и соответствовать только обеспечивается их имя соответствующей области `area` значения маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-374">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="a96e4-375">Третий контроллер не является членом любой области, а может только совпадение, если никакого значения для `area` обеспечивается маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="a96e4-375">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="a96e4-376">С точки зрения соответствия *значение не*, отсутствие `area` значение такое же, как если бы значение `area` были null или является пустой строкой.</span><span class="sxs-lookup"><span data-stu-id="a96e4-376">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="a96e4-377">При выполнении действия внутри области, значения маршрута `area` могут быть использованы как *значение окружения* для маршрутизации, используемый для создания URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="a96e4-377">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="a96e4-378">Это означает, что по умолчанию областей выступать в *прикрепленные* для создания URL-адресов, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="a96e4-378">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="a96e4-379">Основные сведения о IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="a96e4-379">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="a96e4-380">Этот раздел содержит подробное на внутренние компоненты инфраструктуры и как MVC выбирает действие для выполнения.</span><span class="sxs-lookup"><span data-stu-id="a96e4-380">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="a96e4-381">Типичное приложение не требуется пользовательский`IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="a96e4-381">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="a96e4-382">Скорее всего уже использовалась `IActionConstraint` даже если вы не знакомы с интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="a96e4-382">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="a96e4-383">`[HttpGet]` Атрибута и аналогичных `[Http-VERB]` атрибуты, реализующие `IActionConstraint` для ограничения выполнения метода действия.</span><span class="sxs-lookup"><span data-stu-id="a96e4-383">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="a96e4-384">При условии, что обычная маршрут по умолчанию URL-адрес `/Products/Edit` приведет к получению значения `{ controller = Products, action = Edit }`, которой будет соответствовать **оба** действий, показано ниже.</span><span class="sxs-lookup"><span data-stu-id="a96e4-384">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="a96e4-385">В `IActionConstraint` терминология, было бы сказать, что оба эти действия относятся к типу кандидатов - оба они соответствуют данные маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-385">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="a96e4-386">При `HttpGetAttribute` выполняется, появится сообщение, *Edit()* соответствует *получить* и не совпадают с HTTP-команду.</span><span class="sxs-lookup"><span data-stu-id="a96e4-386">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="a96e4-387">`Edit(...)` Действие не имеет ограничения, определенные и поэтому будет соответствовать любой HTTP-команду.</span><span class="sxs-lookup"><span data-stu-id="a96e4-387">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="a96e4-388">Поэтому при условии, что `POST` — только `Edit(...)` соответствует.</span><span class="sxs-lookup"><span data-stu-id="a96e4-388">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="a96e4-389">Но для `GET` обоих этих действий можно по-прежнему соответствуют - Однако действие с `IActionConstraint` всегда считается *лучше* чем действие без.</span><span class="sxs-lookup"><span data-stu-id="a96e4-389">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="a96e4-390">Поэтому так как `Edit()` имеет `[HttpGet]` он считается более точным и будет выбрана, если можно сопоставить обоих этих действий.</span><span class="sxs-lookup"><span data-stu-id="a96e4-390">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="a96e4-391">По существу `IActionConstraint` представляет собой форму *перегрузка*, но вместо перегрузка методов с тем же именем, перегрузка между действиями, которые соответствуют же URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="a96e4-391">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="a96e4-392">Атрибут маршрутизации также использует `IActionConstraint` и может привести к действия от различных контроллеров обоих рассматриваемую кандидатов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-392">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="a96e4-393">Реализация IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="a96e4-393">Implementing IActionConstraint</span></span>

<span data-ttu-id="a96e4-394">Самый простой способ реализации `IActionConstraint` необходимо создать класс, производный от `System.Attribute` и поместите его в действия и контроллеры.</span><span class="sxs-lookup"><span data-stu-id="a96e4-394">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="a96e4-395">MVC автоматически обнаруживать любые `IActionConstraint` , которые применяются как атрибуты.</span><span class="sxs-lookup"><span data-stu-id="a96e4-395">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="a96e4-396">Модель приложения можно использовать для применения ограничений, а это, вероятно, самый гибкий подход, поскольку он позволяет metaprogram способ их применения.</span><span class="sxs-lookup"><span data-stu-id="a96e4-396">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="a96e4-397">В следующем примере ограничение выбирает действие на основе *код страны* из данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="a96e4-397">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="a96e4-398">[Полный пример на GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="a96e4-398">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="a96e4-399">Вы отвечаете за реализацию `Accept` метод и выбрав команду «Заказ» на ограничение для выполнения.</span><span class="sxs-lookup"><span data-stu-id="a96e4-399">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="a96e4-400">В этом случае `Accept` возвращает `true` для обозначения действие совпадение при `country` маршрутизации совпадений значение.</span><span class="sxs-lookup"><span data-stu-id="a96e4-400">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="a96e4-401">Это поведение отличается от `RouteValueAttribute` тем, что позволяет резервный в действие без атрибутов.</span><span class="sxs-lookup"><span data-stu-id="a96e4-401">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="a96e4-402">Образец показывает, что если при определении `en-US` действие, а затем код страны, например `fr-FR` вернется к контроллеру более универсальный, у которого нет `[CountrySpecific(...)]` применения.</span><span class="sxs-lookup"><span data-stu-id="a96e4-402">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="a96e4-403">`Order` Свойство принимает решение, какую *этапа* ограничение является частью.</span><span class="sxs-lookup"><span data-stu-id="a96e4-403">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="a96e4-404">Ограничения действия выполняются в группах, на основе `Order`.</span><span class="sxs-lookup"><span data-stu-id="a96e4-404">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="a96e4-405">Например, все платформы получены атрибуты метода HTTP использовать тот же `Order` значение, чтобы они выполняются в том же состоянии.</span><span class="sxs-lookup"><span data-stu-id="a96e4-405">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="a96e4-406">Может быть необходимо для реализации требуемой политик стадий.</span><span class="sxs-lookup"><span data-stu-id="a96e4-406">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="a96e4-407">Чтобы определить значение для `Order` определите, следует ли применять в ограничении до методы HTTP.</span><span class="sxs-lookup"><span data-stu-id="a96e4-407">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="a96e4-408">Более низкие показатели выполняются в первую очередь.</span><span class="sxs-lookup"><span data-stu-id="a96e4-408">Lower numbers run first.</span></span>
