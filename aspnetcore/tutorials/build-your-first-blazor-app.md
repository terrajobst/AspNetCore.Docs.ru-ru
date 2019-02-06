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
# <a name="build-your-first-blazor-app"></a>Создание приложения Blazor

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

В этом руководстве описано, как создать приложение Blazor, выполнив пошаговые инструкции, и быстро изучить основные возможности платформы Razor Components.

[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([описание скачивания](xref:index#how-to-download-a-sample)). Сведения о необходимых компонентах см. в разделе [Начало работы](xref:razor-components/get-started).

Чтобы создать проект в Visual Studio, сделайте следующее:

1. Выберите **Файл** > **Создать** > **Проект**. Выберите **Интернет** > **Веб-приложение ASP.NET Core**. В поле **Имя** укажите имя проекта — BlazorApp1. Нажмите кнопку **ОК**.

    ![Проект ASP.NET Core](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. Отобразится диалоговое окно **Создание веб-приложения ASP.NET Core**. Убедитесь, что в раскрывающемся списке вверху выбрано **.NET Core**. Выберите **ASP.NET Core 2.1**. Выберите шаблон **Blazor** и нажмите **ОК**.

    ![Диалоговое окно создания приложения Blazor](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. Создав проект, нажмите клавиши **CTRL+F5** для запуска приложения *без отладчика*. Запуск с отладчиком (**F5**) сейчас не поддерживается.

> [!NOTE]
> Если вы не используете Visual Studio, создайте приложение Blazor из командной строки в Windows, macOS или Linux.
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> Перейдите к приложению, используя адрес и порт localhost, которые отображаются в окне консоли после выполнения команды `dotnet run`. Используйте клавиши **CTRL+C** в окне консоли, чтобы завершить работу приложения.

Приложение Blazor запустится в браузере.

![Домашняя страница приложения Blazor](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a>Сборка компонентов

1. Перейдите на каждую из трех страниц приложения: домашнюю, счетчика и получения данных.

    Эти три страницы реализуются посредством трех файлов Razor из папки *Pages*: *Index.cshtml*, *Counter.cshtml* и *FetchData.cshtml*. Каждый из этих файлов реализует компонент, который компилируется и выполняется на стороне клиента в браузере.

1. Нажмите кнопку на странице счетчика.

    ![Домашняя страница приложения Blazor](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    При каждом нажатии кнопки счетчик срабатывает без обновления страницы. Такое поведение на стороне клиента обычно реализуется с помощью JavaScript, тогда как в нашем случае используется компонент `Counter` на C# и .NET.

1. Вот как реализован компонент `Counter` в файле *Counter.cshtml*:

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

    Пользовательский интерфейс компонента `Counter` определяется с помощью обычного HTML. Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor). HTML-разметка и логика отображения C# преобразуются в класс компонента во время сборки. Имя создаваемого класса .NET соответствует имени файла.

    Элементы класса компонента определяются в блоке `@functions`. В блоке `@functions` может указываться состояние (свойства, поля) и методы компонента для обработки событий или определения другой логики компонента. Затем эти элементы можно использовать как часть логики отображения компонента, а также для обработки событий.

    При нажатии кнопки вызывается зарегистрированный обработчик `onclick`компонента `Counter` (метод `IncrementCount`), и компонент `Counter` заново создает свое дерево визуализации. Blazor сравнивает новое и прежнее дерево визуализации и применяет все изменения в модели DOM браузера. Отображаемое значение счетчика обновляется.

1. Измените разметку для компонента `Counter`, чтобы сделать заголовок верхнего уровня *интереснее*.

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. Также измените логику C# компонента `Counter`, чтобы при нажатии кнопки значение счетчика увеличивалось на две единицы вместо одной.

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. Обновите страницу счетчика в браузере, чтобы отобразить изменения.

    ![Счетчик с измененным заголовком](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a>Использование компонентов

Определенный компонент можно использовать для реализации других компонентов. Разметка для использования компонента выглядит как тег HTML с именем, соответствующем типу компонента.

1. Добавьте компонент `Counter` на домашнюю страницу приложения (*Index.cshtml*).

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. Обновите домашнюю страницу в браузере. Теперь на домашней странице появился отдельный экземпляр компонента `Counter`.

    ![Домашняя страница Blazor со счетчиком](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a>Параметры компонентов

Компоненты также могут иметь параметры, которые определяются с помощью частных свойств класса компонента в разделе `[Parameter]`. Используйте атрибуты, чтобы указать аргументы для компонента в разметке. 

1. Обновите компонент `Counter`, задав параметру `IncrementAmount` значение по умолчанию — 1.

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
    > В Visual Studio можно быстро добавлять параметры компонента с помощью фрагмента кода `para`. Введите `para` и дважды нажмите клавишу `Tab`.

1. На домашней странице (*Index.cshtml*) измените значение приращения `Counter` на 10, задав атрибут, который соответствует имени свойства компонента `IncrementCount`.

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. Перезагрузите страницу.

    Счетчик на домашней странице теперь имеет шаг в 10 единиц, тогда как счетчик на странице счетчика по-прежнему добавляет одну единицу при срабатывании.

    ![Счетчик приложения Blazor с шагом в 10 единиц](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a>Маршрутизация к компонентам

Директива `@page` в верхней части файла *Counter.cshtml* указывает, что этот компонент является страницей, к которой можно направлять запросы. В частности компонент `Counter` обрабатывает запросы, направляемые к `/Counter`. Без директивы `@page` компонент не будет обрабатывать перенаправленные запросы, но все равно может использоваться другими компонентами.

## <a name="dependency-injection"></a>Внедрение зависимостей

Службы, зарегистрированные поставщиком служб приложения, доступны компонентам путем [внедрения зависимостей](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Службы могут внедряться в компонент с помощью директивы `@inject`.

Вот как реализован компонент `FetchData` в файле *FetchData.cshtml*: Директива `@inject` используется для внедрения в компонент экземпляра [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient).

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

Компонент `FetchData` использует внедренный экземпляр `HttpClient` для получения данных JSON от сервера при инициализации компонента. На самом деле `HttpClient`, предоставляемый средой выполнения Blazor, реализуется путем взаимодействия JavaScript для вызова API получения данных браузера и отправки запроса (в C# можно вызвать любую библиотеку JavaScript или API браузера). Полученные данные десериализируются в переменную C# `forecasts` в виде массива объектов `WeatherForecast`.

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

Цикл `@foreach` используется для отображения каждого экземпляра прогноза в виде строки в таблице погоды.

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

## <a name="build-a-todo-list"></a>Создание списка дел

Добавьте в приложение новую страницу, представляющую собой простой список дел.

1. В папку *Pages* добавьте пустой текстовый файл с именем *Todo.cshtml*.

1. Задайте начальную разметку страницы.

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. Добавьте страницу списка дел на панель навигации, обновив файл *Shared/NavMenu.cshtml*. Добавьте `NavLink` для страницы дел, добавив следующую разметку элемента списка под существующими элементами списка.

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. Обновите приложение в браузере. Перейдите на созданную страницу списка дел.

    ![Страница списка дел приложения Blazor](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. Добавьте файл *TodoItem.cs* в корень проекта для размещения класса, представляющего элементы списка дел.

1. Используйте следующий код C# для класса `TodoItem`.

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. Вернитесь к компоненту `Todo` в файле *Todo.cshtml* и добавьте поле для добавления дел в блоке `@functions`. Компонент `Todo` использует это поле для сохранения состояния списка дел.

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. Добавьте разметку неупорядоченного списка и цикл `foreach` для отображения каждого элемента списка дела в виде элемента списка.

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

1. Приложению необходимы элементы пользовательского интерфейса для добавления дел в список. Добавьте текстовое поле и кнопку под списком.

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

1. Обновите приложение в браузере.

    ![Добавление кнопки в список дел](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    При нажатии кнопки **Add todo** (Добавить дело) ничего не происходит, так как к кнопке не привязан обработчик событий.

1. Добавьте метод `AddTodo` в компонент `Todo` и зарегистрируйте его для нажатий кнопки с помощью атрибута `onclick`.

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

    Метод C# `AddTodo` вызывается при каждом нажатии кнопки.

1. Чтобы получить заголовок нового элемента списка дел, добавьте строку поля `newTodo` и привяжите ее к значению вводимого текста с помощью атрибута `bind`.

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. Обновите метод `AddTodo`, чтобы добавить `TodoItem` с указываемым названием в список. Не забудьте очистить значение вводимого текста, задав пустую строку для `newTodo`.

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

1. Обновите приложение в браузере. Добавьте несколько дел в список.

    ![Добавление дел в список](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. Наконец, включим возможность помечать сделанные дела галочками. Можно также добавить возможность редактировать каждый элемент списка дел. Добавьте флажок и текстовое поле для ввода для каждого элемента списка дел и привяжите их значения к свойствам `Title` и `IsDone` соответственно.

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

1. Чтобы убедиться, что эти значения привязаны, обновите заголовок `h1` для отображения в нем количества незавершенных дел (`IsDone` — `false`).

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. Готовый компонент `Todo` должен выглядеть следующим образом:

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

Обновите приложение в браузере. Добавьте несколько дел в список.

![Готовый список дел в приложении Blazor](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a>Публикация и развертывание

При использовании Visual Studio выполните следующие действия, чтобы опубликовать приложение Blazor в Azure:

1. В **обозревателе решений** щелкните правой кнопкой мыши проект и выберите **Опубликовать**.

1. В диалоговом оке **Выбор целевого объекта публикации** выберите **Служба приложений**, а затем — **Создать**. Щелкните **Опубликовать**.

    ![Выбор целевого объекта для публикации](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. В диалоговом окне **Создание службы приложений** выберите имя приложения и укажите подписку, группу ресурсов и план размещения. Щелкните **Создать**, чтобы создать службу приложений и опубликовать приложение.

    ![Создание службы приложений](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

Подождите около минуты, пока приложение будет развернуто.

Теперь приложение должно выполняться в Azure. В своем списке дел можете пометить задачу "Создать приложение Blazor" как *выполненную*. Отличная работа!

![Использование Blazor в Azure](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> Если вы не используете Visual Studio, опубликуйте приложение Blazor из командной строки в Windows, macOS или Linux.
>
> ```console
> dotnet publish -c Release
> ```
>
> Развертывание создается в папке */bin/Release/\<имя_целевой_платформы/publish*. Перенесите содержимое папки *publish* на веб-сервер или в службу размещения.
>
> См. дополнительные сведения о [размещении и развертывании](xref:host-and-deploy/razor-components/index#publish-the-app).

## <a name="additional-resources"></a>Дополнительные ресурсы

Пример более сложного приложения Blazor для [поиска авиарейсов](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) доступен на сайте GitHub.

![Приложение Blazor Flight Finder](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
