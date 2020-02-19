---
title: Обработка событий ASP.NET Core Blazor
author: guardrex
description: Сведения о сценариях обработки событий Blazor, включая типы аргументов событий, обратные вызовы событий и управление событиями браузера по умолчанию.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/event-handling
ms.openlocfilehash: 25844ef39aee849072d16f3d73eda0a1c20ee788
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453166"
---
# <a name="aspnet-core-blazor-event-handling"></a><span data-ttu-id="a59e9-103">Обработка событий ASP.NET Core Блазор</span><span class="sxs-lookup"><span data-stu-id="a59e9-103">ASP.NET Core Blazor event handling</span></span>

<span data-ttu-id="a59e9-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="a59e9-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="a59e9-105">Компоненты Razor предоставляют функции обработки событий.</span><span class="sxs-lookup"><span data-stu-id="a59e9-105">Razor components provide event handling features.</span></span> <span data-ttu-id="a59e9-106">Для атрибута HTML-элемента с именем `on{EVENT}` (например, `onclick` и `onsubmit`) со значением, вводимым с помощью делегата, компоненты Razor рассматривают значение атрибута как обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="a59e9-106">For an HTML element attribute named `on{EVENT}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="a59e9-107">Имя атрибута всегда имеет формат [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="a59e9-107">The attribute's name is always formatted [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="a59e9-108">Следующий код вызывает метод `UpdateHeading`, когда кнопка выбрана в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="a59e9-108">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```razor
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

<span data-ttu-id="a59e9-109">Следующий код вызывает метод `CheckChanged` при изменении флажка в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="a59e9-109">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="a59e9-110">Обработчики событий также могут быть асинхронными и возвращать <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="a59e9-110">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="a59e9-111">Нет необходимости вручную вызывать [статехасчанжед](xref:blazor/lifecycle#state-changes).</span><span class="sxs-lookup"><span data-stu-id="a59e9-111">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="a59e9-112">Исключения регистрируются при их возникновении.</span><span class="sxs-lookup"><span data-stu-id="a59e9-112">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="a59e9-113">В следующем примере `UpdateHeading` вызывается асинхронно при выборе кнопки:</span><span class="sxs-lookup"><span data-stu-id="a59e9-113">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```razor
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

## <a name="event-argument-types"></a><span data-ttu-id="a59e9-114">Типы аргументов событий</span><span class="sxs-lookup"><span data-stu-id="a59e9-114">Event argument types</span></span>

<span data-ttu-id="a59e9-115">Для некоторых событий разрешены типы аргументов событий.</span><span class="sxs-lookup"><span data-stu-id="a59e9-115">For some events, event argument types are permitted.</span></span> <span data-ttu-id="a59e9-116">Указание типа события в вызове метода требуется только в том случае, если тип события используется в методе.</span><span class="sxs-lookup"><span data-stu-id="a59e9-116">Specifying an event type in the method call is only necessary if the event type is used in the method.</span></span>

<span data-ttu-id="a59e9-117">Поддерживаемые `EventArgs` приведены в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="a59e9-117">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="a59e9-118">Событие</span><span class="sxs-lookup"><span data-stu-id="a59e9-118">Event</span></span>            | <span data-ttu-id="a59e9-119">Class</span><span class="sxs-lookup"><span data-stu-id="a59e9-119">Class</span></span>                | <span data-ttu-id="a59e9-120">События DOM и заметки</span><span class="sxs-lookup"><span data-stu-id="a59e9-120">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="a59e9-121">Буфер обмена</span><span class="sxs-lookup"><span data-stu-id="a59e9-121">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="a59e9-122">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="a59e9-122">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="a59e9-123">Переместить</span><span class="sxs-lookup"><span data-stu-id="a59e9-123">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="a59e9-124">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="a59e9-124">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="a59e9-125">`DataTransfer` и `DataTransferItem` содержать перетаскиваемые данные элемента.</span><span class="sxs-lookup"><span data-stu-id="a59e9-125">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="a59e9-126">Ошибка</span><span class="sxs-lookup"><span data-stu-id="a59e9-126">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="a59e9-127">Событие</span><span class="sxs-lookup"><span data-stu-id="a59e9-127">Event</span></span>            | `EventArgs`          | <span data-ttu-id="a59e9-128">*Общие сведения*</span><span class="sxs-lookup"><span data-stu-id="a59e9-128">*General*</span></span><br><span data-ttu-id="a59e9-129">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="a59e9-129">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="a59e9-130">*Буфер обмена*</span><span class="sxs-lookup"><span data-stu-id="a59e9-130">*Clipboard*</span></span><br><span data-ttu-id="a59e9-131">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="a59e9-131">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="a59e9-132">*Ввод*</span><span class="sxs-lookup"><span data-stu-id="a59e9-132">*Input*</span></span><br><span data-ttu-id="a59e9-133">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="a59e9-133">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="a59e9-134">*Носитель*</span><span class="sxs-lookup"><span data-stu-id="a59e9-134">*Media*</span></span><br><span data-ttu-id="a59e9-135">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="a59e9-135">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="a59e9-136">Focus</span><span class="sxs-lookup"><span data-stu-id="a59e9-136">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="a59e9-137">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="a59e9-137">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="a59e9-138">Не включает поддержку для `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="a59e9-138">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="a59e9-139">Входные данные</span><span class="sxs-lookup"><span data-stu-id="a59e9-139">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="a59e9-140">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="a59e9-140">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="a59e9-141">Клавиатура</span><span class="sxs-lookup"><span data-stu-id="a59e9-141">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="a59e9-142">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="a59e9-142">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="a59e9-143">Мышь</span><span class="sxs-lookup"><span data-stu-id="a59e9-143">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="a59e9-144">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="a59e9-144">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="a59e9-145">Указатель мыши</span><span class="sxs-lookup"><span data-stu-id="a59e9-145">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="a59e9-146">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="a59e9-146">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="a59e9-147">Колесо мыши</span><span class="sxs-lookup"><span data-stu-id="a59e9-147">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="a59e9-148">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="a59e9-148">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="a59e9-149">Ход выполнения</span><span class="sxs-lookup"><span data-stu-id="a59e9-149">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="a59e9-150">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="a59e9-150">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="a59e9-151">Сенсорные технологии</span><span class="sxs-lookup"><span data-stu-id="a59e9-151">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="a59e9-152">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="a59e9-152">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="a59e9-153">`TouchPoint` представляет одну точку контакта на устройстве с сенсорным вводом.</span><span class="sxs-lookup"><span data-stu-id="a59e9-153">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="a59e9-154">Для получения дополнительных сведений см. следующие ресурсы:</span><span class="sxs-lookup"><span data-stu-id="a59e9-154">For more information, see the following resources:</span></span>

* <span data-ttu-id="a59e9-155">[Классы EventArgs в источнике ссылки на ASP.NET Core (ветвь DotNet/aspnetcore Release/3.1)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="a59e9-155">[EventArgs classes in the ASP.NET Core reference source (dotnet/aspnetcore release/3.1 branch)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span></span>
* <span data-ttu-id="a59e9-156">[Веб-документы MDN: глобалевенсандлерс](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; содержит сведения о том, какие элементы HTML поддерживают каждое событие DOM.</span><span class="sxs-lookup"><span data-stu-id="a59e9-156">[MDN web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; Includes information on which HTML elements support each DOM event.</span></span>

## <a name="lambda-expressions"></a><span data-ttu-id="a59e9-157">Лямбда-выражения</span><span class="sxs-lookup"><span data-stu-id="a59e9-157">Lambda expressions</span></span>

<span data-ttu-id="a59e9-158">[Лямбда-выражения](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) также могут использоваться:</span><span class="sxs-lookup"><span data-stu-id="a59e9-158">[Lambda expressions](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="a59e9-159">Часто бывает удобно закрывать дополнительные значения, например при переборе набора элементов.</span><span class="sxs-lookup"><span data-stu-id="a59e9-159">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="a59e9-160">В следующем примере создаются три кнопки, каждый из которых вызывает `UpdateHeading` передачи аргумента события (`MouseEventArgs`) и номера кнопки (`buttonNumber`) при выборе в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="a59e9-160">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```razor
<h2>@_message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string _message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        _message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="a59e9-161">**Не** используйте переменную цикла (`i`) в цикле `for` непосредственно в лямбда-выражении.</span><span class="sxs-lookup"><span data-stu-id="a59e9-161">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="a59e9-162">В противном случае одна и та же переменная используется во всех лямбда-выражениях, в результате чего значение `i`будет одинаковым во всех лямбдаах.</span><span class="sxs-lookup"><span data-stu-id="a59e9-162">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="a59e9-163">Всегда запишите свое значение в локальную переменную (`buttonNumber` в предыдущем примере), а затем используйте ее.</span><span class="sxs-lookup"><span data-stu-id="a59e9-163">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

## <a name="eventcallback"></a><span data-ttu-id="a59e9-164">Вложенный EventCallback</span><span class="sxs-lookup"><span data-stu-id="a59e9-164">EventCallback</span></span>

<span data-ttu-id="a59e9-165">Распространенным сценарием с вложенными компонентами является желание запускать метод родительского компонента при возникновении события дочернего компонента&mdash;например, когда в дочернем элементе возникает событие `onclick`.</span><span class="sxs-lookup"><span data-stu-id="a59e9-165">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="a59e9-166">Чтобы обеспечить доступ к событиям по компонентам, используйте `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="a59e9-166">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="a59e9-167">Родительский компонент может назначить метод обратного вызова `EventCallback`у дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="a59e9-167">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="a59e9-168">`ChildComponent` в примере приложения (*Components/чилдкомпонент. Razor*) демонстрирует настройку обработчика `onclick`а кнопки на получение делегата `EventCallback` из `ParentComponent`в примере.</span><span class="sxs-lookup"><span data-stu-id="a59e9-168">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="a59e9-169">`EventCallback` вводится `MouseEventArgs`, который подходит для события `onclick` на периферийном устройстве.</span><span class="sxs-lookup"><span data-stu-id="a59e9-169">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="a59e9-170">`ParentComponent` задает для дочернего `EventCallback<T>` (`OnClickCallback`) его метод `ShowMessage`.</span><span class="sxs-lookup"><span data-stu-id="a59e9-170">The `ParentComponent` sets the child's `EventCallback<T>` (`OnClickCallback`) to its `ShowMessage` method.</span></span>

<span data-ttu-id="a59e9-171">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a59e9-171">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@_messageText</b></p>

@code {
    private string _messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        _messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

<span data-ttu-id="a59e9-172">При выборе кнопки в `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="a59e9-172">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="a59e9-173">Вызывается метод `ShowMessage` `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="a59e9-173">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="a59e9-174">`_messageText` обновляется и отображается в `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="a59e9-174">`_messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="a59e9-175">Вызов [статехасчанжед](xref:blazor/lifecycle#state-changes) не требуется в методе обратного вызова (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="a59e9-175">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="a59e9-176">`StateHasChanged` вызывается автоматически для реотрисовки `ParentComponent`, так же как и дочерние события. компонент активируется в обработчиках событий, которые выполняются внутри дочернего элемента.</span><span class="sxs-lookup"><span data-stu-id="a59e9-176">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="a59e9-177">`EventCallback` и `EventCallback<T>` разрешается выполнять асинхронные делегаты.</span><span class="sxs-lookup"><span data-stu-id="a59e9-177">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="a59e9-178">`EventCallback<T>` является строго типизированным и требует определенного типа аргумента.</span><span class="sxs-lookup"><span data-stu-id="a59e9-178">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="a59e9-179">`EventCallback` слабо типизирован и допускает любой тип аргумента.</span><span class="sxs-lookup"><span data-stu-id="a59e9-179">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

<span data-ttu-id="a59e9-180">Вызов `EventCallback` или `EventCallback<T>` с `InvokeAsync` и ожидание <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="a59e9-180">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="a59e9-181">Используйте `EventCallback` и `EventCallback<T>` для обработки событий и параметров компонента привязки.</span><span class="sxs-lookup"><span data-stu-id="a59e9-181">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="a59e9-182">Предпочитать строго типизированный `EventCallback<T>` поверх `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="a59e9-182">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="a59e9-183">`EventCallback<T>` предоставляет более лучшую реакцию на ошибки для пользователей компонента.</span><span class="sxs-lookup"><span data-stu-id="a59e9-183">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="a59e9-184">Как и в случае с другими обработчиками событий пользовательского интерфейса, указание параметра события является необязательным.</span><span class="sxs-lookup"><span data-stu-id="a59e9-184">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="a59e9-185">Используйте `EventCallback`, если не передается значение обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="a59e9-185">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="prevent-default-actions"></a><span data-ttu-id="a59e9-186">Запретить действия по умолчанию</span><span class="sxs-lookup"><span data-stu-id="a59e9-186">Prevent default actions</span></span>

<span data-ttu-id="a59e9-187">Используйте атрибут директивы [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) , чтобы предотвратить действие по умолчанию для события.</span><span class="sxs-lookup"><span data-stu-id="a59e9-187">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="a59e9-188">Если ключ выбран на устройстве ввода, а элемент находится в текстовом поле, то в браузере обычно отображается символ ключа в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="a59e9-188">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="a59e9-189">В следующем примере поведение по умолчанию запрещено путем указания атрибута директивы `@onkeypress:preventDefault`.</span><span class="sxs-lookup"><span data-stu-id="a59e9-189">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="a59e9-190">Счетчик увеличивается, а ключ **+** не записывается в значение элемента `<input>`:</span><span class="sxs-lookup"><span data-stu-id="a59e9-190">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />

@code {
    private int _count = 0;

    private void KeyHandler(KeyboardEventArgs e)
    {
        if (e.Key == "+")
        {
            _count++;
        }
    }
}
```

<span data-ttu-id="a59e9-191">Указание `@on{EVENT}:preventDefault` атрибута без значения эквивалентно `@on{EVENT}:preventDefault="true"`.</span><span class="sxs-lookup"><span data-stu-id="a59e9-191">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="a59e9-192">Значение атрибута также может быть выражением.</span><span class="sxs-lookup"><span data-stu-id="a59e9-192">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="a59e9-193">В следующем примере `_shouldPreventDefault` является полем `bool`, для которого задано значение `true` или `false`:</span><span class="sxs-lookup"><span data-stu-id="a59e9-193">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="a59e9-194">Для предотвращения действия по умолчанию не требуется обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="a59e9-194">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="a59e9-195">Обработчик событий и предотвращение сценариев действий по умолчанию можно использовать независимо друг от друга.</span><span class="sxs-lookup"><span data-stu-id="a59e9-195">The event handler and prevent default action scenarios can be used independently.</span></span>

## <a name="stop-event-propagation"></a><span data-ttu-id="a59e9-196">Отключить распространение событий</span><span class="sxs-lookup"><span data-stu-id="a59e9-196">Stop event propagation</span></span>

<span data-ttu-id="a59e9-197">Используйте атрибут директивы [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) для отмены распространения событий.</span><span class="sxs-lookup"><span data-stu-id="a59e9-197">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="a59e9-198">В следующем примере при установке флажка события Click из второго дочернего `<div>` не распространяются на родительский `<div>`:</span><span class="sxs-lookup"><span data-stu-id="a59e9-198">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
<label>
    <input @bind="_stopPropagation" type="checkbox" />
    Stop Propagation
</label>

<div @onclick="OnSelectParentDiv">
    <h3>Parent div</h3>

    <div @onclick="OnSelectChildDiv">
        Child div that doesn't stop propagation when selected.
    </div>

    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        Child div that stops propagation when selected.
    </div>
</div>

@code {
    private bool _stopPropagation = false;

    private void OnSelectParentDiv() => 
        Console.WriteLine($"The parent div was selected. {DateTime.Now}");
    private void OnSelectChildDiv() => 
        Console.WriteLine($"A child div was selected. {DateTime.Now}");
}
```
