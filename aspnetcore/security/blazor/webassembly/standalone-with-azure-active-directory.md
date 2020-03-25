---
title: Защита ASP.NET Core автономного приложения Blazor сборки с помощью Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: e12c38ed42a4e2714d785ef8f03097246c40d36e
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218987"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a><span data-ttu-id="d029a-102">Защита ASP.NET Core автономного приложения Blazor сборки с помощью Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d029a-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory</span></span>

<span data-ttu-id="d029a-103">[Хавьер Калварро Воронков](https://github.com/javiercn) и [Люк ЛаСаМ](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d029a-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="d029a-104">Чтобы создать изолированное приложение Blazor сборки, использующее [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="d029a-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication:</span></span>

<span data-ttu-id="d029a-105">[Создание клиента AAD и веб-приложения](/azure/active-directory/develop/v2-overview):</span><span class="sxs-lookup"><span data-stu-id="d029a-105">[Create an AAD tenant and web application](/azure/active-directory/develop/v2-overview):</span></span>

<span data-ttu-id="d029a-106">Зарегистрируйте приложение AAD в области **Azure Active Directory** > **Регистрация приложений** портал Azure:</span><span class="sxs-lookup"><span data-stu-id="d029a-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="d029a-107">Укажите **имя** приложения (например, **Blazor клиента AAD**).</span><span class="sxs-lookup"><span data-stu-id="d029a-107">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="d029a-108">Выберите **Поддерживаемые типы учетных записей**.</span><span class="sxs-lookup"><span data-stu-id="d029a-108">Choose a **Supported account types**.</span></span> <span data-ttu-id="d029a-109">Вы можете выбрать **учетные записи в этом каталоге организации только** для этого интерфейса.</span><span class="sxs-lookup"><span data-stu-id="d029a-109">You may select **Accounts in this organizational directory only** for this experience.</span></span>
1. <span data-ttu-id="d029a-110">Оставьте в раскрывающемся списке **URI перенаправления** значение **веб**и укажите универсальный код ресурса (uri) перенаправления `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="d029a-110">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="d029a-111">Отключите **разрешения** > установите флажок **предоставить доступ к администратору для OpenID Connect и offline_access разрешений** .</span><span class="sxs-lookup"><span data-stu-id="d029a-111">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="d029a-112">Выберите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="d029a-112">Select **Register**.</span></span>

<span data-ttu-id="d029a-113">При **проверке Подлинности** > **конфигурации платформы** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="d029a-113">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="d029a-114">Убедитесь, что **URI перенаправления** для `https://localhost:5001/authentication/login-callback` имеется.</span><span class="sxs-lookup"><span data-stu-id="d029a-114">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="d029a-115">Для **неявного предоставления**установите флажки для **маркеров доступа** и **маркеров идентификации**.</span><span class="sxs-lookup"><span data-stu-id="d029a-115">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="d029a-116">Остальные значения по умолчанию для приложения приемлемы для этого интерфейса.</span><span class="sxs-lookup"><span data-stu-id="d029a-116">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="d029a-117">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="d029a-117">Select the **Save** button.</span></span>

<span data-ttu-id="d029a-118">Запишите следующие сведения:</span><span class="sxs-lookup"><span data-stu-id="d029a-118">Record the following information:</span></span>

* <span data-ttu-id="d029a-119">Идентификатор приложения (идентификатор клиента) (например, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="d029a-119">Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="d029a-120">Идентификатор каталога (идентификатор клиента) (например, `22222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="d029a-120">Directory ID (Tenant ID) (for example, `22222222-2222-2222-2222-222222222222`)</span></span>

<span data-ttu-id="d029a-121">Замените заполнители в следующей команде на записанные ранее сведения и выполните команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="d029a-121">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="d029a-122">Чтобы указать расположение выходных данных, которое создает папку проекта, если она не существует, включите параметр OUTPUT в команду с путем (например, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="d029a-122">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="d029a-123">Имя папки также станет частью имени проекта.</span><span class="sxs-lookup"><span data-stu-id="d029a-123">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="d029a-124">Пакет проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="d029a-124">Authentication package</span></span>

<span data-ttu-id="d029a-125">Когда приложение создается для использования рабочих или учебных учетных записей (`SingleOrg`), приложение автоматически получает ссылку на пакет для [библиотеки проверки подлинности Майкрософт](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="d029a-125">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="d029a-126">Пакет предоставляет набор примитивов, которые помогают приложению проверять подлинность пользователей и получать маркеры для вызова защищенных интерфейсов API.</span><span class="sxs-lookup"><span data-stu-id="d029a-126">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="d029a-127">При добавлении проверки подлинности в приложение вручную добавьте пакет в файл проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="d029a-127">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="d029a-128">Замените `{VERSION}` в предыдущей ссылке на пакет версией пакета `Microsoft.AspNetCore.Blazor.Templates`, показанного в статье <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="d029a-128">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="d029a-129">Пакет `Microsoft.Authentication.WebAssembly.Msal` транзитивно добавляет пакет `Microsoft.AspNetCore.Components.WebAssembly.Authentication` в приложение.</span><span class="sxs-lookup"><span data-stu-id="d029a-129">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="d029a-130">Поддержка службы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="d029a-130">Authentication service support</span></span>

<span data-ttu-id="d029a-131">Поддержка проверки подлинности пользователей регистрируется в контейнере службы с помощью метода расширения `AddMsalAuthentication`, предоставленного пакетом `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="d029a-131">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="d029a-132">Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком удостоверений (IP).</span><span class="sxs-lookup"><span data-stu-id="d029a-132">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="d029a-133">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d029a-133">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="d029a-134">Метод `AddMsalAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения.</span><span class="sxs-lookup"><span data-stu-id="d029a-134">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="d029a-135">Значения, необходимые для настройки приложения, можно получить из конфигурации AAD на портале Azure при регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="d029a-135">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="d029a-136">Шаблон Blazor-сборки не автоматически настраивает приложение для запроса маркера доступа для безопасного API.</span><span class="sxs-lookup"><span data-stu-id="d029a-136">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="d029a-137">Чтобы создать маркер в рамках потока входа, добавьте область в область маркера доступа по умолчанию `MsalProviderOptions`:</span><span class="sxs-lookup"><span data-stu-id="d029a-137">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> <span data-ttu-id="d029a-138">Область маркера доступа по умолчанию должна иметь формат `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (например, `11111111-1111-1111-1111-111111111111/API.Access`).</span><span class="sxs-lookup"><span data-stu-id="d029a-138">The default access token scope must be in the format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (for example, `11111111-1111-1111-1111-111111111111/API.Access`).</span></span> <span data-ttu-id="d029a-139">Если для параметра области (как показано на портале Azure) предоставлена схема или схема и узел, то *клиентское приложение* создает необработанное исключение при получении от *приложения API сервера*ответа *401 с несанкционированным* доступом.</span><span class="sxs-lookup"><span data-stu-id="d029a-139">If a scheme or scheme and host is provided to the scope setting (as shown in the Azure Portal), the *Client app* throws an unhandled exception when it receives a *401 Unauthorized* response from the *Server API app*.</span></span>

## <a name="index-page"></a><span data-ttu-id="d029a-140">Страница индексации</span><span class="sxs-lookup"><span data-stu-id="d029a-140">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="d029a-141">Компонент приложения</span><span class="sxs-lookup"><span data-stu-id="d029a-141">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="d029a-142">Компонент Редиректтологин</span><span class="sxs-lookup"><span data-stu-id="d029a-142">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="d029a-143">Компонент Логиндисплай</span><span class="sxs-lookup"><span data-stu-id="d029a-143">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="d029a-144">Компонент проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="d029a-144">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="d029a-145">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d029a-145">Additional resources</span></span>

* <xref:security/authentication/azure-active-directory/index>
