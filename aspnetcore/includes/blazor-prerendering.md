<span data-ttu-id="8a8f8-101">При предварительном отображении серверного приложения Блазор некоторые действия, такие как вызов JavaScript, не возможны, так как соединение с браузером не установлено.</span><span class="sxs-lookup"><span data-stu-id="8a8f8-101">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="8a8f8-102">При предварительной отрисовке компонентов может потребоваться прорисовка по-разному.</span><span class="sxs-lookup"><span data-stu-id="8a8f8-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="8a8f8-103">Чтобы отложить вызовы взаимодействия JavaScript до тех пор, пока не будет установлено подключение к браузеру, можно использовать событие жизненного цикла компонента `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="8a8f8-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="8a8f8-104">Это событие вызывается только после того, как приложение будет полностью визуализировано и установлено клиентское соединение.</span><span class="sxs-lookup"><span data-stu-id="8a8f8-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

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
            JSRuntime.InvokeVoidAsync(
                "setElementValue", myInput, "Value set after render");
        }
    }
}
```

<span data-ttu-id="8a8f8-105">В следующем компоненте показано, как использовать взаимодействие JavaScript как часть логики инициализации компонента способом, совместимым с предварительной отрисовкой.</span><span class="sxs-lookup"><span data-stu-id="8a8f8-105">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="8a8f8-106">Компонент показывает, что можно активировать обновление отрисовки из `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="8a8f8-106">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="8a8f8-107">В этом сценарии разработчику следует избегать создания бесконечного цикла.</span><span class="sxs-lookup"><span data-stu-id="8a8f8-107">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="8a8f8-108">При вызове `JSRuntime.InvokeAsync` `ElementRef` используется только в `OnAfterRenderAsync`, а не в каком-либо предыдущем методе жизненного цикла, так как нет элемента JavaScript до тех пор, пока не будет визуализирован компонент.</span><span class="sxs-lookup"><span data-stu-id="8a8f8-108">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="8a8f8-109">`StateHasChanged` вызывается для реотрисовки компонента с новым состоянием, полученным из вызова взаимодействия JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8a8f8-109">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="8a8f8-110">Код не создает бесконечный цикл, так как `StateHasChanged` вызывается только при `null` `infoFromJs`.</span><span class="sxs-lookup"><span data-stu-id="8a8f8-110">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

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
