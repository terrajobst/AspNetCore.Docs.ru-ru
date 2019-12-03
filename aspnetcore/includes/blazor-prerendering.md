При предварительном отображении серверного приложения Блазор некоторые действия, такие как вызов JavaScript, не возможны, так как соединение с браузером не установлено. При предварительной отрисовке компонентов может потребоваться прорисовка по-разному.

Чтобы отложить вызовы взаимодействия JavaScript до тех пор, пока не будет установлено подключение к браузеру, можно использовать [событие жизненного цикла компонента онафтеррендерасинк](xref:blazor/lifecycle#after-component-render). Это событие вызывается только после того, как приложение будет полностью визуализировано и установлено клиентское соединение.

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

В предыдущем примере кода укажите `setElementText` функцию JavaScript в элементе `<head>` *wwwroot/index.HTML* (блазор Assembly) или *pages/_Host. cshtml* (блазор Server). Функция вызывается с `IJSRuntime.InvokeVoidAsync` и не возвращает значение:

```html
<script>
  window.setElementText = (element, text) => element.innerText = text;
</script>
```

> [!WARNING]
> В предыдущем примере модель DOM (модель DOM) изменяется непосредственно только в демонстрационных целях. Непосредственное изменение модели DOM с помощью JavaScript не рекомендуется в большинстве сценариев, поскольку JavaScript может мешать отслеживанию изменений Блазор.

В следующем компоненте показано, как использовать взаимодействие JavaScript как часть логики инициализации компонента способом, совместимым с предварительной отрисовкой. Компонент показывает, что можно активировать обновление отрисовки из `OnAfterRenderAsync`. В этом сценарии разработчику следует избегать создания бесконечного цикла.

При вызове `JSRuntime.InvokeAsync` `ElementRef` используется только в `OnAfterRenderAsync`, а не в каком-либо предыдущем методе жизненного цикла, так как нет элемента JavaScript до тех пор, пока не будет визуализирован компонент.

[Статехасчанжед](xref:blazor/lifecycle#state-changes) вызывается для реотрисовки компонента с новым состоянием, полученным из вызова взаимодействия JavaScript. Код не создает бесконечный цикл, так как `StateHasChanged` вызывается только при `null``infoFromJs`.

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

В предыдущем примере кода укажите `setElementText` функцию JavaScript в элементе `<head>` *wwwroot/index.HTML* (блазор Assembly) или *pages/_Host. cshtml* (блазор Server). Функция вызывается с `IJSRuntime.InvokeAsync` и возвращает значение:

```html
<script>
  window.setElementText = (element, text) => {
    element.innerText = text;
    return text;
  };
</script>
```

> [!WARNING]
> В предыдущем примере модель DOM (модель DOM) изменяется непосредственно только в демонстрационных целях. Непосредственное изменение модели DOM с помощью JavaScript не рекомендуется в большинстве сценариев, поскольку JavaScript может мешать отслеживанию изменений Блазор.
