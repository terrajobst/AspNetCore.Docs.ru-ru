---
title: Проверка подлинности и авторизация в ASP.NET Core Blazor
author: guardrex
description: Сведения о проверке подлинности и авторизации в Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/21/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: f7ffb4c3d5a05cb916b4f00cdfaf5898634a1a6d
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219029"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="ff831-103">Аутентификация и авторизация в ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="ff831-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="ff831-104">Автор: [Стив Сандерсон](https://github.com/SteveSandersonMS) (Steve Sanderson)</span><span class="sxs-lookup"><span data-stu-id="ff831-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="ff831-105">ASP.NET Core поддерживает настройку и администрирование средств безопасности в приложениях Blazor.</span><span class="sxs-lookup"><span data-stu-id="ff831-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="ff831-106">Сценарии безопасности для серверных приложений Blazor и приложений Blazor WebAssembly отличаются.</span><span class="sxs-lookup"><span data-stu-id="ff831-106">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="ff831-107">Так как серверные приложения Blazor выполняются на стороне сервера, проверки авторизации могут определить следующее:</span><span class="sxs-lookup"><span data-stu-id="ff831-107">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="ff831-108">параметры пользовательского интерфейса, предоставленные пользователю (например, какие пункты меню доступны пользователю);</span><span class="sxs-lookup"><span data-stu-id="ff831-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="ff831-109">правила доступа для приложений и компонентов.</span><span class="sxs-lookup"><span data-stu-id="ff831-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="ff831-110">Приложения Blazor WebAssembly выполняются на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="ff831-110">Blazor WebAssembly apps run on the client.</span></span> <span data-ttu-id="ff831-111">В этом случае авторизация используется *только* для определения отображаемых вариантов пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="ff831-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="ff831-112">Так как пользователь может изменить или обойти проверки на стороне клиента, приложение Blazor WebAssembly не может применять правила авторизации доступа.</span><span class="sxs-lookup"><span data-stu-id="ff831-112">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

<span data-ttu-id="ff831-113">[Соглашения об авторизации Razor Pages](xref:security/authorization/razor-pages-authorization) не применяются к маршрутизируемым компонентам Razor.</span><span class="sxs-lookup"><span data-stu-id="ff831-113">[Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization) don't apply to routable Razor components.</span></span> <span data-ttu-id="ff831-114">Если компонент Razor, не поддерживающий маршрутизацию, [встроен в страницу](xref:blazor/integrate-components#render-components-from-a-page-or-view), соглашения об авторизации страницы оказывают косвенное влияние на компонент Razor, а также на остальную часть содержимого страницы.</span><span class="sxs-lookup"><span data-stu-id="ff831-114">If a non-routable Razor component is [embedded in a page](xref:blazor/integrate-components#render-components-from-a-page-or-view), the page's authorization conventions indirectly affect the Razor component along with the rest of the page's content.</span></span>

## <a name="authentication"></a><span data-ttu-id="ff831-115">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="ff831-115">Authentication</span></span>

<span data-ttu-id="ff831-116">Blazor использует существующие механизмы аутентификации ASP.NET Core для установления личности пользователя.</span><span class="sxs-lookup"><span data-stu-id="ff831-116">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="ff831-117">Конкретный механизм зависит от того, как размещается приложение Blazor (серверное приложение Blazor или приложение Blazor WebAssembly).</span><span class="sxs-lookup"><span data-stu-id="ff831-117">The exact mechanism depends on how the Blazor app is hosted, Blazor Server or Blazor WebAssembly.</span></span>

### <a name="blazor-server-authentication"></a><span data-ttu-id="ff831-118">Аутентификация в приложении Blazor Server</span><span class="sxs-lookup"><span data-stu-id="ff831-118">Blazor Server authentication</span></span>

<span data-ttu-id="ff831-119">Серверные приложения Blazor работают через подключение в реальном времени, созданное с помощью SignalR.</span><span class="sxs-lookup"><span data-stu-id="ff831-119">Blazor Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="ff831-120">[Аутентификация в приложениях на основе SignalR](xref:signalr/authn-and-authz) выполняется при установлении подключения.</span><span class="sxs-lookup"><span data-stu-id="ff831-120">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="ff831-121">Аутентификация может выполняться на основе файлов cookie или других маркеров носителя.</span><span class="sxs-lookup"><span data-stu-id="ff831-121">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="ff831-122">Шаблон серверного проекта Blazor позволяет автоматически настроить аутентификацию при создании проекта.</span><span class="sxs-lookup"><span data-stu-id="ff831-122">The Blazor Server project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ff831-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff831-123">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ff831-124">Следуйте указаниям по работе с Visual Studio (<xref:blazor/get-started>), чтобы создать проект Blazor на стороне сервера с механизмом аутентификации.</span><span class="sxs-lookup"><span data-stu-id="ff831-124">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="ff831-125">Выбрав шаблон **Серверное приложение Blazor** в диалоговом окне **Создать веб-приложение ASP.NET Core**, щелкните **Изменить** в разделе **Проверка подлинности**.</span><span class="sxs-lookup"><span data-stu-id="ff831-125">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="ff831-126">Откроется диалоговое окно с тем же набором механизмов аутентификации, которые доступны для других проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ff831-126">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="ff831-127">**Без аутентификации**.</span><span class="sxs-lookup"><span data-stu-id="ff831-127">**No Authentication**</span></span>
* <span data-ttu-id="ff831-128">**Учетные записи отдельных пользователей**, которые можно хранить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ff831-128">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="ff831-129">в приложении с помощью системы [удостоверений](xref:security/authentication/identity) ASP.NET Core;</span><span class="sxs-lookup"><span data-stu-id="ff831-129">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="ff831-130">в [Azure AD B2C](xref:security/authentication/azure-ad-b2c);</span><span class="sxs-lookup"><span data-stu-id="ff831-130">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="ff831-131">**рабочие или учебные учетные записи**.</span><span class="sxs-lookup"><span data-stu-id="ff831-131">**Work or School Accounts**</span></span>
* <span data-ttu-id="ff831-132">**Проверка подлинности Windows**</span><span class="sxs-lookup"><span data-stu-id="ff831-132">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="ff831-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ff831-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ff831-134">Следуйте указаниям по работе с Visual Studio Code (<xref:blazor/get-started>), чтобы создать проект Blazor на стороне сервера с механизмом аутентификации.</span><span class="sxs-lookup"><span data-stu-id="ff831-134">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="ff831-135">Допустимые значения аутентификации (`{AUTHENTICATION}`) перечислены в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="ff831-135">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="ff831-136">Механизм аутентификации</span><span class="sxs-lookup"><span data-stu-id="ff831-136">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="ff831-137">Значение`{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="ff831-137">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="ff831-138">Без аутентификации</span><span class="sxs-lookup"><span data-stu-id="ff831-138">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="ff831-139">Индивидуальное лицо</span><span class="sxs-lookup"><span data-stu-id="ff831-139">Individual</span></span><br><span data-ttu-id="ff831-140">Пользователи, сохраненные в приложении с помощью ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="ff831-140">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="ff831-141">Индивидуальное лицо</span><span class="sxs-lookup"><span data-stu-id="ff831-141">Individual</span></span><br><span data-ttu-id="ff831-142">Пользователи, сохраненные в [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="ff831-142">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="ff831-143">Рабочие или учебные учетные записи</span><span class="sxs-lookup"><span data-stu-id="ff831-143">Work or School Accounts</span></span><br><span data-ttu-id="ff831-144">Корпоративная аутентификация для отдельного клиента.</span><span class="sxs-lookup"><span data-stu-id="ff831-144">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="ff831-145">Рабочие или учебные учетные записи</span><span class="sxs-lookup"><span data-stu-id="ff831-145">Work or School Accounts</span></span><br><span data-ttu-id="ff831-146">Корпоративная аутентификация для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="ff831-146">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="ff831-147">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="ff831-147">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="ff831-148">Эта команда создает папку, в качестве имени для которой берется значение, предоставленное вместо заполнителя `{APP NAME}`, а затем использует имя папки в качестве имени приложения.</span><span class="sxs-lookup"><span data-stu-id="ff831-148">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="ff831-149">Подробнее см. статью о команде [dotnet new](/dotnet/core/tools/dotnet-new) в руководстве по .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ff831-149">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:

```dotnetcli
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

### <a name="opno-locblazor-webassembly-authentication"></a><span data-ttu-id="ff831-150">Проверка подлинности в Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="ff831-150">Blazor WebAssembly authentication</span></span>

<span data-ttu-id="ff831-151">В приложениях Blazor WebAssembly проверку подлинности можно обойти, так как пользователь может изменять весь код на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="ff831-151">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="ff831-152">Это же справедливо для всех технологий на стороне клиента, включая платформы одностраничного приложения JavaScript или собственных приложений для любой операционной системы.</span><span class="sxs-lookup"><span data-stu-id="ff831-152">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="ff831-153">Добавьте ссылку пакета для [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) в файл проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="ff831-153">Add a package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>

<span data-ttu-id="ff831-154">Реализация пользовательской службы `AuthenticationStateProvider` для приложений Blazor WebAssembly рассматривается в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="ff831-154">Implementation of a custom `AuthenticationStateProvider` service for Blazor WebAssembly apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="ff831-155">Служба AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="ff831-155">AuthenticationStateProvider service</span></span>

<span data-ttu-id="ff831-156">Серверные приложения Blazor включают встроенную службу `AuthenticationStateProvider`, которая получает данные о состоянии проверки подлинности из `HttpContext.User` в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ff831-156">Blazor Server apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="ff831-157">Так состояние аутентификации интегрируется с существующими соответствующими механизмами ASP.NET Core на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="ff831-157">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="ff831-158">`AuthenticationStateProvider` является базовой службой, которую компоненты `AuthorizeView` и `CascadingAuthenticationState` используют для получения состояния аутентификации.</span><span class="sxs-lookup"><span data-stu-id="ff831-158">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="ff831-159">Обычно вы не используете `AuthenticationStateProvider` напрямую.</span><span class="sxs-lookup"><span data-stu-id="ff831-159">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="ff831-160">Выберите подход на основе [компонента AuthorizeView](#authorizeview-component) или [задачи<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter), которые описаны далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="ff831-160">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="ff831-161">Основной недостаток при использовании `AuthenticationStateProvider` напрямую заключается в том, что компонент не получает автоматического уведомления при изменении базовых данных о состоянии аутентификации.</span><span class="sxs-lookup"><span data-stu-id="ff831-161">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="ff831-162">Служба `AuthenticationStateProvider` может предоставить данные <xref:System.Security.Claims.ClaimsPrincipal> о текущем пользователе, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="ff831-162">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="LogUsername">Write user info to console</button>

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

<span data-ttu-id="ff831-163">Если `user.Identity.IsAuthenticated` имеет значение `true`, а пользователь является <xref:System.Security.Claims.ClaimsPrincipal>, можно перечислить утверждения и оценить членство в ролях.</span><span class="sxs-lookup"><span data-stu-id="ff831-163">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="ff831-164">Дополнительные сведения о внедрении зависимостей и службах см. в статьях <xref:blazor/dependency-injection> и <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="ff831-164">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="ff831-165">Реализация пользовательского AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="ff831-165">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="ff831-166">Если вы создаете приложение Blazor WebAssembly или спецификация приложения предусматривает строгое требование включить пользовательский поставщик, создайте реализацию этого поставщика и переопределите `GetAuthenticationStateAsync` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ff831-166">If you're building a Blazor WebAssembly app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Authorization;

namespace BlazorSample.Services
{
    public class CustomAuthStateProvider : AuthenticationStateProvider
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
}
```

<span data-ttu-id="ff831-167">В приложении WebAssembly Blazor служба `CustomAuthStateProvider` регистрируется в `Main` в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ff831-167">In a Blazor WebAssembly app, the `CustomAuthStateProvider` service is registered in `Main` of *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.Extensions.DependencyInjection;
using BlazorSample.Services;

public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddScoped<AuthenticationStateProvider, 
            CustomAuthStateProvider>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

<span data-ttu-id="ff831-168">С помощью `CustomAuthStateProvider`, все пользователи проходят аутентификацию с использованием имени пользователя `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="ff831-168">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="ff831-169">Предоставление состояния аутентификации в качестве каскадного параметра</span><span class="sxs-lookup"><span data-stu-id="ff831-169">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="ff831-170">Если процедурная логика требует данных о состоянии аутентификации, например при выполнении активируемых пользователем действий, для получения таких данных определите каскадный параметр типа `Task<AuthenticationState>`:</span><span class="sxs-lookup"><span data-stu-id="ff831-170">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```razor
@page "/"

<button @onclick="LogUsername">Log username</button>

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

> [!NOTE]
> <span data-ttu-id="ff831-171">В компоненте приложения Blazor WebAssembly добавьте пространство имен `Microsoft.AspNetCore.Components.Authorization` (`@using Microsoft.AspNetCore.Components.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="ff831-171">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Components.Authorization` namespace (`@using Microsoft.AspNetCore.Components.Authorization`).</span></span>

<span data-ttu-id="ff831-172">Если `user.Identity.IsAuthenticated` имеет значение `true`, можно перечислить утверждения и оценить членство в ролях.</span><span class="sxs-lookup"><span data-stu-id="ff831-172">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="ff831-173">Настройте каскадный параметр `Task<AuthenticationState>` с помощью компонентов `AuthorizeRouteView` и `CascadingAuthenticationState` в файле *App.razor*:</span><span class="sxs-lookup"><span data-stu-id="ff831-173">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components in the *App.razor* file:</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization

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

<span data-ttu-id="ff831-174">Добавьте службы для параметров и авторизации в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="ff831-174">Add services for options and authorization to `Program.Main`:</span></span>

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

## <a name="authorization"></a><span data-ttu-id="ff831-175">Авторизация</span><span class="sxs-lookup"><span data-stu-id="ff831-175">Authorization</span></span>

<span data-ttu-id="ff831-176">После аутентификации пользователя применяются правила *авторизации*, которые определяют доступные этому пользователю действия.</span><span class="sxs-lookup"><span data-stu-id="ff831-176">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="ff831-177">Доступ обычно предоставляется или запрещается в зависимости от следующих аспектов:</span><span class="sxs-lookup"><span data-stu-id="ff831-177">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="ff831-178">выполнена ли аутентификация (выполнен ли вход);</span><span class="sxs-lookup"><span data-stu-id="ff831-178">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="ff831-179">есть ли у пользователя определенная *роль*;</span><span class="sxs-lookup"><span data-stu-id="ff831-179">A user is in a *role*.</span></span>
* <span data-ttu-id="ff831-180">есть ли у пользователя определенное *утверждение*;</span><span class="sxs-lookup"><span data-stu-id="ff831-180">A user has a *claim*.</span></span>
* <span data-ttu-id="ff831-181">выполняются ли требования *политики*.</span><span class="sxs-lookup"><span data-stu-id="ff831-181">A *policy* is satisfied.</span></span>

<span data-ttu-id="ff831-182">Каждый из этих аспектов применяется здесь так же, как в приложениях ASP.NET Core MVC или Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ff831-182">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="ff831-183">Дополнительные сведения о безопасности в ASP.NET Core вы найдете в статьях [о безопасности и идентификации в ASP.NET Core](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="ff831-183">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="ff831-184">Компонент AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="ff831-184">AuthorizeView component</span></span>

<span data-ttu-id="ff831-185">Компонент `AuthorizeView` избирательно демонстрирует элементы пользовательского интерфейса в зависимости от того, имеет ли пользователь права на доступ к этим элементам.</span><span class="sxs-lookup"><span data-stu-id="ff831-185">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="ff831-186">Этот подход полезен, если вам нужно просто *отображать* пользователю данные, и не использовать удостоверение пользователя в процедурной логике.</span><span class="sxs-lookup"><span data-stu-id="ff831-186">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="ff831-187">Этот компонент представляет переменную `context` типа `AuthenticationState`, которую можно использовать для доступа к сведениям о пользователе, выполнившем вход:</span><span class="sxs-lookup"><span data-stu-id="ff831-187">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="ff831-188">Также вы можете указать другое содержимое, которое будет отображаться, если пользователь не выполнил аутентификацию:</span><span class="sxs-lookup"><span data-stu-id="ff831-188">You can also supply different content for display if the user isn't authenticated:</span></span>

```razor
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

<span data-ttu-id="ff831-189">Содержимое тегов `<Authorized>` и `<NotAuthorized>` может включать произвольные элементы, например другие интерактивные компоненты.</span><span class="sxs-lookup"><span data-stu-id="ff831-189">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="ff831-190">Условия авторизации, такие как роли или правила для выбора вариантов пользовательского интерфейса и доступа к ним, описаны в разделе [об авторизации](#authorization).</span><span class="sxs-lookup"><span data-stu-id="ff831-190">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="ff831-191">Если условия авторизации не указаны, `AuthorizeView` использует политику по умолчанию со следующими правилами:</span><span class="sxs-lookup"><span data-stu-id="ff831-191">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="ff831-192">выполнившие аутентификацию (выполнившие вход) пользователи считаются авторизованными;</span><span class="sxs-lookup"><span data-stu-id="ff831-192">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="ff831-193">не выполнившие аутентификацию (не выполнившие вход) пользователи считаются не авторизованными.</span><span class="sxs-lookup"><span data-stu-id="ff831-193">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="ff831-194">Авторизация на основе ролей и политик</span><span class="sxs-lookup"><span data-stu-id="ff831-194">Role-based and policy-based authorization</span></span>

<span data-ttu-id="ff831-195">Компонент `AuthorizeView` поддерживает авторизацию на основе *ролей* или *политик*.</span><span class="sxs-lookup"><span data-stu-id="ff831-195">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="ff831-196">Для авторизации на основе ролей примените параметр `Roles`:</span><span class="sxs-lookup"><span data-stu-id="ff831-196">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="ff831-197">Для получения дополнительной информации см. <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="ff831-197">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="ff831-198">Для авторизации на основе политик примените параметр `Policy`:</span><span class="sxs-lookup"><span data-stu-id="ff831-198">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="ff831-199">Авторизация на основе утверждений считается особым случаем авторизации на основе политик.</span><span class="sxs-lookup"><span data-stu-id="ff831-199">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="ff831-200">Например, вы можете определить политику, которая требует наличия определенного утверждения у пользователя.</span><span class="sxs-lookup"><span data-stu-id="ff831-200">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="ff831-201">Для получения дополнительной информации см. <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="ff831-201">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="ff831-202">Эти API могут использоваться в серверных приложениях Blazor и приложениях Blazor WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="ff831-202">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="ff831-203">Если не указано ни `Roles`, ни `Policy`, `AuthorizeView` использует политику по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ff831-203">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="ff831-204">Содержимое, отображаемое при асинхронной аутентификации</span><span class="sxs-lookup"><span data-stu-id="ff831-204">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="ff831-205"> позволяет *асинхронно* определять состояние проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="ff831-205"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="ff831-206">Основной сценарий для такого подхода — приложение Blazor WebAssembly, которое направляет запрос на проверку подлинности во внешнюю конечную точку.</span><span class="sxs-lookup"><span data-stu-id="ff831-206">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="ff831-207">Пока аутентификация выполняется, `AuthorizeView` по умолчанию не отображает содержимое.</span><span class="sxs-lookup"><span data-stu-id="ff831-207">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="ff831-208">Чтобы содержимое отображалось, используйте элемент `<Authorizing>`:</span><span class="sxs-lookup"><span data-stu-id="ff831-208">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

```razor
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

<span data-ttu-id="ff831-209">Такой подход обычно не применим к серверным приложениям Blazor.</span><span class="sxs-lookup"><span data-stu-id="ff831-209">This approach isn't normally applicable to Blazor Server apps.</span></span> <span data-ttu-id="ff831-210">Серверные приложения Blazor узнают состояние проверки подлинности, как только оно устанавливается.</span><span class="sxs-lookup"><span data-stu-id="ff831-210">Blazor Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="ff831-211">Содержимое `Authorizing` можно указать в компоненте `AuthorizeView` для серверного приложения Blazor, но это содержимое никогда не отображается.</span><span class="sxs-lookup"><span data-stu-id="ff831-211">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="ff831-212">Атрибут [Authorize]</span><span class="sxs-lookup"><span data-stu-id="ff831-212">[Authorize] attribute</span></span>

<span data-ttu-id="ff831-213">Атрибут `[Authorize]` можно использовать в компонентах Razor:</span><span class="sxs-lookup"><span data-stu-id="ff831-213">The `[Authorize]` attribute can be used in Razor components:</span></span>

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> <span data-ttu-id="ff831-214">В компоненте приложения Blazor WebAssembly добавьте пространство имен `Microsoft.AspNetCore.Authorization` (`@using Microsoft.AspNetCore.Authorization`) к примерам в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="ff831-214">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` namespace (`@using Microsoft.AspNetCore.Authorization`) to the examples in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ff831-215">Используйте `[Authorize]` только для компонентов `@page`, полученных через маршрутизатор Blazor.</span><span class="sxs-lookup"><span data-stu-id="ff831-215">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="ff831-216">Авторизация выполняется только как аспект маршрутизации и *не* для дочерних компонентов, которые отображаются на странице.</span><span class="sxs-lookup"><span data-stu-id="ff831-216">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="ff831-217">Чтобы разрешить отображение конкретных частей на странице, используйте вместо этого `AuthorizeView`.</span><span class="sxs-lookup"><span data-stu-id="ff831-217">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="ff831-218">Атрибут `[Authorize]` также поддерживает авторизацию на основе ролей или политик.</span><span class="sxs-lookup"><span data-stu-id="ff831-218">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="ff831-219">Для авторизации на основе ролей примените параметр `Roles`:</span><span class="sxs-lookup"><span data-stu-id="ff831-219">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="ff831-220">Для авторизации на основе политик примените параметр `Policy`:</span><span class="sxs-lookup"><span data-stu-id="ff831-220">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="ff831-221">Если не указано ни `Roles`, ни `Policy`, `[Authorize]` использует политику по умолчанию со следующими правилами:</span><span class="sxs-lookup"><span data-stu-id="ff831-221">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="ff831-222">выполнившие аутентификацию (выполнившие вход) пользователи считаются авторизованными;</span><span class="sxs-lookup"><span data-stu-id="ff831-222">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="ff831-223">не выполнившие аутентификацию (не выполнившие вход) пользователи считаются не авторизованными.</span><span class="sxs-lookup"><span data-stu-id="ff831-223">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="ff831-224">Настройка несанкционированного содержимого для компонента маршрутизатора</span><span class="sxs-lookup"><span data-stu-id="ff831-224">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="ff831-225">Компонент `Router` вместе с компонентом `AuthorizeRouteView` позволяет приложению указать пользовательское содержимое для следующих ситуаций:</span><span class="sxs-lookup"><span data-stu-id="ff831-225">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="ff831-226">содержимое не найдено;</span><span class="sxs-lookup"><span data-stu-id="ff831-226">Content isn't found.</span></span>
* <span data-ttu-id="ff831-227">пользователь не удовлетворяет условию `[Authorize]`, которое применено к компоненту</span><span class="sxs-lookup"><span data-stu-id="ff831-227">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="ff831-228">(атрибут `[Authorize]` описан в разделе [Атрибут `[Authorize]`](#authorize-attribute));</span><span class="sxs-lookup"><span data-stu-id="ff831-228">The `[Authorize]` attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="ff831-229">выполняется асинхронная аутентификация.</span><span class="sxs-lookup"><span data-stu-id="ff831-229">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="ff831-230">В стандартном шаблоне проекта Blazor Server есть файл *App.razor* с примером пользовательского содержимого:</span><span class="sxs-lookup"><span data-stu-id="ff831-230">In the default Blazor Server project template, the *App.razor* file demonstrates how to set custom content:</span></span>

```razor
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

<span data-ttu-id="ff831-231">Содержимое тегов `<NotFound>`, `<NotAuthorized>` и `<Authorizing>` может включать произвольные элементы, например другие интерактивные компоненты.</span><span class="sxs-lookup"><span data-stu-id="ff831-231">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="ff831-232">Если элемент `<NotAuthorized>` не указан, `AuthorizeRouteView` использует следующее резервное сообщение:</span><span class="sxs-lookup"><span data-stu-id="ff831-232">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="ff831-233">Уведомление об изменении состояния аутентификации</span><span class="sxs-lookup"><span data-stu-id="ff831-233">Notification about authentication state changes</span></span>

<span data-ttu-id="ff831-234">Если приложение определяет, что изменились базовые данные о состоянии аутентификации (например, пользователь вышел из системы или другой пользователь изменил роль), `AuthenticationStateProvider` пользователя может вызвать необязательный метод `NotifyAuthenticationStateChanged` из базового класса `AuthenticationStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="ff831-234">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="ff831-235">Это действие информирует потребителей данных состояния аутентификации (таких, как `AuthorizeView`) о необходимости повторно отображения с учетом новых данных.</span><span class="sxs-lookup"><span data-stu-id="ff831-235">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="ff831-236">Процедурная логика</span><span class="sxs-lookup"><span data-stu-id="ff831-236">Procedural logic</span></span>

<span data-ttu-id="ff831-237">Если приложению нужно проверять правила авторизации в составе процедурной логики, используйте каскадный параметр с типом `Task<AuthenticationState>`, чтобы получить <xref:System.Security.Claims.ClaimsPrincipal> пользователя.</span><span class="sxs-lookup"><span data-stu-id="ff831-237">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="ff831-238">`Task<AuthenticationState>` можно комбинировать для оценки политик с другими службами, например `IAuthorizationService`.</span><span class="sxs-lookup"><span data-stu-id="ff831-238">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

```razor
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

> [!NOTE]
> <span data-ttu-id="ff831-239">В компоненте приложения Blazor WebAssembly добавьте пространства имен `Microsoft.AspNetCore.Authorization` и `Microsoft.AspNetCore.Components.Authorization`:</span><span class="sxs-lookup"><span data-stu-id="ff831-239">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a><span data-ttu-id="ff831-240">Авторизация в приложениях Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="ff831-240">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="ff831-241">В приложениях Blazor WebAssembly авторизацию можно обойти, так как пользователь может изменять весь код на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="ff831-241">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="ff831-242">Это же справедливо для всех технологий на стороне клиента, включая платформы одностраничного приложения JavaScript или собственных приложений для любой операционной системы.</span><span class="sxs-lookup"><span data-stu-id="ff831-242">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="ff831-243">**Всегда выполняйте проверки авторизации на стороне сервера в конечных точках API, к которым обращается клиентское приложение.**</span><span class="sxs-lookup"><span data-stu-id="ff831-243">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

<span data-ttu-id="ff831-244">Дополнительные сведения см. в статьях в документе <xref:security/blazor/webassembly/index>.</span><span class="sxs-lookup"><span data-stu-id="ff831-244">For more information, see the articles under <xref:security/blazor/webassembly/index>.</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="ff831-245">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="ff831-245">Troubleshoot errors</span></span>

<span data-ttu-id="ff831-246">Распространенные ошибки</span><span class="sxs-lookup"><span data-stu-id="ff831-246">Common errors:</span></span>

* <span data-ttu-id="ff831-247">**Для авторизации требуется каскадный параметр с типом \<AuthenticationState>. Попробуйте предоставить его с помощью CascadingAuthenticationState.**</span><span class="sxs-lookup"><span data-stu-id="ff831-247">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="ff831-248">**Для `authenticationStateTask` возвращается значение `null`**</span><span class="sxs-lookup"><span data-stu-id="ff831-248">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="ff831-249">Вполне вероятно, что проект не был создан на основе шаблона серверного приложения Blazor с включенной проверкой подлинности.</span><span class="sxs-lookup"><span data-stu-id="ff831-249">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="ff831-250">Примените `<CascadingAuthenticationState>` в качестве оболочки некоторой части дерева пользовательского интерфейса, например в *App.razor*, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ff831-250">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="ff831-251">`CascadingAuthenticationState` предоставляет каскадный параметр `Task<AuthenticationState>`, который он в свою очередь получает из базовой службы внедрения зависимостей `AuthenticationStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="ff831-251">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff831-252">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ff831-252">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
* <span data-ttu-id="ff831-253">[Awesome Blazor: проверка подлинности](https://github.com/AdrienTorris/awesome-blazor#authentication) — ссылки на примеры от сообщества</span><span class="sxs-lookup"><span data-stu-id="ff831-253">[Awesome Blazor: Authentication](https://github.com/AdrienTorris/awesome-blazor#authentication) community sample links</span></span>
