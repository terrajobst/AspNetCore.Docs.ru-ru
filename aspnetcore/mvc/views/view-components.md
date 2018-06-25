---
title: Просмотр компонентов в ASP.NET Core
author: rick-anderson
description: Узнайте, как компоненты представления используются в ASP.NET Core и как добавить их в приложения.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/view-components
ms.openlocfilehash: 2b196d8d46942604d1c85eb5f2f073661e5acb30
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278366"
---
# <a name="view-components-in-aspnet-core"></a><span data-ttu-id="45c0a-103">Просмотр компонентов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="45c0a-103">View components in ASP.NET Core</span></span>

<span data-ttu-id="45c0a-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="45c0a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="45c0a-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="45c0a-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="view-components"></a><span data-ttu-id="45c0a-106">Компоненты представлений</span><span class="sxs-lookup"><span data-stu-id="45c0a-106">View components</span></span>

<span data-ttu-id="45c0a-107">Компоненты представлений похожи на частичные представления, но при этом гораздо функциональнее.</span><span class="sxs-lookup"><span data-stu-id="45c0a-107">View components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="45c0a-108">Компоненты представлений не используют привязку модели и зависят только от данных, предоставляемых при их вызове.</span><span class="sxs-lookup"><span data-stu-id="45c0a-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="45c0a-109">Эта статья написана для ASP.NET Core MVC, но компоненты представлений работают и в Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="45c0a-109">This article was written using ASP.NET Core MVC, but view components also work with Razor Pages.</span></span>

<span data-ttu-id="45c0a-110">Компонент представлений:</span><span class="sxs-lookup"><span data-stu-id="45c0a-110">A view component:</span></span>

* <span data-ttu-id="45c0a-111">Выполняет отрисовку фрагмента, а не всего отклика.</span><span class="sxs-lookup"><span data-stu-id="45c0a-111">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="45c0a-112">Включает те же преимущества разделения функций и тестирования, которые доступны между контроллером и представлением.</span><span class="sxs-lookup"><span data-stu-id="45c0a-112">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="45c0a-113">Может иметь параметры и бизнес-логику.</span><span class="sxs-lookup"><span data-stu-id="45c0a-113">Can have parameters and business logic.</span></span>
* <span data-ttu-id="45c0a-114">Обычно вызывается со страницы макета.</span><span class="sxs-lookup"><span data-stu-id="45c0a-114">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="45c0a-115">Компоненты представлений предназначены для случаев, когда имеется многоразовая логика отрисовки, которая слишком сложна для частичного представления, например:</span><span class="sxs-lookup"><span data-stu-id="45c0a-115">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="45c0a-116">Динамические меню навигации</span><span class="sxs-lookup"><span data-stu-id="45c0a-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="45c0a-117">Облако тегов (где выполняется запрос к базе данных)</span><span class="sxs-lookup"><span data-stu-id="45c0a-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="45c0a-118">Панель входа</span><span class="sxs-lookup"><span data-stu-id="45c0a-118">Login panel</span></span>
* <span data-ttu-id="45c0a-119">Корзина для покупок</span><span class="sxs-lookup"><span data-stu-id="45c0a-119">Shopping cart</span></span>
* <span data-ttu-id="45c0a-120">Недавно опубликованные статьи</span><span class="sxs-lookup"><span data-stu-id="45c0a-120">Recently published articles</span></span>
* <span data-ttu-id="45c0a-121">Содержимое боковой панели в типичном блоге</span><span class="sxs-lookup"><span data-stu-id="45c0a-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="45c0a-122">Панель входа, которая должна отображаться на каждой странице и содержит ссылки для выхода или входа в зависимости от состояния входа пользователя</span><span class="sxs-lookup"><span data-stu-id="45c0a-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="45c0a-123">Компонент представления состоит из двух частей: класса (обычно производного от [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) и возвращаемого результата (обычно это представление).</span><span class="sxs-lookup"><span data-stu-id="45c0a-123">A view component consists of two parts: the class (typically derived from [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="45c0a-124">Как и контроллеры, компонент представления может быть объектом POCO, но большинству разработчиков потребуются преимущества методов и свойств, доступные при наследовании от `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="45c0a-125">Создание компонента представления</span><span class="sxs-lookup"><span data-stu-id="45c0a-125">Creating a view component</span></span>

<span data-ttu-id="45c0a-126">Этот раздел содержит общие требования к созданию компонента представления.</span><span class="sxs-lookup"><span data-stu-id="45c0a-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="45c0a-127">Далее в этой статье мы подробно рассмотрим каждый шаг и создадим компонент представления.</span><span class="sxs-lookup"><span data-stu-id="45c0a-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="45c0a-128">Класс компонентов представлений</span><span class="sxs-lookup"><span data-stu-id="45c0a-128">The view component class</span></span>

<span data-ttu-id="45c0a-129">Класс компонентов представлений можно создать одним из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="45c0a-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="45c0a-130">Наследование от *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="45c0a-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="45c0a-131">Декорирование класса с помощью атрибута `[ViewComponent]` или наследование от класса с помощью атрибута `[ViewComponent]`</span><span class="sxs-lookup"><span data-stu-id="45c0a-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="45c0a-132">Создание класса, где имя заканчивается суффиксом *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="45c0a-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="45c0a-133">Как и контроллеры, компоненты представлений должны быть открытыми, невложенными и неабстрактными классами.</span><span class="sxs-lookup"><span data-stu-id="45c0a-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="45c0a-134">Имя компонента представления — это имя класса с удаленным суффиксом "ViewComponent".</span><span class="sxs-lookup"><span data-stu-id="45c0a-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="45c0a-135">Его можно также можно задать явно с помощью свойства `ViewComponentAttribute.Name`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="45c0a-136">Класс компонентов представлений:</span><span class="sxs-lookup"><span data-stu-id="45c0a-136">A view component class:</span></span>

* <span data-ttu-id="45c0a-137">Полностью поддерживает [внедрение зависимостей](../../fundamentals/dependency-injection.md) в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="45c0a-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="45c0a-138">Не участвует в жизненном цикле контроллера, поэтому в компоненте представления невозможно использовать [фильтры](../controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="45c0a-138">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="45c0a-139">Методы компонентов представлений</span><span class="sxs-lookup"><span data-stu-id="45c0a-139">View component methods</span></span>

<span data-ttu-id="45c0a-140">Компонент представления задает свою логику в методе `InvokeAsync`, возвращающем `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="45c0a-141">Параметры берутся непосредственно из вызова компонента представления, а не из привязки модели.</span><span class="sxs-lookup"><span data-stu-id="45c0a-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="45c0a-142">Компонент представления никогда не обрабатывает запрос напрямую.</span><span class="sxs-lookup"><span data-stu-id="45c0a-142">A view component never directly handles a request.</span></span> <span data-ttu-id="45c0a-143">Как правило, компонент представления инициализирует модель и передает ее в представление, вызвав метод `View`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="45c0a-144">Если кратко, методы компонентов представлений:</span><span class="sxs-lookup"><span data-stu-id="45c0a-144">In summary, view component methods:</span></span>

* <span data-ttu-id="45c0a-145">Определяют метод `InvokeAsync`, возвращающий `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="45c0a-146">Обычно инициализируют модель и передают ее в представление, вызвав метод `ViewComponent` `View`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="45c0a-147">Получают параметры из вызывающего метода, а не HTTP, так как привязка модели отсутствует.</span><span class="sxs-lookup"><span data-stu-id="45c0a-147">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="45c0a-148">Недоступны непосредственно в качестве конечной точки HTTP, вызываются из кода (обычно в представлении).</span><span class="sxs-lookup"><span data-stu-id="45c0a-148">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="45c0a-149">Компонент представления никогда не обрабатывает запрос.</span><span class="sxs-lookup"><span data-stu-id="45c0a-149">A view component never handles a request</span></span>
* <span data-ttu-id="45c0a-150">Перегружаются по сигнатуре, а не сведениям из текущего HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="45c0a-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="45c0a-151">Путь поиска представления</span><span class="sxs-lookup"><span data-stu-id="45c0a-151">View search path</span></span>

<span data-ttu-id="45c0a-152">Среда выполнения ищет представление по следующим путям:</span><span class="sxs-lookup"><span data-stu-id="45c0a-152">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="45c0a-153">Views/\<имя_контроллера>/Components/\<имя_компонента_представления>/\<имя_представления></span><span class="sxs-lookup"><span data-stu-id="45c0a-153">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="45c0a-154">Views/Shared/Components/\<имя_компонента_представления>/\<имя_представления></span><span class="sxs-lookup"><span data-stu-id="45c0a-154">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="45c0a-155">По умолчанию для компонента представления используется имя *Default*, то есть файл представления обычно называется *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-155">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="45c0a-156">При создании результата компонента представления или при вызове метода `View` можно указать другое имя представления.</span><span class="sxs-lookup"><span data-stu-id="45c0a-156">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="45c0a-157">Мы рекомендуем назвать файл представления *Default.cshtml* и использовать путь *Views/Shared/Components/\<имя_компонента_представления>/\<имя_представления>*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-157">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="45c0a-158">Компонент представления `PriorityList` в этом примере использует представление компонента представления *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-158">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="45c0a-159">Вызов компонента представления</span><span class="sxs-lookup"><span data-stu-id="45c0a-159">Invoking a view component</span></span>

<span data-ttu-id="45c0a-160">Чтобы использовать компонент представления, вызовите следующий код внутри представления:</span><span class="sxs-lookup"><span data-stu-id="45c0a-160">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="45c0a-161">Эти параметры передаются в метод `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-161">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="45c0a-162">Разрабатываемый в этой статье компонент представления `PriorityList` вызывается из файла представления *Views/Todo/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-162">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="45c0a-163">Следующий код вызывает метод `InvokeAsync` с двумя параметрами:</span><span class="sxs-lookup"><span data-stu-id="45c0a-163">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="45c0a-164">Вызов компонента представления в качестве вспомогательной функции тегов</span><span class="sxs-lookup"><span data-stu-id="45c0a-164">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="45c0a-165">Для ASP.NET Core 1.1 и более поздних версий можно вызвать компонент представления в качестве [вспомогательной функции тегов](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="45c0a-165">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="45c0a-166">Параметры методов и классов, указанные в стиле Pascal, для вспомогательных функций тегов преобразуются в [кебаб-стиль нижнего регистра](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="45c0a-166">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="45c0a-167">Вспомогательная функция тегов для вызова компонента представления использует элемент `<vc></vc>`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-167">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="45c0a-168">Компонент представления задан следующим образом:</span><span class="sxs-lookup"><span data-stu-id="45c0a-168">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="45c0a-169">Примечание. Для использования компонента представления в качестве вспомогательной функции тегов нужно зарегистрировать сборку, содержащую этот компонент представления, с помощью директивы `@addTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-169">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="45c0a-170">Например, если компонент представления находится в сборке с именем "MyWebApp", добавьте в файл `_ViewImports.cshtml` следующую директиву:</span><span class="sxs-lookup"><span data-stu-id="45c0a-170">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="45c0a-171">Зарегистрировать компонент представления в качестве вспомогательной функции тегов можно в любом файле, который ссылается на этот компонент представления.</span><span class="sxs-lookup"><span data-stu-id="45c0a-171">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="45c0a-172">Раздел [Управление областью вспомогательной функции тегов](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) содержит дополнительные сведения о том, как зарегистрировать вспомогательные функции тегов.</span><span class="sxs-lookup"><span data-stu-id="45c0a-172">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="45c0a-173">Метод `InvokeAsync`, используемый в этом учебнике:</span><span class="sxs-lookup"><span data-stu-id="45c0a-173">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="45c0a-174">В разметке вспомогательной функции тегов:</span><span class="sxs-lookup"><span data-stu-id="45c0a-174">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="45c0a-175">В предыдущем примере компонент представления `PriorityList` становится `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-175">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="45c0a-176">Параметры передаются в компонент представления как атрибуты в кебаб-стиле нижнего регистра.</span><span class="sxs-lookup"><span data-stu-id="45c0a-176">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="45c0a-177">Вызов компонента представления непосредственно из контроллера</span><span class="sxs-lookup"><span data-stu-id="45c0a-177">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="45c0a-178">Компоненты представлений обычно вызываются из представления, но их можно вызывать непосредственно из метода контроллера.</span><span class="sxs-lookup"><span data-stu-id="45c0a-178">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="45c0a-179">Если компоненты представлений не определяют конечные точки, например контроллеры, можно легко реализовать действие контроллера, возвращающее содержимое `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-179">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="45c0a-180">В этом примере компонент представления вызывается непосредственно из контроллера:</span><span class="sxs-lookup"><span data-stu-id="45c0a-180">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="45c0a-181">Пошаговое руководство. Создание простого компонента представления</span><span class="sxs-lookup"><span data-stu-id="45c0a-181">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="45c0a-182">[Скачайте](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) начальный код и выполните его сборку и тестирование.</span><span class="sxs-lookup"><span data-stu-id="45c0a-182">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="45c0a-183">Это простой проект с контроллером `Todo`, который отображает список элементов *дел*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-183">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Список дел](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="45c0a-185">Добавление класса ViewComponent</span><span class="sxs-lookup"><span data-stu-id="45c0a-185">Add a ViewComponent class</span></span>

<span data-ttu-id="45c0a-186">Создайте папку *ViewComponents* и добавьте следующий класс `PriorityListViewComponent`:</span><span class="sxs-lookup"><span data-stu-id="45c0a-186">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="45c0a-187">Примечания к коду:</span><span class="sxs-lookup"><span data-stu-id="45c0a-187">Notes on the code:</span></span>

* <span data-ttu-id="45c0a-188">Классы компонентов представлений могут находиться в **любой** папке проекта.</span><span class="sxs-lookup"><span data-stu-id="45c0a-188">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="45c0a-189">Так как имя класса PriorityList**ViewComponent** заканчивается суффиксом **ViewComponent**, среда выполнения использует строку "PriorityList" при ссылке на компонент класса из представления.</span><span class="sxs-lookup"><span data-stu-id="45c0a-189">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="45c0a-190">Я подробнее расскажу об этом чуть позже.</span><span class="sxs-lookup"><span data-stu-id="45c0a-190">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="45c0a-191">Атрибут `[ViewComponent]` может изменить имя, используемое для ссылки на компонент представления.</span><span class="sxs-lookup"><span data-stu-id="45c0a-191">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="45c0a-192">Например, мы могли назвать класс `XYZ` и применить атрибут `ViewComponent`:</span><span class="sxs-lookup"><span data-stu-id="45c0a-192">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="45c0a-193">Приведенный выше атрибут `[ViewComponent]` предписывает средству выбора компонентов представлений использовать имя `PriorityList` при поиске представлений, связанных с данным компонентом, и строку "PriorityList" при ссылке на компонент класса из представления.</span><span class="sxs-lookup"><span data-stu-id="45c0a-193">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="45c0a-194">Я подробнее расскажу об этом чуть позже.</span><span class="sxs-lookup"><span data-stu-id="45c0a-194">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="45c0a-195">Компонент использует [внедрение зависимостей](../../fundamentals/dependency-injection.md) для предоставления контекста данных.</span><span class="sxs-lookup"><span data-stu-id="45c0a-195">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="45c0a-196">`InvokeAsync` предоставляет метод, который возможно вызывать из представления и который может принять произвольное число аргументов.</span><span class="sxs-lookup"><span data-stu-id="45c0a-196">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="45c0a-197">Метод `InvokeAsync` возвращает набор элементов `ToDo`, удовлетворяющих параметрам `isDone` и `maxPriority`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-197">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="45c0a-198">Создание представления Razor для компонента представления</span><span class="sxs-lookup"><span data-stu-id="45c0a-198">Create the view component Razor view</span></span>

* <span data-ttu-id="45c0a-199">Создайте папку *Views/Shared/Components*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-199">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="45c0a-200">Она **должна**  называться *Components*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-200">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="45c0a-201">Создайте папку *Views/Shared/Components/PriorityList*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-201">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="45c0a-202">Ее имя должно соответствовать имени класса представлений компонентов или имени класса без суффикса (если мы следовали соглашению и использовали суффикс *ViewComponent* в имени класса).</span><span class="sxs-lookup"><span data-stu-id="45c0a-202">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="45c0a-203">Если вы использовали атрибут `ViewComponent`, имя класса должно соответствовать его обозначению.</span><span class="sxs-lookup"><span data-stu-id="45c0a-203">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="45c0a-204">Создайте представление Razor *Views/Shared/Components/PriorityList/Default.cshtml*: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="45c0a-204">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="45c0a-205">Представление Razor принимает список `TodoItem` и отображает их.</span><span class="sxs-lookup"><span data-stu-id="45c0a-205">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="45c0a-206">Если метод `InvokeAsync` компонента представления не передает имя представления (как в нашем примере), по соглашению используется имя *Default*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-206">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="45c0a-207">Далее в этом учебнике я покажу, как передать имя представления.</span><span class="sxs-lookup"><span data-stu-id="45c0a-207">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="45c0a-208">Чтобы переопределить стиль по умолчанию для определенного контроллера, добавьте представление в папку соответствующего контроллера (например, *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-208">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="45c0a-209">Если компонент представления связан с конкретным контроллером, его можно добавить в папку этого контроллера (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="45c0a-209">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="45c0a-210">Добавьте `div`, содержащий вызов компонента списка приоритетов, в конец файла *Views/Todo/index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="45c0a-210">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="45c0a-211">`@await Component.InvokeAsync` в разметке показывает синтаксис для вызова компонентов представлений.</span><span class="sxs-lookup"><span data-stu-id="45c0a-211">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="45c0a-212">Первым аргументом является имя компонента, который требуется вызвать.</span><span class="sxs-lookup"><span data-stu-id="45c0a-212">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="45c0a-213">Последующие параметры передаются в компонент.</span><span class="sxs-lookup"><span data-stu-id="45c0a-213">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="45c0a-214">`InvokeAsync` может занять произвольное число аргументов.</span><span class="sxs-lookup"><span data-stu-id="45c0a-214">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="45c0a-215">Проверьте работу приложения.</span><span class="sxs-lookup"><span data-stu-id="45c0a-215">Test the app.</span></span> <span data-ttu-id="45c0a-216">Следующий рисунок показывает список дел и элементы с приоритетом:</span><span class="sxs-lookup"><span data-stu-id="45c0a-216">The following image shows the ToDo list and the priority items:</span></span>

![список дел и элементы с приоритетом](view-components/_static/pi.png)

<span data-ttu-id="45c0a-218">Кроме того, компонент представления можно вызвать непосредственно из контроллера:</span><span class="sxs-lookup"><span data-stu-id="45c0a-218">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![элементы с приоритетом из действия IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="45c0a-220">Указание имени представления</span><span class="sxs-lookup"><span data-stu-id="45c0a-220">Specifying a view name</span></span>

<span data-ttu-id="45c0a-221">Сложному представлению компонента в некоторых условиях может потребоваться указать нестандартное представление.</span><span class="sxs-lookup"><span data-stu-id="45c0a-221">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="45c0a-222">Ниже показано, как указать представление "PVC" из метода `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-222">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="45c0a-223">Обновите метод `InvokeAsync` в классе `PriorityListViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="45c0a-223">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="45c0a-224">Скопируйте файл *Views/Shared/Components/PriorityList/Default.cshtml* в представление *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-224">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="45c0a-225">Добавьте заголовок, чтобы указать используемое представление PVC.</span><span class="sxs-lookup"><span data-stu-id="45c0a-225">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="45c0a-226">Измените *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="45c0a-226">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="45c0a-227">Запустите приложение и проверьте представление PVC.</span><span class="sxs-lookup"><span data-stu-id="45c0a-227">Run the app and verify PVC view.</span></span>

![Компонент представления с приоритетом](view-components/_static/pvc.png)

<span data-ttu-id="45c0a-229">Если представление PVC не отображается, убедитесь, что вы вызываете компонент представления с приоритетом 4 или выше.</span><span class="sxs-lookup"><span data-stu-id="45c0a-229">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="45c0a-230">Проверка пути к представлению</span><span class="sxs-lookup"><span data-stu-id="45c0a-230">Examine the view path</span></span>

* <span data-ttu-id="45c0a-231">Задайте для параметра приоритета значение 3 или меньше, чтобы представление с приоритетом не возвращалось.</span><span class="sxs-lookup"><span data-stu-id="45c0a-231">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="45c0a-232">Временно переименуйте *Views/Todo/Components/PriorityList/Default.cshtml* в *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-232">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="45c0a-233">Проверьте приложение, при этом происходит следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="45c0a-233">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="45c0a-234">Скопируйте *Views/Todo/Components/PriorityList/1Default.cshtml* в *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-234">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="45c0a-235">Добавьте разметку в представление компонента представления дел *Shared*, чтобы указать, что это представление из папки *Shared*.</span><span class="sxs-lookup"><span data-stu-id="45c0a-235">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="45c0a-236">Протестируйте представление компонента **Shared**.</span><span class="sxs-lookup"><span data-stu-id="45c0a-236">Test the **Shared** component view.</span></span>

![Выходные данные дел с представлением компонента Shared](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="45c0a-238">Предотвращение появления неочевидных строк</span><span class="sxs-lookup"><span data-stu-id="45c0a-238">Avoiding magic strings</span></span>

<span data-ttu-id="45c0a-239">Чтобы обеспечить безопасность во время компиляции, можно заменить жестко заданное имя компонента представления на имя класса.</span><span class="sxs-lookup"><span data-stu-id="45c0a-239">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="45c0a-240">Создайте компонент представления без суффикса "ViewComponent":</span><span class="sxs-lookup"><span data-stu-id="45c0a-240">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="45c0a-241">Добавьте инструкцию `using` в файл представления Razor и используйте оператор `nameof`:</span><span class="sxs-lookup"><span data-stu-id="45c0a-241">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="additional-resources"></a><span data-ttu-id="45c0a-242">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="45c0a-242">Additional resources</span></span>

* [<span data-ttu-id="45c0a-243">Внедрение зависимостей в представления</span><span class="sxs-lookup"><span data-stu-id="45c0a-243">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
