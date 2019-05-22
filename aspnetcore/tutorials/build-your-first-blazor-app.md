---
title: Создание приложения Blazor
author: guardrex
description: Пошаговое создание приложения Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: d48b891127f4db929b631c0ddf199c07658e604c
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970123"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="0ecc8-103">Создание приложения Blazor</span><span class="sxs-lookup"><span data-stu-id="0ecc8-103">Build your first Blazor app</span></span>

<span data-ttu-id="0ecc8-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0ecc8-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0ecc8-105">В этом руководстве показано, как создать и изменить приложение Blazor.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="0ecc8-106">Следуйте указаниям из статьи <xref:blazor/get-started>, чтобы создать проект Blazor для этого руководства.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span>

## <a name="build-components"></a><span data-ttu-id="0ecc8-107">Сборка компонентов</span><span class="sxs-lookup"><span data-stu-id="0ecc8-107">Build components</span></span>

1. <span data-ttu-id="0ecc8-108">Перейдите на каждую из трех страниц, размещенных в папке *Pages*: домашнюю, счетчика и получения данных.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-108">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="0ecc8-109">Эти страницы реализованы в виде файлов компонентов Razor *Index.razor*, *Counter.razor* и *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-109">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="0ecc8-110">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-110">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="0ecc8-111">Обычно для увеличения значений счетчика на веб-странице требуется код JavaScript, но Blazor реализует более правильный подход с использованием C#.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-111">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="0ecc8-112">Изучите реализацию компонента Counter в файле *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-112">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="0ecc8-113">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="0ecc8-113">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="0ecc8-114">Пользовательский интерфейс компонента Counter определяется с помощью HTML.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-114">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="0ecc8-115">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="0ecc8-115">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="0ecc8-116">HTML-разметка и логика отображения C# преобразуются в класс компонента во время сборки.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-116">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="0ecc8-117">Имя создаваемого класса .NET совпадает с именем файла.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-117">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="0ecc8-118">Элементы класса компонента определяются в блоке `@functions`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-118">Members of the component class are defined in an `@functions` block.</span></span> <span data-ttu-id="0ecc8-119">В блоке `@functions` указываются состояние (свойства, поля) и методы компонента для обработки событий или определения другой логики компонента.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-119">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="0ecc8-120">Эти элементы можно позднее включить в логику отображения компонента и обработки событий.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-120">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="0ecc8-121">При нажатии кнопки **Click me** (Щелкните здесь) выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="0ecc8-121">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="0ecc8-122">Вызывается обработчик `onclick`, зарегистрированный для компонента Counter (метод `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="0ecc8-122">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="0ecc8-123">Компонент Counter повторно создает свое дерево отображения.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-123">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="0ecc8-124">Выполняется сравнение нового и старого деревьев отображения.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-124">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="0ecc8-125">Применяются только изменения модели DOM (Document Object Model — объектная модель документа).</span><span class="sxs-lookup"><span data-stu-id="0ecc8-125">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="0ecc8-126">Отображаемое значение счетчика обновляется.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-126">The displayed count is updated.</span></span>

1. <span data-ttu-id="0ecc8-127">Измените логику C# для компонента Counter, чтобы при нажатии кнопки значение счетчика увеличивалось на две единицы вместо одной.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-127">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="0ecc8-128">Скомпилируйте и запустите приложение, чтобы увидеть изменения.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-128">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="0ecc8-129">Нажмите кнопку **Click Me** (Щелкните здесь).</span><span class="sxs-lookup"><span data-stu-id="0ecc8-129">Select the **Click me** button.</span></span> <span data-ttu-id="0ecc8-130">Значение счетчика увеличится на две единицы.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-130">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="0ecc8-131">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="0ecc8-131">Use components</span></span>

<span data-ttu-id="0ecc8-132">Включите компонент в другой компонент, используя синтаксис HTML.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-132">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="0ecc8-133">Добавьте компонент Counter в компонент Index, разместив элемент `<Counter />` внутри компонента Index (*Index.razor*).</span><span class="sxs-lookup"><span data-stu-id="0ecc8-133">Add the Counter component to the app's Index component by adding a `<Counter />` element to the Index component (*Index.razor*).</span></span>

   <span data-ttu-id="0ecc8-134">Если вы используете для этой задачи клиентскую часть Blazor, в компонент Index добавляется компонент Survey Prompt (элемент `<SurveyPrompt>`).</span><span class="sxs-lookup"><span data-stu-id="0ecc8-134">If you're using Blazor client-side for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="0ecc8-135">Замените элемент `<SurveyPrompt>` элементом `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-135">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span> <span data-ttu-id="0ecc8-136">Если вы используете для этой задачи серверное приложение Blazor, добавьте к компоненту Index элемент `<Counter>`:</span><span class="sxs-lookup"><span data-stu-id="0ecc8-136">If you're using a Blazor server-side app for this experience, add the `<Counter>` element to the Index component:</span></span>

   <span data-ttu-id="0ecc8-137">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="0ecc8-137">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="0ecc8-138">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-138">Rebuild and run the app.</span></span> <span data-ttu-id="0ecc8-139">Компонент Index имеет свой собственный счетчик.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-139">The Index component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="0ecc8-140">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="0ecc8-140">Component parameters</span></span>

<span data-ttu-id="0ecc8-141">Компоненты также могут принимать параметры.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-141">Components can also have parameters.</span></span> <span data-ttu-id="0ecc8-142">Параметры для компонентов определяются с помощью частных свойств в классе компонента с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-142">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="0ecc8-143">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-143">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="0ecc8-144">Обновите код C# для `@functions` в компоненте:</span><span class="sxs-lookup"><span data-stu-id="0ecc8-144">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="0ecc8-145">Добавьте свойство `IncrementAmount` с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-145">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="0ecc8-146">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-146">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="0ecc8-147">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="0ecc8-147">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="0ecc8-148">Укажите параметр `IncrementAmount` в элементе `<Counter>` для компонента домашней страницы, используя соответствующий атрибут.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-148">Specify an `IncrementAmount` parameter in the Index component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="0ecc8-149">Установите приращение счетчика на десять.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-149">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="0ecc8-150">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="0ecc8-150">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="0ecc8-151">Перезагрузите компонент Index.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-151">Reload the Index component.</span></span> <span data-ttu-id="0ecc8-152">Теперь значение счетчика на домашней странице будет увеличиваться на десять при каждом нажатии кнопки **Click me** (Щелкните здесь).</span><span class="sxs-lookup"><span data-stu-id="0ecc8-152">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="0ecc8-153">Счетчик в компоненте Counter продолжает увеличиваться на единицу.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-153">The counter in the Counter component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="0ecc8-154">Маршрутизация к компонентам</span><span class="sxs-lookup"><span data-stu-id="0ecc8-154">Route to components</span></span>

<span data-ttu-id="0ecc8-155">Директива `@page` в верхней части файла *Counter.razor* указывает, что этот компонент является конечной точкой маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-155">The `@page` directive at the top of the *Counter.razor* file specifies that the Counter component is a routing endpoint.</span></span> <span data-ttu-id="0ecc8-156">Компонент Counter обрабатывает запросы, направляемые к `/counter`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-156">The Counter component handles requests sent to `/counter`.</span></span> <span data-ttu-id="0ecc8-157">Без директивы `@page` этот компонент не будет обрабатывать перенаправленные запросы, но останется доступным для использования другими компонентами.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-157">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="0ecc8-158">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="0ecc8-158">Dependency injection</span></span>

<span data-ttu-id="0ecc8-159">Службы, зарегистрированные в контейнере служб приложения, доступны компонентам через [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0ecc8-159">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="0ecc8-160">Внедрите службы в компонент с помощью директивы `@inject`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-160">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="0ecc8-161">Изучите директивы компонента FetchData.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-161">Examine the directives of the FetchData component.</span></span>

<span data-ttu-id="0ecc8-162">Если вы используете серверное приложение Razor, служба `WeatherForecastService` зарегистрирована как [отдельная](xref:fundamentals/dependency-injection#service-lifetimes), то есть для всего приложения предоставляется один экземпляр этой службы.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-162">If working with a Blazor server-side app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="0ecc8-163">Директива `@inject` используется для внедрения экземпляра службы `WeatherForecastService` в компонент.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-163">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="0ecc8-164">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="0ecc8-164">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="0ecc8-165">Компонент FetchData использует внедренную службу как `ForecastService`, чтобы получить массив объектов `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-165">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="0ecc8-166">Если вы работаете с приложением Blazor на стороне клиента, внедряется `HttpClient` для получения данных прогноза погоды из файла *weather.json*, расположенного в папке *wwwroot/sample-data*.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-166">If working with a Blazor client-side app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="0ecc8-167">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="0ecc8-167">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="0ecc8-168">Цикл [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) используется для отображения каждого экземпляра прогноза в отдельной строке в таблице погоды.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-168">A [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a><span data-ttu-id="0ecc8-169">Создание списка дел</span><span class="sxs-lookup"><span data-stu-id="0ecc8-169">Build a todo list</span></span>

<span data-ttu-id="0ecc8-170">Добавьте в приложение новый компонент, представляющий собой простой список дел.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-170">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="0ecc8-171">Добавьте пустой файл с именем *Todo.cshtml* в папку *Pages*.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-171">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="0ecc8-172">Задайте начальную разметку компонента.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-172">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="0ecc8-173">Добавьте компонент Todo на панель навигации.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-173">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="0ecc8-174">Компонент NavMenu (*Shared/NavMenu.razor*) используется в макете этого приложения.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-174">The NavMenu component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="0ecc8-175">Макетами называются компоненты, которые избавляют от дублирования содержимого в приложении.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-175">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="0ecc8-176">Для получения дополнительной информации см. <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-176">For more information, see <xref:blazor/layouts>.</span></span>

   <span data-ttu-id="0ecc8-177">Добавьте `<NavLink>` для компонента списка дел, добавив следующую разметку с элементом списка под существующими элементами списка в файл *Shared/NavMenu.razor*.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-177">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="0ecc8-178">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-178">Rebuild and run the app.</span></span> <span data-ttu-id="0ecc8-179">Посетите новую страницу Todo, чтобы убедиться, что ссылка на компонент Todo работает правильно.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-179">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="0ecc8-180">При создании приложения Blazor на стороне сервера добавьте пространство имен приложения в файл *\_Imports.razor*.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-180">If building a Blazor server-side app, add the app's namespace to the *\_Imports.razor* file.</span></span> <span data-ttu-id="0ecc8-181">Следующая инструкция `@using` предполагает, что пространство имен приложения является `WebApplication`:</span><span class="sxs-lookup"><span data-stu-id="0ecc8-181">The following `@using` statement assumes that the app's namespace is `WebApplication`:</span></span>

   ```cshtml
   @using WebApplication
   ```
   
   <span data-ttu-id="0ecc8-182">Клиентские приложения Blazor включают пространство имен приложения по умолчанию в файле *\_Imports.razor*.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-182">Blazor client-side apps include the app's namespace by default in the *\_Imports.razor* file.</span></span>

1. <span data-ttu-id="0ecc8-183">Добавьте в корень проекта файл *TodoItem.cs*, который будет размещать класс для элемента списка дел.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-183">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="0ecc8-184">Используйте следующий код C# для класса `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-184">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="0ecc8-185">Снова вернитесь к компоненту Todo (*Pages/Todo.razor*).</span><span class="sxs-lookup"><span data-stu-id="0ecc8-185">Return to the Todo component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="0ecc8-186">Добавьте поле для элементов списка дел в блоке `@functions`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-186">Add a field for the todo items in an `@functions` block.</span></span> <span data-ttu-id="0ecc8-187">Компонент Todo использует это поле для хранения состояния списка дел.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-187">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="0ecc8-188">Добавьте разметку неупорядоченного списка и цикл `foreach` для отображения каждого элемента списка дела в виде элемента списка.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-188">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="0ecc8-189">Приложению необходимы элементы пользовательского интерфейса для добавления элементов в список дел.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-189">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="0ecc8-190">Добавьте текстовое поле и кнопку под списком.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-190">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="0ecc8-191">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-191">Rebuild and run the app.</span></span> <span data-ttu-id="0ecc8-192">При нажатии кнопки **Add todo** (Добавить задачу) ничего не происходит, так как к кнопке не привязан обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-192">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="0ecc8-193">Добавьте метод `AddTodo` в компонент Todo и с помощью атрибута `onclick` зарегистрируйте его на нажатие кнопки.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-193">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="0ecc8-194">Теперь при нажатии кнопки вызывается метод C# `AddTodo`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-194">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="0ecc8-195">Чтобы получить заголовок нового элемента списка дел, добавьте строку поля `newTodo` и привяжите ее к значению вводимого текста с помощью атрибута `bind`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-195">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="0ecc8-196">Обновите метод `AddTodo`, чтобы добавить `TodoItem` с указываемым названием в список.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-196">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="0ecc8-197">Очистите значение текстового поля, задав пустую строку в качестве значения для `newTodo`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-197">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="0ecc8-198">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-198">Rebuild and run the app.</span></span> <span data-ttu-id="0ecc8-199">Добавьте несколько задач в список дел, чтобы проверить работу нового кода.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-199">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="0ecc8-200">Вы можете сделать текст заголовка для каждого элемента списка дел редактируемым, а по дополнительному флажку пользователь сможет отслеживать завершение задач.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-200">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="0ecc8-201">Добавьте флажок для каждого элемента списка дел и привяжите его значение к свойству `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-201">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="0ecc8-202">Замените `@todo.Title` элементом `<input>`, который привязан к `@todo.Title`.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-202">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="0ecc8-203">Чтобы проверить привязку значений, обновите заголовок `<h1>`, чтобы в нем отображалось количество незавершенных дел в списке (`IsDone` имеет значение `false`).</span><span class="sxs-lookup"><span data-stu-id="0ecc8-203">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="0ecc8-204">Готовый компонент Todo (*Pages/Todo.razor*).</span><span class="sxs-lookup"><span data-stu-id="0ecc8-204">The completed Todo component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="0ecc8-205">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-205">Rebuild and run the app.</span></span> <span data-ttu-id="0ecc8-206">Добавьте элементы в список дел, чтобы протестировать новый код.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-206">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="0ecc8-207">Публикация и развертывание приложения</span><span class="sxs-lookup"><span data-stu-id="0ecc8-207">Publish and deploy the app</span></span>

<span data-ttu-id="0ecc8-208">Чтобы опубликовать приложение, воспользуйтесь инструкцией из статьи <xref:host-and-deploy/blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="0ecc8-208">To publish the app, see <xref:host-and-deploy/blazor/index>.</span></span>
