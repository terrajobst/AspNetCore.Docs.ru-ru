При предварительном отображении серверного приложения Блазор некоторые действия, такие как вызов JavaScript, не возможны, так как соединение с браузером не установлено. При предварительной отрисовке компонентов может потребоваться прорисовка по-разному.

Чтобы отложить вызовы взаимодействия JavaScript до тех пор, пока не будет установлено соединение с браузером, `OnAfterRenderAsync` можно использовать событие жизненный цикл компонента. Это событие вызывается только после того, как приложение будет полностью визуализировано и установлено клиентское соединение.

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input @ref="myInput" value="Value set during render" />

@code {
    private ElementReference myInput;

    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            JSRuntime.InvokeAsync<object>(
                "setElementValue", myInput, "Value set after render");
        }
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
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

<p>
    Set value via JS interop call:
    <input id="val-set-by-interop" @ref="myElem" />
</p>

@code {
    private string infoFromJs;
    private ElementReference myElem;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender && infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementValue", myElem, "Hello from interop call");

            StateHasChanged();
        }
    }
}
```