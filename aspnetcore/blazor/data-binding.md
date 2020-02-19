---
title: Привязка данных Blazor ASP.NET Core
author: guardrex
description: Сведения о сценариях привязки данных для компонентов и элементов DOM в Blazor приложениях.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/data-binding
ms.openlocfilehash: c38e6095d4e93d3eead10fa8bb0356b2113bb220
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453160"
---
# <a name="aspnet-core-opno-locblazor-data-binding"></a>Привязка данных Blazor ASP.NET Core

[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)

Привязка данных как к компонентам, так и к элементам DOM осуществляется с помощью атрибута [`@bind`](xref:mvc/views/razor#bind) . В следующем примере свойство `CurrentValue` привязывается к значению текстового поля:

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

Когда текстовое поле теряет фокус, значение свойства обновляется.

Текстовое поле обновляется в пользовательском интерфейсе только при подготовке компонента к просмотру, а не в ответ на изменение значения свойства. Поскольку компоненты отображаются после выполнения кода обработчика событий, обновления свойств *обычно* отражаются в пользовательском интерфейсе сразу после запуска обработчика событий.

Использование `@bind` со свойством `CurrentValue` (`<input @bind="CurrentValue" />`) в основном эквивалентно следующему:

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

При подготовке к просмотру компонента `value` входного элемента берется из свойства `CurrentValue`. Когда пользователь вводит данные в текстовое поле и изменяет фокус элемента, запускается событие `onchange`, а свойству `CurrentValue` присваивается измененное значение. На практике создание кода более сложно, так как `@bind` обрабатывает случаи, когда выполняются преобразования типов. В принципе `@bind` связывает текущее значение выражения с атрибутом `value` и обрабатывает изменения с помощью зарегистрированного обработчика.

Помимо обработки `onchange`ных событий с помощью синтаксиса `@bind`, свойство или поле можно привязать с помощью других событий, указав атрибут [`@bind-value`](xref:mvc/views/razor#bind) с параметром `event` ([`@bind-value:event`](xref:mvc/views/razor#bind)). В следующем примере выполняется привязка свойства `CurrentValue` для события `oninput`.

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

В отличие от `onchange`, которая срабатывает при потере фокуса элементом, `oninput` срабатывает при изменении значения текстового поля.

`@bind-value` в предыдущем примере выполняет привязку:

* Указанное выражение (`CurrentValue`) в атрибут `value` элемента.
* Делегат события Change для события, указанного `@bind-value:event`.

### <a name="unparsable-values"></a>Неанализируемые значения

Когда пользователь предоставляет неинтерпретируемое значение для элемента с привязкой к данным, неанализируемое значение автоматически возвращается к предыдущему значению при срабатывании события привязки.

Рассмотрим следующий сценарий.

* Элемент `<input>` привязан к типу `int` с начальным значением `123`:

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* Пользователь обновляет значение элемента для `123.45` на странице и изменяет фокус элемента.

В приведенном выше сценарии значение элемента возвращается к `123`. Если значение `123.45` отклоняется в пользу исходного значения `123`, пользователь понимает, что их значение не принято.

По умолчанию привязка применяется к событию `onchange` элемента (`@bind="{PROPERTY OR FIELD}"`). Для задания другого события используйте `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}`. Для события `oninput` (`@bind-value:event="oninput"`) переверсия происходит после любого нажатия клавиши, которая вводит неинтерпретируемое значение. При нацеливании на событие `oninput` с типом, привязанным к `int`, пользователю запрещено вводить `.` символ. `.`ный символ немедленно удаляется, поэтому пользователь получает немедленный отзыв о том, что разрешены только целые числа. Существуют сценарии, в которых возвращение значения в событие `oninput` не является идеальным, например, когда пользователю разрешено очищать неинтерпретируемое `<input>` значение. К альтернативам относятся:

* Не используйте событие `oninput`. Используйте событие `onchange` по умолчанию (`@bind="{PROPERTY OR FIELD}"`), где недопустимое значение не возвращается, пока элемент не теряет фокус.
* Выполните привязку к типу, допускающему значение null, например `int?` или `string`, и предоставьте настраиваемую логику для обработки недопустимых записей.
* Используйте [компонент проверки формы](xref:blazor/forms-validation), например `InputNumber` или `InputDate`. Компоненты проверки форм имеют встроенную поддержку для управления недопустимыми входными данными. Компоненты проверки формы:
  * Разрешите пользователю указать недопустимые входные данные и получить ошибки проверки для связанного `EditContext`.
  * Отображает ошибки проверки в пользовательском интерфейсе, не мешая пользователю вводить дополнительные данные из формы.

### <a name="format-strings"></a>Строки формата

Привязка данных работает со строками формата <xref:System.DateTime> с помощью [`@bind:format`](xref:mvc/views/razor#bind). Другие выражения форматирования, такие как денежные или числовые форматы, в настоящее время недоступны.

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

В приведенном выше коде тип поля `<input>` элемента (`type`) по умолчанию имеет значение `text`. `@bind:format` поддерживается для привязки следующих типов .NET:

* <xref:System.DateTime?displayProperty=fullName>
* <xref:System.DateTime?displayProperty=fullName>?
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <xref:System.DateTimeOffset?displayProperty=fullName>?

Атрибут `@bind:format` задает формат даты, применяемый к `value` элемента `<input>`. Формат также используется для анализа значения при возникновении `onchange` события.

Указание формата для типа поля `date` не рекомендуется, так как Blazor имеет встроенную поддержку для форматирования дат. Несмотря на рекомендацию, используйте формат даты `yyyy-MM-dd` для правильной работы привязки только в том случае, если указан формат с типом поля `date`:

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

### <a name="parent-to-child-binding-with-component-parameters"></a>Привязка "родители — потомки" с параметрами компонента

Привязка распознает параметры компонента, где `@bind-{property}` может привязать значение свойства из родительского компонента к дочернему компоненту. Привязка дочернего элемента к родительскому элементу рассматривается в [привязке дочерний к родительскому разделу с цепочкой привязки](#child-to-parent-binding-with-chained-bind) .

Следующий дочерний компонент (`ChildComponent`) имеет параметр компонента `Year` и обратный вызов `YearChanged`:

```razor
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

`EventCallback<T>` объясняется в <xref:blazor/event-handling#eventcallback>.

Следующий родительский компонент использует:

* `ChildComponent` и привязывает параметр `ParentYear` из родительского элемента к параметру `Year` дочернего компонента.
* Событие `onclick` используется для активации метода `ChangeTheYear`. Дополнительные сведения см. в разделе <xref:blazor/event-handling>.

```razor
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

Если значение свойства `ParentYear` изменяется путем нажатия кнопки в `ParentComponent`, свойство `Year` `ChildComponent` обновляется. Новое значение `Year` отображается в пользовательском интерфейсе при перевизуализации `ParentComponent`:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

Параметр `Year` является связываемым, так как имеет сопутствующее `YearChanged` событие, соответствующее типу параметра `Year`.

По соглашению `<ChildComponent @bind-Year="ParentYear" />`, по сути, эквивалентен написанию:

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

Как правило, свойство может быть привязано к соответствующему обработчику событий с помощью атрибута `@bind-property:event`. Например, свойство `MyProp` можно привязать к `MyEventHandler`у с помощью следующих двух атрибутов:

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

### <a name="child-to-parent-binding-with-chained-bind"></a>Привязка дочернего элемента в родитель с сцепленной привязкой

Распространенным сценарием является привязка привязанного к данным параметра к элементу страницы в выходных данных компонента. Этот сценарий называется *связанной* привязкой, так как несколько уровней привязки происходят одновременно.

Связанную с цепочкой привязку нельзя реализовать с помощью синтаксиса `@bind` в элементе страницы. Обработчик событий и значение должны быть указаны отдельно. Однако родительский компонент может использовать `@bind` синтаксис с параметром компонента.

Следующий `PasswordField` компонент (*пассвордфиелд. Razor*):

* Задает значение `<input>` элемента для свойства `Password`.
* Предоставляет изменения свойства `Password` в родительский компонент с помощью [вложенный EventCallback](xref:blazor/event-handling#eventcallback).
* Используется событие `onclick` для активации метода `ToggleShowPassword`. Дополнительные сведения см. в разделе <xref:blazor/event-handling>.

```razor
<h1>Child Component</h2>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool _showPassword;

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
        _showPassword = !_showPassword;
    }
}
```

Компонент `PasswordField` используется в другом компоненте:

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

Для выполнения проверок или перехвата ошибок в пароле в предыдущем примере:

* Создайте резервное поле для `Password` (`_password` в следующем примере кода).
* Выполните проверки или ошибки ловушек в методе задания `Password`.

В следующем примере представлена немедленная реакция пользователя, если в значении пароля используется пробел:

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@_validationMessage</span>

@code {
    private bool _showPassword;
    private string _password;
    private string _validationMessage;

    [Parameter]
    public string Password
    {
        get { return _password ?? string.Empty; }
        set
        {
            if (_password != value)
            {
                if (value.Contains(' '))
                {
                    _validationMessage = "Spaces not allowed!";
                }
                else
                {
                    _password = value;
                    _validationMessage = string.Empty;
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
        _showPassword = !_showPassword;
    }
}
```

### <a name="radio-buttons"></a>Переключатели

Сведения о привязке к переключателям в форме см. в разделе <xref:blazor/forms-validation#work-with-radio-buttons>.
