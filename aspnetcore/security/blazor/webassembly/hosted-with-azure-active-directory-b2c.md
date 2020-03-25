---
title: Обеспечение безопасности размещенного в ASP.NET Core Blazor приложения сборки Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/22/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory-b2c
ms.openlocfilehash: 0083f179f85371d4751fb179194417681fc1a01d
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219068"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory-b2c"></a><span data-ttu-id="9d09f-102">Обеспечение безопасности размещенного в ASP.NET Core Blazor приложения сборки Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="9d09f-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory B2C</span></span>

<span data-ttu-id="9d09f-103">[Хавьер Калварро Воронков](https://github.com/javiercn) и [Люк ЛаСаМ](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9d09f-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="9d09f-104">В этой статье описывается, как создать изолированное приложение Blazor сборки, использующее [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="9d09f-104">This article describes how to create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="9d09f-105">Регистрация приложений в AAD B2C и создание решения</span><span class="sxs-lookup"><span data-stu-id="9d09f-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="9d09f-106">Создание клиента</span><span class="sxs-lookup"><span data-stu-id="9d09f-106">Create a tenant</span></span>

<span data-ttu-id="9d09f-107">Следуйте указаниям в [руководстве по созданию клиента Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-create-tenant) для создания клиента AAD B2C и записи следующих сведений.</span><span class="sxs-lookup"><span data-stu-id="9d09f-107">Follow the guidance in [Tutorial: Create an Azure Active Directory B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) to create an AAD B2C tenant and record the following information:</span></span>

* <span data-ttu-id="9d09f-108">AAD B2C экземпляр (например, `https://contoso.b2clogin.com/`, который включает замыкающую косую черту)</span><span class="sxs-lookup"><span data-stu-id="9d09f-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span>
* <span data-ttu-id="9d09f-109">Домен клиента AAD B2C (например, `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="9d09f-109">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="9d09f-110">Регистрация приложения API сервера</span><span class="sxs-lookup"><span data-stu-id="9d09f-110">Register a server API app</span></span>

<span data-ttu-id="9d09f-111">Следуйте указаниям в [руководстве по регистрации приложения в Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) , чтобы зарегистрировать приложение AAD для *приложения API сервера* в области **Azure Active Directory** > **Регистрация приложений** портал Azure:</span><span class="sxs-lookup"><span data-stu-id="9d09f-111">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="9d09f-112">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="9d09f-112">Select **New registration**.</span></span>
1. <span data-ttu-id="9d09f-113">Укажите **имя** приложения (например, **Blazor Server AAD B2C**).</span><span class="sxs-lookup"><span data-stu-id="9d09f-113">Provide a **Name** for the app (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="9d09f-114">Для **поддерживаемых типов учетных записей**выберите **учетные записи в любом организационном каталоге или любом поставщике удостоверений. Для проверки подлинности пользователей с помощью Azure AD B2C.**</span><span class="sxs-lookup"><span data-stu-id="9d09f-114">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="9d09f-115">(несколько клиентов) для этого интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9d09f-115">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="9d09f-116">В этом сценарии *приложению API сервера* не требуется **универсальный код ресурса (URI) перенаправления** , поэтому оставьте в раскрывающемся списке значение **Web** и не вводите URI перенаправления.</span><span class="sxs-lookup"><span data-stu-id="9d09f-116">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="9d09f-117">Убедитесь, что **разрешения** > **предоставить доступ администратора к OpenID Connect и offline_access разрешения** включены.</span><span class="sxs-lookup"><span data-stu-id="9d09f-117">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="9d09f-118">Выберите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="9d09f-118">Select **Register**.</span></span>

<span data-ttu-id="9d09f-119">В **предоставление API**:</span><span class="sxs-lookup"><span data-stu-id="9d09f-119">In **Expose an API**:</span></span>

1. <span data-ttu-id="9d09f-120">Нажмите **Добавить группу**.</span><span class="sxs-lookup"><span data-stu-id="9d09f-120">Select **Add a scope**.</span></span>
1. <span data-ttu-id="9d09f-121">Выберите **Сохранить и продолжить**.</span><span class="sxs-lookup"><span data-stu-id="9d09f-121">Select **Save and continue**.</span></span>
1. <span data-ttu-id="9d09f-122">Укажите **имя области** (например, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="9d09f-122">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="9d09f-123">Укажите **Отображаемое имя согласия администратора** (например, `Access API`).</span><span class="sxs-lookup"><span data-stu-id="9d09f-123">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="9d09f-124">Введите **Описание согласия администратора** (например, `Allows the app to access server app API endpoints.`).</span><span class="sxs-lookup"><span data-stu-id="9d09f-124">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="9d09f-125">Убедитесь, что для **состояния** задано значение **включено**.</span><span class="sxs-lookup"><span data-stu-id="9d09f-125">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="9d09f-126">Выберите **Добавить область**.</span><span class="sxs-lookup"><span data-stu-id="9d09f-126">Select **Add scope**.</span></span>

<span data-ttu-id="9d09f-127">Запишите следующие сведения:</span><span class="sxs-lookup"><span data-stu-id="9d09f-127">Record the following information:</span></span>

* <span data-ttu-id="9d09f-128">*Приложение API сервера* Идентификатор приложения (идентификатор клиента) (например, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="9d09f-128">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="9d09f-129">Идентификатор каталога (идентификатор клиента) (например, `222222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="9d09f-129">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="9d09f-130">*Приложение API сервера* URI идентификатора приложения (например, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`портал Azure может по умолчанию присвоить значение ИДЕНТИФИКАТОРу клиента).</span><span class="sxs-lookup"><span data-stu-id="9d09f-130">*Server API app* App ID URI (for example, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, the Azure portal might default the value to the Client ID)</span></span>
* <span data-ttu-id="9d09f-131">Область по умолчанию (например, `API.Access`)</span><span class="sxs-lookup"><span data-stu-id="9d09f-131">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="9d09f-132">Регистрация клиентского приложения</span><span class="sxs-lookup"><span data-stu-id="9d09f-132">Register a client app</span></span>

<span data-ttu-id="9d09f-133">Следуйте указаниям в [руководстве по регистрации приложения в Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) еще раз, чтобы зарегистрировать приложение AAD для *клиентского приложения* в области **Azure Active Directory** > **Регистрация приложений** портал Azure.</span><span class="sxs-lookup"><span data-stu-id="9d09f-133">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) again to register an AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="9d09f-134">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="9d09f-134">Select **New registration**.</span></span>
1. <span data-ttu-id="9d09f-135">Укажите **имя** приложения (например, **Blazor клиент AAD B2C**).</span><span class="sxs-lookup"><span data-stu-id="9d09f-135">Provide a **Name** for the app (for example, **Blazor Client AAD B2C**).</span></span>
1. <span data-ttu-id="9d09f-136">Для **поддерживаемых типов учетных записей**выберите **учетные записи в любом организационном каталоге или любом поставщике удостоверений. Для проверки подлинности пользователей с помощью Azure AD B2C.**</span><span class="sxs-lookup"><span data-stu-id="9d09f-136">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="9d09f-137">(несколько клиентов) для этого интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9d09f-137">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="9d09f-138">Оставьте в раскрывающемся списке **URI перенаправления** значение **веб**и укажите универсальный код ресурса (uri) перенаправления `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="9d09f-138">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="9d09f-139">Убедитесь, что **разрешения** > **предоставить доступ администратора к OpenID Connect и offline_access разрешения** включены.</span><span class="sxs-lookup"><span data-stu-id="9d09f-139">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="9d09f-140">Выберите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="9d09f-140">Select **Register**.</span></span>

<span data-ttu-id="9d09f-141">При **проверке Подлинности** > **конфигурации платформы** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="9d09f-141">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="9d09f-142">Убедитесь, что **URI перенаправления** для `https://localhost:5001/authentication/login-callback` имеется.</span><span class="sxs-lookup"><span data-stu-id="9d09f-142">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="9d09f-143">Для **неявного предоставления**установите флажки для **маркеров доступа** и **маркеров идентификации**.</span><span class="sxs-lookup"><span data-stu-id="9d09f-143">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="9d09f-144">Остальные значения по умолчанию для приложения приемлемы для этого интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9d09f-144">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="9d09f-145">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="9d09f-145">Select the **Save** button.</span></span>

<span data-ttu-id="9d09f-146">В **разрешениях API**:</span><span class="sxs-lookup"><span data-stu-id="9d09f-146">In **API permissions**:</span></span>

1. <span data-ttu-id="9d09f-147">Убедитесь, что приложение имеет **Microsoft Graph** > **пользователь.** разрешение на чтение.</span><span class="sxs-lookup"><span data-stu-id="9d09f-147">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="9d09f-148">Выберите **Добавить разрешение** , а затем — **Мои API**.</span><span class="sxs-lookup"><span data-stu-id="9d09f-148">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="9d09f-149">Выберите *приложение API сервера* из столбца **имя** (например, **Blazor Server AAD B2C**).</span><span class="sxs-lookup"><span data-stu-id="9d09f-149">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="9d09f-150">Откройте список **API** .</span><span class="sxs-lookup"><span data-stu-id="9d09f-150">Open the **API** list.</span></span>
1. <span data-ttu-id="9d09f-151">Разрешение доступа к API (например, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="9d09f-151">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="9d09f-152">Выберите **Добавить разрешения**.</span><span class="sxs-lookup"><span data-stu-id="9d09f-152">Select **Add permissions**.</span></span>
1. <span data-ttu-id="9d09f-153">Нажмите кнопку **предоставить содержимое администратора для {имя клиента}** .</span><span class="sxs-lookup"><span data-stu-id="9d09f-153">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="9d09f-154">Выберите **Да** для подтверждения.</span><span class="sxs-lookup"><span data-stu-id="9d09f-154">Select **Yes** to confirm.</span></span>

<span data-ttu-id="9d09f-155">В **домашней** > **Azure AD B2C** > **потоки пользователей**:</span><span class="sxs-lookup"><span data-stu-id="9d09f-155">In **Home** > **Azure AD B2C** > **User flows**:</span></span>

[<span data-ttu-id="9d09f-156">Создание потока пользователя регистрации и входа в систему</span><span class="sxs-lookup"><span data-stu-id="9d09f-156">Create a sign-up and sign-in user flow</span></span>](/azure/active-directory-b2c/tutorial-create-user-flows)

<span data-ttu-id="9d09f-157">Чтобы заполнить `context.User.Identity.Name` в компоненте `LoginDisplay` (*Shared/логиндисплай. Razor*), как минимум, выберите **Application Claims (заявка на приложение** ) > **Отображаемое имя** пользовательский атрибут.</span><span class="sxs-lookup"><span data-stu-id="9d09f-157">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

<span data-ttu-id="9d09f-158">Запишите следующие сведения:</span><span class="sxs-lookup"><span data-stu-id="9d09f-158">Record the following information:</span></span>

* <span data-ttu-id="9d09f-159">Запишите идентификатор приложения *клиентского приложения* (идентификатор клиента) (например, `33333333-3333-3333-3333-333333333333`).</span><span class="sxs-lookup"><span data-stu-id="9d09f-159">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>
* <span data-ttu-id="9d09f-160">Запишите имя потока пользователя для регистрации и входа, созданное для приложения (например, `B2C_1_signupsignin`).</span><span class="sxs-lookup"><span data-stu-id="9d09f-160">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="9d09f-161">Создайте приложение</span><span class="sxs-lookup"><span data-stu-id="9d09f-161">Create the app</span></span>

<span data-ttu-id="9d09f-162">Замените заполнители в следующей команде на записанные ранее сведения и выполните команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="9d09f-162">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho -ssp "{SIGN UP OR SIGN IN POLICY}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="9d09f-163">Чтобы указать расположение выходных данных, которое создает папку проекта, если она не существует, включите параметр OUTPUT в команду с путем (например, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="9d09f-163">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="9d09f-164">Имя папки также станет частью имени проекта.</span><span class="sxs-lookup"><span data-stu-id="9d09f-164">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="9d09f-165">Конфигурация серверного приложения</span><span class="sxs-lookup"><span data-stu-id="9d09f-165">Server app configuration</span></span>

<span data-ttu-id="9d09f-166">*Этот раздел относится к **серверному** приложению решения.*</span><span class="sxs-lookup"><span data-stu-id="9d09f-166">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="9d09f-167">Пакет проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9d09f-167">Authentication package</span></span>

<span data-ttu-id="9d09f-168">Поддержка проверки подлинности и авторизации вызовов ASP.NET Core веб-интерфейсов API обеспечивается `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span><span class="sxs-lookup"><span data-stu-id="9d09f-168">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="9d09f-169">Поддержка службы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9d09f-169">Authentication service support</span></span>

<span data-ttu-id="9d09f-170">Метод `AddAuthentication` настраивает службы проверки подлинности в приложении и настраивает обработчик носителя JWT в качестве метода проверки подлинности по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9d09f-170">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="9d09f-171">Метод `AddAzureADBearer` настраивает определенные параметры в обработчике носителя JWT, который требуется для проверки маркеров, создаваемых Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="9d09f-171">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="9d09f-172">`UseAuthentication` и `UseAuthorization` убедитесь, что:</span><span class="sxs-lookup"><span data-stu-id="9d09f-172">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="9d09f-173">Приложение пытается проанализировать и проверить маркеры в входящих запросах.</span><span class="sxs-lookup"><span data-stu-id="9d09f-173">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="9d09f-174">Любой запрос, пытающийся получить доступ к защищенному ресурсу без соответствующих учетных данных, завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="9d09f-174">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a><span data-ttu-id="9d09f-175">Параметры приложения</span><span class="sxs-lookup"><span data-stu-id="9d09f-175">App settings</span></span>

<span data-ttu-id="9d09f-176">Файл *appSettings. JSON* содержит параметры для настройки обработчика носителя JWT, используемого для проверки маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="9d09f-176">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://{ORGANIZATION}.b2clogin.com/",
    "ClientId": "{API CLIENT ID}",
    "Domain": "{DOMAIN}",
    "SignUpSignInPolicyId": "{SIGN UP OR SIGN IN POLICY}"
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="9d09f-177">Контроллер Веасерфорекаст</span><span class="sxs-lookup"><span data-stu-id="9d09f-177">WeatherForecast controller</span></span>

<span data-ttu-id="9d09f-178">Контроллер Веасерфорекаст (*Controllers/веасерфорекастконтроллер. CS*) предоставляет защищенный API с атрибутом `[Authorize]`, применяемым к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="9d09f-178">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="9d09f-179">**Важно** понимать, что:</span><span class="sxs-lookup"><span data-stu-id="9d09f-179">It's **important** to understand that:</span></span>

* <span data-ttu-id="9d09f-180">Атрибут `[Authorize]` в этом контроллере API является единственным, который защищает этот API от несанкционированного доступа.</span><span class="sxs-lookup"><span data-stu-id="9d09f-180">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="9d09f-181">Атрибут `[Authorize]`, используемый в приложении Blazor сборки, служит указанием для приложения, которое должно быть проверено для правильной работы приложения.</span><span class="sxs-lookup"><span data-stu-id="9d09f-181">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="9d09f-182">Конфигурация клиентского приложения</span><span class="sxs-lookup"><span data-stu-id="9d09f-182">Client app configuration</span></span>

<span data-ttu-id="9d09f-183">*Этот раздел относится к **клиентскому** приложению решения.*</span><span class="sxs-lookup"><span data-stu-id="9d09f-183">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="9d09f-184">Пакет проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9d09f-184">Authentication package</span></span>

<span data-ttu-id="9d09f-185">При создании приложения для использования отдельной учетной записи B2C (`IndividualB2C`) приложение автоматически получает ссылку на пакет для [библиотеки проверки подлинности Майкрософт](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="9d09f-185">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="9d09f-186">Пакет предоставляет набор примитивов, которые помогают приложению проверять подлинность пользователей и получать маркеры для вызова защищенных интерфейсов API.</span><span class="sxs-lookup"><span data-stu-id="9d09f-186">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="9d09f-187">При добавлении проверки подлинности в приложение вручную добавьте пакет в файл проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="9d09f-187">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="9d09f-188">Замените `{VERSION}` в предыдущей ссылке на пакет версией пакета `Microsoft.AspNetCore.Blazor.Templates`, показанного в статье <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="9d09f-188">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="9d09f-189">Пакет `Microsoft.Authentication.WebAssembly.Msal` транзитивно добавляет пакет `Microsoft.AspNetCore.Components.WebAssembly.Authentication` в приложение.</span><span class="sxs-lookup"><span data-stu-id="9d09f-189">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="9d09f-190">Поддержка службы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9d09f-190">Authentication service support</span></span>

<span data-ttu-id="9d09f-191">Поддержка проверки подлинности пользователей регистрируется в контейнере службы с помощью метода расширения `AddMsalAuthentication`, предоставленного пакетом `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="9d09f-191">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="9d09f-192">Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком удостоверений (IP).</span><span class="sxs-lookup"><span data-stu-id="9d09f-192">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="9d09f-193">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9d09f-193">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{APP ID URI}/{DEFAULT SCOPE}");
});
```

<span data-ttu-id="9d09f-194">Метод `AddMsalAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения.</span><span class="sxs-lookup"><span data-stu-id="9d09f-194">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="9d09f-195">Значения, необходимые для настройки приложения, можно получить из конфигурации AAD на портале Azure при регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="9d09f-195">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="9d09f-196">Шаблон Blazor-Assembly автоматически настраивает приложение для запроса маркера доступа для безопасного API для области по умолчанию, предоставленной для команды `dotnet new` (`{APP ID URI}/{DEFAULT SCOPE}`).</span><span class="sxs-lookup"><span data-stu-id="9d09f-196">The Blazor WebAssembly template automatically configures the app to request an access token for a secure API for the default scope provided to the `dotnet new` command (`{APP ID URI}/{DEFAULT SCOPE}`).</span></span>

<span data-ttu-id="9d09f-197">Области токена доступа по умолчанию представляют собой список областей токенов доступа.</span><span class="sxs-lookup"><span data-stu-id="9d09f-197">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="9d09f-198">Включается по умолчанию в запросе на вход.</span><span class="sxs-lookup"><span data-stu-id="9d09f-198">Included by default in the sign in request.</span></span>
* <span data-ttu-id="9d09f-199">Используется для предоставления маркера доступа сразу после проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="9d09f-199">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="9d09f-200">Все области должны принадлежать одному и тому же приложению для правил Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9d09f-200">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="9d09f-201">При необходимости можно добавить дополнительные области для дополнительных приложений API:</span><span class="sxs-lookup"><span data-stu-id="9d09f-201">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{APP ID URI}/{SCOPE}");
});
```

### <a name="index-page"></a><span data-ttu-id="9d09f-202">Страница индексации</span><span class="sxs-lookup"><span data-stu-id="9d09f-202">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a><span data-ttu-id="9d09f-203">Компонент приложения</span><span class="sxs-lookup"><span data-stu-id="9d09f-203">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="9d09f-204">Компонент Редиректтологин</span><span class="sxs-lookup"><span data-stu-id="9d09f-204">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="9d09f-205">Компонент Логиндисплай</span><span class="sxs-lookup"><span data-stu-id="9d09f-205">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="9d09f-206">Компонент проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9d09f-206">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="9d09f-207">Компонент FetchData</span><span class="sxs-lookup"><span data-stu-id="9d09f-207">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="9d09f-208">Запустите приложение</span><span class="sxs-lookup"><span data-stu-id="9d09f-208">Run the app</span></span>

<span data-ttu-id="9d09f-209">Запустите приложение из серверного проекта.</span><span class="sxs-lookup"><span data-stu-id="9d09f-209">Run the app from the Server project.</span></span> <span data-ttu-id="9d09f-210">При использовании Visual Studio выберите серверный проект в **Обозреватель решений** и нажмите кнопку **выполнить** на панели инструментов или запустите приложение из меню **Отладка** .</span><span class="sxs-lookup"><span data-stu-id="9d09f-210">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="9d09f-211">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9d09f-211">Additional resources</span></span>

* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="9d09f-212">Руководство по созданию клиента Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="9d09f-212">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
