---
title: Безопасные Blazor приложения ASP.NET Core Server
author: guardrex
description: Узнайте, как уменьшить Blazor угрозы безопасности для приложений Сервера.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/02/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/server
ms.openlocfilehash: bd03f811d0425fdfdb7bbbc24fea5481b49b8ed3
ms.sourcegitcommit: 9675db7bf4b67ae269f9226b6f6f439b5cce4603
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2020
ms.locfileid: "80626024"
---
# <a name="secure-aspnet-core-blazor-server-apps"></a><span data-ttu-id="79d9d-103">Безопасные приложения ASP.NET Основный Blazor Server</span><span class="sxs-lookup"><span data-stu-id="79d9d-103">Secure ASP.NET Core Blazor Server apps</span></span>

<span data-ttu-id="79d9d-104">Хавьер [Кальварро Нельсон](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="79d9d-104">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

<span data-ttu-id="79d9d-105">Приложения Blazor Server внедряли модель *обработки данных,* в которой сервер и клиент поддерживают долгосрочные отношения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-105">Blazor Server apps adopt a *stateful* data processing model, where the server and client maintain a long-lived relationship.</span></span> <span data-ttu-id="79d9d-106">Постоянное состояние поддерживается [цепью,](xref:blazor/state-management)которая может охватывать соединения, которые также потенциально долгоживущие.</span><span class="sxs-lookup"><span data-stu-id="79d9d-106">The persistent state is maintained by a [circuit](xref:blazor/state-management), which can span connections that are also potentially long-lived.</span></span>

<span data-ttu-id="79d9d-107">Когда пользователь посещает сайт Blazor Server, сервер создает схему в памяти сервера.</span><span class="sxs-lookup"><span data-stu-id="79d9d-107">When a user visits a Blazor Server site, the server creates a circuit in the server's memory.</span></span> <span data-ttu-id="79d9d-108">Схема показывает браузеру, какое содержимое отображать и реагировать на события, например, когда пользователь выбирает кнопку в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="79d9d-108">The circuit indicates to the browser what content to render and responds to events, such as when the user selects a button in the UI.</span></span> <span data-ttu-id="79d9d-109">Для выполнения этих действий схема вызывает функции JavaScript в браузере пользователя и методы .NET на сервере.</span><span class="sxs-lookup"><span data-stu-id="79d9d-109">To perform these actions, a circuit invokes JavaScript functions in the user's browser and .NET methods on the server.</span></span> <span data-ttu-id="79d9d-110">Это двустороннее взаимодействие на основе JavaScript называется [Interop JavaScript (JS interop).](xref:blazor/call-javascript-from-dotnet)</span><span class="sxs-lookup"><span data-stu-id="79d9d-110">This two-way JavaScript-based interaction is referred to as [JavaScript interop (JS interop)](xref:blazor/call-javascript-from-dotnet).</span></span>

<span data-ttu-id="79d9d-111">Поскольку JS interop происходит через Интернет и клиент использует удаленный браузер, приложения Blazor Server разделяют большинство проблем безопасности веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="79d9d-111">Because JS interop occurs over the Internet and the client uses a remote browser, Blazor Server apps share most web app security concerns.</span></span> <span data-ttu-id="79d9d-112">Эта тема описывает общие угрозы для приложений Blazor Server и предоставляет рекомендации по смягчению угроз, ориентированные на приложения, ориентированные на Интернет.</span><span class="sxs-lookup"><span data-stu-id="79d9d-112">This topic describes common threats to Blazor Server apps and provides threat mitigation guidance focused on Internet-facing apps.</span></span>

<span data-ttu-id="79d9d-113">В ограниченных средах, таких как внутри корпоративных сетей или интрасетей, некоторые из руководящих принципов смягчения либо:</span><span class="sxs-lookup"><span data-stu-id="79d9d-113">In constrained environments, such as inside corporate networks or intranets, some of the mitigation guidance either:</span></span>

* <span data-ttu-id="79d9d-114">Не применяется в ограниченной среде.</span><span class="sxs-lookup"><span data-stu-id="79d9d-114">Doesn't apply in the constrained environment.</span></span>
* <span data-ttu-id="79d9d-115">Не стоит затрат на реализацию, так как риск безопасности является низким в ограниченной среде.</span><span class="sxs-lookup"><span data-stu-id="79d9d-115">Isn't worth the cost to implement because the security risk is low in a constrained environment.</span></span>

## <a name="blazor-server-project-template"></a><span data-ttu-id="79d9d-116">Шаблон проекта Blazor Server</span><span class="sxs-lookup"><span data-stu-id="79d9d-116">Blazor Server project template</span></span>

<span data-ttu-id="79d9d-117">Шаблон проекта Blazor Server может быть настроен для проверки подлинности при создании проекта.</span><span class="sxs-lookup"><span data-stu-id="79d9d-117">The Blazor Server project template can be configured for authentication when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="79d9d-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79d9d-118">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="79d9d-119">Следуйте указаниям по работе с Visual Studio (<xref:blazor/get-started>), чтобы создать проект Blazor на стороне сервера с механизмом аутентификации.</span><span class="sxs-lookup"><span data-stu-id="79d9d-119">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="79d9d-120">Выбрав шаблон **Серверное приложение Blazor** в диалоговом окне **Создать веб-приложение ASP.NET Core**, щелкните **Изменить** в разделе **Проверка подлинности**.</span><span class="sxs-lookup"><span data-stu-id="79d9d-120">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="79d9d-121">Откроется диалоговое окно с тем же набором механизмов аутентификации, которые доступны для других проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="79d9d-121">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="79d9d-122">**Отсутствие аутентификации**</span><span class="sxs-lookup"><span data-stu-id="79d9d-122">**No Authentication**</span></span>
* <span data-ttu-id="79d9d-123">\*\*\*\* Учетные записи отдельных пользователей&ndash;, которые можно хранить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="79d9d-123">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="79d9d-124">в приложении с помощью системы [удостоверений](xref:security/authentication/identity) ASP.NET Core;</span><span class="sxs-lookup"><span data-stu-id="79d9d-124">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="79d9d-125">в [Azure AD B2C](xref:security/authentication/azure-ad-b2c);</span><span class="sxs-lookup"><span data-stu-id="79d9d-125">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="79d9d-126">**Рабочие или школьные счета**</span><span class="sxs-lookup"><span data-stu-id="79d9d-126">**Work or School Accounts**</span></span>
* <span data-ttu-id="79d9d-127">**Проверка подлинности Windows**</span><span class="sxs-lookup"><span data-stu-id="79d9d-127">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="79d9d-128">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79d9d-128">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="79d9d-129">Следуйте указаниям по работе с Visual Studio Code (<xref:blazor/get-started>), чтобы создать проект Blazor на стороне сервера с механизмом аутентификации.</span><span class="sxs-lookup"><span data-stu-id="79d9d-129">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="79d9d-130">Допустимые значения аутентификации (`{AUTHENTICATION}`) перечислены в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="79d9d-130">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="79d9d-131">Механизм аутентификации</span><span class="sxs-lookup"><span data-stu-id="79d9d-131">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="79d9d-132">Значение `{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="79d9d-132">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="79d9d-133">Без аутентификации</span><span class="sxs-lookup"><span data-stu-id="79d9d-133">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="79d9d-134">Индивидуальное лицо</span><span class="sxs-lookup"><span data-stu-id="79d9d-134">Individual</span></span><br><span data-ttu-id="79d9d-135">Пользователи, сохраненные в приложении с помощью ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="79d9d-135">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="79d9d-136">Индивидуальное лицо</span><span class="sxs-lookup"><span data-stu-id="79d9d-136">Individual</span></span><br><span data-ttu-id="79d9d-137">Пользователи, сохраненные в [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="79d9d-137">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="79d9d-138">Рабочие или учебные учетные записи</span><span class="sxs-lookup"><span data-stu-id="79d9d-138">Work or School Accounts</span></span><br><span data-ttu-id="79d9d-139">Корпоративная аутентификация для отдельного клиента.</span><span class="sxs-lookup"><span data-stu-id="79d9d-139">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="79d9d-140">Рабочие или учебные учетные записи</span><span class="sxs-lookup"><span data-stu-id="79d9d-140">Work or School Accounts</span></span><br><span data-ttu-id="79d9d-141">Корпоративная аутентификация для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="79d9d-141">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="79d9d-142">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="79d9d-142">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="79d9d-143">Эта команда создает папку, в качестве имени для которой берется значение, предоставленное вместо заполнителя `{APP NAME}`, а затем использует имя папки в качестве имени приложения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-143">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="79d9d-144">Подробнее см. статью о команде [dotnet new](/dotnet/core/tools/dotnet-new) в руководстве по .NET Core.</span><span class="sxs-lookup"><span data-stu-id="79d9d-144">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="79d9d-145">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="79d9d-145">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="79d9d-146">Следуйте Visual Studio для Mac <xref:blazor/get-started> руководство в статье.</span><span class="sxs-lookup"><span data-stu-id="79d9d-146">Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.</span></span>

1. <span data-ttu-id="79d9d-147">На **настройке нового** шага Blazor Server App выберите **индивидуальную аутентификацию (в приложении)** из уровня **аутентификации.**</span><span class="sxs-lookup"><span data-stu-id="79d9d-147">On the **Configure your new Blazor Server App** step, select **Individual Authentication (in-app)** from the **Authentication** drop down.</span></span>

1. <span data-ttu-id="79d9d-148">Приложение создано для отдельных пользователей, хранящихся в приложении с ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="79d9d-148">The app is created for individual users stored in the app with ASP.NET Core Identity.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="79d9d-149">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="79d9d-149">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="79d9d-150">Следуйте руководству .NET Core CLI в <xref:blazor/get-started> статье, чтобы создать новый проект Blazor Server с механизмом аутентификации:</span><span class="sxs-lookup"><span data-stu-id="79d9d-150">Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="79d9d-151">Допустимые значения аутентификации (`{AUTHENTICATION}`) перечислены в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="79d9d-151">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="79d9d-152">Механизм аутентификации</span><span class="sxs-lookup"><span data-stu-id="79d9d-152">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="79d9d-153">Значение `{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="79d9d-153">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="79d9d-154">Без аутентификации</span><span class="sxs-lookup"><span data-stu-id="79d9d-154">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="79d9d-155">Индивидуальное лицо</span><span class="sxs-lookup"><span data-stu-id="79d9d-155">Individual</span></span><br><span data-ttu-id="79d9d-156">Пользователи, сохраненные в приложении с помощью ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="79d9d-156">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="79d9d-157">Индивидуальное лицо</span><span class="sxs-lookup"><span data-stu-id="79d9d-157">Individual</span></span><br><span data-ttu-id="79d9d-158">Пользователи, сохраненные в [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="79d9d-158">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="79d9d-159">Рабочие или учебные учетные записи</span><span class="sxs-lookup"><span data-stu-id="79d9d-159">Work or School Accounts</span></span><br><span data-ttu-id="79d9d-160">Корпоративная аутентификация для отдельного клиента.</span><span class="sxs-lookup"><span data-stu-id="79d9d-160">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="79d9d-161">Рабочие или учебные учетные записи</span><span class="sxs-lookup"><span data-stu-id="79d9d-161">Work or School Accounts</span></span><br><span data-ttu-id="79d9d-162">Корпоративная аутентификация для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="79d9d-162">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="79d9d-163">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="79d9d-163">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="79d9d-164">Эта команда создает папку, в качестве имени для которой берется значение, предоставленное вместо заполнителя `{APP NAME}`, а затем использует имя папки в качестве имени приложения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-164">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="79d9d-165">Подробнее см. статью о команде [dotnet new](/dotnet/core/tools/dotnet-new) в руководстве по .NET Core.</span><span class="sxs-lookup"><span data-stu-id="79d9d-165">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

---

## <a name="pass-tokens-to-a-blazor-server-app"></a><span data-ttu-id="79d9d-166">Передайте токены в приложение Blazor Server</span><span class="sxs-lookup"><span data-stu-id="79d9d-166">Pass tokens to a Blazor Server app</span></span>

<span data-ttu-id="79d9d-167">Authenticate приложение Blazor Server, как вы бы с обычным Razor Страницы или MVC приложение.</span><span class="sxs-lookup"><span data-stu-id="79d9d-167">Authenticate the Blazor Server app as you would with a regular Razor Pages or MVC app.</span></span> <span data-ttu-id="79d9d-168">Предоставление и сохранение маркеров в файле проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="79d9d-168">Provision and save the tokens to the authentication cookie.</span></span> <span data-ttu-id="79d9d-169">Пример:</span><span class="sxs-lookup"><span data-stu-id="79d9d-169">For example:</span></span>

```csharp
using Microsoft.AspNetCore.Authentication.OpenIdConnect;

...

services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
{
    options.ResponseType = "code";
    options.SaveTokens = true;

    options.Scope.Add("offline_access");
    options.Scope.Add("{SCOPE}");
    options.Resource = "{RESOURCE}";
});
```

<span data-ttu-id="79d9d-170">Для примера кода, `Startup.ConfigureServices` включая [Passing tokens to a server-side Blazor application](https://github.com/javiercn/blazor-server-aad-sample)полный пример, см.</span><span class="sxs-lookup"><span data-stu-id="79d9d-170">For sample code, including a complete `Startup.ConfigureServices` example, see the [Passing tokens to a server-side Blazor application](https://github.com/javiercn/blazor-server-aad-sample).</span></span>

<span data-ttu-id="79d9d-171">Определите класс для прохождения в исходном состоянии приложения с помощью токенов доступа и обновления:</span><span class="sxs-lookup"><span data-stu-id="79d9d-171">Define a class to pass in the initial app state with the access and refresh tokens:</span></span>

```csharp
public class InitialApplicationState
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

<span data-ttu-id="79d9d-172">Определите **объем** услуг поставщика токенов, который может быть использован в приложении Blazor для устранения токенов из DI:</span><span class="sxs-lookup"><span data-stu-id="79d9d-172">Define a **scoped** token provider service that can be used within the Blazor app to resolve the tokens from DI:</span></span>

```csharp
using System;
using System.Security.Claims;
using System.Threading.Tasks;

public class TokenProvider
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

<span data-ttu-id="79d9d-173">В, `Startup.ConfigureServices`добавить услуги для:</span><span class="sxs-lookup"><span data-stu-id="79d9d-173">In `Startup.ConfigureServices`, add services for:</span></span>

* `IHttpClientFactory`
* `TokenProvider`

```csharp
services.AddHttpClient();
services.AddScoped<TokenProvider>();
```

<span data-ttu-id="79d9d-174">В файле *_Host.cshtml* создайте `InitialApplicationState` и передайте его в качестве параметра приложению:</span><span class="sxs-lookup"><span data-stu-id="79d9d-174">In the *_Host.cshtml* file, create and instance of `InitialApplicationState` and pass it as a parameter to the app:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authentication

...

@{
    var tokens = new InitialApplicationState
    {
        AccessToken = await HttpContext.GetTokenAsync("access_token"),
        RefreshToken = await HttpContext.GetTokenAsync("refresh_token")
    };
}

<app>
    <component type="typeof(App)" param-InitialState="tokens" 
        render-mode="ServerPrerendered" />
</app>
```

<span data-ttu-id="79d9d-175">В `App` компоненте (*App.razor*), решить службы и инициализировать его с данными из параметра:</span><span class="sxs-lookup"><span data-stu-id="79d9d-175">In the `App` component (*App.razor*), resolve the service and initialize it with the data from the parameter:</span></span>

```razor
@inject TokenProvider TokensProvider

...

@code {
    [Parameter]
    public InitialApplicationState InitialState { get; set; }

    protected override Task OnInitializedAsync()
    {
        TokensProvider.AccessToken = InitialState.AccessToken;
        TokensProvider.RefreshToken = InitialState.RefreshToken;

        return base.OnInitializedAsync();
    }
}
```

<span data-ttu-id="79d9d-176">В службе, которая делает безопасный запрос API, введите поставщика маркеров и извлеките маркер для вызова API:</span><span class="sxs-lookup"><span data-stu-id="79d9d-176">In the service that makes a secure API request, inject the token provider and retrieve the token to call the API:</span></span>

```csharp
public class WeatherForecastService
{
    private readonly TokenProvider _store;

    public WeatherForecastService(IHttpClientFactory clientFactory, 
        TokenProvider tokenProvider)
    {
        Client = clientFactory.CreateClient();
        _store = tokenProvider;
    }

    public HttpClient Client { get; }

    public async Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        var token = _store.AccessToken;
        var request = new HttpRequestMessage(HttpMethod.Get, 
            "https://localhost:5003/WeatherForecast");
        request.Headers.Add("Authorization", $"Bearer {token}");
        var response = await Client.SendAsync(request);
        response.EnsureSuccessStatusCode();

        return await response.Content.ReadAsAsync<WeatherForecast[]>();
    }
}
```

## <a name="resource-exhaustion"></a><span data-ttu-id="79d9d-177">Исчерпание ресурсов</span><span class="sxs-lookup"><span data-stu-id="79d9d-177">Resource exhaustion</span></span>

<span data-ttu-id="79d9d-178">Исчерпание ресурсов может произойти, когда клиент взаимодействует с сервером и заставляет сервер потреблять избыточные ресурсы.</span><span class="sxs-lookup"><span data-stu-id="79d9d-178">Resource exhaustion can occur when a client interacts with the server and causes the server to consume excessive resources.</span></span> <span data-ttu-id="79d9d-179">Чрезмерное потребление ресурсов в первую очередь влияет на:</span><span class="sxs-lookup"><span data-stu-id="79d9d-179">Excessive resource consumption primarily affects:</span></span>

* [<span data-ttu-id="79d9d-180">Процессора</span><span class="sxs-lookup"><span data-stu-id="79d9d-180">CPU</span></span>](#cpu)
* [<span data-ttu-id="79d9d-181">Память</span><span class="sxs-lookup"><span data-stu-id="79d9d-181">Memory</span></span>](#memory)
* [<span data-ttu-id="79d9d-182">Клиентские подключения</span><span class="sxs-lookup"><span data-stu-id="79d9d-182">Client connections</span></span>](#client-connections)

<span data-ttu-id="79d9d-183">Атаки отказов в обслуживании (DoS) обычно направлены на исчерпание ресурсов приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="79d9d-183">Denial of service (DoS) attacks usually seek to exhaust an app or server's resources.</span></span> <span data-ttu-id="79d9d-184">Однако исчерпание ресурсов не обязательно является результатом атаки на систему.</span><span class="sxs-lookup"><span data-stu-id="79d9d-184">However, resource exhaustion isn't necessarily the result of an attack on the system.</span></span> <span data-ttu-id="79d9d-185">Например, ограниченные ресурсы могут быть исчерпаны из-за высокого спроса пользователей.</span><span class="sxs-lookup"><span data-stu-id="79d9d-185">For example, finite resources can be exhausted due to high user demand.</span></span> <span data-ttu-id="79d9d-186">DoS дополнительно покрывается в разделе [атак «Отказ в обслуживании» (DoS).](#denial-of-service-dos-attacks)</span><span class="sxs-lookup"><span data-stu-id="79d9d-186">DoS is covered further in the [Denial of service (DoS) attacks](#denial-of-service-dos-attacks) section.</span></span>

<span data-ttu-id="79d9d-187">Ресурсы, не внерами к инфраструктуре Blazor, такие как базы данных и обработки файлов (используется для чтения и записи файлов), также могут испытывать истощение ресурсов.</span><span class="sxs-lookup"><span data-stu-id="79d9d-187">Resources external to the Blazor framework, such as databases and file handles (used to read and write files), may also experience resource exhaustion.</span></span> <span data-ttu-id="79d9d-188">Для получения дополнительной информации см. <xref:performance/performance-best-practices>.</span><span class="sxs-lookup"><span data-stu-id="79d9d-188">For more information, see <xref:performance/performance-best-practices>.</span></span>

### <a name="cpu"></a><span data-ttu-id="79d9d-189">ЦП</span><span class="sxs-lookup"><span data-stu-id="79d9d-189">CPU</span></span>

<span data-ttu-id="79d9d-190">Исчерпание процессора может произойти, когда один или несколько клиентов заставляют сервер выполнять интенсивную работу процессора.</span><span class="sxs-lookup"><span data-stu-id="79d9d-190">CPU exhaustion can occur when one or more clients force the server to perform intensive CPU work.</span></span>

<span data-ttu-id="79d9d-191">Например, рассмотрим приложение Blazor Server, которое вычисляет *номер Fibonnacci.*</span><span class="sxs-lookup"><span data-stu-id="79d9d-191">For example, consider a Blazor Server app that calculates a *Fibonnacci number*.</span></span> <span data-ttu-id="79d9d-192">Номер Фибонначчи производится из последовательности Фибонначчи, где каждое число в последовательности является суммой двух предыдущих чисел.</span><span class="sxs-lookup"><span data-stu-id="79d9d-192">A Fibonnacci number is produced from a Fibonnacci sequence, where each number in the sequence is the sum of the two preceding numbers.</span></span> <span data-ttu-id="79d9d-193">Объем работы, необходимый для достижения ответа, зависит от длины последовательности и размера исходного значения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-193">The amount of work required to reach the answer depends on the length of the sequence and the size of the initial value.</span></span> <span data-ttu-id="79d9d-194">Если приложение не ограничивает запрос клиента, расчеты, интенсивные для процессора, могут доминировать во времени процессора и снижать производительность других задач.</span><span class="sxs-lookup"><span data-stu-id="79d9d-194">If the app doesn't place limits on a client's request, the CPU-intensive calculations may dominate the CPU's time and diminish the performance of other tasks.</span></span> <span data-ttu-id="79d9d-195">Чрезмерное потребление ресурсов является проблемой безопасности, влияющих на доступность.</span><span class="sxs-lookup"><span data-stu-id="79d9d-195">Excessive resource consumption is a security concern impacting availability.</span></span>

<span data-ttu-id="79d9d-196">Исчерпание процессора является проблемой для всех общедоступных приложений.</span><span class="sxs-lookup"><span data-stu-id="79d9d-196">CPU exhaustion is a concern for all public-facing apps.</span></span> <span data-ttu-id="79d9d-197">В обычных веб-приложениях запросы и подключения продлятесь в качестве гарантии, но приложения Blazor Server не обеспечивают одинаковых гарантий.</span><span class="sxs-lookup"><span data-stu-id="79d9d-197">In regular web apps, requests and connections time out as a safeguard, but Blazor Server apps don't provide the same safeguards.</span></span> <span data-ttu-id="79d9d-198">Приложения Blazor Server должны включать соответствующие проверки и ограничения перед выполнением потенциально интенсивной работы с процессором.</span><span class="sxs-lookup"><span data-stu-id="79d9d-198">Blazor Server apps must include appropriate checks and limits before performing potentially CPU-intensive work.</span></span>

### <a name="memory"></a><span data-ttu-id="79d9d-199">Память</span><span class="sxs-lookup"><span data-stu-id="79d9d-199">Memory</span></span>

<span data-ttu-id="79d9d-200">Исчерпание памяти может произойти, когда один или несколько клиентов заставляют сервер потреблять большое количество памяти.</span><span class="sxs-lookup"><span data-stu-id="79d9d-200">Memory exhaustion can occur when one or more clients force the server to consume a large amount of memory.</span></span>

<span data-ttu-id="79d9d-201">Например, рассмотрим боковое приложение Blazor-server с компонентом, который принимает и отображает список элементов.</span><span class="sxs-lookup"><span data-stu-id="79d9d-201">For example, consider a Blazor-server side app with a component that accepts and displays a list of items.</span></span> <span data-ttu-id="79d9d-202">Если приложение Blazor не ограничивает количество разрешенных элементов или количество элементов, отображаемых клиенту, интенсивная обработка и визуализация памяти может доминировать в памяти сервера до такой степени, что производительность сервера страдает.</span><span class="sxs-lookup"><span data-stu-id="79d9d-202">If the Blazor app doesn't place limits on the number of items allowed or the number of items rendered back to the client, the memory-intensive processing and rendering may dominate the server's memory to the point where performance of the server suffers.</span></span> <span data-ttu-id="79d9d-203">Сервер может выйти из строя или замедлиться до такой степени, что он, как представляется, разбился.</span><span class="sxs-lookup"><span data-stu-id="79d9d-203">The server may crash or slow to the point that it appears to have crashed.</span></span>

<span data-ttu-id="79d9d-204">Рассмотрим следующий сценарий для поддержания и отображения списка элементов, относящихся к сценарию истощения памяти на сервере:</span><span class="sxs-lookup"><span data-stu-id="79d9d-204">Consider the following scenario for maintaining and displaying a list of items that pertain to a potential memory exhaustion scenario on the server:</span></span>

* <span data-ttu-id="79d9d-205">Элементы в `List<MyItem>` свойстве или поле используют память сервера.</span><span class="sxs-lookup"><span data-stu-id="79d9d-205">The items in a `List<MyItem>` property or field use the server's memory.</span></span> <span data-ttu-id="79d9d-206">Если приложение позволяет списку элементов расти неограниченно, есть риск того, что у сервера не будет памяти.</span><span class="sxs-lookup"><span data-stu-id="79d9d-206">If the app allows the list of items to grow unbounded, there's a risk of the server running out of memory.</span></span> <span data-ttu-id="79d9d-207">Запуск из памяти приводит к окончанию текущего сеанса (авария), и все параллельные сеансы в экземпляре сервера получают исключение из непамяти.</span><span class="sxs-lookup"><span data-stu-id="79d9d-207">Running out of memory causes the current session to end (crash) and all of the concurrent sessions in that server instance receive an out-of-memory exception.</span></span> <span data-ttu-id="79d9d-208">Чтобы предотвратить возникновение этого сценария, приложение должно использовать структуру данных, которая накладывает ограничение элемента на одновременных пользователей.</span><span class="sxs-lookup"><span data-stu-id="79d9d-208">To prevent this scenario from occurring, the app must use a data structure that imposes an item limit on concurrent users.</span></span>
* <span data-ttu-id="79d9d-209">Если схема paging не используется для рендеринга, сервер использует дополнительную память для объектов, которые не видны в системе обработки.</span><span class="sxs-lookup"><span data-stu-id="79d9d-209">If a paging scheme isn't used for rendering, the server uses additional memory for objects that aren't visible in the UI.</span></span> <span data-ttu-id="79d9d-210">Без ограничения количества элементов требования к памяти могут исчерпать доступную память сервера.</span><span class="sxs-lookup"><span data-stu-id="79d9d-210">Without a limit on the number of items, memory demands may exhaust the available server memory.</span></span> <span data-ttu-id="79d9d-211">Чтобы предотвратить этот сценарий, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="79d9d-211">To prevent this scenario, use one of the following approaches:</span></span>
  * <span data-ttu-id="79d9d-212">Используйте paginated списки при визуализации.</span><span class="sxs-lookup"><span data-stu-id="79d9d-212">Use paginated lists when rendering.</span></span>
  * <span data-ttu-id="79d9d-213">Отображайте только первые 100-1000 элементов и требуйте от пользователя ввести критерии поиска, чтобы найти элементы за пределами отображаемых элементов.</span><span class="sxs-lookup"><span data-stu-id="79d9d-213">Only display the first 100 to 1,000 items and require the user to enter search criteria to find items beyond the items displayed.</span></span>
  * <span data-ttu-id="79d9d-214">Для более продвинутого сценария визуализации реализуйте списки или сетки, *поддерживающие виртуализацию.*</span><span class="sxs-lookup"><span data-stu-id="79d9d-214">For a more advanced rendering scenario, implement lists or grids that support *virtualization*.</span></span> <span data-ttu-id="79d9d-215">Используя виртуализацию, списки только визуализации подмножество элементов в настоящее время видны пользователю.</span><span class="sxs-lookup"><span data-stu-id="79d9d-215">Using virtualization, lists only render a subset of items currently visible to the user.</span></span> <span data-ttu-id="79d9d-216">Когда пользователь взаимодействует с панелью прокрутки в пользовательском интерфейсе, компонент отображает только те элементы, которые необходимы для отображения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-216">When the user interacts with the scrollbar in the UI, the component renders only those items required for display.</span></span> <span data-ttu-id="79d9d-217">Элементы, которые в настоящее время не требуются для отображения, могут храниться во вторичном хранилище, что является идеальным подходом.</span><span class="sxs-lookup"><span data-stu-id="79d9d-217">The items that aren't currently required for display can be held in secondary storage, which is the ideal approach.</span></span> <span data-ttu-id="79d9d-218">Неотображенные элементы также могут быть в памяти, что является менее идеальным.</span><span class="sxs-lookup"><span data-stu-id="79d9d-218">Undisplayed items can also be held in memory, which is less ideal.</span></span>

<span data-ttu-id="79d9d-219">Приложения Blazor Server предлагают аналогичную модель программирования для других платформ uI для приложений с состоянием, таких как WPF, Windows Forms или Blazor Web Assembly.</span><span class="sxs-lookup"><span data-stu-id="79d9d-219">Blazor Server apps offer a similar programming model to other UI frameworks for stateful apps, such as WPF, Windows Forms, or Blazor WebAssembly.</span></span> <span data-ttu-id="79d9d-220">Основное отличие заключается в том, что в нескольких средах uI память, потребляемая приложением, принадлежит клиенту и влияет только на этого отдельного клиента.</span><span class="sxs-lookup"><span data-stu-id="79d9d-220">The main difference is that in several of the UI frameworks the memory consumed by the app belongs to the client and only affects that individual client.</span></span> <span data-ttu-id="79d9d-221">Например, приложение Blazor WebAssembly работает полностью на клиенте и использует только ресурсы памяти клиента.</span><span class="sxs-lookup"><span data-stu-id="79d9d-221">For example, a Blazor WebAssembly app runs entirely on the client and only uses client memory resources.</span></span> <span data-ttu-id="79d9d-222">В сценарии Blazor Server память, потребляемая приложением, принадлежит серверу и распределяется между клиентами на экземпляре сервера.</span><span class="sxs-lookup"><span data-stu-id="79d9d-222">In the Blazor Server scenario, the memory consumed by the app belongs to the server and is shared among clients on the server instance.</span></span>

<span data-ttu-id="79d9d-223">Требования к памяти на стороне сервера являются соображениями для всех приложений Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="79d9d-223">Server-side memory demands are a consideration for all Blazor Server apps.</span></span> <span data-ttu-id="79d9d-224">Однако большинство веб-приложений являются апосильными, и память, используемая при обработке запроса, освобождается при возврате ответа.</span><span class="sxs-lookup"><span data-stu-id="79d9d-224">However, most web apps are stateless, and the memory used while processing a request is released when the response is returned.</span></span> <span data-ttu-id="79d9d-225">В качестве общей рекомендации не позволяйте клиентам выделять неограниченное количество памяти, как в любом другом приложении на стороне сервера, которое сохраняет сятные клиентские соединения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-225">As a general recommendation, don't permit clients to allocate an unbound amount of memory as in any other server-side app that persists client connections.</span></span> <span data-ttu-id="79d9d-226">Память, потребляемая приложением Blazor Server, сохраняется дольше, чем один запрос.</span><span class="sxs-lookup"><span data-stu-id="79d9d-226">The memory consumed by a Blazor Server app persists for a longer time than a single request.</span></span>

> [!NOTE]
> <span data-ttu-id="79d9d-227">Во время разработки можно использовать профайлер или отслеживать, чтобы оценить требования клиентов к памяти.</span><span class="sxs-lookup"><span data-stu-id="79d9d-227">During development, a profiler can be used or a trace captured to assess memory demands of clients.</span></span> <span data-ttu-id="79d9d-228">Профайлер или трассировка не захватит память, выделенную определенному клиенту.</span><span class="sxs-lookup"><span data-stu-id="79d9d-228">A profiler or trace won't capture the memory allocated to a specific client.</span></span> <span data-ttu-id="79d9d-229">Чтобы запечатлеть использование памяти конкретного клиента во время разработки, захват свалки и изучить требование памяти всех объектов, коренится в цепи пользователя.</span><span class="sxs-lookup"><span data-stu-id="79d9d-229">To capture the memory use of a specific client during development, capture a dump and examine the memory demand of all the objects rooted at a user's circuit.</span></span>

### <a name="client-connections"></a><span data-ttu-id="79d9d-230">Клиентские подключения</span><span class="sxs-lookup"><span data-stu-id="79d9d-230">Client connections</span></span>

<span data-ttu-id="79d9d-231">Исчерпание связи может произойти, когда один или несколько клиентов открывают слишком много одновременных подключений к серверу, не позволяя другим клиентам устанавливать новые соединения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-231">Connection exhaustion can occur when one or more clients open too many concurrent connections to the server, preventing other clients from establishing new connections.</span></span>

<span data-ttu-id="79d9d-232">Клиенты Blazor устанавливают единое соединение за сеанс и сохраняют подключение открытым до тех пор, пока окно браузера открыто.</span><span class="sxs-lookup"><span data-stu-id="79d9d-232">Blazor clients establish a single connection per session and keep the connection open for as long as the browser window is open.</span></span> <span data-ttu-id="79d9d-233">Требования к серверу обслуживания всех подключений не специфичны для приложений Blazor.</span><span class="sxs-lookup"><span data-stu-id="79d9d-233">The demands on the server of maintaining all of the connections isn't specific to Blazor apps.</span></span> <span data-ttu-id="79d9d-234">Учитывая постоянный характер подключений и состояние приложений Blazor Server, исчерпание связи является большим риском для доступности приложения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-234">Given the persistent nature of the connections and the stateful nature of Blazor Server apps, connection exhaustion is a greater risk to availability of the app.</span></span>

<span data-ttu-id="79d9d-235">По умолчанию количество подключений на одного пользователя для приложения Blazor Server не ограничено.</span><span class="sxs-lookup"><span data-stu-id="79d9d-235">By default, there's no limit on the number of connections per user for a Blazor Server app.</span></span> <span data-ttu-id="79d9d-236">Если приложение требует ограничения соединения, возьмите один или несколько из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="79d9d-236">If the app requires a connection limit, take one or more of the following approaches:</span></span>

* <span data-ttu-id="79d9d-237">Требуется аутентификация, что, естественно, ограничивает возможность несанкционированного подключения пользователей к приложению.</span><span class="sxs-lookup"><span data-stu-id="79d9d-237">Require authentication, which naturally limits the ability of unauthorized users to connect to the app.</span></span> <span data-ttu-id="79d9d-238">Для того чтобы этот сценарий был эффективным, пользователи должны быть лишены возможности подготовить новых пользователей по своей воле.</span><span class="sxs-lookup"><span data-stu-id="79d9d-238">For this scenario to be effective, users must be prevented from provisioning new users at will.</span></span>
* <span data-ttu-id="79d9d-239">Ограничьте количество подключений на пользователя.</span><span class="sxs-lookup"><span data-stu-id="79d9d-239">Limit the number of connections per user.</span></span> <span data-ttu-id="79d9d-240">Ограничение соединений может быть достигнуто с помощью следующих подходов.</span><span class="sxs-lookup"><span data-stu-id="79d9d-240">Limiting connections can be accomplished via the following approaches.</span></span> <span data-ttu-id="79d9d-241">Упражнение, чтобы позволить законным пользователям получить доступ к приложению (например, когда установлен лимит соединения на основе IP-адреса клиента).</span><span class="sxs-lookup"><span data-stu-id="79d9d-241">Exercise care to allow legitimate users to access the app (for example, when a connection limit is established based on the client's IP address).</span></span>
  * <span data-ttu-id="79d9d-242">На уровне приложения:</span><span class="sxs-lookup"><span data-stu-id="79d9d-242">At the app level:</span></span>
    * <span data-ttu-id="79d9d-243">Расширяемость конечных точек.</span><span class="sxs-lookup"><span data-stu-id="79d9d-243">Endpoint routing extensibility.</span></span>
    * <span data-ttu-id="79d9d-244">Требуется аутентификация для подключения к приложению и отслеживания активных сеансов на пользователя.</span><span class="sxs-lookup"><span data-stu-id="79d9d-244">Require authentication to connect to the app and keep track of the active sessions per user.</span></span>
    * <span data-ttu-id="79d9d-245">Отклонить новые сессии по достижении предела.</span><span class="sxs-lookup"><span data-stu-id="79d9d-245">Reject new sessions upon reaching a limit.</span></span>
    * <span data-ttu-id="79d9d-246">Прокси WebSocket подключается к приложению с помощью прокси- и прокси, такой как [служба Azure SignalR,](/azure/azure-signalr/signalr-overview) которая мультиплексирует подключение клиентов к приложению.</span><span class="sxs-lookup"><span data-stu-id="79d9d-246">Proxy WebSocket connections to an app through the use of a proxy, such as the [Azure SignalR Service](/azure/azure-signalr/signalr-overview) that multiplexes connections from clients to an app.</span></span> <span data-ttu-id="79d9d-247">Это обеспечивает приложение с большей емкостью соединения, чем один клиент может установить, предотвращая клиента от исчерпания соединений с сервером.</span><span class="sxs-lookup"><span data-stu-id="79d9d-247">This provides an app with greater connection capacity than a single client can establish, preventing a client from exhausting the connections to the server.</span></span>
  * <span data-ttu-id="79d9d-248">На уровне сервера: Используйте прокси/шлюз перед приложением.</span><span class="sxs-lookup"><span data-stu-id="79d9d-248">At the server level: Use a proxy/gateway in front of the app.</span></span> <span data-ttu-id="79d9d-249">Например, [Azure Front Door](/azure/frontdoor/front-door-overview) позволяет определять, управлять и контролировать глобальную передвижение веб-трафика в приложение.</span><span class="sxs-lookup"><span data-stu-id="79d9d-249">For example, [Azure Front Door](/azure/frontdoor/front-door-overview) enables you to define, manage, and monitor the global routing of web traffic to an app.</span></span>

## <a name="denial-of-service-dos-attacks"></a><span data-ttu-id="79d9d-250">Атаки типа «отказ в обслуживании» (DoS)</span><span class="sxs-lookup"><span data-stu-id="79d9d-250">Denial of service (DoS) attacks</span></span>

<span data-ttu-id="79d9d-251">Отказ в обслуживании (DoS) атаки связаны с клиентом, заставляя сервер исчерпать один или несколько своих ресурсов, что делает приложение недоступным.</span><span class="sxs-lookup"><span data-stu-id="79d9d-251">Denial of service (DoS) attacks involve a client causing the server to exhaust one or more of its resources making the app unavailable.</span></span> <span data-ttu-id="79d9d-252">Приложения Blazor Server включают некоторые ограничения по умолчанию и полагаются на другие ограничения ASP.NET core и SignalR для защиты от DoS-атак:</span><span class="sxs-lookup"><span data-stu-id="79d9d-252">Blazor Server apps include some default limits and rely on other ASP.NET Core and SignalR limits to protect against DoS attacks:</span></span>

| <span data-ttu-id="79d9d-253">Лимит приложения Blazor Server</span><span class="sxs-lookup"><span data-stu-id="79d9d-253">Blazor Server app limit</span></span>                            | <span data-ttu-id="79d9d-254">Описание</span><span class="sxs-lookup"><span data-stu-id="79d9d-254">Description</span></span> | <span data-ttu-id="79d9d-255">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="79d9d-255">Default</span></span> |
| ------------------------------------------------------- | ----------- | ------- |
| `CircuitOptions.DisconnectedCircuitMaxRetained`         | <span data-ttu-id="79d9d-256">Максимальное количество отключенных цепей, которые данный сервер удерживает в памяти одновременно.</span><span class="sxs-lookup"><span data-stu-id="79d9d-256">Maximum number of disconnected circuits that a given server holds in memory at a time.</span></span> | <span data-ttu-id="79d9d-257">100</span><span class="sxs-lookup"><span data-stu-id="79d9d-257">100</span></span> |
| `CircuitOptions.DisconnectedCircuitRetentionPeriod`     | <span data-ttu-id="79d9d-258">Максимальное количество времени, когда отключенная цепь удерживается в памяти перед тем, как быть снесенной.</span><span class="sxs-lookup"><span data-stu-id="79d9d-258">Maximum amount of time a disconnected circuit is held in memory before being torn down.</span></span> | <span data-ttu-id="79d9d-259">3 минуты</span><span class="sxs-lookup"><span data-stu-id="79d9d-259">3 minutes</span></span> |
| `CircuitOptions.JSInteropDefaultCallTimeout`            | <span data-ttu-id="79d9d-260">Максимальное время сервера ждет до синхронизации асинхронного вызова функции JavaScript.</span><span class="sxs-lookup"><span data-stu-id="79d9d-260">Maximum amount of time the server waits before timing out an asynchronous JavaScript function invocation.</span></span> | <span data-ttu-id="79d9d-261">1 минута</span><span class="sxs-lookup"><span data-stu-id="79d9d-261">1 minute</span></span> |
| `CircuitOptions.MaxBufferedUnacknowledgedRenderBatches` | <span data-ttu-id="79d9d-262">Максимальное количество неподтвержденных пакетов рендеров, которые сервер сохраняет в памяти на каждую цепь в данный момент времени для поддержки надежного переподключения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-262">Maximum number of unacknowledged render batches the server keeps in memory per circuit at a given time to support robust reconnection.</span></span> <span data-ttu-id="79d9d-263">После достижения предела сервер прекращает производство новых партий рендеров до тех пор, пока одна или несколько партий не будут признаны клиентом.</span><span class="sxs-lookup"><span data-stu-id="79d9d-263">After reaching the limit, the server stops producing new render batches until one or more batches have been acknowledged by the client.</span></span> | <span data-ttu-id="79d9d-264">10</span><span class="sxs-lookup"><span data-stu-id="79d9d-264">10</span></span> |


| <span data-ttu-id="79d9d-265">SignalR и ASP.NET основных пределов</span><span class="sxs-lookup"><span data-stu-id="79d9d-265">SignalR and ASP.NET Core limit</span></span>             | <span data-ttu-id="79d9d-266">Описание</span><span class="sxs-lookup"><span data-stu-id="79d9d-266">Description</span></span> | <span data-ttu-id="79d9d-267">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="79d9d-267">Default</span></span> |
| ------------------------------------------ | ----------- | ------- |
| `CircuitOptions.MaximumReceiveMessageSize` | <span data-ttu-id="79d9d-268">Размер сообщения для отдельного сообщения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-268">Message size for an individual message.</span></span> | <span data-ttu-id="79d9d-269">32 КБ</span><span class="sxs-lookup"><span data-stu-id="79d9d-269">32 KB</span></span> |

## <a name="interactions-with-the-browser-client"></a><span data-ttu-id="79d9d-270">Взаимодействие с браузером (клиент)</span><span class="sxs-lookup"><span data-stu-id="79d9d-270">Interactions with the browser (client)</span></span>

<span data-ttu-id="79d9d-271">Клиент взаимодействует с сервером через JS interop отправки событий и завершения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-271">A client interacts with the server through JS interop event dispatching and render completion.</span></span> <span data-ttu-id="79d9d-272">JS interop связи идет в обоих направлениях между JavaScript и .NET:</span><span class="sxs-lookup"><span data-stu-id="79d9d-272">JS interop communication goes both ways between JavaScript and .NET:</span></span>

* <span data-ttu-id="79d9d-273">События браузера отправляются от клиента к серверу асинхронным способом.</span><span class="sxs-lookup"><span data-stu-id="79d9d-273">Browser events are dispatched from the client to the server in an asynchronous fashion.</span></span>
* <span data-ttu-id="79d9d-274">Сервер реагирует асинхронно rerendering uI по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="79d9d-274">The server responds asynchronously rerendering the UI as necessary.</span></span>

### <a name="javascript-functions-invoked-from-net"></a><span data-ttu-id="79d9d-275">Функции JavaScript, вызванные из .NET</span><span class="sxs-lookup"><span data-stu-id="79d9d-275">JavaScript functions invoked from .NET</span></span>

<span data-ttu-id="79d9d-276">Для звонков от методов .NET до JavaScript:</span><span class="sxs-lookup"><span data-stu-id="79d9d-276">For calls from .NET methods to JavaScript:</span></span>

* <span data-ttu-id="79d9d-277">Все вызовы имеют настраиваемый тайм-аут, после <xref:System.OperationCanceledException> которого они терпят неудачу, возвращая сью к вызывающему абоненту.</span><span class="sxs-lookup"><span data-stu-id="79d9d-277">All invocations have a configurable timeout after which they fail, returning a <xref:System.OperationCanceledException> to the caller.</span></span>
  * <span data-ttu-id="79d9d-278">Там в по умолчанию тайм-аут для звонков ()`CircuitOptions.JSInteropDefaultCallTimeout`в одну минуту.</span><span class="sxs-lookup"><span data-stu-id="79d9d-278">There's a default timeout for the calls (`CircuitOptions.JSInteropDefaultCallTimeout`) of one minute.</span></span> <span data-ttu-id="79d9d-279">Чтобы настроить этот предел, см. <xref:blazor/call-javascript-from-dotnet#harden-js-interop-calls></span><span class="sxs-lookup"><span data-stu-id="79d9d-279">To configure this limit, see <xref:blazor/call-javascript-from-dotnet#harden-js-interop-calls>.</span></span>
  * <span data-ttu-id="79d9d-280">Токен отмены может быть предоставлен для контроля за отменой на основе по счету.</span><span class="sxs-lookup"><span data-stu-id="79d9d-280">A cancellation token can be provided to control the cancellation on a per-call basis.</span></span> <span data-ttu-id="79d9d-281">По умолчанию тайм-аут вызова по умолчанию, где это возможно, и ограниченный по времени любой звонок клиенту, если предоставляется токен отмены.</span><span class="sxs-lookup"><span data-stu-id="79d9d-281">Rely on the default call timeout where possible and time-bound any call to the client if a cancellation token is provided.</span></span>
* <span data-ttu-id="79d9d-282">Результатвызова вызова JavaScript нельзя доверять.</span><span class="sxs-lookup"><span data-stu-id="79d9d-282">The result of a JavaScript call can't be trusted.</span></span> <span data-ttu-id="79d9d-283">Клиент Blazor приложения, работающий в браузере, ищет функцию JavaScript для ввода.</span><span class="sxs-lookup"><span data-stu-id="79d9d-283">The Blazor app client running in the browser searches for the JavaScript function to invoke.</span></span> <span data-ttu-id="79d9d-284">Функция вызывается, и либо результат, либо ошибка производится.</span><span class="sxs-lookup"><span data-stu-id="79d9d-284">The function is invoked, and either the result or an error is produced.</span></span> <span data-ttu-id="79d9d-285">Злоумышленник может попытаться:</span><span class="sxs-lookup"><span data-stu-id="79d9d-285">A malicious client can attempt to:</span></span>
  * <span data-ttu-id="79d9d-286">Причина проблемы в приложении, вернув ошибку из функции JavaScript.</span><span class="sxs-lookup"><span data-stu-id="79d9d-286">Cause an issue in the app by returning an error from the JavaScript function.</span></span>
  * <span data-ttu-id="79d9d-287">Вызвать непреднамеренное поведение на сервере, вернув неожиданный результат из функции JavaScript.</span><span class="sxs-lookup"><span data-stu-id="79d9d-287">Induce an unintended behavior on the server by returning an unexpected result from the JavaScript function.</span></span>

<span data-ttu-id="79d9d-288">Примите следующие меры предосторожности, чтобы остерегаться предыдущих сценариев:</span><span class="sxs-lookup"><span data-stu-id="79d9d-288">Take the following precautions to guard against the preceding scenarios:</span></span>

* <span data-ttu-id="79d9d-289">Оберните вызовы JS interop в [инструкциях try-catch](/dotnet/csharp/language-reference/keywords/try-catch) для учета ошибок, которые могут возникнуть во время вызовов.</span><span class="sxs-lookup"><span data-stu-id="79d9d-289">Wrap JS interop calls within [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements to account for errors that might occur during the invocations.</span></span> <span data-ttu-id="79d9d-290">Для получения дополнительной информации см. <xref:blazor/handle-errors#javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="79d9d-290">For more information, see <xref:blazor/handle-errors#javascript-interop>.</span></span>
* <span data-ttu-id="79d9d-291">Проверка данных, возвращенных из вызовов JS interop, включая сообщения об ошибках, перед принятием каких-либо действий.</span><span class="sxs-lookup"><span data-stu-id="79d9d-291">Validate data returned from JS interop invocations, including error messages, before taking any action.</span></span>

### <a name="net-methods-invoked-from-the-browser"></a><span data-ttu-id="79d9d-292">Методы .NET, вызванные из браузера</span><span class="sxs-lookup"><span data-stu-id="79d9d-292">.NET methods invoked from the browser</span></span>

<span data-ttu-id="79d9d-293">Не доверяйте вызовам от методов JavaScript к .NET.</span><span class="sxs-lookup"><span data-stu-id="79d9d-293">Don't trust calls from JavaScript to .NET methods.</span></span> <span data-ttu-id="79d9d-294">При воздействии метода JavaScript метода .NET подумайте о том, как вызывается метод .NET:</span><span class="sxs-lookup"><span data-stu-id="79d9d-294">When a .NET method is exposed to JavaScript, consider how the .NET method is invoked:</span></span>

* <span data-ttu-id="79d9d-295">Относитесь к любому методу .NET, подверженным JavaScript, как к публичной конечной точке приложения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-295">Treat any .NET method exposed to JavaScript as you would a public endpoint to the app.</span></span>
  * <span data-ttu-id="79d9d-296">Проверка ввода.</span><span class="sxs-lookup"><span data-stu-id="79d9d-296">Validate input.</span></span>
    * <span data-ttu-id="79d9d-297">Убедитесь, что значения находятся в пределах ожидаемых диапазонов.</span><span class="sxs-lookup"><span data-stu-id="79d9d-297">Ensure that values are within expected ranges.</span></span>
    * <span data-ttu-id="79d9d-298">Убедитесь, что пользователь имеет разрешение на выполнение запрошенного действия.</span><span class="sxs-lookup"><span data-stu-id="79d9d-298">Ensure that the user has permission to perform the action requested.</span></span>
  * <span data-ttu-id="79d9d-299">Не выделяйте чрезмерное количество ресурсов в рамках вызова метода .NET.</span><span class="sxs-lookup"><span data-stu-id="79d9d-299">Don't allocate an excessive quantity of resources as part of the .NET method invocation.</span></span> <span data-ttu-id="79d9d-300">Например, выполните проверки и уместите ограничения на использование процессора и памяти.</span><span class="sxs-lookup"><span data-stu-id="79d9d-300">For example, perform checks and place limits on CPU and memory use.</span></span>
  * <span data-ttu-id="79d9d-301">Примите во внимание, что статические и экземплярные методы могут быть подвержены воздействию клиентов JavaScript.</span><span class="sxs-lookup"><span data-stu-id="79d9d-301">Take into account that static and instance methods can be exposed to JavaScript clients.</span></span> <span data-ttu-id="79d9d-302">Избегайте совместного использования состояния между сеансами, если в проекте не содержится призывки к совместному использованию состояния с соответствующими ограничениями.</span><span class="sxs-lookup"><span data-stu-id="79d9d-302">Avoid sharing state across sessions unless the design calls for sharing state with appropriate constraints.</span></span>
    * <span data-ttu-id="79d9d-303">Например, методы, подвергающиеся воздействию `DotNetReference` объектов, которые первоначально создаются в результате инъекции зависимости (DI), объекты должны быть зарегистрированы в качестве объектов, на которые будут созданы.</span><span class="sxs-lookup"><span data-stu-id="79d9d-303">For instance methods exposed through `DotNetReference` objects that are originally created through dependency injection (DI), the objects should be registered as scoped objects.</span></span> <span data-ttu-id="79d9d-304">Это относится к любой Blazor службе DI, которую использует приложение Server.</span><span class="sxs-lookup"><span data-stu-id="79d9d-304">This applies to any DI service that the Blazor Server app uses.</span></span>
    * <span data-ttu-id="79d9d-305">Для статических методов избегайте создания состояния, которое не может быть передано клиенту, если приложение явно не делится по-своему состоянием между всеми пользователями на экземпляре сервера.</span><span class="sxs-lookup"><span data-stu-id="79d9d-305">For static methods, avoid establishing state that can't be scoped to the client unless the app is explicitly sharing state by-design across all users on a server instance.</span></span>
  * <span data-ttu-id="79d9d-306">Избегайте передачи данных, предоставленных пользователями в параметрах, вызовам JavaScript.</span><span class="sxs-lookup"><span data-stu-id="79d9d-306">Avoid passing user-supplied data in parameters to JavaScript calls.</span></span> <span data-ttu-id="79d9d-307">Если передача данных по параметрам абсолютно необходима, убедитесь, что код JavaScript обрабатывает передачу данных без введения уязвимостей [кросс-сайтных скриптов (XSS).](#cross-site-scripting-xss)</span><span class="sxs-lookup"><span data-stu-id="79d9d-307">If passing data in parameters is absolutely required, ensure that the JavaScript code handles passing the data without introducing [Cross-site scripting (XSS)](#cross-site-scripting-xss) vulnerabilities.</span></span> <span data-ttu-id="79d9d-308">Например, не записывайте данные, предоставленные пользователем, `innerHTML` в модель объекта документа (DOM), установив свойство элемента.</span><span class="sxs-lookup"><span data-stu-id="79d9d-308">For example, don't write user-supplied data to the Document Object Model (DOM) by setting the `innerHTML` property of an element.</span></span> <span data-ttu-id="79d9d-309">Рассмотрите возможность использования [политики безопасности содержимого (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) для отстранивания `eval` и других небезопасных примитивов JavaScript.</span><span class="sxs-lookup"><span data-stu-id="79d9d-309">Consider using [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) to disable `eval` and other unsafe JavaScript primitives.</span></span>
* <span data-ttu-id="79d9d-310">Избегайте реализации пользовательской отправки вызовов .NET поверх реализации диспетчерской платформы.</span><span class="sxs-lookup"><span data-stu-id="79d9d-310">Avoid implementing custom dispatching of .NET invocations on top of the framework's dispatching implementation.</span></span> <span data-ttu-id="79d9d-311">Разоблачение методов .NET в браузере является продвинутым сценарием, не рекомендованный для общей Blazor разработки.</span><span class="sxs-lookup"><span data-stu-id="79d9d-311">Exposing .NET methods to the browser is an advanced scenario, not recommended for general Blazor development.</span></span>

### <a name="events"></a><span data-ttu-id="79d9d-312">События</span><span class="sxs-lookup"><span data-stu-id="79d9d-312">Events</span></span>

<span data-ttu-id="79d9d-313">События предоставляют точку Blazor входа в приложение Server.</span><span class="sxs-lookup"><span data-stu-id="79d9d-313">Events provide an entry point to a Blazor Server app.</span></span> <span data-ttu-id="79d9d-314">Те же правила защиты конечных точек в Blazor веб-приложениях применяются к обработке событий в приложениях Server.</span><span class="sxs-lookup"><span data-stu-id="79d9d-314">The same rules for safeguarding endpoints in web apps apply to event handling in Blazor Server apps.</span></span> <span data-ttu-id="79d9d-315">Злоумышленник может отправить любые данные, которые он хочет отправить в качестве полезной нагрузки для события.</span><span class="sxs-lookup"><span data-stu-id="79d9d-315">A malicious client can send any data it wishes to send as the payload for an event.</span></span>

<span data-ttu-id="79d9d-316">Пример:</span><span class="sxs-lookup"><span data-stu-id="79d9d-316">For example:</span></span>

* <span data-ttu-id="79d9d-317">Событие изменения для `<select>` может отправить значение, которое не входит в параметры, которые приложение представило клиенту.</span><span class="sxs-lookup"><span data-stu-id="79d9d-317">A change event for a `<select>` could send a value that isn't within the options that the app presented to the client.</span></span>
* <span data-ttu-id="79d9d-318">An `<input>` может отправлять любые текстовые данные на сервер, минуя проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="79d9d-318">An `<input>` could send any text data to the server, bypassing client-side validation.</span></span>

<span data-ttu-id="79d9d-319">Приложение должно проверить данные для любого события, которое обрабатывает приложение.</span><span class="sxs-lookup"><span data-stu-id="79d9d-319">The app must validate the data for any event that the app handles.</span></span> <span data-ttu-id="79d9d-320">Компоненты Blazor каркасных [форм](xref:blazor/forms-validation) выполняют основные проверки.</span><span class="sxs-lookup"><span data-stu-id="79d9d-320">The Blazor framework [forms components](xref:blazor/forms-validation) perform basic validations.</span></span> <span data-ttu-id="79d9d-321">Если приложение использует компоненты пользовательских форм, пользовательский код должен быть написан для проверки данных событий по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="79d9d-321">If the app uses custom forms components, custom code must be written to validate event data as appropriate.</span></span>

Blazor<span data-ttu-id="79d9d-322">События сервера являются асинхронными, поэтому несколько событий могут быть отправлены на сервер до того, как приложение успеет отреагировать, создав новый рендер.</span><span class="sxs-lookup"><span data-stu-id="79d9d-322"> Server events are asynchronous, so multiple events can be dispatched to the server before the app has time to react by producing a new render.</span></span> <span data-ttu-id="79d9d-323">Это имеет некоторые последствия для безопасности.</span><span class="sxs-lookup"><span data-stu-id="79d9d-323">This has some security implications to consider.</span></span> <span data-ttu-id="79d9d-324">Ограничение действий клиента в приложении должно выполняться внутри обработчиков событий и не зависеть от текущего состояния отобрасированного представления.</span><span class="sxs-lookup"><span data-stu-id="79d9d-324">Limiting client actions in the app must be performed inside event handlers and not depend on the current rendered view state.</span></span>

<span data-ttu-id="79d9d-325">Рассмотрим встречный компонент, который должен позволить пользователю приравлить счетчик максимум в три раза.</span><span class="sxs-lookup"><span data-stu-id="79d9d-325">Consider a counter component that should allow a user to increment a counter a maximum of three times.</span></span> <span data-ttu-id="79d9d-326">Кнопка для приращения `count`счетчика условно зависит от значения:</span><span class="sxs-lookup"><span data-stu-id="79d9d-326">The button to increment the counter is conditionally based on the value of `count`:</span></span>

```razor
<p>Count: @count<p>

@if (count < 3)
{
    <button @onclick="IncrementCount" value="Increment count" />
}

@code 
{
    private int count = 0;

    private void IncrementCount()
    {
        count++;
    }
}
```

<span data-ttu-id="79d9d-327">Клиент может отправить одно или несколько событий приращения до того, как платформа производит новый рендер этого компонента.</span><span class="sxs-lookup"><span data-stu-id="79d9d-327">A client can dispatch one or more increment events before the framework produces a new render of this component.</span></span> <span data-ttu-id="79d9d-328">В результате пользователь `count` может приравлять ее *более трех раз,* поскольку кнопка не удаляется пользовательским интерфейсом достаточно быстро.</span><span class="sxs-lookup"><span data-stu-id="79d9d-328">The result is that the `count` can be incremented *over three times* by the user because the button isn't removed by the UI quickly enough.</span></span> <span data-ttu-id="79d9d-329">Правильный способ достижения предела `count` в три приращения показан в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="79d9d-329">The correct way to achieve the limit of three `count` increments is shown in the following example:</span></span>

```razor
<p>Count: @count<p>

@if (count < 3)
{
    <button @onclick="IncrementCount" value="Increment count" />
}

@code 
{
    private int count = 0;

    private void IncrementCount()
    {
        if (count < 3)
        {
            count++;
        }
    }
}
```

<span data-ttu-id="79d9d-330">Добавляя `if (count < 3) { ... }` чек внутри обработчика, решение `count` о приращении основано на текущем состоянии приложения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-330">By adding the `if (count < 3) { ... }` check inside the handler, the decision to increment `count` is based on the current app state.</span></span> <span data-ttu-id="79d9d-331">Решение не основано на состоянии uI, как это было в предыдущем примере, которое может быть временно устаревшим.</span><span class="sxs-lookup"><span data-stu-id="79d9d-331">The decision isn't based on the state of the UI as it was in the previous example, which might be temporarily stale.</span></span>

### <a name="guard-against-multiple-dispatches"></a><span data-ttu-id="79d9d-332">Защитите от нескольких диспетчеров</span><span class="sxs-lookup"><span data-stu-id="79d9d-332">Guard against multiple dispatches</span></span>

<span data-ttu-id="79d9d-333">Если обратный вызов события вызывает длительную операцию асинхронно, например, получение данных из внешней службы или базы данных, рассмотрите возможность использования охранника.</span><span class="sxs-lookup"><span data-stu-id="79d9d-333">If an event callback invokes a long running operation asynchronously, such as fetching data from an external service or database, consider using a guard.</span></span> <span data-ttu-id="79d9d-334">Охранник может предотвратить пользователя от очереди до нескольких операций в то время как операция продолжается с визуальной обратной связи.</span><span class="sxs-lookup"><span data-stu-id="79d9d-334">The guard can prevent the user from queueing up multiple operations while the operation is in progress with visual feedback.</span></span> <span data-ttu-id="79d9d-335">Следующий компонент `isLoading` код `true` `GetForecastAsync` устанавливает в то время как получение данных с сервера.</span><span class="sxs-lookup"><span data-stu-id="79d9d-335">The following component code sets `isLoading` to `true` while `GetForecastAsync` obtains data from the server.</span></span> <span data-ttu-id="79d9d-336">В `isLoading` `true`то время как, кнопка отключена в uI:</span><span class="sxs-lookup"><span data-stu-id="79d9d-336">While `isLoading` is `true`, the button is disabled in the UI:</span></span>

```razor
@page "/fetchdata"
@using BlazorServerSample.Data
@inject WeatherForecastService ForecastService

<button disabled="@isLoading" @onclick="UpdateForecasts">Update</button>

@code {
    private bool isLoading;
    private WeatherForecast[] forecasts;

    private async Task UpdateForecasts()
    {
        if (!isLoading)
        {
            isLoading = true;
            forecasts = await ForecastService.GetForecastAsync(DateTime.Now);
            isLoading = false;
        }
    }
}
```

<span data-ttu-id="79d9d-337">Шаблон охраны, показанный в предыдущем примере, работает, если фоновая `async` - `await` операция выполняется асинхронно с шаблоном.</span><span class="sxs-lookup"><span data-stu-id="79d9d-337">The guard pattern demonstrated in the preceding example works if the background operation is executed asynchronously with the `async`-`await` pattern.</span></span>

### <a name="cancel-early-and-avoid-use-after-dispose"></a><span data-ttu-id="79d9d-338">Отмена досрочно и избегайте использования после удаления</span><span class="sxs-lookup"><span data-stu-id="79d9d-338">Cancel early and avoid use-after-dispose</span></span>

<span data-ttu-id="79d9d-339">В дополнение к использованию охранника, как описано в <xref:System.Threading.CancellationToken> [разделе Охрана против нескольких диспетчеров,](#guard-against-multiple-dispatches) рассмотреть возможность использования для отмены длительных операций, когда компонент удаляется.</span><span class="sxs-lookup"><span data-stu-id="79d9d-339">In addition to using a guard as described in the [Guard against multiple dispatches](#guard-against-multiple-dispatches) section, consider using a <xref:System.Threading.CancellationToken> to cancel long-running operations when the component is disposed.</span></span> <span data-ttu-id="79d9d-340">Этот подход имеет дополнительное преимущество, избегая *использования после удаления* компонентов:</span><span class="sxs-lookup"><span data-stu-id="79d9d-340">This approach has the added benefit of avoiding *use-after-dispose* in components:</span></span>

```razor
@implements IDisposable

...

@code {
    private readonly CancellationTokenSource TokenSource = 
        new CancellationTokenSource();

    private async Task UpdateForecasts()
    {
        ...

        forecasts = await ForecastService.GetForecastAsync(DateTime.Now, 
            TokenSource.Token);

        if (TokenSource.Token.IsCancellationRequested)
        {
           return;
        }

        ...
    }

    public void Dispose()
    {
        TokenSource.Cancel();
    }
}
```

### <a name="avoid-events-that-produce-large-amounts-of-data"></a><span data-ttu-id="79d9d-341">Избегайте событий, которые производят большие объемы данных</span><span class="sxs-lookup"><span data-stu-id="79d9d-341">Avoid events that produce large amounts of data</span></span>

<span data-ttu-id="79d9d-342">Некоторые события DOM, `oninput` `onscroll`такие как или могут создавать большой объем данных.</span><span class="sxs-lookup"><span data-stu-id="79d9d-342">Some DOM events, such as `oninput` or `onscroll`, can produce a large amount of data.</span></span> <span data-ttu-id="79d9d-343">Избегайте использования Blazor этих событий в приложениях сервера.</span><span class="sxs-lookup"><span data-stu-id="79d9d-343">Avoid using these events in Blazor server apps.</span></span>

## <a name="additional-security-guidance"></a><span data-ttu-id="79d9d-344">Дополнительное руководство по безопасности</span><span class="sxs-lookup"><span data-stu-id="79d9d-344">Additional security guidance</span></span>

<span data-ttu-id="79d9d-345">Рекомендации по обеспечению безопасности Blazor ASP.NET приложения Core применяются к приложениям Server и охватываются следующими разделами:</span><span class="sxs-lookup"><span data-stu-id="79d9d-345">The guidance for securing ASP.NET Core apps apply to Blazor Server apps and are covered in the following sections:</span></span>

* [<span data-ttu-id="79d9d-346">Регистрация и конфиденциальные данные</span><span class="sxs-lookup"><span data-stu-id="79d9d-346">Logging and sensitive data</span></span>](#logging-and-sensitive-data)
* [<span data-ttu-id="79d9d-347">Защита информации в пути с помощью HTTPS</span><span class="sxs-lookup"><span data-stu-id="79d9d-347">Protect information in transit with HTTPS</span></span>](#protect-information-in-transit-with-https)
* <span data-ttu-id="79d9d-348">[Кросс-сайт сценарий (XSS)](#cross-site-scripting-xss))</span><span class="sxs-lookup"><span data-stu-id="79d9d-348">[Cross-site scripting (XSS)](#cross-site-scripting-xss))</span></span>
* [<span data-ttu-id="79d9d-349">Защита от крестного происхождения</span><span class="sxs-lookup"><span data-stu-id="79d9d-349">Cross-origin protection</span></span>](#cross-origin-protection)
* [<span data-ttu-id="79d9d-350">Нажмите-джекинг</span><span class="sxs-lookup"><span data-stu-id="79d9d-350">Click-jacking</span></span>](#click-jacking)
* [<span data-ttu-id="79d9d-351">Открытые перенаправления</span><span class="sxs-lookup"><span data-stu-id="79d9d-351">Open redirects</span></span>](#open-redirects)

### <a name="logging-and-sensitive-data"></a><span data-ttu-id="79d9d-352">Регистрация и конфиденциальные данные</span><span class="sxs-lookup"><span data-stu-id="79d9d-352">Logging and sensitive data</span></span>

<span data-ttu-id="79d9d-353">Взаимодействия Между клиентом и сервером JS интероп записываются <xref:Microsoft.Extensions.Logging.ILogger> в журналы сервера с экземплярами.</span><span class="sxs-lookup"><span data-stu-id="79d9d-353">JS interop interactions between the client and server are recorded in the server's logs with <xref:Microsoft.Extensions.Logging.ILogger> instances.</span></span> Blazor<span data-ttu-id="79d9d-354">избегает регистрации конфиденциальной информации, такой как фактические события или вводы и выводы JS interop.</span><span class="sxs-lookup"><span data-stu-id="79d9d-354"> avoids logging sensitive information, such as actual events or JS interop inputs and outputs.</span></span>

<span data-ttu-id="79d9d-355">При возникновении ошибки на сервере фреймворк уведомляет клиента и срывает сеанс.</span><span class="sxs-lookup"><span data-stu-id="79d9d-355">When an error occurs on the server, the framework notifies the client and tears down the session.</span></span> <span data-ttu-id="79d9d-356">По умолчанию клиент получает общее сообщение об ошибке, которое можно увидеть в инструментах разработчика браузера.</span><span class="sxs-lookup"><span data-stu-id="79d9d-356">By default, the client receives a generic error message that can be seen in the browser's developer tools.</span></span>

<span data-ttu-id="79d9d-357">Ошибка на стороне клиента не включает стек вызовов и не содержит подробной информации о причине ошибки, но журналы сервера содержат такую информацию.</span><span class="sxs-lookup"><span data-stu-id="79d9d-357">The client-side error doesn't include the callstack and doesn't provide detail on the cause of the error, but server logs do contain such information.</span></span> <span data-ttu-id="79d9d-358">Для целей разработки конфиденциальная информация об ошибках может быть доступна клиенту, включив подробные ошибки.</span><span class="sxs-lookup"><span data-stu-id="79d9d-358">For development purposes, sensitive error information can be made available to the client by enabling detailed errors.</span></span>

<span data-ttu-id="79d9d-359">Включить подробные ошибки с:</span><span class="sxs-lookup"><span data-stu-id="79d9d-359">Enable detailed errors with:</span></span>

* <span data-ttu-id="79d9d-360">`CircuitOptions.DetailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="79d9d-360">`CircuitOptions.DetailedErrors`.</span></span>
* <span data-ttu-id="79d9d-361">`DetailedErrors`конфигурация ключа.</span><span class="sxs-lookup"><span data-stu-id="79d9d-361">`DetailedErrors` configuration key.</span></span> <span data-ttu-id="79d9d-362">Например, установите переменную среды `ASPNETCORE_DETAILEDERRORS` на значение . `true`</span><span class="sxs-lookup"><span data-stu-id="79d9d-362">For example, set the `ASPNETCORE_DETAILEDERRORS` environment variable to a value of `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="79d9d-363">Разоблачение информации об ошибках для клиентов в Интернете является угрозой безопасности, которую всегда следует избегать.</span><span class="sxs-lookup"><span data-stu-id="79d9d-363">Exposing error information to clients on the Internet is a security risk that should always be avoided.</span></span>

### <a name="protect-information-in-transit-with-https"></a><span data-ttu-id="79d9d-364">Защита информации в пути с помощью HTTPS</span><span class="sxs-lookup"><span data-stu-id="79d9d-364">Protect information in transit with HTTPS</span></span>

Blazor<span data-ttu-id="79d9d-365">Сервер SignalR используется для связи между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="79d9d-365"> Server uses SignalR for communication between the client and the server.</span></span> Blazor<span data-ttu-id="79d9d-366">Сервер обычно использует транспорт, который SignalR ведет переговоры, который обычно WebSockets.</span><span class="sxs-lookup"><span data-stu-id="79d9d-366"> Server normally uses the transport that SignalR negotiates, which is typically WebSockets.</span></span>

Blazor<span data-ttu-id="79d9d-367">Сервер не обеспечивает целостность и конфиденциальность данных, отправляемых между сервером и клиентом.</span><span class="sxs-lookup"><span data-stu-id="79d9d-367"> Server doesn't ensure the integrity and confidentiality of the data sent between the server and the client.</span></span> <span data-ttu-id="79d9d-368">Всегда используйте HTTPS.</span><span class="sxs-lookup"><span data-stu-id="79d9d-368">Always use HTTPS.</span></span>

### <a name="cross-site-scripting-xss"></a><span data-ttu-id="79d9d-369">Кросс-сайт сценарий (XSS)</span><span class="sxs-lookup"><span data-stu-id="79d9d-369">Cross-site scripting (XSS)</span></span>

<span data-ttu-id="79d9d-370">Кросс-сайт скриптов (XSS) позволяет несанкционированной стороне выполнять произвольные логики в контексте браузера.</span><span class="sxs-lookup"><span data-stu-id="79d9d-370">Cross-site scripting (XSS) allows an unauthorized party to execute arbitrary logic in the context of the browser.</span></span> <span data-ttu-id="79d9d-371">Скомпрометированное приложение потенциально может запустить произвольный код на клиенте.</span><span class="sxs-lookup"><span data-stu-id="79d9d-371">A compromised app could potentially run arbitrary code on the client.</span></span> <span data-ttu-id="79d9d-372">Уязвимость может быть использована для потенциального выполнения ряда вредоносных действий против сервера:</span><span class="sxs-lookup"><span data-stu-id="79d9d-372">The vulnerability could be used to potentially perform a number of malicious actions against the server:</span></span>

* <span data-ttu-id="79d9d-373">Отправка поддельных/недействительных событий на сервер.</span><span class="sxs-lookup"><span data-stu-id="79d9d-373">Dispatch fake/invalid events to the server.</span></span>
* <span data-ttu-id="79d9d-374">Сбой диспетчерской отправки/недействительные завершения рендера.</span><span class="sxs-lookup"><span data-stu-id="79d9d-374">Dispatch fail/invalid render completions.</span></span>
* <span data-ttu-id="79d9d-375">Избегайте отправки завершения рендеринга.</span><span class="sxs-lookup"><span data-stu-id="79d9d-375">Avoid dispatching render completions.</span></span>
* <span data-ttu-id="79d9d-376">Отправка интеропот звонков с JavaScript на .NET.</span><span class="sxs-lookup"><span data-stu-id="79d9d-376">Dispatch interop calls from JavaScript to .NET.</span></span>
* <span data-ttu-id="79d9d-377">Измените ответ на интероп-звонки с .NET на JavaScript.</span><span class="sxs-lookup"><span data-stu-id="79d9d-377">Modify the response of interop calls from .NET to JavaScript.</span></span>
* <span data-ttu-id="79d9d-378">Избегайте отправки результатов .NET в JS interop.</span><span class="sxs-lookup"><span data-stu-id="79d9d-378">Avoid dispatching .NET to JS interop results.</span></span>

<span data-ttu-id="79d9d-379">Платформа Blazor Server принимает меры для защиты от некоторых предыдущих угроз:</span><span class="sxs-lookup"><span data-stu-id="79d9d-379">The Blazor Server framework takes steps to protect against some of the preceding threats:</span></span>

* <span data-ttu-id="79d9d-380">Прекращает выпуск новых обновлений uI, если клиент не признает партии рендеров.</span><span class="sxs-lookup"><span data-stu-id="79d9d-380">Stops producing new UI updates if the client isn't acknowledging render batches.</span></span> <span data-ttu-id="79d9d-381">Настроен с `CircuitOptions.MaxBufferedUnacknowledgedRenderBatches`.</span><span class="sxs-lookup"><span data-stu-id="79d9d-381">Configured with `CircuitOptions.MaxBufferedUnacknowledgedRenderBatches`.</span></span>
* <span data-ttu-id="79d9d-382">Время из любого .NET на JavaScript вызова после одной минуты, не получая ответа от клиента.</span><span class="sxs-lookup"><span data-stu-id="79d9d-382">Times out any .NET to JavaScript call after one minute without receiving a response from the client.</span></span> <span data-ttu-id="79d9d-383">Настроен с `CircuitOptions.JSInteropDefaultCallTimeout`.</span><span class="sxs-lookup"><span data-stu-id="79d9d-383">Configured with `CircuitOptions.JSInteropDefaultCallTimeout`.</span></span>
* <span data-ttu-id="79d9d-384">Выполняет базовую проверку на всех входных данных, поступающих из браузера во время JS interop:</span><span class="sxs-lookup"><span data-stu-id="79d9d-384">Performs basic validation on all input coming from the browser during JS interop:</span></span>
  * <span data-ttu-id="79d9d-385">Ссылки .NET являются действительными и типа, ожидаемого методом .NET.</span><span class="sxs-lookup"><span data-stu-id="79d9d-385">.NET references are valid and of the type expected by the .NET method.</span></span>
  * <span data-ttu-id="79d9d-386">Данные не являются пороками.</span><span class="sxs-lookup"><span data-stu-id="79d9d-386">The data isn't malformed.</span></span>
  * <span data-ttu-id="79d9d-387">Правильное количество аргументов для метода присутствует в полезной нагрузке.</span><span class="sxs-lookup"><span data-stu-id="79d9d-387">The correct number of arguments for the method are present in the payload.</span></span>
  * <span data-ttu-id="79d9d-388">Аргументы или результаты могут быть десериализованы правильно, прежде чем ссылаться на метод.</span><span class="sxs-lookup"><span data-stu-id="79d9d-388">The arguments or result can be deserialized correctly before invoking the method.</span></span>
* <span data-ttu-id="79d9d-389">Выполняет базовую проверку во всех входных данных, поступающих из браузера из рассылки событий:</span><span class="sxs-lookup"><span data-stu-id="79d9d-389">Performs basic validation in all input coming from the browser from dispatched events:</span></span>
  * <span data-ttu-id="79d9d-390">Событие имеет действительный тип.</span><span class="sxs-lookup"><span data-stu-id="79d9d-390">The event has a valid type.</span></span>
  * <span data-ttu-id="79d9d-391">Данные для события могут быть десериализованы.</span><span class="sxs-lookup"><span data-stu-id="79d9d-391">The data for the event can be deserialized.</span></span>
  * <span data-ttu-id="79d9d-392">Обработчик событий связан с событием.</span><span class="sxs-lookup"><span data-stu-id="79d9d-392">There's an event handler associated with the event.</span></span>

<span data-ttu-id="79d9d-393">В дополнение к гарантиям, которые реализует платформа, приложение должно быть закодировано разработчиком для защиты от угроз и принятия соответствующих мер:</span><span class="sxs-lookup"><span data-stu-id="79d9d-393">In addition to the safeguards that the framework implements, the app must be coded by the developer to safeguard against threats and take appropriate actions:</span></span>

* <span data-ttu-id="79d9d-394">Всегда проверяйте данные при обработке событий.</span><span class="sxs-lookup"><span data-stu-id="79d9d-394">Always validate data when handling events.</span></span>
* <span data-ttu-id="79d9d-395">Принять соответствующие меры при получении недействительных данных:</span><span class="sxs-lookup"><span data-stu-id="79d9d-395">Take appropriate action upon receiving invalid data:</span></span>
  * <span data-ttu-id="79d9d-396">Игнорировать данные и возвращаться.</span><span class="sxs-lookup"><span data-stu-id="79d9d-396">Ignore the data and return.</span></span> <span data-ttu-id="79d9d-397">Это позволяет приложению продолжать обработку запросов.</span><span class="sxs-lookup"><span data-stu-id="79d9d-397">This allows the app to continue processing requests.</span></span>
  * <span data-ttu-id="79d9d-398">Если приложение определяет, что вход является незаконным и не может быть произведен законным клиентом, бросьте исключение.</span><span class="sxs-lookup"><span data-stu-id="79d9d-398">If the app determines that the input is illegitimate and couldn't be produced by legitimate client, throw an exception.</span></span> <span data-ttu-id="79d9d-399">Бросая исключение срывает цепь и завершает сеанс.</span><span class="sxs-lookup"><span data-stu-id="79d9d-399">Throwing an exception tears down the circuit and ends the session.</span></span>
* <span data-ttu-id="79d9d-400">Не доверяйте сообщению об ошибке, предоставляемому завершением пакетов, включенных в журналы.</span><span class="sxs-lookup"><span data-stu-id="79d9d-400">Don't trust the error message provided by render batch completions included in the logs.</span></span> <span data-ttu-id="79d9d-401">Ошибка предоставляется клиентом и, как правило, не может быть доверено, так как клиент может быть скомпрометирован.</span><span class="sxs-lookup"><span data-stu-id="79d9d-401">The error is provided by the client and can't generally be trusted, as the client might be compromised.</span></span>
* <span data-ttu-id="79d9d-402">Не доверяйте входным вызовам на JS interop в любом направлении между методами JavaScript и .NET.</span><span class="sxs-lookup"><span data-stu-id="79d9d-402">Don't trust the input on JS interop calls in either direction between JavaScript and .NET methods.</span></span>
* <span data-ttu-id="79d9d-403">Приложение отвечает за проверку того, что содержание аргументов и результатов является действительным, даже если аргументы или результаты правильно десериализованы.</span><span class="sxs-lookup"><span data-stu-id="79d9d-403">The app is responsible for validating that the content of arguments and results are valid, even if the arguments or results are correctly deserialized.</span></span>

<span data-ttu-id="79d9d-404">Для того чтобы уязвимость XSS существовала, приложение должно включать пользовательский ввод в отрисованную страницу.</span><span class="sxs-lookup"><span data-stu-id="79d9d-404">For a XSS vulnerability to exist, the app must incorporate user input in the rendered page.</span></span> Blazor<span data-ttu-id="79d9d-405">Компоненты сервера выполняют этап компиляции времени, в котором разметка в файле *.razor* преобразуется в процедурную логику C.</span><span class="sxs-lookup"><span data-stu-id="79d9d-405"> Server components execute a compile-time step where the markup in a *.razor* file is transformed into procedural C# logic.</span></span> <span data-ttu-id="79d9d-406">Во время выполнения логика C's создает *дерево рендеров,* описывающее элементы, текст и компоненты ребенка.</span><span class="sxs-lookup"><span data-stu-id="79d9d-406">At runtime, the C# logic builds a *render tree* describing the elements, text, and child components.</span></span> <span data-ttu-id="79d9d-407">Это применяется к DOM браузера через последовательность инструкций JavaScript (или сериализованв к HTML в случае prerendering):</span><span class="sxs-lookup"><span data-stu-id="79d9d-407">This is applied to the browser's DOM via a sequence of JavaScript instructions (or is serialized to HTML in the case of prerendering):</span></span>

* <span data-ttu-id="79d9d-408">Пользовательский ввод, отображаемый `@someStringValue`через обычный синтаксис Razor (например, ) не предоставляет уязвимость XSS, потому что синтаксис Razor добавляется в DOM с помощью команд, которые могут писать только текст.</span><span class="sxs-lookup"><span data-stu-id="79d9d-408">User input rendered via normal Razor syntax (for example, `@someStringValue`) doesn't expose a XSS vulnerability because the Razor syntax is added to the DOM via commands that can only write text.</span></span> <span data-ttu-id="79d9d-409">Даже если значение включает html разметку, значение отображается как статический текст.</span><span class="sxs-lookup"><span data-stu-id="79d9d-409">Even if the value includes HTML markup, the value is displayed as static text.</span></span> <span data-ttu-id="79d9d-410">При предварительном рендеринге вывод кодируется HTML, который также отображает содержимое как статический текст.</span><span class="sxs-lookup"><span data-stu-id="79d9d-410">When prerendering, the output is HTML-encoded, which also displays the content as static text.</span></span>
* <span data-ttu-id="79d9d-411">Теги скриптов не допускаются и не должны быть включены в дерево визы компонента приложения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-411">Script tags aren't allowed and shouldn't be included in the app's component render tree.</span></span> <span data-ttu-id="79d9d-412">Если тег скрипта включен в разметку компонента, генерируется ошибка времени компиляции.</span><span class="sxs-lookup"><span data-stu-id="79d9d-412">If a script tag is included in a component's markup, a compile-time error is generated.</span></span>
* <span data-ttu-id="79d9d-413">Авторы компонентов могут авторить компоненты в СЗ без использования Razor.</span><span class="sxs-lookup"><span data-stu-id="79d9d-413">Component authors can author components in C# without using Razor.</span></span> <span data-ttu-id="79d9d-414">Автор компонента отвечает за использование правильных AIS при выходе.</span><span class="sxs-lookup"><span data-stu-id="79d9d-414">The component author is responsible for using the correct APIs when emitting output.</span></span> <span data-ttu-id="79d9d-415">Например, `builder.AddContent(0, someUserSuppliedString)` использовать, а *не,* `builder.AddMarkupContent(0, someUserSuppliedString)`как последний может создать уязвимость XSS.</span><span class="sxs-lookup"><span data-stu-id="79d9d-415">For example, use `builder.AddContent(0, someUserSuppliedString)` and *not* `builder.AddMarkupContent(0, someUserSuppliedString)`, as the latter could create a XSS vulnerability.</span></span>

<span data-ttu-id="79d9d-416">В рамках защиты от атак XSS рассмотрите возможность внедрения мер по смягчению последствий XSS, таких как [политика безопасности содержимого (CSP).](https://developer.mozilla.org/docs/Web/HTTP/CSP)</span><span class="sxs-lookup"><span data-stu-id="79d9d-416">As part of protecting against XSS attacks, consider implementing XSS mitigations, such as [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP).</span></span>

<span data-ttu-id="79d9d-417">Для получения дополнительной информации см. <xref:security/cross-site-scripting>.</span><span class="sxs-lookup"><span data-stu-id="79d9d-417">For more information, see <xref:security/cross-site-scripting>.</span></span>

### <a name="cross-origin-protection"></a><span data-ttu-id="79d9d-418">Защита от крестного происхождения</span><span class="sxs-lookup"><span data-stu-id="79d9d-418">Cross-origin protection</span></span>

<span data-ttu-id="79d9d-419">Атаки кросс-происхождения связаны с клиентом другого происхождения, выполняющим действие против сервера.</span><span class="sxs-lookup"><span data-stu-id="79d9d-419">Cross-origin attacks involve a client from a different origin performing an action against the server.</span></span> <span data-ttu-id="79d9d-420">Вредоносное действие, как правило, запрос GET или форма POST (Кросс-сайт Запрос подделка, CSRF), но открытие вредоносного WebSocket также возможно.</span><span class="sxs-lookup"><span data-stu-id="79d9d-420">The malicious action is typically a GET request or a form POST (Cross-Site Request Forgery, CSRF), but opening a malicious WebSocket is also possible.</span></span> Blazor<span data-ttu-id="79d9d-421">Приложения сервера предлагают [те же SignalR гарантии, что любое другое приложение, используюеееееее протокол концентратора:](xref:signalr/security)</span><span class="sxs-lookup"><span data-stu-id="79d9d-421"> Server apps offer [the same guarantees that any other SignalR app using the hub protocol offer](xref:signalr/security):</span></span>

* Blazor<span data-ttu-id="79d9d-422">Приложения сервера могут быть доступны для перекрестного происхождения, если не будут приняты дополнительные меры для его предотвращения.</span><span class="sxs-lookup"><span data-stu-id="79d9d-422"> Server apps can be accessed cross-origin unless additional measures are taken to prevent it.</span></span> <span data-ttu-id="79d9d-423">Чтобы отключить доступ к кросс-происхождению, либо отключите CORS в конечной точке, `DisableCorsAttribute` добавив Blazor промежуточное программное обеспечение CORS в конвейер и добавив в конечную точку метаданные или ограничивайте набор разрешенных истоков путем [настройки SignalR для совместного использования ресурсов между корнями.](xref:signalr/security#cross-origin-resource-sharing)</span><span class="sxs-lookup"><span data-stu-id="79d9d-423">To disable cross-origin access, either disable CORS in the endpoint by adding the CORS middleware to the pipeline and adding the `DisableCorsAttribute` to the Blazor endpoint metadata or limit the set of allowed origins by [configuring SignalR for cross-origin resource sharing](xref:signalr/security#cross-origin-resource-sharing).</span></span>
* <span data-ttu-id="79d9d-424">Если CORS включен, могут потребоваться дополнительные шаги для защиты приложения в зависимости от конфигурации CORS.</span><span class="sxs-lookup"><span data-stu-id="79d9d-424">If CORS is enabled, extra steps might be required to protect the app depending on the CORS configuration.</span></span> <span data-ttu-id="79d9d-425">Если CORS включен глобально, CORS может Blazor быть отключен для `DisableCorsAttribute` концентратора Server, добавив `hub.MapBlazorHub()`метаданные в метаданные конечных точек после вызова.</span><span class="sxs-lookup"><span data-stu-id="79d9d-425">If CORS is globally enabled, CORS can be disabled for the Blazor Server hub by adding the `DisableCorsAttribute` metadata to the endpoint metadata after calling `hub.MapBlazorHub()`.</span></span>

<span data-ttu-id="79d9d-426">Для получения дополнительной информации см. <xref:security/anti-request-forgery>.</span><span class="sxs-lookup"><span data-stu-id="79d9d-426">For more information, see <xref:security/anti-request-forgery>.</span></span>

### <a name="click-jacking"></a><span data-ttu-id="79d9d-427">Нажмите-джекинг</span><span class="sxs-lookup"><span data-stu-id="79d9d-427">Click-jacking</span></span>

<span data-ttu-id="79d9d-428">Click-jacking включает визуализацию сайта `<iframe>` как внутри сайта другого происхождения, чтобы обмануть пользователя в выполнении действий на сайте, подвергаемому атакой.</span><span class="sxs-lookup"><span data-stu-id="79d9d-428">Click-jacking involves rendering a site as an `<iframe>` inside a site from a different origin in order to trick the user into performing actions on the site under attack.</span></span>

<span data-ttu-id="79d9d-429">Для защиты приложения от рендеринга внутри `<iframe>`содержимого используйте политику [безопасности содержимого (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) и `X-Frame-Options` заголовок.</span><span class="sxs-lookup"><span data-stu-id="79d9d-429">To protect an app from rendering inside of an `<iframe>`, use [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) and the `X-Frame-Options` header.</span></span> <span data-ttu-id="79d9d-430">Для получения дополнительной [информации см. веб-документы MDN: X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options).</span><span class="sxs-lookup"><span data-stu-id="79d9d-430">For more information, see [MDN web docs: X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options).</span></span>

### <a name="open-redirects"></a><span data-ttu-id="79d9d-431">Открытые перенаправления</span><span class="sxs-lookup"><span data-stu-id="79d9d-431">Open redirects</span></span>

<span data-ttu-id="79d9d-432">При Blazor запуске сеанса приложения Server сервер выполняет базовую проверку URL-адресов, отправленных в рамках запуска сеанса.</span><span class="sxs-lookup"><span data-stu-id="79d9d-432">When a Blazor Server app session starts, the server performs basic validation of the URLs sent as part of starting the session.</span></span> <span data-ttu-id="79d9d-433">Платформа проверяет, что базовый URL является родительским URL-адресом перед созданием схемы.</span><span class="sxs-lookup"><span data-stu-id="79d9d-433">The framework checks that the base URL is a parent of the current URL before establishing the circuit.</span></span> <span data-ttu-id="79d9d-434">Дополнительные проверки в рамках не проводятся.</span><span class="sxs-lookup"><span data-stu-id="79d9d-434">No additional checks are performed by the framework.</span></span>

<span data-ttu-id="79d9d-435">Когда пользователь выбирает ссылку на клиенте, URL-адрес ссылки отправляется на сервер, который определяет, какие действия следует предпринять.</span><span class="sxs-lookup"><span data-stu-id="79d9d-435">When a user selects a link on the client, the URL for the link is sent to the server, which determines what action to take.</span></span> <span data-ttu-id="79d9d-436">Например, приложение может выполнять навигацию на стороне клиента или указывать браузеру для переопределения в новое место.</span><span class="sxs-lookup"><span data-stu-id="79d9d-436">For example, the app may perform a client-side navigation or indicate to the browser to go to the new location.</span></span>

<span data-ttu-id="79d9d-437">Компоненты могут также запускать навигационные `NavigationManager`запросы программно с помощью .</span><span class="sxs-lookup"><span data-stu-id="79d9d-437">Components can also trigger navigation requests programatically through the use of `NavigationManager`.</span></span> <span data-ttu-id="79d9d-438">В таких сценариях приложение может выполнять навигацию на стороне клиента или указывать браузеру для переопределения в новое место.</span><span class="sxs-lookup"><span data-stu-id="79d9d-438">In such scenarios, the app might perform a client-side navigation or indicate to the browser to go to the new location.</span></span>

<span data-ttu-id="79d9d-439">Компоненты должны:</span><span class="sxs-lookup"><span data-stu-id="79d9d-439">Components must:</span></span>

* <span data-ttu-id="79d9d-440">Избегайте использования пользовательского ввода как части аргументов навигационных вызовов.</span><span class="sxs-lookup"><span data-stu-id="79d9d-440">Avoid using user input as part of the navigation call arguments.</span></span>
* <span data-ttu-id="79d9d-441">Проверка аргументов, чтобы убедиться, что цель разрешена приложением.</span><span class="sxs-lookup"><span data-stu-id="79d9d-441">Validate arguments to ensure that the target is allowed by the app.</span></span>

<span data-ttu-id="79d9d-442">В противном случае злоумышленник может заставить браузер перейти на сайт, контролируемый злоумышленником.</span><span class="sxs-lookup"><span data-stu-id="79d9d-442">Otherwise, a malicious user can force the browser to go to an attacker-controlled site.</span></span> <span data-ttu-id="79d9d-443">В этом случае злоумышленник уловки приложение в использовании некоторых пользовательских `NavigationManager.Navigate` ввода как часть вызова метода.</span><span class="sxs-lookup"><span data-stu-id="79d9d-443">In this scenario, the attacker tricks the app into using some user input as part of the invocation of the `NavigationManager.Navigate` method.</span></span>

<span data-ttu-id="79d9d-444">Этот совет также применяется при визуализации ссылок как часть приложения:</span><span class="sxs-lookup"><span data-stu-id="79d9d-444">This advice also applies when rendering links as part of the app:</span></span>

* <span data-ttu-id="79d9d-445">Если возможно, используйте относительные ссылки.</span><span class="sxs-lookup"><span data-stu-id="79d9d-445">If possible, use relative links.</span></span>
* <span data-ttu-id="79d9d-446">Проверить, что абсолютные направления ссылки действительны, прежде чем включить их в страницу.</span><span class="sxs-lookup"><span data-stu-id="79d9d-446">Validate that absolute link destinations are valid before including them in a page.</span></span>

<span data-ttu-id="79d9d-447">Для получения дополнительной информации см. <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="79d9d-447">For more information, see <xref:security/preventing-open-redirects>.</span></span>

## <a name="authentication-and-authorization"></a><span data-ttu-id="79d9d-448">Аутентификация и авторизация</span><span class="sxs-lookup"><span data-stu-id="79d9d-448">Authentication and authorization</span></span>

<span data-ttu-id="79d9d-449">Для получения рекомендаций по <xref:security/blazor/index>аутентификации и авторизации см.</span><span class="sxs-lookup"><span data-stu-id="79d9d-449">For guidance on authentication and authorization, see <xref:security/blazor/index>.</span></span>

## <a name="security-checklist"></a><span data-ttu-id="79d9d-450">Контрольный список по безопасности</span><span class="sxs-lookup"><span data-stu-id="79d9d-450">Security checklist</span></span>

<span data-ttu-id="79d9d-451">Следующий список соображений безопасности не является исчерпывающим:</span><span class="sxs-lookup"><span data-stu-id="79d9d-451">The following list of security considerations isn't comprehensive:</span></span>

* <span data-ttu-id="79d9d-452">Проверка аргументов из событий.</span><span class="sxs-lookup"><span data-stu-id="79d9d-452">Validate arguments from events.</span></span>
* <span data-ttu-id="79d9d-453">Проверка входных данных и результатов вызовов JS interop.</span><span class="sxs-lookup"><span data-stu-id="79d9d-453">Validate inputs and results from JS interop calls.</span></span>
* <span data-ttu-id="79d9d-454">Избегайте использования (или проверки) пользовательского ввода для вызовов .NET к interop JS.</span><span class="sxs-lookup"><span data-stu-id="79d9d-454">Avoid using (or validate beforehand) user input for .NET to JS interop calls.</span></span>
* <span data-ttu-id="79d9d-455">Предотвратите выделение клиенту неограниченного объема памяти.</span><span class="sxs-lookup"><span data-stu-id="79d9d-455">Prevent the client from allocating an unbound amount of memory.</span></span>
  * <span data-ttu-id="79d9d-456">Данные внутри компонента.</span><span class="sxs-lookup"><span data-stu-id="79d9d-456">Data within the component.</span></span>
  * <span data-ttu-id="79d9d-457">`DotNetObject`ссылки возвращены клиенту.</span><span class="sxs-lookup"><span data-stu-id="79d9d-457">`DotNetObject` references returned to the client.</span></span>
* <span data-ttu-id="79d9d-458">Защитите от нескольких диспетчеров.</span><span class="sxs-lookup"><span data-stu-id="79d9d-458">Guard against multiple dispatches.</span></span>
* <span data-ttu-id="79d9d-459">Отмена длительных операций при удалении компонента.</span><span class="sxs-lookup"><span data-stu-id="79d9d-459">Cancel long-running operations when the component is disposed.</span></span>
* <span data-ttu-id="79d9d-460">Избегайте событий, которые производят большие объемы данных.</span><span class="sxs-lookup"><span data-stu-id="79d9d-460">Avoid events that produce large amounts of data.</span></span>
* <span data-ttu-id="79d9d-461">Избегайте использования пользовательского ввода `NavigationManager.Navigate` как части вызовов и проверки пользовательских ввода для URL-адресов в отношении набора разрешенных истоков, если это неизбежно.</span><span class="sxs-lookup"><span data-stu-id="79d9d-461">Avoid using user input as part of calls to `NavigationManager.Navigate` and validate user input for URLs against a set of allowed origins first if unavoidable.</span></span>
* <span data-ttu-id="79d9d-462">Не принимать решения о авторизации на основе состояния пользователя, а только из состояния компонента.</span><span class="sxs-lookup"><span data-stu-id="79d9d-462">Don't make authorization decisions based on the state of the UI but only from component state.</span></span>
* <span data-ttu-id="79d9d-463">Для защиты от атак XSS можно использовать [политику безопасности содержимого (CSP).](https://developer.mozilla.org/docs/Web/HTTP/CSP)</span><span class="sxs-lookup"><span data-stu-id="79d9d-463">Consider using [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) to protect against XSS attacks.</span></span>
* <span data-ttu-id="79d9d-464">Рассмотрите возможность использования CSP и [X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options) для защиты от кликов.</span><span class="sxs-lookup"><span data-stu-id="79d9d-464">Consider using CSP and [X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options) to protect against click-jacking.</span></span>
* <span data-ttu-id="79d9d-465">Убедитесь, что настройки CORS являются уместными при Blazor включении CORS или явном отключить CORS для приложений.</span><span class="sxs-lookup"><span data-stu-id="79d9d-465">Ensure CORS settings are appropriate when enabling CORS or explicitly disable CORS for Blazor apps.</span></span>
* <span data-ttu-id="79d9d-466">Проверьте, чтобы ограничения на сервер Blazor для приложения обеспечивали приемлемый пользовательский опыт без неприемлемых уровней риска.</span><span class="sxs-lookup"><span data-stu-id="79d9d-466">Test to ensure that the server-side limits for the Blazor app provide an acceptable user experience without unacceptable levels of risk.</span></span>
