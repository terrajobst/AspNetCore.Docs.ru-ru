---
title: Создание и использование компонентов ASP.NET Core Razor
author: guardrex
description: Узнайте, как создавать и использовать компоненты Razor, включая способы привязки к данным, обработки событий и управления жизненным циклом компонентов.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: blazor/components
ms.openlocfilehash: 3e0966bf978c99fc00db7682bea3292306cbb03c
ms.sourcegitcommit: d81912782a8b0bd164f30a516ad80f8defb5d020
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179028"
---
# <a name="create-and-use-aspnet-core-razor-components"></a>Создание и использование компонентов ASP.NET Core Razor

[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

Приложения блазор создаются с помощью *компонентов*. Компонент — это автономный фрагмент пользовательского интерфейса, например страница, диалоговое окно или форма. Компонент включает разметку HTML и логику обработки, необходимую для вставки данных или реагирования на события пользовательского интерфейса. Компоненты являются гибкими и легковесными. Они могут быть вложенными, повторно использованы и совместно использоваться проектами.

## <a name="component-classes"></a>Классы компонентов

Компоненты реализуются в файлах компонентов [Razor](xref:mvc/views/razor) ( *. Razor*) с помощью комбинации C# и HTML-разметки. Компонент в Блазор формально называется *компонентом Razor*.

Имя компонента должно начинаться с символа верхнего регистра. Например, *микулкомпонент. Razor* является допустимым, а *микулкомпонент. Razor* является недопустимым.

Пользовательский интерфейс для компонента определяется с помощью HTML. Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor). При компиляции приложения логика разметки и C# отрисовки HTML преобразуется в класс компонента. Имя созданного класса соответствует имени файла.

Элементы класса компонента определяются в блоке `@code`. В блоке `@code` состояние компонента (свойства, поля) указывается с помощью методов обработки событий или для определения другой логики компонента. Допускается более одного блока `@code`.

> [!NOTE]
> В предыдущих предварительных версиях ASP.NET Core 3,0 блоки `@functions` использовались для той же цели, что и блоки `@code` в компонентах Razor. блоки `@functions` продолжают работать в компонентах Razor, но рекомендуется использовать блок `@code` в ASP.NET Core 3,0 Preview 6 или более поздней версии.

Члены компонента можно использовать как часть логики визуализации компонента с помощью C# выражений, начинающихся с `@`. Например, C# поле отображается путем добавления префикса `@` к имени поля. В следующем примере вычисляется и выводится следующее:

* `_headingFontStyle` в значение свойства CSS для `font-style`.
* `_headingText` на содержимое элемента `<h1>`.

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

После первоначального отображения компонента Компонент повторно создает дерево рендеринга в ответ на события. Затем блазор сравнивает новое дерево отрисовки с предыдущим и применяет любые изменения к модель DOMу (DOM) браузера.

Компоненты являются обычными C# классами и могут быть помещены в любое место внутри проекта. Компоненты, создающие веб-страницы, обычно находятся в папке *страниц* . Компоненты, не являющиеся страницами, часто помещаются в *общую* папку или пользовательскую папку, добавленную в проект. Чтобы использовать пользовательскую папку, добавьте пространство имен пользовательской папки либо в родительский компонент, либо в файл *_Imports. Razor* приложения. Например, следующее пространство имен делает компоненты в папке *Components* доступной, когда корневое пространство имен приложения имеет значение `WebApplication`:

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>Интеграция компонентов в Razor Pages и приложения MVC

Используйте компоненты с существующими Razor Pages и приложениями MVC. Нет необходимости переписывать существующие страницы или представления для использования компонентов Razor. При отображении страницы или представления компоненты предварительно отображаются.

Чтобы отобразить компонент из страницы или представления, используйте вспомогательный метод HTML `RenderComponentAsync<TComponent>`:

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

Хотя страницы и представления могут использовать компоненты, наоборот это не так. Компоненты не могут использовать сценарии представления и страницы, такие как частичные представления и разделы. Чтобы использовать логику из частичного представления в компоненте, разнесите логику частичного представления в компонент.

Дополнительные сведения о подготовке компонентов к просмотру и управлении состоянием компонентов в Блазор Server Apps см. в статье <xref:blazor/hosting-models>.

## <a name="use-components"></a>Использование компонентов

Компоненты могут включать другие компоненты, объявляя их с помощью синтаксиса HTML-элементов. Разметка для использования компонента выглядит как тег HTML с именем, соответствующем типу компонента.

Привязка атрибута чувствительна к регистру. Например, `@bind` является допустимым, а `@Bind` является недопустимым.

Следующая разметка в *index. Razor* визуализирует экземпляр `HeadingComponent`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

*Components/хеадингкомпонент. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

Если компонент содержит HTML-элемент с первой буквой в верхнем регистре, который не соответствует имени компонента, выдается предупреждение, указывающее, что элемент имеет непредвиденное имя. Добавление оператора `@using` для пространства имен компонента делает компонент доступным, что приводит к удалению предупреждения.

## <a name="component-parameters"></a>Параметры компонентов

Компоненты могут иметь *Параметры компонента*, которые определяются с помощью общедоступных свойств класса Component с атрибутом `[Parameter]`. Используйте атрибуты, чтобы указать аргументы для компонента в разметке.

*Components/чилдкомпонент. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

В следующем примере `ParentComponent` задает значение свойства `Title` для `ChildComponent`.

*Pages/паренткомпонент. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a>Дочернее содержимое

Компоненты могут устанавливать содержимое другого компонента. Назначение компонента предоставляет содержимое между тегами, задающих принимающий компонент.

В следующем примере `ChildComponent` имеет свойство `ChildContent`, представляющее `RenderFragment`, которое представляет сегмент пользовательского интерфейса для отрисовки. Значение `ChildContent` размещается в разметке компонента, где должно быть визуализировано содержимое. Значение `ChildContent` получено от родительского компонента и отображается в `panel-body` панели начальной загрузки.

*Components/чилдкомпонент. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> Свойство, получающее содержимое `RenderFragment`, должно иметь имя `ChildContent` по соглашению.

Следующий `ParentComponent` может предоставить содержимое для отрисовки `ChildComponent`, поместив содержимое в теги `<ChildComponent>`.

*Pages/паренткомпонент. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a>Атрибуты Сплаттинг и произвольные параметры

Компоненты могут записывать и отображать дополнительные атрибуты в дополнение к объявленным параметрам компонента. Дополнительные атрибуты могут быть записаны в словарь, а затем *сплаттед* на элемент при подготовке компонента к просмотру с помощью директивы Razor [@attributes](xref:mvc/views/razor#attributes) . Этот сценарий полезен при определении компонента, который создает элемент разметки, поддерживающий разнообразные настройки. Например, может быть утомительно определять атрибуты отдельно для `<input>`, который поддерживает много параметров.

В следующем примере первый элемент `<input>` (`id="useIndividualParams"`) использует параметры отдельных компонентов, а второй элемент `<input>` (`id="useAttributesDict"`) использует атрибут Сплаттинг:

```cshtml
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

Тип параметра должен реализовывать `IEnumerable<KeyValuePair<string, object>>` со строковыми ключами. Использование `IReadOnlyDictionary<string, object>` также является параметром в этом сценарии.

Отображаемые элементы `<input>`, использующие оба подхода, идентичны:

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

Чтобы принять произвольные атрибуты, определите параметр компонента с помощью атрибута `[Parameter]` со свойством `CaptureUnmatchedValues`, установленным в значение `true`:

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

Свойство `CaptureUnmatchedValues` в `[Parameter]` позволяет параметру сопоставлять все атрибуты, не соответствующие какому-либо другому параметру. Компонент может определять только один параметр с `CaptureUnmatchedValues`. Тип свойства, используемый с `CaptureUnmatchedValues`, должен быть назначен из `Dictionary<string, object>` со строковыми ключами. в этом сценарии также используются параметры `IEnumerable<KeyValuePair<string, object>>` или `IReadOnlyDictionary<string, object>`.

## <a name="data-binding"></a>Привязка данных

Привязка данных как к компонентам, так и к элементам DOM осуществляется с помощью атрибута [@bind](xref:mvc/views/razor#bind) . В следующем примере свойство `CurrentValue` привязывается к значению текстового поля:

```cshtml
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

Когда текстовое поле теряет фокус, значение свойства обновляется.

Текстовое поле обновляется в пользовательском интерфейсе только при подготовке компонента к просмотру, а не в ответ на изменение значения свойства. Поскольку компоненты отображаются после выполнения кода обработчика событий, обновления свойств *обычно* отражаются в пользовательском интерфейсе сразу после запуска обработчика событий.

Использование `@bind` со свойством `CurrentValue` (`<input @bind="CurrentValue" />`) в основном эквивалентно следующему:

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

При подготовке к просмотру компонента `value` элемента input берется из свойства `CurrentValue`. Когда пользователь вводит текст в текстовое поле и изменяет фокус элемента, возникает событие `onchange`, а свойству `CurrentValue` присваивается измененное значение. На практике создание кода более сложно, так как `@bind` обрабатывает случаи, когда выполняются преобразования типов. В принципе, `@bind` связывает текущее значение выражения с атрибутом `value` и обрабатывает изменения с помощью зарегистрированного обработчика.

Помимо обработки событий `onchange` с синтаксисом `@bind`, свойство или поле можно привязать с помощью других событий, указав атрибут [@bind-value](xref:mvc/views/razor#bind) с параметром `event` ([@bind-value:event](xref:mvc/views/razor#bind)). В следующем примере привязывается свойство `CurrentValue` для события `oninput`:

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

В отличие от `onchange`, которое срабатывает при потере фокуса элементом, `oninput` срабатывает при изменении значения текстового поля.

**Неанализируемые значения**

Когда пользователь предоставляет неинтерпретируемое значение для элемента с привязкой к данным, неанализируемое значение автоматически возвращается к предыдущему значению при срабатывании события привязки.

Рассмотрим следующий сценарий.

* Элемент `<input>` привязан к типу `int` с начальным значением `123`:

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* Пользователь обновляет значение элемента на `123.45` на странице и изменяет фокус элемента.

В приведенном выше сценарии значение элемента возвращается к `123`. Если значение `123.45` отклоняется в пользу исходного значения `123`, пользователь понимает, что их значение не принято.

По умолчанию привязка применяется к событию `onchange` элемента (`@bind="{PROPERTY OR FIELD}"`). Чтобы задать другое событие, используйте `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}`. Для события `oninput` (`@bind-value:event="oninput"`) переверсия происходит после любого нажатия клавиши, которая вводит неинтерпретируемое значение. При нацеливании на событие `oninput` с типом, связанным с @no__t -1, пользователю запрещено вводить символ `.`. Символ `.` немедленно удаляется, поэтому пользователь получает немедленный отзыв о том, что разрешены только целые числа. Существуют сценарии, в которых возвращение значения события `oninput` не является идеальным решением, например, когда пользователю нужно удалить неинтерпретируемое значение `<input>`. К альтернативам относятся:

* Не используйте событие `oninput`. Используйте событие `onchange` по умолчанию (`@bind="{PROPERTY OR FIELD}"`), где недопустимое значение не возвращается, пока элемент не теряет фокус.
* Выполните привязку к типу, допускающему значение null, например `int?` или `string`, и предоставьте настраиваемую логику для обработки недопустимых записей.
* Используйте [компонент проверки формы](xref:blazor/forms-validation), например `InputNumber` или `InputDate`. Компоненты проверки форм имеют встроенную поддержку для управления недопустимыми входными данными. Компоненты проверки формы:
  * Позволяет пользователю указать недопустимые входные данные и получить ошибки проверки для связанного `EditContext`.
  * Отображает ошибки проверки в пользовательском интерфейсе, не мешая пользователю вводить дополнительные данные из формы.

**Глобализация**

значения `@bind` форматируются для вывода и анализируются с использованием правил текущего языка и региональных параметров.

Доступ к текущему языку и региональным параметрам можно получить из свойства <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.

[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) используется для следующих типов полей (`<input type="{TYPE}" />`):

* `date`
* `number`

Предыдущие типы полей:

* Отображаются с использованием соответствующих правил форматирования на основе браузера.
* Не может содержать текст в свободной форме.
* Предоставление характеристик взаимодействия с пользователем в зависимости от реализации браузера.

Следующие типы полей имеют особые требования к форматированию, которые в настоящее время не поддерживаются Блазор, так как они не поддерживаются всеми основными браузерами.

* `datetime-local`
* `month`
* `week`

`@bind` поддерживает параметр `@bind:culture`, чтобы предоставить <xref:System.Globalization.CultureInfo?displayProperty=fullName> для синтаксического анализа и форматирования значения. Указание языка и региональных параметров не рекомендуется при использовании типов полей `date` и `number`. `date` и `number` имеют встроенную поддержку Блазор, которая предоставляет требуемый язык и региональные параметры.

Сведения о том, как задать язык и региональные параметры пользователя, см. в разделе [локализация](#localization).

**Строки формата**

Привязка данных работает со строками формата <xref:System.DateTime> с помощью [@bind:format](xref:mvc/views/razor#bind). Другие выражения форматирования, такие как денежные или числовые форматы, в настоящее время недоступны.

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

В приведенном выше коде тип поля элемента `<input>` (`type`) по умолчанию имеет значение `text`. `@bind:format` поддерживается для привязки следующих типов .NET:

* <xref:System.DateTime?displayProperty=fullName>
* <xref:System.DateTime?displayProperty=fullName>?
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <xref:System.DateTimeOffset?displayProperty=fullName>?

Атрибут `@bind:format` задает формат даты, применяемый к `value` элемента `<input>`. Формат также используется для анализа значения при возникновении события `onchange`.

Указание формата для типа поля `date` не рекомендуется, так как Блазор имеет встроенную поддержку для форматирования дат.

**Параметры компонента**

Привязка распознает параметры компонента, где `@bind-{property}` может привязать значение свойства к нескольким компонентам.

Следующий дочерний компонент (`ChildComponent`) имеет параметр компонента `Year` и обратный вызов `YearChanged`:

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

`EventCallback<T>` объясняется в разделе [вложенный EventCallback](#eventcallback) .

Следующий родительский компонент использует `ChildComponent` и привязывает параметр `ParentYear` от родительского к параметру `Year` в дочернем компоненте:

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

При загрузке `ParentComponent` создается следующая разметка:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Если значение свойства `ParentYear` было изменено нажатием кнопки в `ParentComponent`, то обновляется свойство `Year` объекта `ChildComponent`. Новое значение `Year` отображается в пользовательском интерфейсе при повторной отрисовке `ParentComponent`:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

Параметр `Year` доступен для привязки, так как имеет сопутствующее событие `YearChanged`, соответствующее типу параметра `Year`.

По соглашению `<ChildComponent @bind-Year="ParentYear" />` по сути является аналогом записи:

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

Как правило, свойство может быть привязано к соответствующему обработчику событий с помощью атрибута `@bind-property:event`. Например, свойство `MyProp` можно привязать к `MyEventHandler` с помощью следующих двух атрибутов:

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a>Обработка событий

Компоненты Razor предоставляют функции обработки событий. Для атрибута элемента HTML с именем `on{event}` (например, `onclick` и `onsubmit`) со значением, определяемым делегатом, компоненты Razor рассматривают значение атрибута как обработчик событий. Имя атрибута всегда отформатировано [@on {Event}](xref:mvc/views/razor#onevent).

Следующий код вызывает метод `UpdateHeading`, когда в пользовательском интерфейсе выбрана кнопка:

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

Следующий код вызывает метод `CheckChanged`, когда флажок изменяется в пользовательском интерфейсе:

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

Обработчики событий также могут быть асинхронными и возвращать <xref:System.Threading.Tasks.Task>. Нет необходимости вручную вызывать `StateHasChanged()`. Исключения регистрируются при их возникновении.

В следующем примере `UpdateHeading` вызывается асинхронно при выборе этой кнопки:

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a>Типы аргументов событий

Для некоторых событий разрешены типы аргументов событий. Если доступ к одному из этих типов событий не нужен, в вызове метода это не требуется.

Поддерживаемые `EventArgs` приведены в следующей таблице.

| событие | Класс |
| ----- | ----- |
| буфер обмена        | `ClipboardEventArgs` |
| Переместить             | `DragEventArgs` &ndash; `DataTransfer` и `DataTransferItem` содержат перетаскиваемые данные элемента. |
| Ошибка            | `ErrorEventArgs` |
| Фокус            | `FocusEventArgs` &ndash; не включает поддержку `relatedTarget`. |
| Изменение`<input>` | `ChangeEventArgs` |
| Клавиатура         | `KeyboardEventArgs` |
| Мышь            | `MouseEventArgs` |
| Указатель мыши    | `PointerEventArgs` |
| Колесо мыши      | `WheelEventArgs` |
| Ход выполнения         | `ProgressEventArgs` |
| Сенсорные технологии            | `TouchEventArgs` &ndash; `TouchPoint` представляет одну точку контакта на сенсорном устройстве. |

Сведения о свойствах и поведении событий, описанных в приведенной выше таблице, см. в разделе [классы EventArgs в источнике ссылки (ветвь ASPNET/AspNetCore Release/3.0)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).

### <a name="lambda-expressions"></a>Лямбда-выражения

Лямбда-выражения также могут использоваться:

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Часто бывает удобно закрывать дополнительные значения, например при переборе набора элементов. В следующем примере создаются три кнопки, каждый из которых вызывает `UpdateHeading`, передавая аргумент события (`MouseEventArgs`) и номер кнопки (`buttonNumber`) при выборе в пользовательском интерфейсе:

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> **Не** используйте переменную цикла (`i`) в цикле `for` непосредственно в лямбда-выражении. В противном случае одна и та же переменная используется всеми лямбда-выражениями, в результате чего значение `i` будет одинаковым для всех лямбда-выражений. Всегда запишите свое значение в локальную переменную (`buttonNumber` в предыдущем примере), а затем используйте ее.

### <a name="eventcallback"></a>Вложенный EventCallback

Распространенным сценарием с вложенными компонентами является желание запускать метод родительского компонента при возникновении события дочернего компонента @ no__t-0for, когда в дочернем элементе возникает событие `onclick`. Чтобы обеспечить доступ к событиям по компонентам, используйте `EventCallback`. Родительский компонент может назначить метод обратного вызова `EventCallback` дочернего компонента.

@No__t-0 в примере приложения демонстрирует настройку обработчика `onclick` кнопки на получение делегата `EventCallback` из примера `ParentComponent`. @No__t-0 вводится `MouseEventArgs`, что подходит для события `onclick` на периферийном устройстве:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

@No__t-0 устанавливает `EventCallback<T>` дочернего элемента в метод `ShowMessage`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

При выборе кнопки в `ChildComponent`:

* Вызывается метод `ParentComponent` `ShowMessage`. `messageText` обновляется и отображается в `ParentComponent`.
* Вызов `StateHasChanged` не требуется в методе обратного вызова (`ShowMessage`). `StateHasChanged` вызывается автоматически для повторной визуализации `ParentComponent`, как и дочерние события. компонент активируется в обработчиках событий, которые выполняются внутри дочернего элемента.

`EventCallback` и `EventCallback<T>` допускают использование асинхронных делегатов. `EventCallback<T>` является строго типизированным и требует определенного типа аргумента. `EventCallback` слабо типизирован и допускает любой тип аргумента.

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

Вызовите `EventCallback` или `EventCallback<T>` с `InvokeAsync` и ожидайте <xref:System.Threading.Tasks.Task>:

```csharp
await callback.InvokeAsync(arg);
```

Используйте `EventCallback` и `EventCallback<T>` для обработки событий и параметров компонента привязки.

Предпочитать строго типизированный `EventCallback<T>` на `EventCallback`. `EventCallback<T>` обеспечивает лучшую реакцию на ошибки для пользователей компонента. Как и в случае с другими обработчиками событий пользовательского интерфейса, указание параметра события является необязательным. Используйте `EventCallback`, если не передается значение обратного вызова.

## <a name="chained-bind"></a>Привязка к цепочке

Распространенным сценарием является привязка привязанного к данным параметра к элементу страницы в выходных данных компонента. Этот сценарий называется *связанной* привязкой, так как несколько уровней привязки происходят одновременно.

Связанную с цепочкой привязку нельзя реализовать с помощью синтаксиса `@bind` в элементе страницы. Обработчик событий и значение должны быть указаны отдельно. Однако родительский компонент может использовать синтаксис `@bind` с параметром компонента.

Следующий компонент `PasswordField` (*пассвордфиелд. Razor*):

* Задает значение элемента `<input>` для свойства `Password`.
* Предоставляет изменения свойства `Password` в родительский компонент с помощью [вложенный EventCallback](#eventcallback).

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

Компонент `PasswordField` используется в другом компоненте:

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

Для выполнения проверок или перехвата ошибок в пароле в предыдущем примере:

* Создайте резервное поле для `Password` (`password` в следующем примере кода).
* Выполните проверки или ошибки ловушек в методе задания `Password`.

В следующем примере представлена немедленная реакция пользователя, если в значении пароля используется пробел:

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

## <a name="capture-references-to-components"></a>Запись ссылок на компоненты

Ссылки на компоненты предоставляют способ ссылки на экземпляр компонента, чтобы можно было выполнять команды для этого экземпляра, например `Show` или `Reset`. Чтобы записать ссылку на компонент, сделайте следующее:

* Добавьте атрибут [@ref](xref:mvc/views/razor#ref) к дочернему компоненту.
* Определите поле с тем же типом, что и у дочернего компонента.

```cshtml
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

При подготовке компонента к просмотру поле `loginDialog` заполняется экземпляром дочернего компонента `MyLoginDialog`. Затем можно вызывать методы .NET в экземпляре компонента.

> [!IMPORTANT]
> Переменная `loginDialog` заполняется только после подготовки компонента, а ее выходные данные включают элемент `MyLoginDialog`. До этого момента нет никаких ссылок на. Чтобы управлять ссылками на компоненты после завершения подготовки компонента к просмотру, используйте методы `OnAfterRenderAsync` или `OnAfterRender`.

При захвате ссылок на компоненты используется аналогичный синтаксис для [записи ссылок на элементы](xref:blazor/javascript-interop#capture-references-to-elements), но это не функция [взаимодействия JavaScript](xref:blazor/javascript-interop) . Ссылки на компоненты не передаются в код JavaScript @ no__t-0they're используются только в коде .NET.

> [!NOTE]
> **Не** используйте ссылки на компоненты для изменения состояния дочерних компонентов. Вместо этого используйте обычные декларативные параметры для передачи данных дочерним компонентам. Использование обычных декларативных параметров приводит к тому, что дочерние компоненты автоматически визуализируются в нужное время.

## <a name="invoke-component-methods-externally-to-update-state"></a>Вызывать методы компонента извне для обновления состояния

Блазор использует `SynchronizationContext` для принудительного применения одного логического потока выполнения. Методы жизненного цикла компонента и любые обратные вызовы событий, вызванные Блазор, выполняются на этом `SynchronizationContext`. В случае, если компонент должен быть обновлен на основе внешнего события, такого как таймер или другие уведомления, используйте метод `InvokeAsync`, который будет отправляться в Блазор `SynchronizationContext`.

Например, рассмотрим *службу уведомления* , которая может уведомлять любой компонент, выполняющий прослушивание, в обновленном состоянии:

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

Использование `NotifierService` для обновления компонента:

```cshtml
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

В предыдущем примере `NotifierService` вызывает метод `OnNotify` компонента за пределами Блазор `SynchronizationContext`. `InvokeAsync` используется для переключения на правильный контекст и постановку в очередь визуализации.

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a>Использование \@key для управления сохранением элементов и компонентов

При последующем изменении списка элементов или компонентов, а также элементов или компонентов алгоритм сравнения Блазор должен решить, какие из предыдущих элементов или компонентов могут быть сохранены и как объекты модели должны сопоставляться с ними. Обычно этот процесс выполняется автоматически и его можно игнорировать, но в некоторых случаях может потребоваться управление процессом.

Рассмотрим следующий пример.

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

Содержимое коллекции `People` может изменяться при вставке, удалении или повторном упорядочении записей. Когда компонент перерисовывается, компонент `<DetailsEditor>` может измениться на получение различных значений параметров `Details`. Это может привести к более сложной визуализации, чем ожидалось. В некоторых случаях перерисовка может привести к появлению видимых различий в поведении, таких как потеря фокуса элементов.

Процесс сопоставления можно контролировать с помощью атрибута директивы `@key`. `@key` приводит к тому, что алгоритм сравнения гарантирует сохранение элементов или компонентов на основе значения ключа:

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

При изменении коллекции `People` алгоритм сравнения удерживает связь между экземплярами `<DetailsEditor>` и экземплярами `person`:

* Если `Person` удаляется из списка `People`, из пользовательского интерфейса удаляется только соответствующий экземпляр `<DetailsEditor>`. Другие экземпляры остаются без изменений.
* Если в некоторой позиции в списке вставляется `Person`, то в соответствующей позиции вставляется один новый экземпляр `<DetailsEditor>`. Другие экземпляры остаются без изменений.
* При повторном упорядочении записей `Person` соответствующие экземпляры `<DetailsEditor>` сохраняются и повторно упорядочиваются в пользовательском интерфейсе.

В некоторых сценариях использование `@key` уменьшает сложность повторного отображения и позволяет избежать потенциальных проблем с элементами, изменяющимися с отслеживанием состояния DOM, например положением фокуса.

> [!IMPORTANT]
> Ключи являются локальными для каждого элемента или компонента контейнера. Ключи не сравниваются глобально по всему документу.

### <a name="when-to-use-key"></a>Когда следует использовать @no__t 0key

Как правило, имеет смысл использовать `@key` при подготовке списка (например, в блоке `@foreach`), а подходящее значение — для определения `@key`.

Можно также использовать `@key`, чтобы предотвратить сохранение Блазор элемента или поддерева компонента при изменении объекта.

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

Если `@currentPerson` меняется, директива атрибута `@key` заставляет Блазор отбросить весь `<div>` и его потомков и перестроить поддерево в пользовательском интерфейсе с помощью новых элементов и компонентов. Это может быть полезно, если необходимо гарантировать, что при изменении `@currentPerson` состояние пользовательского интерфейса не сохраняется.

### <a name="when-not-to-use-key"></a>Когда не следует использовать \@key

При использовании сравнения с `@key` возникают затраты на производительность. Затраты на производительность не велики, но при управлении правилами сохранения элементов или компонентов вместо них следует указывать только `@key`.

Даже если `@key` не используется, Блазор сохраняет дочерние элементы и экземпляры компонентов насколько это возможно. Единственным преимуществом использования `@key` является управление *тем, как* экземпляры модели сопоставляются с сохраненными экземплярами компонента, а не алгоритмом сравнения, который выбирает сопоставление.

### <a name="what-values-to-use-for-key"></a>Какие значения следует использовать для @no__t 0key

Как правило, имеет смысл указать один из следующих видов значений для `@key`:

* Экземпляры объектов Model (например, экземпляр `Person`, как в предыдущем примере). Это гарантирует сохранение на основе равенства ссылок на объекты.
* Уникальные идентификаторы (например, значения первичного ключа типа `int`, `string` или `Guid`).

Убедитесь, что значения, используемые для `@key`, не конфликтуют. Если конфликтные значения обнаруживаются в одном родительском элементе, Блазор создает исключение, поскольку оно не может детерминированно сопоставлять старые элементы или компоненты с новыми элементами или компонентами. Используйте только уникальные значения, такие как экземпляры объекта или значения первичного ключа.

## <a name="lifecycle-methods"></a>Методы жизненного цикла

`OnInitializedAsync` и `OnInitialized` выполнить код для инициализации компонента. Для выполнения асинхронной операции используйте `OnInitializedAsync` и ключевое слово `await` для операции:

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Для синхронной операции используйте `OnInitialized`:

```csharp
protected override void OnInitialized()
{
    ...
}
```

`OnParametersSetAsync` и `OnParametersSet` вызываются, когда компонент получил параметры от родительского объекта и значения присваиваются свойствам. Эти методы выполняются после инициализации компонента и при каждом отображении компонента:

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

`OnAfterRenderAsync` и `OnAfterRender` вызываются после завершения подготовки компонента к просмотру. В этот момент заполнены ссылки на элементы и компоненты. Используйте этот этап для выполнения дополнительных шагов инициализации с помощью готового к просмотру содержимого, например для активации сторонних библиотек JavaScript, которые работают с визуализированными элементами DOM.

`OnAfterRender` *не вызывается при предварительной отрисовке на сервере.*

Параметр `firstRender` для `OnAfterRenderAsync` и `OnAfterRender`:

* Задайте значение `true` при первом вызове экземпляра компонента.
* Гарантирует, что работа по инициализации выполняется только один раз.

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a>Обработка незавершенных асинхронных действий при отрисовке

Асинхронные действия, выполняемые в событиях жизненного цикла, могут не завершиться до подготовки компонента к просмотру. Объекты могут быть `null` или не заполнены данными во время выполнения метода жизненного цикла. Предоставьте логику отрисовки для подтверждения инициализации объектов. Отрисовывает элементы пользовательского интерфейса заполнителя (например, сообщение загрузки), а объекты — `null`.

В компоненте `FetchData` шаблонов Блазор `OnInitializedAsync` переопределяется в асичронаусли получения данных прогноза (`forecasts`). Если `forecasts` равно `null`, пользователю выводится сообщение о загрузке. После того, как `Task` возвращено `OnInitializedAsync`, компонент перерисовывается с обновленным состоянием.

*Pages/FetchData.razor*:

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a>Выполнить код до установки параметров

`SetParameters` можно переопределить для выполнения кода перед установкой параметров:

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

Если `base.SetParameters` не вызывается, Пользовательский код может интерпретировать значение входящих параметров любым необходимым образом. Например, входящие параметры не обязательно должны быть назначены свойствам класса.

### <a name="suppress-refreshing-of-the-ui"></a>Отключить обновление пользовательского интерфейса

`ShouldRender` можно переопределить, чтобы отключить обновление пользовательского интерфейса. Если реализация возвращает `true`, Пользовательский интерфейс обновляется. Даже при переопределении `ShouldRender` компонент всегда первоначально готовится к просмотру.

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a>Освобождение компонентов с помощью IDisposable

Если компонент реализует <xref:System.IDisposable>, [метод Dispose](/dotnet/standard/garbage-collection/implementing-dispose) вызывается при удалении компонента из пользовательского интерфейса. Следующий компонент использует `@implements IDisposable` и метод `Dispose`:

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a>Маршрутизация

Маршрутизация в Блазор достигается путем предоставления шаблона маршрута для каждого доступного компонента в приложении.

При компиляции файла Razor с директивой `@page` создаваемому классу присваивается значение <xref:Microsoft.AspNetCore.Mvc.RouteAttribute>, задающее шаблон маршрута. Во время выполнения маршрутизатор ищет классы компонентов с `RouteAttribute` и отображает, какой из компонентов имеет шаблон маршрута, соответствующий запрошенному URL-адресу.

К компоненту можно применить несколько шаблонов маршрутов. Следующий компонент отвечает на запросы для `/BlazorRoute` и `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a>Параметры маршрута

Компоненты могут принимать параметры маршрута из шаблона маршрута, указанного в директиве `@page`. Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента.

*Компонент параметра маршрута*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

Необязательные параметры не поддерживаются, поэтому в приведенном выше примере применяются две директивы `@page`. Первый позволяет переходить к компоненту без параметра. Вторая директива `@page` принимает параметр маршрута `{text}` и присваивает значение свойству `Text`.

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>Наследование базового класса для взаимодействия с кодом программной части

Файлы компонентов сочетают разметку HTML C# и обрабатывают код в одном файле. Директива `@inherits` может использоваться для предоставления Блазор приложений с помощью «кода программной части», который отделяет разметку компонентов от кода обработки.

[Пример приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) показывает, как компонент может наследовать базовый класс `BlazorRocksBase` для предоставления свойств и методов компонента.

*Pages/блазорроккс. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

*BlazorRocksBase.CS*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

Базовый класс должен быть производным от `ComponentBase`.

## <a name="import-components"></a>Импорт компонентов

Пространство имен компонента, созданного с помощью Razor, основано на (в порядке приоритета):

* [@namespace](xref:mvc/views/razor#namespace) обозначение в разметке файла Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).
* @No__t проекта-0 в файле проекта (`<RootNamespace>BlazorSample</RootNamespace>`).
* Имя проекта, полученное из имени файла проекта (*CSPROJ*), и путь из корневого каталога проекта к компоненту. Например, платформа разрешает *{root проекта}/Пажес/индекс.Разор* (*блазорсампле. csproj*) к пространству имен `BlazorSample.Pages`. Компоненты следуют C# правилам привязки имен. Для компонента `Index` в этом примере компоненты в области являются всеми компонентами:
  * В той же папке *страницы*.
  * Компоненты в корне проекта, которые не задают явно другое пространство имен.

Компоненты, определенные в другом пространстве имен, передаются в область с помощью директивы Razor [@using](xref:mvc/views/razor#using) .

Если другой компонент, `NavMenu.razor`, существует в папке *блазорсампле/Shared/* Folder, компонент можно использовать в `Index.razor` со следующей инструкцией `@using`:

```cshtml
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

На компоненты также можно ссылаться с помощью полных имен, для которых не требуется директива [@using](xref:mvc/views/razor#using) :

```cshtml
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> Квалификация `global::` не поддерживается.
>
> Импорт компонентов с псевдонимами `using` (например, `@using Foo = Bar`) не поддерживается.
>
> Частично определенные имена не поддерживаются. Например, добавление `@using BlazorSample` и ссылка `NavMenu.razor` с помощью `<Shared.NavMenu></Shared.NavMenu>` не поддерживается.

## <a name="conditional-html-element-attributes"></a>Атрибуты условного HTML-элемента

Атрибуты HTML-элемента условно подготавливаются в зависимости от значения .NET. Если значение равно `false` или `null`, то атрибут не отображается. Если значение равно `true`, то атрибут отображается в виде сворачивания.

В следующем примере `IsCompleted` определяет, отображается ли `checked` в разметке элемента:

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

Если `IsCompleted` равно `true`, флажок отображается следующим образом:

```html
<input type="checkbox" checked />
```

Если `IsCompleted` равно `false`, флажок отображается следующим образом:

```html
<input type="checkbox" />
```

Дополнительные сведения см. в разделе <xref:mvc/views/razor>.

> [!WARNING]
> Некоторые атрибуты HTML, такие как [ARIA-Pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), не работают должным образом, если тип .net — `bool`. В этих случаях используйте тип `string` вместо `bool`.

## <a name="raw-html"></a>Необработанный HTML

Строки обычно подготавливаются с помощью текстовых узлов DOM, что означает, что любая разметка, которую они могут содержать, игнорируется и обрабатывается как литеральный текст. Для отрисовки необработанного HTML-кода заключите содержимое HTML в значение `MarkupString`. Значение анализируется как HTML или SVG и вставляется в модель DOM.

> [!WARNING]
> Визуализация необработанного HTML-кода, созданного из любого ненадежного источника, является **угрозой безопасности** , и ее следует избегать.

В следующем примере показано использование типа `MarkupString` для добавления блока статического HTML-содержимого в отображаемые выходные данные компонента.

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Шаблонные компоненты

Шаблонные компоненты — это компоненты, принимающие один или несколько шаблонов пользовательского интерфейса в качестве параметров, которые затем можно использовать как часть логики отрисовки компонента. Шаблонные компоненты позволяют создавать более высокие компоненты более высокого уровня, чем обычные компоненты. Ниже приведены несколько примеров.

* Компонент таблицы, позволяющий пользователю указывать шаблоны для заголовков, строк и нижних колонтитулов таблицы.
* Компонент списка, позволяющий пользователю указать шаблон для отрисовки элементов в списке.

### <a name="template-parameters"></a>Параметры шаблона

Шаблонный компонент определяется путем указания одного или нескольких параметров компонента типа `RenderFragment` или `RenderFragment<T>`. Фрагмент отрисовки представляет сегмент пользовательского интерфейса для отрисовки. `RenderFragment<T>` принимает параметр типа, который может быть указан при вызове фрагмента прорисовки.

компонент `TableTemplate`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

При использовании компонент-шаблона можно указать параметры шаблона с помощью дочерних элементов, совпадающих с именами параметров (`TableHeader` и `RowTemplate` в следующем примере):

```cshtml
<TableTemplate Items="pets">
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

### <a name="template-context-parameters"></a>Параметры контекста шаблона

Аргументы компонента типа `RenderFragment<T>`, переданные как элементы, имеют неявный параметр с именем `context` (например, из предыдущего примера кода, `@context.PetId`), но можно изменить имя параметра с помощью атрибута `Context` в дочернем элементе. В следующем примере атрибуту `Context` элемента `RowTemplate` задается параметр `pet`:

```cshtml
<TableTemplate Items="pets">
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

Кроме того, можно указать атрибут `Context` в элементе Component. Указанный атрибут `Context` применяется ко всем указанным параметрам шаблона. Это может быть полезно, если необходимо указать имя параметра содержимого для неявного дочернего содержимого (без обертывания дочернего элемента). В следующем примере атрибут `Context` отображается в элементе `TableTemplate` и применяется ко всем параметрам шаблона:

```cshtml
<TableTemplate Items="pets" Context="pet">
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

### <a name="generic-typed-components"></a>Универсальные типы компонентов

Шаблонные компоненты часто вводятся в универсальном виде. Например, универсальный компонент `ListViewTemplate` можно использовать для отображения значений `IEnumerable<T>`. Чтобы определить универсальный компонент, используйте директиву [@typeparam](xref:mvc/views/razor#typeparam) для указания параметров типа:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

При использовании компонентов универсальной типизации параметр типа выводится, если это возможно:

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

В противном случае параметр типа необходимо явно указать с помощью атрибута, совпадающего с именем параметра типа. В следующем примере `TItem="Pet"` указывает тип:

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>Каскадные значения и параметры

В некоторых сценариях неудобно передавать данные из компонента-предка в дочерний компонент с помощью [параметров компонента](#component-parameters), особенно при наличии нескольких слоев компонентов. Каскадные значения и параметры позволяют решить эту проблему, предоставляя удобный способ, позволяющий компоненту-предку предоставить значение всем его дочерним компонентам. Каскадные значения и параметры также предоставляют подход для координации компонентов.

### <a name="theme-example"></a>Пример темы

В следующем примере из примера приложения класс `ThemeInfo` указывает сведения о теме для перетекания иерархии компонентов таким образом, чтобы все кнопки в пределах данной части приложения совместно совпадали с тем же стилем.

*Уисемеклассес/themeinfo указывает расположение. CS*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Компонент-предок может предоставлять каскадное значение с помощью компонента каскадного значения. Компонент `CascadingValue` заключает в оболочку поддерево иерархии компонентов и предоставляет одно значение всем компонентам в этом поддереве.

Например, в примере приложения указываются сведения о теме (`ThemeInfo`) в одном из макетов приложения в виде каскадного параметра для всех компонентов, составляющих тело макета свойства `@Body`. `ButtonClass` присвоено значение `btn-success` в компоненте макета. Любой дочерний компонент может использовать это свойство через каскадный объект `ThemeInfo`.

компонент `CascadingValuesParametersLayout`:

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

Чтобы использовать каскадные значения, компоненты объявляют каскадные параметры с помощью атрибута `[CascadingParameter]`. Каскадные значения привязываются к каскадным параметрам по типу.

В примере приложения компонент `CascadingValuesParametersTheme` привязывает каскадное значение `ThemeInfo` к каскадному параметру. Параметр используется для задания класса CSS для одной из кнопок, отображаемых компонентом.

компонент `CascadingValuesParametersTheme`:

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

Чтобы вычислить несколько значений одного типа в одном поддереве, укажите уникальную строку `Name` для каждого компонента `CascadingValue` и соответствующего `CascadingParameter`. В следующем примере два компонента `CascadingValue` разворачивают разные экземпляры `MyCascadingType` по имени:

```cshtml
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

В компоненте-потомках каскадные параметры получают значения из соответствующих каскадных значений в компоненте-предке по имени:

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a>Пример Табсет

Каскадные параметры также позволяют компонентам работать в иерархии компонентов. Например, рассмотрим следующий пример *табсет* в примере приложения.

В примере приложения имеется интерфейс `ITab`, который реализуется вкладками:

[!code-csharp[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

Компонент `CascadingValuesParametersTabSet` использует компонент `TabSet`, который содержит несколько компонентов `Tab`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

Дочерние компоненты `Tab` не передаются в качестве параметров в `TabSet`. Вместо этого дочерние компоненты `Tab` являются частью дочернего содержимого `TabSet`. Однако `TabSet` по-прежнему необходимо узнать о каждом компоненте `Tab`, чтобы он мог отобразить заголовки и активную вкладку. Чтобы обеспечить эту координацию, не требуя дополнительного кода, компонент `TabSet` *может предоставить себя как каскадное значение* , которое затем выбирается дочерними компонентами `Tab`.

компонент `TabSet`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

Дочерние компоненты `Tab` захватывают содержащий `TabSet` как каскадный параметр, поэтому компоненты `Tab` добавляют себя в `TabSet` и координирует, какая вкладка активна.

компонент `Tab`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a>Шаблоны Razor

Фрагменты визуализации можно определить с помощью синтаксиса шаблона Razor. Шаблоны Razor позволяют определить фрагмент пользовательского интерфейса и принять следующий формат:

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

В следующем примере показано, как указать значения `RenderFragment` и `RenderFragment<T>` и визуализировать шаблоны непосредственно в компоненте. Фрагменты рендеринга также могут передаваться в качестве аргументов в [шаблонные компоненты](#templated-components).

```cshtml
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

Отображаемые выходные данные предыдущего кода:

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a>Логика Рендертрибуилдер вручную

`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` предоставляет методы для управления компонентами и элементами, включая создание компонентов вручную в C# коде.

> [!NOTE]
> Использование `RenderTreeBuilder` для создания компонентов является расширенным сценарием. Неправильно сформированный компонент (например, незакрытый тег разметки) может привести к неопределенному поведению.

Рассмотрим следующий компонент `PetDetails`, который можно вручную встроить в другой компонент:

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

В следующем примере цикл в методе `CreateComponent` создает три компонента `PetDetails`. При вызове методов `RenderTreeBuilder` для создания компонентов (`OpenComponent` и `AddAttribute`) порядковые номера представляют собой номера строк исходного кода. Алгоритм разницы Блазор зависит от порядковых номеров, соответствующих разным строкам кода, а не отдельных вызовов вызова. При создании компонента с методами `RenderTreeBuilder` жестко указывайте аргументы для порядковых номеров. **Использование вычисления или счетчика для формирования порядкового номера может привести к снижению производительности.** Дополнительные сведения см. в разделе [порядковые номера относятся к разделу номера строк кода и порядок выполнения](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .

компонент `BuiltContent`:

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
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

> ! ! Типы в `Microsoft.AspNetCore.Components.RenderTree` позволяют обрабатывать *результаты* операций отрисовки. Это внутренние сведения о реализации Блазор Framework. Эти типы следует считать *нестабильными* и могут быть изменены в будущих выпусках.

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a>Порядковые номера связаны с номерами строк кода, а не с порядком выполнения

Файлы блазор `.razor` всегда компилируются. Это, вероятно, является отличным преимуществом для `.razor`, так как этап компиляции можно использовать для вставки сведений, повышающих производительность приложения во время выполнения.

Ключевым примером этих улучшений являются *порядковые номера*. Порядковые номера указывают среде выполнения, какие выходные данные поступили из разных и упорядоченных строк кода. Среда выполнения использует эти сведения для создания эффективных различий дерева в линейное время, что является гораздо быстрее, чем обычно возможно для общего алгоритма различения дерева.

Рассмотрим следующий простой файл `.razor`:

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

Предыдущий код компилируется примерно следующим образом:

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

Когда код выполняется впервые, если `someFlag` равно `true`, построитель получит следующее:

| Sequence | Type      | Data   |
| :------: | --------- | :----: |
| 0        | Текстовый узел | Первая  |
| 1        | Текстовый узел | Вторая |

Представьте, что `someFlag` становится `false`, а разметка снова готовится к просмотру. На этот раз построитель получит:

| Sequence | Type       | Data   |
| :------: | ---------- | :----: |
| 1        | Текстовый узел  | Вторая |

Когда среда выполнения выполняет различие, она видит, что элемент в последовательности `0` был удален, поэтому он создает следующий тривиальный *сценарий редактирования*:

* Удалите первый текстовый узел.

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a>Что становится неправильным, если вы создаете порядковые номера программным способом

Вместо этого Представьте себе, что вы написали следующую логику конструктора деревьев визуализации:

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

Теперь первые выходные данные:

| Sequence | Type      | Data   |
| :------: | --------- | :----: |
| 0        | Текстовый узел | Первая  |
| 1        | Текстовый узел | Вторая |

Этот результат идентичен предыдущему случаю, поэтому отрицательные проблемы не возникают. `someFlag` `false` при второй отрисовке, а выходные данные:

| Sequence | Type      | Data   |
| :------: | --------- | ------ |
| 0        | Текстовый узел | Вторая |

На этот раз алгоритм diff видит, что были внесены *два* изменения, и алгоритм создает следующий сценарий редактирования:

* Измените значение первого текстового узла на `Second`.
* Удалите второй текстовый узел.

При формировании порядковых номеров теряются все полезные сведения о том, где в исходном коде представлены ветви и циклы `if/else`. Это приводит к **удвоению в два раза больше** , чем раньше.

Это тривиальный пример. В более реалистичных случаях со сложными и глубокими вложенными структурами, особенно с циклами, затраты на производительность более серьезны. Вместо того, чтобы сразу определять, какие блоки или ветви цикла были вставлены или удалены, алгоритм diff должен рекурсивно пребираться в деревьях отрисовки и, как правило, создает гораздо более длительные операции редактирования, так как он сообщает о том, как старую и новую структуры связь друг с другом.

#### <a name="guidance-and-conclusions"></a>Руководство и выводы

* Производительность приложения снижается, если порядковые номера создаются динамически.
* Платформа не может автоматически создавать собственные порядковые номера во время выполнения, поскольку необходимая информация не существует, если она не захвачена во время компиляции.
* Не записывайте длинные блоки, реализованные вручную `RenderTreeBuilder`. Предпочитать файлы `.razor` и позволяют компилятору работать с порядковыми номерами. Если не удается избежать @no__t логики вручную, разделите длинные блоки кода на более мелкие части, Упакованные в вызовы `OpenRegion` @ no__t-2 @ no__t-3. Каждый регион имеет собственное отдельное пространство порядковых номеров, поэтому вы можете перезапускаться от нуля (или любого другого произвольного числа) внутри каждого региона.
* Если порядковые номера задаются жестко, то для алгоритма diff требуется, чтобы только порядковые номера увеличиваются в значении. Начальное значение и зазоры несущественны. Один из этих вариантов — использовать номер строки кода в качестве порядкового номера или начать с нуля и увеличить на единицу или сотни (или любой другой интервал). 
* Блазор использует порядковые номера, а другие платформы пользовательского интерфейса для различения структуры не используют их. Сравнение выполняется гораздо быстрее, если используются порядковые номера, и Блазор имеет преимущество этапа компиляции, который автоматически обрабатывает порядковые номера для разработчиков, занимающихся созданием файлов `.razor`.

## <a name="localization"></a>Локализация

Блазор серверные приложения локализованы по [промежуточного слоя локализации](xref:fundamentals/localization#localization-middleware). По промежуточного слоя выбирает соответствующие языки и региональные параметры для пользователей, запрашивающих ресурсы из приложения.

Язык и региональные параметры можно задать с помощью одного из следующих подходов:

* [Файлы "cookie"](#cookies)
* [Предоставление пользовательского интерфейса для выбора языка и региональных параметров](#provide-ui-to-choose-the-culture)

Дополнительные сведения и примеры см. в разделе <xref:fundamentals/localization>.

### <a name="cookies"></a>Файлы cookie

Файл cookie культуры локализации может сохранять язык и региональные параметры пользователя. Файл cookie создается методом `OnGet` страницы узла приложения (*pages/Host. cshtml. CS*). По промежуточного слоя локализации считывает файл cookie при последующих запросах, чтобы задать язык и региональные параметры пользователя. 

Использование файла cookie гарантирует, что соединение WebSocket сможет правильно распространить культуру. Если схемы локализации основаны на пути URL-адреса или строке запроса, схема может не поддерживать работу с WebSockets, поэтому она не сохраняет культуру. Поэтому рекомендуемым подходом является использование файла cookie языка и региональных параметров локализации.

Любой метод можно использовать для назначения языка и региональных параметров, если язык и региональные параметры сохраняются в файле cookie локализации. Если у приложения уже есть установленная схема локализации для ASP.NET Core на стороне сервера, продолжайте использовать существующую инфраструктуру локализации приложения и задавайте файл cookie культуры локализации в схеме приложения.

В следующем примере показано, как задать текущий язык и региональные параметры в файле cookie, который может быть прочитан по промежуточного слоя локализации. Создайте файл *pages/Host. cshtml. CS* со следующим содержимым в приложении блазор Server:

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

Локализация обрабатывается в приложении:

1. Браузер отправляет в приложение исходный HTTP-запрос.
1. Язык и региональные параметры назначаются по промежуточного слоя локализации.
1. Метод `OnGet` в *_Host. cshtml. CS* сохраняет язык и региональные параметры в файле cookie как часть ответа.
1. Браузер открывает соединение WebSocket для создания интерактивного сеанса Блазор Server.
1. По промежуточного слоя для локализации считывает файл cookie и назначает язык и региональные параметры.
1. Сеанс сервера Блазор начинается с правильного языка и региональных параметров.

## <a name="provide-ui-to-choose-the-culture"></a>Предоставление пользовательского интерфейса для выбора языка и региональных параметров

Для предоставления пользовательского интерфейса, позволяющего пользователю выбирать язык и региональные параметры, рекомендуется *подход на основе перенаправления* . Процесс аналогичен тому, что происходит в веб-приложении, когда пользователь пытается получить доступ к защищенному ресурсу @ no__t-0the пользователь перенаправляется на страницу входа, а затем перенаправляется обратно к исходному ресурсу. 

Приложение сохраняет выбранный язык и региональные параметры пользователя с помощью перенаправления к контроллеру. Контроллер устанавливает выбранный язык и региональные параметры пользователя в файл cookie и перенаправляет пользователя обратно к исходному коду URI.

Установите конечную точку HTTP на сервере, чтобы задать выбранный язык и региональные параметры пользователя в файле cookie, и выполните перенаправление обратно в исходный URI:

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> Используйте результат действия `LocalRedirect`, чтобы предотвратить атаки с открытым перенаправлением. Дополнительные сведения см. в разделе <xref:security/preventing-open-redirects>.

В следующем компоненте показан пример выполнения начального перенаправления, когда пользователь выбирает язык и региональные параметры:

```cshtml
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a>Использование сценариев локализации .NET в приложениях Блазор

В приложениях Блазор доступны следующие сценарии локализации и глобализации .NET:

* . Система ресурсов NET
* Форматирование чисел и дат, зависящих от языка и региональных параметров

Функция `@bind` блазор выполняет глобализацию на основе текущего языка и региональных параметров пользователя. Дополнительные сведения см. в разделе [Привязка данных](#data-binding) .

В настоящее время поддерживается ограниченный набор сценариев локализации ASP.NET Core:

* `IStringLocalizer<>` *поддерживается* в приложениях блазор.
* `IHtmlLocalizer<>`, `IViewLocalizer<>` и локализация заметок к данным — ASP.NET Core сценарии MVC и **не поддерживаются** в приложениях блазор.

Дополнительные сведения см. в разделе <xref:fundamentals/localization>.

## <a name="scalable-vector-graphics-svg-images"></a>Масштабируемые изображения векторной графики (SVG)

Поскольку Блазор визуализирует HTML-файлы, поддерживаемые браузером изображения, включая масштабируемые векторные графики (SVG *),* поддерживаются с помощью тега `<img>`:

```html
<img alt="Example image" src="some-image.svg" />
```

Аналогичным образом SVG-изображения поддерживаются в правилах CSS файла стилей ( *. CSS*):

```css
.my-element {
    background-image: url("some-image.svg");
}
```

Однако встроенная разметка SVG не поддерживается во всех сценариях. Если поместить тег `<svg>` непосредственно в файл компонента (*Razor*), то поддерживается базовая визуализация изображений, но многие расширенные сценарии еще не поддерживаются. Например, в настоящее время Теги `<use>` не учитываются, и `@bind` нельзя использовать с некоторыми тегами SVG. В будущем выпуске мы планируем устранить эти ограничения.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/blazor/server> &ndash; включает рекомендации по созданию серверных приложений Блазор, которые должны конкурировать с нехваткой ресурсов.
