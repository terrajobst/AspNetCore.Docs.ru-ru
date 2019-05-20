---
title: Создание приложения Blazor
author: guardrex
description: Пошаговое создание приложения Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: c1b142ebdbd85eb10ddf8c8b70edd9782732a4f1
ms.sourcegitcommit: 3ee6ee0051c3d2c8d47a58cb17eef1a84a4c46a0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621101"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="ccc6c-103">Создание приложения Blazor</span><span class="sxs-lookup"><span data-stu-id="ccc6c-103">Build your first Blazor app</span></span>

<span data-ttu-id="ccc6c-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ccc6c-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ccc6c-105">В этом руководстве показано, как создать и изменить приложение Blazor.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="ccc6c-106">Следуйте указаниям из статьи <xref:blazor/get-started>, чтобы создать проект Blazor для этого руководства.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span>

## <a name="build-components"></a><span data-ttu-id="ccc6c-107">Сборка компонентов</span><span class="sxs-lookup"><span data-stu-id="ccc6c-107">Build components</span></span>

1. <span data-ttu-id="ccc6c-108">Перейдите на каждую из трех страниц, размещенных в папке *Pages*: домашнюю, счетчика и получения данных.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-108">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="ccc6c-109">Эти страницы реализованы в виде файлов компонентов Razor *Index.razor*, *Counter.razor* и *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-109">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="ccc6c-110">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-110">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="ccc6c-111">Обычно для увеличения значений счетчика на веб-странице требуется код JavaScript, но Blazor реализует более правильный подход с использованием C#.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-111">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="ccc6c-112">Изучите реализацию компонента Counter в файле *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-112">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="ccc6c-113">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="ccc6c-113">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="ccc6c-114">Пользовательский интерфейс компонента Counter определяется с помощью HTML.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-114">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="ccc6c-115">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="ccc6c-115">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="ccc6c-116">HTML-разметка и логика отображения C# преобразуются в класс компонента во время сборки.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-116">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="ccc6c-117">Имя создаваемого класса .NET совпадает с именем файла.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-117">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="ccc6c-118">Элементы класса компонента определяются в блоке `@functions`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-118">Members of the component class are defined in an `@functions` block.</span></span> <span data-ttu-id="ccc6c-119">В блоке `@functions` указываются состояние (свойства, поля) и методы компонента для обработки событий или определения другой логики компонента.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-119">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="ccc6c-120">Эти элементы можно позднее включить в логику отображения компонента и обработки событий.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-120">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="ccc6c-121">При нажатии кнопки **Click me** (Щелкните здесь) выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="ccc6c-121">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="ccc6c-122">Вызывается обработчик `onclick`, зарегистрированный для компонента Counter (метод `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="ccc6c-122">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="ccc6c-123">Компонент Counter повторно создает свое дерево отображения.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-123">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="ccc6c-124">Выполняется сравнение нового и старого деревьев отображения.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-124">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="ccc6c-125">Применяются только изменения модели DOM (Document Object Model — объектная модель документа).</span><span class="sxs-lookup"><span data-stu-id="ccc6c-125">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="ccc6c-126">Отображаемое значение счетчика обновляется.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-126">The displayed count is updated.</span></span>

1. <span data-ttu-id="ccc6c-127">Измените логику C# для компонента Counter, чтобы при нажатии кнопки значение счетчика увеличивалось на две единицы вместо одной.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-127">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="ccc6c-128">Скомпилируйте и запустите приложение, чтобы увидеть изменения.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-128">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="ccc6c-129">Нажмите кнопку **Click Me** (Щелкните здесь).</span><span class="sxs-lookup"><span data-stu-id="ccc6c-129">Select the **Click me** button.</span></span> <span data-ttu-id="ccc6c-130">Значение счетчика увеличится на две единицы.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-130">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="ccc6c-131">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="ccc6c-131">Use components</span></span>

<span data-ttu-id="ccc6c-132">Включите компонент в другой компонент, используя синтаксис HTML.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-132">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="ccc6c-133">Добавьте компонент Counter в компонент Index, разместив элемент `<Counter />` внутри компонента Index (*Index.razor*).</span><span class="sxs-lookup"><span data-stu-id="ccc6c-133">Add the Counter component to the app's Index component by adding a `<Counter />` element to the Index component (*Index.razor*).</span></span>

   <span data-ttu-id="ccc6c-134">Если вы используете для этой задачи клиентскую часть Blazor, в компонент Index добавляется компонент Survey Prompt (элемент `<SurveyPrompt>`).</span><span class="sxs-lookup"><span data-stu-id="ccc6c-134">If you're using Blazor client-side for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="ccc6c-135">Замените элемент `<SurveyPrompt>` элементом `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-135">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span> <span data-ttu-id="ccc6c-136">Если вы используете для этой задачи серверное приложение Blazor, добавьте к компоненту Index элемент `<Counter>`:</span><span class="sxs-lookup"><span data-stu-id="ccc6c-136">If you're using a Blazor server-side app for this experience, add the `<Counter>` element to the Index component:</span></span>

   <span data-ttu-id="ccc6c-137">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="ccc6c-137">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="ccc6c-138">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-138">Rebuild and run the app.</span></span> <span data-ttu-id="ccc6c-139">Компонент Index имеет свой собственный счетчик.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-139">The Index component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="ccc6c-140">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="ccc6c-140">Component parameters</span></span>

<span data-ttu-id="ccc6c-141">Компоненты также могут принимать параметры.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-141">Components can also have parameters.</span></span> <span data-ttu-id="ccc6c-142">Параметры для компонентов определяются с помощью частных свойств в классе компонента с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-142">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="ccc6c-143">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-143">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="ccc6c-144">Обновите код C# для `@functions` в компоненте:</span><span class="sxs-lookup"><span data-stu-id="ccc6c-144">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="ccc6c-145">Добавьте свойство `IncrementAmount` с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-145">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="ccc6c-146">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-146">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="ccc6c-147">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="ccc6c-147">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="ccc6c-148">Укажите параметр `IncrementAmount` в элементе `<Counter>` для компонента домашней страницы, используя соответствующий атрибут.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-148">Specify an `IncrementAmount` parameter in the Index component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="ccc6c-149">Установите приращение счетчика на десять.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-149">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="ccc6c-150">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="ccc6c-150">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="ccc6c-151">Перезагрузите компонент Index.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-151">Reload the Index component.</span></span> <span data-ttu-id="ccc6c-152">Теперь значение счетчика на домашней странице будет увеличиваться на десять при каждом нажатии кнопки **Click me** (Щелкните здесь).</span><span class="sxs-lookup"><span data-stu-id="ccc6c-152">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="ccc6c-153">Счетчик в компоненте Counter продолжает увеличиваться на единицу.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-153">The counter in the Counter component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="ccc6c-154">Маршрутизация к компонентам</span><span class="sxs-lookup"><span data-stu-id="ccc6c-154">Route to components</span></span>

<span data-ttu-id="ccc6c-155">Директива `@page` в верхней части файла *Counter.razor* указывает, что этот компонент является конечной точкой маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-155">The `@page` directive at the top of the *Counter.razor* file specifies that the Counter component is a routing endpoint.</span></span> <span data-ttu-id="ccc6c-156">Компонент Counter обрабатывает запросы, направляемые к `/counter`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-156">The Counter component handles requests sent to `/counter`.</span></span> <span data-ttu-id="ccc6c-157">Без директивы `@page` этот компонент не будет обрабатывать перенаправленные запросы, но останется доступным для использования другими компонентами.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-157">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="ccc6c-158">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="ccc6c-158">Dependency injection</span></span>

<span data-ttu-id="ccc6c-159">Службы, зарегистрированные в контейнере служб приложения, доступны компонентам через [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ccc6c-159">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ccc6c-160">Внедрите службы в компонент с помощью директивы `@inject`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-160">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="ccc6c-161">Изучите директивы компонента FetchData.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-161">Examine the directives of the FetchData component.</span></span>

<span data-ttu-id="ccc6c-162">Если вы используете серверное приложение Razor, служба `WeatherForecastService` зарегистрирована как [отдельная](xref:fundamentals/dependency-injection#service-lifetimes), то есть для всего приложения предоставляется один экземпляр этой службы.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-162">If working with a Blazor server-side app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="ccc6c-163">Директива `@inject` используется для внедрения экземпляра службы `WeatherForecastService` в компонент.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-163">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="ccc6c-164">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="ccc6c-164">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="ccc6c-165">Компонент FetchData использует внедренную службу как `ForecastService`, чтобы получить массив объектов `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-165">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="ccc6c-166">Если вы работаете с приложением Blazor на стороне клиента, внедряется `HttpClient` для получения данных прогноза погоды из файла *weather.json*, расположенного в папке *wwwroot/sample-data*.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-166">If working with a Blazor client-side app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="ccc6c-167">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="ccc6c-167">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="ccc6c-168">Цикл [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) используется для отображения каждого экземпляра прогноза в отдельной строке в таблице погоды.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-168">A [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a><span data-ttu-id="ccc6c-169">Создание списка дел</span><span class="sxs-lookup"><span data-stu-id="ccc6c-169">Build a todo list</span></span>

<span data-ttu-id="ccc6c-170">Добавьте в приложение новый компонент, представляющий собой простой список дел.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-170">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="ccc6c-171">Добавьте пустой файл с именем *Todo.cshtml* в папку *Pages*.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-171">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="ccc6c-172">Задайте начальную разметку компонента.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-172">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="ccc6c-173">Добавьте компонент Todo на панель навигации.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-173">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="ccc6c-174">Компонент NavMenu (*Shared/NavMenu.razor*) используется в макете этого приложения.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-174">The NavMenu component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="ccc6c-175">Макетами называются компоненты, которые избавляют от дублирования содержимого в приложении.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-175">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="ccc6c-176">Для получения дополнительной информации см. <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-176">For more information, see <xref:blazor/layouts>.</span></span>

   <span data-ttu-id="ccc6c-177">Добавьте `<NavLink>` для компонента списка дел, добавив следующую разметку с элементом списка под существующими элементами списка в файл *Shared/NavMenu.razor*.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-177">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="ccc6c-178">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-178">Rebuild and run the app.</span></span> <span data-ttu-id="ccc6c-179">Посетите новую страницу Todo, чтобы убедиться, что ссылка на компонент Todo работает правильно.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-179">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="ccc6c-180">При создании приложения Blazor на стороне сервера добавьте пространство имен приложения в файл *\_Imports.razor*.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-180">If building a Blazor server-side app, add the app's namespace to the *\_Imports.razor* file.</span></span> <span data-ttu-id="ccc6c-181">Следующая инструкция `@using` предполагает, что пространство имен приложения является `WebApplication`:</span><span class="sxs-lookup"><span data-stu-id="ccc6c-181">The following `@using` statement assumes that the app's namespace is `WebApplication`:</span></span>

   ```cshtml
   @using WebApplication
   ```
   
   <span data-ttu-id="ccc6c-182">Клиентские приложения Blazor включают пространство имен приложения по умолчанию в файле *\_Imports.razor*.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-182">Blazor client-side apps include the app's namespace by default in the *\_Imports.razor* file.</span></span>

1. <span data-ttu-id="ccc6c-183">Добавьте в корень проекта файл *TodoItem.cs*, который будет размещать класс для элемента списка дел.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-183">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="ccc6c-184">Используйте следующий код C# для класса `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-184">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="ccc6c-185">Снова вернитесь к компоненту Todo (*Pages/Todo.razor*).</span><span class="sxs-lookup"><span data-stu-id="ccc6c-185">Return to the Todo component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="ccc6c-186">Добавьте поле для элементов списка дел в блоке `@functions`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-186">Add a field for the todo items in an `@functions` block.</span></span> <span data-ttu-id="ccc6c-187">Компонент Todo использует это поле для хранения состояния списка дел.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-187">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="ccc6c-188">Добавьте разметку неупорядоченного списка и цикл `foreach` для отображения каждого элемента списка дела в виде элемента списка.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-188">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="ccc6c-189">Приложению необходимы элементы пользовательского интерфейса для добавления элементов в список дел.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-189">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="ccc6c-190">Добавьте текстовое поле и кнопку под списком.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-190">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="ccc6c-191">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-191">Rebuild and run the app.</span></span> <span data-ttu-id="ccc6c-192">При нажатии кнопки **Add todo** (Добавить задачу) ничего не происходит, так как к кнопке не привязан обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-192">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="ccc6c-193">Добавьте метод `AddTodo` в компонент Todo и с помощью атрибута `onclick` зарегистрируйте его на нажатие кнопки.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-193">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="ccc6c-194">Теперь при нажатии кнопки вызывается метод C# `AddTodo`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-194">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="ccc6c-195">Чтобы получить заголовок нового элемента списка дел, добавьте строку поля `newTodo` и привяжите ее к значению вводимого текста с помощью атрибута `bind`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-195">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. <span data-ttu-id="ccc6c-196">Обновите метод `AddTodo`, чтобы добавить `TodoItem` с указываемым названием в список.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-196">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="ccc6c-197">Очистите значение текстового поля, задав пустую строку в качестве значения для `newTodo`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-197">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="ccc6c-198">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-198">Rebuild and run the app.</span></span> <span data-ttu-id="ccc6c-199">Добавьте несколько задач в список дел, чтобы проверить работу нового кода.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-199">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="ccc6c-200">Вы можете сделать текст заголовка для каждого элемента списка дел редактируемым, а по дополнительному флажку пользователь сможет отслеживать завершение задач.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-200">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="ccc6c-201">Добавьте флажок для каждого элемента списка дел и привяжите его значение к свойству `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-201">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="ccc6c-202">Замените `@todo.Title` элементом `<input>`, который привязан к `@todo.Title`.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-202">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="ccc6c-203">Чтобы проверить привязку значений, обновите заголовок `<h1>`, чтобы в нем отображалось количество незавершенных дел в списке (`IsDone` имеет значение `false`).</span><span class="sxs-lookup"><span data-stu-id="ccc6c-203">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="ccc6c-204">Готовый компонент Todo (*Pages/Todo.razor*).</span><span class="sxs-lookup"><span data-stu-id="ccc6c-204">The completed Todo component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="ccc6c-205">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-205">Rebuild and run the app.</span></span> <span data-ttu-id="ccc6c-206">Добавьте элементы в список дел, чтобы протестировать новый код.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-206">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="ccc6c-207">Публикация и развертывание приложения</span><span class="sxs-lookup"><span data-stu-id="ccc6c-207">Publish and deploy the app</span></span>

<span data-ttu-id="ccc6c-208">Чтобы опубликовать приложение, воспользуйтесь инструкцией из статьи <xref:host-and-deploy/blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="ccc6c-208">To publish the app, see <xref:host-and-deploy/blazor/index>.</span></span>
