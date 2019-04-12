---
title: Создание и использование компонентов Razor
author: guardrex
description: Узнайте, как создать и использовать компоненты Razor, включая как привязка к данным, обработка событий и управление компонент жизненные циклы.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/components
ms.openlocfilehash: f8ac7f3844b94a162e8d1c45f80ae153d89536ee
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515696"
---
# <a name="create-and-use-razor-components"></a>Создание и использование компонентов Razor

По [Люк Лэтем](https://github.com/guardrex), [Дэниэл рот](https://github.com/danroth27), и [Morné Zaayman](https://github.com/MorneZaayman)

[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([описание скачивания](xref:index#how-to-download-a-sample)). Сведения о необходимых компонентах см. в разделе [Начало работы](xref:razor-components/get-started).

Razor компоненты приложения создаются с помощью *компоненты*. Компонент — это автономное фрагмент пользовательский интерфейс (UI), например страницы, диалоговое окно или форму. Компонент включает разметки HTML и логика обработки, необходимые для вставки данных и реагировать на события пользовательского интерфейса. Компоненты, гибкий и простой. Они могут быть вложенными повторно и совместно использоваться проектами.

## <a name="component-classes"></a>Классы компонентов

Компоненты обычно выполнены в файлах Razor компонента (*.razor*) с помощью сочетания C# и HTML-разметку (*.cshtml* файлы используются в приложениях Blazor).

Компоненты могут разрабатываться в приложениях Razor компонентов, с помощью *.cshtml* расширение файла, до тех пор, пока файлы определяются как файлы Razor компонента, с помощью `_RazorComponentInclude` свойства MSBuild. Например, приложение, созданное с помощью шаблона Razor компонент указывает, что все *.cshtml* файлы в разделе *компоненты* папки, которые должны рассматриваться как файлы Razor компоненты:

```xml
<_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
```

Пользовательский Интерфейс для компонента определяется с помощью HTML. Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor). При компиляции приложения компонентов Razor, HTML-разметка и C# логикой отрисовки, преобразуются в класс компонента. Имя создаваемого класса соответствует имени файла.

Члены класса компонента определены `@functions` блока (более одного `@functions` блок является допустимым). В `@functions` блока, состояние компонентов (свойства, поля) указывается с помощью методов для обработки событий или для определения других логикой компонента.

Затем можно использовать члены компонента, как отрисовки части компонента приложения логики с помощью C# выражения, которые начинаются с `@`. Например C# поле визуализируется с помощью префикса `@` на имя поля. Следующий пример вычисляет и отображает:

* `_headingFontStyle` значение свойства CSS для `font-style`.
* `_headingText` к содержимому `<h1>` элемент.

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

После изначально визуализируется компонента, компонент повторно создает его дерева визуализации в ответ на события. Компоненты Razor затем сравнивает новое дерево визуализации от предыдущей и применяет все изменения для модели объектов обозревателя документов (DOM).

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>Интегрировать компоненты в Razor Pages и MVC-приложения

Используйте компоненты с существующих приложений Razor Pages и MVC. Нет необходимости переписывать существующие страницы или представления, чтобы использовать компоненты Razor. При отображении страницы или представления, компоненты являются предварительно визуализированных&dagger; в то же время. 

> [!NOTE]
> &dagger;Предварительной визуализации на сервере включен для Razor компонентов приложений по умолчанию. Blazor клиентских приложений будут поддерживать предварительной визуализации в предстоящем выпуске предварительной версии 4. Дополнительные сведения см. в разделе [обновить шаблоны/по промежуточного слоя для использования MapFallbackToPage/файла](https://github.com/aspnet/AspNetCore/issues/8852).

Для подготовки к просмотру компонента из страницы или представления, используйте `RenderComponentAsync<TComponent>` вспомогательный метод HTML:

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

Компоненты к просмотру из страницы и представления еще не интерактивных в выпуске предварительной версии 3. Например выбора кнопки не приводит к возникновению вызов метода. Следующей предварительной версии будет устранить это ограничение и добавить поддержку для подготовки к просмотру компоненты, используя обычный синтаксис элементов и атрибутов.

Хотя страницы и представления можно использовать компоненты, обратное не так. Компоненты нельзя использовать сценарии конкретного представления и страницы, такие как частичные представления и разделах. Для использования логики из частичного представления в компоненте, показателем логику частичного представления в компонент.

## <a name="using-components"></a>Использование компонентов

Компоненты можно добавить и другие компоненты, объявляя их с использованием синтаксиса элемента HTML. Разметка для использования компонента выглядит как тег HTML с именем, соответствующем типу компонента.

Следующая разметка отображает `HeadingComponent` (*HeadingComponent.cshtml*) экземпляр:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a>Параметры компонентов

Компоненты могут иметь *параметры компонента*, которые определены с помощью *закрытые* оформлен свойства класса компонента `[Parameter]`. Используйте атрибуты, чтобы указать аргументы для компонента в разметке.

В следующем примере `ParentComponent` задает значение `Title` свойство `ChildComponent`:

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5)]

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a>Дочерний тип содержимого

Компоненты можно установить содержимое из другого компонента. Назначение компонента предоставляет содержимое между тегами, указывающие получатель. Например `ParentComponent` можно предоставить содержимое для подготовки к просмотру, дочернего компонента, поместив содержимое внутри `<ChildComponent>` теги.

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6-7)]

У компонента дочерних `ChildContent` свойство, которое представляет `RenderFragment`. Значение `ChildContent` располагается в разметку дочерних компонентов, где должны отображаться содержимое. В следующем примере значение `ChildContent` получен из родительского компонента и отображено внутри панели начальной загрузки `panel-body`.

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> Получение свойств `RenderFragment` содержимого должен иметь имя `ChildContent` по соглашению.

## <a name="data-binding"></a>Привязка данных

Привязка данных для компонентов и элементов DOM осуществляется с помощью `bind` атрибута. Следующий пример связывает `ItalicsCheck` свойство флажок проверки состояния:

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck">
```

Если флажок выбран и очищен, значение свойства присваивается `true` и `false`, соответственно.

Флажок в пользовательском Интерфейсе обновляется, только в том случае, когда компонент отображается, не в ответ на изменение значения свойства. Так как компоненты отображаются после выполнения код обработчика событий, обновления свойств обычно отражаются в пользовательском Интерфейсе сразу же.

С помощью `bind` с `CurrentValue` свойство (`<input bind="@CurrentValue">`) является по существу эквивалентно следующему:

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)">
```

При отображении компонента `value` элемента ввода поступают из `CurrentValue` свойство. Когда пользователь вводит в текстовом поле `onchange` события и `CurrentValue` свойству присваивается измененное значение. На самом деле, формирования кода является чуть более сложный, так как `bind` обрабатывает несколько случаев, когда выполняются преобразования типов. В принципе `bind` связывает текущее значение выражения с `value` атрибут и обрабатывает изменения, с помощью зарегистрированного обработчика.

В дополнение к `onchange`, свойство можно привязать с помощью другие события, например `oninput` обеспечивается более подробно о том, что для привязки к:

```cshtml
<input type="text" bind-value-oninput="@CurrentValue">
```

В отличие от `onchange`, `oninput` активируется для каждого символа, вводимых в текстовое поле.

**Строки формата**

Привязка данных работает с <xref:System.DateTime> форматирования строк. В настоящее время недоступны другие выражения формат, например валюта или числовые форматы.

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd">

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

`format-value` Атрибут указывает формат даты для применения к `value` из `input` элемент. Выполнить синтаксический анализ значение также используется формат при `onchange` событием.

**Параметры компонентов**

Привязки также распознает параметры компонента, где `bind-{property}` можно привязать значение свойства по компонентам.

В следующем компоненте используются `ChildComponent` и привязывает `ParentYear` параметр из родительского `Year` параметр в компоненте дочерних:

Родительский компонент:

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

Дочерний компонент:

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

Загрузка `ParentComponent` создает следующую разметку:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Если значение `ParentYear` изменении свойства, нажав кнопку в `ParentComponent`, `Year` свойство `ChildComponent` обновляется. Новое значение `Year` отображается в пользовательском Интерфейсе при `ParentComponent` когда:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

`Year` Параметра возможности привязки, поскольку в нем вспомогательное `YearChanged` событие, которое совпадает с типом `Year` параметра.

По соглашению `<ChildComponent bind-Year="@ParentYear" />` фактически эквивалентна записи,

```cshtml
    <ChildComponent bind-Year-YearChanged="@ParentYear" />
```

В общем случае можно привязать свойство к соответствующий обработчик событий с помощью `bind-property-event` атрибута.

## <a name="event-handling"></a>Обработка событий

Компоненты Razor обеспечивают функции обработки событий. Для атрибута элемент HTML с именем `on<event>` (например, `onclick`, `onsubmit`) со значением, типом делегата, компоненты Razor обрабатывает значение атрибута как обработчик событий. Имя атрибута всегда начинается с `on`.

Следующий код вызывает `UpdateHeading` метод при нажатии кнопки в пользовательском Интерфейсе:

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

Следующий код вызывает `CheckboxChanged` метода при его изменении в пользовательском Интерфейсе:

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged">

@functions {
    private void CheckboxChanged()
    {
        ...
    }
}
```

Обработчики событий также может быть асинхронной и возврата <xref:System.Threading.Tasks.Task>. Нет необходимости вручную вызывать функции `StateHasChanged()`. Исключения записываются при их возникновении.

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

Для некоторых событий допускаются типы аргументов событий, связанных с событием. Если доступ к одному из таких типов событий, не является обязательным, это не обязательно в вызове метода.

Приведен список событий, поддерживаемые аргументы.

* UIEventArgs
* UIChangeEventArgs
* UIKeyboardEventArgs
* UIMouseEventArgs

Можно также использовать лямбда-выражения:

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Часто бывает удобно закрыть через дополнительные значения, такие как при итерации по набору элементов. В следующем примере создается три кнопки, каждый из которых вызывает `UpdateHeading` передача аргумента события (`UIMouseEventArgs`) и ее номер кнопки (`buttonNumber`) при выборе в пользовательском Интерфейсе:

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> Сделать **не** использование переменной цикла (`i`) в `for` цикл непосредственно в лямбда-выражение. В противном случае используется та же переменная, все лямбда-выражения, в результате чего `i`от значения, чтобы быть одинаковым во всех лямбда-выражения. Всегда записать его значение в локальную переменную (`buttonNumber` в предыдущем примере) и затем использовать его.

## <a name="capture-references-to-components"></a>Захват ссылки на компоненты

Ссылки на компонент предоставить образом получить ссылку на экземпляр компонента таким образом, вы сможете выполнять команды для этого экземпляра, такие как `Show` или `Reset`. Чтобы записать ссылку на компонент, добавьте `ref` атрибут дочерний компонент, а затем определите поле с тем же именем и типом как дочерний компонент.

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

При отображении компонента `loginDialog` поле заполняется `MyLoginDialog` дочерний экземпляр компонента. Затем можно вызывать методы .NET экземпляра компонента.

> [!IMPORTANT]
> `loginDialog` Переменной заполняется только после того как компонент отображается, и его выходные данные содержат `MyLoginDialog` элемент. До этого момента нет ничего для ссылки. Используются для операций ссылки на компонентов после завершения подготовки к просмотру компонента `OnAfterRenderAsync` или `OnAfterRender` методы.

Тогда как захват ссылки на компонент использует аналогичный синтаксис для [захват ссылок на элементы](xref:razor-components/javascript-interop#capture-references-to-elements), это не так [взаимодействия JavaScript](xref:razor-components/javascript-interop) функции. Ссылки на компонент не передаются в код JavaScript; они используются только в коде .NET.

> [!NOTE]
> Сделать **не** использовать ссылки на компонент изменяемая состояние дочерних компонентов. Вместо этого используйте обычный декларативные параметры для передачи данных дочерними компонентами. В результате дочерними компонентами для rerender правильность времени автоматически.

## <a name="lifecycle-methods"></a>Методы жизненного цикла

`OnInitAsync` и `OnInit` выполнять код для инициализации компонента. Чтобы выполнить асинхронную операцию, используйте `OnInitAsync` и `await` ключевое слово на операцию:

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

Синхронную операцию, используйте `OnInit`:

```csharp
protected override void OnInit()
{
    ...
}
```

`OnParametersSetAsync` и `OnParametersSet` вызываются, когда компонент получил параметров из его родительского элемента и значения присваиваются свойствам. Эти методы применяются после инициализации компонента и затем каждый раз компонент отображается:

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

`OnAfterRenderAsync` и `OnAfterRender` вызываются после компонент завершил отрисовку. На этом этапе заполняются ссылки на элемент и компонента. Позволяет выполнять дополнительные действия по инициализации с использованием готового для просмотра содержимого, например активация сторонних библиотек JavaScript, которые оперируют визуализированных элементов DOM этого этапа.

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

`SetParameters` можно переопределить, чтобы выполнять код до параметры заданы:

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

Если `base.SetParameters` не вызван, пользовательский код может интерпретировать входящее значение параметров в любом способом требуется. Например параметры входящего не обязательно должны назначаться свойства класса.

`ShouldRender` можно переопределить для подавления обновление пользовательского интерфейса. Если реализация возвращает `true`, обновляется пользовательский Интерфейс. Даже если `ShouldRender` будет переопределено, компонент всегда изначально визуализируется.

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a>Реализация компонента с IDisposable

Если компонент реализует <xref:System.IDisposable>, [метод Dispose](/dotnet/standard/garbage-collection/implementing-dispose) вызывается, когда компонент будет удален из пользовательского интерфейса. В следующем компоненте используются `@implements IDisposable` и `Dispose` метод:

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a>Маршрутизация

Маршрутизация в компонентах Razor достигается с помощью шаблона маршрута для каждого компонента, доступного в приложении.

Когда *.cshtml* файл с `@page` компилируется директивы, созданном классе ему присваивается <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> указания шаблона маршрута. Во время выполнения, маршрутизатор ищет классы компонентов с `RouteAttribute` и отображает, какой компонент имеет шаблон маршрута, соответствующий запрошенного URL-адреса.

Несколько шаблонов маршрута могут применяться к компоненту. Следующий компонент отвечает на запросы для `/BlazorRoute` и `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a>Параметры маршрута

Компоненты могут получать параметры маршрута с шаблоном маршрута в `@page` директива. Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента.

*RouteParameter.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

Необязательные параметры не поддерживаются, так что два `@page` директивы применяются в приведенном выше примере. Первый позволяет навигации к компоненту без параметров. Второй `@page` принимает директивы `{text}` параметра маршрута и присваивает это значение `Text` свойства.

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>Наследование базовый класс для взаимодействия кода «программной»

Файлы компонентов (*.cshtml*) смешивать разметки HTML и C# обработке кода в одном файле. `@inherits` Директива может использоваться для предоставления Razor компонентов приложений с интерфейсом «кода», разделяющий разметки компонента из кода обработки.

[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) показано, как компонент может наследовать базовому классу, `BlazorRocksBase`, чтобы предоставлять методы и свойства компонента.

*BlazorRocks.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

*BlazorRocksBase.cs*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

Базовый класс должен быть производным от `ComponentBase`.

## <a name="razor-support"></a>Поддержка Razor

**Директивы Razor**

В следующей таблице показаны директивы Razor.

| Директива | Описание |
| --------- | ----------- |
| [\@Функции](xref:mvc/views/razor#section-5) | Добавляет C# блок кода в компонент. |
| `@implements` | Реализует интерфейс для класса созданный компонент. |
| [\@Наследует](xref:mvc/views/razor#section-3) | Предоставляет полный контроль над класс, который наследует компонента. |
| [\@внедрить](xref:mvc/views/razor#section-4) | Позволяет услуг внедрения из [контейнер службы](xref:fundamentals/dependency-injection). Дополнительные сведения: [Внедрение зависимостей в представления](xref:mvc/views/dependency-injection). |
| `@layout` | Указывает компонента макета. Компоненты макета используются во избежание дублирования и несогласованности. |
| [\@Страница](xref:razor-pages/index#razor-pages) | Указывает, что компонент должен обрабатывать запросы напрямую. `@page` Директивы можно указать параметром маршрута и необязательные параметры. В отличие от Razor Pages `@page` директива не обязательно должен быть первой директивой в верхней части файла. Дополнительные сведения см. в разделе [Маршрутизация](xref:razor-components/routing). |
| [\@С помощью](xref:mvc/views/razor#using) | Добавляет C# `using` директивы в класс созданный компонент. |
| [\@addTagHelper](xref:mvc/views/razor#tag-helpers) | Использовать `@addTagHelper` для использования компонента в другой сборке, чем сборка приложения. |

**Условные атрибуты**

Атрибуты отображаются по условию на основе значения .NET. Если значение равно `false` или `null`, атрибут не отображается. Если значение равно `true`, атрибут визуализируется в свернутом состоянии.

В следующем примере `IsCompleted` определяет `checked` визуализируется в разметке элемента управления:

```cshtml
<input type="checkbox" checked="@IsCompleted">

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

Если `IsCompleted` является `true`, флажок отображается как:

```html
<input type="checkbox" checked>
```

Если `IsCompleted` является `false`, флажок отображается как:

```html
<input type="checkbox">
```

**Дополнительные сведения о Razor**

Дополнительные сведения о Razor, см. в разделе [Справочник по синтаксису Razor](xref:mvc/views/razor).

## <a name="raw-html"></a>Необработанный HTML

Строки обычно отображаются с использованием DOM текстовые узлы, что означает, что любой разметку, с которой они могут содержать игнорируется и обрабатывается как обычный текст. Чтобы отобразить необработанный HTML, поместить содержимое HTML в `MarkupString` значение. Значение анализируется как HTML или SVG и вставить в модель DOM.

> [!WARNING]
> Визуализации необработанный HTML, созданный из любой ненадежных источником является **угрозу безопасности** следует избегать использования!

В следующем примере показано использование `MarkupString` тип добавляемого блока статического содержимого HTML выводимые данные компонента:

```html
@((MarkupString)myMarkup)

@functions {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Компоненты на основе шаблона

Шаблонный компонентами являются компоненты, которые принимают один или несколько шаблонов пользовательского интерфейса в качестве параметров, которые затем могут использоваться как часть логики отображения компонента. Шаблонный компоненты позволяют создавать компоненты более высокого уровня, более многократно используемых, чем обычных компонентов. Включают несколько примеров:

* Таблица компонент, который позволяет пользователю указать шаблоны для заголовка, строки и нижнего колонтитула таблицы.
* Компонент списка, который позволяет пользователю указать шаблон для подготовки к просмотру элементов в списке.

### <a name="template-parameters"></a>Параметры шаблона

Компонент шаблона определяется путем указания одного или нескольких параметров компонента типа `RenderFragment` или `RenderFragment<T>`. Отрисовки фрагмент представляет часть пользовательского интерфейса, который отображается в компоненте. Фрагмент визуализации при необходимости принимает параметр, который можно указать при вызове фрагмента отрисовки.

*Components/TableTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

При использовании компонента, шаблон, параметры шаблона можно задать с помощью дочерних элементов, совпадающих с именами параметров (`TableHeader` и `RowTemplate` в следующем примере):

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a>Контекстные параметры шаблона

Компонент аргументы типа `RenderFragment<T>` передаваемое в качестве элементов имеют неявный параметр с именем `context` (например из в предыдущем образце кода `@context.PetId`), но можно изменить имя параметра с помощью `Context` атрибут дочерних элементов элемент. В следующем примере `RowTemplate` элемента `Context` атрибут задает `pet` параметр:

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

Кроме того, можно указать `Context` атрибута элемента компонента. Указанный `Context` атрибут применяется для всех остальных параметров указанного шаблона. Это может быть полезно, когда требуется указать имя параметра содержимого для неявного дочерний тип содержимого (без переносов любой дочерний элемент). В следующем примере `Context` атрибут находится в `TableTemplate` элемент и применяется для всех параметров шаблона:

```cshtml
<TableTemplate Items="@pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a>Компоненты типизированными универсальными

Шаблонный компоненты часто универсально типизированы. Например, можно использовать общий компонент шаблон представления списка для подготовки к просмотру `IEnumerable<T>` значения. Чтобы определить общий компонент, используйте `@typeparam` директиву, чтобы задать параметры типа.

*Components/ListViewTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml)]

При использовании компонентов типизированными универсальными, параметр типа выводится, если это возможно:

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

В противном случае параметр типа необходимо явно указать с помощью атрибута, который совпадает с именем параметра типа. В следующем примере `TItem="Pet"` указывает тип:

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>Каскадные значения и параметры

В некоторых сценариях, нет смысла для вывода данных из компонента предка в дочерних компонентов с помощью [параметры компонента](#component-parameters), особенно в том случае, если существуют несколько уровней компонента. Каскадные значения и параметры решить эту проблему, предоставляя удобный способ для предка компонента для предоставления значения для всех его дочерних компонентов. Каскадные значения и параметры также предоставляют способ компоненты для координации.

### <a name="theme-example"></a>Пример темы

В следующем *темы* пример из примера приложения, `ThemeInfo` класс задает информацию темы к нижележащим сайтам иерархии компонент таким образом, все кнопки в определенной части приложения тот же стиль.

*UIThemeClasses/ThemeInfo.cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Компонент предка можно указать каскадные значение с помощью каскадных значение компонента. Компонент каскадных значения ветви иерархии компонент заключает в оболочку и предоставляет одно значение для всех компонентов в этом поддереве.

Например, в примере приложения задает сведения о теме (`ThemeInfo`) в одном из макетов приложений как каскадного параметра для всех компонентов, составляющих тело макета `@Body` свойство. `ButtonClass` присваивается значение `btn-success` в компоненте макета. Любой потомок компонент может использовать это свойство через `ThemeInfo` каскадных объекта.

*Shared/CascadingValuesParametersLayout.cshtml*:

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@functions {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

Чтобы использовать каскадных значений, компоненты объявить каскадных параметров с помощью `[CascadingParameter]` атрибута или в зависимости от строковое значение имени:

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

Привязки со строковым значением имя относится, если у вас несколько каскадных значений одного типа и необходимо разделить их в данном поддереве.

Каскадные значения привязаны к каскадные параметры по типу.

В примере приложения каскадные параметры темы значения компонента привязывается к `ThemeInfo` каскадных значение для каскадного параметра. Параметр используется для задания класс CSS для одной из кнопок, отображаемому в компоненте.

*Pages/CascadingValuesParametersTheme.cshtml*:

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    private int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a>Пример TabSet

Каскадные параметры также включить компоненты для совместной работы по всей иерархии компонента. Например, рассмотрим следующие *TabSet* пример, в примере приложения.

В примере приложения есть `ITab` интерфейса, реализуйте вкладки:

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

Каскадные параметры TabSet значения компонент использует компонент набор вкладок, который содержит несколько компонентов вкладки:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

Вкладка дочерние компоненты не явно передаются как параметры в набор вкладок. Вместо этого дочерние вкладку компоненты являются частью дочернего содержимого набор вкладок. Тем не менее набор вкладок по-прежнему необходимо знать о каждом компоненте вкладку, чтобы он мог преобразовать заголовки и активной вкладки. Чтобы включить эту координацию без необходимости написания дополнительного кода, компонент набор вкладок *можно указать сам как значение каскадных* , затем выбирается дочерних компонентов вкладки.

*Components/TabSet.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

Дочерние записи компонентов вкладку содержащего набор вкладок как каскадного параметра, поэтому компоненты вкладку добавлять себя в набор вкладок и координат на вкладках активна.

*Components/Tab.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a>Шаблоны Razor

Визуализации фрагментов можно определить с помощью синтаксиса Razor шаблона. Шаблоны Razor — это способ определения пользовательского интерфейса фрагмента и предполагают следующий формат:

```cshtml
@<tag>...</tag>
```

В следующем примере показан способ указания `RenderFragment` и `RenderFragment<T>` значения.

*RazorTemplates.cshtml*:

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

Отображать фрагментов, определенных с помощью Razor, шаблоны можно передаются как аргументы шаблона компонентов или отображена непосредственно. Например предыдущий шаблоны непосредственно выводятся в виде следующая разметка Razor:

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

Отображенные выходные данные:

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a>Вручную RenderTreeBuilder логики

`Microsoft.AspNetCore.Components.RenderTree` содержит методы для работы компонентов и элементов, в том числе по созданию компонентов вручную в C# кода.

> [!NOTE]
> Использование `RenderTreeBuilder` для создания компонентов — это расширенный сценарий. Компонент имеет неправильный формат (например, тег незакрытые разметки) может привести к неопределенному поведению.

Рассмотрим следующий компонент Pet сведения (*PetDetails.razor* в Razor компонентов; *PetDetails.cshtml* в Blazor), который может быть вручную встроен в другой компонент:

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

В следующем примере цикла в `CreateComponent` метод создает три компонента Pet сведения. При вызове `RenderTreeBuilder` методы для создания компонентов (`OpenComponent` и `AddAttribute`), порядковые номера, номера строк исходного кода. Razor компоненты, которые алгоритм различие зависит от различных строкам кода, а не отдельными порядковые номера вызовите вызовов. При создании компонента с `RenderTreeBuilder` методы, жестко задавать аргументы для последовательности чисел. **Использование вычисления или счетчиков для создания порядковый номер может привести к снижению производительности.**

Построение компонента (*BuiltContent.razor* в Razor компонентов; *BuiltContent.cshtml* в Blazor):

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" onclick="@RenderComponent">
    Create three Pet Details components
</button>

@functions {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```
