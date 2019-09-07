<span data-ttu-id="9632f-101">Хотя Блазор приложение на стороне сервера является предварительной отрисовкой, некоторые действия, такие как вызов JavaScript, не возможны, так как соединение с браузером не установлено.</span><span class="sxs-lookup"><span data-stu-id="9632f-101">While a Blazor server-side app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="9632f-102">При предварительной отрисовке компонентов может потребоваться прорисовка по-разному.</span><span class="sxs-lookup"><span data-stu-id="9632f-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="9632f-103">Чтобы отложить вызовы взаимодействия JavaScript до тех пор, пока не будет установлено соединение с браузером, `OnAfterRenderAsync` можно использовать событие жизненный цикл компонента.</span><span class="sxs-lookup"><span data-stu-id="9632f-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="9632f-104">Это событие вызывается только после того, как приложение будет полностью визуализировано и установлено клиентское соединение.</span><span class="sxs-lookup"><span data-stu-id="9632f-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

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

<span data-ttu-id="9632f-105">В следующем компоненте показано, как использовать взаимодействие JavaScript как часть логики инициализации компонента способом, совместимым с предварительной отрисовкой.</span><span class="sxs-lookup"><span data-stu-id="9632f-105">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="9632f-106">Компонент показывает, что можно активировать обновление отрисовки из `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="9632f-106">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="9632f-107">В этом сценарии разработчику следует избегать создания бесконечного цикла.</span><span class="sxs-lookup"><span data-stu-id="9632f-107">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="9632f-108">Метод `JSRuntime.InvokeAsync` , где вызывается, используется только `OnAfterRenderAsync` в, а не в любом из предыдущих методов жизненного цикла, `ElementRef` поскольку нет элемента JavaScript до тех пор, пока не будет визуализирован компонент.</span><span class="sxs-lookup"><span data-stu-id="9632f-108">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="9632f-109">`StateHasChanged`вызывается для реотрисовки компонента с новым состоянием, полученным из вызова взаимодействия JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9632f-109">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="9632f-110">Код не создает бесконечный цикл, так `StateHasChanged` как вызывается только `infoFromJs` тогда `null`, когда имеет значение.</span><span class="sxs-lookup"><span data-stu-id="9632f-110">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

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

<span data-ttu-id="9632f-111">Для условного отображения другого содержимого в зависимости от того, является ли приложение предварительно отображаемым содержимым `IsConnected` , используйте свойство `IComponentContext` в службе.</span><span class="sxs-lookup"><span data-stu-id="9632f-111">To conditionally render different content based on whether the app is currently prerendering content, use the `IsConnected` property on the `IComponentContext` service.</span></span> <span data-ttu-id="9632f-112">При выполнении на стороне сервера возвращает `IsConnected` `true` , только если имеется активное соединение с клиентом.</span><span class="sxs-lookup"><span data-stu-id="9632f-112">When running server-side, `IsConnected` only returns `true` if there's an active connection to the client.</span></span> <span data-ttu-id="9632f-113">Он всегда возвращает `true` значение при запуске на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="9632f-113">It always returns `true` when running client-side.</span></span>

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
