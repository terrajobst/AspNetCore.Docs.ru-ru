<span data-ttu-id="1f0d1-101">Страница, созданная компонентом `Authentication` (*pages/Authentication. Razor*), определяет маршруты, необходимые для обработки различных стадий проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="1f0d1-101">The page produced by the `Authentication` component (*Pages/Authentication.razor*) defines the routes required for handling different authentication stages.</span></span>

<span data-ttu-id="1f0d1-102">Компонент `RemoteAuthenticatorView`:</span><span class="sxs-lookup"><span data-stu-id="1f0d1-102">The `RemoteAuthenticatorView` component:</span></span>

* <span data-ttu-id="1f0d1-103">Предоставляется пакетом `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="1f0d1-103">Is provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span>
* <span data-ttu-id="1f0d1-104">Управляет выполнением соответствующих действий на каждом этапе проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="1f0d1-104">Manages performing the appropriate actions at each stage of authentication.</span></span>

```razor
@page "/authentication/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code {
    [Parameter]
    public string Action { get; set; }
}
```
