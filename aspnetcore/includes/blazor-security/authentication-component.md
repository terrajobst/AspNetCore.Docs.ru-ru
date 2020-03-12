Страница, созданная компонентом `Authentication` (*pages/Authentication. Razor*), определяет маршруты, необходимые для обработки различных стадий проверки подлинности.

Компонент `RemoteAuthenticatorView`:

* Предоставляется пакетом `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.
* Управляет выполнением соответствующих действий на каждом этапе проверки подлинности.

```razor
@page "/authentication/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code {
    [Parameter]
    public string Action { get; set; }
}
```
