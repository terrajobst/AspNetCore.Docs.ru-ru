---
title: ASP.NET Core Blazor дополнительные сценарии безопасности для сборки
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/additional-scenarios
ms.openlocfilehash: fe87ce76d8e181de788188b021616f2a09833585
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083695"
---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a>ASP.NET Core Блазор дополнительные сценарии безопасности для сборки

[Хавьер Калварро Воронков](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="handle-token-request-errors"></a>Обработку ошибок запросов маркера

Когда одностраничное приложение (SPA) выполняет проверку подлинности пользователя с помощью Open ID Connect (OIDC), состояние проверки подлинности сохраняется локально в рамках SPA и в поставщике удостоверений (IP) в виде файла cookie сеанса, который задается в результате предоставления учетных данных пользователем.

Маркеры, которые IP-адрес выдает пользователю, обычно действительны в течение короткого периода времени, примерно один час, поэтому клиентское приложение должно регулярно получать новые токены. В противном случае пользователь будет зарегистрирован после истечения срока действия назначенных токенов. В большинстве случаев клиенты OIDC могут подготавливать новые маркеры, не требуя от пользователя повторной проверки подлинности благодаря состоянию аутентификации или сеансу, который хранится в IP-адресе.

В некоторых случаях клиент не может получить маркер без взаимодействия с пользователем, например, когда по какой-либо причине пользователь явно выходит из IP. Эта ситуация возникает, если пользователь посещает `https://login.microsoftonline.com` и выполнит выход из нее. В этих сценариях приложение не знает о немедленном выходе пользователя из систему. Любой токен, который удерживает клиент, может быть недействительным. Кроме того, клиент не может подготавливать новый маркер без взаимодействия с пользователем после истечения срока действия текущего маркера.

Эти сценарии не относятся к проверке подлинности на основе маркеров. Они являются частью природы одностраничные приложения. Проверка подлинности с помощью файлов cookie также не вызывает API сервера, если удаляется файл cookie для аутентификации.

Когда приложение выполняет вызовы API к защищенным ресурсам, необходимо учитывать следующее:

* Чтобы создать новый маркер доступа для вызова API, пользователю может потребоваться выполнить повторную проверку подлинности.
* Даже если у клиента есть токен, который кажется допустимым, вызов сервера может завершиться ошибкой, так как маркер был отозван пользователем.

Когда приложение запрашивает маркер, существует два возможных результата:

* Запрос выполнен успешно, и приложение имеет действительный токен.
* Запрос завершается ошибкой, и приложение должно повторно пройти проверку подлинности пользователя, чтобы получить новый маркер.

При сбое запроса маркера необходимо решить, следует ли сохранить текущее состояние перед выполнением перенаправления. Существует несколько подходов, повышающих уровень сложности:

* Сохранение текущего состояния страницы в хранилище сеансов. Во время `OnInitializeAsync`проверьте, можно ли восстановить состояние, прежде чем продолжить.
* Добавьте параметр строки запроса и используйте его в качестве способа сигнализации приложению о необходимости повторного расконсервации ранее сохраненного состояния.
* Добавьте параметр строки запроса с уникальным идентификатором для хранения данных в хранилище сеанса без риска конфликтов с другими элементами.

В приведенном ниже примере показано, как выполнить следующие задачи.

* Сохраните состояние перед перенаправлением на страницу входа.
* Восстановите предыдущее состояние после проверки подлинности с помощью параметра строки запроса.

```razor
<EditForm Model="User" @onsubmit="OnSaveAsync">
    <label>User
        <InputText @bind-Value="User.Name" />
    </label>
    <label>Last name
        <InputText @bind-Value="User.LastName" />
    </label>
</EditForm>

@code {
    public class Profile
    {
        public string Name { get; set; }
        public string LastName { get; set; }
    }

    public Profile User { get; set; } = new Profile();

    protected async override Task OnInitializedAsync()
    {
        var currentQuery = new Uri(Navigation.Uri).Query;

        if (currentQuery.Contains("state=resumeSavingProfile"))
        {
            User = await JS.InvokeAsync<Profile>("sessionStorage.getState", 
                "resumeSavingProfile");
        }
    }

    public async Task OnSaveAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var resumeUri = Navigation.Uri + $"?state=resumeSavingProfile";

        var tokenResult = await AuthenticationService.RequestAccessToken(
            new AccessTokenRequestOptions
            {
                ReturnUrl = resumeUri
            });

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            await httpClient.PostJsonAsync("Save", User);
        }
        else
        {
            await JS.InvokeVoidAsync("sessionStorage.setState", 
                "resumeSavingProfile", User);
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }
    }
}
```

## <a name="save-app-state-before-an-authentication-operation"></a>Сохранить состояние приложения перед операцией проверки подлинности

Во время операции аутентификации существуют случаи, когда необходимо сохранить состояние приложения, прежде чем браузер перенаправится на IP-адрес. Это может быть так, если вы используете нечто вроде контейнера состояния и хотите восстановить состояние после завершения проверки подлинности. Можно использовать пользовательский объект состояния проверки подлинности, чтобы сохранить состояние конкретного приложения или ссылку на него, а затем восстановить это состояние после успешного завершения операции проверки подлинности.

компонент `Authentication` (*pages/Authentication. Razor*):

```razor
@page "/authentication/{action}"
@inject JSRuntime JS
@inject StateContainer State
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorViewCore Action="@Action" 
    AuthenticationState="AuthenticationState" OnLoginSucceded="RestoreState" 
    OnLogoutSucceded="RestoreState" />

@code {
    [Parameter]
    public string Action { get; set; }

    public class ApplicationAuthenticationState : RemoteAuthenticationState
    {
        public string Id { get; set; }
    }

    protected async override Task OnInitializedAsync()
    {
        if (RemoteAuthenticationActions.IsAction(RemoteAuthenticationActions.LogIn, 
            Action))
        {
            AuthenticationState.Id = Guid.NewGuid().ToString();
            await JS.InvokeVoidAsync("sessionStorage.setKey", 
                AuthenticationState.Id, State.Store());
        }
    }

    public async Task RestoreState(ApplicationAuthenticationState state)
    {
        var stored = await JS.InvokeAsync<string>("sessionStorage.getKey", 
            state.Id);
        State.FromStore(stored);
    }

    public ApplicationAuthenticationState AuthenticationState { get; set; } = 
        new ApplicationAuthenticationState();
}
```

## <a name="request-additional-access-tokens"></a>Запрос дополнительных маркеров доступа

Большинству приложений требуется только маркер доступа для взаимодействия с защищенными ресурсами, которые они используют. В некоторых сценариях для взаимодействия с двумя и более ресурсами приложению может потребоваться несколько маркеров. Метод `IAccessTokenProvider.RequestToken` предоставляет перегрузку, которая позволяет приложению подготавливать маркер с заданным набором областей, как показано в следующем примере:

```csharp
var tokenResult = await AuthenticationService.RequestAccessToken(
    new AccessTokenRequestOptions
    {
        Scopes = new[] { "https://graph.microsoft.com/Mail.Send", 
            "https://graph.microsoft.com/User.Read" }
    });
```

## <a name="customize-app-routes"></a>Настройка маршрутов приложений

По умолчанию библиотека `Microsoft.AspNetCore.Components.WebAssembly.Authentication` использует маршруты, приведенные в следующей таблице, для представления различных состояний проверки подлинности.

| Маршрут                            | Цель |
| -------------------------------- | ------- |
| `authentication/login`           | Активирует операцию входа. |
| `authentication/login-callback`  | Обрабатывает результат любой операции входа. |
| `authentication/login-failed`    | Отображает сообщения об ошибках при сбое операции входа по какой-либо причине. |
| `authentication/logout`          | Активирует операцию выхода. |
| `authentication/logout-callback` | Обрабатывает результат операции выхода. |
| `authentication/logout-failed`   | Отображает сообщения об ошибках при сбое операции выхода по какой-либо причине. |
| `authentication/logged-out`      | Указывает, что пользователь успешно выполнил выход. |
| `authentication/profile`         | Активирует операцию для изменения профиля пользователя. |
| `authentication/register`        | Активирует операцию для регистрации нового пользователя. |

Маршруты, показанные в предыдущей таблице, можно настроить с помощью `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`. При настройке параметров для предоставления настраиваемых маршрутов убедитесь, что у приложения есть маршрут, который обрабатывает каждый путь.

В следующем примере все пути имеют префикс `/security`.

компонент `Authentication` (*pages/Authentication. Razor*):

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

`Program.Main` (*Program.CS*):

```csharp
builder.Services.AddApiAuthorization(options => { 
    options.AuthenticationPaths.LogInPath = "security/login";
    options.AuthenticationPaths.LogInCallbackPath = "security/login-callback";
    options.AuthenticationPaths.LogInFailedPath = "security/login-failed";
    options.AuthenticationPaths.LogOutPath = "security/logout";
    options.AuthenticationPaths.LogOutCallbackPath = "security/logout-callback";
    options.AuthenticationPaths.LogOutFailedPath = "security/logout-failed";
    options.AuthenticationPaths.LogOutSucceededPath = "security/logged-out";
    options.AuthenticationPaths.ProfilePath = "security/profile";
    options.AuthenticationPaths.RegisterPath = "security/register";
});
```

Если требование вызывается для совершенно разных путей, задайте маршруты, как описано выше, и отрисовываете `RemoteAuthenticatorView` с помощью явного параметра Action:

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

При необходимости вы можете разбить пользовательский интерфейс на разные страницы.

## <a name="customize-the-authentication-user-interface"></a>Настройка пользовательского интерфейса проверки подлинности

`RemoteAuthenticatorView` включает набор элементов пользовательского интерфейса по умолчанию для каждого состояния проверки подлинности. Каждое состояние можно настроить, передав пользовательский `RenderFragment`. Чтобы настроить отображаемый текст во время первоначального входа в систему, можно изменить `RemoteAuthenticatorView` следующим образом.

компонент `Authentication` (*pages/Authentication. Razor*):

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action">
    <LoggingIn>
        You are about to be redirected to https://login.microsoftonline.com.
    </LoggingIn>
</RemoteAuthenticatorView>

@code{
    [Parameter]
    public string Action { get; set; }
}
```

В `RemoteAuthenticatorView` есть один фрагмент, который можно использовать для каждого маршрута проверки подлинности, как показано в следующей таблице.

| Маршрут                            | Fragment             |
| -------------------------------- | -------------------- |
| `authentication/login`           | `<LoggingIn>`        |
| `authentication/login-callback`  | `<CompletingLogIn>`  |
| `authentication/login-failed`    | `<LogInFailed>`      |
| `authentication/logout`          | `<LoggingOut>`       |
| `authentication/logout-callback` | `<CompletingLogOut>` |
| `authentication/logout-failed`   | `<LogOutFailed>`     |
| `authentication/logged-out`      | `<LogOutSucceeded>`  |
| `authentication/profile`         | `<UserProfile>`      |
| `authentication/register`        | `<Registering>`      |
