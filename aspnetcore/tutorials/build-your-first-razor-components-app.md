---
title: Создайте свое первое приложение на основе Razor Components
author: guardrex
description: Пошаговое создание приложения на основе Razor Components и базовые понятия, относящиеся к компонентам Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 0c3dd2366581d73bad44e2911602e13c6c0daf9a
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159347"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="51693-103">Создайте свое первое приложение на основе Razor Components</span><span class="sxs-lookup"><span data-stu-id="51693-103">Build your first Razor Components app</span></span>

<span data-ttu-id="51693-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="51693-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="51693-105">В этом руководстве показано, как создать приложение с помощью Razor Components, и описаны основные понятия Razor Components.</span><span class="sxs-lookup"><span data-stu-id="51693-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="51693-106">При работе с этим руководством вы можете использовать проект на основе Razor Components (поддерживается в .NET Core 3.0 и более поздних версиях) или проект на основе Blazor (будет поддерживаться в будущих выпусках .NET Core).</span><span class="sxs-lookup"><span data-stu-id="51693-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="51693-107">Сведения о работе с Razor Components для ASP.NET Core (*рекомендуется*):</span><span class="sxs-lookup"><span data-stu-id="51693-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="51693-108">Выполните инструкции из статьи <xref:razor-components/get-started>, чтобы создать проект на основе Razor Components.</span><span class="sxs-lookup"><span data-stu-id="51693-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="51693-109">Задайте для проекта имя `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="51693-109">Name the project `RazorComponents`.</span></span>
* <span data-ttu-id="51693-110">На основе шаблона Razor Components создает решение с несколькими проектами.</span><span class="sxs-lookup"><span data-stu-id="51693-110">A multi-project solution is created from the Razor Components template.</span></span> <span data-ttu-id="51693-111">Проект на основе Razor Components создается с именем *RazorComponents.App*.</span><span class="sxs-lookup"><span data-stu-id="51693-111">The Razor Components project is generated as *RazorComponents.App*.</span></span>

<span data-ttu-id="51693-112">Сведения для работы с Blazor:</span><span class="sxs-lookup"><span data-stu-id="51693-112">For an experience using Blazor:</span></span>

* <span data-ttu-id="51693-113">Выполните инструкции из статьи <xref:spa/blazor/get-started>, чтобы создать проект на основе Blazor.</span><span class="sxs-lookup"><span data-stu-id="51693-113">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="51693-114">Задайте для проекта имя `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="51693-114">Name the project `Blazor`.</span></span>
* <span data-ttu-id="51693-115">На основе шаблона Blazor создается решение с одним проектом.</span><span class="sxs-lookup"><span data-stu-id="51693-115">A single-project solution is created from the Blazor template.</span></span>

<span data-ttu-id="51693-116">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="51693-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="51693-117">Предварительные условия описаны в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="51693-117">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="51693-118">Сборка компонентов</span><span class="sxs-lookup"><span data-stu-id="51693-118">Build components</span></span>

1. <span data-ttu-id="51693-119">Перейдите на каждую из трех страниц приложения: домашнюю, счетчика и получения данных.</span><span class="sxs-lookup"><span data-stu-id="51693-119">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="51693-120">Эти страницы реализуются файлами Razor, которые находятся в папке *Pages*: *Index.cshtml*, *Counter.cshtml* и *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="51693-120">These pages are implemented by Razor files in the *Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span>

1. <span data-ttu-id="51693-121">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="51693-121">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="51693-122">Обычно для увеличения значений счетчика на веб-странице требуется код JavaScript, но Razor Components реализует более правильный подход с использованием C#.</span><span class="sxs-lookup"><span data-stu-id="51693-122">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="51693-123">Изучите реализацию компонента Counter в файле *Counter.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="51693-123">Examine the implementation of the Counter component in the *Counter.cshtml* file.</span></span>

   <span data-ttu-id="51693-124">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="51693-124">*Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.cshtml)]

   <span data-ttu-id="51693-125">Пользовательский интерфейс компонента Counter определяется с помощью HTML.</span><span class="sxs-lookup"><span data-stu-id="51693-125">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="51693-126">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="51693-126">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="51693-127">HTML-разметка и логика отображения C# преобразуются в класс компонента во время сборки.</span><span class="sxs-lookup"><span data-stu-id="51693-127">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="51693-128">Имя создаваемого класса .NET совпадает с именем файла.</span><span class="sxs-lookup"><span data-stu-id="51693-128">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="51693-129">Элементы класса компонента определяются в блоке `@functions`.</span><span class="sxs-lookup"><span data-stu-id="51693-129">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="51693-130">В блоке `@functions` указываются состояние (свойства, поля) и методы компонента для обработки событий или определения другой логики компонента.</span><span class="sxs-lookup"><span data-stu-id="51693-130">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="51693-131">Эти элементы можно позднее включить в логику отображения компонента и обработки событий.</span><span class="sxs-lookup"><span data-stu-id="51693-131">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="51693-132">При нажатии кнопки **Click me** (Щелкните здесь) выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="51693-132">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="51693-133">Вызывается обработчик `onclick`, зарегистрированный для компонента Counter (метод `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="51693-133">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="51693-134">Компонент Counter повторно создает свое дерево отображения.</span><span class="sxs-lookup"><span data-stu-id="51693-134">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="51693-135">Выполняется сравнение нового и старого деревьев отображения.</span><span class="sxs-lookup"><span data-stu-id="51693-135">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="51693-136">Применяются только изменения модели DOM (Document Object Model — объектная модель документа).</span><span class="sxs-lookup"><span data-stu-id="51693-136">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="51693-137">Отображаемое значение счетчика обновляется.</span><span class="sxs-lookup"><span data-stu-id="51693-137">The displayed count is updated.</span></span>

1. <span data-ttu-id="51693-138">Измените логику C# для компонента Counter, чтобы при нажатии кнопки значение счетчика увеличивалось на две единицы вместо одной.</span><span class="sxs-lookup"><span data-stu-id="51693-138">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.cshtml?highlight=14)]

1. <span data-ttu-id="51693-139">Скомпилируйте и запустите приложение, чтобы увидеть изменения.</span><span class="sxs-lookup"><span data-stu-id="51693-139">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="51693-140">Нажмите кнопку **Click me** (Щелкните здесь), и значение счетчика увеличится на два.</span><span class="sxs-lookup"><span data-stu-id="51693-140">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="51693-141">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="51693-141">Use components</span></span>

<span data-ttu-id="51693-142">Включите компонент в другой компонент, используя синтаксис в стиле HTML.</span><span class="sxs-lookup"><span data-stu-id="51693-142">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="51693-143">Добавьте компонент Counter в компонент Index (домашняя страница), разместив `<Counter />` внутри компонента Index.</span><span class="sxs-lookup"><span data-stu-id="51693-143">Add the Counter component to the app's Index (home page) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="51693-144">Если вы используете Blazor для этой задачи, в компонент индекса добавляется компонент опроса Prompt (элемент `<SurveyPrompt>`).</span><span class="sxs-lookup"><span data-stu-id="51693-144">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="51693-145">Замените элемент `<SurveyPrompt>` элементом `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="51693-145">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="51693-146">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="51693-146">*Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.cshtml?highlight=7)]

1. <span data-ttu-id="51693-147">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="51693-147">Rebuild and run the app.</span></span> <span data-ttu-id="51693-148">На домашней странице есть свой счетчик.</span><span class="sxs-lookup"><span data-stu-id="51693-148">The home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="51693-149">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="51693-149">Component parameters</span></span>

<span data-ttu-id="51693-150">Компоненты также могут принимать параметры.</span><span class="sxs-lookup"><span data-stu-id="51693-150">Components can also have parameters.</span></span> <span data-ttu-id="51693-151">Параметры для компонентов определяются с помощью частных свойств в классе компонента с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="51693-151">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="51693-152">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="51693-152">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="51693-153">Обновите код C# для `@functions` в компоненте:</span><span class="sxs-lookup"><span data-stu-id="51693-153">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="51693-154">Добавьте свойство `IncrementAmount` с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="51693-154">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="51693-155">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="51693-155">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="51693-156">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="51693-156">*Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Counter.cshtml?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="51693-157">Укажите параметр `IncrementAmount` в элементе `<Counter>` для компонента домашней страницы, используя атрибут.</span><span class="sxs-lookup"><span data-stu-id="51693-157">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="51693-158">Установите приращение счетчика на десять.</span><span class="sxs-lookup"><span data-stu-id="51693-158">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="51693-159">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="51693-159">*Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Index.cshtml?highlight=7)]

1. <span data-ttu-id="51693-160">Перезагрузите страницу.</span><span class="sxs-lookup"><span data-stu-id="51693-160">Reload the page.</span></span> <span data-ttu-id="51693-161">Теперь счетчик на домашней странице будет увеличиваться на десять при каждом нажатии кнопки **Click me** (Щелкните здесь).</span><span class="sxs-lookup"><span data-stu-id="51693-161">The home page counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="51693-162">А счетчик на странице *Counter* увеличивается на единицу.</span><span class="sxs-lookup"><span data-stu-id="51693-162">The counter on the *Counter* page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="51693-163">Маршрутизация к компонентам</span><span class="sxs-lookup"><span data-stu-id="51693-163">Route to components</span></span>

<span data-ttu-id="51693-164">Директива `@page` в верхней части файла *Counter.cshtml* указывает, что этот компонент является конечной точкой маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="51693-164">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="51693-165">Компонент Counter обрабатывает запросы, направляемые к `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="51693-165">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="51693-166">Без директивы `@page` этот компонент не будет обрабатывать перенаправленные запросы, но остается доступным для использования другими компонентами.</span><span class="sxs-lookup"><span data-stu-id="51693-166">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="51693-167">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="51693-167">Dependency injection</span></span>

<span data-ttu-id="51693-168">Службы, зарегистрированные в контейнере служб приложения, доступны компонентам через [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="51693-168">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="51693-169">Внедрите службы в компонент с помощью директивы `@inject`.</span><span class="sxs-lookup"><span data-stu-id="51693-169">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="51693-170">Изучите директивы компонента FetchData (*Pages/FetchData.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="51693-170">Examine the directives of the FetchData component (*Pages/FetchData.cshtml*).</span></span> <span data-ttu-id="51693-171">Директива `@inject` используется для внедрения в компонент экземпляра службы `WeatherForecastService`.</span><span class="sxs-lookup"><span data-stu-id="51693-171">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=3)]

<span data-ttu-id="51693-172">Служба `WeatherForecastService` зарегистрирована как [отдельная](xref:fundamentals/dependency-injection#service-lifetimes), то есть для всего приложения предоставляется один экземпляр этой службы.</span><span class="sxs-lookup"><span data-stu-id="51693-172">The `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span>

<span data-ttu-id="51693-173">Компонент FetchData использует внедренную службу как `ForecastService`, чтобы получить массив объектов `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="51693-173">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.cshtml?highlight=6)]

<span data-ttu-id="51693-174">Цикл [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) используется для отображения каждого экземпляра прогноза в отдельной строке в таблице погоды.</span><span class="sxs-lookup"><span data-stu-id="51693-174">A [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.cshtml?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="51693-175">Создание списка дел</span><span class="sxs-lookup"><span data-stu-id="51693-175">Build a todo list</span></span>

<span data-ttu-id="51693-176">Добавьте в приложение новую страницу, представляющую собой простой список дел.</span><span class="sxs-lookup"><span data-stu-id="51693-176">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="51693-177">В папку *Pages* добавьте пустой файл с именем *Todo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="51693-177">Add an empty file to the *Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="51693-178">Задайте начальную разметку страницы.</span><span class="sxs-lookup"><span data-stu-id="51693-178">Provide the initial markup for the page:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="51693-179">Добавьте страницу списка дел на панель навигации.</span><span class="sxs-lookup"><span data-stu-id="51693-179">Add the Todo page to the navigation bar.</span></span>

   <span data-ttu-id="51693-180">Компонент NavMenu (*Shared/NavMenu.csthml*) используется в макете этого приложения.</span><span class="sxs-lookup"><span data-stu-id="51693-180">The NavMenu component (*Shared/NavMenu.csthml*) is used in the app's layout.</span></span> <span data-ttu-id="51693-181">Макетами называются компоненты, которые избавляют от дублирования содержимого в приложении.</span><span class="sxs-lookup"><span data-stu-id="51693-181">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="51693-182">Для получения дополнительной информации см. <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="51693-182">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="51693-183">Добавьте `<NavLink>` на страницу списка дел, добавив следующую разметку с элементом списка под существующими элементами списка в файл *Shared/NavMenu.csthml*.</span><span class="sxs-lookup"><span data-stu-id="51693-183">Add a `<NavLink>` for the Todo page by adding the following list item markup below the existing list items in the *Shared/NavMenu.csthml* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="51693-184">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="51693-184">Rebuild and run the app.</span></span> <span data-ttu-id="51693-185">Посетите новую страницу Todo, чтобы убедиться, что ссылка на страницу Todo работает правильно.</span><span class="sxs-lookup"><span data-stu-id="51693-185">Visit the new Todo page to confirm that the link to the Todo page works.</span></span>

1. <span data-ttu-id="51693-186">Добавьте в корень проекта файл *TodoItem.cs*, который будет размещать класс для элемента списка дел.</span><span class="sxs-lookup"><span data-stu-id="51693-186">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="51693-187">Используйте следующий код C# для класса `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="51693-187">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/TodoItem.cs)]

1. <span data-ttu-id="51693-188">Снова вернитесь к компоненту Todo (*Todo.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="51693-188">Return to the Todo component (*Todo.cshtml*):</span></span>

   * <span data-ttu-id="51693-189">Добавьте поле в список дел в блоке `@functions`.</span><span class="sxs-lookup"><span data-stu-id="51693-189">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="51693-190">Компонент Todo использует это поле для хранения состояния списка дел.</span><span class="sxs-lookup"><span data-stu-id="51693-190">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="51693-191">Добавьте разметку неупорядоченного списка и цикл `foreach` для отображения каждого элемента списка дела в виде элемента списка.</span><span class="sxs-lookup"><span data-stu-id="51693-191">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData4.cshtml?highlight=5-10,12-14)]

1. <span data-ttu-id="51693-192">Приложению необходимы элементы пользовательского интерфейса для добавления дел в список.</span><span class="sxs-lookup"><span data-stu-id="51693-192">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="51693-193">Добавьте текстовое поле и кнопку под списком.</span><span class="sxs-lookup"><span data-stu-id="51693-193">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData5.cshtml?highlight=12-13)]

1. <span data-ttu-id="51693-194">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="51693-194">Rebuild and run the app.</span></span> <span data-ttu-id="51693-195">При нажатии кнопки **Add todo** (Добавить задачу) ничего не происходит, так как к кнопке не привязан обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="51693-195">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="51693-196">Добавьте метод `AddTodo` в компонент Todo и с помощью атрибута `onclick` зарегистрируйте его на нажатие кнопки.</span><span class="sxs-lookup"><span data-stu-id="51693-196">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData6.cshtml?highlight=2,7-10)]

   <span data-ttu-id="51693-197">Теперь при нажатии кнопки вызывается метод C# `AddTodo`.</span><span class="sxs-lookup"><span data-stu-id="51693-197">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="51693-198">Чтобы получить заголовок нового элемента списка дел, добавьте строку поля `newTodo` и привяжите ее к значению вводимого текста с помощью атрибута `bind`.</span><span class="sxs-lookup"><span data-stu-id="51693-198">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData7.cshtml?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="51693-199">Обновите метод `AddTodo`, чтобы добавить `TodoItem` с указываемым названием в список.</span><span class="sxs-lookup"><span data-stu-id="51693-199">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="51693-200">Очистите значение текстового поля, задав пустую строку в качестве значения для `newTodo`.</span><span class="sxs-lookup"><span data-stu-id="51693-200">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData8.cshtml?highlight=19-26)]

1. <span data-ttu-id="51693-201">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="51693-201">Rebuild and run the app.</span></span> <span data-ttu-id="51693-202">Добавьте несколько элементов в список Todo, чтобы протестировать работу нового кода.</span><span class="sxs-lookup"><span data-stu-id="51693-202">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="51693-203">Вы можете сделать текст заголовка для каждого элемента списка дел редактируемым, а по дополнительному флажку пользователь сможет отслеживать завершение задач.</span><span class="sxs-lookup"><span data-stu-id="51693-203">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="51693-204">Добавьте флажок для каждого элемента списка дел и привяжите его значение к свойству `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="51693-204">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="51693-205">Замените `@todo.Title` элементом `<input>`, который привязан к `@todo.Title`.</span><span class="sxs-lookup"><span data-stu-id="51693-205">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData9.cshtml?highlight=5-6)]

1. <span data-ttu-id="51693-206">Чтобы проверить привязку значений, обновите заголовок `<h1>`, чтобы в нем отображалось количество незавершенных дел в списке (`IsDone` имеет значение `false`).</span><span class="sxs-lookup"><span data-stu-id="51693-206">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="51693-207">Готовый компонент Todo (*Todo.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="51693-207">The completed Todo component (*Todo.cshtml*):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Todo.cshtml)]

1. <span data-ttu-id="51693-208">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="51693-208">Rebuild and run the app.</span></span> <span data-ttu-id="51693-209">Добавьте элементы в список дел, чтобы протестировать новый код.</span><span class="sxs-lookup"><span data-stu-id="51693-209">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="51693-210">Публикация и развертывание приложения</span><span class="sxs-lookup"><span data-stu-id="51693-210">Publish and deploy the app</span></span>

<span data-ttu-id="51693-211">Чтобы опубликовать приложение, воспользуйтесь инструкцией из статьи <xref:host-and-deploy/razor-components/index#publish-the-app>.</span><span class="sxs-lookup"><span data-stu-id="51693-211">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
