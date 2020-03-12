<span data-ttu-id="4337a-101">Компонент `FetchData` показывает, как:</span><span class="sxs-lookup"><span data-stu-id="4337a-101">The `FetchData` component shows how to:</span></span>

* <span data-ttu-id="4337a-102">Подготавливает маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="4337a-102">Provision an access token.</span></span>
* <span data-ttu-id="4337a-103">Используйте маркер доступа для вызова API защищенных ресурсов в *серверном* приложении.</span><span class="sxs-lookup"><span data-stu-id="4337a-103">Use the access token to call a protected resource API in the *Server* app.</span></span>

<span data-ttu-id="4337a-104">Директива `@attribute [Authorize]` указывает системе авторизации веб – сборки Блазор, что пользователь должен быть авторизован для посещения этого компонента.</span><span class="sxs-lookup"><span data-stu-id="4337a-104">The `@attribute [Authorize]` directive indicates to the Blazor WebAssembly authorization system that the user must be authorized in order to visit this component.</span></span> <span data-ttu-id="4337a-105">Наличие атрибута в *клиентском* приложении не мешает вызову API на сервере без соответствующих учетных данных.</span><span class="sxs-lookup"><span data-stu-id="4337a-105">The presence of the attribute in the *Client* app doesn't prevent the API on the server from being called without proper credentials.</span></span> <span data-ttu-id="4337a-106">*Серверное* приложение также должно использовать `[Authorize]` на соответствующих конечных точках, чтобы правильно защитить их.</span><span class="sxs-lookup"><span data-stu-id="4337a-106">The *Server* app also must use `[Authorize]` on the appropriate endpoints to correctly protect them.</span></span>

<span data-ttu-id="4337a-107">`AuthenticationService.RequestAccessToken();` позаботится о запросе маркера доступа, который можно добавить в запрос для вызова API.</span><span class="sxs-lookup"><span data-stu-id="4337a-107">`AuthenticationService.RequestAccessToken();` takes care of requesting an access token that can be added to the request to call the API.</span></span> <span data-ttu-id="4337a-108">Если маркер кэшируется или служба может подготавливать новый маркер доступа без вмешательства пользователя, запрос маркера будет выполнен.</span><span class="sxs-lookup"><span data-stu-id="4337a-108">If the token is cached or the service is able to provision a new access token without user interaction, the token request succeeds.</span></span> <span data-ttu-id="4337a-109">В противном случае запрос маркера завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="4337a-109">Otherwise, the token request fails.</span></span>

<span data-ttu-id="4337a-110">Чтобы получить фактический токен для включения в запрос, приложение должно проверить, что запрос был обработан, вызвав `tokenResult.TryGetToken(out var token)`.</span><span class="sxs-lookup"><span data-stu-id="4337a-110">In order to obtain the actual token to include in the request, the app must check that the request succeeded by calling `tokenResult.TryGetToken(out var token)`.</span></span> 

<span data-ttu-id="4337a-111">Если запрос был успешным, переменная маркера заполняется маркером доступа.</span><span class="sxs-lookup"><span data-stu-id="4337a-111">If the request was successful, the token variable is populated with the access token.</span></span> <span data-ttu-id="4337a-112">Свойство `Value` маркера предоставляет строку литерала для включения в заголовок запроса `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="4337a-112">The `Value` property of the token exposes the literal string to include in the `Authorization` request header.</span></span>

<span data-ttu-id="4337a-113">Если запрос завершился ошибкой, так как не удалось подготовить маркер без взаимодействия с пользователем, результат маркера содержит URL-адрес перенаправления.</span><span class="sxs-lookup"><span data-stu-id="4337a-113">If the request failed because the token couldn't be provisioned without user interaction, the token result contains a redirect URL.</span></span> <span data-ttu-id="4337a-114">При переходе по этому URL-адресу пользователь переходит на страницу входа и возвращается к текущей странице после успешной проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="4337a-114">Navigating to this URL takes the user to the login page and back to the current page after a successful authentication.</span></span>

```razor
@page "/fetchdata"
...
@attribute [Authorize]

...

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var tokenResult = await AuthenticationService.RequestAccessToken();

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            forecasts = await httpClient.GetJsonAsync<WeatherForecast[]>(
                "WeatherForecast");
        }
        else
        {
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }

    }
}
```

<span data-ttu-id="4337a-115">Дополнительные сведения см. в статье [Сохранение состояния приложения перед выполнением проверки подлинности](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation).</span><span class="sxs-lookup"><span data-stu-id="4337a-115">For more information, see [Save app state before an authentication operation](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation).</span></span>
