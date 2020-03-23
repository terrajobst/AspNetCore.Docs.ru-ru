---
title: Создание первого приложения Blazor
author: guardrex
description: Узнайте, как создать приложение Blazor по шагам.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2020
no-loc:
- Blazor
uid: tutorials/first-blazor-app
ms.openlocfilehash: 8b3802a6ffe3613e5d4ca65c57fafc3f404c8329
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434503"
---
# <a name="build-your-first-opno-locblazor-app"></a><span data-ttu-id="99b3b-103">Создание первого приложения Blazor</span><span class="sxs-lookup"><span data-stu-id="99b3b-103">Build your first Blazor app</span></span>

<span data-ttu-id="99b3b-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="99b3b-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="99b3b-105">В этом учебнике показано, как создать и изменить приложение Blazor.</span><span class="sxs-lookup"><span data-stu-id="99b3b-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

## <a name="build-components"></a><span data-ttu-id="99b3b-106">Сборка компонентов</span><span class="sxs-lookup"><span data-stu-id="99b3b-106">Build components</span></span>

1. <span data-ttu-id="99b3b-107">Следуйте указаниям из статьи <xref:blazor/get-started>, чтобы создать проект Blazor для этого учебника.</span><span class="sxs-lookup"><span data-stu-id="99b3b-107">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="99b3b-108">Назовите проект *ToDoList*.</span><span class="sxs-lookup"><span data-stu-id="99b3b-108">Name the project *ToDoList*.</span></span>

1. <span data-ttu-id="99b3b-109">Перейдите на каждую из трех страниц, размещенных в папке *Pages*: домашнюю, счетчика и получения данных.</span><span class="sxs-lookup"><span data-stu-id="99b3b-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="99b3b-110">Эти страницы реализованы в виде файлов компонентов Razor *Index.razor*, *Counter.razor* и *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="99b3b-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="99b3b-111">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="99b3b-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="99b3b-112">Обычно для увеличения значений счетчика на веб-странице требуется код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="99b3b-112">Incrementing a counter in a webpage normally requires writing JavaScript.</span></span> <span data-ttu-id="99b3b-113">Благодаря Blazor можно использовать вместо него C#.</span><span class="sxs-lookup"><span data-stu-id="99b3b-113">With Blazor, you can write C# instead.</span></span>

1. <span data-ttu-id="99b3b-114">Изучите реализацию компонента `Counter` в файле *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="99b3b-114">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="99b3b-115">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="99b3b-115">*Pages/Counter.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="99b3b-116">Пользовательский интерфейс компонента `Counter` определяется с помощью HTML.</span><span class="sxs-lookup"><span data-stu-id="99b3b-116">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="99b3b-117">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="99b3b-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="99b3b-118">HTML-разметка и логика отображения C# преобразуются в класс компонента во время сборки.</span><span class="sxs-lookup"><span data-stu-id="99b3b-118">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="99b3b-119">Имя создаваемого класса .NET совпадает с именем файла.</span><span class="sxs-lookup"><span data-stu-id="99b3b-119">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="99b3b-120">Элементы класса компонента определяются в блоке `@code`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="99b3b-121">В блоке `@code` указываются состояние (свойства, поля) и методы компонента для обработки событий или определения другой логики компонента.</span><span class="sxs-lookup"><span data-stu-id="99b3b-121">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="99b3b-122">Эти элементы можно позднее включить в логику отображения компонента и обработки событий.</span><span class="sxs-lookup"><span data-stu-id="99b3b-122">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="99b3b-123">При нажатии кнопки **Click me** (Щелкните здесь) выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="99b3b-123">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="99b3b-124">Вызывается обработчик `onclick`, зарегистрированный для компонента `Counter` (метод `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="99b3b-124">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="99b3b-125">Компонент `Counter` повторно создает свое дерево отображения.</span><span class="sxs-lookup"><span data-stu-id="99b3b-125">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="99b3b-126">Выполняется сравнение нового и старого деревьев отображения.</span><span class="sxs-lookup"><span data-stu-id="99b3b-126">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="99b3b-127">Применяются только изменения модели DOM (Document Object Model — объектная модель документа).</span><span class="sxs-lookup"><span data-stu-id="99b3b-127">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="99b3b-128">Отображаемое значение счетчика обновляется.</span><span class="sxs-lookup"><span data-stu-id="99b3b-128">The displayed count is updated.</span></span>

1. <span data-ttu-id="99b3b-129">Измените логику C# компонента `Counter`, чтобы при нажатии кнопки значение счетчика увеличивалось на две единицы вместо одной.</span><span class="sxs-lookup"><span data-stu-id="99b3b-129">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="99b3b-130">Скомпилируйте и запустите приложение, чтобы увидеть изменения.</span><span class="sxs-lookup"><span data-stu-id="99b3b-130">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="99b3b-131">Нажмите кнопку **Click Me** (Щелкните здесь).</span><span class="sxs-lookup"><span data-stu-id="99b3b-131">Select the **Click me** button.</span></span> <span data-ttu-id="99b3b-132">Значение счетчика увеличится на две единицы.</span><span class="sxs-lookup"><span data-stu-id="99b3b-132">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="99b3b-133">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="99b3b-133">Use components</span></span>

<span data-ttu-id="99b3b-134">Включите компонент в другой компонент, используя синтаксис HTML.</span><span class="sxs-lookup"><span data-stu-id="99b3b-134">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="99b3b-135">Добавьте компонент `Counter` в компонент `Index` приложения, разместив элемент `<Counter />` внутри компонента `Index` (*Index.razor*).</span><span class="sxs-lookup"><span data-stu-id="99b3b-135">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="99b3b-136">Если вы выполняете эту задачу с помощью WebAssembly Blazor, это значит, что компонент `SurveyPrompt` использует компонент `Index`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-136">If you're using Blazor WebAssembly for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="99b3b-137">Замените элемент `<SurveyPrompt>` элементом `<Counter />`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-137">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="99b3b-138">Если вы используете для этой задачи серверное приложение Blazor, добавьте к компоненту `Index` элемент `<Counter />`:</span><span class="sxs-lookup"><span data-stu-id="99b3b-138">If you're using a Blazor Server app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="99b3b-139">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="99b3b-139">*Pages/Index.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="99b3b-140">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="99b3b-140">Rebuild and run the app.</span></span> <span data-ttu-id="99b3b-141">Компонент `Index` имеет свой собственный счетчик.</span><span class="sxs-lookup"><span data-stu-id="99b3b-141">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="99b3b-142">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="99b3b-142">Component parameters</span></span>

<span data-ttu-id="99b3b-143">Компоненты также могут принимать параметры.</span><span class="sxs-lookup"><span data-stu-id="99b3b-143">Components can also have parameters.</span></span> <span data-ttu-id="99b3b-144">Параметры для компонентов определяются с помощью открытых свойств в классе компонента с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-144">Component parameters are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="99b3b-145">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="99b3b-145">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="99b3b-146">Обновите код C# для `@code` в компоненте следующим образом:</span><span class="sxs-lookup"><span data-stu-id="99b3b-146">Update the component's `@code` C# code as follows:</span></span>

   * <span data-ttu-id="99b3b-147">Добавьте открытое свойство `IncrementAmount` с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-147">Add a public `IncrementAmount` property with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="99b3b-148">Изменение метод `IncrementCount`, чтобы он использовал свойство `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-148">Change the `IncrementCount` method to use the `IncrementAmount` property when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="99b3b-149">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="99b3b-149">*Pages/Counter.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

   <!-- Add back when supported.
       > [!NOTE]
       > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
   -->

1. <span data-ttu-id="99b3b-150">Укажите параметр `IncrementAmount` в элементе `<Counter>` для компонента `Index`, используя атрибут.</span><span class="sxs-lookup"><span data-stu-id="99b3b-150">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="99b3b-151">Установите приращение счетчика на десять.</span><span class="sxs-lookup"><span data-stu-id="99b3b-151">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="99b3b-152">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="99b3b-152">*Pages/Index.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="99b3b-153">Перезагрузите компонент `Index`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-153">Reload the `Index` component.</span></span> <span data-ttu-id="99b3b-154">Теперь значение счетчика на домашней странице будет увеличиваться на десять при каждом нажатии кнопки **Click me** (Щелкните здесь).</span><span class="sxs-lookup"><span data-stu-id="99b3b-154">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="99b3b-155">Счетчик в компоненте `Counter` продолжает увеличиваться на единицу.</span><span class="sxs-lookup"><span data-stu-id="99b3b-155">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="99b3b-156">Маршрутизация к компонентам</span><span class="sxs-lookup"><span data-stu-id="99b3b-156">Route to components</span></span>

<span data-ttu-id="99b3b-157">Директива `@page` в верхней части файла *Counter.razor* указывает, что компонент `Counter` является конечной точкой маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="99b3b-157">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="99b3b-158">Компонент `Counter` обрабатывает запросы, направляемые к `/counter`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-158">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="99b3b-159">Без директивы `@page` этот компонент не будет обрабатывать перенаправленные запросы, но останется доступным для использования другими компонентами.</span><span class="sxs-lookup"><span data-stu-id="99b3b-159">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="99b3b-160">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="99b3b-160">Dependency injection</span></span>

### <a name="opno-locblazor-server-experience"></a><span data-ttu-id="99b3b-161">Взаимодействие с сервером Blazor</span><span class="sxs-lookup"><span data-stu-id="99b3b-161">Blazor Server experience</span></span>

<span data-ttu-id="99b3b-162">Если вы используете серверное приложение Blazor, служба `WeatherForecastService` зарегистрирована как [отдельная](xref:fundamentals/dependency-injection#service-lifetimes) в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-162">If working with a Blazor Server app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="99b3b-163">Экземпляр этой службы предоставляется для всего приложения посредством [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="99b3b-163">An instance of the service is available throughout the app via [dependency injection (DI)](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="99b3b-164">Директива `@inject` используется для внедрения экземпляра службы `WeatherForecastService` в компонент `FetchData`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-164">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the `FetchData` component.</span></span>

<span data-ttu-id="99b3b-165">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="99b3b-165">*Pages/FetchData.razor*:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="99b3b-166">Компонент `FetchData` использует внедренную службу как `ForecastService`, чтобы получить массив объектов `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-166">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="opno-locblazor-webassembly-experience"></a><span data-ttu-id="99b3b-167">Взаимодействие с WebAssembly Blazor</span><span class="sxs-lookup"><span data-stu-id="99b3b-167">Blazor WebAssembly experience</span></span>

<span data-ttu-id="99b3b-168">Если вы работаете с приложением WebAssembly Blazor, внедряется `HttpClient` для получения данных прогноза погоды из файла *weather.json*, расположенного в папке *wwwroot/sample-data*.</span><span class="sxs-lookup"><span data-stu-id="99b3b-168">If working with a Blazor WebAssembly app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder.</span></span>

<span data-ttu-id="99b3b-169">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="99b3b-169">*Pages/FetchData.razor*:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="99b3b-170">Цикл [`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) используется для отображения каждого экземпляра прогноза в отдельной строке в таблице погоды.</span><span class="sxs-lookup"><span data-stu-id="99b3b-170">An [`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="99b3b-171">Создание списка дел</span><span class="sxs-lookup"><span data-stu-id="99b3b-171">Build a todo list</span></span>

<span data-ttu-id="99b3b-172">Добавьте в приложение новый компонент, представляющий собой простой список дел.</span><span class="sxs-lookup"><span data-stu-id="99b3b-172">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="99b3b-173">Добавьте в приложение новый компонент Razor `Todo` в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="99b3b-173">Add a new `Todo` Razor component to the app in the *Pages* folder.</span></span> <span data-ttu-id="99b3b-174">Щелкните в Visual Studio правой кнопкой мыши папку **Pages** и выберите пункт **Добавить** > **Создать элемент** > **Компонент Razor**.</span><span class="sxs-lookup"><span data-stu-id="99b3b-174">In Visual Studio, right-click the **Pages** folder and select **Add** > **New Item** > **Razor Component**.</span></span> <span data-ttu-id="99b3b-175">Присвойте файлу компонента имя *Todo.Razor*.</span><span class="sxs-lookup"><span data-stu-id="99b3b-175">Name the component's file *Todo.razor*.</span></span> <span data-ttu-id="99b3b-176">В других средах разработки необходимо добавить в папку **Pages** пустой файл с именем *Todo.razor*.</span><span class="sxs-lookup"><span data-stu-id="99b3b-176">In other development environments, add a blank file to the **Pages** folder named *Todo.razor*.</span></span>

1. <span data-ttu-id="99b3b-177">Задайте начальную разметку компонента.</span><span class="sxs-lookup"><span data-stu-id="99b3b-177">Provide the initial markup for the component:</span></span>

   ```razor
   @page "/todo"

   <h3>Todo</h3>
   ```

1. <span data-ttu-id="99b3b-178">Добавьте компонент `Todo` на панель навигации.</span><span class="sxs-lookup"><span data-stu-id="99b3b-178">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="99b3b-179">Компонент `NavMenu` (*Shared/NavMenu.razor*) используется в макете этого приложения.</span><span class="sxs-lookup"><span data-stu-id="99b3b-179">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="99b3b-180">Макетами называются компоненты, которые избавляют от дублирования содержимого в приложении.</span><span class="sxs-lookup"><span data-stu-id="99b3b-180">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="99b3b-181">Добавьте `<NavLink>` для компонента `Todo`, включив следующую разметку с элементом списка под существующими элементами списка в файл *Shared/NavMenu.razor*.</span><span class="sxs-lookup"><span data-stu-id="99b3b-181">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="99b3b-182">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="99b3b-182">Rebuild and run the app.</span></span> <span data-ttu-id="99b3b-183">Перейдите на новую страницу Todo, чтобы убедиться, что ссылка на компонент `Todo` работает правильно.</span><span class="sxs-lookup"><span data-stu-id="99b3b-183">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="99b3b-184">Добавьте в корень проекта файл *TodoItem.cs*, который будет размещать класс для элемента списка дел.</span><span class="sxs-lookup"><span data-stu-id="99b3b-184">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="99b3b-185">Используйте следующий код C# для класса `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-185">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="99b3b-186">Снова вернитесь к компоненту `Todo` (*Pages/Todo.razor*).</span><span class="sxs-lookup"><span data-stu-id="99b3b-186">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="99b3b-187">Добавьте поле для элементов списка дел в блоке `@code`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-187">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="99b3b-188">Компонент `Todo` использует это поле для сохранения состояния списка дел.</span><span class="sxs-lookup"><span data-stu-id="99b3b-188">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="99b3b-189">Добавьте разметку неупорядоченного списка и цикл `foreach` для отображения каждого элемента списка дела в виде элемента списка (`<li>`).</span><span class="sxs-lookup"><span data-stu-id="99b3b-189">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="99b3b-190">Приложению необходимы элементы пользовательского интерфейса для добавления элементов в список дел.</span><span class="sxs-lookup"><span data-stu-id="99b3b-190">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="99b3b-191">Добавьте текстовое поле (`<input>`) и кнопку (`<button>`) под неупорядоченным списком (`<ul>...</ul>`).</span><span class="sxs-lookup"><span data-stu-id="99b3b-191">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="99b3b-192">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="99b3b-192">Rebuild and run the app.</span></span> <span data-ttu-id="99b3b-193">При нажатии кнопки **Add todo** (Добавить задачу) ничего не происходит, так как к кнопке не привязан обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="99b3b-193">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="99b3b-194">Добавьте метод `AddTodo` в компонент `Todo` и зарегистрируйте его для нажатий кнопки с помощью атрибута `@onclick`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-194">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="99b3b-195">Теперь при нажатии кнопки вызывается метод C# `AddTodo`:</span><span class="sxs-lookup"><span data-stu-id="99b3b-195">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="99b3b-196">Чтобы получить заголовок нового элемента списка дел, добавьте строку поля `newTodo` в верхней части блока `@code` и привяжите ее к значению вводимого текста с помощью атрибута `bind` в элементе `<input>`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-196">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="99b3b-197">Обновите метод `AddTodo`, чтобы добавить `TodoItem` с указываемым названием в список.</span><span class="sxs-lookup"><span data-stu-id="99b3b-197">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="99b3b-198">Очистите значение текстового поля, задав пустую строку в качестве значения для `newTodo`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-198">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="99b3b-199">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="99b3b-199">Rebuild and run the app.</span></span> <span data-ttu-id="99b3b-200">Добавьте несколько задач в список дел, чтобы проверить работу нового кода.</span><span class="sxs-lookup"><span data-stu-id="99b3b-200">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="99b3b-201">Вы можете сделать текст заголовка для каждого элемента списка дел редактируемым, а по дополнительному флажку пользователь сможет отслеживать завершение задач.</span><span class="sxs-lookup"><span data-stu-id="99b3b-201">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="99b3b-202">Добавьте флажок для каждого элемента списка дел и привяжите его значение к свойству `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-202">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="99b3b-203">Замените `@todo.Title` элементом `<input>`, который привязан к `@todo.Title`.</span><span class="sxs-lookup"><span data-stu-id="99b3b-203">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="99b3b-204">Чтобы проверить привязку значений, обновите заголовок `<h1>`, чтобы в нем отображалось количество незавершенных дел в списке (`IsDone` имеет значение `false`).</span><span class="sxs-lookup"><span data-stu-id="99b3b-204">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```razor
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="99b3b-205">Готовый компонент `Todo` (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="99b3b-205">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="99b3b-206">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="99b3b-206">Rebuild and run the app.</span></span> <span data-ttu-id="99b3b-207">Добавьте элементы в список дел, чтобы протестировать новый код.</span><span class="sxs-lookup"><span data-stu-id="99b3b-207">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
