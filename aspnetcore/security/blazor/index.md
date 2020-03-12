---
title: Проверка подлинности и авторизация в ASP.NET Core Blazor
author: guardrex
description: Сведения о проверке подлинности и авторизации в Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: ba00c0370094df4e0ae0479aeb23eceea6ef04f0
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083347"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="0c87c-103">Аутентификация и авторизация в ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="0c87c-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="0c87c-104">Автор: [Стив Сандерсон](https://github.com/SteveSandersonMS) (Steve Sanderson)</span><span class="sxs-lookup"><span data-stu-id="0c87c-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="0c87c-105">ASP.NET Core поддерживает настройку и администрирование средств безопасности в приложениях Blazor.</span><span class="sxs-lookup"><span data-stu-id="0c87c-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="0c87c-106">Сценарии безопасности для серверных приложений Blazor и приложений Blazor WebAssembly отличаются.</span><span class="sxs-lookup"><span data-stu-id="0c87c-106">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="0c87c-107">Так как серверные приложения Blazor выполняются на стороне сервера, проверки авторизации могут определить следующее:</span><span class="sxs-lookup"><span data-stu-id="0c87c-107">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="0c87c-108">параметры пользовательского интерфейса, предоставленные пользователю (например, какие пункты меню доступны пользователю);</span><span class="sxs-lookup"><span data-stu-id="0c87c-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="0c87c-109">правила доступа для приложений и компонентов.</span><span class="sxs-lookup"><span data-stu-id="0c87c-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="0c87c-110">Приложения Blazor WebAssembly выполняются на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="0c87c-110">Blazor WebAssembly apps run on the client.</span></span> <span data-ttu-id="0c87c-111">В этом случае авторизация используется *только* для определения отображаемых вариантов пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="0c87c-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="0c87c-112">Так как пользователь может изменить или обойти проверки на стороне клиента, приложение Blazor WebAssembly не может применять правила авторизации доступа.</span><span class="sxs-lookup"><span data-stu-id="0c87c-112">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="0c87c-113">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="0c87c-113">Authentication</span></span>

<span data-ttu-id="0c87c-114">Blazor использует существующие механизмы аутентификации ASP.NET Core для установления личности пользователя.</span><span class="sxs-lookup"><span data-stu-id="0c87c-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="0c87c-115">Конкретный механизм зависит от того, как размещается приложение Blazor (серверное приложение Blazor или приложение Blazor WebAssembly).</span><span class="sxs-lookup"><span data-stu-id="0c87c-115">The exact mechanism depends on how the Blazor app is hosted, Blazor Server or Blazor WebAssembly.</span></span>

### <a name="blazor-server-authentication"></a><span data-ttu-id="0c87c-116">Аутентификация в приложении Blazor Server</span><span class="sxs-lookup"><span data-stu-id="0c87c-116">Blazor Server authentication</span></span>

<span data-ttu-id="0c87c-117">Серверные приложения Blazor работают через подключение в реальном времени, созданное с помощью SignalR.</span><span class="sxs-lookup"><span data-stu-id="0c87c-117">Blazor Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="0c87c-118">[Аутентификация в приложениях на основе SignalR](xref:signalr/authn-and-authz) выполняется при установлении подключения.</span><span class="sxs-lookup"><span data-stu-id="0c87c-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="0c87c-119">Аутентификация может выполняться на основе файлов cookie или других маркеров носителя.</span><span class="sxs-lookup"><span data-stu-id="0c87c-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="0c87c-120">Шаблон серверного проекта Blazor позволяет автоматически настроить аутентификацию при создании проекта.</span><span class="sxs-lookup"><span data-stu-id="0c87c-120">The Blazor Server project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="0c87c-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c87c-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0c87c-122">Следуйте указаниям по работе с Visual Studio (<xref:blazor/get-started>), чтобы создать проект Blazor на стороне сервера с механизмом аутентификации.</span><span class="sxs-lookup"><span data-stu-id="0c87c-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="0c87c-123">Выбрав шаблон **Серверное приложение Blazor** в диалоговом окне **Создать веб-приложение ASP.NET Core**, щелкните **Изменить** в разделе **Проверка подлинности**.</span><span class="sxs-lookup"><span data-stu-id="0c87c-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="0c87c-124">Откроется диалоговое окно с тем же набором механизмов аутентификации, которые доступны для других проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c87c-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="0c87c-125">**Без аутентификации**.</span><span class="sxs-lookup"><span data-stu-id="0c87c-125">**No Authentication**</span></span>
* <span data-ttu-id="0c87c-126">**Учетные записи отдельных пользователей**, которые можно хранить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0c87c-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="0c87c-127">в приложении с помощью системы [удостоверений](xref:security/authentication/identity) ASP.NET Core;</span><span class="sxs-lookup"><span data-stu-id="0c87c-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="0c87c-128">в [Azure AD B2C](xref:security/authentication/azure-ad-b2c);</span><span class="sxs-lookup"><span data-stu-id="0c87c-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="0c87c-129">**рабочие или учебные учетные записи**.</span><span class="sxs-lookup"><span data-stu-id="0c87c-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="0c87c-130">**Проверка подлинности Windows**</span><span class="sxs-lookup"><span data-stu-id="0c87c-130">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="0c87c-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0c87c-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0c87c-132">Следуйте указаниям по работе с Visual Studio Code (<xref:blazor/get-started>), чтобы создать проект Blazor на стороне сервера с механизмом аутентификации.</span><span class="sxs-lookup"><span data-stu-id="0c87c-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="0c87c-133">Допустимые значения аутентификации (`{AUTHENTICATION}`) перечислены в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="0c87c-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="0c87c-134">Механизм аутентификации</span><span class="sxs-lookup"><span data-stu-id="0c87c-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="0c87c-135">Значение`{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="0c87c-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="0c87c-136">Без аутентификации</span><span class="sxs-lookup"><span data-stu-id="0c87c-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="0c87c-137">Индивидуальное лицо</span><span class="sxs-lookup"><span data-stu-id="0c87c-137">Individual</span></span><br><span data-ttu-id="0c87c-138">Пользователи, сохраненные в приложении с помощью ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="0c87c-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="0c87c-139">Индивидуальное лицо</span><span class="sxs-lookup"><span data-stu-id="0c87c-139">Individual</span></span><br><span data-ttu-id="0c87c-140">Пользователи, сохраненные в [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="0c87c-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="0c87c-141">Рабочие или учебные учетные записи</span><span class="sxs-lookup"><span data-stu-id="0c87c-141">Work or School Accounts</span></span><br><span data-ttu-id="0c87c-142">Корпоративная аутентификация для отдельного клиента.</span><span class="sxs-lookup"><span data-stu-id="0c87c-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="0c87c-143">Рабочие или учебные учетные записи</span><span class="sxs-lookup"><span data-stu-id="0c87c-143">Work or School Accounts</span></span><br><span data-ttu-id="0c87c-144">Корпоративная аутентификация для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="0c87c-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="0c87c-145">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="0c87c-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="0c87c-146">Эта команда создает папку, в качестве имени для которой берется значение, предоставленное вместо заполнителя `{APP NAME}`, а затем использует имя папки в качестве имени приложения.</span><span class="sxs-lookup"><span data-stu-id="0c87c-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="0c87c-147">Подробнее см. статью о команде [dotnet new](/dotnet/core/tools/dotnet-new) в руководстве по .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c87c-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

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

### <a name="opno-locblazor-webassembly-authentication"></a><span data-ttu-id="0c87c-148">Проверка подлинности в Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="0c87c-148">Blazor WebAssembly authentication</span></span>

<span data-ttu-id="0c87c-149">В приложениях Blazor WebAssembly проверку подлинности можно обойти, так как пользователь может изменять весь код на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="0c87c-149">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="0c87c-150">Это же справедливо для всех технологий на стороне клиента, включая платформы одностраничного приложения JavaScript или собственных приложений для любой операционной системы.</span><span class="sxs-lookup"><span data-stu-id="0c87c-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="0c87c-151">Добавьте ссылку пакета для [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) в файл проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="0c87c-151">Add a package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>

<span data-ttu-id="0c87c-152">Реализация пользовательской службы `AuthenticationStateProvider` для приложений Blazor WebAssembly рассматривается в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="0c87c-152">Implementation of a custom `AuthenticationStateProvider` service for Blazor WebAssembly apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="0c87c-153">Служба AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="0c87c-153">AuthenticationStateProvider service</span></span>

<span data-ttu-id="0c87c-154">Серверные приложения Blazor включают встроенную службу `AuthenticationStateProvider`, которая получает данные о состоянии проверки подлинности из `HttpContext.User` в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c87c-154">Blazor Server apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="0c87c-155">Так состояние аутентификации интегрируется с существующими соответствующими механизмами ASP.NET Core на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="0c87c-155">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="0c87c-156">`AuthenticationStateProvider` является базовой службой, которую компоненты `AuthorizeView` и `CascadingAuthenticationState` используют для получения состояния аутентификации.</span><span class="sxs-lookup"><span data-stu-id="0c87c-156">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="0c87c-157">Обычно вы не используете `AuthenticationStateProvider` напрямую.</span><span class="sxs-lookup"><span data-stu-id="0c87c-157">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="0c87c-158">Выберите подход на основе [компонента AuthorizeView](#authorizeview-component) или [задачи<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter), которые описаны далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="0c87c-158">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="0c87c-159">Основной недостаток при использовании `AuthenticationStateProvider` напрямую заключается в том, что компонент не получает автоматического уведомления при изменении базовых данных о состоянии аутентификации.</span><span class="sxs-lookup"><span data-stu-id="0c87c-159">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="0c87c-160">Служба `AuthenticationStateProvider` может предоставить данные <xref:System.Security.Claims.ClaimsPrincipal> о текущем пользователе, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="0c87c-160">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

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

<span data-ttu-id="0c87c-161">Если `user.Identity.IsAuthenticated` имеет значение `true`, а пользователь является <xref:System.Security.Claims.ClaimsPrincipal>, можно перечислить утверждения и оценить членство в ролях.</span><span class="sxs-lookup"><span data-stu-id="0c87c-161">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="0c87c-162">Дополнительные сведения о внедрении зависимостей и службах см. в статьях <xref:blazor/dependency-injection> и <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="0c87c-162">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="0c87c-163">Реализация пользовательского AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="0c87c-163">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="0c87c-164">Если вы создаете приложение Blazor WebAssembly или спецификация приложения предусматривает строгое требование включить пользовательский поставщик, создайте реализацию этого поставщика и переопределите `GetAuthenticationStateAsync` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0c87c-164">If you're building a Blazor WebAssembly app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

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

<span data-ttu-id="0c87c-165">В приложении WebAssembly Blazor служба `CustomAuthStateProvider` регистрируется в `Main` в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="0c87c-165">In a Blazor WebAssembly app, the `CustomAuthStateProvider` service is registered in `Main` of *Program.cs*:</span></span>

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

<span data-ttu-id="0c87c-166">С помощью `CustomAuthStateProvider`, все пользователи проходят аутентификацию с использованием имени пользователя `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="0c87c-166">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="0c87c-167">Предоставление состояния аутентификации в качестве каскадного параметра</span><span class="sxs-lookup"><span data-stu-id="0c87c-167">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="0c87c-168">Если процедурная логика требует данных о состоянии аутентификации, например при выполнении активируемых пользователем действий, для получения таких данных определите каскадный параметр типа `Task<AuthenticationState>`:</span><span class="sxs-lookup"><span data-stu-id="0c87c-168">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

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
> <span data-ttu-id="0c87c-169">В компоненте приложения Blazor WebAssembly добавьте пространство имен `Microsoft.AspNetCore.Components.Authorization` (`@using Microsoft.AspNetCore.Components.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="0c87c-169">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Components.Authorization` namespace (`@using Microsoft.AspNetCore.Components.Authorization`).</span></span>

<span data-ttu-id="0c87c-170">Если `user.Identity.IsAuthenticated` имеет значение `true`, можно перечислить утверждения и оценить членство в ролях.</span><span class="sxs-lookup"><span data-stu-id="0c87c-170">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="0c87c-171">Настройте каскадный параметр `Task<AuthenticationState>` с помощью компонентов `AuthorizeRouteView` и `CascadingAuthenticationState` в файле *App.razor*:</span><span class="sxs-lookup"><span data-stu-id="0c87c-171">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components in the *App.razor* file:</span></span>

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

<span data-ttu-id="0c87c-172">Добавьте службы для параметров и авторизации в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="0c87c-172">Add services for options and authorization to `Program.Main`:</span></span>

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

## <a name="authorization"></a><span data-ttu-id="0c87c-173">Авторизация</span><span class="sxs-lookup"><span data-stu-id="0c87c-173">Authorization</span></span>

<span data-ttu-id="0c87c-174">После аутентификации пользователя применяются правила *авторизации*, которые определяют доступные этому пользователю действия.</span><span class="sxs-lookup"><span data-stu-id="0c87c-174">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="0c87c-175">Доступ обычно предоставляется или запрещается в зависимости от следующих аспектов:</span><span class="sxs-lookup"><span data-stu-id="0c87c-175">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="0c87c-176">выполнена ли аутентификация (выполнен ли вход);</span><span class="sxs-lookup"><span data-stu-id="0c87c-176">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="0c87c-177">есть ли у пользователя определенная *роль*;</span><span class="sxs-lookup"><span data-stu-id="0c87c-177">A user is in a *role*.</span></span>
* <span data-ttu-id="0c87c-178">есть ли у пользователя определенное *утверждение*;</span><span class="sxs-lookup"><span data-stu-id="0c87c-178">A user has a *claim*.</span></span>
* <span data-ttu-id="0c87c-179">выполняются ли требования *политики*.</span><span class="sxs-lookup"><span data-stu-id="0c87c-179">A *policy* is satisfied.</span></span>

<span data-ttu-id="0c87c-180">Каждый из этих аспектов применяется здесь так же, как в приложениях ASP.NET Core MVC или Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0c87c-180">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="0c87c-181">Дополнительные сведения о безопасности в ASP.NET Core вы найдете в статьях [о безопасности и идентификации в ASP.NET Core](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="0c87c-181">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="0c87c-182">Компонент AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="0c87c-182">AuthorizeView component</span></span>

<span data-ttu-id="0c87c-183">Компонент `AuthorizeView` избирательно демонстрирует элементы пользовательского интерфейса в зависимости от того, имеет ли пользователь права на доступ к этим элементам.</span><span class="sxs-lookup"><span data-stu-id="0c87c-183">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="0c87c-184">Этот подход полезен, если вам нужно просто *отображать* пользователю данные, и не использовать удостоверение пользователя в процедурной логике.</span><span class="sxs-lookup"><span data-stu-id="0c87c-184">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="0c87c-185">Этот компонент представляет переменную `context` типа `AuthenticationState`, которую можно использовать для доступа к сведениям о пользователе, выполнившем вход:</span><span class="sxs-lookup"><span data-stu-id="0c87c-185">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="0c87c-186">Также вы можете указать другое содержимое, которое будет отображаться, если пользователь не выполнил аутентификацию:</span><span class="sxs-lookup"><span data-stu-id="0c87c-186">You can also supply different content for display if the user isn't authenticated:</span></span>

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

<span data-ttu-id="0c87c-187">Содержимое тегов `<Authorized>` и `<NotAuthorized>` может включать произвольные элементы, например другие интерактивные компоненты.</span><span class="sxs-lookup"><span data-stu-id="0c87c-187">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="0c87c-188">Условия авторизации, такие как роли или правила для выбора вариантов пользовательского интерфейса и доступа к ним, описаны в разделе [об авторизации](#authorization).</span><span class="sxs-lookup"><span data-stu-id="0c87c-188">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="0c87c-189">Если условия авторизации не указаны, `AuthorizeView` использует политику по умолчанию со следующими правилами:</span><span class="sxs-lookup"><span data-stu-id="0c87c-189">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="0c87c-190">выполнившие аутентификацию (выполнившие вход) пользователи считаются авторизованными;</span><span class="sxs-lookup"><span data-stu-id="0c87c-190">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="0c87c-191">не выполнившие аутентификацию (не выполнившие вход) пользователи считаются не авторизованными.</span><span class="sxs-lookup"><span data-stu-id="0c87c-191">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="0c87c-192">Авторизация на основе ролей и политик</span><span class="sxs-lookup"><span data-stu-id="0c87c-192">Role-based and policy-based authorization</span></span>

<span data-ttu-id="0c87c-193">Компонент `AuthorizeView` поддерживает авторизацию на основе *ролей* или *политик*.</span><span class="sxs-lookup"><span data-stu-id="0c87c-193">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="0c87c-194">Для авторизации на основе ролей примените параметр `Roles`:</span><span class="sxs-lookup"><span data-stu-id="0c87c-194">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="0c87c-195">Для получения дополнительной информации см. <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="0c87c-195">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="0c87c-196">Для авторизации на основе политик примените параметр `Policy`:</span><span class="sxs-lookup"><span data-stu-id="0c87c-196">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="0c87c-197">Авторизация на основе утверждений считается особым случаем авторизации на основе политик.</span><span class="sxs-lookup"><span data-stu-id="0c87c-197">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="0c87c-198">Например, вы можете определить политику, которая требует наличия определенного утверждения у пользователя.</span><span class="sxs-lookup"><span data-stu-id="0c87c-198">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="0c87c-199">Для получения дополнительной информации см. <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="0c87c-199">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="0c87c-200">Эти API могут использоваться в серверных приложениях Blazor и приложениях Blazor WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="0c87c-200">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="0c87c-201">Если не указано ни `Roles`, ни `Policy`, `AuthorizeView` использует политику по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0c87c-201">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="0c87c-202">Содержимое, отображаемое при асинхронной аутентификации</span><span class="sxs-lookup"><span data-stu-id="0c87c-202">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="0c87c-203"> позволяет *асинхронно* определять состояние проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="0c87c-203"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="0c87c-204">Основной сценарий для такого подхода — приложение Blazor WebAssembly, которое направляет запрос на проверку подлинности во внешнюю конечную точку.</span><span class="sxs-lookup"><span data-stu-id="0c87c-204">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="0c87c-205">Пока аутентификация выполняется, `AuthorizeView` по умолчанию не отображает содержимое.</span><span class="sxs-lookup"><span data-stu-id="0c87c-205">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="0c87c-206">Чтобы содержимое отображалось, используйте элемент `<Authorizing>`:</span><span class="sxs-lookup"><span data-stu-id="0c87c-206">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

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

<span data-ttu-id="0c87c-207">Такой подход обычно не применим к серверным приложениям Blazor.</span><span class="sxs-lookup"><span data-stu-id="0c87c-207">This approach isn't normally applicable to Blazor Server apps.</span></span> <span data-ttu-id="0c87c-208">Серверные приложения Blazor узнают состояние проверки подлинности, как только оно устанавливается.</span><span class="sxs-lookup"><span data-stu-id="0c87c-208">Blazor Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="0c87c-209">Содержимое `Authorizing` можно указать в компоненте `AuthorizeView` для серверного приложения Blazor, но это содержимое никогда не отображается.</span><span class="sxs-lookup"><span data-stu-id="0c87c-209">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="0c87c-210">Атрибут [Authorize]</span><span class="sxs-lookup"><span data-stu-id="0c87c-210">[Authorize] attribute</span></span>

<span data-ttu-id="0c87c-211">Атрибут `[Authorize]` можно использовать в компонентах Razor:</span><span class="sxs-lookup"><span data-stu-id="0c87c-211">The `[Authorize]` attribute can be used in Razor components:</span></span>

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> <span data-ttu-id="0c87c-212">В компоненте приложения Blazor WebAssembly добавьте пространство имен `Microsoft.AspNetCore.Authorization` (`@using Microsoft.AspNetCore.Authorization`) к примерам в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="0c87c-212">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` namespace (`@using Microsoft.AspNetCore.Authorization`) to the examples in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0c87c-213">Используйте `[Authorize]` только для компонентов `@page`, полученных через маршрутизатор Blazor.</span><span class="sxs-lookup"><span data-stu-id="0c87c-213">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="0c87c-214">Авторизация выполняется только как аспект маршрутизации и *не* для дочерних компонентов, которые отображаются на странице.</span><span class="sxs-lookup"><span data-stu-id="0c87c-214">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="0c87c-215">Чтобы разрешить отображение конкретных частей на странице, используйте вместо этого `AuthorizeView`.</span><span class="sxs-lookup"><span data-stu-id="0c87c-215">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="0c87c-216">Атрибут `[Authorize]` также поддерживает авторизацию на основе ролей или политик.</span><span class="sxs-lookup"><span data-stu-id="0c87c-216">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="0c87c-217">Для авторизации на основе ролей примените параметр `Roles`:</span><span class="sxs-lookup"><span data-stu-id="0c87c-217">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="0c87c-218">Для авторизации на основе политик примените параметр `Policy`:</span><span class="sxs-lookup"><span data-stu-id="0c87c-218">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="0c87c-219">Если не указано ни `Roles`, ни `Policy`, `[Authorize]` использует политику по умолчанию со следующими правилами:</span><span class="sxs-lookup"><span data-stu-id="0c87c-219">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="0c87c-220">выполнившие аутентификацию (выполнившие вход) пользователи считаются авторизованными;</span><span class="sxs-lookup"><span data-stu-id="0c87c-220">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="0c87c-221">не выполнившие аутентификацию (не выполнившие вход) пользователи считаются не авторизованными.</span><span class="sxs-lookup"><span data-stu-id="0c87c-221">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="0c87c-222">Настройка несанкционированного содержимого для компонента маршрутизатора</span><span class="sxs-lookup"><span data-stu-id="0c87c-222">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="0c87c-223">Компонент `Router` вместе с компонентом `AuthorizeRouteView` позволяет приложению указать пользовательское содержимое для следующих ситуаций:</span><span class="sxs-lookup"><span data-stu-id="0c87c-223">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="0c87c-224">содержимое не найдено;</span><span class="sxs-lookup"><span data-stu-id="0c87c-224">Content isn't found.</span></span>
* <span data-ttu-id="0c87c-225">пользователь не удовлетворяет условию `[Authorize]`, которое применено к компоненту</span><span class="sxs-lookup"><span data-stu-id="0c87c-225">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="0c87c-226">(атрибут `[Authorize]` описан в разделе [Атрибут `[Authorize]`](#authorize-attribute));</span><span class="sxs-lookup"><span data-stu-id="0c87c-226">The `[Authorize]` attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="0c87c-227">выполняется асинхронная аутентификация.</span><span class="sxs-lookup"><span data-stu-id="0c87c-227">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="0c87c-228">В стандартном шаблоне проекта Blazor Server есть файл *App.razor* с примером пользовательского содержимого:</span><span class="sxs-lookup"><span data-stu-id="0c87c-228">In the default Blazor Server project template, the *App.razor* file demonstrates how to set custom content:</span></span>

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

<span data-ttu-id="0c87c-229">Содержимое тегов `<NotFound>`, `<NotAuthorized>` и `<Authorizing>` может включать произвольные элементы, например другие интерактивные компоненты.</span><span class="sxs-lookup"><span data-stu-id="0c87c-229">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="0c87c-230">Если элемент `<NotAuthorized>` не указан, `AuthorizeRouteView` использует следующее резервное сообщение:</span><span class="sxs-lookup"><span data-stu-id="0c87c-230">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="0c87c-231">Уведомление об изменении состояния аутентификации</span><span class="sxs-lookup"><span data-stu-id="0c87c-231">Notification about authentication state changes</span></span>

<span data-ttu-id="0c87c-232">Если приложение определяет, что изменились базовые данные о состоянии аутентификации (например, пользователь вышел из системы или другой пользователь изменил роль), `AuthenticationStateProvider` пользователя может вызвать необязательный метод `NotifyAuthenticationStateChanged` из базового класса `AuthenticationStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="0c87c-232">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="0c87c-233">Это действие информирует потребителей данных состояния аутентификации (таких, как `AuthorizeView`) о необходимости повторно отображения с учетом новых данных.</span><span class="sxs-lookup"><span data-stu-id="0c87c-233">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="0c87c-234">Процедурная логика</span><span class="sxs-lookup"><span data-stu-id="0c87c-234">Procedural logic</span></span>

<span data-ttu-id="0c87c-235">Если приложению нужно проверять правила авторизации в составе процедурной логики, используйте каскадный параметр с типом `Task<AuthenticationState>`, чтобы получить <xref:System.Security.Claims.ClaimsPrincipal> пользователя.</span><span class="sxs-lookup"><span data-stu-id="0c87c-235">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="0c87c-236">`Task<AuthenticationState>` можно комбинировать для оценки политик с другими службами, например `IAuthorizationService`.</span><span class="sxs-lookup"><span data-stu-id="0c87c-236">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

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
> <span data-ttu-id="0c87c-237">В компоненте приложения Blazor WebAssembly добавьте пространства имен `Microsoft.AspNetCore.Authorization` и `Microsoft.AspNetCore.Components.Authorization`:</span><span class="sxs-lookup"><span data-stu-id="0c87c-237">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a><span data-ttu-id="0c87c-238">Авторизация в приложениях Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="0c87c-238">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="0c87c-239">В приложениях Blazor WebAssembly авторизацию можно обойти, так как пользователь может изменять весь код на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="0c87c-239">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="0c87c-240">Это же справедливо для всех технологий на стороне клиента, включая платформы одностраничного приложения JavaScript или собственных приложений для любой операционной системы.</span><span class="sxs-lookup"><span data-stu-id="0c87c-240">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="0c87c-241">**Всегда выполняйте проверки авторизации на стороне сервера в конечных точках API, к которым обращается клиентское приложение.**</span><span class="sxs-lookup"><span data-stu-id="0c87c-241">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

<span data-ttu-id="0c87c-242">Дополнительные сведения см. в статьях в документе <xref:security/blazor/webassembly/index>.</span><span class="sxs-lookup"><span data-stu-id="0c87c-242">For more information, see the articles under <xref:security/blazor/webassembly/index>.</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="0c87c-243">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="0c87c-243">Troubleshoot errors</span></span>

<span data-ttu-id="0c87c-244">Распространенные ошибки</span><span class="sxs-lookup"><span data-stu-id="0c87c-244">Common errors:</span></span>

* <span data-ttu-id="0c87c-245">**Для авторизации требуется каскадный параметр с типом \<AuthenticationState>. Попробуйте предоставить его с помощью CascadingAuthenticationState.**</span><span class="sxs-lookup"><span data-stu-id="0c87c-245">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="0c87c-246">**Для `authenticationStateTask` возвращается значение `null`**</span><span class="sxs-lookup"><span data-stu-id="0c87c-246">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="0c87c-247">Вполне вероятно, что проект не был создан на основе шаблона серверного приложения Blazor с включенной проверкой подлинности.</span><span class="sxs-lookup"><span data-stu-id="0c87c-247">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="0c87c-248">Примените `<CascadingAuthenticationState>` в качестве оболочки некоторой части дерева пользовательского интерфейса, например в *App.razor*, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0c87c-248">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="0c87c-249">`CascadingAuthenticationState` предоставляет каскадный параметр `Task<AuthenticationState>`, который он в свою очередь получает из базовой службы внедрения зависимостей `AuthenticationStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="0c87c-249">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0c87c-250">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0c87c-250">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
* <span data-ttu-id="0c87c-251">[Awesome Blazor: проверка подлинности](https://github.com/AdrienTorris/awesome-blazor#authentication) — ссылки на примеры от сообщества</span><span class="sxs-lookup"><span data-stu-id="0c87c-251">[Awesome Blazor: Authentication](https://github.com/AdrienTorris/awesome-blazor#authentication) community sample links</span></span>
