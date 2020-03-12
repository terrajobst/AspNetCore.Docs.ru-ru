Компонент `App` (*app. Razor*) похож на компонент `App`, который находится в блазор Server Apps:

* Компонент `CascadingAuthenticationState` управляет предоставлением `AuthenticationState` остальным приложениям.
* Компонент `AuthorizeRouteView` гарантирует, что текущий пользователь имеет право доступа к определенной странице или иным образом отображает компонент `RedirectToLogin`.
* Компонент `RedirectToLogin` управляет перенаправлением неавторизованных пользователей на страницу входа.

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(Program).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData" 
                DefaultLayout="@typeof(MainLayout)">
                <NotAuthorized>
                    <RedirectToLogin />
                </NotAuthorized>
            </AuthorizeRouteView>
        </Found>
        <NotFound>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```
