---
title: Создайте свое первое приложение на основе Razor Components
author: guardrex
description: Пошаговое создание приложения на основе Razor Components и базовые понятия, относящиеся к компонентам Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: c0f7b27fdfc770f8001625ecb3bf8d50af517b99
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "57978427"
---
# <a name="build-your-first-razor-components-app"></a>Создайте свое первое приложение на основе Razor Components

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

В этом руководстве показано, как создать приложение с помощью Razor Components, и описаны основные понятия Razor Components. При работе с этим руководством вы можете использовать проект на основе Razor Components (поддерживается в .NET Core 3.0 и более поздних версиях) или проект на основе Blazor (будет поддерживаться в будущих выпусках .NET Core).

Сведения о работе с Razor Components для ASP.NET Core (*рекомендуется*):

* Выполните инструкции из статьи <xref:razor-components/get-started>, чтобы создать проект на основе Razor Components.
* Задайте для проекта имя `RazorComponents`.

Сведения для работы с Blazor:

* Выполните инструкции из статьи <xref:spa/blazor/get-started>, чтобы создать проект на основе Blazor.
* Задайте для проекта имя `Blazor`.

[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([описание скачивания](xref:index#how-to-download-a-sample)). Предварительные условия описаны в следующих разделах.

## <a name="build-components"></a>Сборка компонентов

1. Перейдите к каждой из трех страниц приложения в папке *Components/Pages* (в Blazor это *Pages*): домашнюю, счетчика и получения данных. Эти страницы реализуются такими файлами компонентов Razor, как *Index.razor*, *Counter.razor* и *FetchData.razor* (в Blazor по-прежнему используется расширение файла *.cshtml* — *Index.cshtml*, *Counter.cshtml* и *FetchData.cshtml*).

1. На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы. Обычно для увеличения значений счетчика на веб-странице требуется код JavaScript, но Razor Components реализует более правильный подход с использованием C#.

1. Изучите реализацию компонента Counter в файле *Counter.razor*.

   *Components/Pages/Counter.razor* (*Pages/Counter.cshtml* в Blazor):

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   Пользовательский интерфейс компонента Counter определяется с помощью HTML. Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor). HTML-разметка и логика отображения C# преобразуются в класс компонента во время сборки. Имя создаваемого класса .NET совпадает с именем файла.

   Элементы класса компонента определяются в блоке `@functions`. В блоке `@functions` указываются состояние (свойства, поля) и методы компонента для обработки событий или определения другой логики компонента. Эти элементы можно позднее включить в логику отображения компонента и обработки событий.

   При нажатии кнопки **Click me** (Щелкните здесь) выполняются следующие действия:

   * Вызывается обработчик `onclick`, зарегистрированный для компонента Counter (метод `IncrementCount`).
   * Компонент Counter повторно создает свое дерево отображения.
   * Выполняется сравнение нового и старого деревьев отображения.
   * Применяются только изменения модели DOM (Document Object Model — объектная модель документа). Отображаемое значение счетчика обновляется.

1. Измените логику C# для компонента Counter, чтобы при нажатии кнопки значение счетчика увеличивалось на две единицы вместо одной.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Скомпилируйте и запустите приложение, чтобы увидеть изменения. Нажмите кнопку **Click me** (Щелкните здесь), и значение счетчика увеличится на два.

## <a name="use-components"></a>Использование компонентов

Включите компонент в другой компонент, используя синтаксис в стиле HTML.

1. Добавьте компонент Counter в компонент Index (домашняя страница), разместив `<Counter />` внутри компонента Index.

   Если вы используете Blazor для этой задачи, в компонент индекса добавляется компонент опроса Prompt (элемент `<SurveyPrompt>`). Замените элемент `<SurveyPrompt>` элементом `<Counter>`.

   *Components/Pages/Index.razor* (*Pages/Index.cshtml* в Blazor):

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. Скомпилируйте и запустите приложение. На домашней странице есть свой счетчик.

## <a name="component-parameters"></a>Параметры компонентов

Компоненты также могут принимать параметры. Параметры для компонентов определяются с помощью частных свойств в классе компонента с атрибутом `[Parameter]`. Используйте атрибуты, чтобы указать аргументы для компонента в разметке.

1. Обновите код C# для `@functions` в компоненте:

   * Добавьте свойство `IncrementAmount` с атрибутом `[Parameter]`.
   * Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.

   *Components/Pages/Counter.razor* (*Pages/Counter.cshtml* в Blazor):

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Укажите параметр `IncrementAmount` в элементе `<Counter>` для компонента домашней страницы, используя атрибут. Установите приращение счетчика на десять.

   *Components/Pages/Index.razor* (*Pages/Index.cshtml* в Blazor):

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. Перезагрузите страницу. Теперь счетчик на домашней странице будет увеличиваться на десять при каждом нажатии кнопки **Click me** (Щелкните здесь). А счетчик на странице *Counter* увеличивается на единицу.

## <a name="route-to-components"></a>Маршрутизация к компонентам

Директива `@page` в верхней части файла *Counter.razor* указывает, что этот компонент является конечной точкой маршрутизации. Компонент Counter обрабатывает запросы, направляемые к `/Counter`. Без директивы `@page` этот компонент не будет обрабатывать перенаправленные запросы, но остается доступным для использования другими компонентами.

## <a name="dependency-injection"></a>Внедрение зависимостей

Службы, зарегистрированные в контейнере служб приложения, доступны компонентам через [внедрение зависимостей](xref:fundamentals/dependency-injection). Внедрите службы в компонент с помощью директивы `@inject`.

Изучите директивы компонента FetchData. Директива `@inject` используется для внедрения в компонент экземпляра службы `WeatherForecastService`.

*Components/Pages/FetchData.razor* (*Pages/FetchData.cshtml* в Blazor):

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

Служба `WeatherForecastService` зарегистрирована как [отдельная](xref:fundamentals/dependency-injection#service-lifetimes), то есть для всего приложения предоставляется один экземпляр этой службы.

Компонент FetchData использует внедренную службу как `ForecastService`, чтобы получить массив объектов `WeatherForecast`.

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

Цикл [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) используется для отображения каждого экземпляра прогноза в отдельной строке в таблице погоды.

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>Создание списка дел

Добавьте в приложение новую страницу, представляющую собой простой список дел.

1. Добавьте пустой файл в папку *Components/Pages* (папка *Pages* в Blazor) с именем *Todo.razor*.

1. Задайте начальную разметку страницы.

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Добавьте страницу списка дел на панель навигации.

   Компонент NavMenu (*Components/Shared/NavMenu.razor* или *Shared/NavMenu.cshtml* в Blazor) используется в макете приложения. Макетами называются компоненты, которые избавляют от дублирования содержимого в приложении. Для получения дополнительной информации см. <xref:razor-components/layouts>.

   Добавьте `<NavLink>` на страницу списка дел. Для этого добавьте представленную ниже разметку с элементом списка под существующими элементами списка в файл *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* в Blazor).

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Скомпилируйте и запустите приложение. Посетите новую страницу Todo, чтобы убедиться, что ссылка на страницу Todo работает правильно.

1. Добавьте в корень проекта файл *TodoItem.cs*, который будет размещать класс для элемента списка дел. Используйте следующий код C# для класса `TodoItem`.

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. Вернитесь к компоненту Todo (*Components/Pages/Todo.razor* или *Pages/Todo.cshtml* в Blazor).

   * Добавьте поле в список дел в блоке `@functions`. Компонент Todo использует это поле для хранения состояния списка дел.
   * Добавьте разметку неупорядоченного списка и цикл `foreach` для отображения каждого элемента списка дела в виде элемента списка.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. Приложению необходимы элементы пользовательского интерфейса для добавления дел в список. Добавьте текстовое поле и кнопку под списком.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Скомпилируйте и запустите приложение. При нажатии кнопки **Add todo** (Добавить задачу) ничего не происходит, так как к кнопке не привязан обработчик событий.

1. Добавьте метод `AddTodo` в компонент Todo и с помощью атрибута `onclick` зарегистрируйте его на нажатие кнопки.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   Теперь при нажатии кнопки вызывается метод C# `AddTodo`.

1. Чтобы получить заголовок нового элемента списка дел, добавьте строку поля `newTodo` и привяжите ее к значению вводимого текста с помощью атрибута `bind`.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. Обновите метод `AddTodo`, чтобы добавить `TodoItem` с указываемым названием в список. Очистите значение текстового поля, задав пустую строку в качестве значения для `newTodo`.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Скомпилируйте и запустите приложение. Добавьте несколько элементов в список Todo, чтобы протестировать работу нового кода.

1. Вы можете сделать текст заголовка для каждого элемента списка дел редактируемым, а по дополнительному флажку пользователь сможет отслеживать завершение задач. Добавьте флажок для каждого элемента списка дел и привяжите его значение к свойству `IsDone`. Замените `@todo.Title` элементом `<input>`, который привязан к `@todo.Title`.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Чтобы проверить привязку значений, обновите заголовок `<h1>`, чтобы в нем отображалось количество незавершенных дел в списке (`IsDone` имеет значение `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Завершенный компонент Todo (*Components/Pages/Todo.razor* или *Pages/Todo.cshtml* в Blazor):

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. Скомпилируйте и запустите приложение. Добавьте элементы в список дел, чтобы протестировать новый код.

## <a name="publish-and-deploy-the-app"></a>Публикация и развертывание приложения

Чтобы опубликовать приложение, воспользуйтесь инструкцией из статьи <xref:host-and-deploy/razor-components/index#publish-the-app>.
