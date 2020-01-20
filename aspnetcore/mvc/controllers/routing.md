---
title: Маршрутизация к действиям контроллера в ASP.NET Core
author: rick-anderson
description: Узнайте, как в MVC ASP.NET Core используется ПО промежуточного слоя маршрутизации для сопоставления URL-адресов входящих запросов с действиями.
ms.author: riande
ms.date: 12/05/2019
uid: mvc/controllers/routing
ms.openlocfilehash: 8cf7e74df292a614f287eff8561a22187f6558ce
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/10/2020
ms.locfileid: "75866063"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="4d888-103">Маршрутизация к действиям контроллера в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d888-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="4d888-104">Авторы: [Райан Новак](https://github.com/rynowak) (Ryan Nowak) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="4d888-104">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4d888-105">В ASP.NET Core MVC используется [ПО промежуточного слоя](xref:fundamentals/middleware/index) маршрутизации для сопоставления URL-адресов входящих запросов с действиями.</span><span class="sxs-lookup"><span data-stu-id="4d888-105">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="4d888-106">Маршруты определяются в коде запуска или атрибутах.</span><span class="sxs-lookup"><span data-stu-id="4d888-106">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="4d888-107">Они описывают то, как пути URL-адресов должны сопоставляться с действиями.</span><span class="sxs-lookup"><span data-stu-id="4d888-107">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="4d888-108">С помощью маршрутов также формируются URL-адреса (для ссылок), отправляемые в ответах.</span><span class="sxs-lookup"><span data-stu-id="4d888-108">Routes are also used to generate URLs (for links) sent out in responses.</span></span>

<span data-ttu-id="4d888-109">Действия маршрутизируются либо на основе соглашений, либо с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-109">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="4d888-110">При добавлении маршрута к контроллеру или действию они становятся маршрутизируемыми с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-110">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="4d888-111">Дополнительные сведения см. в разделе [Смешанная маршрутизация](#routing-mixed-ref-label).</span><span class="sxs-lookup"><span data-stu-id="4d888-111">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="4d888-112">В этом документе описывается взаимодействие между MVC и маршрутизацией и использование возможностей маршрутизации в типичных приложениях MVC.</span><span class="sxs-lookup"><span data-stu-id="4d888-112">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="4d888-113">Подробные сведения о расширенной маршрутизации см. в статье [Маршрутизация](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="4d888-113">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="4d888-114">Настройка ПО промежуточного слоя маршрутизации</span><span class="sxs-lookup"><span data-stu-id="4d888-114">Setting up Routing Middleware</span></span>

<span data-ttu-id="4d888-115">Метод *Configure* содержит код, который выглядит примерно так:</span><span class="sxs-lookup"><span data-stu-id="4d888-115">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="4d888-116">В вызове `UseMvc` метод `MapRoute` используется для создания одного маршрута, который называется маршрутом `default`.</span><span class="sxs-lookup"><span data-stu-id="4d888-116">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="4d888-117">В большинстве приложений MVC используется маршрут с шаблоном, сходным с маршрутом `default`.</span><span class="sxs-lookup"><span data-stu-id="4d888-117">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="4d888-118">Шаблон маршрута `"{controller=Home}/{action=Index}/{id?}"` может сопоставлять путь URL-адреса, например `/Products/Details/5`, и извлекает значения маршрута `{ controller = Products, action = Details, id = 5 }` путем разбивки пути на лексемы.</span><span class="sxs-lookup"><span data-stu-id="4d888-118">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="4d888-119">MVC попытается найти контроллер с именем `ProductsController` и выполнить действие `Details`:</span><span class="sxs-lookup"><span data-stu-id="4d888-119">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="4d888-120">Обратите внимание на то, что в этом примере привязка модели будет использовать значение `id = 5`, чтобы присвоить параметру `id` значение `5` при вызове действия.</span><span class="sxs-lookup"><span data-stu-id="4d888-120">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="4d888-121">Дополнительные сведения см. в разделе [Привязка модели](../models/model-binding.md).</span><span class="sxs-lookup"><span data-stu-id="4d888-121">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="4d888-122">Использование маршрута `default`:</span><span class="sxs-lookup"><span data-stu-id="4d888-122">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="4d888-123">Шаблон маршрута:</span><span class="sxs-lookup"><span data-stu-id="4d888-123">The route template:</span></span>

* <span data-ttu-id="4d888-124">`{controller=Home}` определяет `Home` в качестве объекта `controller` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4d888-124">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="4d888-125">`{action=Index}` определяет `Index` в качестве объекта `action` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4d888-125">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="4d888-126">`{id?}` определяет необязательный параметр `id`.</span><span class="sxs-lookup"><span data-stu-id="4d888-126">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="4d888-127">Параметры маршрута по умолчанию и необязательные параметры необязательно должны присутствовать в пути URL-адреса для сопоставления.</span><span class="sxs-lookup"><span data-stu-id="4d888-127">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="4d888-128">Подробное описание синтаксиса шаблона маршрута см. в разделе [Справочник по шаблону маршрута](../../fundamentals/routing.md#route-template-reference).</span><span class="sxs-lookup"><span data-stu-id="4d888-128">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="4d888-129">`"{controller=Home}/{action=Index}/{id?}"` может сопоставляться с путем URL-адреса `/` и выдает значения маршрута `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="4d888-129">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="4d888-130">Для объектов `controller` и `action` используются значения по умолчанию. `id` не имеет значения, так как в пути URL-адреса нет соответствующего сегмента.</span><span class="sxs-lookup"><span data-stu-id="4d888-130">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="4d888-131">MVC будет использовать эти значения маршрута для выбора действия `HomeController` и `Index`:</span><span class="sxs-lookup"><span data-stu-id="4d888-131">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="4d888-132">При использовании этого определения контроллера и шаблона маршрута действие `HomeController.Index` будет выполняться для любых из следующих путей URL-адресов:</span><span class="sxs-lookup"><span data-stu-id="4d888-132">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="4d888-133">Универсальный метод `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="4d888-133">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="4d888-134">Можно использовать вместо следующего метода:</span><span class="sxs-lookup"><span data-stu-id="4d888-134">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="4d888-135">`UseMvc` и `UseMvcWithDefaultRoute` добавляют экземпляр `RouterMiddleware` в конвейер ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="4d888-135">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="4d888-136">MVC не взаимодействует с ПО промежуточного слоя напрямую, а использует маршрутизацию для обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="4d888-136">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="4d888-137">MVC подключается к маршрутам посредством экземпляра `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="4d888-137">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="4d888-138">Код метода `UseMvc` имеет примерно следующий вид:</span><span class="sxs-lookup"><span data-stu-id="4d888-138">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="4d888-139">Метод `UseMvc` не определяет маршруты напрямую, а добавляет заполнитель в коллекцию маршрутов для маршрута `attribute`.</span><span class="sxs-lookup"><span data-stu-id="4d888-139">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="4d888-140">Перегрузка `UseMvc(Action<IRouteBuilder>)` позволяет добавлять собственные маршруты, а также поддерживает маршрутизацию с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-140">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="4d888-141">Метод `UseMvc` и все его варианты добавляют заполнитель для маршрута на основе атрибутов. Маршрутизация с помощью атрибутов доступна всегда вне зависимости от того, как настроен метод `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="4d888-141">`UseMvc` and all of its variations add a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="4d888-142">Метод `UseMvcWithDefaultRoute` определяет маршрут по умолчанию и поддерживает маршрутизацию с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-142">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="4d888-143">В разделе [Маршрутизация с помощью атрибутов](#attribute-routing-ref-label) приводятся более подробные сведения о маршрутизации с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-143">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="4d888-144">Маршрутизация на основе соглашений</span><span class="sxs-lookup"><span data-stu-id="4d888-144">Conventional routing</span></span>

<span data-ttu-id="4d888-145">Маршрут `default`:</span><span class="sxs-lookup"><span data-stu-id="4d888-145">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="4d888-146">является примером *маршрутизации на основе соглашений*.</span><span class="sxs-lookup"><span data-stu-id="4d888-146">is an example of a *conventional routing*.</span></span> <span data-ttu-id="4d888-147">Такой стиль называется *маршрутизацией на основе соглашений* по той причине, что он предполагает определение *соглашения* для путей URL-адресов:</span><span class="sxs-lookup"><span data-stu-id="4d888-147">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="4d888-148">первый сегмент пути сопоставляется с именем контроллера;</span><span class="sxs-lookup"><span data-stu-id="4d888-148">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="4d888-149">второй сегмент сопоставляется с именем действия;</span><span class="sxs-lookup"><span data-stu-id="4d888-149">the second maps to the action name.</span></span>

* <span data-ttu-id="4d888-150">третий сегмент используется для необязательного параметра `id`, который сопоставляется с сущностью модели.</span><span class="sxs-lookup"><span data-stu-id="4d888-150">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="4d888-151">При использовании этого маршрута `default` путь URL-адреса `/Products/List` сопоставляется с действием `ProductsController.List`, а путь `/Blog/Article/17` сопоставляется с `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="4d888-151">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="4d888-152">Такое сопоставление основывается **только** на именах контроллера и действия, но не на пространствах имен, расположениях исходных файлов или параметрах метода.</span><span class="sxs-lookup"><span data-stu-id="4d888-152">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="4d888-153">Использование маршрутизации на основе соглашений с маршрутом по умолчанию позволяет быстро создавать приложение, не придумывая новый шаблон URL-адреса для каждого определяемого действия.</span><span class="sxs-lookup"><span data-stu-id="4d888-153">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="4d888-154">В случае с приложением с действиями в стиле CRUD единообразие URL-адресов для всех контроллеров позволяет упростить код и сделать пользовательский интерфейс более предсказуемым.</span><span class="sxs-lookup"><span data-stu-id="4d888-154">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="4d888-155">Параметр `id` определяется в шаблоне маршрута как необязательный. Это означает, что действия могут выполняться, даже если идентификатор не указан в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="4d888-155">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="4d888-156">Как правило, если параметр `id` отсутствует в URL-адресе, привязка модели присваивает ему значение `0`, и в результате в базе данных не будет найдена сущность, соответствующая `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="4d888-156">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="4d888-157">Маршрутизация с помощью атрибутов обеспечивает детальный контроль, позволяя настраивать идентификатор как обязательный лишь для некоторых действий.</span><span class="sxs-lookup"><span data-stu-id="4d888-157">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="4d888-158">В документации необязательные параметры, такие как `id`, будут включаться, только если они, скорее всего, могут использоваться в соответствующей ситуации.</span><span class="sxs-lookup"><span data-stu-id="4d888-158">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="4d888-159">Несколько маршрутов</span><span class="sxs-lookup"><span data-stu-id="4d888-159">Multiple routes</span></span>

<span data-ttu-id="4d888-160">В метод `UseMvc` можно добавить несколько маршрутов, добавив дополнительные вызовы `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="4d888-160">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="4d888-161">Таким образом можно определить несколько соглашений или добавить маршруты на основе соглашений, предназначенные для определенного действия, например:</span><span class="sxs-lookup"><span data-stu-id="4d888-161">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="4d888-162">Маршрут `blog` здесь — это *выделенный маршрут на основе соглашения*. Это означает, что он использует систему маршрутизации на основе соглашений, но предназначен для определенного действия.</span><span class="sxs-lookup"><span data-stu-id="4d888-162">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="4d888-163">Так как параметры `controller` и `action` отсутствуют в шаблоне маршрута, они могут иметь только значения по умолчанию, поэтому этот маршрут всегда будет сопоставляться с действием `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="4d888-163">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="4d888-164">Маршруты в коллекции маршрутов упорядочены и обрабатываются в порядке добавления.</span><span class="sxs-lookup"><span data-stu-id="4d888-164">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="4d888-165">Поэтому в этом примере маршрут `blog` будет проверяться перед маршрутом `default`.</span><span class="sxs-lookup"><span data-stu-id="4d888-165">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="4d888-166">В *выделенных маршрутах на основе соглашений* часто используются обобщающие параметры, такие как `{*article}`, для фиксации оставшейся части пути URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="4d888-166">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="4d888-167">Из-за этого маршрут может оказаться слишком универсальным, то есть он будет соответствовать URL-адресам, которые должны сопоставляться с другими маршрутами.</span><span class="sxs-lookup"><span data-stu-id="4d888-167">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="4d888-168">Чтобы избежать этой проблемы, такие универсальные маршруты следует помещать в конце таблицы маршрутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-168">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="4d888-169">Откат</span><span class="sxs-lookup"><span data-stu-id="4d888-169">Fallback</span></span>

<span data-ttu-id="4d888-170">В ходе обработки запроса MVC проверяет, можно ли найти контроллер и действие в приложении с помощью предоставленных значений маршрута.</span><span class="sxs-lookup"><span data-stu-id="4d888-170">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="4d888-171">Если нет действия, соответствующего значениям, маршрут считается не сопоставленным, и проверяется следующий маршрут.</span><span class="sxs-lookup"><span data-stu-id="4d888-171">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="4d888-172">Этот процесс называется *откатом* и призван упростить обработку случаев, когда маршруты на основе соглашений перекрываются.</span><span class="sxs-lookup"><span data-stu-id="4d888-172">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="4d888-173">Разрешение неоднозначности действий</span><span class="sxs-lookup"><span data-stu-id="4d888-173">Disambiguating actions</span></span>

<span data-ttu-id="4d888-174">Если при маршрутизации найдены два соответствующих действия, платформа MVC должна устранить неоднозначность, выбрав наиболее подходящее из них, или создать исключение.</span><span class="sxs-lookup"><span data-stu-id="4d888-174">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="4d888-175">Пример:</span><span class="sxs-lookup"><span data-stu-id="4d888-175">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="4d888-176">Этот контроллер определяет два действия, которые соответствуют пути URL-адреса `/Products/Edit/17` и маршрутизируют данные `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="4d888-176">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="4d888-177">Это типичный шаблон для контроллеров MVC, в котором метод `Edit(int)` отображает форму для изменения сведений о продукте, а метод `Edit(int, Product)` обрабатывает отправленную форму.</span><span class="sxs-lookup"><span data-stu-id="4d888-177">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="4d888-178">Чтобы это было возможным, платформа MVC должна выбрать `Edit(int, Product)` для HTTP-запроса `POST` и `Edit(int)` для любой другой HTTP-команды.</span><span class="sxs-lookup"><span data-stu-id="4d888-178">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="4d888-179">`HttpPostAttribute` (`[HttpPost]`) — это реализация интерфейса `IActionConstraint`, которая позволяет выбрать действие, только если HTTP-команда — `POST`.</span><span class="sxs-lookup"><span data-stu-id="4d888-179">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="4d888-180">Наличие интерфейса `IActionConstraint` делает метод `Edit(int, Product)` более подходящим вариантом, чем `Edit(int)`, поэтому `Edit(int, Product)` будет проверяться первым.</span><span class="sxs-lookup"><span data-stu-id="4d888-180">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="4d888-181">Пользовательские реализации `IActionConstraint` потребуется создавать только в особых ситуациях, однако важно понимать роль таких атрибутов, как `HttpPostAttribute`, — аналогичные атрибуты определены для других HTTP-команд.</span><span class="sxs-lookup"><span data-stu-id="4d888-181">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="4d888-182">При маршрутизации на основе соглашений для действий часто используются одинаковые имена в рамках рабочего процесса `show form -> submit form`.</span><span class="sxs-lookup"><span data-stu-id="4d888-182">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="4d888-183">Удобство такого шаблона станет очевидным после ознакомления с разделом [Сведения об интерфейсе IActionConstraint](#understanding-iactionconstraint).</span><span class="sxs-lookup"><span data-stu-id="4d888-183">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="4d888-184">Если найдено несколько совпадений и MVC не может определить наиболее подходящий маршрут, создается исключение `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="4d888-184">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="4d888-185">Имена маршрутов</span><span class="sxs-lookup"><span data-stu-id="4d888-185">Route names</span></span>

<span data-ttu-id="4d888-186">Строки `"blog"` и `"default"` в следующих примерах представляют собой имена маршрутов:</span><span class="sxs-lookup"><span data-stu-id="4d888-186">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="4d888-187">Имя маршрута — это логическое имя, которое позволяет использовать именованный маршрут для формирования URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="4d888-187">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="4d888-188">Имена значительно упрощают создание URL-адресов, если оно представляет сложность из-за порядка маршрутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-188">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="4d888-189">Имена маршрутов должны быть уникальными в пределах приложения.</span><span class="sxs-lookup"><span data-stu-id="4d888-189">Route names must be unique application-wide.</span></span>

<span data-ttu-id="4d888-190">Имена маршрутов не влияют на сопоставление URL-адресов или обработку запросов; они служат только для формирования URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="4d888-190">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="4d888-191">В статье [Маршрутизация](xref:fundamentals/routing) приводятся более подробные сведения о формировании URL-адресов, в том числе во вспомогательных объектах MVC.</span><span class="sxs-lookup"><span data-stu-id="4d888-191">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="4d888-192">Маршрутизация с помощью атрибутов</span><span class="sxs-lookup"><span data-stu-id="4d888-192">Attribute routing</span></span>

<span data-ttu-id="4d888-193">При маршрутизации с помощью атрибутов используется набор атрибутов для сопоставления действий непосредственно с шаблонами маршрутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-193">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="4d888-194">В приведенном ниже примере в методе `Configure` используется `app.UseMvc();`, и маршрут не передается.</span><span class="sxs-lookup"><span data-stu-id="4d888-194">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="4d888-195">`HomeController` будет соответствовать набору URL-адресов, аналогичных тем, которым соответствует маршрут по умолчанию `{controller=Home}/{action=Index}/{id?}`:</span><span class="sxs-lookup"><span data-stu-id="4d888-195">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="4d888-196">Действие `HomeController.Index()` будет выполняться для любого из путей URL-адресов `/`, `/Home` или `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="4d888-196">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="4d888-197">В этом примере показано ключевое различие между маршрутизацией с помощью атрибутов и маршрутизацией на основе соглашений.</span><span class="sxs-lookup"><span data-stu-id="4d888-197">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="4d888-198">При использовании маршрутизации с помощью атрибутов для указания маршрута требуется больше входных данных; маршрут по умолчанию на основе соглашения более лаконичен.</span><span class="sxs-lookup"><span data-stu-id="4d888-198">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="4d888-199">Однако маршрутизация с помощью атрибутов обеспечивает более точный контроль над тем, какие шаблоны маршрутов применяются к каждому действию, и требует такого контроля.</span><span class="sxs-lookup"><span data-stu-id="4d888-199">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="4d888-200">В случае с маршрутизацией с помощью атрибутов имя контроллера и имена действий **не** имеют значения при выборе действия.</span><span class="sxs-lookup"><span data-stu-id="4d888-200">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="4d888-201">В этом примере сопоставление будет производиться с теми же URL-адресами, что и в предыдущем.</span><span class="sxs-lookup"><span data-stu-id="4d888-201">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="4d888-202">Приведенные выше шаблоны маршрутов не определяют параметры маршрутов для `action`, `area` и `controller`.</span><span class="sxs-lookup"><span data-stu-id="4d888-202">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="4d888-203">По сути, эти параметры маршрутов не разрешены в маршрутах на основе атрибутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-203">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="4d888-204">Так как шаблон маршрута уже связан с действием, не имеет смысла анализировать имя действия в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="4d888-204">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="4d888-205">Маршрутизация с помощью атрибутов Http[Verb]</span><span class="sxs-lookup"><span data-stu-id="4d888-205">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="4d888-206">При маршрутизации с помощью атрибутов также могут использоваться атрибуты `Http[Verb]`, такие как `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="4d888-206">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="4d888-207">Все эти атрибуты могут принимать шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="4d888-207">All of these attributes can accept a route template.</span></span> <span data-ttu-id="4d888-208">В этом примере показаны два действия, которые совпадают с одним шаблоном маршрута:</span><span class="sxs-lookup"><span data-stu-id="4d888-208">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="4d888-209">Для такого пути URL-адреса, как `/products`, действие `ProductsApi.ListProducts` будет выполнено, если HTTP-командой является `GET`, а действие `ProductsApi.CreateProduct` будет выполнено, если HTTP-командой является `POST`.</span><span class="sxs-lookup"><span data-stu-id="4d888-209">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="4d888-210">При маршрутизации с помощью атрибутов URL-адрес сначала сопоставляется с набором шаблоном маршрутов, определяемых атрибутами маршрутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-210">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="4d888-211">После того как найден соответствующий шаблон маршрута, применяются ограничения `IActionConstraint` для определения действий, которые могут быть выполнены.</span><span class="sxs-lookup"><span data-stu-id="4d888-211">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="4d888-212">При разработке REST API редко приходится использовать `[Route(...)]` для метода действия, так как действие принимает все методы HTTP.</span><span class="sxs-lookup"><span data-stu-id="4d888-212">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method as the action will accept all HTTP methods.</span></span> <span data-ttu-id="4d888-213">Предпочтительнее применять более конкретные атрибуты `Http*Verb*Attributes`, чтобы точно определить поддерживаемые API возможности.</span><span class="sxs-lookup"><span data-stu-id="4d888-213">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="4d888-214">Клиенты интерфейсов REST API должны знать, какие пути и HTTP-команды сопоставляются с определенными логическими операциями.</span><span class="sxs-lookup"><span data-stu-id="4d888-214">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="4d888-215">Так как маршрут на основе атрибутов применяется к определенному действию, можно легко сделать параметры обязательными в рамках определения шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="4d888-215">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="4d888-216">В этом примере параметр `id` является обязательным в пути URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="4d888-216">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="4d888-217">Действие `ProductsApi.GetProduct(int)` будет выполнено для такого пути URL-адреса, как `/products/3`, но не для такого пути URL-адреса, как `/products`.</span><span class="sxs-lookup"><span data-stu-id="4d888-217">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="4d888-218">Полное описание шаблонов маршрутов и связанных параметров см. в статье [Маршрутизация](../../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="4d888-218">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="4d888-219">Имя маршрута</span><span class="sxs-lookup"><span data-stu-id="4d888-219">Route Name</span></span>

<span data-ttu-id="4d888-220">В следующем коде определяется *имя маршрута*`Products_List`:</span><span class="sxs-lookup"><span data-stu-id="4d888-220">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="4d888-221">Имена маршрутов могут использоваться для формирования URL-адреса на основе определенного маршрута.</span><span class="sxs-lookup"><span data-stu-id="4d888-221">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="4d888-222">Они не влияют на то, как производится сопоставление URL-адресов при маршрутизации, и служат только для формирования URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="4d888-222">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="4d888-223">Имена маршрутов должны быть уникальными в пределах приложения.</span><span class="sxs-lookup"><span data-stu-id="4d888-223">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="4d888-224">Сравните это поведение с *маршрутом по умолчанию* на основе соглашения, в котором параметр `id` определяется как необязательный (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="4d888-224">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="4d888-225">Такая возможность точно указывать интерфейсы API имеет преимущества. Например, она позволяет направлять `/products` и `/products/5` в разные действия.</span><span class="sxs-lookup"><span data-stu-id="4d888-225">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="4d888-226">Объединение маршрутов</span><span class="sxs-lookup"><span data-stu-id="4d888-226">Combining routes</span></span>

<span data-ttu-id="4d888-227">Чтобы избежать лишних повторов при маршрутизации с помощью атрибутов, атрибуты маршрута для контроллера объединяются с атрибутами маршрута для отдельных действий.</span><span class="sxs-lookup"><span data-stu-id="4d888-227">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="4d888-228">Все шаблоны маршрутов, определенные в контроллере, добавляются перед шаблонами маршрутов для действий.</span><span class="sxs-lookup"><span data-stu-id="4d888-228">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="4d888-229">В результате добавления атрибута маршрута для контроллера **все** его действия будут использовать маршрутизацию с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-229">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="4d888-230">В этом примере путь URL-адреса `/products` может соответствовать `ProductsApi.ListProducts`, а путь URL-адреса `/products/5` — `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="4d888-230">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="4d888-231">Оба эти действия соответствуют только HTTP-запросу `GET`, так как они помечены атрибутом `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="4d888-231">Both of these actions only match HTTP `GET` because they're marked with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="4d888-232">Шаблоны маршрутов, применяемые к действию, которое начинается с символа `/` или `~/`, не объединяются с шаблонами маршрутов, применяемыми к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="4d888-232">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="4d888-233">Этот пример соответствует такому же набору путей URL-адресов, что и *маршрут по умолчанию*.</span><span class="sxs-lookup"><span data-stu-id="4d888-233">This example matches a set of URL paths similar to the *default route*.</span></span>

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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="4d888-234">Упорядочение маршрутов на основе атрибутов</span><span class="sxs-lookup"><span data-stu-id="4d888-234">Ordering attribute routes</span></span>

<span data-ttu-id="4d888-235">В отличие от маршрутов на основе соглашений, которые выполняются в определенном порядке, для маршрутизации с помощью атрибутов формируется дерево, и все маршруты сопоставляются одновременно.</span><span class="sxs-lookup"><span data-stu-id="4d888-235">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="4d888-236">Это равносильно тому, как если бы записи маршрутов находились в идеальном порядке: наиболее конкретные маршруты имеют возможность выполнения перед более общими.</span><span class="sxs-lookup"><span data-stu-id="4d888-236">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="4d888-237">Например, такой маршрут, как `blog/search/{topic}`, является более конкретным по сравнению с `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="4d888-237">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="4d888-238">С логической точки зрения, маршрут `blog/search/{topic}` по умолчанию выполняется первым, так как это единственная разумная очередность.</span><span class="sxs-lookup"><span data-stu-id="4d888-238">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="4d888-239">При использовании маршрутизации на основе соглашений разработчик отвечает за расположение маршрутов в нужном порядке.</span><span class="sxs-lookup"><span data-stu-id="4d888-239">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="4d888-240">При маршрутизации с помощью атрибутов порядок может настраиваться с помощью свойства `Order` всех атрибутов маршрутов, предоставляемых платформой.</span><span class="sxs-lookup"><span data-stu-id="4d888-240">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="4d888-241">Маршруты обрабатываются в порядке возрастания значения свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="4d888-241">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="4d888-242">Порядок по умолчанию — `0`.</span><span class="sxs-lookup"><span data-stu-id="4d888-242">The default order is `0`.</span></span> <span data-ttu-id="4d888-243">Маршрут, для которого задано значение `Order = -1`, будет выполняться перед маршрутами, для которых порядок не задан.</span><span class="sxs-lookup"><span data-stu-id="4d888-243">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="4d888-244">Маршрут, для которого задано значение `Order = 1`, будет выполняться после маршрутов с порядком по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4d888-244">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="4d888-245">Старайтесь не использовать свойство `Order`.</span><span class="sxs-lookup"><span data-stu-id="4d888-245">Avoid depending on `Order`.</span></span> <span data-ttu-id="4d888-246">Если для правильной маршрутизации в пространстве URL-адресов требуются явно заданные значения порядка, скорее всего, это будет вызывать путаницу и в среде клиентов.</span><span class="sxs-lookup"><span data-stu-id="4d888-246">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="4d888-247">Как правило, при маршрутизации с помощью атрибутов правильный маршрут выбирается посредством сопоставления URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="4d888-247">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="4d888-248">Если порядок по умолчанию для формирования URL-адресов не работает, использовать имя маршрута в качестве переопределения, как правило, проще, чем применять свойство `Order`.</span><span class="sxs-lookup"><span data-stu-id="4d888-248">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="4d888-249">Средства маршрутизации в Razor Pages и контроллере MVC имеют общую реализацию.</span><span class="sxs-lookup"><span data-stu-id="4d888-249">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="4d888-250">Сведения о порядке маршрутизации в Razor Pages см. в статье [Razor Pages route and app conventions in ASP.NET Core](xref:razor-pages/razor-pages-conventions#route-order) (Соглашения для маршрутизации и приложений Razor Pages в ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="4d888-250">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="4d888-251">Замена токенов в шаблонах маршрутов ([controller], [action], [area])</span><span class="sxs-lookup"><span data-stu-id="4d888-251">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="4d888-252">Для удобства маршрута на основе атрибутов поддерживают *замену токенов* путем заключения токена в квадратные скобки (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="4d888-252">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="4d888-253">Токены `[action]`, `[area]` и `[controller]` заменяются значениями имени действия, имени области и имени контроллера из действия, в котором определен маршрут.</span><span class="sxs-lookup"><span data-stu-id="4d888-253">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="4d888-254">В следующем примере действия могут соответствовать путям URL-адресов, как описано в комментариях:</span><span class="sxs-lookup"><span data-stu-id="4d888-254">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="4d888-255">Замена токенов происходит на последнем этапе создания маршрутов на основе атрибутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-255">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="4d888-256">Приведенный выше пример будет работать так же, как следующий код:</span><span class="sxs-lookup"><span data-stu-id="4d888-256">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="4d888-257">Маршруты на основе атрибутов могут также сочетаться с наследованием.</span><span class="sxs-lookup"><span data-stu-id="4d888-257">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="4d888-258">Эта возможность особенно эффективна при использовании вместе с заменой токенов.</span><span class="sxs-lookup"><span data-stu-id="4d888-258">This is particularly powerful combined with token replacement.</span></span>

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

<span data-ttu-id="4d888-259">Замена токенов также применяется к именам маршрутов, определенным в маршрутах на основе атрибутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-259">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="4d888-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` создает уникальное имя маршрута для каждого действия.</span><span class="sxs-lookup"><span data-stu-id="4d888-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="4d888-261">Для сопоставления с литеральным разделителем замены токенов `[` или `]` его следует экранировать путем повтора символа (`[[` или `]]`).</span><span class="sxs-lookup"><span data-stu-id="4d888-261">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker range=">= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="4d888-262">Использование преобразователя параметров для настройки замены токенов</span><span class="sxs-lookup"><span data-stu-id="4d888-262">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="4d888-263">Замену токенов можно настроить, используя преобразователь параметров.</span><span class="sxs-lookup"><span data-stu-id="4d888-263">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="4d888-264">Преобразователь параметров реализует `IOutboundParameterTransformer` и преобразует значения параметров.</span><span class="sxs-lookup"><span data-stu-id="4d888-264">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="4d888-265">Например, пользовательский преобразователь параметра `SlugifyParameterTransformer` изменяет значение маршрута `SubscriptionManagement` на `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="4d888-265">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="4d888-266">`RouteTokenTransformerConvention` является соглашением для модели приложения, которое:</span><span class="sxs-lookup"><span data-stu-id="4d888-266">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="4d888-267">Применяет преобразователь параметров ко всем маршрутам атрибута в приложении.</span><span class="sxs-lookup"><span data-stu-id="4d888-267">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="4d888-268">Настраивает значения токена маршрут атрибута при замене.</span><span class="sxs-lookup"><span data-stu-id="4d888-268">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="4d888-269">`RouteTokenTransformerConvention` регистрируется в качестве параметра в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4d888-269">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

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

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="4d888-270">Несколько маршрутов</span><span class="sxs-lookup"><span data-stu-id="4d888-270">Multiple Routes</span></span>

<span data-ttu-id="4d888-271">Маршрутизация с помощью атрибутов поддерживает определение нескольких маршрутов к одному и тому же действию.</span><span class="sxs-lookup"><span data-stu-id="4d888-271">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="4d888-272">Наиболее распространенный случай использования этой возможности — имитация поведения *маршрута по умолчанию на основе соглашения*, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="4d888-272">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="4d888-273">Добавление нескольких атрибутов маршрута для контроллера означает, что каждый из них будет объединяться с каждым из атрибутов маршрута, определенных для методов действий.</span><span class="sxs-lookup"><span data-stu-id="4d888-273">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="4d888-274">Если несколько атрибутов маршрута (реализующих интерфейс `IActionConstraint`) добавлены для действия, каждое ограничение действия объединяется с шаблоном маршрута из атрибута, в котором оно определено.</span><span class="sxs-lookup"><span data-stu-id="4d888-274">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="4d888-275">Хотя использование нескольких маршрутов к действиям может показаться очень эффективной возможностью, лучше, чтобы пространство URL-адресов приложения оставалось простым и четко организованным.</span><span class="sxs-lookup"><span data-stu-id="4d888-275">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="4d888-276">Используйте несколько маршрутов к действиям только там, где это необходимо, например для поддержки существующих клиентов.</span><span class="sxs-lookup"><span data-stu-id="4d888-276">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="4d888-277">Задание необязательных параметров, значений по умолчанию и ограничений для маршрутов на основе атрибутов</span><span class="sxs-lookup"><span data-stu-id="4d888-277">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="4d888-278">Маршруты на основе атрибутов поддерживают тот же синтаксис для указания необязательных параметров, значений по умолчанию и ограничений, что и маршруты на основе соглашений.</span><span class="sxs-lookup"><span data-stu-id="4d888-278">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="4d888-279">Подробное описание синтаксиса шаблона маршрута см. в разделе [Справочник по шаблону маршрута](../../fundamentals/routing.md#route-template-reference).</span><span class="sxs-lookup"><span data-stu-id="4d888-279">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="4d888-280">Пользовательские атрибуты маршрута с использованием `IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="4d888-280">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="4d888-281">Все атрибуты маршрутов, предоставляемые платформой ( `[Route(...)]`, `[HttpGet(...)]` и т. д.), реализуют интерфейс `IRouteTemplateProvider`.</span><span class="sxs-lookup"><span data-stu-id="4d888-281">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="4d888-282">MVC ищет атрибуты в классах контроллеров и методах действий при запуске приложения и использует те из них, которые реализуют интерфейс `IRouteTemplateProvider`, для формирования начального набора маршрутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-282">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="4d888-283">Вы можете реализовать интерфейс `IRouteTemplateProvider` для определения собственных атрибутов маршрутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-283">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="4d888-284">Каждая реализация `IRouteTemplateProvider` позволяет определить один маршрут с пользовательским шаблоном маршрута, порядком и именем.</span><span class="sxs-lookup"><span data-stu-id="4d888-284">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="4d888-285">Атрибут в приведенном выше примере автоматически присваивает шаблону `Template` значение `"api/[controller]"` при применении `[MyApiController]`.</span><span class="sxs-lookup"><span data-stu-id="4d888-285">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="4d888-286">Настройка маршрутов на основе атрибутов с помощью модели приложения</span><span class="sxs-lookup"><span data-stu-id="4d888-286">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="4d888-287">*Модель приложения* — это объектная модель, которая создается при запуске со всеми метаданными, используемыми платформой MVC для маршрутизации и выполнения действий.</span><span class="sxs-lookup"><span data-stu-id="4d888-287">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="4d888-288">*Модель приложения* включает в себя все данные, собранные из атрибутов маршрутов (посредством интерфейса `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="4d888-288">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="4d888-289">Вы можете создать *соглашения*, чтобы изменить модель приложения при запуске с целью настройки маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="4d888-289">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="4d888-290">В этом разделе приводится простой пример настройки маршрутизации с помощью модели приложения.</span><span class="sxs-lookup"><span data-stu-id="4d888-290">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="4d888-291">Смешанная маршрутизация с помощью атрибутов и маршрутизация на основе соглашений</span><span class="sxs-lookup"><span data-stu-id="4d888-291">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="4d888-292">В приложениях MVC маршрутизация с помощью атрибутов и маршрутизация на основе соглашений могут использоваться вместе.</span><span class="sxs-lookup"><span data-stu-id="4d888-292">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="4d888-293">Маршруты на основе соглашений часто применяются для контроллеров, предоставляющих HTML-страницы для браузеров, а маршруты на основе атрибутов — для контроллеров, предоставляющих интерфейсы REST API.</span><span class="sxs-lookup"><span data-stu-id="4d888-293">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="4d888-294">Действия маршрутизируются либо на основе соглашений, либо с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-294">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="4d888-295">При добавлении маршрута к контроллеру или действию они становятся маршрутизируемыми с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="4d888-295">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="4d888-296">Действия, определяющие маршруты на основе атрибутов, недоступны по маршрутам на основе соглашений, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="4d888-296">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="4d888-297">**Любой** атрибут маршрута контроллера делает все действия в атрибуте контроллера маршрутизируемыми.</span><span class="sxs-lookup"><span data-stu-id="4d888-297">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="4d888-298">Два типа систем маршрутизации отличает процесс, выполняемый после нахождения URL-адреса, соответствующего шаблону маршрута.</span><span class="sxs-lookup"><span data-stu-id="4d888-298">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="4d888-299">При маршрутизации на основе соглашений значения маршрута из соответствия используются для выбора действия и контроллера из таблицы подстановки, содержащей все действия с маршрутизацией на основе соглашений.</span><span class="sxs-lookup"><span data-stu-id="4d888-299">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="4d888-300">При маршрутизации с помощью атрибутов каждый шаблон уже связан с действием, и дальнейшая подстановка не требуется.</span><span class="sxs-lookup"><span data-stu-id="4d888-300">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="4d888-301">Сложные сегменты</span><span class="sxs-lookup"><span data-stu-id="4d888-301">Complex segments</span></span>

<span data-ttu-id="4d888-302">Сложные сегменты (например, `[Route("/dog{token}cat")]`) обрабатываются путем "нежадного" сопоставления литералов справа налево.</span><span class="sxs-lookup"><span data-stu-id="4d888-302">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="4d888-303">Описание см. в [исходном коде](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296).</span><span class="sxs-lookup"><span data-stu-id="4d888-303">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="4d888-304">Дополнительные сведения см. в [этой проблеме](https://github.com/aspnet/AspNetCore.Docs/issues/8197).</span><span class="sxs-lookup"><span data-stu-id="4d888-304">For more information, see [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="4d888-305">Формирование URL-адреса</span><span class="sxs-lookup"><span data-stu-id="4d888-305">URL Generation</span></span>

<span data-ttu-id="4d888-306">Приложения MVC могут использовать функции формирования URL-адреса, предоставляемые системой маршрутизации, для создания URL-ссылок на действия.</span><span class="sxs-lookup"><span data-stu-id="4d888-306">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="4d888-307">Формирование URL-адресов устраняет необходимость в их жестком задании, что делает код надежнее и проще в обслуживании.</span><span class="sxs-lookup"><span data-stu-id="4d888-307">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="4d888-308">В этом разделе рассматриваются функции формирования URL-адреса, предоставляемые платформой MVC, и описываются лишь базовые принципы их работы.</span><span class="sxs-lookup"><span data-stu-id="4d888-308">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="4d888-309">Подробное описание формирования URL-адреса см. в статье [Маршрутизация](../../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="4d888-309">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="4d888-310">Интерфейс `IUrlHelper` — это базовый компонент инфраструктуры, обеспечивающий взаимодействие между MVC и системой маршрутизации для формирования URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="4d888-310">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="4d888-311">Доступ к экземпляру `IUrlHelper` в контроллерах, представлениях и компонентах представлений можно получить посредством свойства `Url`.</span><span class="sxs-lookup"><span data-stu-id="4d888-311">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="4d888-312">В этом примере интерфейс `IUrlHelper` используется посредством свойства `Controller.Url` для формирования URL-адреса другого действия.</span><span class="sxs-lookup"><span data-stu-id="4d888-312">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="4d888-313">Если в приложении применяется маршрут по умолчанию на основе соглашения, значением переменной `url` будет строка с путем URL-адреса `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="4d888-313">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="4d888-314">Этот путь URL-адреса создается системой маршрутизации путем объединения значений маршрута из текущего запроса (значения окружения) со значениями, переданными в `Url.Action`, и подстановки этих значений в шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="4d888-314">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="4d888-315">Значение каждого параметра маршрута в шаблоне маршрута заменяется соответствующими именами со значениями и значениями окружения.</span><span class="sxs-lookup"><span data-stu-id="4d888-315">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="4d888-316">Параметр маршрута, у которого нет значения, может принимать значение по умолчанию, если оно имеется, или пропускаться, если он является необязательным (как в случае с параметром `id` в этом примере).</span><span class="sxs-lookup"><span data-stu-id="4d888-316">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="4d888-317">Сформировать URL-адрес не удастся, если у любого из обязательных параметров маршрута не будет соответствующего значения.</span><span class="sxs-lookup"><span data-stu-id="4d888-317">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="4d888-318">Если для маршрута не удалось сформировать URL-адрес, проверяется следующий маршрут, пока не будут проверены все маршруты или не будет найдено соответствие.</span><span class="sxs-lookup"><span data-stu-id="4d888-318">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="4d888-319">В примере `Url.Action` выше предполагается использование маршрутизации на основе соглашений, однако формирование URL-адреса происходит аналогичным образом и в случае с маршрутизацией с помощью атрибутов, хотя принципы иные.</span><span class="sxs-lookup"><span data-stu-id="4d888-319">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="4d888-320">При применении маршрутизации на основе соглашений шаблон расширяется с помощью значений маршрута, и значения маршрута для `controller` и `action` обычно находятся в этом шаблоне. Это возможно по той причине, что URL-адреса, сопоставленные системой маршрутизации, следуют *соглашению*.</span><span class="sxs-lookup"><span data-stu-id="4d888-320">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="4d888-321">При маршрутизации с помощью атрибутов значения маршрута для `controller` и `action` не могут находиться в шаблоне. Вместо этого они применяются для поиска нужного шаблона.</span><span class="sxs-lookup"><span data-stu-id="4d888-321">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="4d888-322">В этом примере используется маршрутизация с помощью атрибутов:</span><span class="sxs-lookup"><span data-stu-id="4d888-322">This example uses attribute routing:</span></span>

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="4d888-323">MVC создает таблицу подстановки, содержащую все действия с маршрутизацией с помощью атрибутов, и сопоставляет значения `controller` и `action` для выбора шаблона маршрута, с помощью которого должен формироваться URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="4d888-323">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="4d888-324">В приведенном выше примере создается URL-адрес `custom/url/to/destination`.</span><span class="sxs-lookup"><span data-stu-id="4d888-324">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="4d888-325">Формирование URL-адресов по имени действия</span><span class="sxs-lookup"><span data-stu-id="4d888-325">Generating URLs by action name</span></span>

<span data-ttu-id="4d888-326">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="4d888-326">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="4d888-327">`Action`) и все связанные перегрузки предполагают необходимость указания целевого объекта путем задания имени контроллера и имени действия.</span><span class="sxs-lookup"><span data-stu-id="4d888-327">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="4d888-328">При использовании метода `Url.Action` текущие значения маршрута для `controller` и `action` уже определены: значения `controller` и `action` включены как в *значения окружения*, так и в **обычные** *значения*.</span><span class="sxs-lookup"><span data-stu-id="4d888-328">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="4d888-329">Метод `Url.Action` всегда использует текущие значения `action` и `controller` и формирует путь URL-адреса, ведущий к текущему действию.</span><span class="sxs-lookup"><span data-stu-id="4d888-329">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="4d888-330">При формировании URL-адреса система маршрутизации пытается использовать значения окружения для заполнения сведений, которые вы не предоставили.</span><span class="sxs-lookup"><span data-stu-id="4d888-330">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="4d888-331">При использовании такого маршрута, как `{a}/{b}/{c}/{d}`, и значений окружения `{ a = Alice, b = Bob, c = Carol, d = David }` система маршрутизации имеет достаточно информации для формирования URL-адреса без дополнительных значений, так как все параметры маршрута имеют значения.</span><span class="sxs-lookup"><span data-stu-id="4d888-331">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="4d888-332">Если добавить значение `{ d = Donovan }`, значение `{ d = David }` не будет учитываться, и будет сформирован путь URL-адреса `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="4d888-332">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="4d888-333">Пути URL-адресов являются иерархическими.</span><span class="sxs-lookup"><span data-stu-id="4d888-333">URL paths are hierarchical.</span></span> <span data-ttu-id="4d888-334">Если в приведенном выше примере добавить значение `{ c = Cheryl }`, пропущены будут оба значения `{ c = Carol, d = David }`.</span><span class="sxs-lookup"><span data-stu-id="4d888-334">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="4d888-335">В этом случае параметр `d` больше не имеет значения и сформировать URL-адрес не удастся.</span><span class="sxs-lookup"><span data-stu-id="4d888-335">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="4d888-336">Потребуется указать нужные значения для параметров `c` и `d`.</span><span class="sxs-lookup"><span data-stu-id="4d888-336">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="4d888-337">Эта проблема может возникнуть с маршрутом по умолчанию (`{controller}/{action}/{id?}`), но на практике она встречается редко, так как `Url.Action` всегда явным образом задает значения `controller` и `action`.</span><span class="sxs-lookup"><span data-stu-id="4d888-337">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="4d888-338">Более длинные перегрузки `Url.Action` также принимают дополнительный объект *значений маршрута* для предоставления значений параметров маршрута, отличных от `controller` и `action`.</span><span class="sxs-lookup"><span data-stu-id="4d888-338">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="4d888-339">Чаще всего он применяется с параметром `id`, например `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="4d888-339">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="4d888-340">В соответствии с соглашением объект *значений маршрута* обычно имеет анонимный тип, но это может быть также экземпляр `IDictionary<>` или *обычный объект .NET*.</span><span class="sxs-lookup"><span data-stu-id="4d888-340">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="4d888-341">Остальные значения маршрута, которые не соответствуют параметрам маршрута, помещаются в строку запроса.</span><span class="sxs-lookup"><span data-stu-id="4d888-341">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="4d888-342">Чтобы создать абсолютный URL-адрес, используйте перегрузку, принимающую `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="4d888-342">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="4d888-343">Формирование URL-адресов по маршруту</span><span class="sxs-lookup"><span data-stu-id="4d888-343">Generating URLs by route</span></span>

<span data-ttu-id="4d888-344">В приведенном выше коде демонстрировалось формирование URL-адреса путем передачи имен контроллера и действия.</span><span class="sxs-lookup"><span data-stu-id="4d888-344">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="4d888-345">Интерфейс `IUrlHelper` также предоставляет семейство методов `Url.RouteUrl`.</span><span class="sxs-lookup"><span data-stu-id="4d888-345">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="4d888-346">Эти методы похожи на `Url.Action`, но они не копируют текущие значения `action` и `controller` в значения маршрута.</span><span class="sxs-lookup"><span data-stu-id="4d888-346">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="4d888-347">Наиболее распространенный способ их применения — указание имени определенного маршрута, который должен использоваться для формирования URL-адреса, как правило, *без* указания имени контроллера или действия.</span><span class="sxs-lookup"><span data-stu-id="4d888-347">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="4d888-348">Формирование URL-адресов в HTML</span><span class="sxs-lookup"><span data-stu-id="4d888-348">Generating URLs in HTML</span></span>

<span data-ttu-id="4d888-349">Интерфейс `IHtmlHelper` предоставляет методы `HtmlHelper``Html.BeginForm` и `Html.ActionLink` для создания элементов `<form>` и `<a>` соответственно.</span><span class="sxs-lookup"><span data-stu-id="4d888-349">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="4d888-350">Эти методы используют метод `Url.Action` для формирования URL-адреса и принимают одинаковые аргументы.</span><span class="sxs-lookup"><span data-stu-id="4d888-350">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="4d888-351">Эквивалентами методов `Url.RouteUrl` для `HtmlHelper` являются методы `Html.BeginRouteForm` и `Html.RouteLink`, которые имеют схожие функции.</span><span class="sxs-lookup"><span data-stu-id="4d888-351">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="4d888-352">Для формирования URL-адресов используются вспомогательные функции тегов `form` и `<a>`.</span><span class="sxs-lookup"><span data-stu-id="4d888-352">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="4d888-353">Обе они реализуются с помощью интерфейса `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="4d888-353">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="4d888-354">Дополнительные сведения см. в статье [Работа с формами](../views/working-with-forms.md).</span><span class="sxs-lookup"><span data-stu-id="4d888-354">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="4d888-355">Внутри представлений интерфейс `IUrlHelper` доступен посредством свойства `Url` для особых случаев формирования URL-адресов, помимо описанных выше.</span><span class="sxs-lookup"><span data-stu-id="4d888-355">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="4d888-356">Формирование URL-адресов в результатах действий</span><span class="sxs-lookup"><span data-stu-id="4d888-356">Generating URLS in Action Results</span></span>

<span data-ttu-id="4d888-357">В предыдущих примерах было продемонстрировано использование интерфейса `IUrlHelper` в контроллере, хотя чаще всего URL-адреса формируются в контроллерах в рамках результата действия.</span><span class="sxs-lookup"><span data-stu-id="4d888-357">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="4d888-358">Базовые классы `ControllerBase` и `Controller` предоставляют удобные методы для результатов действий, ссылающихся на другое действие.</span><span class="sxs-lookup"><span data-stu-id="4d888-358">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="4d888-359">Одним из типичных способов их применения является перенаправление после принятия входных данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="4d888-359">One typical usage is to redirect after accepting user input.</span></span>

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

<span data-ttu-id="4d888-360">Фабричные методы результатов действий по своей структуре похожи на методы интерфейса `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="4d888-360">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="4d888-361">Выделенные маршруты на основе соглашений</span><span class="sxs-lookup"><span data-stu-id="4d888-361">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="4d888-362">При маршрутизации на основе соглашений может использоваться особый тип определения маршрута — *выделенный маршрут на основе соглашения*.</span><span class="sxs-lookup"><span data-stu-id="4d888-362">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="4d888-363">В приведенном ниже примере маршрут с именем `blog` является выделенным маршрутом на основе соглашения.</span><span class="sxs-lookup"><span data-stu-id="4d888-363">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="4d888-364">При использовании таких определений маршрутов метод `Url.Action("Index", "Home")` создаст путь URL-адреса `/` с маршрутом `default`. В чем же причина?</span><span class="sxs-lookup"><span data-stu-id="4d888-364">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="4d888-365">Можно было бы предположить, что значений маршрута `{ controller = Home, action = Index }` было бы достаточно для формирования URL-адреса с помощью `blog` и результатом было бы `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="4d888-365">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="4d888-366">В случае с выделенными маршрутами на основе соглашений используется особое поведение значений по умолчанию, для которых нет соответствующих параметров маршрута. Благодаря ему маршрут не может быть слишком универсальным при формировании URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="4d888-366">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="4d888-367">В этом случае значения по умолчанию — `{ controller = Blog, action = Article }`, но параметров маршрута `controller` и `action` нет.</span><span class="sxs-lookup"><span data-stu-id="4d888-367">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="4d888-368">Когда система маршрутизации производит формирование URL-адреса, предоставленные значения должны соответствовать значениям по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4d888-368">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="4d888-369">Сформировать URL-адрес с помощью `blog` не удастся, так как значения `{ controller = Home, action = Index }` не соответствуют `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="4d888-369">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="4d888-370">После этого система маршрутизации выполнит попытку использовать маршрут `default`, которая завершится успешно.</span><span class="sxs-lookup"><span data-stu-id="4d888-370">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="4d888-371">Области</span><span class="sxs-lookup"><span data-stu-id="4d888-371">Areas</span></span>

<span data-ttu-id="4d888-372">[Области](areas.md) — это возможность MVC, которая служит для объединения связанных функций в группу в виде отдельного пространства имен маршрутизации (для действий контроллеров) и структуры папок (для представлений).</span><span class="sxs-lookup"><span data-stu-id="4d888-372">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="4d888-373">Благодаря областям приложение может иметь несколько контроллеров с одинаковыми именами, если у них будут разные *области*.</span><span class="sxs-lookup"><span data-stu-id="4d888-373">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="4d888-374">При использовании областей создается иерархия в целях маршрутизации. Для этого к `controller` и `action` добавляется еще один параметр маршрута, `area`.</span><span class="sxs-lookup"><span data-stu-id="4d888-374">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="4d888-375">В этом разделе рассматривается взаимодействие системы маршрутизации с областями. Подробные сведения об использовании областей с представлениями см. в статье [Области](areas.md).</span><span class="sxs-lookup"><span data-stu-id="4d888-375">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="4d888-376">В следующем примере в MVC настраивается использование маршрута на основе соглашения по умолчанию и *маршрута области* для области с именем `Blog`:</span><span class="sxs-lookup"><span data-stu-id="4d888-376">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="4d888-377">Результатом сопоставления первого маршрута с таким путем URL-адреса, как `/Manage/Users/AddUser`, будут значения маршрута `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="4d888-377">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="4d888-378">Значение маршрута `area` получается на основе значения по умолчанию для параметра `area`. По сути, маршрут, создаваемый методом `MapAreaRoute`, эквивалентен следующему:</span><span class="sxs-lookup"><span data-stu-id="4d888-378">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="4d888-379">Метод `MapAreaRoute` создает маршрут с помощью значения по умолчанию и ограничения для `area` с использованием предоставленного имени маршрута (в данном случае `Blog`).</span><span class="sxs-lookup"><span data-stu-id="4d888-379">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="4d888-380">Значение по умолчанию гарантирует, что маршрут всегда создает значение `{ area = Blog, ... }`. Ограничение требует значения `{ area = Blog, ... }` для формирования URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="4d888-380">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="4d888-381">При маршрутизации на основе соглашений учитывается порядок.</span><span class="sxs-lookup"><span data-stu-id="4d888-381">Conventional routing is order-dependent.</span></span> <span data-ttu-id="4d888-382">Как правило, маршруты с областями должны находиться раньше в таблице маршрутов, так как они более точные, чем маршруты без областей.</span><span class="sxs-lookup"><span data-stu-id="4d888-382">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="4d888-383">В предыдущем примере значения маршрута будут соответствовать следующему действию:</span><span class="sxs-lookup"><span data-stu-id="4d888-383">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="4d888-384">`AreaAttribute` обозначает контроллер в рамках области. В таком случае говорят, что контроллер находится в области `Blog`.</span><span class="sxs-lookup"><span data-stu-id="4d888-384">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="4d888-385">Контроллеры без атрибута `[Area]` не входят ни в одну область и **не** будут соответствовать маршруту, если система маршрутизации предоставляет значение маршрута `area`.</span><span class="sxs-lookup"><span data-stu-id="4d888-385">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="4d888-386">В приведенном ниже примере только первый контроллер может соответствовать значениям маршрута `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="4d888-386">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="4d888-387">Пространство имен каждого контроллера приведено здесь для полноты. В противном случае между контроллерами возник бы конфликт именования, и произошла бы ошибка компилятора.</span><span class="sxs-lookup"><span data-stu-id="4d888-387">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="4d888-388">Имена пространств классов не влияют на маршрутизацию в MVC.</span><span class="sxs-lookup"><span data-stu-id="4d888-388">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="4d888-389">Первые два контроллера входят в области и будут соответствовать запросу, только если соответствующее имя области предоставлено значением маршрута `area`.</span><span class="sxs-lookup"><span data-stu-id="4d888-389">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="4d888-390">Третий контроллер не входит ни в одну область и может соответствовать запросу, только если значение `area` не предоставлено системой маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="4d888-390">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="4d888-391">В плане сопоставления *отсутствующих значений* отсутствие значения `area` равносильно тому, как если значением `area` было бы NULL или пустая строка.</span><span class="sxs-lookup"><span data-stu-id="4d888-391">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="4d888-392">При выполнении действия внутри области значение маршрута для `area` будет доступно как *значение окружения*, которое система маршрутизации использует для формирования URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="4d888-392">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="4d888-393">Это означает, что по умолчанию области являются *фиксированными* при формировании URL-адресов, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="4d888-393">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="4d888-394">Основные сведения об интерфейсе IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="4d888-394">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="4d888-395">В этом разделе описываются внутренние принципы работы платформы и то, как MVC выбирает действие, которое необходимо выполнить.</span><span class="sxs-lookup"><span data-stu-id="4d888-395">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="4d888-396">Типичному приложению пользовательская реализация `IActionConstraint` не требуется.</span><span class="sxs-lookup"><span data-stu-id="4d888-396">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="4d888-397">Скорее всего, вы уже пользовались интерфейсом `IActionConstraint`, даже если не знакомы с ним.</span><span class="sxs-lookup"><span data-stu-id="4d888-397">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="4d888-398">Атрибут `[HttpGet]` и сходные атрибуты `[Http-VERB]` реализуют интерфейс `IActionConstraint`, чтобы ограничить выполнение метода действия.</span><span class="sxs-lookup"><span data-stu-id="4d888-398">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="4d888-399">В случае с маршрутом по умолчанию на основе соглашения путь URL-адреса `/Products/Edit` даст значения `{ controller = Products, action = Edit }`, которые будут соответствовать **обоим** приведенным здесь действиям.</span><span class="sxs-lookup"><span data-stu-id="4d888-399">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="4d888-400">В терминологии `IActionConstraint` можно сказать, что оба действия считаются кандидатами, так как они оба соответствуют данным маршрута.</span><span class="sxs-lookup"><span data-stu-id="4d888-400">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="4d888-401">Когда метод `HttpGetAttribute` выполняется, он сообщает, что *Edit()* является соответствием для запроса *GET*, но не является соответствием для каких-либо иных HTTP-команд.</span><span class="sxs-lookup"><span data-stu-id="4d888-401">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="4d888-402">Для действия `Edit(...)` ограничения не определены, поэтому оно соответствует любой HTTP-команде.</span><span class="sxs-lookup"><span data-stu-id="4d888-402">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="4d888-403">Поэтому если рассматривать запрос `POST`, соответствием будет только `Edit(...)`.</span><span class="sxs-lookup"><span data-stu-id="4d888-403">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="4d888-404">Однако в случае с запросом `GET` соответствовать могут оба действия, хотя действие с `IActionConstraint` всегда считается *имеющим приоритет*.</span><span class="sxs-lookup"><span data-stu-id="4d888-404">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="4d888-405">Таким образом, поскольку действие `Edit()` имеет атрибут `[HttpGet]`, оно считается более конкретным и будет выбрано в случае соответствия обоих действий.</span><span class="sxs-lookup"><span data-stu-id="4d888-405">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="4d888-406">По сути, интерфейс `IActionConstraint` представляет собой своего рода *перегрузку*, но вместо перегрузки методов с одинаковыми именами он перегружает действия, соответствующие одному и тому же URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="4d888-406">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="4d888-407">При маршрутизации с помощью атрибутов также применяется интерфейс `IActionConstraint`, в результате чего кандидатами могут считаться действия из разных контроллеров.</span><span class="sxs-lookup"><span data-stu-id="4d888-407">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="4d888-408">Реализация IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="4d888-408">Implementing IActionConstraint</span></span>

<span data-ttu-id="4d888-409">Самый простой способ реализовать интерфейс `IActionConstraint` — создать класс, производный от `System.Attribute`, и добавить его в действия и контроллеры.</span><span class="sxs-lookup"><span data-stu-id="4d888-409">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="4d888-410">MVC автоматически обнаруживает экземпляры `IActionConstraint`, применяемые как атрибуты.</span><span class="sxs-lookup"><span data-stu-id="4d888-410">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="4d888-411">Вы можете использовать модель приложения для применения ограничений, и это, пожалуй, самый гибкий подход, так как он позволяет метапрограммировать способ их применения.</span><span class="sxs-lookup"><span data-stu-id="4d888-411">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="4d888-412">В приведенном ниже примере ограничение выбирает действие на основе *кода страны* из данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="4d888-412">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="4d888-413">[Полный пример можно найти в GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="4d888-413">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="4d888-414">Вы отвечаете за реализацию метода `Accept` и определение очередности применения ограничений.</span><span class="sxs-lookup"><span data-stu-id="4d888-414">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="4d888-415">В этом случае метод `Accept` возвращает значение `true`, означающее, что действие является соответствующим при соответствии значения маршрута `country`.</span><span class="sxs-lookup"><span data-stu-id="4d888-415">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="4d888-416">Таким образом, он позволяет возвращаться к действию без атрибута, чем отличается от метода `RouteValueAttribute`.</span><span class="sxs-lookup"><span data-stu-id="4d888-416">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="4d888-417">В примере показано, что если определено действие `en-US`, то для кода страны `fr-FR` будет выбран более общий контроллер, к которому не применяется ограничение `[CountrySpecific(...)]`.</span><span class="sxs-lookup"><span data-stu-id="4d888-417">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="4d888-418">Свойство `Order` определяет, к какому *этапу* относится ограничение.</span><span class="sxs-lookup"><span data-stu-id="4d888-418">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="4d888-419">Ограничения действий применяются группами в соответствии со значением `Order`.</span><span class="sxs-lookup"><span data-stu-id="4d888-419">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="4d888-420">Например, все предоставляемые платформой атрибуты HTTP-методов имеют одинаковое значение `Order`, благодаря чему они выполняются на одном этапе.</span><span class="sxs-lookup"><span data-stu-id="4d888-420">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="4d888-421">Чтобы реализовать требуемые политики, можно использовать любое количество этапов.</span><span class="sxs-lookup"><span data-stu-id="4d888-421">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="4d888-422">Чтобы выбрать значение `Order`, подумайте, должно ли ограничение применяться перед выполнением HTTP-методов.</span><span class="sxs-lookup"><span data-stu-id="4d888-422">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="4d888-423">Чем меньше значение, тем раньше применяется ограничение.</span><span class="sxs-lookup"><span data-stu-id="4d888-423">Lower numbers run first.</span></span>
