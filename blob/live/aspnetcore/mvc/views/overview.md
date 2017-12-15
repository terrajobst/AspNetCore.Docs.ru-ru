---
title: "Представления в ASP.NET Core MVC"
author: ardalis
description: "Узнайте, как обрабатывать представления презентации данных приложения и взаимодействия с пользователем в ASP.NET Core MVC."
keywords: "ASP.NET Core просмотра MVC, razor, viewmodel, viewdata, viewbag"
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 2562d4e5fb85159e6ccb47990f54448ddc188077
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2017
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="60853-104">Представления в ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="60853-104">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="60853-105">По [Стив Смит](https://ardalis.com/) и [Latham Люк](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="60853-105">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="60853-106">В этом документе описаны представления, используемые в приложениях ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="60853-106">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="60853-107">На страницах Razor Подробнее [Общие сведения о страницах Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="60853-107">For information on Razor Pages, see [Introduction to Razor Pages](xref:mvc/razor-pages/index).</span></span>

<span data-ttu-id="60853-108">В **M**одели -**V**росмотр -**C**шаблон ontroller (MVC) *представление* обрабатывает приложения данных представления и взаимодействия с пользователями.</span><span class="sxs-lookup"><span data-stu-id="60853-108">In the **M**odel-**V**iew-**C**ontroller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="60853-109">Представление является шаблон HTML с внедренными [разметки Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="60853-109">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="60853-110">Разметка Razor, является код, который взаимодействует с HTML-разметку для создания веб-страницы, который отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="60853-110">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="60853-111">В ASP.NET Core MVC, представления, *.cshtml* файлы, использующие [языка программирования C#](/dotnet/csharp/) в разметке Razor.</span><span class="sxs-lookup"><span data-stu-id="60853-111">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="60853-112">Как правило, просматривать файлы группируются в папки для каждого приложения [контроллеров](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="60853-112">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="60853-113">Папки, хранятся в *представления* папку в корне приложения:</span><span class="sxs-lookup"><span data-stu-id="60853-113">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Папка представления в обозревателе Visual Studio решение открыта с открытым для отображения файлов About.cshtml, Contact.cshtml и Index.cshtml корневая папка](overview/_static/views_solution_explorer.png)

<span data-ttu-id="60853-115">*Главная* представленного контроллера *Главная* папки *представления* папки.</span><span class="sxs-lookup"><span data-stu-id="60853-115">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="60853-116">*Главная* папка содержит представления для *о*, *контакт*, и *индекс* веб-страниц (Домашняя страница).</span><span class="sxs-lookup"><span data-stu-id="60853-116">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="60853-117">Когда пользователи запрашивают эти три веб-страниц, действия контроллера в *Главная* контроллера определяют, какой из трех представлений используется для построения и возврата веб-страницы для пользователя.</span><span class="sxs-lookup"><span data-stu-id="60853-117">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="60853-118">Используйте [макеты](xref:mvc/views/layout) разделы согласованное веб-страницы и снизить повторяющийся код.</span><span class="sxs-lookup"><span data-stu-id="60853-118">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="60853-119">Макеты часто содержат заголовок, элементы навигации и меню и нижний колонтитул.</span><span class="sxs-lookup"><span data-stu-id="60853-119">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="60853-120">Колонтитулы обычно содержат стандартный разметки для многих элементов метаданных и ссылки на ресурсы сценариев и стилей.</span><span class="sxs-lookup"><span data-stu-id="60853-120">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="60853-121">Макеты помогают избежать этот стандартный текст разметки в представления.</span><span class="sxs-lookup"><span data-stu-id="60853-121">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="60853-122">[Разделяемые представления](xref:mvc/views/partial) снижения дублирования кода за счет управления допускающих многократное использование частей представления.</span><span class="sxs-lookup"><span data-stu-id="60853-122">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="60853-123">Например частичное представление может использоваться для Биография автора на веб-сайте блога, который отображается в нескольких представлениях.</span><span class="sxs-lookup"><span data-stu-id="60853-123">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="60853-124">Биография автора обычного представления содержимого, не требующий код, выполняемый для получения содержимого веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="60853-124">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="60853-125">Содержимое Биография автора доступна в представление с помощью модели привязки сама по себе, поэтому лучше всего использовать частичное представление для этого типа содержимого.</span><span class="sxs-lookup"><span data-stu-id="60853-125">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="60853-126">[Просмотр компонентов](xref:mvc/views/view-components) : аналогично частичного представления, в том, что они позволяют уменьшить повторяющихся частей кода, но они не подходят для Просмотр содержимого, которое требуется код, выполняемый на сервере для визуализации веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="60853-126">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="60853-127">Просмотр компонентов полезны в тех случаях, когда отображаемого содержимого требует взаимодействия с базы данных, таких как веб-сайта Корзина для покупок.</span><span class="sxs-lookup"><span data-stu-id="60853-127">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="60853-128">Просмотр компонентов не ограничиваясь привязки модели для создания веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="60853-128">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="60853-129">Преимущества использования представлений</span><span class="sxs-lookup"><span data-stu-id="60853-129">Benefits of using views</span></span>

<span data-ttu-id="60853-130">Представления позволяют установить [ **S**eparation **o**f **C**oncerns (SoC) разработки](http://deviq.com/separation-of-concerns/) в приложении MVC, разделяя разметки интерфейса пользователя из другие части приложения.</span><span class="sxs-lookup"><span data-stu-id="60853-130">Views help to establish a [**S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="60853-131">После разработки SoC делает приложение модульной, который предоставляет несколько преимуществ:</span><span class="sxs-lookup"><span data-stu-id="60853-131">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="60853-132">Это проще, так как она лучше организована приложение.</span><span class="sxs-lookup"><span data-stu-id="60853-132">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="60853-133">Представления обычно группируются по функции приложения.</span><span class="sxs-lookup"><span data-stu-id="60853-133">Views are generally grouped by app feature.</span></span> <span data-ttu-id="60853-134">Это облегчает поиск связанные представления, при работе с компонентом.</span><span class="sxs-lookup"><span data-stu-id="60853-134">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="60853-135">Слабо части приложения.</span><span class="sxs-lookup"><span data-stu-id="60853-135">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="60853-136">Можно создать и обновить представления приложения отдельно от бизнес-логику и данные компоненты доступа.</span><span class="sxs-lookup"><span data-stu-id="60853-136">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="60853-137">Представления приложения можно изменить, не имея обновления других частей приложения.</span><span class="sxs-lookup"><span data-stu-id="60853-137">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="60853-138">Тестирование части интерфейса пользователя приложения, поскольку представления представляют собой отдельные блоки проще.</span><span class="sxs-lookup"><span data-stu-id="60853-138">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="60853-139">Из-за повышения управляемости скорее всего, будут выполнены случайно повторения разделов пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="60853-139">Due to better organization, it's less likely that you'll accidently repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="60853-140">Создание представления</span><span class="sxs-lookup"><span data-stu-id="60853-140">Creating a view</span></span>

<span data-ttu-id="60853-141">Представления, относящиеся к контроллеру создаются в *представления / [Имя_контроллера]* папки.</span><span class="sxs-lookup"><span data-stu-id="60853-141">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="60853-142">Представления, которые являются общими для контроллеров, помещаются в *представления/Общие* папки.</span><span class="sxs-lookup"><span data-stu-id="60853-142">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="60853-143">Чтобы создать представление, добавьте новый файл и присвойте ему имя, совпадающее с именем его соответствующему контроллеру действие с *.cshtml* расширение имени файла.</span><span class="sxs-lookup"><span data-stu-id="60853-143">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="60853-144">Чтобы создать представление, которое соответствует *о* действия в *Главная* контроллер, создание *About.cshtml* файла в *представления/домашние*папки:</span><span class="sxs-lookup"><span data-stu-id="60853-144">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="60853-145">*Razor* разметки начинается с `@` символов.</span><span class="sxs-lookup"><span data-stu-id="60853-145">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="60853-146">Код выполнения операторы C#, поместив C# в [блоки кода Razor](xref:mvc/views/razor#razor-code-blocks) отключить в фигурные скобки (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="60853-146">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="60853-147">Например, см. раздел Назначение «О программе» `ViewData["Title"]` показано выше.</span><span class="sxs-lookup"><span data-stu-id="60853-147">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="60853-148">Можно отобразить значения в HTML, просто ссылаясь на значение с `@` символов.</span><span class="sxs-lookup"><span data-stu-id="60853-148">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="60853-149">Просмотреть содержимое `<h2>` и `<h3>` элементов, описанных выше.</span><span class="sxs-lookup"><span data-stu-id="60853-149">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="60853-150">Просмотр контента, показанный выше — только часть всей веб-страницы, который отображается для пользователя.</span><span class="sxs-lookup"><span data-stu-id="60853-150">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="60853-151">Остальная часть макет страницы и другие общие аспекты представления указываются в других файлах представление.</span><span class="sxs-lookup"><span data-stu-id="60853-151">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="60853-152">Дополнительные сведения см. в разделе [макета раздела](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="60853-152">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="60853-153">Как указать представления, контроллеры</span><span class="sxs-lookup"><span data-stu-id="60853-153">How controllers specify views</span></span>

<span data-ttu-id="60853-154">Представления обычно возвращаются из действия, как [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), который является типом [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="60853-154">Views are typically returned from actions as a [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="60853-155">Методе действия, можно создать и возвращать `ViewResult` напрямую, но, обычно не делается.</span><span class="sxs-lookup"><span data-stu-id="60853-155">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="60853-156">Поскольку наследуют большинство контроллеров [контроллера](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), можно просто использовать `View` вспомогательный метод для возврата `ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="60853-156">Since most controllers inherit from [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="60853-157">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="60853-157">*HomeController.cs*</span></span>

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="60853-158">По возвращении из этого действия *About.cshtml* , показанном в предыдущем разделе подготавливается к просмотру как следующей веб-странице:</span><span class="sxs-lookup"><span data-stu-id="60853-158">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![О страницы к просмотру в браузере Edge](overview/_static/about-page.png)

<span data-ttu-id="60853-160">`View` Вспомогательный метод имеет несколько перегрузок.</span><span class="sxs-lookup"><span data-stu-id="60853-160">The `View` helper method has several overloads.</span></span> <span data-ttu-id="60853-161">При необходимости можно указать:</span><span class="sxs-lookup"><span data-stu-id="60853-161">You can optionally specify:</span></span>

* <span data-ttu-id="60853-162">Явные представлению для возврата:</span><span class="sxs-lookup"><span data-stu-id="60853-162">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```
* <span data-ttu-id="60853-163">Объект [модели](xref:mvc/models/model-binding) для передачи представления:</span><span class="sxs-lookup"><span data-stu-id="60853-163">A [model](xref:mvc/models/model-binding) to pass to the the view:</span></span>

  ```csharp
  return View(Orders);
  ```
* <span data-ttu-id="60853-164">Представления и модель.</span><span class="sxs-lookup"><span data-stu-id="60853-164">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="60853-165">Представления обнаружения</span><span class="sxs-lookup"><span data-stu-id="60853-165">View discovery</span></span>

<span data-ttu-id="60853-166">Если действие возвращает представление, называется *представление обнаружения* имеет место.</span><span class="sxs-lookup"><span data-stu-id="60853-166">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="60853-167">Этот процесс определяет, какой файл представления используется на основе имени представления.</span><span class="sxs-lookup"><span data-stu-id="60853-167">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="60853-168">Поведение по умолчанию `View` метод (`return View();`) возвращает представление с тем же именем, что и метод действия, из которого он вызывается.</span><span class="sxs-lookup"><span data-stu-id="60853-168">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="60853-169">Например *о* `ActionResult` имя метода контроллера используется для поиска с именем файла представления *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="60853-169">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="60853-170">Во-первых, среда выполнения ищет в *представления / [Имя_контроллера]* папки для представления.</span><span class="sxs-lookup"><span data-stu-id="60853-170">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="60853-171">Если он не находит соответствующее представление, он выполняет *Shared* папки для представления.</span><span class="sxs-lookup"><span data-stu-id="60853-171">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="60853-172">Не важно, если возвращается неявно `ViewResult` с `return View();` или явным образом передать имя представления для `View` метод с `return View("<ViewName>");`.</span><span class="sxs-lookup"><span data-stu-id="60853-172">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="60853-173">В обоих случаях представление обнаружения выполняет поиск соответствующего файла представления в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="60853-173">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="60853-174">*Представления или\[Имя_контроллера]\[ViewName] .cshtml*</span><span class="sxs-lookup"><span data-stu-id="60853-174">*Views/\[ControllerName]\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="60853-175">*Представления/Общие/\[ViewName] .cshtml*</span><span class="sxs-lookup"><span data-stu-id="60853-175">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="60853-176">Путь к файлу представления могут быть предоставлены вместо имени представления.</span><span class="sxs-lookup"><span data-stu-id="60853-176">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="60853-177">Если с помощью абсолютный путь, начиная с корневого каталога приложения (при необходимости, начиная с «/» или «~ /»), *.cshtml* должно быть указано расширение:</span><span class="sxs-lookup"><span data-stu-id="60853-177">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="60853-178">Можно также использовать относительный путь для указания представления в разных каталогах без *.cshtml* расширения.</span><span class="sxs-lookup"><span data-stu-id="60853-178">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="60853-179">Внутри `HomeController`, можно вернуть *индекс* представление вашей *управление* представления по относительному пути:</span><span class="sxs-lookup"><span data-stu-id="60853-179">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="60853-180">Аналогичным образом, можно указать текущий каталог конкретного контроллера с «. /» префикс:</span><span class="sxs-lookup"><span data-stu-id="60853-180">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="60853-181">[Разделяемые представления](xref:mvc/views/partial) и [просматривать компоненты](xref:mvc/views/view-components) использовать механизмы обнаружения, аналогично (но не идентичен).</span><span class="sxs-lookup"><span data-stu-id="60853-181">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="60853-182">Вы можете настроить соглашение по умолчанию для способ представления находятся в приложении с помощью пользовательского [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span><span class="sxs-lookup"><span data-stu-id="60853-182">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="60853-183">Представление обнаружения использует поиск просмотреть файлы по имени файла.</span><span class="sxs-lookup"><span data-stu-id="60853-183">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="60853-184">Если базовая файловая система с учетом регистра, имена представлений, скорее всего, регистр учитывается.</span><span class="sxs-lookup"><span data-stu-id="60853-184">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="60853-185">Для совместимости с разными операционными системами учитывать регистр между контроллером и имена действий и имена файлов и папок связанного представления.</span><span class="sxs-lookup"><span data-stu-id="60853-185">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="60853-186">Если возникнет сообщение об ошибке, который не удается найти файл представления при работе с учетом регистра файловой системы, убедитесь, что регистр соответствует между запрошенное представление файла и имя файла представления.</span><span class="sxs-lookup"><span data-stu-id="60853-186">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="60853-187">Следуйте рекомендациям по организации структуры файлов для представлений, чтобы отразить отношения между контроллерами, действий и представления для удобства и простоты.</span><span class="sxs-lookup"><span data-stu-id="60853-187">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="60853-188">Передача данных с представлениями</span><span class="sxs-lookup"><span data-stu-id="60853-188">Passing data to views</span></span>

<span data-ttu-id="60853-189">Можно передавать данные представления, используя несколько подходов.</span><span class="sxs-lookup"><span data-stu-id="60853-189">You can pass data to views using several approaches.</span></span> <span data-ttu-id="60853-190">Наиболее надежный подход — указать [модель](xref:mvc/models/model-binding) типа в представлении.</span><span class="sxs-lookup"><span data-stu-id="60853-190">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="60853-191">Эта модель часто называется *viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="60853-191">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="60853-192">Экземпляр типа viewmodel передачи в представление из действия.</span><span class="sxs-lookup"><span data-stu-id="60853-192">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="60853-193">Использование viewmodel для передачи данных в представление позволяет воспользоваться преимуществами представлению *строгого* проверку типов.</span><span class="sxs-lookup"><span data-stu-id="60853-193">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="60853-194">*Строгая типизация* (или *строго типизированных*) означает, что каждая переменная и константа имеет явно определенного типа (например, `string`, `int`, или `DateTime`).</span><span class="sxs-lookup"><span data-stu-id="60853-194">*Strong typing* (or *strongly-typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="60853-195">Во время компиляции проверяется правильность типов, используемых в представлении.</span><span class="sxs-lookup"><span data-stu-id="60853-195">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="60853-196">[Visual Studio](https://www.visualstudio.com/vs/) и [кода Visual Studio](https://code.visualstudio.com/) список членов строго типизированный класс, используя функцию [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="60853-196">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly-typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="60853-197">Если вы хотите просмотреть свойства viewmodel, введите имя переменной для viewmodel, за которым следует (`.`).</span><span class="sxs-lookup"><span data-stu-id="60853-197">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="60853-198">Это помогает быстрее писать код с меньшим количеством ошибок.</span><span class="sxs-lookup"><span data-stu-id="60853-198">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="60853-199">Укажите модель с помощью `@model` директивы.</span><span class="sxs-lookup"><span data-stu-id="60853-199">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="60853-200">Использование модели с `@Model`:</span><span class="sxs-lookup"><span data-stu-id="60853-200">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="60853-201">Для предоставления модели представления, контроллер передает его в качестве параметра:</span><span class="sxs-lookup"><span data-stu-id="60853-201">To provide the model to the view, the controller passes it as a parameter:</span></span>

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

<span data-ttu-id="60853-202">Существуют ограничения на типы модели, которые могут использоваться для представления.</span><span class="sxs-lookup"><span data-stu-id="60853-202">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="60853-203">Мы рекомендуем использовать **P**чный **O**ld **C**LR **O**viewmodels объекта (POCO) с незначительной или нулевой (методы) задано поведение.</span><span class="sxs-lookup"><span data-stu-id="60853-203">We recommend using **P**lain **O**ld **C**LR **O**bject (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="60853-204">Как правило, классы viewmodel хранятся либо в *моделей* папки или отдельного *ViewModels* папку в корне приложения.</span><span class="sxs-lookup"><span data-stu-id="60853-204">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="60853-205">*Адрес* viewmodel, используемый в приведенном выше примере является viewmodel POCO, хранятся в файле с именем *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="60853-205">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

> [!NOTE]
> <span data-ttu-id="60853-206">Ничего не запрещает использовать те же классы для типов viewmodel и ваш бизнес-типов модели.</span><span class="sxs-lookup"><span data-stu-id="60853-206">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="60853-207">Тем не менее использование отдельных моделей позволяет представлений, чтобы отличаться независимо от бизнес-логику и данные доступа частей приложения.</span><span class="sxs-lookup"><span data-stu-id="60853-207">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="60853-208">Разделение моделей и viewmodels также имеет преимущества безопасности, если модели используют [привязки модели](xref:mvc/models/model-binding) и [проверки](xref:mvc/models/validation) для данных, передаваемых в приложение со стороны пользователя.</span><span class="sxs-lookup"><span data-stu-id="60853-208">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a><span data-ttu-id="60853-209">Слабо типизированных данных (ViewData и ViewBag)</span><span class="sxs-lookup"><span data-stu-id="60853-209">Weakly-typed data (ViewData and ViewBag)</span></span>

<span data-ttu-id="60853-210">Примечание: `ViewBag` доступен не на страницах Razor.</span><span class="sxs-lookup"><span data-stu-id="60853-210">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="60853-211">Помимо строго типизированного представления представления имеют доступ к *слабо типизированной* (также называемый *слабо типизированной*) сбор данных.</span><span class="sxs-lookup"><span data-stu-id="60853-211">In addition to strongly-typed views, views have access to a *weakly-typed* (also called *loosely-typed*) collection of data.</span></span> <span data-ttu-id="60853-212">В отличие от строгие типы *слабое типы* (или *свободные типы*) означает, что не явно объявить тип данных, которые вы используете.</span><span class="sxs-lookup"><span data-stu-id="60853-212">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="60853-213">Коллекция слабо типизированных данных можно использовать для передачи небольших объемов данных и из него контроллеры и представления.</span><span class="sxs-lookup"><span data-stu-id="60853-213">You can use the collection of weakly-typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="60853-214">Передача данных между...</span><span class="sxs-lookup"><span data-stu-id="60853-214">Passing data between a ...</span></span>                        | <span data-ttu-id="60853-215">Пример</span><span class="sxs-lookup"><span data-stu-id="60853-215">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="60853-216">Контроллером и представлением</span><span class="sxs-lookup"><span data-stu-id="60853-216">Controller and a view</span></span>                             | <span data-ttu-id="60853-217">Заполнение раскрывающегося списка с данными.</span><span class="sxs-lookup"><span data-stu-id="60853-217">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="60853-218">Просмотр и [режим разметки](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="60853-218">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="60853-219">Установка  **\<title >** содержимое элемента в режиме разметки из файла представления.</span><span class="sxs-lookup"><span data-stu-id="60853-219">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="60853-220">[Частичное представление](xref:mvc/views/partial) и представления</span><span class="sxs-lookup"><span data-stu-id="60853-220">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="60853-221">Мини-приложение, отображающий данные, основанные на веб-страницы по запросу пользователя.</span><span class="sxs-lookup"><span data-stu-id="60853-221">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="60853-222">Эта коллекция позволяет ссылаться на через `ViewData` или `ViewBag` свойства для контроллеров и представлений.</span><span class="sxs-lookup"><span data-stu-id="60853-222">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="60853-223">`ViewData` Свойство представляет собой словарь слабо типизированных объектов.</span><span class="sxs-lookup"><span data-stu-id="60853-223">The `ViewData` property is a dictionary of weakly-typed objects.</span></span> <span data-ttu-id="60853-224">`ViewBag` Свойство является оболочкой вокруг `ViewData` , предоставляющий динамических свойств для базового `ViewData` коллекции.</span><span class="sxs-lookup"><span data-stu-id="60853-224">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span>

<span data-ttu-id="60853-225">`ViewData`и `ViewBag` динамически разрешаются во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="60853-225">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="60853-226">Так как они не обеспечивают проверку типов во время компиляции, они обычно более ошибкам, чем с помощью viewmodel.</span><span class="sxs-lookup"><span data-stu-id="60853-226">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="60853-227">По этой причине некоторые разработчики предпочитают минимально или никогда не использовать `ViewData` и `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="60853-227">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>


<a name="VD"></a>

<span data-ttu-id="60853-228">**ViewData**</span><span class="sxs-lookup"><span data-stu-id="60853-228">**ViewData**</span></span>

<span data-ttu-id="60853-229">`ViewData`— [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) объектов, доступных через `string` ключей.</span><span class="sxs-lookup"><span data-stu-id="60853-229">`ViewData` is a [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="60853-230">Строковые данные могут храниться и использоваться напрямую не требуется для приведения к типу, но другие необходимо привести `ViewData` значений для конкретных типов объектов, при их извлечении.</span><span class="sxs-lookup"><span data-stu-id="60853-230">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="60853-231">Можно использовать `ViewData` передачи данных из контроллеры с представлениями и в пределах представления, включая [разделяемые представления](xref:mvc/views/partial) и [макеты](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="60853-231">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="60853-232">Ниже приведен пример, который задает значения для приветствие и адрес с помощью `ViewData` в действии:</span><span class="sxs-lookup"><span data-stu-id="60853-232">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

<span data-ttu-id="60853-233">Работать с данными в представлении:</span><span class="sxs-lookup"><span data-stu-id="60853-233">Work with the data in a view:</span></span>

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

<span data-ttu-id="60853-234">**ViewBag**</span><span class="sxs-lookup"><span data-stu-id="60853-234">**ViewBag**</span></span>

<span data-ttu-id="60853-235">Примечание: `ViewBag` доступен не на страницах Razor.</span><span class="sxs-lookup"><span data-stu-id="60853-235">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="60853-236">`ViewBag`— [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) объект, предоставляющий динамический доступ для объектов, хранящихся в `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="60853-236">`ViewBag` is a [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="60853-237">`ViewBag`может быть удобнее работать с, поскольку он не требует приведения.</span><span class="sxs-lookup"><span data-stu-id="60853-237">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="60853-238">В следующем примере показано, как использовать `ViewBag` с тем же, как с помощью `ViewData` выше:</span><span class="sxs-lookup"><span data-stu-id="60853-238">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

<span data-ttu-id="60853-239">**Одновременное использование ViewData и ViewBag**</span><span class="sxs-lookup"><span data-stu-id="60853-239">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="60853-240">Примечание: `ViewBag` доступен не на страницах Razor.</span><span class="sxs-lookup"><span data-stu-id="60853-240">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="60853-241">Поскольку `ViewData` и `ViewBag` ссылаться на тот же базовый `ViewData` коллекции, можно использовать оба `ViewData` и `ViewBag` его смешивать и сопоставление между ними при чтении и записи значения.</span><span class="sxs-lookup"><span data-stu-id="60853-241">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="60853-242">Установка заголовка с помощью `ViewBag` и описание с помощью `ViewData` в верхней части *About.cshtml* представления:</span><span class="sxs-lookup"><span data-stu-id="60853-242">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="60853-243">Чтение свойств, но обратная использование `ViewData` и `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="60853-243">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="60853-244">В *_Layout.cshtml* файла, получите title, используя `ViewData` и получить описание с помощью `ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="60853-244">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="60853-245">Помните, что строки не требуют приведение для `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="60853-245">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="60853-246">Можно использовать `@ViewData["Title"]` без приведения.</span><span class="sxs-lookup"><span data-stu-id="60853-246">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="60853-247">Использование обеих `ViewData` и `ViewBag` в одном works времени, что смешивание и сопоставление чтения и записи свойства.</span><span class="sxs-lookup"><span data-stu-id="60853-247">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="60853-248">Подготавливается к просмотру следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="60853-248">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="60853-249">**Представление различий между ViewData и ViewBag**</span><span class="sxs-lookup"><span data-stu-id="60853-249">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="60853-250">`ViewBag`доступен не на страницах Razor.</span><span class="sxs-lookup"><span data-stu-id="60853-250">`ViewBag` is not available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="60853-251">Является производным от [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), поэтому он поддерживает словарь свойств, которые можно использовать, такие как `ContainsKey`, `Add`, `Remove`, и `Clear`.</span><span class="sxs-lookup"><span data-stu-id="60853-251">Derives from [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="60853-252">Ключи в словаре являются строками, поэтому может быть пробелов.</span><span class="sxs-lookup"><span data-stu-id="60853-252">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="60853-253">Пример: `ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="60853-253">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="60853-254">Любой тип, отличный от `string` должны быть приведены в представлении, чтобы использовать `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="60853-254">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="60853-255">Является производным от [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), поэтому оно позволяет создавать динамические свойства, с помощью точечной нотации (`@ViewBag.SomeKey = <value or object>`), и приведение не требуется.</span><span class="sxs-lookup"><span data-stu-id="60853-255">Derives from [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="60853-256">Синтаксис `ViewBag` позволяет быстрее добавить контроллеры и представления.</span><span class="sxs-lookup"><span data-stu-id="60853-256">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="60853-257">Проще проверять значения null.</span><span class="sxs-lookup"><span data-stu-id="60853-257">Simpler to check for null values.</span></span> <span data-ttu-id="60853-258">Пример: `@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="60853-258">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="60853-259">**Когда следует использовать ViewData или ViewBag**</span><span class="sxs-lookup"><span data-stu-id="60853-259">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="60853-260">Оба `ViewData` и `ViewBag` не менее допустимые подходы для передачи небольших объемов данных между контроллерами и представлениями.</span><span class="sxs-lookup"><span data-stu-id="60853-260">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="60853-261">Выбрать один из них зависит от приоритета.</span><span class="sxs-lookup"><span data-stu-id="60853-261">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="60853-262">Можно комбинировать и сопоставлять `ViewData` и `ViewBag` объектов, тем не менее, код проще для понимания и обслуживания с одним из подходов использовать постоянно.</span><span class="sxs-lookup"><span data-stu-id="60853-262">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="60853-263">Оба подхода используют динамически разрешенных во время выполнения и таким образом подвержена приводит к ошибкам времени выполнения.</span><span class="sxs-lookup"><span data-stu-id="60853-263">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="60853-264">Некоторые команды разработчиков избежать их.</span><span class="sxs-lookup"><span data-stu-id="60853-264">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="60853-265">Динамические представления</span><span class="sxs-lookup"><span data-stu-id="60853-265">Dynamic views</span></span>

<span data-ttu-id="60853-266">Представления, которые не следует объявлять модели типа с помощью `@model` и которые имеют экземпляр модели, переданные им (например, `return View(Address);`) могут ссылаться на свойства этого экземпляра динамически:</span><span class="sxs-lookup"><span data-stu-id="60853-266">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="60853-267">Этот компонент обеспечивает гибкость, но не предлагает защиту компиляции или IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="60853-267">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="60853-268">Если свойство не существует, происходит сбой создания веб-страницы во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="60853-268">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="60853-269">Дополнительные возможности представления</span><span class="sxs-lookup"><span data-stu-id="60853-269">More view features</span></span>

<span data-ttu-id="60853-270">[Вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro) облегчить добавление поведения на стороне сервера в существующий HTML-теги.</span><span class="sxs-lookup"><span data-stu-id="60853-270">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="60853-271">Использование вспомогательных функций тегов позволяет избежать необходимости писать пользовательский код или вспомогательные объекты внутри представлений.</span><span class="sxs-lookup"><span data-stu-id="60853-271">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="60853-272">Вспомогательных функций тегов применяются как атрибуты для HTML-элементов и редакторов, которые не удается обработать их, учитываются.</span><span class="sxs-lookup"><span data-stu-id="60853-272">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="60853-273">Это позволяет редактировать и визуализации разметки представления в различных средств.</span><span class="sxs-lookup"><span data-stu-id="60853-273">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="60853-274">Создание настраиваемой HTML-разметки, может осуществляться с многие встроенные вспомогательные методы HTML.</span><span class="sxs-lookup"><span data-stu-id="60853-274">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="60853-275">Более сложная логика пользовательского интерфейса могут быть обработаны [Просмотр компонентов](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="60853-275">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="60853-276">Просмотр компонентов содержат же SoC, контроллеры и представления предоставляют.</span><span class="sxs-lookup"><span data-stu-id="60853-276">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="60853-277">Их можно устранить потребность в действиями и представлениями, обрабатывающих данные, используемые Общие элементы пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="60853-277">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="60853-278">Как и многие другие аспекты ASP.NET Core представления поддерживают [внедрения зависимостей](xref:fundamentals/dependency-injection), позволяя службам для [внедрены в представлениях](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="60853-278">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
