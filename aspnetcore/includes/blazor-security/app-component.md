<span data-ttu-id="3cd01-101">Компонент `App` (*app. Razor*) похож на компонент `App`, который находится в блазор Server Apps:</span><span class="sxs-lookup"><span data-stu-id="3cd01-101">The `App` component (*App.razor*) is similar to the `App` component found in Blazor Server apps:</span></span>

* <span data-ttu-id="3cd01-102">Компонент `CascadingAuthenticationState` управляет предоставлением `AuthenticationState` остальным приложениям.</span><span class="sxs-lookup"><span data-stu-id="3cd01-102">The `CascadingAuthenticationState` component manages exposing the `AuthenticationState` to the rest of the app.</span></span>
* <span data-ttu-id="3cd01-103">Компонент `AuthorizeRouteView` гарантирует, что текущий пользователь имеет право доступа к определенной странице или иным образом отображает компонент `RedirectToLogin`.</span><span class="sxs-lookup"><span data-stu-id="3cd01-103">The `AuthorizeRouteView` component makes sure that the current user is authorized to access a given page or otherwise renders the `RedirectToLogin` component.</span></span>
* <span data-ttu-id="3cd01-104">Компонент `RedirectToLogin` управляет перенаправлением неавторизованных пользователей на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="3cd01-104">The `RedirectToLogin` component manages redirecting unauthorized users to the login page.</span></span>

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
