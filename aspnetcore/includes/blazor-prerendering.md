Хотя Блазор приложение на стороне сервера является предварительной отрисовкой, некоторые действия, такие как вызов JavaScript, не возможны, так как соединение с браузером не установлено. При предварительной отрисовке компонентов может потребоваться прорисовка по-разному.

Чтобы отложить вызовы взаимодействия JavaScript до тех пор, пока не будет установлено соединение с браузером, `OnAfterRenderAsync` можно использовать событие жизненный цикл компонента. Это событие вызывается только после того, как приложение будет полностью визуализировано и установлено клиентское соединение.

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input @ref="myInput" @ref:suppressField value="Value set during render" />

@code {
    private ElementReference myInput;

    protected override void OnAfterRender()
    {
        JSRuntime.InvokeAsync<object>(
            "setElementValue", myInput, "Value set after render");
    }
}
```

В следующем компоненте показано, как использовать взаимодействие JavaScript как часть логики инициализации компонента способом, совместимым с предварительной отрисовкой. Компонент показывает, что можно активировать обновление отрисовки из `OnAfterRenderAsync`. В этом сценарии разработчику следует избегать создания бесконечного цикла.

Метод `JSRuntime.InvokeAsync` , где вызывается, используется только `OnAfterRenderAsync` в, а не в любом из предыдущих методов жизненного цикла, `ElementRef` поскольку нет элемента JavaScript до тех пор, пока не будет визуализирован компонент.

`StateHasChanged`вызывается для реотрисовки компонента с новым состоянием, полученным из вызова взаимодействия JavaScript. Код не создает бесконечный цикл, так `StateHasChanged` как вызывается только `infoFromJs` тогда `null`, когда имеет значение.

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IComponentContext ComponentContext
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

<p>
    Set value via JS interop call:
    <input id="val-set-by-interop" @ref="myElem" @ref:suppressField />
</p>

@code {
    private string infoFromJs;
    private ElementReference myElem;

    protected override async Task OnAfterRenderAsync()
    {
        // TEMPORARY: Currently we need this guard to avoid making the interop
        // call during prerendering. Soon this will be unnecessary because we
        // will change OnAfterRenderAsync so that it won't run during the
        // prerendering phase.
        if (!ComponentContext.IsConnected)
        {
            return;
        }

        if (infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementValue", myElem, "Hello from interop call");

            StateHasChanged();
        }
    }
}
```

Для условного отображения другого содержимого в зависимости от того, является ли приложение предварительно отображаемым содержимым `IsConnected` , используйте свойство `IComponentContext` в службе. При выполнении на стороне сервера возвращает `IsConnected` `true` , только если имеется активное соединение с клиентом. Он всегда возвращает `true` значение при запуске на стороне клиента.

```cshtml
@page "/isconnected-example"
@using Microsoft.AspNetCore.Components.Services
@inject IComponentContext ComponentContext

<h1>IsConnected Example</h1>

<p>
    Current state:
    <strong id="connected-state">
        @(ComponentContext.IsConnected ? "connected" : "not connected")
    </strong>
</p>

<p>
    Clicks:
    <strong id="count">@count</strong>
    <button id="increment-count" @onclick="@(() => count++)">Click me</button>
</p>

@code {
    private int count;
}
```
