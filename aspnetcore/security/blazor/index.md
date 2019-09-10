---
title: Аутентификация и авторизация в ASP.NET Core Blazor
author: guardrex
description: Сведения об аутентификации и авторизации в Blazor
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: security/blazor/index
ms.openlocfilehash: 2ba7b0612c2be50ae0797c50dc3cb0d63c0f0c2d
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800510"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="326e6-103">Аутентификация и авторизация в ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="326e6-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="326e6-104">Автор: [Стив Сандерсон](https://github.com/SteveSandersonMS) (Steve Sanderson)</span><span class="sxs-lookup"><span data-stu-id="326e6-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="326e6-105">ASP.NET Core поддерживает настройку и администрирование средств безопасности в приложениях Blazor.</span><span class="sxs-lookup"><span data-stu-id="326e6-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="326e6-106">Сценарии безопасности для серверных и клиентских приложений Blazor существенно отличаются.</span><span class="sxs-lookup"><span data-stu-id="326e6-106">Security scenarios differ between Blazor server-side and client-side apps.</span></span> <span data-ttu-id="326e6-107">Так как серверные приложения Blazor выполняются на стороне сервера, проверки авторизации определяют следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="326e6-107">Because Blazor server-side apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="326e6-108">параметры пользовательского интерфейса, предоставленные пользователю (например, какие пункты меню доступны пользователю);</span><span class="sxs-lookup"><span data-stu-id="326e6-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="326e6-109">правила доступа для приложений и компонентов.</span><span class="sxs-lookup"><span data-stu-id="326e6-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="326e6-110">Клиентские приложения Blazor выполняются на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="326e6-110">Blazor client-side apps run on the client.</span></span> <span data-ttu-id="326e6-111">В этом случае авторизация используется *только* для определения отображаемых вариантов пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="326e6-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="326e6-112">Так как пользователь может изменить или обойти проверки на стороне клиента, клиентское приложение Blazor не может применять правила авторизации доступа.</span><span class="sxs-lookup"><span data-stu-id="326e6-112">Since client-side checks can be modified or bypassed by a user, a Blazor client-side app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="326e6-113">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="326e6-113">Authentication</span></span>

<span data-ttu-id="326e6-114">Blazor использует существующие механизмы аутентификации ASP.NET Core для установления личности пользователя.</span><span class="sxs-lookup"><span data-stu-id="326e6-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="326e6-115">Конкретный механизм зависит от того, как размещается приложение Blazor — на стороне сервера или клиента.</span><span class="sxs-lookup"><span data-stu-id="326e6-115">The exact mechanism depends on how the Blazor app is hosted, server-side or client-side.</span></span>

### <a name="blazor-server-side-authentication"></a><span data-ttu-id="326e6-116">Аутентификация Blazor на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="326e6-116">Blazor server-side authentication</span></span>

<span data-ttu-id="326e6-117">Приложения Blazor на стороне сервера работают через подключение реального времени, созданное с помощью SignalR.</span><span class="sxs-lookup"><span data-stu-id="326e6-117">Blazor server-side apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="326e6-118">[Аутентификация в приложениях на основе SignalR](xref:signalr/authn-and-authz) выполняется при установлении подключения.</span><span class="sxs-lookup"><span data-stu-id="326e6-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="326e6-119">Аутентификация может выполняться на основе файлов cookie или других маркеров носителя.</span><span class="sxs-lookup"><span data-stu-id="326e6-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="326e6-120">Шаблон проекта Blazor на стороне сервера может автоматически настроить аутентификацию при создании проекта.</span><span class="sxs-lookup"><span data-stu-id="326e6-120">The Blazor server-side project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="326e6-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="326e6-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="326e6-122">Следуйте указаниям для Visual Studio, которые представлены в статье <xref:blazor/get-started>, чтобы создать проект Blazor на стороне сервера с механизмом аутентификации.</span><span class="sxs-lookup"><span data-stu-id="326e6-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism.</span></span>

<span data-ttu-id="326e6-123">Выбрав шаблон **Серверное приложение Blazor** в диалоговом окне **Создать веб-приложение ASP.NET Core**, щелкните **Изменить** в разделе **Проверка подлинности**.</span><span class="sxs-lookup"><span data-stu-id="326e6-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="326e6-124">Откроется диалоговое окно с тем же набором механизмов аутентификации, которые доступны для других проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="326e6-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="326e6-125">**Без аутентификации**.</span><span class="sxs-lookup"><span data-stu-id="326e6-125">**No Authentication**</span></span>
* <span data-ttu-id="326e6-126">**Учетные записи отдельных пользователей** &ndash; которые можно хранить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="326e6-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="326e6-127">в приложении с помощью системы [удостоверений](xref:security/authentication/identity) ASP.NET Core;</span><span class="sxs-lookup"><span data-stu-id="326e6-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="326e6-128">в [Azure AD B2C](xref:security/authentication/azure-ad-b2c);</span><span class="sxs-lookup"><span data-stu-id="326e6-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="326e6-129">**рабочие или учебные учетные записи**.</span><span class="sxs-lookup"><span data-stu-id="326e6-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="326e6-130">**Проверка подлинности Windows**</span><span class="sxs-lookup"><span data-stu-id="326e6-130">**Windows Authentication**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="326e6-131">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="326e6-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="326e6-132">Следуйте указаниям для Visual Studio Code, которые представлены в статье <xref:blazor/get-started>, чтобы создать проект Blazor на стороне сервера с механизмом аутентификации.</span><span class="sxs-lookup"><span data-stu-id="326e6-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism:</span></span>

```console
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="326e6-133">Допустимые значения аутентификации (`{AUTHENTICATION}`) перечислены в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="326e6-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="326e6-134">Механизм аутентификации</span><span class="sxs-lookup"><span data-stu-id="326e6-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="326e6-135">Значение`{AUTHENTICATION}` </span><span class="sxs-lookup"><span data-stu-id="326e6-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="326e6-136">Без аутентификации</span><span class="sxs-lookup"><span data-stu-id="326e6-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="326e6-137">Индивидуальное лицо</span><span class="sxs-lookup"><span data-stu-id="326e6-137">Individual</span></span><br><span data-ttu-id="326e6-138">Пользователи, сохраненные в приложении с помощью ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="326e6-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="326e6-139">Индивидуальное лицо</span><span class="sxs-lookup"><span data-stu-id="326e6-139">Individual</span></span><br><span data-ttu-id="326e6-140">Пользователи, сохраненные в [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="326e6-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="326e6-141">Рабочие или учебные учетные записи</span><span class="sxs-lookup"><span data-stu-id="326e6-141">Work or School Accounts</span></span><br><span data-ttu-id="326e6-142">Корпоративная аутентификация для отдельного клиента.</span><span class="sxs-lookup"><span data-stu-id="326e6-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="326e6-143">Рабочие или учебные учетные записи</span><span class="sxs-lookup"><span data-stu-id="326e6-143">Work or School Accounts</span></span><br><span data-ttu-id="326e6-144">Корпоративная аутентификация для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="326e6-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="326e6-145">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="326e6-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="326e6-146">Эта команда создает папку, в качестве имени для которой берется значение, предоставленное вместо заполнителя `{APP NAME}`, а затем использует имя папки в качестве имени приложения.</span><span class="sxs-lookup"><span data-stu-id="326e6-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="326e6-147">Подробнее см. статью о команде [dotnet new](/dotnet/core/tools/dotnet-new) в руководстве по .NET Core.</span><span class="sxs-lookup"><span data-stu-id="326e6-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism:

```console
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.

| Authentication mechanism                                                                 | `{AUTHENTICATION}` value |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| No Authentication                                                                        | `None`                   |
| Individual<br>Users stored in the app with ASP.NET Core Identity.                        | `Individual`             |
| Individual<br>Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Work or School Accounts<br>Organizational authentication for a single tenant.            | `SingleOrg`              |
| Work or School Accounts<br>Organizational authentication for multiple tenants.           | `MultiOrg`               |
| Windows Authentication                                                                   | `Windows`                |

The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name. For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.

-->

---

### <a name="blazor-client-side-authentication"></a><span data-ttu-id="326e6-148">Аутентификация Blazor на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="326e6-148">Blazor client-side authentication</span></span>

<span data-ttu-id="326e6-149">В приложениях Blazor на стороне клиента аутентификацию можно обойти, так как пользователь может изменять любой код на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="326e6-149">In Blazor client-side apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="326e6-150">Это же справедливо для всех технологий на стороне клиента, включая платформы одностраничного приложения JavaScript или собственных приложений для любой операционной системы.</span><span class="sxs-lookup"><span data-stu-id="326e6-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="326e6-151">Реализация пользовательской службы `AuthenticationStateProvider` для приложений Blazor на стороне клиента рассматривается в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="326e6-151">Implementation of a custom `AuthenticationStateProvider` service for Blazor client-side apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="326e6-152">Служба AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="326e6-152">AuthenticationStateProvider service</span></span>

<span data-ttu-id="326e6-153">Приложения Blazor на стороне сервера включают встроенную службу `AuthenticationStateProvider`, которая получает данные о состоянии аутентификации из `HttpContext.User` в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="326e6-153">Blazor server-side apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="326e6-154">Так состояние аутентификации интегрируется с существующими соответствующими механизмами ASP.NET Core на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="326e6-154">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="326e6-155">`AuthenticationStateProvider` является базовой службой, которую компоненты `AuthorizeView` и `CascadingAuthenticationState` используют для получения состояния аутентификации.</span><span class="sxs-lookup"><span data-stu-id="326e6-155">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="326e6-156">Обычно вы не используете `AuthenticationStateProvider` напрямую.</span><span class="sxs-lookup"><span data-stu-id="326e6-156">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="326e6-157">Выберите подход на основе [компонента AuthorizeView](#authorizeview-component) или [задачи<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter), которые описаны далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="326e6-157">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="326e6-158">Основной недостаток при использовании `AuthenticationStateProvider` напрямую заключается в том, что компонент не получает автоматического уведомления при изменении базовых данных о состоянии аутентификации.</span><span class="sxs-lookup"><span data-stu-id="326e6-158">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="326e6-159">Служба `AuthenticationStateProvider` может предоставить данные <xref:System.Security.Claims.ClaimsPrincipal> о текущем пользователе, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="326e6-159">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```cshtml
@page "/"
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="@LogUsername">Write user info to console</button>

@code {
    private async Task LogUsername()
    {
        var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

<span data-ttu-id="326e6-160">Если `user.Identity.IsAuthenticated` имеет значение `true`, а пользователь является <xref:System.Security.Claims.ClaimsPrincipal>, можно перечислить утверждения и оценить членство в ролях.</span><span class="sxs-lookup"><span data-stu-id="326e6-160">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="326e6-161">Дополнительные сведения о внедрении зависимостей и службах см. в статьях <xref:blazor/dependency-injection> и <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="326e6-161">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="326e6-162">Реализация пользовательского AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="326e6-162">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="326e6-163">Если вы создаете приложение Blazor на стороне клиента или спецификация приложения включает строгое требование пользовательского поставщика, следует создать реализацию этого поставщика и переопределить `GetAuthenticationStateAsync` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="326e6-163">If you're building a Blazor client-side app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

```csharp
class CustomAuthStateProvider : AuthenticationStateProvider
{
    public override Task<AuthenticationState> GetAuthenticationStateAsync()
    {
        var identity = new ClaimsIdentity(new[]
        {
            new Claim(ClaimTypes.Name, "mrfibuli"),
        }, "Fake authentication type");

        var user = new ClaimsPrincipal(identity);

        return Task.FromResult(new AuthenticationState(user));
    }
}
```

<span data-ttu-id="326e6-164">Служба `CustomAuthStateProvider` регистрируется в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="326e6-164">The `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
}
```

<span data-ttu-id="326e6-165">С помощью `CustomAuthStateProvider`, все пользователи проходят аутентификацию с использованием имени пользователя `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="326e6-165">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="326e6-166">Предоставление состояния аутентификации в качестве каскадного параметра</span><span class="sxs-lookup"><span data-stu-id="326e6-166">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="326e6-167">Если процедурная логика требует данных о состоянии аутентификации, например при выполнении активируемых пользователем действий, для получения таких данных определите каскадный параметр типа `Task<AuthenticationState>`:</span><span class="sxs-lookup"><span data-stu-id="326e6-167">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```cshtml
@page "/"

<button @onclick="@LogUsername">Log username</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task LogUsername()
    {
        var authState = await authenticationStateTask;
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

<span data-ttu-id="326e6-168">Если `user.Identity.IsAuthenticated` имеет значение `true`, можно перечислить утверждения и оценить членство в ролях.</span><span class="sxs-lookup"><span data-stu-id="326e6-168">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="326e6-169">Настройте каскадный параметр `Task<AuthenticationState>` с помощью компонентов `AuthorizeRouteView` и `CascadingAuthenticationState`:</span><span class="sxs-lookup"><span data-stu-id="326e6-169">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components:</span></span>

```cshtml
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

## <a name="authorization"></a><span data-ttu-id="326e6-170">Авторизация</span><span class="sxs-lookup"><span data-stu-id="326e6-170">Authorization</span></span>

<span data-ttu-id="326e6-171">После аутентификации пользователя применяются правила *авторизации*, которые определяют доступные этому пользователю действия.</span><span class="sxs-lookup"><span data-stu-id="326e6-171">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="326e6-172">Доступ обычно предоставляется или запрещается в зависимости от следующих аспектов:</span><span class="sxs-lookup"><span data-stu-id="326e6-172">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="326e6-173">выполнена ли аутентификация (выполнен ли вход);</span><span class="sxs-lookup"><span data-stu-id="326e6-173">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="326e6-174">есть ли у пользователя определенная *роль*;</span><span class="sxs-lookup"><span data-stu-id="326e6-174">A user is in a *role*.</span></span>
* <span data-ttu-id="326e6-175">есть ли у пользователя определенное *утверждение*;</span><span class="sxs-lookup"><span data-stu-id="326e6-175">A user has a *claim*.</span></span>
* <span data-ttu-id="326e6-176">выполняются ли требования *политики*.</span><span class="sxs-lookup"><span data-stu-id="326e6-176">A *policy* is satisfied.</span></span>

<span data-ttu-id="326e6-177">Каждый из этих аспектов применяется здесь так же, как в приложениях ASP.NET Core MVC или Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="326e6-177">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="326e6-178">Дополнительные сведения о безопасности в ASP.NET Core вы найдете в статьях [о безопасности и идентификации в ASP.NET Core](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="326e6-178">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="326e6-179">Компонент AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="326e6-179">AuthorizeView component</span></span>

<span data-ttu-id="326e6-180">Компонент `AuthorizeView` избирательно демонстрирует элементы пользовательского интерфейса в зависимости от того, имеет ли пользователь права на доступ к этим элементам.</span><span class="sxs-lookup"><span data-stu-id="326e6-180">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="326e6-181">Этот подход полезен, если вам нужно просто *отображать* пользователю данные, и не использовать удостоверение пользователя в процедурной логике.</span><span class="sxs-lookup"><span data-stu-id="326e6-181">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="326e6-182">Этот компонент представляет переменную `context` типа `AuthenticationState`, которую можно использовать для доступа к сведениям о пользователе, выполнившем вход:</span><span class="sxs-lookup"><span data-stu-id="326e6-182">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="326e6-183">Также вы можете указать другое содержимое, которое будет отображаться, если пользователь не выполнил аутентификацию:</span><span class="sxs-lookup"><span data-stu-id="326e6-183">You can also supply different content for display if the user isn't authenticated:</span></span>

```cshtml
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <NotAuthorized>
        <h1>Authentication Failure!</h1>
        <p>You're not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

<span data-ttu-id="326e6-184">Содержимое тегов `<Authorized>` и `<NotAuthorized>` может включать произвольные элементы, например другие интерактивные компоненты.</span><span class="sxs-lookup"><span data-stu-id="326e6-184">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="326e6-185">Условия авторизации, такие как роли или правила для выбора вариантов пользовательского интерфейса и доступа к ним, описаны в разделе [об авторизации](#authorization).</span><span class="sxs-lookup"><span data-stu-id="326e6-185">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="326e6-186">Если условия авторизации не указаны, `AuthorizeView` использует политику по умолчанию со следующими правилами:</span><span class="sxs-lookup"><span data-stu-id="326e6-186">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="326e6-187">выполнившие аутентификацию (выполнившие вход) пользователи считаются авторизованными;</span><span class="sxs-lookup"><span data-stu-id="326e6-187">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="326e6-188">не выполнившие аутентификацию (не выполнившие вход) пользователи считаются не авторизованными.</span><span class="sxs-lookup"><span data-stu-id="326e6-188">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="326e6-189">Авторизация на основе ролей и политик</span><span class="sxs-lookup"><span data-stu-id="326e6-189">Role-based and policy-based authorization</span></span>

<span data-ttu-id="326e6-190">Компонент `AuthorizeView` поддерживает авторизацию на основе *ролей* или *политик*.</span><span class="sxs-lookup"><span data-stu-id="326e6-190">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="326e6-191">Для авторизации на основе ролей примените параметр `Roles`:</span><span class="sxs-lookup"><span data-stu-id="326e6-191">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="326e6-192">Дополнительные сведения можно найти по адресу: <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="326e6-192">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="326e6-193">Для авторизации на основе политик примените параметр `Policy`:</span><span class="sxs-lookup"><span data-stu-id="326e6-193">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="326e6-194">Авторизация на основе утверждений считается особым случаем авторизации на основе политик.</span><span class="sxs-lookup"><span data-stu-id="326e6-194">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="326e6-195">Например, вы можете определить политику, которая требует наличия определенного утверждения у пользователя.</span><span class="sxs-lookup"><span data-stu-id="326e6-195">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="326e6-196">Дополнительные сведения можно найти по адресу: <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="326e6-196">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="326e6-197">Эти API могут использоваться в приложениях Blazor на стороне сервера и на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="326e6-197">These APIs can be used in either Blazor server-side or Blazor client-side apps.</span></span>

<span data-ttu-id="326e6-198">Если не указано ни `Roles`, ни `Policy`, `AuthorizeView` использует политику по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="326e6-198">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="326e6-199">Содержимое, отображаемое при асинхронной аутентификации</span><span class="sxs-lookup"><span data-stu-id="326e6-199">Content displayed during asynchronous authentication</span></span>

<span data-ttu-id="326e6-200">Blazor позволяет *асинхронно* определять состояние аутентификации.</span><span class="sxs-lookup"><span data-stu-id="326e6-200">Blazor allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="326e6-201">Основной сценарий для такого подхода — приложение Blazor на стороне клиента, которое направляет запрос на аутентификацию во внешнюю конечную точку.</span><span class="sxs-lookup"><span data-stu-id="326e6-201">The primary scenario for this approach is in Blazor client-side apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="326e6-202">Пока аутентификация выполняется, `AuthorizeView` по умолчанию не отображает содержимое.</span><span class="sxs-lookup"><span data-stu-id="326e6-202">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="326e6-203">Чтобы содержимое отображалось, используйте элемент `<Authorizing>`:</span><span class="sxs-lookup"><span data-stu-id="326e6-203">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

```cshtml
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <Authorizing>
        <h1>Authentication in progress</h1>
        <p>You can only see this content while authentication is in progress.</p>
    </Authorizing>
</AuthorizeView>
```

<span data-ttu-id="326e6-204">Такой подход обычно не применим к приложениям Blazor на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="326e6-204">This approach isn't normally applicable to Blazor server-side apps.</span></span> <span data-ttu-id="326e6-205">Приложения Blazor на стороне сервера узнают состояние аутентификации, как только устанавливается состояние.</span><span class="sxs-lookup"><span data-stu-id="326e6-205">Blazor server-side apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="326e6-206">Содержимое `Authorizing` можно указать в компоненте `AuthorizeView` для приложения Blazor на стороне сервера, но это содержимое никогда не отображается.</span><span class="sxs-lookup"><span data-stu-id="326e6-206">`Authorizing` content can be provided in a Blazor server-side app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="326e6-207">Атрибут [Authorize]</span><span class="sxs-lookup"><span data-stu-id="326e6-207">[Authorize] attribute</span></span>

<span data-ttu-id="326e6-208">Так же, как и `[Authorize]` с контроллером MVC или страницей Razor, приложение может использовать `[Authorize]` с компонентами Razor:</span><span class="sxs-lookup"><span data-stu-id="326e6-208">Just like an app can use `[Authorize]` with an MVC controller or Razor page, `[Authorize]` can also be used with Razor Components:</span></span>

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> <span data-ttu-id="326e6-209">Используйте `[Authorize]` только для компонентов `@page`, полученных через маршрутизатор Blazor.</span><span class="sxs-lookup"><span data-stu-id="326e6-209">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="326e6-210">Авторизация выполняется только как аспект маршрутизации и *не* для дочерних компонентов, которые отображаются на странице.</span><span class="sxs-lookup"><span data-stu-id="326e6-210">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="326e6-211">Чтобы разрешить отображение конкретных частей на странице, используйте вместо этого `AuthorizeView`.</span><span class="sxs-lookup"><span data-stu-id="326e6-211">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="326e6-212">Возможно, потребуется добавить `@using Microsoft.AspNetCore.Authorization` в сам компонент или в файл *_Imports.razor*, чтобы компиляция компонента выполнялась нормально.</span><span class="sxs-lookup"><span data-stu-id="326e6-212">You may need to add `@using Microsoft.AspNetCore.Authorization` either to the component or to the *_Imports.razor* file in order for the component to compile.</span></span>

<span data-ttu-id="326e6-213">Атрибут `[Authorize]` также поддерживает авторизацию на основе ролей или политик.</span><span class="sxs-lookup"><span data-stu-id="326e6-213">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="326e6-214">Для авторизации на основе ролей примените параметр `Roles`:</span><span class="sxs-lookup"><span data-stu-id="326e6-214">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="326e6-215">Для авторизации на основе политик примените параметр `Policy`:</span><span class="sxs-lookup"><span data-stu-id="326e6-215">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="326e6-216">Если не указано ни `Roles`, ни `Policy`, `[Authorize]` использует политику по умолчанию со следующими правилами:</span><span class="sxs-lookup"><span data-stu-id="326e6-216">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="326e6-217">выполнившие аутентификацию (выполнившие вход) пользователи считаются авторизованными;</span><span class="sxs-lookup"><span data-stu-id="326e6-217">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="326e6-218">не выполнившие аутентификацию (не выполнившие вход) пользователи считаются не авторизованными.</span><span class="sxs-lookup"><span data-stu-id="326e6-218">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="326e6-219">Настройка несанкционированного содержимого для компонента маршрутизатора</span><span class="sxs-lookup"><span data-stu-id="326e6-219">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="326e6-220">Компонент `Router` вместе с компонентом `AuthorizeRouteView` позволяет приложению указать пользовательское содержимое для следующих ситуаций:</span><span class="sxs-lookup"><span data-stu-id="326e6-220">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="326e6-221">содержимое не найдено;</span><span class="sxs-lookup"><span data-stu-id="326e6-221">Content isn't found.</span></span>
* <span data-ttu-id="326e6-222">пользователь не удовлетворяет условию `[Authorize]`, которое применено к компоненту</span><span class="sxs-lookup"><span data-stu-id="326e6-222">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="326e6-223">(атрибут `[Authorize]` описан в [этом разделе](#authorize-attribute));</span><span class="sxs-lookup"><span data-stu-id="326e6-223">The `[Authorize]` attribute is covered in the [[Authorize] attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="326e6-224">выполняется асинхронная аутентификация.</span><span class="sxs-lookup"><span data-stu-id="326e6-224">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="326e6-225">В стандартном шаблоне проекта Blazor на стороне сервера есть файл *App.razor* с примером пользовательского содержимого:</span><span class="sxs-lookup"><span data-stu-id="326e6-225">In the default Blazor server-side project template, the *App.razor* file demonstrates how to set custom content:</span></span>

```cshtml
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)">
            <NotAuthorized>
                <h1>Sorry</h1>
                <p>You're not authorized to reach this page.</p>
                <p>You may need to log in as a different user.</p>
            </NotAuthorized>
            <Authorizing>
                <h1>Authentication in progress</h1>
                <p>Only visible while authentication is in progress.</p>
            </Authorizing>
        </AuthorizeRouteView>
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <h1>Sorry</h1>
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

<span data-ttu-id="326e6-226">Содержимое тегов `<NotFound>`, `<NotAuthorized>` и `<Authorizing>` может включать произвольные элементы, например другие интерактивные компоненты.</span><span class="sxs-lookup"><span data-stu-id="326e6-226">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="326e6-227">Если элемент `<NotAuthorized>` не указан, `AuthorizeRouteView` использует следующее резервное сообщение:</span><span class="sxs-lookup"><span data-stu-id="326e6-227">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="326e6-228">Уведомление об изменении состояния аутентификации</span><span class="sxs-lookup"><span data-stu-id="326e6-228">Notification about authentication state changes</span></span>

<span data-ttu-id="326e6-229">Если приложение определяет, что изменились базовые данные о состоянии аутентификации (например, пользователь вышел из системы или другой пользователь изменил роль), `AuthenticationStateProvider` пользователя может вызвать необязательный метод `NotifyAuthenticationStateChanged` из базового класса `AuthenticationStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="326e6-229">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="326e6-230">Это действие информирует потребителей данных состояния аутентификации (таких, как `AuthorizeView`) о необходимости повторно отображения с учетом новых данных.</span><span class="sxs-lookup"><span data-stu-id="326e6-230">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="326e6-231">Процедурная логика</span><span class="sxs-lookup"><span data-stu-id="326e6-231">Procedural logic</span></span>

<span data-ttu-id="326e6-232">Если приложению нужно проверять правила авторизации в составе процедурной логики, используйте каскадный параметр с типом `Task<AuthenticationState>`, чтобы получить <xref:System.Security.Claims.ClaimsPrincipal> пользователя.</span><span class="sxs-lookup"><span data-stu-id="326e6-232">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="326e6-233">`Task<AuthenticationState>` можно комбинировать для оценки политик с другими службами, например `IAuthorizationService`.</span><span class="sxs-lookup"><span data-stu-id="326e6-233">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

```cshtml
@inject IAuthorizationService AuthorizationService

<button @onclick="@DoSomething">Do something important</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task DoSomething()
    {
        var user = (await authenticationStateTask).User;

        if (user.Identity.IsAuthenticated)
        {
            // Perform an action only available to authenticated (signed-in) users.
        }

        if (user.IsInRole("admin"))
        {
            // Perform an action only available to users in the 'admin' role.
        }

        if ((await AuthorizationService.AuthorizeAsync(user, "content-editor"))
            .Succeeded)
        {
            // Perform an action only available to users satisfying the 
            // 'content-editor' policy.
        }
    }
}
```

## <a name="authorization-in-blazor-client-side-apps"></a><span data-ttu-id="326e6-234">Авторизация в приложениях Blazor на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="326e6-234">Authorization in Blazor client-side apps</span></span>

<span data-ttu-id="326e6-235">В приложениях Blazor на стороне клиента авторизацию можно обойти, так как пользователь может изменять любой код на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="326e6-235">In Blazor client-side apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="326e6-236">Это же справедливо для всех технологий на стороне клиента, включая платформы одностраничного приложения JavaScript или собственных приложений для любой операционной системы.</span><span class="sxs-lookup"><span data-stu-id="326e6-236">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="326e6-237">**Всегда выполняйте проверки авторизации на стороне сервера в конечных точках API, к которым обращается клиентское приложение.**</span><span class="sxs-lookup"><span data-stu-id="326e6-237">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="326e6-238">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="326e6-238">Troubleshoot errors</span></span>

<span data-ttu-id="326e6-239">Распространенные ошибки</span><span class="sxs-lookup"><span data-stu-id="326e6-239">Common errors:</span></span>

* <span data-ttu-id="326e6-240">**Для авторизации требуется каскадный параметр с типом \<AuthenticationState>. Попробуйте предоставить его с помощью CascadingAuthenticationState.**</span><span class="sxs-lookup"><span data-stu-id="326e6-240">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="326e6-241">**Для `authenticationStateTask` возвращается значение `null`**</span><span class="sxs-lookup"><span data-stu-id="326e6-241">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="326e6-242">Вполне вероятно, что проект не был создан на основе шаблона приложения Blazor на стороне сервера с включенной аутентификацией.</span><span class="sxs-lookup"><span data-stu-id="326e6-242">It's likely that the project wasn't created using a Blazor server-side template with authentication enabled.</span></span> <span data-ttu-id="326e6-243">Примените `<CascadingAuthenticationState>` в качестве оболочки некоторой части дерева пользовательского интерфейса, например в *App.razor*, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="326e6-243">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="326e6-244">`CascadingAuthenticationState` предоставляет каскадный параметр `Task<AuthenticationState>`, который он в свою очередь получает из базовой службы внедрения зависимостей `AuthenticationStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="326e6-244">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="326e6-245">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="326e6-245">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server-side>
* <xref:security/authentication/windowsauth>
