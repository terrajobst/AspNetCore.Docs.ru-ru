Компонент `FetchData` показывает, как:

* Подготавливает маркер доступа.
* Используйте маркер доступа для вызова API защищенных ресурсов в *серверном* приложении.

Директива `@attribute [Authorize]` указывает системе авторизации веб – сборки Блазор, что пользователь должен быть авторизован для посещения этого компонента. Наличие атрибута в *клиентском* приложении не мешает вызову API на сервере без соответствующих учетных данных. *Серверное* приложение также должно использовать `[Authorize]` на соответствующих конечных точках, чтобы правильно защитить их.

`AuthenticationService.RequestAccessToken();` позаботится о запросе маркера доступа, который можно добавить в запрос для вызова API. Если маркер кэшируется или служба может подготавливать новый маркер доступа без вмешательства пользователя, запрос маркера будет выполнен. В противном случае запрос маркера завершится ошибкой.

Чтобы получить фактический токен для включения в запрос, приложение должно проверить, что запрос был обработан, вызвав `tokenResult.TryGetToken(out var token)`. 

Если запрос был успешным, переменная маркера заполняется маркером доступа. Свойство `Value` маркера предоставляет строку литерала для включения в заголовок запроса `Authorization`.

Если запрос завершился ошибкой, так как не удалось подготовить маркер без взаимодействия с пользователем, результат маркера содержит URL-адрес перенаправления. При переходе по этому URL-адресу пользователь переходит на страницу входа и возвращается к текущей странице после успешной проверки подлинности.

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

Дополнительные сведения см. в статье [Сохранение состояния приложения перед выполнением проверки подлинности](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation).
