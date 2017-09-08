---
title: "Общие сведения об основных ASP.NET MVC"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 3e95ccd8aeda54a050cd656ea0c831fd928f53a2
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="56897-103">Общие сведения об основных ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="56897-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="56897-104">По [Стив Смит](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="56897-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="56897-105">Основные компоненты ASP.NET MVC — многофункциональную платформу для построения веб-приложений и шаблон разработки Model-View-Controller с помощью API-интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="56897-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="56897-106">Что такое шаблон MVC</span><span class="sxs-lookup"><span data-stu-id="56897-106">What is the MVC pattern?</span></span>

<span data-ttu-id="56897-107">Шаблон архитектуры Model-View-Controller (MVC) разделяет приложение на три группы компонентов: модели, представления и контроллеры.</span><span class="sxs-lookup"><span data-stu-id="56897-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="56897-108">Этот шаблон позволяет достигнуть [Разделение областей ответственности](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="56897-108">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="56897-109">С помощью этого шаблона, запросы пользователей направляются контроллер, который отвечает за работу с моделью для выполнения действий пользователя и/или получить результаты запросов.</span><span class="sxs-lookup"><span data-stu-id="56897-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="56897-110">Контроллер выбирает представления для отображения для пользователя и предоставляет ему все данные модели, которые требуются.</span><span class="sxs-lookup"><span data-stu-id="56897-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="56897-111">На следующей диаграмме показаны три основных компонента и какие из них ссылаются другие:</span><span class="sxs-lookup"><span data-stu-id="56897-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![Шаблон MVC](overview/_static/mvc.png)

<span data-ttu-id="56897-113">Это разграничение обязанностей позволяет масштабировать приложения с точки зрения сложности, так как это упрощает код, отлаживать и тестировать нечто (модели, представления или контроллер) с одним заданием (и соответствует [персональной ответственности ](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="56897-113">This delineation of responsibilities helps you scale the application in terms of complexity because it’s easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="56897-114">Более сложен для обновления, тестирования и отладки кода, имеющее зависимости, которые распределены между двумя или несколькими из этих трех областей.</span><span class="sxs-lookup"><span data-stu-id="56897-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="56897-115">Например логики интерфейса пользователя, как правило, изменение чаще, чем бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="56897-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="56897-116">Если код и бизнес-логики представления объединяются в один объект, необходимо изменить объект, содержащий бизнес-логику, каждый раз при изменении пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="56897-116">If presentation code and business logic are combined in a single object, you have to modify an object containing business logic every time you change the user interface.</span></span> <span data-ttu-id="56897-117">Это может привести к ошибкам и требуется повторное все бизнес-логики, после изменения каждой минимальным пользовательским интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="56897-117">This is likely to introduce errors and require the retesting of all business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="56897-118">Представление и контроллер зависят от модели.</span><span class="sxs-lookup"><span data-stu-id="56897-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="56897-119">Однако модели зависит от контроллера ни представления.</span><span class="sxs-lookup"><span data-stu-id="56897-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="56897-120">Это является одним из ключевых преимуществ разделения.</span><span class="sxs-lookup"><span data-stu-id="56897-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="56897-121">Это разделение позволяет выполнять построение и тестирование модели независимо от визуального представления.</span><span class="sxs-lookup"><span data-stu-id="56897-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="56897-122">Обязанности модели</span><span class="sxs-lookup"><span data-stu-id="56897-122">Model Responsibilities</span></span>

<span data-ttu-id="56897-123">Модели в MVC-приложении представляет состояние приложения и бизнес-логика или операций, которые должны выполняться в нем.</span><span class="sxs-lookup"><span data-stu-id="56897-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="56897-124">Бизнес-логики должны инкапсулироваться в модели, вместе с любой реализации логики для сохранения состояния приложения.</span><span class="sxs-lookup"><span data-stu-id="56897-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="56897-125">Строго типизированного представления обычно используют типы ViewModel специально разработан для хранения данных для отображения в этом представлении; контроллер будет создать и заполнить эти экземпляры ViewModel из модели.</span><span class="sxs-lookup"><span data-stu-id="56897-125">Strongly-typed views will typically use ViewModel types specifically designed to contain the data to display on that view; the controller will create and populate these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="56897-126">Существует множество способов для организации модели в приложение, использующее шаблон архитектуры MVC.</span><span class="sxs-lookup"><span data-stu-id="56897-126">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="56897-127">Дополнительные сведения о некоторых [различные типы моделей](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="56897-127">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="56897-128">Просмотр ответственности</span><span class="sxs-lookup"><span data-stu-id="56897-128">View Responsibilities</span></span>

<span data-ttu-id="56897-129">Представления отвечают за представления содержимого через пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="56897-129">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="56897-130">Они используют [представлений Razor](#razor-view-engine) для внедрения кода .NET в разметке HTML.</span><span class="sxs-lookup"><span data-stu-id="56897-130">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="56897-131">Должно быть минимальным логики представления и логики в них должны быть связаны с представления содержимого.</span><span class="sxs-lookup"><span data-stu-id="56897-131">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="56897-132">Если файлы необходимости выполнять большую часть логики в представлении для отображения данных из сложной модели, рассмотрите возможность использования [компонент представления](views/view-components.md), ViewModel, или Просмотр шаблона для упрощения просмотра.</span><span class="sxs-lookup"><span data-stu-id="56897-132">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="56897-133">Обязанности контроллера</span><span class="sxs-lookup"><span data-stu-id="56897-133">Controller Responsibilities</span></span>

<span data-ttu-id="56897-134">Контроллеры служат для управления взаимодействием с пользователем, работу с моделью и Выбор представления для отображения.</span><span class="sxs-lookup"><span data-stu-id="56897-134">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="56897-135">В приложении MVC представления только отображают сведения; контроллер обрабатывает и реагирует на ввод данных пользователем и взаимодействия.</span><span class="sxs-lookup"><span data-stu-id="56897-135">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="56897-136">В шаблоне MVC контроллер является начальной точкой и выбирается какая модель типы для работы с и представления для отображения (поэтому ее имя — он управляет, как приложение реагирует на конкретный запрос).</span><span class="sxs-lookup"><span data-stu-id="56897-136">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="56897-137">Контроллеры не следует слишком сложным по слишком много ответственности.</span><span class="sxs-lookup"><span data-stu-id="56897-137">Controllers should not be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="56897-138">Чтобы становились логику контроллера слишком сложным, используйте [персональной ответственности](http://deviq.com/single-responsibility-principle/) к бизнес-логике push из контроллера и в модели домена.</span><span class="sxs-lookup"><span data-stu-id="56897-138">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="56897-139">Если обнаружится, что ваши действия контроллера часто выполнять те же самые виды действий, можно выполнить [не повторять самостоятельно принцип](http://deviq.com/don-t-repeat-yourself/) путем перемещения этих общих действий в [фильтры](#filters).</span><span class="sxs-lookup"><span data-stu-id="56897-139">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="56897-140">Что такое основные ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="56897-140">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="56897-141">Платформа ASP.NET Core MVC является упрощенных с открытым исходным легковесной платформой оптимизирован для использования с ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="56897-141">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="56897-142">ASP.NET Core MVC предоставляет основанный на шаблонах способ создания динамических веб-сайтов, позволяющий четкого разделения проблем.</span><span class="sxs-lookup"><span data-stu-id="56897-142">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="56897-143">Предоставляет полный контроль над разметки, согласованной с TDD разработкой поддерживает и использует новейшие веб-стандарты.</span><span class="sxs-lookup"><span data-stu-id="56897-143">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="56897-144">Функции</span><span class="sxs-lookup"><span data-stu-id="56897-144">Features</span></span>

<span data-ttu-id="56897-145">Основные ASP.NET MVC включает следующие параметры.</span><span class="sxs-lookup"><span data-stu-id="56897-145">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="56897-146">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="56897-146">Routing</span></span>](#routing)
* [<span data-ttu-id="56897-147">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="56897-147">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="56897-148">Проверка модели</span><span class="sxs-lookup"><span data-stu-id="56897-148">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="56897-149">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="56897-149">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="56897-150">Фильтры</span><span class="sxs-lookup"><span data-stu-id="56897-150">Filters</span></span>](#filters)
* [<span data-ttu-id="56897-151">Области</span><span class="sxs-lookup"><span data-stu-id="56897-151">Areas</span></span>](#areas)
* [<span data-ttu-id="56897-152">Веб-API</span><span class="sxs-lookup"><span data-stu-id="56897-152">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="56897-153">Возможности тестирования</span><span class="sxs-lookup"><span data-stu-id="56897-153">Testability</span></span>](#testability)
* [<span data-ttu-id="56897-154">Обработчик представлений Razor</span><span class="sxs-lookup"><span data-stu-id="56897-154">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="56897-155">Строго типизированные представления</span><span class="sxs-lookup"><span data-stu-id="56897-155">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="56897-156">Вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="56897-156">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="56897-157">Просмотр компонентов</span><span class="sxs-lookup"><span data-stu-id="56897-157">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="56897-158">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="56897-158">Routing</span></span>

<span data-ttu-id="56897-159">ASP.NET Core MVC, построены на основе [маршрутизации ASP.NET Core](../fundamentals/routing.md), мощный компонент сопоставления URL-адресов, который позволяет создавать приложения, которые имеют понятными и для поиска URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="56897-159">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="56897-160">Это позволяет определить именования шаблонов URL-адрес приложения, которые хорошо для оптимизации для поисковых систем (SEO) и компоновки без учета организацию файлов на веб-сервере.</span><span class="sxs-lookup"><span data-stu-id="56897-160">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="56897-161">Вы можете определить свои маршруты с помощью синтаксиса удобный маршрута шаблона, поддерживающего значение ограничения маршрута, значения по умолчанию и необязательных значений.</span><span class="sxs-lookup"><span data-stu-id="56897-161">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="56897-162">*Маршрутизация на основе соглашения о* принимает приложения позволяет определить URL-адрес глобально форматов, и каким образом каждый из этих форматов сопоставляется метод определенное действие на данный контроллер.</span><span class="sxs-lookup"><span data-stu-id="56897-162">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="56897-163">При получении входящего запроса, механизм маршрутизации выполняет синтаксический анализ URL-адрес соответствует его к одному из определенных форматов URL-адрес и затем вызывает метод действия связанного контроллера.</span><span class="sxs-lookup"><span data-stu-id="56897-163">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="56897-164">*Атрибут маршрутизации* позволяет указать сведения о маршрутизации по контроллеров и действий с помощью атрибутов, которые определяют маршрутов для приложения.</span><span class="sxs-lookup"><span data-stu-id="56897-164">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="56897-165">Это означает, определениях маршрута находятся рядом с контроллера и действия, с которым они связаны.</span><span class="sxs-lookup"><span data-stu-id="56897-165">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [1, 4]}} -->

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a><span data-ttu-id="56897-166">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="56897-166">Model binding</span></span>

<span data-ttu-id="56897-167">ASP.NET Core MVC [привязки модели](models/model-binding.md) преобразует данные запроса клиента (значений формы, данные о маршруте, параметров строки запроса, заголовки HTTP) в объекты, которые могут справляться с контроллером.</span><span class="sxs-lookup"><span data-stu-id="56897-167">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="56897-168">В результате логику контроллера не имеет для этого нужно определять данные поступившего запроса; он просто имеет данные в качестве параметров его методов действия.</span><span class="sxs-lookup"><span data-stu-id="56897-168">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="56897-169">Проверка модели</span><span class="sxs-lookup"><span data-stu-id="56897-169">Model validation</span></span>

<span data-ttu-id="56897-170">Основные ASP.NET MVC поддерживает [проверки](models/validation.md) дополняя модель объекта с атрибуты проверки данных заметок.</span><span class="sxs-lookup"><span data-stu-id="56897-170">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="56897-171">Атрибуты проверки проверяются на стороне клиента, прежде чем значения передаются на сервер, а также вызывается на сервере перед выполнением действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="56897-171">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [4, 5, 8, 9]}} -->

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

<span data-ttu-id="56897-172">Действие контроллера:</span><span class="sxs-lookup"><span data-stu-id="56897-172">A controller action:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [3]}} -->

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // If we got this far, something failed, redisplay form
    return View(model);
}
```

<span data-ttu-id="56897-173">Платформа будет обрабатывать проверки запроса данных на клиенте и на сервере.</span><span class="sxs-lookup"><span data-stu-id="56897-173">The framework will handle validating request data both on the client and on the server.</span></span> <span data-ttu-id="56897-174">Логику проверки, указанные на типы модели добавляется в готовом для просмотра представления в виде ненавязчивой заметок и обеспечивается в браузере с [jQuery проверки](http://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="56897-174">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](http://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="56897-175">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="56897-175">Dependency injection</span></span>

<span data-ttu-id="56897-176">ASP.NET Core имеет встроенную поддержку [внедрения зависимости (DI)](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="56897-176">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="56897-177">В ASP.NET MVC Core [контроллеров](controllers/dependency-injection.md) можно запроса необходимых служб через их конструкторы, позволяя выполните [явные зависимости принцип](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="56897-177">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="56897-178">Можно также использовать приложение [внедрение зависимостей в представлении файлы](views/dependency-injection.md), с использованием `@inject` директиву:</span><span class="sxs-lookup"><span data-stu-id="56897-178">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@inject SomeService ServiceName
<!DOCTYPE html>
<html>
<head>
  <title>@ServiceName.GetTitle</title>
</head>
<body>
  <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a><span data-ttu-id="56897-179">Фильтры</span><span class="sxs-lookup"><span data-stu-id="56897-179">Filters</span></span>

<span data-ttu-id="56897-180">[Фильтры](controllers/filters.md) помочь разработчикам инкапсулировать проблемы с перекрестными, такие как обработка исключений или авторизации.</span><span class="sxs-lookup"><span data-stu-id="56897-180">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="56897-181">Фильтры включить выполнение пользовательской логики до и после обработки методов действий и можно настроить для запуска в определенных точках при конвейерной обработки для данного запроса.</span><span class="sxs-lookup"><span data-stu-id="56897-181">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="56897-182">Фильтры могут применяться к контроллерам или действия, как атрибуты (или может выполняться глобально).</span><span class="sxs-lookup"><span data-stu-id="56897-182">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="56897-183">Несколько фильтров (например, `Authorize`) включаются в структуре.</span><span class="sxs-lookup"><span data-stu-id="56897-183">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {

```

### <a name="areas"></a><span data-ttu-id="56897-184">Области</span><span class="sxs-lookup"><span data-stu-id="56897-184">Areas</span></span>

<span data-ttu-id="56897-185">[Области](controllers/areas.md) предоставляют способ разделения большого ASP.NET Core MVC, веб-приложения на более мелкие функциональные группы.</span><span class="sxs-lookup"><span data-stu-id="56897-185">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="56897-186">Область является структурой MVC, Расположенной внутри приложения.</span><span class="sxs-lookup"><span data-stu-id="56897-186">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="56897-187">В проект MVC логические компоненты, например модели, контроллера и представления хранятся в разных папках и MVC использует соглашения об именовании, чтобы создать связь между этими компонентами.</span><span class="sxs-lookup"><span data-stu-id="56897-187">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="56897-188">Для большого приложения может быть выгодно для разделения приложения на отдельные области высокий уровень функциональных возможностей.</span><span class="sxs-lookup"><span data-stu-id="56897-188">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="56897-189">Например, для электронной коммерции приложение с несколько подразделений, таких как извлечение, выставлении счетов и поиска и т. д. Каждый из этих частей имеют свои собственные логический компонент представления, контроллеры и модели.</span><span class="sxs-lookup"><span data-stu-id="56897-189">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="56897-190">Веб-API</span><span class="sxs-lookup"><span data-stu-id="56897-190">Web APIs</span></span>

<span data-ttu-id="56897-191">Помимо прекрасно подходит для создания веб-сайтов, ASP.NET Core MVC имеет отличную поддержку для построения веб-API.</span><span class="sxs-lookup"><span data-stu-id="56897-191">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="56897-192">Можно создавать службы, могут получить доступ широкого диапазона клиентов, включая браузеры и мобильные устройства.</span><span class="sxs-lookup"><span data-stu-id="56897-192">You can build services that can reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="56897-193">Платформа включает поддержку для согласования содержимого HTTP со встроенной поддержкой [форматирование данных](models/formatting.md) формате JSON или XML.</span><span class="sxs-lookup"><span data-stu-id="56897-193">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="56897-194">Запись [пользовательские модули форматирования](advanced/custom-formatters.md) для добавления поддержки для собственных форматов.</span><span class="sxs-lookup"><span data-stu-id="56897-194">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="56897-195">Чтобы обеспечить поддержку гипермедиа используйте компоновки.</span><span class="sxs-lookup"><span data-stu-id="56897-195">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="56897-196">Легко включить поддержку [ресурсов независимо от источника (CORS) для управления доступом](http://www.w3.org/TR/cors/) , чтобы ваш веб-API может совместно использоваться в нескольких веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="56897-196">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="56897-197">Возможности тестирования</span><span class="sxs-lookup"><span data-stu-id="56897-197">Testability</span></span>

<span data-ttu-id="56897-198">Использование framework интерфейсы и внедрение зависимостей сделать хорошо подходит для модульного тестирования и платформа включает функции (например, TestHost и InMemory поставщика Entity Framework), которые обеспечивают [интеграционного тестирования](../testing/integration-testing.md) быстрого и легко также.</span><span class="sxs-lookup"><span data-stu-id="56897-198">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="56897-199">Дополнительные сведения о [тестирование логики контроллера](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="56897-199">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="56897-200">Обработчик представлений Razor</span><span class="sxs-lookup"><span data-stu-id="56897-200">Razor view engine</span></span>

<span data-ttu-id="56897-201">[Представления ASP.NET Core MVC](views/overview.md) использовать [представлений Razor](views/razor.md) для визуализации представления.</span><span class="sxs-lookup"><span data-stu-id="56897-201">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="56897-202">Razor — это язык разметки compact, выразительный и жидкости шаблон для определения представлений с помощью встроенного кода C#.</span><span class="sxs-lookup"><span data-stu-id="56897-202">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="56897-203">Razor используется для динамического создания веб-содержимого на сервере.</span><span class="sxs-lookup"><span data-stu-id="56897-203">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="56897-204">Серверный код полностью можно комбинировать с содержимым стороны клиента и код.</span><span class="sxs-lookup"><span data-stu-id="56897-204">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="56897-205">С помощью представлений Razor, можно определить [макеты](views/layout.md), [разделяемые представления](views/partial.md) и подставляемые разделов.</span><span class="sxs-lookup"><span data-stu-id="56897-205">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="56897-206">Строго типизированные представления</span><span class="sxs-lookup"><span data-stu-id="56897-206">Strongly typed views</span></span>

<span data-ttu-id="56897-207">Представлений Razor в MVC может быть строго типизированным в зависимости от модели.</span><span class="sxs-lookup"><span data-stu-id="56897-207">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="56897-208">Контроллеры можно передать модель строго типизированные представления, включение представлений, чтобы иметь проверку типов и поддержку IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="56897-208">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="56897-209">Например, следующее представление определяется модель управляемого типа `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="56897-209">For example, the following view defines a model of type `IEnumerable<Product>`:</span></span>

```html
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="56897-210">Вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="56897-210">Tag Helpers</span></span>

<span data-ttu-id="56897-211">[Вспомогательных функций тегов](views/tag-helpers/intro.md) код стороны сервер мог участвовать в создании и подготовке к просмотру элементов HTML в файлах Razor.</span><span class="sxs-lookup"><span data-stu-id="56897-211">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="56897-212">Вспомогательных функций тегов можно использовать для определения настраиваемых тегов (например, `<environment>`) или для изменения поведения существующие теги (например, `<label>`).</span><span class="sxs-lookup"><span data-stu-id="56897-212">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="56897-213">Вспомогательных функций тегов привязки к определенным элементам, на основе имени элемента и его атрибуты.</span><span class="sxs-lookup"><span data-stu-id="56897-213">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="56897-214">Они предоставляют преимущества отрисовки на стороне сервера сохранением возможности редактирования HTML.</span><span class="sxs-lookup"><span data-stu-id="56897-214">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="56897-215">Существует множество встроенных вспомогательных функций тегов для общих задач — например для создания формы, ссылки, загрузки средств и пакетов дополнительные - и еще более доступные из открытых репозиториев GitHub и в качестве NuGet.</span><span class="sxs-lookup"><span data-stu-id="56897-215">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="56897-216">Вспомогательных функций тегов разрабатываются в C#, и они предназначены для HTML-элементов на основе имени элемента, имя атрибута или родительского тега.</span><span class="sxs-lookup"><span data-stu-id="56897-216">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="56897-217">Например, встроенная LinkTagHelper можно использовать, чтобы создать ссылку на `Login` действие `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="56897-217">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3]}} -->

```html
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="56897-218">`EnvironmentTagHelper` Может использоваться для включения различных наборов символов в зависимости от среды выполнения, например разработки, промежуточной или производственной представления (например, raw или уменьшенный):</span><span class="sxs-lookup"><span data-stu-id="56897-218">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 3, 4, 9]}} -->

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

<span data-ttu-id="56897-219">Вспомогательных функций тегов предоставить функции разработки понятного имени HTML и IntelliSense среду с широкими возможностями для создания разметки HTML и Razor.</span><span class="sxs-lookup"><span data-stu-id="56897-219">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="56897-220">Большинство встроенных вспомогательных функций тегов целевой существующих элементов HTML и предоставить серверные атрибуты для элемента.</span><span class="sxs-lookup"><span data-stu-id="56897-220">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="56897-221">Просмотр компонентов</span><span class="sxs-lookup"><span data-stu-id="56897-221">View Components</span></span>

<span data-ttu-id="56897-222">[Просмотр компонентов](views/view-components.md) позволяют упаковать логики отрисовки и использовать его в приложении.</span><span class="sxs-lookup"><span data-stu-id="56897-222">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="56897-223">Они аналогичны [разделяемые представления](views/partial.md), но с логикой, связанные.</span><span class="sxs-lookup"><span data-stu-id="56897-223">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
