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
# <a name="aspnet-core-blazor-event-handling"></a>Обработка событий ASP.NET Core Блазор

[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)

Компоненты Razor предоставляют функции обработки событий. Для атрибута HTML-элемента с именем `on{EVENT}` (например, `onclick` и `onsubmit`) со значением, вводимым с помощью делегата, компоненты Razor рассматривают значение атрибута как обработчик событий. Имя атрибута всегда имеет формат [`@on{EVENT}`](xref:mvc/views/razor#onevent).

Следующий код вызывает метод `UpdateHeading`, когда кнопка выбрана в пользовательском интерфейсе:

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

Следующий код вызывает метод `CheckChanged` при изменении флажка в пользовательском интерфейсе:

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

Обработчики событий также могут быть асинхронными и возвращать <xref:System.Threading.Tasks.Task>. Нет необходимости вручную вызывать [статехасчанжед](xref:blazor/lifecycle#state-changes). Исключения регистрируются при их возникновении.

В следующем примере `UpdateHeading` вызывается асинхронно при выборе кнопки:

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

## <a name="event-argument-types"></a>Типы аргументов событий

Для некоторых событий разрешены типы аргументов событий. Указание типа события в вызове метода требуется только в том случае, если тип события используется в методе.

Поддерживаемые `EventArgs` приведены в следующей таблице.

| Событие            | Class                | События DOM и заметки |
| ---------------- | -------------------- | -------------------- |
| Буфер обмена        | `ClipboardEventArgs` | `oncut`, `oncopy`, `onpaste` |
| Переместить             | `DragEventArgs`      | `ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`<br><br>`DataTransfer` и `DataTransferItem` содержать перетаскиваемые данные элемента. |
| Ошибка            | `ErrorEventArgs`     | `onerror` |
| Событие            | `EventArgs`          | *Общие сведения*<br>`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`<br><br>*Буфер обмена*<br>`onbeforecut`, `onbeforecopy`, `onbeforepaste`<br><br>*Ввод*<br>`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`<br><br>*Носитель*<br>`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting` |
| Focus            | `FocusEventArgs`     | `onfocus`, `onblur`, `onfocusin`, `onfocusout`<br><br>Не включает поддержку для `relatedTarget`. |
| Входные данные            | `ChangeEventArgs`    | `onchange`, `oninput` |
| Клавиатура         | `KeyboardEventArgs`  | `onkeydown`, `onkeypress`, `onkeyup` |
| Мышь            | `MouseEventArgs`     | `onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout` |
| Указатель мыши    | `PointerEventArgs`   | `onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture` |
| Колесо мыши      | `WheelEventArgs`     | `onwheel`, `onmousewheel` |
| Ход выполнения         | `ProgressEventArgs`  | `onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout` |
| Сенсорные технологии            | `TouchEventArgs`     | `ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`<br><br>`TouchPoint` представляет одну точку контакта на устройстве с сенсорным вводом. |

Для получения дополнительных сведений см. следующие ресурсы:

* [Классы EventArgs в источнике ссылки на ASP.NET Core (ветвь DotNet/aspnetcore Release/3.1)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).
* [Веб-документы MDN: глобалевенсандлерс](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; содержит сведения о том, какие элементы HTML поддерживают каждое событие DOM.

## <a name="lambda-expressions"></a>Лямбда-выражения

[Лямбда-выражения](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) также могут использоваться:

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Часто бывает удобно закрывать дополнительные значения, например при переборе набора элементов. В следующем примере создаются три кнопки, каждый из которых вызывает `UpdateHeading` передачи аргумента события (`MouseEventArgs`) и номера кнопки (`buttonNumber`) при выборе в пользовательском интерфейсе:

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
> **Не** используйте переменную цикла (`i`) в цикле `for` непосредственно в лямбда-выражении. В противном случае одна и та же переменная используется во всех лямбда-выражениях, в результате чего значение `i`будет одинаковым во всех лямбдаах. Всегда запишите свое значение в локальную переменную (`buttonNumber` в предыдущем примере), а затем используйте ее.

## <a name="eventcallback"></a>Вложенный EventCallback

Распространенным сценарием с вложенными компонентами является желание запускать метод родительского компонента при возникновении события дочернего компонента&mdash;например, когда в дочернем элементе возникает событие `onclick`. Чтобы обеспечить доступ к событиям по компонентам, используйте `EventCallback`. Родительский компонент может назначить метод обратного вызова `EventCallback`у дочернего компонента.

`ChildComponent` в примере приложения (*Components/чилдкомпонент. Razor*) демонстрирует настройку обработчика `onclick`а кнопки на получение делегата `EventCallback` из `ParentComponent`в примере. `EventCallback` вводится `MouseEventArgs`, который подходит для события `onclick` на периферийном устройстве.

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

`ParentComponent` задает для дочернего `EventCallback<T>` (`OnClickCallback`) его метод `ShowMessage`.

*Pages/паренткомпонент. Razor*:

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

При выборе кнопки в `ChildComponent`:

* Вызывается метод `ShowMessage` `ParentComponent`. `_messageText` обновляется и отображается в `ParentComponent`.
* Вызов [статехасчанжед](xref:blazor/lifecycle#state-changes) не требуется в методе обратного вызова (`ShowMessage`). `StateHasChanged` вызывается автоматически для реотрисовки `ParentComponent`, так же как и дочерние события. компонент активируется в обработчиках событий, которые выполняются внутри дочернего элемента.

`EventCallback` и `EventCallback<T>` разрешается выполнять асинхронные делегаты. `EventCallback<T>` является строго типизированным и требует определенного типа аргумента. `EventCallback` слабо типизирован и допускает любой тип аргумента.

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

Вызов `EventCallback` или `EventCallback<T>` с `InvokeAsync` и ожидание <xref:System.Threading.Tasks.Task>:

```csharp
await callback.InvokeAsync(arg);
```

Используйте `EventCallback` и `EventCallback<T>` для обработки событий и параметров компонента привязки.

Предпочитать строго типизированный `EventCallback<T>` поверх `EventCallback`. `EventCallback<T>` предоставляет более лучшую реакцию на ошибки для пользователей компонента. Как и в случае с другими обработчиками событий пользовательского интерфейса, указание параметра события является необязательным. Используйте `EventCallback`, если не передается значение обратного вызова.

## <a name="prevent-default-actions"></a>Запретить действия по умолчанию

Используйте атрибут директивы [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) , чтобы предотвратить действие по умолчанию для события.

Если ключ выбран на устройстве ввода, а элемент находится в текстовом поле, то в браузере обычно отображается символ ключа в текстовом поле. В следующем примере поведение по умолчанию запрещено путем указания атрибута директивы `@onkeypress:preventDefault`. Счетчик увеличивается, а ключ **+** не записывается в значение элемента `<input>`:

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

Указание `@on{EVENT}:preventDefault` атрибута без значения эквивалентно `@on{EVENT}:preventDefault="true"`.

Значение атрибута также может быть выражением. В следующем примере `_shouldPreventDefault` является полем `bool`, для которого задано значение `true` или `false`:

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

Для предотвращения действия по умолчанию не требуется обработчик событий. Обработчик событий и предотвращение сценариев действий по умолчанию можно использовать независимо друг от друга.

## <a name="stop-event-propagation"></a>Отключить распространение событий

Используйте атрибут директивы [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) для отмены распространения событий.

В следующем примере при установке флажка события Click из второго дочернего `<div>` не распространяются на родительский `<div>`:

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
