---
title: "Просмотр компонентов"
author: rick-anderson
description: "Просмотр компонентов предполагается, что в любом у логики отрисовки для повторного использования."
keywords: "ASP.NET Core, просмотр компонентов частичного представления"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: ab4705b7-59d7-4f31-bc97-ea7f292fe926
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: bb8a889c66ec9ca0c0aec7b4a4184d7c19858d78
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="view-components"></a><span data-ttu-id="5888e-104">Просмотр компонентов</span><span class="sxs-lookup"><span data-stu-id="5888e-104">View components</span></span>

<span data-ttu-id="5888e-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="5888e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="5888e-106">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="5888e-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)

## <a name="introducing-view-components"></a><span data-ttu-id="5888e-107">Знакомство с приложением Просмотр компонентов</span><span class="sxs-lookup"><span data-stu-id="5888e-107">Introducing view components</span></span>

<span data-ttu-id="5888e-108">Новое в ASP.NET Core MVC, просмотр компонентов похожи на частичного представления, но они являются более мощными.</span><span class="sxs-lookup"><span data-stu-id="5888e-108">New to ASP.NET Core MVC, view components are similar to partial views, but they are much more powerful.</span></span> <span data-ttu-id="5888e-109">Просмотр компонентов не использовать привязки модели и только зависят от данных, которые предоставляют при вызове в него.</span><span class="sxs-lookup"><span data-stu-id="5888e-109">View components don’t use model binding, and only depend on the data you provide when calling into it.</span></span> <span data-ttu-id="5888e-110">Компонент представления:</span><span class="sxs-lookup"><span data-stu-id="5888e-110">A view component:</span></span>

* <span data-ttu-id="5888e-111">Подготавливает к просмотру фрагмента, а не весь ответ</span><span class="sxs-lookup"><span data-stu-id="5888e-111">Renders a chunk rather than a whole response</span></span>
* <span data-ttu-id="5888e-112">Включает в себя же разделения задач и преимущества тестирования найден между контроллером и представлением</span><span class="sxs-lookup"><span data-stu-id="5888e-112">Includes the same separation-of-concerns and testability benefits found between a controller and view</span></span>
* <span data-ttu-id="5888e-113">Может иметь параметры и бизнес-логики</span><span class="sxs-lookup"><span data-stu-id="5888e-113">Can have parameters and business logic</span></span>
* <span data-ttu-id="5888e-114">Обычно вызывается из макета страницы</span><span class="sxs-lookup"><span data-stu-id="5888e-114">Is typically invoked from a layout page</span></span>

<span data-ttu-id="5888e-115">Просмотр компонентов предназначены в любом, что у вас есть логики отрисовки для повторного использования, слишком сложен для частичного представления, такие как:</span><span class="sxs-lookup"><span data-stu-id="5888e-115">View components are intended anywhere you have reusable rendering logic that is too complex for a partial view, such as:</span></span>

* <span data-ttu-id="5888e-116">Меню динамических переходов</span><span class="sxs-lookup"><span data-stu-id="5888e-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="5888e-117">Облако тегов (где запрос к базе данных)</span><span class="sxs-lookup"><span data-stu-id="5888e-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="5888e-118">Панель имени входа</span><span class="sxs-lookup"><span data-stu-id="5888e-118">Login panel</span></span>
* <span data-ttu-id="5888e-119">Корзина для покупок</span><span class="sxs-lookup"><span data-stu-id="5888e-119">Shopping cart</span></span>
* <span data-ttu-id="5888e-120">Недавно опубликованные статьи</span><span class="sxs-lookup"><span data-stu-id="5888e-120">Recently published articles</span></span>
* <span data-ttu-id="5888e-121">Боковая панель содержимого в типичных блога</span><span class="sxs-lookup"><span data-stu-id="5888e-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="5888e-122">Имя входа панель, в которой будут отображены на каждой странице и Показать ссылки выйти из системы или журнала, в зависимости от журнала в состояние пользователя</span><span class="sxs-lookup"><span data-stu-id="5888e-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="5888e-123">Компонент представления состоит из двух частей: класс (обычно получается из [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) и в результате возвращается (обычно представление).</span><span class="sxs-lookup"><span data-stu-id="5888e-123">A view component consists of two parts: the class (typically derived from [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="5888e-124">Подобно контроллеров, представление компонента может быть POCO, но большинство разработчиков будет нужно воспользоваться преимуществами методов и свойств, доступных путем наследования от `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="5888e-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="5888e-125">Создание представления компонента</span><span class="sxs-lookup"><span data-stu-id="5888e-125">Creating a view component</span></span>

<span data-ttu-id="5888e-126">Этот раздел содержит основные требования, чтобы создать представление компонента.</span><span class="sxs-lookup"><span data-stu-id="5888e-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="5888e-127">Далее в этой статье мы будем проверьте каждый шаг подробно и создать представление компонента.</span><span class="sxs-lookup"><span data-stu-id="5888e-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="5888e-128">Класс компонента представления</span><span class="sxs-lookup"><span data-stu-id="5888e-128">The view component class</span></span>

<span data-ttu-id="5888e-129">Класс компонента представления могут создаваться с помощью любого из следующих:</span><span class="sxs-lookup"><span data-stu-id="5888e-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="5888e-130">Наследование от *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="5888e-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="5888e-131">Декорирования класс с `[ViewComponent]` атрибута или производные от класса с `[ViewComponent]` атрибута</span><span class="sxs-lookup"><span data-stu-id="5888e-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="5888e-132">Создание класса, где имя должно заканчиваться суффиксом *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="5888e-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="5888e-133">Как и контроллеров Просмотр компонентов должна быть public, не являющегося вложенным и неабстрактного классы.</span><span class="sxs-lookup"><span data-stu-id="5888e-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="5888e-134">Имя компонента представление — это имя класса с суффиксом «ViewComponent» удалены.</span><span class="sxs-lookup"><span data-stu-id="5888e-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="5888e-135">Его можно также явно указать с помощью `ViewComponentAttribute.Name` свойство.</span><span class="sxs-lookup"><span data-stu-id="5888e-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="5888e-136">Класс компонента представления:</span><span class="sxs-lookup"><span data-stu-id="5888e-136">A view component class:</span></span>

* <span data-ttu-id="5888e-137">Полностью поддерживает конструктор [внедрения зависимости](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="5888e-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="5888e-138">Не участвуют в жизненном цикле контроллер, то есть нельзя использовать [фильтры](../controllers/filters.md) в компоненте представления</span><span class="sxs-lookup"><span data-stu-id="5888e-138">Does not take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="5888e-139">Представление методов компонента</span><span class="sxs-lookup"><span data-stu-id="5888e-139">View component methods</span></span>

<span data-ttu-id="5888e-140">Компонент представления задает его логику в `InvokeAsync` метод, возвращающий `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="5888e-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="5888e-141">Параметры берутся непосредственно из вызова компонента представления, не из привязки модели.</span><span class="sxs-lookup"><span data-stu-id="5888e-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="5888e-142">Никогда не компонент представления непосредственно обрабатывает запрос.</span><span class="sxs-lookup"><span data-stu-id="5888e-142">A view component never directly handles a request.</span></span> <span data-ttu-id="5888e-143">Как правило, представление компонента инициализирует модель и передает его в представление, вызвав `View` метод.</span><span class="sxs-lookup"><span data-stu-id="5888e-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="5888e-144">Таким образом просмотрите методов компонента:</span><span class="sxs-lookup"><span data-stu-id="5888e-144">In summary, view component methods:</span></span>

* <span data-ttu-id="5888e-145">Определение `InvokeAsync` метод, возвращающий`IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="5888e-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="5888e-146">Обычно инициализирует модель и передает его в представление, вызвав `ViewComponent` `View` метод</span><span class="sxs-lookup"><span data-stu-id="5888e-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="5888e-147">Параметры берутся из вызывающего метода, не HTTP, нет привязки модели</span><span class="sxs-lookup"><span data-stu-id="5888e-147">Parameters come from the calling method, not HTTP, there is no model binding</span></span>
* <span data-ttu-id="5888e-148">Не доступны непосредственно как конечную точку HTTP, они вызываются из кода приложения (обычно в представление).</span><span class="sxs-lookup"><span data-stu-id="5888e-148">Are not reachable directly as an HTTP endpoint, they are invoked from your code (usually in a view).</span></span> <span data-ttu-id="5888e-149">Компонент представления никогда не обрабатывается запрос</span><span class="sxs-lookup"><span data-stu-id="5888e-149">A view component never handles a request</span></span>
* <span data-ttu-id="5888e-150">Перегружено подписи, а не все данные из текущего HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="5888e-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="5888e-151">Путь к представлению поиска</span><span class="sxs-lookup"><span data-stu-id="5888e-151">View search path</span></span>

<span data-ttu-id="5888e-152">Среда выполнения ищет в представлении в следующие пути:</span><span class="sxs-lookup"><span data-stu-id="5888e-152">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="5888e-153">Представления или\<controller_name > /Components/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="5888e-153">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="5888e-154">Представления, компонентов/Общие/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="5888e-154">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="5888e-155">Представление является именем по умолчанию для компонента представление *по умолчанию*, то есть файл представления обычно называться *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5888e-155">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="5888e-156">Можно указать другое имя представления, при создании компонента результат представления или при вызове `View` метода.</span><span class="sxs-lookup"><span data-stu-id="5888e-156">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="5888e-157">Мы рекомендуем, назовите файл представления *Default.cshtml* и использовать *компонентов/представления/Общие/\<view_component_name > /\<view_name >* пути.</span><span class="sxs-lookup"><span data-stu-id="5888e-157">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="5888e-158">`PriorityList` Использует компонент представления, используемые в данном примере *Views/Shared/Components/PriorityList/Default.cshtml* для этого представления view.</span><span class="sxs-lookup"><span data-stu-id="5888e-158">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="5888e-159">Вызов компонента представления</span><span class="sxs-lookup"><span data-stu-id="5888e-159">Invoking a view component</span></span>

<span data-ttu-id="5888e-160">Чтобы использовать компонент представления, следует вызвать следующий внутри представления:</span><span class="sxs-lookup"><span data-stu-id="5888e-160">To use the view component, call the following inside a view:</span></span>

```html
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="5888e-161">Параметры передаются `InvokeAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="5888e-161">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="5888e-162">`PriorityList` Из вызывается компонент представления, разработанные в статье *Views/Todo/Index.cshtml* файл представления.</span><span class="sxs-lookup"><span data-stu-id="5888e-162">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="5888e-163">В следующих `InvokeAsync` метод вызывается с двумя параметрами:</span><span class="sxs-lookup"><span data-stu-id="5888e-163">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="5888e-164">Вызов компонента представления как вспомогательный тега</span><span class="sxs-lookup"><span data-stu-id="5888e-164">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="5888e-165">Для ASP.NET Core 1.1 и более поздних версиях можно вызвать компонент представления как [вспомогательный тег](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="5888e-165">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="5888e-166">Преобразуется в стиле Pascal класса и метода параметры для вспомогательных функций тегов их [нижний регистр kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="5888e-166">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="5888e-167">Вспомогательный объект тег для вызова компонента представление использует `<vc></vc>` элемента.</span><span class="sxs-lookup"><span data-stu-id="5888e-167">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="5888e-168">Представления компонента указывается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5888e-168">The view component is specified as follows:</span></span>

```html
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="5888e-169">Примечание: Для использования в качестве тега вспомогательный компонент представления, необходимо зарегистрировать сборку, содержащую компонент представления с помощью `@addTagHelper` директивы.</span><span class="sxs-lookup"><span data-stu-id="5888e-169">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="5888e-170">Например, если представление компонент находится в сборке с именем «MyWebApp», добавьте следующую директиву для `_ViewImports.cshtml` файла:</span><span class="sxs-lookup"><span data-stu-id="5888e-170">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```csharp
@addTagHelper *, MyWebApp
```

<span data-ttu-id="5888e-171">Компонент представления можно зарегистрировать как вспомогательный тег в любой файл, который ссылается на компонент представления.</span><span class="sxs-lookup"><span data-stu-id="5888e-171">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="5888e-172">В разделе [управление область вспомогательный тега](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) Дополнительные сведения о том, как зарегистрировать вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="5888e-172">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="5888e-173">`InvokeAsync` Метод, используемый в этом учебнике:</span><span class="sxs-lookup"><span data-stu-id="5888e-173">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="5888e-174">В разметке вспомогательный тег:</span><span class="sxs-lookup"><span data-stu-id="5888e-174">In Tag Helper markup:</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="5888e-175">В примере выше `PriorityList` становится компонент представления `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="5888e-175">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="5888e-176">Параметры в компонент представления, передаются как атрибуты в нижний регистр kebab.</span><span class="sxs-lookup"><span data-stu-id="5888e-176">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="5888e-177">Вызов компонента представление непосредственно из контроллера</span><span class="sxs-lookup"><span data-stu-id="5888e-177">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="5888e-178">Просмотр компонентов обычно вызываются из представления, но их можно вызывать непосредственно из метода контроллера.</span><span class="sxs-lookup"><span data-stu-id="5888e-178">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="5888e-179">Хотя Просмотр компонентов следует определять конечные точки, например контроллеров, можно легко реализовать действия контроллера, который возвращает содержимое `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="5888e-179">While view components do not define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="5888e-180">В этом примере компонент представления вызывается непосредственно из контроллера:</span><span class="sxs-lookup"><span data-stu-id="5888e-180">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="5888e-181">Пошаговое руководство: Создание простого представления компонента</span><span class="sxs-lookup"><span data-stu-id="5888e-181">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="5888e-182">[Загрузить](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), построения и тестирования начальный код.</span><span class="sxs-lookup"><span data-stu-id="5888e-182">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="5888e-183">Это простой проект с `Todo` контроллер, который отображает список *Todo* элементов.</span><span class="sxs-lookup"><span data-stu-id="5888e-183">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Список ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="5888e-185">Добавьте класс ViewComponent</span><span class="sxs-lookup"><span data-stu-id="5888e-185">Add a ViewComponent class</span></span>

<span data-ttu-id="5888e-186">Создание *ViewComponents* папки и добавьте следующее `PriorityListViewComponent` класса:</span><span class="sxs-lookup"><span data-stu-id="5888e-186">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="5888e-187">Примечания к коду.</span><span class="sxs-lookup"><span data-stu-id="5888e-187">Notes on the code:</span></span>

* <span data-ttu-id="5888e-188">Представление классов компонентов может содержаться в **любой** папку в проекте.</span><span class="sxs-lookup"><span data-stu-id="5888e-188">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="5888e-189">Так как класс именем PriorityList**ViewComponent** заканчивается суффиксом **ViewComponent**, среда выполнения будет использовать строку «PriorityList» при ссылке на класс компонента из представления.</span><span class="sxs-lookup"><span data-stu-id="5888e-189">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="5888e-190">Я объясню, более подробно далее.</span><span class="sxs-lookup"><span data-stu-id="5888e-190">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="5888e-191">`[ViewComponent]` Атрибута можно изменить имя, используемое для ссылки на компонент представления.</span><span class="sxs-lookup"><span data-stu-id="5888e-191">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="5888e-192">Например, может с именем класса `XYZ` и применяется `ViewComponent` атрибута:</span><span class="sxs-lookup"><span data-stu-id="5888e-192">For example, we could have named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="5888e-193">`[ViewComponent]` Атрибут выше сообщает Выбор компонентов представления для использования имени `PriorityList` при поиске представлений, связанных с компонентом и строка «PriorityList» для ссылки на класс компонента из представления.</span><span class="sxs-lookup"><span data-stu-id="5888e-193">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="5888e-194">Я объясню, более подробно далее.</span><span class="sxs-lookup"><span data-stu-id="5888e-194">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="5888e-195">Компонент использует [внедрения зависимостей](../../fundamentals/dependency-injection.md) для предоставления контекста данных.</span><span class="sxs-lookup"><span data-stu-id="5888e-195">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="5888e-196">`InvokeAsync`предоставляет метод, который может вызываться из представления, который может занять произвольное число аргументов.</span><span class="sxs-lookup"><span data-stu-id="5888e-196">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="5888e-197">`InvokeAsync` Метод возвращает набор `ToDo` элементы, которые удовлетворяют `isDone` и `maxPriority` параметров.</span><span class="sxs-lookup"><span data-stu-id="5888e-197">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="5888e-198">Создание представления view компонента Razor</span><span class="sxs-lookup"><span data-stu-id="5888e-198">Create the view component Razor view</span></span>

* <span data-ttu-id="5888e-199">Создание *компонентовпредставления/Общие/* папки.</span><span class="sxs-lookup"><span data-stu-id="5888e-199">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="5888e-200">Эта папка **должен** называться *компонентов*.</span><span class="sxs-lookup"><span data-stu-id="5888e-200">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="5888e-201">Создание *представления/общие/компоненты или PriorityList* папки.</span><span class="sxs-lookup"><span data-stu-id="5888e-201">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="5888e-202">Этот имя папки должно соответствовать имени класса представления компонента или имя класса, за вычетом суффикс (если мы следовали правилам и использовать *ViewComponent* суффикса в имени класса).</span><span class="sxs-lookup"><span data-stu-id="5888e-202">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="5888e-203">Если вы использовали `ViewComponent` атрибут, имя класса пришлось бы соответствует обозначение атрибута.</span><span class="sxs-lookup"><span data-stu-id="5888e-203">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="5888e-204">Создание *Views/Shared/Components/PriorityList/Default.cshtml* представления Razor:[!code-html[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="5888e-204">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-html[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="5888e-205">Представления Razor принимает список `TodoItem` и отображает их.</span><span class="sxs-lookup"><span data-stu-id="5888e-205">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="5888e-206">Если компонент представления `InvokeAsync` метод не передает имя представления (как в нашем примере) *по умолчанию* используется для имени представления по соглашению.</span><span class="sxs-lookup"><span data-stu-id="5888e-206">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="5888e-207">Далее в этом учебнике я покажу, как передать имя представления.</span><span class="sxs-lookup"><span data-stu-id="5888e-207">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="5888e-208">Чтобы переопределить стили по умолчанию для определенного контроллера, добавления представления к папке контроллера представления (например *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="5888e-208">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="5888e-209">Если компонент представления зависит от конкретного контроллера, его можно добавить в папку конкретного контроллера (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="5888e-209">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="5888e-210">Добавить `div` содержащем вызов компонент списка приоритет в нижнюю часть *Views/Todo/index.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="5888e-210">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="5888e-211">Разметка `@await Component.InvokeAsync` показан синтаксис для вызова компонентов представления.</span><span class="sxs-lookup"><span data-stu-id="5888e-211">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="5888e-212">Первым аргументом является имя компонента, который нужно вызвать или вызова.</span><span class="sxs-lookup"><span data-stu-id="5888e-212">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="5888e-213">Последующие аргументы передаются в компонент.</span><span class="sxs-lookup"><span data-stu-id="5888e-213">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="5888e-214">`InvokeAsync`может занять произвольное число аргументов.</span><span class="sxs-lookup"><span data-stu-id="5888e-214">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="5888e-215">Тестирование приложения.</span><span class="sxs-lookup"><span data-stu-id="5888e-215">Test the app.</span></span> <span data-ttu-id="5888e-216">На следующем рисунке показана ToDo list и элементы с приоритетом:</span><span class="sxs-lookup"><span data-stu-id="5888e-216">The following image shows the ToDo list and the priority items:</span></span>

![элементы списка и приоритет TODO](view-components/_static/pi.png)

<span data-ttu-id="5888e-218">Компонент представления также можно вызывать непосредственно из контроллера:</span><span class="sxs-lookup"><span data-stu-id="5888e-218">You can also call the view component directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![элементы с приоритетом от IndexVC действия](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="5888e-220">Указание имени представления</span><span class="sxs-lookup"><span data-stu-id="5888e-220">Specifying a view name</span></span>

<span data-ttu-id="5888e-221">Сложные представление компонента может потребоваться указать представления не по умолчанию при некоторых условиях.</span><span class="sxs-lookup"><span data-stu-id="5888e-221">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="5888e-222">Ниже показано, как указание представления «PVC» из `InvokeAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="5888e-222">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="5888e-223">Обновление `InvokeAsync` метод `PriorityListViewComponent` класса.</span><span class="sxs-lookup"><span data-stu-id="5888e-223">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="5888e-224">Копировать *Views/Shared/Components/PriorityList/Default.cshtml* файл, чтобы представление с именем *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5888e-224">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="5888e-225">Добавьте заголовок, чтобы указать, что представление PVC используется.</span><span class="sxs-lookup"><span data-stu-id="5888e-225">Add a heading to indicate the PVC view is being used.</span></span>

[!code-html[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="5888e-226">Обновление *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5888e-226">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="5888e-227">Запустите приложение и проверить PVC представление.</span><span class="sxs-lookup"><span data-stu-id="5888e-227">Run the app and verify PVC view.</span></span>

![Компонент представления приоритет](view-components/_static/pvc.png)

<span data-ttu-id="5888e-229">Если представление PVC не отображается, убедитесь, что вы вызываете компонент представления с приоритетом 4 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="5888e-229">If the PVC view is not rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="5888e-230">Проверьте путь к представлению</span><span class="sxs-lookup"><span data-stu-id="5888e-230">Examine the view path</span></span>

* <span data-ttu-id="5888e-231">Измените параметр priority три или менее, чтобы представление priority не возвращается.</span><span class="sxs-lookup"><span data-stu-id="5888e-231">Change the priority parameter to three or less so the priority view is not returned.</span></span>
* <span data-ttu-id="5888e-232">Временно переименуйте *Views/Todo/Components/PriorityList/Default.cshtml* для *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5888e-232">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="5888e-233">Тестирование приложения, вы получите следующую ошибку:</span><span class="sxs-lookup"><span data-stu-id="5888e-233">Test the app, you'll get the following error:</span></span>

   <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' was not found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="5888e-234">Копировать *Views/Todo/Components/PriorityList/1Default.cshtml* для *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5888e-234">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="5888e-235">Добавить некоторую разметку для *Shared* Todo представление компонентное представление для указания представления — от *Shared* папки.</span><span class="sxs-lookup"><span data-stu-id="5888e-235">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="5888e-236">Тест **Shared** компонентное представление.</span><span class="sxs-lookup"><span data-stu-id="5888e-236">Test the **Shared** component view.</span></span>

![Вывод ToDo с Shared компонентное представление](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="5888e-238">Предотвращение magic строк</span><span class="sxs-lookup"><span data-stu-id="5888e-238">Avoiding magic strings</span></span>

<span data-ttu-id="5888e-239">Следует компилировать безопасности времени можно заменить имя компонента жестко представление с именем класса.</span><span class="sxs-lookup"><span data-stu-id="5888e-239">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="5888e-240">Создайте представление компонента без суффикса «ViewComponent»:</span><span class="sxs-lookup"><span data-stu-id="5888e-240">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="5888e-241">Добавить `using` инструкцию, чтобы ваш Razor просмотра файла, а `nameof` оператор:</span><span class="sxs-lookup"><span data-stu-id="5888e-241">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a><span data-ttu-id="5888e-242">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5888e-242">Additional Resources</span></span>

* [<span data-ttu-id="5888e-243">Внедрение зависимостей в представления</span><span class="sxs-lookup"><span data-stu-id="5888e-243">Dependency injection into views</span></span>](dependency-injection.md)
