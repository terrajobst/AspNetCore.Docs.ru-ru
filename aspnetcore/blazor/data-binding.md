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
# <a name="aspnet-core-opno-locblazor-data-binding"></a><span data-ttu-id="97820-103">Привязка данных Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="97820-103">ASP.NET Core Blazor data binding</span></span>

<span data-ttu-id="97820-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="97820-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="97820-105">Привязка данных как к компонентам, так и к элементам DOM осуществляется с помощью атрибута [`@bind`](xref:mvc/views/razor#bind) .</span><span class="sxs-lookup"><span data-stu-id="97820-105">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="97820-106">В следующем примере свойство `CurrentValue` привязывается к значению текстового поля:</span><span class="sxs-lookup"><span data-stu-id="97820-106">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="97820-107">Когда текстовое поле теряет фокус, значение свойства обновляется.</span><span class="sxs-lookup"><span data-stu-id="97820-107">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="97820-108">Текстовое поле обновляется в пользовательском интерфейсе только при подготовке компонента к просмотру, а не в ответ на изменение значения свойства.</span><span class="sxs-lookup"><span data-stu-id="97820-108">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="97820-109">Поскольку компоненты отображаются после выполнения кода обработчика событий, обновления свойств *обычно* отражаются в пользовательском интерфейсе сразу после запуска обработчика событий.</span><span class="sxs-lookup"><span data-stu-id="97820-109">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="97820-110">Использование `@bind` со свойством `CurrentValue` (`<input @bind="CurrentValue" />`) в основном эквивалентно следующему:</span><span class="sxs-lookup"><span data-stu-id="97820-110">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="97820-111">При подготовке к просмотру компонента `value` входного элемента берется из свойства `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="97820-111">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="97820-112">Когда пользователь вводит данные в текстовое поле и изменяет фокус элемента, запускается событие `onchange`, а свойству `CurrentValue` присваивается измененное значение.</span><span class="sxs-lookup"><span data-stu-id="97820-112">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="97820-113">На практике создание кода более сложно, так как `@bind` обрабатывает случаи, когда выполняются преобразования типов.</span><span class="sxs-lookup"><span data-stu-id="97820-113">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="97820-114">В принципе `@bind` связывает текущее значение выражения с атрибутом `value` и обрабатывает изменения с помощью зарегистрированного обработчика.</span><span class="sxs-lookup"><span data-stu-id="97820-114">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="97820-115">Помимо обработки `onchange`ных событий с помощью синтаксиса `@bind`, свойство или поле можно привязать с помощью других событий, указав атрибут [`@bind-value`](xref:mvc/views/razor#bind) с параметром `event` ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="97820-115">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [`@bind-value`](xref:mvc/views/razor#bind) attribute with an `event` parameter ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="97820-116">В следующем примере выполняется привязка свойства `CurrentValue` для события `oninput`.</span><span class="sxs-lookup"><span data-stu-id="97820-116">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="97820-117">В отличие от `onchange`, которая срабатывает при потере фокуса элементом, `oninput` срабатывает при изменении значения текстового поля.</span><span class="sxs-lookup"><span data-stu-id="97820-117">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="97820-118">`@bind-value` в предыдущем примере выполняет привязку:</span><span class="sxs-lookup"><span data-stu-id="97820-118">`@bind-value` in the preceding example binds:</span></span>

* <span data-ttu-id="97820-119">Указанное выражение (`CurrentValue`) в атрибут `value` элемента.</span><span class="sxs-lookup"><span data-stu-id="97820-119">The specified expression (`CurrentValue`) to the element's `value` attribute.</span></span>
* <span data-ttu-id="97820-120">Делегат события Change для события, указанного `@bind-value:event`.</span><span class="sxs-lookup"><span data-stu-id="97820-120">A change event delegate to the event specified by `@bind-value:event`.</span></span>

### <a name="unparsable-values"></a><span data-ttu-id="97820-121">Неанализируемые значения</span><span class="sxs-lookup"><span data-stu-id="97820-121">Unparsable values</span></span>

<span data-ttu-id="97820-122">Когда пользователь предоставляет неинтерпретируемое значение для элемента с привязкой к данным, неанализируемое значение автоматически возвращается к предыдущему значению при срабатывании события привязки.</span><span class="sxs-lookup"><span data-stu-id="97820-122">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="97820-123">Рассмотрим следующий сценарий.</span><span class="sxs-lookup"><span data-stu-id="97820-123">Consider the following scenario:</span></span>

* <span data-ttu-id="97820-124">Элемент `<input>` привязан к типу `int` с начальным значением `123`:</span><span class="sxs-lookup"><span data-stu-id="97820-124">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="97820-125">Пользователь обновляет значение элемента для `123.45` на странице и изменяет фокус элемента.</span><span class="sxs-lookup"><span data-stu-id="97820-125">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="97820-126">В приведенном выше сценарии значение элемента возвращается к `123`.</span><span class="sxs-lookup"><span data-stu-id="97820-126">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="97820-127">Если значение `123.45` отклоняется в пользу исходного значения `123`, пользователь понимает, что их значение не принято.</span><span class="sxs-lookup"><span data-stu-id="97820-127">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="97820-128">По умолчанию привязка применяется к событию `onchange` элемента (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="97820-128">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="97820-129">Для задания другого события используйте `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}`.</span><span class="sxs-lookup"><span data-stu-id="97820-129">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="97820-130">Для события `oninput` (`@bind-value:event="oninput"`) переверсия происходит после любого нажатия клавиши, которая вводит неинтерпретируемое значение.</span><span class="sxs-lookup"><span data-stu-id="97820-130">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="97820-131">При нацеливании на событие `oninput` с типом, привязанным к `int`, пользователю запрещено вводить `.` символ.</span><span class="sxs-lookup"><span data-stu-id="97820-131">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="97820-132">`.`ный символ немедленно удаляется, поэтому пользователь получает немедленный отзыв о том, что разрешены только целые числа.</span><span class="sxs-lookup"><span data-stu-id="97820-132">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="97820-133">Существуют сценарии, в которых возвращение значения в событие `oninput` не является идеальным, например, когда пользователю разрешено очищать неинтерпретируемое `<input>` значение.</span><span class="sxs-lookup"><span data-stu-id="97820-133">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="97820-134">К альтернативам относятся:</span><span class="sxs-lookup"><span data-stu-id="97820-134">Alternatives include:</span></span>

* <span data-ttu-id="97820-135">Не используйте событие `oninput`.</span><span class="sxs-lookup"><span data-stu-id="97820-135">Don't use the `oninput` event.</span></span> <span data-ttu-id="97820-136">Используйте событие `onchange` по умолчанию (`@bind="{PROPERTY OR FIELD}"`), где недопустимое значение не возвращается, пока элемент не теряет фокус.</span><span class="sxs-lookup"><span data-stu-id="97820-136">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="97820-137">Выполните привязку к типу, допускающему значение null, например `int?` или `string`, и предоставьте настраиваемую логику для обработки недопустимых записей.</span><span class="sxs-lookup"><span data-stu-id="97820-137">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="97820-138">Используйте [компонент проверки формы](xref:blazor/forms-validation), например `InputNumber` или `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="97820-138">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="97820-139">Компоненты проверки форм имеют встроенную поддержку для управления недопустимыми входными данными.</span><span class="sxs-lookup"><span data-stu-id="97820-139">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="97820-140">Компоненты проверки формы:</span><span class="sxs-lookup"><span data-stu-id="97820-140">Form validation components:</span></span>
  * <span data-ttu-id="97820-141">Разрешите пользователю указать недопустимые входные данные и получить ошибки проверки для связанного `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="97820-141">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="97820-142">Отображает ошибки проверки в пользовательском интерфейсе, не мешая пользователю вводить дополнительные данные из формы.</span><span class="sxs-lookup"><span data-stu-id="97820-142">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

### <a name="format-strings"></a><span data-ttu-id="97820-143">Строки формата</span><span class="sxs-lookup"><span data-stu-id="97820-143">Format strings</span></span>

<span data-ttu-id="97820-144">Привязка данных работает со строками формата <xref:System.DateTime> с помощью [`@bind:format`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="97820-144">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="97820-145">Другие выражения форматирования, такие как денежные или числовые форматы, в настоящее время недоступны.</span><span class="sxs-lookup"><span data-stu-id="97820-145">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="97820-146">В приведенном выше коде тип поля `<input>` элемента (`type`) по умолчанию имеет значение `text`.</span><span class="sxs-lookup"><span data-stu-id="97820-146">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="97820-147">`@bind:format` поддерживается для привязки следующих типов .NET:</span><span class="sxs-lookup"><span data-stu-id="97820-147">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="97820-148"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="97820-148"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="97820-149"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="97820-149"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="97820-150">Атрибут `@bind:format` задает формат даты, применяемый к `value` элемента `<input>`.</span><span class="sxs-lookup"><span data-stu-id="97820-150">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="97820-151">Формат также используется для анализа значения при возникновении `onchange` события.</span><span class="sxs-lookup"><span data-stu-id="97820-151">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="97820-152">Указание формата для типа поля `date` не рекомендуется, так как Blazor имеет встроенную поддержку для форматирования дат.</span><span class="sxs-lookup"><span data-stu-id="97820-152">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="97820-153">Несмотря на рекомендацию, используйте формат даты `yyyy-MM-dd` для правильной работы привязки только в том случае, если указан формат с типом поля `date`:</span><span class="sxs-lookup"><span data-stu-id="97820-153">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

### <a name="parent-to-child-binding-with-component-parameters"></a><span data-ttu-id="97820-154">Привязка "родители — потомки" с параметрами компонента</span><span class="sxs-lookup"><span data-stu-id="97820-154">Parent-to-child binding with component parameters</span></span>

<span data-ttu-id="97820-155">Привязка распознает параметры компонента, где `@bind-{property}` может привязать значение свойства из родительского компонента к дочернему компоненту.</span><span class="sxs-lookup"><span data-stu-id="97820-155">Binding recognizes component parameters, where `@bind-{property}` can bind a property value from a parent component down to a child component.</span></span> <span data-ttu-id="97820-156">Привязка дочернего элемента к родительскому элементу рассматривается в [привязке дочерний к родительскому разделу с цепочкой привязки](#child-to-parent-binding-with-chained-bind) .</span><span class="sxs-lookup"><span data-stu-id="97820-156">Binding from a child to a parent is covered in the [Child-to-parent binding with chained bind](#child-to-parent-binding-with-chained-bind) section.</span></span>

<span data-ttu-id="97820-157">Следующий дочерний компонент (`ChildComponent`) имеет параметр компонента `Year` и обратный вызов `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="97820-157">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="97820-158">`EventCallback<T>` объясняется в <xref:blazor/event-handling#eventcallback>.</span><span class="sxs-lookup"><span data-stu-id="97820-158">`EventCallback<T>` is explained in <xref:blazor/event-handling#eventcallback>.</span></span>

<span data-ttu-id="97820-159">Следующий родительский компонент использует:</span><span class="sxs-lookup"><span data-stu-id="97820-159">The following parent component uses:</span></span>

* <span data-ttu-id="97820-160">`ChildComponent` и привязывает параметр `ParentYear` из родительского элемента к параметру `Year` дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="97820-160">`ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component.</span></span>
* <span data-ttu-id="97820-161">Событие `onclick` используется для активации метода `ChangeTheYear`.</span><span class="sxs-lookup"><span data-stu-id="97820-161">The `onclick` event is used to trigger the `ChangeTheYear` method.</span></span> <span data-ttu-id="97820-162">Дополнительные сведения см. в разделе <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="97820-162">For more information, see <xref:blazor/event-handling>.</span></span>

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

<span data-ttu-id="97820-163">При загрузке `ParentComponent` создается следующая разметка:</span><span class="sxs-lookup"><span data-stu-id="97820-163">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="97820-164">Если значение свойства `ParentYear` изменяется путем нажатия кнопки в `ParentComponent`, свойство `Year` `ChildComponent` обновляется.</span><span class="sxs-lookup"><span data-stu-id="97820-164">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="97820-165">Новое значение `Year` отображается в пользовательском интерфейсе при перевизуализации `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="97820-165">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="97820-166">Параметр `Year` является связываемым, так как имеет сопутствующее `YearChanged` событие, соответствующее типу параметра `Year`.</span><span class="sxs-lookup"><span data-stu-id="97820-166">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="97820-167">По соглашению `<ChildComponent @bind-Year="ParentYear" />`, по сути, эквивалентен написанию:</span><span class="sxs-lookup"><span data-stu-id="97820-167">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="97820-168">Как правило, свойство может быть привязано к соответствующему обработчику событий с помощью атрибута `@bind-property:event`.</span><span class="sxs-lookup"><span data-stu-id="97820-168">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="97820-169">Например, свойство `MyProp` можно привязать к `MyEventHandler`у с помощью следующих двух атрибутов:</span><span class="sxs-lookup"><span data-stu-id="97820-169">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

### <a name="child-to-parent-binding-with-chained-bind"></a><span data-ttu-id="97820-170">Привязка дочернего элемента в родитель с сцепленной привязкой</span><span class="sxs-lookup"><span data-stu-id="97820-170">Child-to-parent binding with chained bind</span></span>

<span data-ttu-id="97820-171">Распространенным сценарием является привязка привязанного к данным параметра к элементу страницы в выходных данных компонента.</span><span class="sxs-lookup"><span data-stu-id="97820-171">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="97820-172">Этот сценарий называется *связанной* привязкой, так как несколько уровней привязки происходят одновременно.</span><span class="sxs-lookup"><span data-stu-id="97820-172">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="97820-173">Связанную с цепочкой привязку нельзя реализовать с помощью синтаксиса `@bind` в элементе страницы.</span><span class="sxs-lookup"><span data-stu-id="97820-173">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="97820-174">Обработчик событий и значение должны быть указаны отдельно.</span><span class="sxs-lookup"><span data-stu-id="97820-174">The event handler and value must be specified separately.</span></span> <span data-ttu-id="97820-175">Однако родительский компонент может использовать `@bind` синтаксис с параметром компонента.</span><span class="sxs-lookup"><span data-stu-id="97820-175">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="97820-176">Следующий `PasswordField` компонент (*пассвордфиелд. Razor*):</span><span class="sxs-lookup"><span data-stu-id="97820-176">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="97820-177">Задает значение `<input>` элемента для свойства `Password`.</span><span class="sxs-lookup"><span data-stu-id="97820-177">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="97820-178">Предоставляет изменения свойства `Password` в родительский компонент с помощью [вложенный EventCallback](xref:blazor/event-handling#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="97820-178">Exposes changes of the `Password` property to a parent component with an [EventCallback](xref:blazor/event-handling#eventcallback).</span></span>
* <span data-ttu-id="97820-179">Используется событие `onclick` для активации метода `ToggleShowPassword`.</span><span class="sxs-lookup"><span data-stu-id="97820-179">Uses the `onclick` event is used to trigger the `ToggleShowPassword` method.</span></span> <span data-ttu-id="97820-180">Дополнительные сведения см. в разделе <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="97820-180">For more information, see <xref:blazor/event-handling>.</span></span>

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

<span data-ttu-id="97820-181">Компонент `PasswordField` используется в другом компоненте:</span><span class="sxs-lookup"><span data-stu-id="97820-181">The `PasswordField` component is used in another component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

<span data-ttu-id="97820-182">Для выполнения проверок или перехвата ошибок в пароле в предыдущем примере:</span><span class="sxs-lookup"><span data-stu-id="97820-182">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="97820-183">Создайте резервное поле для `Password` (`_password` в следующем примере кода).</span><span class="sxs-lookup"><span data-stu-id="97820-183">Create a backing field for `Password` (`_password` in the following example code).</span></span>
* <span data-ttu-id="97820-184">Выполните проверки или ошибки ловушек в методе задания `Password`.</span><span class="sxs-lookup"><span data-stu-id="97820-184">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="97820-185">В следующем примере представлена немедленная реакция пользователя, если в значении пароля используется пробел:</span><span class="sxs-lookup"><span data-stu-id="97820-185">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

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

### <a name="radio-buttons"></a><span data-ttu-id="97820-186">Переключатели</span><span class="sxs-lookup"><span data-stu-id="97820-186">Radio buttons</span></span>

<span data-ttu-id="97820-187">Сведения о привязке к переключателям в форме см. в разделе <xref:blazor/forms-validation#work-with-radio-buttons>.</span><span class="sxs-lookup"><span data-stu-id="97820-187">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>
