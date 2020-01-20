---
no-loc:
- Blazor
- SignalR
ms.openlocfilehash: 5f3e22e04fe18149ec5a8acb42f42a8ef83a7664
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159910"
---
<span data-ttu-id="af5d0-101">Во время предварительной отрисовки Blazor серверного приложения некоторые действия, такие как вызов JavaScript, не возможны, так как соединение с браузером не установлено.</span><span class="sxs-lookup"><span data-stu-id="af5d0-101">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="af5d0-102">При предварительной отрисовке компонентов может потребоваться прорисовка по-разному.</span><span class="sxs-lookup"><span data-stu-id="af5d0-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="af5d0-103">Чтобы отложить вызовы взаимодействия JavaScript до тех пор, пока не будет установлено подключение к браузеру, можно использовать [событие жизненного цикла компонента онафтеррендерасинк](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="af5d0-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the [OnAfterRenderAsync component lifecycle event](xref:blazor/lifecycle#after-component-render).</span></span> <span data-ttu-id="af5d0-104">Это событие вызывается только после того, как приложение будет полностью визуализировано и установлено клиентское соединение.</span><span class="sxs-lookup"><span data-stu-id="af5d0-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<div @ref="divElement">Text during render</div>

@code {
    private ElementReference divElement;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            await JSRuntime.InvokeVoidAsync(
                "setElementText", divElement, "Text after render");
        }
    }
}
```

<span data-ttu-id="af5d0-105">В предыдущем примере кода укажите `setElementText` функцию JavaScript в элементе `<head>` *wwwroot/index.HTML* (Blazor Assembly) или *pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="af5d0-105">For the preceding example code, provide a `setElementText` JavaScript function inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> <span data-ttu-id="af5d0-106">Функция вызывается с `IJSRuntime.InvokeVoidAsync` и не возвращает значение:</span><span class="sxs-lookup"><span data-stu-id="af5d0-106">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

```html
<script>
  window.setElementText = (element, text) => element.innerText = text;
</script>
```

> [!WARNING]
> <span data-ttu-id="af5d0-107">В предыдущем примере модель DOM (модель DOM) изменяется непосредственно только в демонстрационных целях.</span><span class="sxs-lookup"><span data-stu-id="af5d0-107">The preceding example modifies the Document Object Model (DOM) directly for demonstration purposes only.</span></span> <span data-ttu-id="af5d0-108">Непосредственное изменение модели DOM с помощью JavaScript не рекомендуется в большинстве сценариев, поскольку JavaScript может мешать отслеживанию изменений Blazor.</span><span class="sxs-lookup"><span data-stu-id="af5d0-108">Directly modifying the DOM with JavaScript isn't recommended in most scenarios because JavaScript can interfere with Blazor's change tracking.</span></span>

<span data-ttu-id="af5d0-109">В следующем компоненте показано, как использовать взаимодействие JavaScript как часть логики инициализации компонента способом, совместимым с предварительной отрисовкой.</span><span class="sxs-lookup"><span data-stu-id="af5d0-109">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="af5d0-110">Компонент показывает, что можно активировать обновление отрисовки из `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="af5d0-110">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="af5d0-111">В этом сценарии разработчику следует избегать создания бесконечного цикла.</span><span class="sxs-lookup"><span data-stu-id="af5d0-111">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="af5d0-112">При вызове `JSRuntime.InvokeAsync` `ElementRef` используется только в `OnAfterRenderAsync`, а не в каком-либо предыдущем методе жизненного цикла, так как нет элемента JavaScript до тех пор, пока не будет визуализирован компонент.</span><span class="sxs-lookup"><span data-stu-id="af5d0-112">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="af5d0-113">[Статехасчанжед](xref:blazor/lifecycle#state-changes) вызывается для реотрисовки компонента с новым состоянием, полученным из вызова взаимодействия JavaScript.</span><span class="sxs-lookup"><span data-stu-id="af5d0-113">[StateHasChanged](xref:blazor/lifecycle#state-changes) is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="af5d0-114">Код не создает бесконечный цикл, так как `StateHasChanged` вызывается только при `null``infoFromJs`.</span><span class="sxs-lookup"><span data-stu-id="af5d0-114">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

Set value via JS interop call:
<div id="val-set-by-interop" @ref="divElement"></div>

@code {
    private string infoFromJs;
    private ElementReference divElement;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender && infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementText", divElement, "Hello from interop call!");

            StateHasChanged();
        }
    }
}
```

<span data-ttu-id="af5d0-115">В предыдущем примере кода укажите `setElementText` функцию JavaScript в элементе `<head>` *wwwroot/index.HTML* (Blazor Assembly) или *pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="af5d0-115">For the preceding example code, provide a `setElementText` JavaScript function inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> <span data-ttu-id="af5d0-116">Функция вызывается с `IJSRuntime.InvokeAsync` и возвращает значение:</span><span class="sxs-lookup"><span data-stu-id="af5d0-116">The function is called with `IJSRuntime.InvokeAsync` and returns a value:</span></span>

```html
<script>
  window.setElementText = (element, text) => {
    element.innerText = text;
    return text;
  };
</script>
```

> [!WARNING]
> <span data-ttu-id="af5d0-117">В предыдущем примере модель DOM (модель DOM) изменяется непосредственно только в демонстрационных целях.</span><span class="sxs-lookup"><span data-stu-id="af5d0-117">The preceding example modifies the Document Object Model (DOM) directly for demonstration purposes only.</span></span> <span data-ttu-id="af5d0-118">Непосредственное изменение модели DOM с помощью JavaScript не рекомендуется в большинстве сценариев, поскольку JavaScript может мешать отслеживанию изменений Blazor.</span><span class="sxs-lookup"><span data-stu-id="af5d0-118">Directly modifying the DOM with JavaScript isn't recommended in most scenarios because JavaScript can interfere with Blazor's change tracking.</span></span>
