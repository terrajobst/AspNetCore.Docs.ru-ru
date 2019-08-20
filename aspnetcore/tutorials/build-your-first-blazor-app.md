---
title: Создание приложения Blazor
author: guardrex
description: Пошаговое создание приложения Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/26/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 88999350b9c7631e15ba9d0242da5880c8ae71ff
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994212"
---
# <a name="build-your-first-blazor-app"></a>Создание приложения Blazor

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)

В этом руководстве показано, как создать и изменить приложение Blazor.

Следуйте указаниям из статьи <xref:blazor/get-started>, чтобы создать проект Blazor для этого руководства. Назовите проект *ToDoList*.

## <a name="build-components"></a>Сборка компонентов

1. Перейдите на каждую из трех страниц, размещенных в папке *Pages*: домашнюю, счетчика и получения данных. Эти страницы реализованы в виде файлов компонентов Razor *Index.razor*, *Counter.razor* и *FetchData.razor*.

1. На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы. Обычно для увеличения значений счетчика на веб-странице требуется код JavaScript, но Blazor реализует более правильный подход с использованием C#.

1. Изучите реализацию компонента `Counter` в файле *Counter.razor*.

   *Pages/Counter.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   Пользовательский интерфейс компонента `Counter` определяется с помощью HTML. Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor). HTML-разметка и логика отображения C# преобразуются в класс компонента во время сборки. Имя создаваемого класса .NET совпадает с именем файла.

   Элементы класса компонента определяются в блоке `@code`. В блоке `@code` указываются состояние (свойства, поля) и методы компонента для обработки событий или определения другой логики компонента. Эти элементы можно позднее включить в логику отображения компонента и обработки событий.

   При нажатии кнопки **Click me** (Щелкните здесь) выполняются следующие действия:

   * Вызывается обработчик `onclick`, зарегистрированный для компонента `Counter` (метод `IncrementCount`).
   * Компонент `Counter` повторно создает свое дерево отображения.
   * Выполняется сравнение нового и старого деревьев отображения.
   * Применяются только изменения модели DOM (Document Object Model — объектная модель документа). Отображаемое значение счетчика обновляется.

1. Измените логику C# компонента `Counter`, чтобы при нажатии кнопки значение счетчика увеличивалось на две единицы вместо одной.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Скомпилируйте и запустите приложение, чтобы увидеть изменения. Нажмите кнопку **Click Me** (Щелкните здесь). Значение счетчика увеличится на две единицы.

## <a name="use-components"></a>Использование компонентов

Включите компонент в другой компонент, используя синтаксис HTML.

1. Добавьте компонент `Counter` в компонент `Index` приложения, разместив элемент `<Counter />` внутри компонента `Index` (*Index.razor*).

   Если вы выполняете эту задачу с помощью клиентской части Blazor, это значит, что компонент `Index` использует компонент `SurveyPrompt`. Замените элемент `<SurveyPrompt>` элементом `<Counter />`. Если вы используете для этой задачи серверное приложение Blazor, добавьте к компоненту `Index` элемент `<Counter />`:

   *Pages/Index.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. Скомпилируйте и запустите приложение. Компонент `Index` имеет свой собственный счетчик.

## <a name="component-parameters"></a>Параметры компонентов

Компоненты также могут принимать параметры. Параметры для компонентов определяются с помощью открытых свойств в классе компонента с атрибутом `[Parameter]`. Используйте атрибуты, чтобы указать аргументы для компонента в разметке.

1. Обновите код C# для `@code` в компоненте:

   * Добавьте свойство `IncrementAmount` с атрибутом `[Parameter]`.
   * Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.

   *Pages/Counter.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Укажите параметр `IncrementAmount` в элементе `<Counter>` для компонента `Index`, используя атрибут. Установите приращение счетчика на десять.

   *Pages/Index.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. Перезагрузите компонент `Index`. Теперь значение счетчика на домашней странице будет увеличиваться на десять при каждом нажатии кнопки **Click me** (Щелкните здесь). Счетчик в компоненте `Counter` продолжает увеличиваться на единицу.

## <a name="route-to-components"></a>Маршрутизация к компонентам

Директива `@page` в верхней части файла *Counter.razor* указывает, что компонент `Counter` является конечной точкой маршрутизации. Компонент `Counter` обрабатывает запросы, направляемые к `/counter`. Без директивы `@page` этот компонент не будет обрабатывать перенаправленные запросы, но останется доступным для использования другими компонентами.

## <a name="dependency-injection"></a>Внедрение зависимостей

Службы, зарегистрированные в контейнере служб приложения, доступны компонентам через [внедрение зависимостей](xref:fundamentals/dependency-injection). Внедрите службы в компонент с помощью директивы `@inject`.

Изучите директивы компонента `FetchData`.

Если вы используете серверное приложение Razor, служба `WeatherForecastService` зарегистрирована как [отдельная](xref:fundamentals/dependency-injection#service-lifetimes), то есть для всего приложения предоставляется один экземпляр этой службы. Директива `@inject` используется для внедрения экземпляра службы `WeatherForecastService` в компонент.

*Pages/FetchData.razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

Компонент `FetchData` использует внедренную службу как `ForecastService`, чтобы получить массив объектов `WeatherForecast`.

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

Если вы работаете с приложением Blazor на стороне клиента, внедряется `HttpClient` для получения данных прогноза погоды из файла *weather.json*, расположенного в папке *wwwroot/sample-data*.

*Pages/FetchData.razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

Цикл [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) используется для отображения каждого экземпляра прогноза в отдельной строке в таблице погоды.

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a>Создание списка дел

Добавьте в приложение новый компонент, представляющий собой простой список дел.

1. Добавьте пустой файл с именем *Todo.cshtml* в папку *Pages*.

1. Задайте начальную разметку компонента.

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Добавьте компонент `Todo` на панель навигации.

   Компонент `NavMenu` (*Shared/NavMenu.razor*) используется в макете этого приложения. Макетами называются компоненты, которые избавляют от дублирования содержимого в приложении.

   Добавьте `<NavLink>` для компонента `Todo`, включив следующую разметку с элементом списка под существующими элементами списка в файл *Shared/NavMenu.razor*.

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Скомпилируйте и запустите приложение. Перейдите на новую страницу Todo, чтобы убедиться, что ссылка на компонент `Todo` работает правильно.

1. Добавьте в корень проекта файл *TodoItem.cs*, который будет размещать класс для элемента списка дел. Используйте следующий код C# для класса `TodoItem`.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. Снова вернитесь к компоненту `Todo` (*Pages/Todo.razor*).

   * Добавьте поле для элементов списка дел в блоке `@code`. Компонент `Todo` использует это поле для сохранения состояния списка дел.
   * Добавьте разметку неупорядоченного списка и цикл `foreach` для отображения каждого элемента списка дела в виде элемента списка (`<li>`).

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. Приложению необходимы элементы пользовательского интерфейса для добавления элементов в список дел. Добавьте текстовое поле (`<input>`) и кнопку (`<button>`) под неупорядоченным списком (`<ul>...</ul>`).

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Скомпилируйте и запустите приложение. При нажатии кнопки **Add todo** (Добавить задачу) ничего не происходит, так как к кнопке не привязан обработчик событий.

1. Добавьте метод `AddTodo` в компонент `Todo` и зарегистрируйте его для нажатий кнопки с помощью атрибута `@onclick`. Теперь при нажатии кнопки вызывается метод C# `AddTodo`:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. Чтобы получить заголовок нового элемента списка дел, добавьте строку поля `newTodo` в верхней части блока `@code` и привяжите ее к значению вводимого текста с помощью атрибута `bind` в элементе `<input>`.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="@newTodo" />
   ```

1. Обновите метод `AddTodo`, чтобы добавить `TodoItem` с указываемым названием в список. Очистите значение текстового поля, задав пустую строку в качестве значения для `newTodo`.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Скомпилируйте и запустите приложение. Добавьте несколько задач в список дел, чтобы проверить работу нового кода.

1. Вы можете сделать текст заголовка для каждого элемента списка дел редактируемым, а по дополнительному флажку пользователь сможет отслеживать завершение задач. Добавьте флажок для каждого элемента списка дел и привяжите его значение к свойству `IsDone`. Замените `@todo.Title` элементом `<input>`, который привязан к `@todo.Title`.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Чтобы проверить привязку значений, обновите заголовок `<h1>`, чтобы в нем отображалось количество незавершенных дел в списке (`IsDone` имеет значение `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Готовый компонент `Todo` (*Pages/Todo.razor*):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. Скомпилируйте и запустите приложение. Добавьте элементы в список дел, чтобы протестировать новый код.

> [!div class="nextstepaction"]
> <xref:blazor/components>
