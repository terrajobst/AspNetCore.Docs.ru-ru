---
title: Защита ASP.NET Core автономного приложения Blazor сборки с помощью учетных записей Майкрософт
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: 6883af3486256e7c6905626d8da09e8ae0c4a896
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083653"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a><span data-ttu-id="474f0-102">Защита ASP.NET Core автономного приложения Blazor сборки с помощью учетных записей Майкрософт</span><span class="sxs-lookup"><span data-stu-id="474f0-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Microsoft Accounts</span></span>

<span data-ttu-id="474f0-103">[Хавьер Калварро Воронков](https://github.com/javiercn) и [Люк ЛаСаМ](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="474f0-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="474f0-104">Чтобы создать изолированное приложение Blazor сборки, использующее [учетные записи Майкрософт с Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="474f0-104">To create a Blazor WebAssembly standalone app that uses [Microsoft Accounts with Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) for authentication:</span></span>

1. [<span data-ttu-id="474f0-105">Создание клиента AAD и веб-приложения</span><span class="sxs-lookup"><span data-stu-id="474f0-105">Create an AAD tenant and web application</span></span>](/azure/active-directory/develop/v2-overview)

   <span data-ttu-id="474f0-106">Зарегистрируйте приложение AAD в области **Azure Active Directory** > **Регистрация приложений** портал Azure:</span><span class="sxs-lookup"><span data-stu-id="474f0-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

   <span data-ttu-id="474f0-107">1 \.</span><span class="sxs-lookup"><span data-stu-id="474f0-107">1\.</span></span> <span data-ttu-id="474f0-108">Укажите **имя** приложения (например, **Blazor клиента AAD**).</span><span class="sxs-lookup"><span data-stu-id="474f0-108">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span><br>
   <span data-ttu-id="474f0-109">2 \.</span><span class="sxs-lookup"><span data-stu-id="474f0-109">2\.</span></span> <span data-ttu-id="474f0-110">В списке **Поддерживаемые типы учетных записей**выберите **учетные записи в любом организационном каталоге**.</span><span class="sxs-lookup"><span data-stu-id="474f0-110">In **Supported account types**, select **Accounts in any organizational directory**.</span></span><br>
   <span data-ttu-id="474f0-111">3 \.</span><span class="sxs-lookup"><span data-stu-id="474f0-111">3\.</span></span> <span data-ttu-id="474f0-112">Оставьте в раскрывающемся списке **URI перенаправления** значение **веб**и укажите универсальный код ресурса (uri) перенаправления `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="474f0-112">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span><br>
   <span data-ttu-id="474f0-113">4 \.</span><span class="sxs-lookup"><span data-stu-id="474f0-113">4\.</span></span> <span data-ttu-id="474f0-114">Отключите **разрешения** > установите флажок **предоставить доступ к администратору для OpenID Connect и offline_access разрешений** .</span><span class="sxs-lookup"><span data-stu-id="474f0-114">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span><br>
   <span data-ttu-id="474f0-115">5 \.</span><span class="sxs-lookup"><span data-stu-id="474f0-115">5\.</span></span> <span data-ttu-id="474f0-116">Выберите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="474f0-116">Select **Register**.</span></span>

   <span data-ttu-id="474f0-117">При **проверке Подлинности** > **конфигурации платформы** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="474f0-117">In **Authentication** > **Platform configurations** > **Web**:</span></span>

   <span data-ttu-id="474f0-118">1 \.</span><span class="sxs-lookup"><span data-stu-id="474f0-118">1\.</span></span> <span data-ttu-id="474f0-119">Убедитесь, что **URI перенаправления** для `https://localhost:5001/authentication/login-callback` имеется.</span><span class="sxs-lookup"><span data-stu-id="474f0-119">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span><br>
   <span data-ttu-id="474f0-120">2 \.</span><span class="sxs-lookup"><span data-stu-id="474f0-120">2\.</span></span> <span data-ttu-id="474f0-121">Для **неявного предоставления**установите флажки для **маркеров доступа** и **маркеров идентификации**.</span><span class="sxs-lookup"><span data-stu-id="474f0-121">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span><br>
   <span data-ttu-id="474f0-122">3 \.</span><span class="sxs-lookup"><span data-stu-id="474f0-122">3\.</span></span> <span data-ttu-id="474f0-123">Остальные значения по умолчанию для приложения приемлемы для этого интерфейса.</span><span class="sxs-lookup"><span data-stu-id="474f0-123">The remaining defaults for the app are acceptable for this experience.</span></span><br>
   <span data-ttu-id="474f0-124">4 \.</span><span class="sxs-lookup"><span data-stu-id="474f0-124">4\.</span></span> <span data-ttu-id="474f0-125">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="474f0-125">Select the **Save** button.</span></span>

   <span data-ttu-id="474f0-126">Запишите идентификатор приложения (идентификатор клиента) (например, `11111111-1111-1111-1111-111111111111`).</span><span class="sxs-lookup"><span data-stu-id="474f0-126">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

1. <span data-ttu-id="474f0-127">Замените заполнители в следующей команде на записанные ранее сведения и выполните команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="474f0-127">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   <span data-ttu-id="474f0-128">Чтобы указать расположение выходных данных, которое создает папку проекта, если она не существует, включите параметр OUTPUT в команду с путем (например, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="474f0-128">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="474f0-129">Имя папки также станет частью имени проекта.</span><span class="sxs-lookup"><span data-stu-id="474f0-129">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="474f0-130">После создания приложения вы сможете:</span><span class="sxs-lookup"><span data-stu-id="474f0-130">After creating the app, you should be able to:</span></span>

* <span data-ttu-id="474f0-131">Войдите в приложение, используя учетную запись Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="474f0-131">Log into the app using a Microsoft Account.</span></span>
* <span data-ttu-id="474f0-132">Запросите маркеры доступа для API-интерфейсов Майкрософт, используя тот же подход, что и для автономных Blazorных приложений, при условии, что приложение настроено правильно.</span><span class="sxs-lookup"><span data-stu-id="474f0-132">Request access tokens for Microsoft APIs using the same approach as for standalone Blazor apps provided that you have configured the app correctly.</span></span> <span data-ttu-id="474f0-133">Дополнительные сведения см. [в разделе Краткое руководство. Настройка приложения для предоставления веб-API](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span><span class="sxs-lookup"><span data-stu-id="474f0-133">For more information, see [Quickstart: Configure an application to expose web APIs](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span></span>

## <a name="authentication-package"></a><span data-ttu-id="474f0-134">Пакет проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="474f0-134">Authentication package</span></span>

<span data-ttu-id="474f0-135">Когда приложение создается для использования рабочих или учебных учетных записей (`SingleOrg`), приложение автоматически получает ссылку на пакет для [библиотеки проверки подлинности Майкрософт](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="474f0-135">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="474f0-136">Пакет предоставляет набор примитивов, которые помогают приложению проверять подлинность пользователей и получать маркеры для вызова защищенных интерфейсов API.</span><span class="sxs-lookup"><span data-stu-id="474f0-136">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="474f0-137">При добавлении проверки подлинности в приложение вручную добавьте пакет в файл проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="474f0-137">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="474f0-138">Замените `{VERSION}` в предыдущей ссылке на пакет версией пакета `Microsoft.AspNetCore.Blazor.Templates`, показанного в статье <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="474f0-138">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="474f0-139">Пакет `Microsoft.Authentication.WebAssembly.Msal` транзитивно добавляет пакет `Microsoft.AspNetCore.Components.WebAssembly.Authentication` в приложение.</span><span class="sxs-lookup"><span data-stu-id="474f0-139">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="474f0-140">Поддержка службы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="474f0-140">Authentication service support</span></span>

<span data-ttu-id="474f0-141">Поддержка проверки подлинности пользователей регистрируется в контейнере службы с помощью метода расширения `AddMsalAuthentication`, предоставленного пакетом `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="474f0-141">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="474f0-142">Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком удостоверений (IP).</span><span class="sxs-lookup"><span data-stu-id="474f0-142">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="474f0-143">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="474f0-143">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="474f0-144">Метод `AddMsalAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения.</span><span class="sxs-lookup"><span data-stu-id="474f0-144">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="474f0-145">Значения, необходимые для настройки приложения, можно получить из конфигурации учетных записей Майкрософт при регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="474f0-145">The values required for configuring the app can be obtained from the Microsoft Accounts configuration when you register the app.</span></span>

## <a name="index-page"></a><span data-ttu-id="474f0-146">Главная страница</span><span class="sxs-lookup"><span data-stu-id="474f0-146">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a><span data-ttu-id="474f0-147">Компонент приложения</span><span class="sxs-lookup"><span data-stu-id="474f0-147">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="474f0-148">Компонент Редиректтологин</span><span class="sxs-lookup"><span data-stu-id="474f0-148">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="474f0-149">Компонент Логиндисплай</span><span class="sxs-lookup"><span data-stu-id="474f0-149">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="474f0-150">Компонент проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="474f0-150">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="474f0-151">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="474f0-151">Additional resources</span></span>

* [<span data-ttu-id="474f0-152">Краткое руководство. Регистрация приложения на платформе Microsoft Identity</span><span class="sxs-lookup"><span data-stu-id="474f0-152">Quickstart: Register an application with the Microsoft identity platform</span></span>](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [<span data-ttu-id="474f0-153">Краткое руководство. Настройка приложения для предоставления веб-API</span><span class="sxs-lookup"><span data-stu-id="474f0-153">Quickstart: Configure an application to expose web APIs</span></span>](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
