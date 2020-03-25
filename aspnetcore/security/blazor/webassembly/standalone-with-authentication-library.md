---
title: Защита ASP.NET Core автономного приложения Blazor сборки с помощью библиотеки проверки подлинности
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: ea50d94835b044f9c3d6a0561868f081d32cb62a
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219013"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a><span data-ttu-id="921c7-102">Защита ASP.NET Core автономного приложения Blazor сборки с помощью библиотеки проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="921c7-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with the Authentication library</span></span>

<span data-ttu-id="921c7-103">[Хавьер Калварро Воронков](https://github.com/javiercn) и [Люк ЛаСаМ](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="921c7-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="921c7-104">*Для Azure Active Directory (AAD) и Azure Active Directory B2C (AAD B2C) не следуйте указаниям в этом разделе. См. разделы, посвященные AAD и AAD B2C, в этом узле оглавления.*</span><span class="sxs-lookup"><span data-stu-id="921c7-104">*For Azure Active Directory (AAD) and Azure Active Directory B2C (AAD B2C), don't follow the guidance in this topic. See the AAD and AAD B2C topics in this table of contents node.*</span></span>

<span data-ttu-id="921c7-105">Чтобы создать изолированное приложение Blazorной сборки, использующее библиотеку `Microsoft.AspNetCore.Components.WebAssembly.Authentication`, выполните в командной оболочке следующую команду:</span><span class="sxs-lookup"><span data-stu-id="921c7-105">To create a Blazor WebAssembly standalone app that uses `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library, execute the following command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual
```

<span data-ttu-id="921c7-106">Чтобы указать расположение выходных данных, которое создает папку проекта, если она не существует, включите параметр OUTPUT в команду с путем (например, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="921c7-106">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="921c7-107">Имя папки также станет частью имени проекта.</span><span class="sxs-lookup"><span data-stu-id="921c7-107">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="921c7-108">В Visual Studio [создайте Blazor приложение сборки](xref:blazor/get-started).</span><span class="sxs-lookup"><span data-stu-id="921c7-108">In Visual Studio, [create a Blazor WebAssembly app](xref:blazor/get-started).</span></span> <span data-ttu-id="921c7-109">Настройте **проверку подлинности** для **отдельных учетных записей пользователей** с помощью параметра **сохранить учетные записи пользователей в приложении** .</span><span class="sxs-lookup"><span data-stu-id="921c7-109">Set **Authentication** to **Individual User Accounts** with the **Store user accounts in-app** option.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="921c7-110">Пакет проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="921c7-110">Authentication package</span></span>

<span data-ttu-id="921c7-111">Когда приложение создается для использования отдельных учетных записей пользователей, приложение автоматически получает ссылку на пакет для `Microsoft.AspNetCore.Components.WebAssembly.Authentication` пакета в файле проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="921c7-111">When an app is created to use Individual User Accounts, the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="921c7-112">Пакет предоставляет набор примитивов, которые помогают приложению проверять подлинность пользователей и получать маркеры для вызова защищенных интерфейсов API.</span><span class="sxs-lookup"><span data-stu-id="921c7-112">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="921c7-113">При добавлении проверки подлинности в приложение вручную добавьте пакет в файл проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="921c7-113">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="921c7-114">Замените `{VERSION}` в предыдущей ссылке на пакет версией пакета `Microsoft.AspNetCore.Blazor.Templates`, показанного в статье <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="921c7-114">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="921c7-115">Поддержка службы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="921c7-115">Authentication service support</span></span>

<span data-ttu-id="921c7-116">Поддержка проверки подлинности пользователей регистрируется в контейнере службы с помощью метода расширения `AddOidcAuthentication`, предоставленного пакетом `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="921c7-116">Support for authenticating users is registered in the service container with the `AddOidcAuthentication` extension method provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="921c7-117">Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком удостоверений (IP).</span><span class="sxs-lookup"><span data-stu-id="921c7-117">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="921c7-118">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="921c7-118">*Program.cs*:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="921c7-119">Поддержка проверки подлинности для автономных приложений предлагается с помощью Open ID Connect (OIDC).</span><span class="sxs-lookup"><span data-stu-id="921c7-119">Authentication support for standalone apps is offered using Open ID Connect (OIDC).</span></span> <span data-ttu-id="921c7-120">Метод `AddOidcAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения с помощью OIDC.</span><span class="sxs-lookup"><span data-stu-id="921c7-120">The `AddOidcAuthentication` method accepts a callback to configure the parameters required to authenticate an app using OIDC.</span></span> <span data-ttu-id="921c7-121">Значения, необходимые для настройки приложения, можно получить из IP-адреса, совместимого с OIDC.</span><span class="sxs-lookup"><span data-stu-id="921c7-121">The values required for configuring the app can be obtained from the OIDC-compliant IP.</span></span> <span data-ttu-id="921c7-122">Получите значения при регистрации приложения, которое обычно происходит на веб-портале.</span><span class="sxs-lookup"><span data-stu-id="921c7-122">Obtain the values when you register the app, which typically occurs in their online portal.</span></span>

## <a name="index-page"></a><span data-ttu-id="921c7-123">Страница индексации</span><span class="sxs-lookup"><span data-stu-id="921c7-123">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

## <a name="app-component"></a><span data-ttu-id="921c7-124">Компонент приложения</span><span class="sxs-lookup"><span data-stu-id="921c7-124">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="921c7-125">Компонент Редиректтологин</span><span class="sxs-lookup"><span data-stu-id="921c7-125">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="921c7-126">Компонент Логиндисплай</span><span class="sxs-lookup"><span data-stu-id="921c7-126">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="921c7-127">Компонент проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="921c7-127">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
