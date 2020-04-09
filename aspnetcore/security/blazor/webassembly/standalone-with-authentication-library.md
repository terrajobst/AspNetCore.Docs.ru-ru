---
title: Защищайте Blazor автономное приложение ASP.NET Core WebAssembly с библиотекой аутентификации
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: 893fff10df37e1c2be549604f4cb83cd20049108
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977045"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a><span data-ttu-id="27851-102">Защищайте Blazor автономное приложение ASP.NET Core WebAssembly с библиотекой аутентификации</span><span class="sxs-lookup"><span data-stu-id="27851-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with the Authentication library</span></span>

<span data-ttu-id="27851-103">[Хавьер Кальварро Нельсон](https://github.com/javiercn) и Люк [Лэйтам](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="27851-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="27851-104">*Для Active Directory Azure (AAD) и Azure Active Directory B2C (AAD B2C) не следуйте указаниям в этой теме. В этой таблице узлов содержимого смотрите темы AAD и AAD B2C.*</span><span class="sxs-lookup"><span data-stu-id="27851-104">*For Azure Active Directory (AAD) and Azure Active Directory B2C (AAD B2C), don't follow the guidance in this topic. See the AAD and AAD B2C topics in this table of contents node.*</span></span>

<span data-ttu-id="27851-105">Чтобы создать Blazor автономное приложение WebAssembly, используюваее `Microsoft.AspNetCore.Components.WebAssembly.Authentication` библиотеку, выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="27851-105">To create a Blazor WebAssembly standalone app that uses `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library, execute the following command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual
```

<span data-ttu-id="27851-106">Чтобы указать местона вывода, которое создает папку проекта, если она не существует, включите опцию вывода в команду с путем (например, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="27851-106">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="27851-107">Имя папки также становится частью названия проекта.</span><span class="sxs-lookup"><span data-stu-id="27851-107">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="27851-108">В Visual Studio [создайте приложение Blazor WebAssembly](xref:blazor/get-started).</span><span class="sxs-lookup"><span data-stu-id="27851-108">In Visual Studio, [create a Blazor WebAssembly app](xref:blazor/get-started).</span></span> <span data-ttu-id="27851-109">Установите **аутентификацию** для **индивидуальных учетных записей пользователей** с помощью опции приложения **для пользователей Магазина.**</span><span class="sxs-lookup"><span data-stu-id="27851-109">Set **Authentication** to **Individual User Accounts** with the **Store user accounts in-app** option.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="27851-110">Пакет аутентификации</span><span class="sxs-lookup"><span data-stu-id="27851-110">Authentication package</span></span>

<span data-ttu-id="27851-111">Когда приложение создается для использования индивидуальных учетных записей `Microsoft.AspNetCore.Components.WebAssembly.Authentication` пользователей, приложение автоматически получает ссылку на пакет для пакета в файле проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="27851-111">When an app is created to use Individual User Accounts, the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="27851-112">Пакет предоставляет набор примитивов, которые помогают приложению аутентифицировать пользователей и получать токены для вызова защищенных AIS.</span><span class="sxs-lookup"><span data-stu-id="27851-112">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="27851-113">При добавлении аутентификации в приложение вручную добавьте пакет в файл проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="27851-113">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="27851-114">Замените `{VERSION}` в предыдущем пакете ссылку на версию `Microsoft.AspNetCore.Blazor.Templates` пакета, показанную <xref:blazor/get-started> в статье.</span><span class="sxs-lookup"><span data-stu-id="27851-114">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="27851-115">Поддержка службы аутентификации</span><span class="sxs-lookup"><span data-stu-id="27851-115">Authentication service support</span></span>

<span data-ttu-id="27851-116">Поддержка аутентификации пользователей регистрируется `AddOidcAuthentication` в сервисном `Microsoft.AspNetCore.Components.WebAssembly.Authentication` контейнере с методом расширения, предусмотренным пакетом.</span><span class="sxs-lookup"><span data-stu-id="27851-116">Support for authenticating users is registered in the service container with the `AddOidcAuthentication` extension method provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="27851-117">Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком идентификационных данных (IP).</span><span class="sxs-lookup"><span data-stu-id="27851-117">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="27851-118">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="27851-118">*Program.cs*:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="27851-119">Поддержка аутентификации автономных приложений предлагается с помощью Open ID Connect (OIDC).</span><span class="sxs-lookup"><span data-stu-id="27851-119">Authentication support for standalone apps is offered using Open ID Connect (OIDC).</span></span> <span data-ttu-id="27851-120">Метод `AddOidcAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения с помощью OIDC.</span><span class="sxs-lookup"><span data-stu-id="27851-120">The `AddOidcAuthentication` method accepts a callback to configure the parameters required to authenticate an app using OIDC.</span></span> <span data-ttu-id="27851-121">Значения, необходимые для настройки приложения, могут быть получены с IP-адреса, совместимым с OIDC.</span><span class="sxs-lookup"><span data-stu-id="27851-121">The values required for configuring the app can be obtained from the OIDC-compliant IP.</span></span> <span data-ttu-id="27851-122">Получить значения при регистрации приложения, которое обычно происходит в их интернет-портале.</span><span class="sxs-lookup"><span data-stu-id="27851-122">Obtain the values when you register the app, which typically occurs in their online portal.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="27851-123">Области маркеров доступа</span><span class="sxs-lookup"><span data-stu-id="27851-123">Access token scopes</span></span>

<span data-ttu-id="27851-124">Шаблон Blazor WebAssembly не настраивает приложение автоматически, чтобы запросить токен доступа для защищенного API.</span><span class="sxs-lookup"><span data-stu-id="27851-124">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="27851-125">Чтобы предоставить токен как часть потока ввоза, добавьте область к `OidcProviderOptions`примку токенов по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="27851-125">To provision a token as part of the sign-in flow, add the scope to the default token scopes of the `OidcProviderOptions`:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="27851-126">Если портал Azure предоставляет область URI и **приложение выбрасывает необработанное исключение,** когда получает *401 несанкционированный* ответ от API, попробуйте использовать область URI, которая не включает схему и узел.</span><span class="sxs-lookup"><span data-stu-id="27851-126">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="27851-127">Например, портал Azure может предоставить один из следующих форматов URI:</span><span class="sxs-lookup"><span data-stu-id="27851-127">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="27851-128">Поставка области URI без схемы и хозяина:</span><span class="sxs-lookup"><span data-stu-id="27851-128">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="27851-129">Для получения дополнительной информации см. <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span><span class="sxs-lookup"><span data-stu-id="27851-129">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

## <a name="imports-file"></a><span data-ttu-id="27851-130">Файл импорта</span><span class="sxs-lookup"><span data-stu-id="27851-130">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="27851-131">Страница индексации</span><span class="sxs-lookup"><span data-stu-id="27851-131">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

## <a name="app-component"></a><span data-ttu-id="27851-132">Компонент приложения</span><span class="sxs-lookup"><span data-stu-id="27851-132">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="27851-133">ПеренаправлениеКомпонентToLogin</span><span class="sxs-lookup"><span data-stu-id="27851-133">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="27851-134">Компонент LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="27851-134">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="27851-135">Компонент аутентификации</span><span class="sxs-lookup"><span data-stu-id="27851-135">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="27851-136">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="27851-136">Additional resources</span></span>

* [<span data-ttu-id="27851-137">Запрос дополнительных токенов доступа</span><span class="sxs-lookup"><span data-stu-id="27851-137">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
