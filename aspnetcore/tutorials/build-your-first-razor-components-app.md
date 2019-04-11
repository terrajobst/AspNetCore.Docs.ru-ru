---
title: Создайте свое первое приложение на основе Razor Components
author: guardrex
description: Пошаговое создание приложения на основе Razor Components и базовые понятия, относящиеся к компонентам Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 697c4659bcc9952ffe9868fe9b3c0d28019bc369
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468780"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="c0ead-103">Создайте свое первое приложение на основе Razor Components</span><span class="sxs-lookup"><span data-stu-id="c0ead-103">Build your first Razor Components app</span></span>

<span data-ttu-id="c0ead-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c0ead-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="c0ead-105">В этом руководстве показано, как создать приложение с помощью Razor Components, и описаны основные понятия Razor Components.</span><span class="sxs-lookup"><span data-stu-id="c0ead-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="c0ead-106">При работе с этим руководством вы можете использовать проект на основе Razor Components (поддерживается в .NET Core 3.0 и более поздних версиях) или проект на основе Blazor (будет поддерживаться в будущих выпусках .NET Core).</span><span class="sxs-lookup"><span data-stu-id="c0ead-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="c0ead-107">Сведения о работе с Razor Components для ASP.NET Core (*рекомендуется*):</span><span class="sxs-lookup"><span data-stu-id="c0ead-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="c0ead-108">Выполните инструкции из статьи <xref:razor-components/get-started>, чтобы создать проект на основе Razor Components.</span><span class="sxs-lookup"><span data-stu-id="c0ead-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="c0ead-109">Задайте для проекта имя `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-109">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="c0ead-110">Сведения для работы с Blazor:</span><span class="sxs-lookup"><span data-stu-id="c0ead-110">For an experience using Blazor:</span></span>

* <span data-ttu-id="c0ead-111">Выполните инструкции из статьи <xref:spa/blazor/get-started>, чтобы создать проект на основе Blazor.</span><span class="sxs-lookup"><span data-stu-id="c0ead-111">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="c0ead-112">Задайте для проекта имя `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-112">Name the project `Blazor`.</span></span>

<span data-ttu-id="c0ead-113">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c0ead-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="c0ead-114">Предварительные условия описаны в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="c0ead-114">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="c0ead-115">Сборка компонентов</span><span class="sxs-lookup"><span data-stu-id="c0ead-115">Build components</span></span>

1. <span data-ttu-id="c0ead-116">Перейдите к каждой из трех страниц приложения в папке *Components/Pages* (в Blazor это *Pages*): домашнюю, счетчика и получения данных.</span><span class="sxs-lookup"><span data-stu-id="c0ead-116">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="c0ead-117">Эти страницы реализуются такими файлами компонентов Razor, как *Index.razor*, *Counter.razor* и *FetchData.razor*</span><span class="sxs-lookup"><span data-stu-id="c0ead-117">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="c0ead-118">(в Blazor по-прежнему используется расширение файла *.cshtml* — *Index.cshtml*, *Counter.cshtml* и *FetchData.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="c0ead-118">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="c0ead-119">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="c0ead-119">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="c0ead-120">Обычно для увеличения значений счетчика на веб-странице требуется код JavaScript, но Razor Components реализует более правильный подход с использованием C#.</span><span class="sxs-lookup"><span data-stu-id="c0ead-120">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="c0ead-121">Изучите реализацию компонента Counter в файле *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="c0ead-121">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="c0ead-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* в Blazor):</span><span class="sxs-lookup"><span data-stu-id="c0ead-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="c0ead-123">Пользовательский интерфейс компонента Counter определяется с помощью HTML.</span><span class="sxs-lookup"><span data-stu-id="c0ead-123">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="c0ead-124">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="c0ead-124">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="c0ead-125">HTML-разметка и логика отображения C# преобразуются в класс компонента во время сборки.</span><span class="sxs-lookup"><span data-stu-id="c0ead-125">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="c0ead-126">Имя создаваемого класса .NET совпадает с именем файла.</span><span class="sxs-lookup"><span data-stu-id="c0ead-126">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="c0ead-127">Элементы класса компонента определяются в блоке `@functions`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-127">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="c0ead-128">В блоке `@functions` указываются состояние (свойства, поля) и методы компонента для обработки событий или определения другой логики компонента.</span><span class="sxs-lookup"><span data-stu-id="c0ead-128">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="c0ead-129">Эти элементы можно позднее включить в логику отображения компонента и обработки событий.</span><span class="sxs-lookup"><span data-stu-id="c0ead-129">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="c0ead-130">При нажатии кнопки **Click me** (Щелкните здесь) выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="c0ead-130">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="c0ead-131">Вызывается обработчик `onclick`, зарегистрированный для компонента Counter (метод `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="c0ead-131">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="c0ead-132">Компонент Counter повторно создает свое дерево отображения.</span><span class="sxs-lookup"><span data-stu-id="c0ead-132">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="c0ead-133">Выполняется сравнение нового и старого деревьев отображения.</span><span class="sxs-lookup"><span data-stu-id="c0ead-133">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="c0ead-134">Применяются только изменения модели DOM (Document Object Model — объектная модель документа).</span><span class="sxs-lookup"><span data-stu-id="c0ead-134">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="c0ead-135">Отображаемое значение счетчика обновляется.</span><span class="sxs-lookup"><span data-stu-id="c0ead-135">The displayed count is updated.</span></span>

1. <span data-ttu-id="c0ead-136">Измените логику C# для компонента Counter, чтобы при нажатии кнопки значение счетчика увеличивалось на две единицы вместо одной.</span><span class="sxs-lookup"><span data-stu-id="c0ead-136">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="c0ead-137">Скомпилируйте и запустите приложение, чтобы увидеть изменения.</span><span class="sxs-lookup"><span data-stu-id="c0ead-137">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="c0ead-138">Нажмите кнопку **Click me** (Щелкните здесь), и значение счетчика увеличится на два.</span><span class="sxs-lookup"><span data-stu-id="c0ead-138">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="c0ead-139">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="c0ead-139">Use components</span></span>

<span data-ttu-id="c0ead-140">Включите компонент в другой компонент, используя синтаксис в стиле HTML.</span><span class="sxs-lookup"><span data-stu-id="c0ead-140">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="c0ead-141">Добавьте компонент Counter в компонент Index (домашняя страница), разместив элемент `<Counter />` внутри компонента Index.</span><span class="sxs-lookup"><span data-stu-id="c0ead-141">Add the Counter component to the app's Index (Home) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="c0ead-142">Если вы используете Blazor для этой задачи, в компонент индекса добавляется компонент опроса Prompt (элемент `<SurveyPrompt>`).</span><span class="sxs-lookup"><span data-stu-id="c0ead-142">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="c0ead-143">Замените элемент `<SurveyPrompt>` элементом `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-143">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="c0ead-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* в Blazor):</span><span class="sxs-lookup"><span data-stu-id="c0ead-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="c0ead-145">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c0ead-145">Rebuild and run the app.</span></span> <span data-ttu-id="c0ead-146">На домашней странице есть свой счетчик.</span><span class="sxs-lookup"><span data-stu-id="c0ead-146">The Home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="c0ead-147">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="c0ead-147">Component parameters</span></span>

<span data-ttu-id="c0ead-148">Компоненты также могут принимать параметры.</span><span class="sxs-lookup"><span data-stu-id="c0ead-148">Components can also have parameters.</span></span> <span data-ttu-id="c0ead-149">Параметры для компонентов определяются с помощью частных свойств в классе компонента с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-149">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="c0ead-150">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="c0ead-150">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="c0ead-151">Обновите код C# для `@functions` в компоненте:</span><span class="sxs-lookup"><span data-stu-id="c0ead-151">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="c0ead-152">Добавьте свойство `IncrementAmount` с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-152">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="c0ead-153">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-153">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="c0ead-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* в Blazor):</span><span class="sxs-lookup"><span data-stu-id="c0ead-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="c0ead-155">Укажите параметр `IncrementAmount` в элементе `<Counter>` для компонента домашней страницы, используя атрибут.</span><span class="sxs-lookup"><span data-stu-id="c0ead-155">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="c0ead-156">Установите приращение счетчика на десять.</span><span class="sxs-lookup"><span data-stu-id="c0ead-156">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="c0ead-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* в Blazor):</span><span class="sxs-lookup"><span data-stu-id="c0ead-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="c0ead-158">Перезагрузите домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="c0ead-158">Reload the Home page.</span></span> <span data-ttu-id="c0ead-159">Теперь значение счетчика на домашней странице будет увеличиваться на десять при каждом нажатии кнопки **Click me** (Щелкните здесь).</span><span class="sxs-lookup"><span data-stu-id="c0ead-159">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="c0ead-160">А значение счетчика на странице Counter увеличивается на единицу.</span><span class="sxs-lookup"><span data-stu-id="c0ead-160">The counter on the Counter page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="c0ead-161">Маршрутизация к компонентам</span><span class="sxs-lookup"><span data-stu-id="c0ead-161">Route to components</span></span>

<span data-ttu-id="c0ead-162">Директива `@page` в верхней части файла *Counter.razor* указывает, что этот компонент является конечной точкой маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="c0ead-162">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="c0ead-163">Компонент Counter обрабатывает запросы, направляемые к `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-163">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="c0ead-164">Без директивы `@page` этот компонент не будет обрабатывать перенаправленные запросы, но остается доступным для использования другими компонентами.</span><span class="sxs-lookup"><span data-stu-id="c0ead-164">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="c0ead-165">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="c0ead-165">Dependency injection</span></span>

<span data-ttu-id="c0ead-166">Службы, зарегистрированные в контейнере служб приложения, доступны компонентам через [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c0ead-166">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c0ead-167">Внедрите службы в компонент с помощью директивы `@inject`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-167">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="c0ead-168">Изучите директивы компонента FetchData в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c0ead-168">Examine the directives of the FetchData component in the sample app.</span></span>

<span data-ttu-id="c0ead-169">В примере приложения Razor Components служба `WeatherForecastService` зарегистрирована как [отдельная](xref:fundamentals/dependency-injection#service-lifetimes), то есть для всего приложения предоставляется один экземпляр этой службы.</span><span class="sxs-lookup"><span data-stu-id="c0ead-169">In the Razor Components sample app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="c0ead-170">Директива `@inject` используется для внедрения экземпляра службы `WeatherForecastService` в компонент.</span><span class="sxs-lookup"><span data-stu-id="c0ead-170">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="c0ead-171">*Components/Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="c0ead-171">*Components/Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="c0ead-172">Компонент FetchData использует внедренную службу как `ForecastService`, чтобы получить массив объектов `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-172">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="c0ead-173">В версии Blazor примера приложения `HttpClient` внедряется для получения данных прогноза погоды из файла *weather.json* в папке *wwwroot/sample-data*.</span><span class="sxs-lookup"><span data-stu-id="c0ead-173">In the Blazor version of the sample app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="c0ead-174">*Pages/FetchData.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c0ead-174">*Pages/FetchData.cshtml*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

<span data-ttu-id="c0ead-175">В обеих версиях примера приложения цикл [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) используется для отображения каждого экземпляра прогноза в отдельной строке в таблице погоды.</span><span class="sxs-lookup"><span data-stu-id="c0ead-175">In both sample apps, a [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="c0ead-176">Создание списка дел</span><span class="sxs-lookup"><span data-stu-id="c0ead-176">Build a todo list</span></span>

<span data-ttu-id="c0ead-177">Добавьте в приложение новый компонент, представляющий собой простой список дел.</span><span class="sxs-lookup"><span data-stu-id="c0ead-177">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="c0ead-178">Добавьте в пример приложения пустой файл:</span><span class="sxs-lookup"><span data-stu-id="c0ead-178">Add an empty file to the sample app:</span></span>

   * <span data-ttu-id="c0ead-179">если вы используете Razor Components, добавьте файл *Todo.razor* в папку *Components/Pages*;</span><span class="sxs-lookup"><span data-stu-id="c0ead-179">For the Razor Components experience, add a *Todo.razor* file to the *Components/Pages* folder.</span></span>
   * <span data-ttu-id="c0ead-180">если вы используете Blazor, добавьте файл *Todo.cshtml* в папку *Pages*.</span><span class="sxs-lookup"><span data-stu-id="c0ead-180">For the Blazor experience, add a *Todo.cshtml* file to the *Pages* folder.</span></span>

1. <span data-ttu-id="c0ead-181">Задайте начальную разметку компонента.</span><span class="sxs-lookup"><span data-stu-id="c0ead-181">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="c0ead-182">Добавьте компонент Todo на панель навигации.</span><span class="sxs-lookup"><span data-stu-id="c0ead-182">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="c0ead-183">Компонент NavMenu (*Components/Shared/NavMenu.razor* или *Shared/NavMenu.cshtml* в Blazor) используется в макете приложения.</span><span class="sxs-lookup"><span data-stu-id="c0ead-183">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="c0ead-184">Макетами называются компоненты, которые избавляют от дублирования содержимого в приложении.</span><span class="sxs-lookup"><span data-stu-id="c0ead-184">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="c0ead-185">Для получения дополнительной информации см. <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="c0ead-185">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="c0ead-186">Добавьте `<NavLink>` в компонент Todo. Для этого добавьте представленную ниже разметку с элементом списка под существующие элементы списка в файле *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* в Blazor).</span><span class="sxs-lookup"><span data-stu-id="c0ead-186">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="c0ead-187">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c0ead-187">Rebuild and run the app.</span></span> <span data-ttu-id="c0ead-188">Посетите новую страницу Todo, чтобы убедиться, что ссылка на компонент Todo работает правильно.</span><span class="sxs-lookup"><span data-stu-id="c0ead-188">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="c0ead-189">Добавьте в корень проекта файл *TodoItem.cs*, который будет размещать класс для элемента списка дел.</span><span class="sxs-lookup"><span data-stu-id="c0ead-189">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="c0ead-190">Используйте следующий код C# для класса `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-190">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="c0ead-191">Вернитесь к компоненту Todo (*Components/Pages/Todo.razor* или *Pages/Todo.cshtml* в Blazor).</span><span class="sxs-lookup"><span data-stu-id="c0ead-191">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="c0ead-192">Добавьте поле в список дел в блоке `@functions`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-192">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="c0ead-193">Компонент Todo использует это поле для хранения состояния списка дел.</span><span class="sxs-lookup"><span data-stu-id="c0ead-193">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="c0ead-194">Добавьте разметку неупорядоченного списка и цикл `foreach` для отображения каждого элемента списка дела в виде элемента списка.</span><span class="sxs-lookup"><span data-stu-id="c0ead-194">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="c0ead-195">Приложению необходимы элементы пользовательского интерфейса для добавления дел в список.</span><span class="sxs-lookup"><span data-stu-id="c0ead-195">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="c0ead-196">Добавьте текстовое поле и кнопку под списком.</span><span class="sxs-lookup"><span data-stu-id="c0ead-196">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="c0ead-197">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c0ead-197">Rebuild and run the app.</span></span> <span data-ttu-id="c0ead-198">При нажатии кнопки **Add todo** (Добавить задачу) ничего не происходит, так как к кнопке не привязан обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="c0ead-198">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="c0ead-199">Добавьте метод `AddTodo` в компонент Todo и с помощью атрибута `onclick` зарегистрируйте его на нажатие кнопки.</span><span class="sxs-lookup"><span data-stu-id="c0ead-199">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="c0ead-200">Теперь при нажатии кнопки вызывается метод C# `AddTodo`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-200">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="c0ead-201">Чтобы получить заголовок нового элемента списка дел, добавьте строку поля `newTodo` и привяжите ее к значению вводимого текста с помощью атрибута `bind`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-201">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. <span data-ttu-id="c0ead-202">Обновите метод `AddTodo`, чтобы добавить `TodoItem` с указываемым названием в список.</span><span class="sxs-lookup"><span data-stu-id="c0ead-202">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="c0ead-203">Очистите значение текстового поля, задав пустую строку в качестве значения для `newTodo`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-203">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="c0ead-204">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c0ead-204">Rebuild and run the app.</span></span> <span data-ttu-id="c0ead-205">Добавьте несколько элементов в список Todo, чтобы протестировать работу нового кода.</span><span class="sxs-lookup"><span data-stu-id="c0ead-205">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="c0ead-206">Вы можете сделать текст заголовка для каждого элемента списка дел редактируемым, а по дополнительному флажку пользователь сможет отслеживать завершение задач.</span><span class="sxs-lookup"><span data-stu-id="c0ead-206">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="c0ead-207">Добавьте флажок для каждого элемента списка дел и привяжите его значение к свойству `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-207">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="c0ead-208">Замените `@todo.Title` элементом `<input>`, который привязан к `@todo.Title`.</span><span class="sxs-lookup"><span data-stu-id="c0ead-208">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="c0ead-209">Чтобы проверить привязку значений, обновите заголовок `<h1>`, чтобы в нем отображалось количество незавершенных дел в списке (`IsDone` имеет значение `false`).</span><span class="sxs-lookup"><span data-stu-id="c0ead-209">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="c0ead-210">Завершенный компонент Todo (*Components/Pages/Todo.razor* или *Pages/Todo.cshtml* в Blazor):</span><span class="sxs-lookup"><span data-stu-id="c0ead-210">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="c0ead-211">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c0ead-211">Rebuild and run the app.</span></span> <span data-ttu-id="c0ead-212">Добавьте элементы в список дел, чтобы протестировать новый код.</span><span class="sxs-lookup"><span data-stu-id="c0ead-212">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="c0ead-213">Публикация и развертывание приложения</span><span class="sxs-lookup"><span data-stu-id="c0ead-213">Publish and deploy the app</span></span>

<span data-ttu-id="c0ead-214">Чтобы опубликовать приложение, воспользуйтесь инструкцией из статьи <xref:host-and-deploy/razor-components-blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="c0ead-214">To publish the app, see <xref:host-and-deploy/razor-components-blazor/index>.</span></span>
