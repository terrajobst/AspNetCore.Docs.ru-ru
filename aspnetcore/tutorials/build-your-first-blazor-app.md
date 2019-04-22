---
title: Создание приложения Blazor
author: guardrex
description: Пошаговое создание приложения Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 310eb211f68076d6f52d6427940e07736d833e5b
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614753"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="0413e-103">Создание приложения Blazor</span><span class="sxs-lookup"><span data-stu-id="0413e-103">Build your first Blazor app</span></span>

<span data-ttu-id="0413e-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0413e-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="0413e-105">В этом руководстве показано, как создать приложение с помощью серверных компонентов Razor или Blazor на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="0413e-105">This tutorial shows you how to build an app with server-side Razor Components or client-side Blazor.</span></span>

<span data-ttu-id="0413e-106">Сведения о работе с серверными компонентами Razor для ASP.NET Core (*рекомендуется*):</span><span class="sxs-lookup"><span data-stu-id="0413e-106">For an experience using server-side ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="0413e-107">Выполните инструкции из руководства по *работе с компонентами Razor* в статье <xref:blazor/get-started#server-side-razor-components-experience>, чтобы создать проект на основе компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="0413e-107">Follow the *Razor Components experience* guidance in <xref:blazor/get-started#server-side-razor-components-experience> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="0413e-108">Задайте для проекта имя `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="0413e-108">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="0413e-109">Сведения для работы с Blazor:</span><span class="sxs-lookup"><span data-stu-id="0413e-109">For an experience using Blazor:</span></span>

* <span data-ttu-id="0413e-110">Выполните инструкции из руководства по *работе с компонентами BLazor* в статье <xref:blazor/get-started#client-side-blazor-experience>, чтобы создать проект на основе Blazor.</span><span class="sxs-lookup"><span data-stu-id="0413e-110">Follow the *Blazor experience* guidance in <xref:blazor/get-started#client-side-blazor-experience> to create a Blazor-based project.</span></span>
* <span data-ttu-id="0413e-111">Задайте для проекта имя `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="0413e-111">Name the project `Blazor`.</span></span>

<span data-ttu-id="0413e-112">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0413e-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="0413e-113">Предварительные условия описаны в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="0413e-113">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="0413e-114">Сборка компонентов</span><span class="sxs-lookup"><span data-stu-id="0413e-114">Build components</span></span>

1. <span data-ttu-id="0413e-115">Перейдите к каждой из трех страниц приложения в папке *Components/Pages* (в Blazor это *Pages*): домашнюю, счетчика и получения данных.</span><span class="sxs-lookup"><span data-stu-id="0413e-115">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="0413e-116">Эти страницы реализуются такими файлами компонентов Razor, как *Index.razor*, *Counter.razor* и *FetchData.razor*</span><span class="sxs-lookup"><span data-stu-id="0413e-116">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="0413e-117">(в Blazor по-прежнему используется расширение файла *.cshtml* — *Index.cshtml*, *Counter.cshtml* и *FetchData.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="0413e-117">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="0413e-118">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="0413e-118">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="0413e-119">Обычно для увеличения значений счетчика на веб-странице требуется код JavaScript, но Blazor реализует более правильный подход с использованием C#.</span><span class="sxs-lookup"><span data-stu-id="0413e-119">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="0413e-120">Изучите реализацию компонента Counter в файле *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="0413e-120">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="0413e-121">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* в Blazor):</span><span class="sxs-lookup"><span data-stu-id="0413e-121">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="0413e-122">Пользовательский интерфейс компонента Counter определяется с помощью HTML.</span><span class="sxs-lookup"><span data-stu-id="0413e-122">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="0413e-123">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="0413e-123">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="0413e-124">HTML-разметка и логика отображения C# преобразуются в класс компонента во время сборки.</span><span class="sxs-lookup"><span data-stu-id="0413e-124">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="0413e-125">Имя создаваемого класса .NET совпадает с именем файла.</span><span class="sxs-lookup"><span data-stu-id="0413e-125">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="0413e-126">Элементы класса компонента определяются в блоке `@functions`.</span><span class="sxs-lookup"><span data-stu-id="0413e-126">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="0413e-127">В блоке `@functions` указываются состояние (свойства, поля) и методы компонента для обработки событий или определения другой логики компонента.</span><span class="sxs-lookup"><span data-stu-id="0413e-127">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="0413e-128">Эти элементы можно позднее включить в логику отображения компонента и обработки событий.</span><span class="sxs-lookup"><span data-stu-id="0413e-128">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="0413e-129">При нажатии кнопки **Click me** (Щелкните здесь) выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="0413e-129">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="0413e-130">Вызывается обработчик `onclick`, зарегистрированный для компонента Counter (метод `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="0413e-130">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="0413e-131">Компонент Counter повторно создает свое дерево отображения.</span><span class="sxs-lookup"><span data-stu-id="0413e-131">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="0413e-132">Выполняется сравнение нового и старого деревьев отображения.</span><span class="sxs-lookup"><span data-stu-id="0413e-132">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="0413e-133">Применяются только изменения модели DOM (Document Object Model — объектная модель документа).</span><span class="sxs-lookup"><span data-stu-id="0413e-133">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="0413e-134">Отображаемое значение счетчика обновляется.</span><span class="sxs-lookup"><span data-stu-id="0413e-134">The displayed count is updated.</span></span>

1. <span data-ttu-id="0413e-135">Измените логику C# для компонента Counter, чтобы при нажатии кнопки значение счетчика увеличивалось на две единицы вместо одной.</span><span class="sxs-lookup"><span data-stu-id="0413e-135">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="0413e-136">Скомпилируйте и запустите приложение, чтобы увидеть изменения.</span><span class="sxs-lookup"><span data-stu-id="0413e-136">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="0413e-137">Нажмите кнопку **Click me** (Щелкните здесь), и значение счетчика увеличится на два.</span><span class="sxs-lookup"><span data-stu-id="0413e-137">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="0413e-138">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="0413e-138">Use components</span></span>

<span data-ttu-id="0413e-139">Включите компонент в другой компонент, используя синтаксис в стиле HTML.</span><span class="sxs-lookup"><span data-stu-id="0413e-139">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="0413e-140">Добавьте компонент Counter в компонент Index (домашняя страница), разместив элемент `<Counter />` внутри компонента Index.</span><span class="sxs-lookup"><span data-stu-id="0413e-140">Add the Counter component to the app's Index (Home) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="0413e-141">Если вы используете Blazor для этой задачи, в компонент индекса добавляется компонент опроса Prompt (элемент `<SurveyPrompt>`).</span><span class="sxs-lookup"><span data-stu-id="0413e-141">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="0413e-142">Замените элемент `<SurveyPrompt>` элементом `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="0413e-142">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="0413e-143">*Components/Pages/Index.razor* (*Pages/Index.cshtml* в Blazor):</span><span class="sxs-lookup"><span data-stu-id="0413e-143">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="0413e-144">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0413e-144">Rebuild and run the app.</span></span> <span data-ttu-id="0413e-145">На домашней странице есть свой счетчик.</span><span class="sxs-lookup"><span data-stu-id="0413e-145">The Home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="0413e-146">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="0413e-146">Component parameters</span></span>

<span data-ttu-id="0413e-147">Компоненты также могут принимать параметры.</span><span class="sxs-lookup"><span data-stu-id="0413e-147">Components can also have parameters.</span></span> <span data-ttu-id="0413e-148">Параметры для компонентов определяются с помощью частных свойств в классе компонента с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="0413e-148">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="0413e-149">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="0413e-149">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="0413e-150">Обновите код C# для `@functions` в компоненте:</span><span class="sxs-lookup"><span data-stu-id="0413e-150">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="0413e-151">Добавьте свойство `IncrementAmount` с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="0413e-151">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="0413e-152">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="0413e-152">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="0413e-153">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* в Blazor):</span><span class="sxs-lookup"><span data-stu-id="0413e-153">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="0413e-154">Укажите параметр `IncrementAmount` в элементе `<Counter>` для компонента домашней страницы, используя атрибут.</span><span class="sxs-lookup"><span data-stu-id="0413e-154">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="0413e-155">Установите приращение счетчика на десять.</span><span class="sxs-lookup"><span data-stu-id="0413e-155">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="0413e-156">*Components/Pages/Index.razor* (*Pages/Index.cshtml* в Blazor):</span><span class="sxs-lookup"><span data-stu-id="0413e-156">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="0413e-157">Перезагрузите домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="0413e-157">Reload the Home page.</span></span> <span data-ttu-id="0413e-158">Теперь значение счетчика на домашней странице будет увеличиваться на десять при каждом нажатии кнопки **Click me** (Щелкните здесь).</span><span class="sxs-lookup"><span data-stu-id="0413e-158">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="0413e-159">А значение счетчика на странице Counter увеличивается на единицу.</span><span class="sxs-lookup"><span data-stu-id="0413e-159">The counter on the Counter page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="0413e-160">Маршрутизация к компонентам</span><span class="sxs-lookup"><span data-stu-id="0413e-160">Route to components</span></span>

<span data-ttu-id="0413e-161">Директива `@page` в верхней части файла *Counter.razor* указывает, что этот компонент является конечной точкой маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="0413e-161">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="0413e-162">Компонент Counter обрабатывает запросы, направляемые к `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="0413e-162">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="0413e-163">Без директивы `@page` этот компонент не будет обрабатывать перенаправленные запросы, но остается доступным для использования другими компонентами.</span><span class="sxs-lookup"><span data-stu-id="0413e-163">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="0413e-164">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="0413e-164">Dependency injection</span></span>

<span data-ttu-id="0413e-165">Службы, зарегистрированные в контейнере служб приложения, доступны компонентам через [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0413e-165">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="0413e-166">Внедрите службы в компонент с помощью директивы `@inject`.</span><span class="sxs-lookup"><span data-stu-id="0413e-166">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="0413e-167">Изучите директивы компонента FetchData в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="0413e-167">Examine the directives of the FetchData component in the sample app.</span></span>

<span data-ttu-id="0413e-168">В примере приложения Razor Components служба `WeatherForecastService` зарегистрирована как [отдельная](xref:fundamentals/dependency-injection#service-lifetimes), то есть для всего приложения предоставляется один экземпляр этой службы.</span><span class="sxs-lookup"><span data-stu-id="0413e-168">In the Razor Components sample app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="0413e-169">Директива `@inject` используется для внедрения экземпляра службы `WeatherForecastService` в компонент.</span><span class="sxs-lookup"><span data-stu-id="0413e-169">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="0413e-170">*Components/Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="0413e-170">*Components/Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="0413e-171">Компонент FetchData использует внедренную службу как `ForecastService`, чтобы получить массив объектов `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="0413e-171">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="0413e-172">В версии Blazor примера приложения `HttpClient` внедряется для получения данных прогноза погоды из файла *weather.json* в папке *wwwroot/sample-data*.</span><span class="sxs-lookup"><span data-stu-id="0413e-172">In the Blazor version of the sample app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="0413e-173">*Pages/FetchData.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0413e-173">*Pages/FetchData.cshtml*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

<span data-ttu-id="0413e-174">В обеих версиях примера приложения цикл [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) используется для отображения каждого экземпляра прогноза в отдельной строке в таблице погоды.</span><span class="sxs-lookup"><span data-stu-id="0413e-174">In both sample apps, a [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="0413e-175">Создание списка дел</span><span class="sxs-lookup"><span data-stu-id="0413e-175">Build a todo list</span></span>

<span data-ttu-id="0413e-176">Добавьте в приложение новый компонент, представляющий собой простой список дел.</span><span class="sxs-lookup"><span data-stu-id="0413e-176">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="0413e-177">Добавьте в пример приложения пустой файл:</span><span class="sxs-lookup"><span data-stu-id="0413e-177">Add an empty file to the sample app:</span></span>

   * <span data-ttu-id="0413e-178">если вы используете Razor Components, добавьте файл *Todo.razor* в папку *Components/Pages*;</span><span class="sxs-lookup"><span data-stu-id="0413e-178">For the Razor Components experience, add a *Todo.razor* file to the *Components/Pages* folder.</span></span>
   * <span data-ttu-id="0413e-179">если вы используете Blazor, добавьте файл *Todo.cshtml* в папку *Pages*.</span><span class="sxs-lookup"><span data-stu-id="0413e-179">For the Blazor experience, add a *Todo.cshtml* file to the *Pages* folder.</span></span>

1. <span data-ttu-id="0413e-180">Задайте начальную разметку компонента.</span><span class="sxs-lookup"><span data-stu-id="0413e-180">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="0413e-181">Добавьте компонент Todo на панель навигации.</span><span class="sxs-lookup"><span data-stu-id="0413e-181">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="0413e-182">Компонент NavMenu (*Components/Shared/NavMenu.razor* или *Shared/NavMenu.cshtml* в Blazor) используется в макете приложения.</span><span class="sxs-lookup"><span data-stu-id="0413e-182">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="0413e-183">Макетами называются компоненты, которые избавляют от дублирования содержимого в приложении.</span><span class="sxs-lookup"><span data-stu-id="0413e-183">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="0413e-184">Для получения дополнительной информации см. <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="0413e-184">For more information, see <xref:blazor/layouts>.</span></span>

   <span data-ttu-id="0413e-185">Добавьте `<NavLink>` в компонент Todo. Для этого добавьте представленную ниже разметку с элементом списка под существующие элементы списка в файле *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* в Blazor).</span><span class="sxs-lookup"><span data-stu-id="0413e-185">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="0413e-186">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0413e-186">Rebuild and run the app.</span></span> <span data-ttu-id="0413e-187">Посетите новую страницу Todo, чтобы убедиться, что ссылка на компонент Todo работает правильно.</span><span class="sxs-lookup"><span data-stu-id="0413e-187">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="0413e-188">Добавьте в корень проекта файл *TodoItem.cs*, который будет размещать класс для элемента списка дел.</span><span class="sxs-lookup"><span data-stu-id="0413e-188">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="0413e-189">Используйте следующий код C# для класса `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="0413e-189">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="0413e-190">Вернитесь к компоненту Todo (*Components/Pages/Todo.razor* или *Pages/Todo.cshtml* в Blazor).</span><span class="sxs-lookup"><span data-stu-id="0413e-190">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="0413e-191">Добавьте поле в список дел в блоке `@functions`.</span><span class="sxs-lookup"><span data-stu-id="0413e-191">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="0413e-192">Компонент Todo использует это поле для хранения состояния списка дел.</span><span class="sxs-lookup"><span data-stu-id="0413e-192">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="0413e-193">Добавьте разметку неупорядоченного списка и цикл `foreach` для отображения каждого элемента списка дела в виде элемента списка.</span><span class="sxs-lookup"><span data-stu-id="0413e-193">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="0413e-194">Приложению необходимы элементы пользовательского интерфейса для добавления дел в список.</span><span class="sxs-lookup"><span data-stu-id="0413e-194">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="0413e-195">Добавьте текстовое поле и кнопку под списком.</span><span class="sxs-lookup"><span data-stu-id="0413e-195">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="0413e-196">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0413e-196">Rebuild and run the app.</span></span> <span data-ttu-id="0413e-197">При нажатии кнопки **Add todo** (Добавить задачу) ничего не происходит, так как к кнопке не привязан обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="0413e-197">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="0413e-198">Добавьте метод `AddTodo` в компонент Todo и с помощью атрибута `onclick` зарегистрируйте его на нажатие кнопки.</span><span class="sxs-lookup"><span data-stu-id="0413e-198">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="0413e-199">Теперь при нажатии кнопки вызывается метод C# `AddTodo`.</span><span class="sxs-lookup"><span data-stu-id="0413e-199">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="0413e-200">Чтобы получить заголовок нового элемента списка дел, добавьте строку поля `newTodo` и привяжите ее к значению вводимого текста с помощью атрибута `bind`.</span><span class="sxs-lookup"><span data-stu-id="0413e-200">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. <span data-ttu-id="0413e-201">Обновите метод `AddTodo`, чтобы добавить `TodoItem` с указываемым названием в список.</span><span class="sxs-lookup"><span data-stu-id="0413e-201">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="0413e-202">Очистите значение текстового поля, задав пустую строку в качестве значения для `newTodo`.</span><span class="sxs-lookup"><span data-stu-id="0413e-202">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="0413e-203">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0413e-203">Rebuild and run the app.</span></span> <span data-ttu-id="0413e-204">Добавьте несколько элементов в список Todo, чтобы протестировать работу нового кода.</span><span class="sxs-lookup"><span data-stu-id="0413e-204">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="0413e-205">Вы можете сделать текст заголовка для каждого элемента списка дел редактируемым, а по дополнительному флажку пользователь сможет отслеживать завершение задач.</span><span class="sxs-lookup"><span data-stu-id="0413e-205">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="0413e-206">Добавьте флажок для каждого элемента списка дел и привяжите его значение к свойству `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="0413e-206">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="0413e-207">Замените `@todo.Title` элементом `<input>`, который привязан к `@todo.Title`.</span><span class="sxs-lookup"><span data-stu-id="0413e-207">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="0413e-208">Чтобы проверить привязку значений, обновите заголовок `<h1>`, чтобы в нем отображалось количество незавершенных дел в списке (`IsDone` имеет значение `false`).</span><span class="sxs-lookup"><span data-stu-id="0413e-208">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="0413e-209">Завершенный компонент Todo (*Components/Pages/Todo.razor* или *Pages/Todo.cshtml* в Blazor):</span><span class="sxs-lookup"><span data-stu-id="0413e-209">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="0413e-210">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0413e-210">Rebuild and run the app.</span></span> <span data-ttu-id="0413e-211">Добавьте элементы в список дел, чтобы протестировать новый код.</span><span class="sxs-lookup"><span data-stu-id="0413e-211">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="0413e-212">Публикация и развертывание приложения</span><span class="sxs-lookup"><span data-stu-id="0413e-212">Publish and deploy the app</span></span>

<span data-ttu-id="0413e-213">Чтобы опубликовать приложение, воспользуйтесь инструкцией из статьи <xref:host-and-deploy/blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="0413e-213">To publish the app, see <xref:host-and-deploy/blazor/index>.</span></span>
