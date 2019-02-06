---
title: Создание приложения Blazor
author: guardrex
description: Создайте приложение Blazor, выполнив пошаговые инструкции, и быстро изучите основные возможности платформы Razor Components.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 99396e200eb121524abccc20a7d461062c94ecc7
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668030"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="e74c2-103">Создание приложения Blazor</span><span class="sxs-lookup"><span data-stu-id="e74c2-103">Build your first Blazor app</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="e74c2-104">В этом руководстве описано, как создать приложение Blazor, выполнив пошаговые инструкции, и быстро изучить основные возможности платформы Razor Components.</span><span class="sxs-lookup"><span data-stu-id="e74c2-104">In this tutorial, you build a Blazor app step-by-step and quickly learn the basic features of the Razor Components framework.</span></span>

<span data-ttu-id="e74c2-105">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e74c2-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="e74c2-106">Сведения о необходимых компонентах см. в разделе [Начало работы](xref:razor-components/get-started).</span><span class="sxs-lookup"><span data-stu-id="e74c2-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="e74c2-107">Чтобы создать проект в Visual Studio, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="e74c2-107">To create the project in Visual Studio:</span></span>

1. <span data-ttu-id="e74c2-108">Выберите **Файл** > **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="e74c2-108">Select **File** > **New** > **Project**.</span></span> <span data-ttu-id="e74c2-109">Выберите **Интернет** > **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e74c2-109">Select **Web** > **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="e74c2-110">В поле **Имя** укажите имя проекта — BlazorApp1.</span><span class="sxs-lookup"><span data-stu-id="e74c2-110">Name the project "BlazorApp1" in the **Name** field.</span></span> <span data-ttu-id="e74c2-111">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="e74c2-111">Select **OK**.</span></span>

    ![Проект ASP.NET Core](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. <span data-ttu-id="e74c2-113">Отобразится диалоговое окно **Создание веб-приложения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e74c2-113">The **New ASP.NET Core Web Application** dialog appears.</span></span> <span data-ttu-id="e74c2-114">Убедитесь, что в раскрывающемся списке вверху выбрано **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e74c2-114">Make sure **.NET Core** is selected at the top.</span></span> <span data-ttu-id="e74c2-115">Выберите **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="e74c2-115">Select **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="e74c2-116">Выберите шаблон **Blazor** и нажмите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="e74c2-116">Choose the **Blazor** template and select **OK**.</span></span>

    ![Диалоговое окно создания приложения Blazor](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. <span data-ttu-id="e74c2-118">Создав проект, нажмите клавиши **CTRL+F5** для запуска приложения *без отладчика*.</span><span class="sxs-lookup"><span data-stu-id="e74c2-118">Once the project is created, press **Ctrl-F5** to run the app *without the debugger*.</span></span> <span data-ttu-id="e74c2-119">Запуск с отладчиком (**F5**) сейчас не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="e74c2-119">Running with the debugger (**F5**) isn't supported at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="e74c2-120">Если вы не используете Visual Studio, создайте приложение Blazor из командной строки в Windows, macOS или Linux.</span><span class="sxs-lookup"><span data-stu-id="e74c2-120">If not using Visual Studio, create the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> <span data-ttu-id="e74c2-121">Перейдите к приложению, используя адрес и порт localhost, которые отображаются в окне консоли после выполнения команды `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-121">Navigate to the app using the localhost address and port provided in the console window output after `dotnet run` is executed.</span></span> <span data-ttu-id="e74c2-122">Используйте клавиши **CTRL+C** в окне консоли, чтобы завершить работу приложения.</span><span class="sxs-lookup"><span data-stu-id="e74c2-122">Use **Ctrl-C** in the console window to shutdown the app.</span></span>

<span data-ttu-id="e74c2-123">Приложение Blazor запустится в браузере.</span><span class="sxs-lookup"><span data-stu-id="e74c2-123">The Blazor app runs in the browser:</span></span>

![Домашняя страница приложения Blazor](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a><span data-ttu-id="e74c2-125">Сборка компонентов</span><span class="sxs-lookup"><span data-stu-id="e74c2-125">Build components</span></span>

1. <span data-ttu-id="e74c2-126">Перейдите на каждую из трех страниц приложения: домашнюю, счетчика и получения данных.</span><span class="sxs-lookup"><span data-stu-id="e74c2-126">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span>

    <span data-ttu-id="e74c2-127">Эти три страницы реализуются посредством трех файлов Razor из папки *Pages*: *Index.cshtml*, *Counter.cshtml* и *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e74c2-127">These three pages are implemented by the three Razor files in the *Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span> <span data-ttu-id="e74c2-128">Каждый из этих файлов реализует компонент, который компилируется и выполняется на стороне клиента в браузере.</span><span class="sxs-lookup"><span data-stu-id="e74c2-128">Each of these files implements a component that's compiled and executed client-side in the browser.</span></span>

1. <span data-ttu-id="e74c2-129">Нажмите кнопку на странице счетчика.</span><span class="sxs-lookup"><span data-stu-id="e74c2-129">Select the button on the Counter page.</span></span>

    ![Домашняя страница приложения Blazor](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    <span data-ttu-id="e74c2-131">При каждом нажатии кнопки счетчик срабатывает без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="e74c2-131">Each time the button is selected, the counter is incremented without a page refresh.</span></span> <span data-ttu-id="e74c2-132">Такое поведение на стороне клиента обычно реализуется с помощью JavaScript, тогда как в нашем случае используется компонент `Counter` на C# и .NET.</span><span class="sxs-lookup"><span data-stu-id="e74c2-132">Normally, this kind of client-side behavior is handled in JavaScript; but here, it's implemented in C# and .NET by the `Counter` component.</span></span>

1. <span data-ttu-id="e74c2-133">Вот как реализован компонент `Counter` в файле *Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e74c2-133">Take a look at the implementation of the `Counter` component in the *Counter.cshtml* file:</span></span>

    ```cshtml
    @page "/counter"

    <h1>Counter</h1>

    <p>Current count: @currentCount</p>

    <button class="btn btn-primary" onclick="@IncrementCount">Click me</button>

    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
    ```

    <span data-ttu-id="e74c2-134">Пользовательский интерфейс компонента `Counter` определяется с помощью обычного HTML.</span><span class="sxs-lookup"><span data-stu-id="e74c2-134">The UI for the `Counter` component is defined using normal HTML.</span></span> <span data-ttu-id="e74c2-135">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="e74c2-135">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span></span> <span data-ttu-id="e74c2-136">HTML-разметка и логика отображения C# преобразуются в класс компонента во время сборки.</span><span class="sxs-lookup"><span data-stu-id="e74c2-136">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="e74c2-137">Имя создаваемого класса .NET соответствует имени файла.</span><span class="sxs-lookup"><span data-stu-id="e74c2-137">The name of the generated .NET class matches the name of the file.</span></span>

    <span data-ttu-id="e74c2-138">Элементы класса компонента определяются в блоке `@functions`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-138">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="e74c2-139">В блоке `@functions` может указываться состояние (свойства, поля) и методы компонента для обработки событий или определения другой логики компонента.</span><span class="sxs-lookup"><span data-stu-id="e74c2-139">In the `@functions` block, component state (properties, fields) and methods can be specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="e74c2-140">Затем эти элементы можно использовать как часть логики отображения компонента, а также для обработки событий.</span><span class="sxs-lookup"><span data-stu-id="e74c2-140">These members can then be used as part of the component's rendering logic and for handling events.</span></span>

    <span data-ttu-id="e74c2-141">При нажатии кнопки вызывается зарегистрированный обработчик `onclick`компонента `Counter` (метод `IncrementCount`), и компонент `Counter` заново создает свое дерево визуализации.</span><span class="sxs-lookup"><span data-stu-id="e74c2-141">When the button is selected, the `Counter` component's registered `onclick` handler is called (the `IncrementCount` method) and the `Counter` component regenerates its render tree.</span></span> <span data-ttu-id="e74c2-142">Blazor сравнивает новое и прежнее дерево визуализации и применяет все изменения в модели DOM браузера.</span><span class="sxs-lookup"><span data-stu-id="e74c2-142">Blazor compares the new render tree against the previous one and applies any modifications to the browser Document Object Model (DOM).</span></span> <span data-ttu-id="e74c2-143">Отображаемое значение счетчика обновляется.</span><span class="sxs-lookup"><span data-stu-id="e74c2-143">The displayed count is updated.</span></span>

1. <span data-ttu-id="e74c2-144">Измените разметку для компонента `Counter`, чтобы сделать заголовок верхнего уровня *интереснее*.</span><span class="sxs-lookup"><span data-stu-id="e74c2-144">Update the markup for the `Counter` component to make the top-level header more *exciting*.</span></span>

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. <span data-ttu-id="e74c2-145">Также измените логику C# компонента `Counter`, чтобы при нажатии кнопки значение счетчика увеличивалось на две единицы вместо одной.</span><span class="sxs-lookup"><span data-stu-id="e74c2-145">Also modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. <span data-ttu-id="e74c2-146">Обновите страницу счетчика в браузере, чтобы отобразить изменения.</span><span class="sxs-lookup"><span data-stu-id="e74c2-146">Refresh the counter page in the browser to see the changes.</span></span>

    ![Счетчик с измененным заголовком](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a><span data-ttu-id="e74c2-148">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="e74c2-148">Use components</span></span>

<span data-ttu-id="e74c2-149">Определенный компонент можно использовать для реализации других компонентов.</span><span class="sxs-lookup"><span data-stu-id="e74c2-149">After a component is defined, the component can be used to implement other components.</span></span> <span data-ttu-id="e74c2-150">Разметка для использования компонента выглядит как тег HTML с именем, соответствующем типу компонента.</span><span class="sxs-lookup"><span data-stu-id="e74c2-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

1. <span data-ttu-id="e74c2-151">Добавьте компонент `Counter` на домашнюю страницу приложения (*Index.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="e74c2-151">Add a `Counter` component to the Home page of the app (*Index.cshtml*).</span></span>

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. <span data-ttu-id="e74c2-152">Обновите домашнюю страницу в браузере.</span><span class="sxs-lookup"><span data-stu-id="e74c2-152">Refresh the home page in the browser.</span></span> <span data-ttu-id="e74c2-153">Теперь на домашней странице появился отдельный экземпляр компонента `Counter`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-153">Note the separate instance of the `Counter` component on the Home page.</span></span>

    ![Домашняя страница Blazor со счетчиком](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a><span data-ttu-id="e74c2-155">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="e74c2-155">Component parameters</span></span>

<span data-ttu-id="e74c2-156">Компоненты также могут иметь параметры, которые определяются с помощью частных свойств класса компонента в разделе `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-156">Components can also have parameters, which are defined using private properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="e74c2-157">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="e74c2-157">Use attributes to specify arguments for a component in markup.</span></span> 

1. <span data-ttu-id="e74c2-158">Обновите компонент `Counter`, задав параметру `IncrementAmount` значение по умолчанию — 1.</span><span class="sxs-lookup"><span data-stu-id="e74c2-158">Update the `Counter` component to have an `IncrementAmount` parameter that defaults to 1.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        [Parameter]
        private int IncrementAmount { get; set; } = 1;

        void IncrementCount()
        {
            currentCount += IncrementAmount;
        }
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="e74c2-159">В Visual Studio можно быстро добавлять параметры компонента с помощью фрагмента кода `para`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-159">From Visual Studio, you can quickly add a component parameter by using the `para` snippet.</span></span> <span data-ttu-id="e74c2-160">Введите `para` и дважды нажмите клавишу `Tab`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-160">Type `para` and then press the `Tab` key twice.</span></span>

1. <span data-ttu-id="e74c2-161">На домашней странице (*Index.cshtml*) измените значение приращения `Counter` на 10, задав атрибут, который соответствует имени свойства компонента `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-161">On the Home page (*Index.cshtml*), change the increment amount for the `Counter` to 10 by setting an attribute that matches the name of the component's property for `IncrementCount`.</span></span>

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. <span data-ttu-id="e74c2-162">Перезагрузите страницу.</span><span class="sxs-lookup"><span data-stu-id="e74c2-162">Reload the page.</span></span>

    <span data-ttu-id="e74c2-163">Счетчик на домашней странице теперь имеет шаг в 10 единиц, тогда как счетчик на странице счетчика по-прежнему добавляет одну единицу при срабатывании.</span><span class="sxs-lookup"><span data-stu-id="e74c2-163">The counter on the Home page now increments by 10, while the counter on the Counter page still increments by 1.</span></span>

    ![Счетчик приложения Blazor с шагом в 10 единиц](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a><span data-ttu-id="e74c2-165">Маршрутизация к компонентам</span><span class="sxs-lookup"><span data-stu-id="e74c2-165">Route to components</span></span>

<span data-ttu-id="e74c2-166">Директива `@page` в верхней части файла *Counter.cshtml* указывает, что этот компонент является страницей, к которой можно направлять запросы.</span><span class="sxs-lookup"><span data-stu-id="e74c2-166">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a page to which requests can be routed.</span></span> <span data-ttu-id="e74c2-167">В частности компонент `Counter` обрабатывает запросы, направляемые к `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-167">Specifically, the `Counter` component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="e74c2-168">Без директивы `@page` компонент не будет обрабатывать перенаправленные запросы, но все равно может использоваться другими компонентами.</span><span class="sxs-lookup"><span data-stu-id="e74c2-168">Without the `@page` directive, the component wouldn't handle routed requests, but the component could still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="e74c2-169">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="e74c2-169">Dependency injection</span></span>

<span data-ttu-id="e74c2-170">Службы, зарегистрированные поставщиком служб приложения, доступны компонентам путем [внедрения зависимостей](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e74c2-170">Services registered with the app's service provider are available to components via [dependency injection (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span></span> <span data-ttu-id="e74c2-171">Службы могут внедряться в компонент с помощью директивы `@inject`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-171">Services can be injected into a component using the `@inject` directive.</span></span>

<span data-ttu-id="e74c2-172">Вот как реализован компонент `FetchData` в файле *FetchData.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e74c2-172">Take a look at the implementation of the `FetchData` component in *FetchData.cshtml*.</span></span> <span data-ttu-id="e74c2-173">Директива `@inject` используется для внедрения в компонент экземпляра [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient).</span><span class="sxs-lookup"><span data-stu-id="e74c2-173">The `@inject` directive is used to inject an [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) instance into the component.</span></span>

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

<span data-ttu-id="e74c2-174">Компонент `FetchData` использует внедренный экземпляр `HttpClient` для получения данных JSON от сервера при инициализации компонента.</span><span class="sxs-lookup"><span data-stu-id="e74c2-174">The `FetchData` component uses the injected `HttpClient` to retrieve JSON data from the server when the component is initialized.</span></span> <span data-ttu-id="e74c2-175">На самом деле `HttpClient`, предоставляемый средой выполнения Blazor, реализуется путем взаимодействия JavaScript для вызова API получения данных браузера и отправки запроса (в C# можно вызвать любую библиотеку JavaScript или API браузера).</span><span class="sxs-lookup"><span data-stu-id="e74c2-175">Under the covers, the `HttpClient` provided by the Blazor runtime is implemented using JavaScript interop to call the underlying browser's Fetch API to send the request (from C#, it's possible to call any JavaScript library or browser API).</span></span> <span data-ttu-id="e74c2-176">Полученные данные десериализируются в переменную C# `forecasts` в виде массива объектов `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-176">The retrieved data is deserialized into the `forecasts` C# variable as an array of `WeatherForecast` objects.</span></span>

```cshtml
@functions {
    WeatherForecast[] forecasts;

    protected override async Task OnInitAsync()
    {
        forecasts = await Http.GetJsonAsync<WeatherForecast[]>("/sample-data/weather.json");
    }

    class WeatherForecast
    {
        public DateTime Date { get; set; }
        public int TemperatureC { get; set; }
        public int TemperatureF { get; set; }
        public string Summary { get; set; }
    }
}
```

<span data-ttu-id="e74c2-177">Цикл `@foreach` используется для отображения каждого экземпляра прогноза в виде строки в таблице погоды.</span><span class="sxs-lookup"><span data-stu-id="e74c2-177">A `@foreach` loop is used to render each forecast instance as a row in the weather table.</span></span>

```cshtml
<table class="table">
    <thead>
        <tr>
            <th>Date</th>
            <th>Temp. (C)</th>
            <th>Temp. (F)</th>
            <th>Summary</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var forecast in forecasts)
        {
            <tr>
                <td>@forecast.Date.ToShortDateString()</td>
                <td>@forecast.TemperatureC</td>
                <td>@forecast.TemperatureF</td>
                <td>@forecast.Summary</td>
            </tr>
        }
    </tbody>
</table>
```

## <a name="build-a-todo-list"></a><span data-ttu-id="e74c2-178">Создание списка дел</span><span class="sxs-lookup"><span data-stu-id="e74c2-178">Build a todo list</span></span>

<span data-ttu-id="e74c2-179">Добавьте в приложение новую страницу, представляющую собой простой список дел.</span><span class="sxs-lookup"><span data-stu-id="e74c2-179">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="e74c2-180">В папку *Pages* добавьте пустой текстовый файл с именем *Todo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e74c2-180">Add an empty text file to the *Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="e74c2-181">Задайте начальную разметку страницы.</span><span class="sxs-lookup"><span data-stu-id="e74c2-181">Provide the initial markup for the page.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. <span data-ttu-id="e74c2-182">Добавьте страницу списка дел на панель навигации, обновив файл *Shared/NavMenu.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e74c2-182">Add the Todo page to the navigation bar by updating *Shared/NavMenu.cshtml*.</span></span> <span data-ttu-id="e74c2-183">Добавьте `NavLink` для страницы дел, добавив следующую разметку элемента списка под существующими элементами списка.</span><span class="sxs-lookup"><span data-stu-id="e74c2-183">Add a `NavLink` for the Todo page by adding the following list item markup below the existing list items.</span></span>

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. <span data-ttu-id="e74c2-184">Обновите приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="e74c2-184">Refresh the app in the browser.</span></span> <span data-ttu-id="e74c2-185">Перейдите на созданную страницу списка дел.</span><span class="sxs-lookup"><span data-stu-id="e74c2-185">See the new Todo page.</span></span>

    ![Страница списка дел приложения Blazor](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. <span data-ttu-id="e74c2-187">Добавьте файл *TodoItem.cs* в корень проекта для размещения класса, представляющего элементы списка дел.</span><span class="sxs-lookup"><span data-stu-id="e74c2-187">Add a *TodoItem.cs* file to the root of the project to hold a class to represent the todo items.</span></span>

1. <span data-ttu-id="e74c2-188">Используйте следующий код C# для класса `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-188">Use the following C# code for the `TodoItem` class.</span></span>

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. <span data-ttu-id="e74c2-189">Вернитесь к компоненту `Todo` в файле *Todo.cshtml* и добавьте поле для добавления дел в блоке `@functions`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-189">Go back to the `Todo` component in *Todo.cshtml* and add a field for the todos in a `@functions` block.</span></span> <span data-ttu-id="e74c2-190">Компонент `Todo` использует это поле для сохранения состояния списка дел.</span><span class="sxs-lookup"><span data-stu-id="e74c2-190">The `Todo` component uses this field to maintain the state of the todo list.</span></span>

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="e74c2-191">Добавьте разметку неупорядоченного списка и цикл `foreach` для отображения каждого элемента списка дела в виде элемента списка.</span><span class="sxs-lookup"><span data-stu-id="e74c2-191">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>
    ```

1. <span data-ttu-id="e74c2-192">Приложению необходимы элементы пользовательского интерфейса для добавления дел в список.</span><span class="sxs-lookup"><span data-stu-id="e74c2-192">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="e74c2-193">Добавьте текстовое поле и кнопку под списком.</span><span class="sxs-lookup"><span data-stu-id="e74c2-193">Add a text input and a button below the list.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" />
    <button>Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="e74c2-194">Обновите приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="e74c2-194">Refresh the browser.</span></span>

    ![Добавление кнопки в список дел](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    <span data-ttu-id="e74c2-196">При нажатии кнопки **Add todo** (Добавить дело) ничего не происходит, так как к кнопке не привязан обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="e74c2-196">Nothing happens when the **Add todo** button is selected because no event handler is wired up to the button.</span></span>

1. <span data-ttu-id="e74c2-197">Добавьте метод `AddTodo` в компонент `Todo` и зарегистрируйте его для нажатий кнопки с помощью атрибута `onclick`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-197">Add an `AddTodo` method to the `Todo` component and register it for button clicks using the `onclick` attribute.</span></span>

    ```cshtml
    <input placeholder="Something todo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();

        void AddTodo()
        {
            // Todo: Add the todo
        }
    }
    ```

    <span data-ttu-id="e74c2-198">Метод C# `AddTodo` вызывается при каждом нажатии кнопки.</span><span class="sxs-lookup"><span data-stu-id="e74c2-198">The `AddTodo` C# method is called every time the button is selected.</span></span>

1. <span data-ttu-id="e74c2-199">Чтобы получить заголовок нового элемента списка дел, добавьте строку поля `newTodo` и привяжите ее к значению вводимого текста с помощью атрибута `bind`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-199">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute.</span></span>

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. <span data-ttu-id="e74c2-200">Обновите метод `AddTodo`, чтобы добавить `TodoItem` с указываемым названием в список.</span><span class="sxs-lookup"><span data-stu-id="e74c2-200">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="e74c2-201">Не забудьте очистить значение вводимого текста, задав пустую строку для `newTodo`.</span><span class="sxs-lookup"><span data-stu-id="e74c2-201">Don't forget to clear the value of the text input by setting `newTodo` to an empty string.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

1. <span data-ttu-id="e74c2-202">Обновите приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="e74c2-202">Refresh the browser.</span></span> <span data-ttu-id="e74c2-203">Добавьте несколько дел в список.</span><span class="sxs-lookup"><span data-stu-id="e74c2-203">Add some todos to the todo list.</span></span>

    ![Добавление дел в список](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. <span data-ttu-id="e74c2-205">Наконец, включим возможность помечать сделанные дела галочками.</span><span class="sxs-lookup"><span data-stu-id="e74c2-205">Lastly, what's a todo list without check boxes?</span></span> <span data-ttu-id="e74c2-206">Можно также добавить возможность редактировать каждый элемент списка дел.</span><span class="sxs-lookup"><span data-stu-id="e74c2-206">The title text for each todo item can be made editable as well.</span></span> <span data-ttu-id="e74c2-207">Добавьте флажок и текстовое поле для ввода для каждого элемента списка дел и привяжите их значения к свойствам `Title` и `IsDone` соответственно.</span><span class="sxs-lookup"><span data-stu-id="e74c2-207">Add a check box input and text input for each todo item and bind their values to the `Title` and `IsDone` properties, respectively.</span></span>

    ```cshtml
    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>
    ```

1. <span data-ttu-id="e74c2-208">Чтобы убедиться, что эти значения привязаны, обновите заголовок `h1` для отображения в нем количества незавершенных дел (`IsDone` — `false`).</span><span class="sxs-lookup"><span data-stu-id="e74c2-208">To verify that these values are bound, update the `h1` header to show a count of the number of todo items that are not yet done (`IsDone` is `false`).</span></span>

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. <span data-ttu-id="e74c2-209">Готовый компонент `Todo` должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e74c2-209">The completed `Todo` component should look like this:</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

<span data-ttu-id="e74c2-210">Обновите приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="e74c2-210">Refresh the app in the browser.</span></span> <span data-ttu-id="e74c2-211">Добавьте несколько дел в список.</span><span class="sxs-lookup"><span data-stu-id="e74c2-211">Try adding some todo items.</span></span>

![Готовый список дел в приложении Blazor](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a><span data-ttu-id="e74c2-213">Публикация и развертывание</span><span class="sxs-lookup"><span data-stu-id="e74c2-213">Publish and deploy</span></span>

<span data-ttu-id="e74c2-214">При использовании Visual Studio выполните следующие действия, чтобы опубликовать приложение Blazor в Azure:</span><span class="sxs-lookup"><span data-stu-id="e74c2-214">When using Visual Studio, perform the following steps to publish the Todo Blazor app to Azure:</span></span>

1. <span data-ttu-id="e74c2-215">В **обозревателе решений** щелкните правой кнопкой мыши проект и выберите **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="e74c2-215">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="e74c2-216">В диалоговом оке **Выбор целевого объекта публикации** выберите **Служба приложений**, а затем — **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e74c2-216">In the **Pick a publish target** dialog, select **App Service** and **Create New**.</span></span> <span data-ttu-id="e74c2-217">Щелкните **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="e74c2-217">Select **Publish**.</span></span>

    ![Выбор целевого объекта для публикации](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. <span data-ttu-id="e74c2-219">В диалоговом окне **Создание службы приложений** выберите имя приложения и укажите подписку, группу ресурсов и план размещения.</span><span class="sxs-lookup"><span data-stu-id="e74c2-219">In the **Create App Service** dialog, choose a name for the app and select the subscription, resource group, and hosting plan.</span></span> <span data-ttu-id="e74c2-220">Щелкните **Создать**, чтобы создать службу приложений и опубликовать приложение.</span><span class="sxs-lookup"><span data-stu-id="e74c2-220">Select **Create** to create the app service and publish the app.</span></span>

    ![Создание службы приложений](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

<span data-ttu-id="e74c2-222">Подождите около минуты, пока приложение будет развернуто.</span><span class="sxs-lookup"><span data-stu-id="e74c2-222">Wait a minute or so for the app to be deployed.</span></span>

<span data-ttu-id="e74c2-223">Теперь приложение должно выполняться в Azure.</span><span class="sxs-lookup"><span data-stu-id="e74c2-223">The app should now be running in Azure.</span></span> <span data-ttu-id="e74c2-224">В своем списке дел можете пометить задачу "Создать приложение Blazor" как *выполненную*.</span><span class="sxs-lookup"><span data-stu-id="e74c2-224">Mark the todo item to build your first Blazor app as *done*.</span></span> <span data-ttu-id="e74c2-225">Отличная работа!</span><span class="sxs-lookup"><span data-stu-id="e74c2-225">Nice job!</span></span>

![Использование Blazor в Azure](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> <span data-ttu-id="e74c2-227">Если вы не используете Visual Studio, опубликуйте приложение Blazor из командной строки в Windows, macOS или Linux.</span><span class="sxs-lookup"><span data-stu-id="e74c2-227">If not using Visual Studio, publish the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet publish -c Release
> ```
>
> <span data-ttu-id="e74c2-228">Развертывание создается в папке */bin/Release/\<имя_целевой_платформы/publish*.</span><span class="sxs-lookup"><span data-stu-id="e74c2-228">The deployment is created in the */bin/Release/\<target-framework>/publish* folder.</span></span> <span data-ttu-id="e74c2-229">Перенесите содержимое папки *publish* на веб-сервер или в службу размещения.</span><span class="sxs-lookup"><span data-stu-id="e74c2-229">Move the contents of the *publish* folder to the server or hosting service.</span></span>
>
> <span data-ttu-id="e74c2-230">См. дополнительные сведения о [размещении и развертывании](xref:host-and-deploy/razor-components/index#publish-the-app).</span><span class="sxs-lookup"><span data-stu-id="e74c2-230">For more information, see the [Host and deploy](xref:host-and-deploy/razor-components/index#publish-the-app) topic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e74c2-231">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e74c2-231">Additional resources</span></span>

<span data-ttu-id="e74c2-232">Пример более сложного приложения Blazor для [поиска авиарейсов](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) доступен на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="e74c2-232">For a more involved Blazor sample app, check out the [Flight Finder sample](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) on GitHub.</span></span>

![Приложение Blazor Flight Finder](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
