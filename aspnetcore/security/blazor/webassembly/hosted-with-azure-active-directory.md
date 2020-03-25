---
title: Обеспечение безопасности размещенного в ASP.NET Core Blazor приложения сборки Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: fc16a7212254e73efd4cea8155975f293e5d9ebb
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219289"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a><span data-ttu-id="b69c7-102">Обеспечение безопасности размещенного в ASP.NET Core Blazor приложения сборки Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b69c7-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory</span></span>

<span data-ttu-id="b69c7-103">[Хавьер Калварро Воронков](https://github.com/javiercn) и [Люк ЛаСаМ](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b69c7-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



<span data-ttu-id="b69c7-104">В этой статье описывается создание [размещенного вBlazor приложения](xref:blazor/hosting-models#blazor-webassembly) , использующего [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b69c7-104">This article describes how to create a [Blazor WebAssembly hosted app](xref:blazor/hosting-models#blazor-webassembly) that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="b69c7-105">Регистрация приложений в AAD B2C и создание решения</span><span class="sxs-lookup"><span data-stu-id="b69c7-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="b69c7-106">Создание клиента</span><span class="sxs-lookup"><span data-stu-id="b69c7-106">Create a tenant</span></span>

<span data-ttu-id="b69c7-107">Следуйте указаниям в [кратком руководстве по настройке клиента](/azure/active-directory/develop/quickstart-create-new-tenant) для создания клиента в AAD.</span><span class="sxs-lookup"><span data-stu-id="b69c7-107">Follow the guidance in [Quickstart: Set up a tenant](/azure/active-directory/develop/quickstart-create-new-tenant) to create a tenant in AAD.</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="b69c7-108">Регистрация приложения API сервера</span><span class="sxs-lookup"><span data-stu-id="b69c7-108">Register a server API app</span></span>

<span data-ttu-id="b69c7-109">Следуйте указаниям в [кратком руководстве: регистрация приложения с помощью платформы удостоверений Майкрософт](/azure/active-directory/develop/quickstart-register-app) и последующих разделов Azure AAD, чтобы зарегистрировать приложение AAD для *приложения API сервера* в **Azure Active Directory** > **Регистрация приложений** области портал Azure:</span><span class="sxs-lookup"><span data-stu-id="b69c7-109">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="b69c7-110">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-110">Select **New registration**.</span></span>
1. <span data-ttu-id="b69c7-111">Укажите **имя** приложения (например, **Blazor Server AAD**).</span><span class="sxs-lookup"><span data-stu-id="b69c7-111">Provide a **Name** for the app (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="b69c7-112">Выберите **Поддерживаемые типы учетных записей**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-112">Choose a **Supported account types**.</span></span> <span data-ttu-id="b69c7-113">Для этого интерфейса можно выбрать **учетные записи только в этом каталоге Организации** (один клиент).</span><span class="sxs-lookup"><span data-stu-id="b69c7-113">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="b69c7-114">В этом сценарии *приложению API сервера* не требуется **универсальный код ресурса (URI) перенаправления** , поэтому оставьте в раскрывающемся списке значение **Web** и не вводите URI перенаправления.</span><span class="sxs-lookup"><span data-stu-id="b69c7-114">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="b69c7-115">Отключите **разрешения** > установите флажок **предоставить доступ к администратору для OpenID Connect и offline_access разрешений** .</span><span class="sxs-lookup"><span data-stu-id="b69c7-115">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="b69c7-116">Выберите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-116">Select **Register**.</span></span>

<span data-ttu-id="b69c7-117">В окне **разрешения API**удалите разрешение **Microsoft Graph** > **пользователь. чтение** , так как приложению не требуется доступ для входа или профиля уер.</span><span class="sxs-lookup"><span data-stu-id="b69c7-117">In **API permissions**, remove the **Microsoft Graph** > **User.Read** permission, as the app doesn't require sign in or uer profile access.</span></span>

<span data-ttu-id="b69c7-118">В **предоставление API**:</span><span class="sxs-lookup"><span data-stu-id="b69c7-118">In **Expose an API**:</span></span>

1. <span data-ttu-id="b69c7-119">Нажмите **Добавить группу**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-119">Select **Add a scope**.</span></span>
1. <span data-ttu-id="b69c7-120">Выберите **Сохранить и продолжить**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-120">Select **Save and continue**.</span></span>
1. <span data-ttu-id="b69c7-121">Укажите **имя области** (например, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="b69c7-121">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="b69c7-122">Укажите **Отображаемое имя согласия администратора** (например, `Access API`).</span><span class="sxs-lookup"><span data-stu-id="b69c7-122">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="b69c7-123">Введите **Описание согласия администратора** (например, `Allows the app to access server app API endpoints.`).</span><span class="sxs-lookup"><span data-stu-id="b69c7-123">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="b69c7-124">Убедитесь, что для **состояния** задано значение **включено**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-124">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="b69c7-125">Выберите **Добавить область**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-125">Select **Add scope**.</span></span>

<span data-ttu-id="b69c7-126">Запишите следующие сведения:</span><span class="sxs-lookup"><span data-stu-id="b69c7-126">Record the following information:</span></span>

* <span data-ttu-id="b69c7-127">*Приложение API сервера* Идентификатор приложения (идентификатор клиента) (например, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="b69c7-127">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="b69c7-128">Идентификатор каталога (идентификатор клиента) (например, `222222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="b69c7-128">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="b69c7-129">Домен клиента AAD (например, `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="b69c7-129">AAD Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>
* <span data-ttu-id="b69c7-130">Область по умолчанию (например, `API.Access`)</span><span class="sxs-lookup"><span data-stu-id="b69c7-130">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="b69c7-131">Регистрация клиентского приложения</span><span class="sxs-lookup"><span data-stu-id="b69c7-131">Register a client app</span></span>

<span data-ttu-id="b69c7-132">Следуйте указаниям в [кратком руководстве: регистрация приложения с помощью платформы удостоверений Майкрософт](/azure/active-directory/develop/quickstart-register-app) и последующих разделов Azure AAD для регистрации приложения AAD для *клиентского приложения* в **Azure Active Directory** > **Регистрация приложений** области портал Azure:</span><span class="sxs-lookup"><span data-stu-id="b69c7-132">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register a AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="b69c7-133">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-133">Select **New registration**.</span></span>
1. <span data-ttu-id="b69c7-134">Укажите **имя** приложения (например, **Blazor клиента AAD**).</span><span class="sxs-lookup"><span data-stu-id="b69c7-134">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="b69c7-135">Выберите **Поддерживаемые типы учетных записей**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-135">Choose a **Supported account types**.</span></span> <span data-ttu-id="b69c7-136">Для этого интерфейса можно выбрать **учетные записи только в этом каталоге Организации** (один клиент).</span><span class="sxs-lookup"><span data-stu-id="b69c7-136">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="b69c7-137">Оставьте в раскрывающемся списке **URI перенаправления** значение **веб**и укажите универсальный код ресурса (uri) перенаправления `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="b69c7-137">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="b69c7-138">Отключите **разрешения** > установите флажок **предоставить доступ к администратору для OpenID Connect и offline_access разрешений** .</span><span class="sxs-lookup"><span data-stu-id="b69c7-138">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="b69c7-139">Выберите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-139">Select **Register**.</span></span>

<span data-ttu-id="b69c7-140">При **проверке Подлинности** > **конфигурации платформы** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="b69c7-140">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="b69c7-141">Убедитесь, что **URI перенаправления** для `https://localhost:5001/authentication/login-callback` имеется.</span><span class="sxs-lookup"><span data-stu-id="b69c7-141">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="b69c7-142">Для **неявного предоставления**установите флажки для **маркеров доступа** и **маркеров идентификации**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-142">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="b69c7-143">Остальные значения по умолчанию для приложения приемлемы для этого интерфейса.</span><span class="sxs-lookup"><span data-stu-id="b69c7-143">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="b69c7-144">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-144">Select the **Save** button.</span></span>

<span data-ttu-id="b69c7-145">В **разрешениях API**:</span><span class="sxs-lookup"><span data-stu-id="b69c7-145">In **API permissions**:</span></span>

1. <span data-ttu-id="b69c7-146">Убедитесь, что приложение имеет **Microsoft Graph** > **пользователь.** разрешение на чтение.</span><span class="sxs-lookup"><span data-stu-id="b69c7-146">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="b69c7-147">Выберите **Добавить разрешение** , а затем — **Мои API**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-147">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="b69c7-148">Выберите *приложение API сервера* из столбца **имя** (например, **Blazor Server AAD**).</span><span class="sxs-lookup"><span data-stu-id="b69c7-148">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="b69c7-149">Откройте список **API** .</span><span class="sxs-lookup"><span data-stu-id="b69c7-149">Open the **API** list.</span></span>
1. <span data-ttu-id="b69c7-150">Разрешение доступа к API (например, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="b69c7-150">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="b69c7-151">Выберите **Добавить разрешения**.</span><span class="sxs-lookup"><span data-stu-id="b69c7-151">Select **Add permissions**.</span></span>
1. <span data-ttu-id="b69c7-152">Нажмите кнопку **предоставить содержимое администратора для {имя клиента}** .</span><span class="sxs-lookup"><span data-stu-id="b69c7-152">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="b69c7-153">Выберите **Да** для подтверждения.</span><span class="sxs-lookup"><span data-stu-id="b69c7-153">Select **Yes** to confirm.</span></span>

<span data-ttu-id="b69c7-154">Запишите идентификатор приложения *клиентского приложения* (идентификатор клиента) (например, `33333333-3333-3333-3333-333333333333`).</span><span class="sxs-lookup"><span data-stu-id="b69c7-154">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="b69c7-155">Создайте приложение</span><span class="sxs-lookup"><span data-stu-id="b69c7-155">Create the app</span></span>

<span data-ttu-id="b69c7-156">Замените заполнители в следующей команде на записанные ранее сведения и выполните команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="b69c7-156">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP CLIENT ID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

<span data-ttu-id="b69c7-157">Чтобы указать расположение выходных данных, которое создает папку проекта, если она не существует, включите параметр OUTPUT в команду с путем (например, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="b69c7-157">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="b69c7-158">Имя папки также станет частью имени проекта.</span><span class="sxs-lookup"><span data-stu-id="b69c7-158">The folder name also becomes part of the project's name.</span></span>

> [!NOTE]
> <span data-ttu-id="b69c7-159">Важные изменения конфигурации для области маркера доступа по умолчанию см. в разделе [Поддержка службы проверки подлинности](#Authentication service support) .</span><span class="sxs-lookup"><span data-stu-id="b69c7-159">See the [Authentication service support](#Authentication service support) section for an important configuration change to the default access token scope.</span></span> <span data-ttu-id="b69c7-160">Значение, предоставленное шаблоном Blazor сборки, необходимо изменить вручную после того, как *клиентское приложение* будет создано из шаблона.</span><span class="sxs-lookup"><span data-stu-id="b69c7-160">The value provided by the Blazor WebAssembly template must be manually changed after the *Client app* is created from the template.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="b69c7-161">Конфигурация серверного приложения</span><span class="sxs-lookup"><span data-stu-id="b69c7-161">Server app configuration</span></span>

<span data-ttu-id="b69c7-162">*Этот раздел относится к **серверному** приложению решения.*</span><span class="sxs-lookup"><span data-stu-id="b69c7-162">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="b69c7-163">Пакет проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="b69c7-163">Authentication package</span></span>

<span data-ttu-id="b69c7-164">Поддержка проверки подлинности и авторизации вызовов ASP.NET Core веб-интерфейсов API обеспечивается `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span><span class="sxs-lookup"><span data-stu-id="b69c7-164">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="b69c7-165">Поддержка службы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="b69c7-165">Authentication service support</span></span>

<span data-ttu-id="b69c7-166">Метод `AddAuthentication` настраивает службы проверки подлинности в приложении и настраивает обработчик носителя JWT в качестве метода проверки подлинности по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b69c7-166">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="b69c7-167">Метод `AddAzureADBearer` настраивает определенные параметры в обработчике носителя JWT, который требуется для проверки маркеров, создаваемых Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="b69c7-167">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="b69c7-168">`UseAuthentication` и `UseAuthorization` убедитесь, что:</span><span class="sxs-lookup"><span data-stu-id="b69c7-168">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="b69c7-169">Приложение пытается проанализировать и проверить маркеры в входящих запросах.</span><span class="sxs-lookup"><span data-stu-id="b69c7-169">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="b69c7-170">Любой запрос, пытающийся получить доступ к защищенному ресурсу без соответствующих учетных данных, завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="b69c7-170">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a><span data-ttu-id="b69c7-171">Параметры приложения</span><span class="sxs-lookup"><span data-stu-id="b69c7-171">App settings</span></span>

<span data-ttu-id="b69c7-172">Файл *appSettings. JSON* содержит параметры для настройки обработчика носителя JWT, используемого для проверки маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="b69c7-172">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{API CLIENT ID}",
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="b69c7-173">Контроллер Веасерфорекаст</span><span class="sxs-lookup"><span data-stu-id="b69c7-173">WeatherForecast controller</span></span>

<span data-ttu-id="b69c7-174">Контроллер Веасерфорекаст (*Controllers/веасерфорекастконтроллер. CS*) предоставляет защищенный API с атрибутом `[Authorize]`, применяемым к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="b69c7-174">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="b69c7-175">**Важно** понимать, что:</span><span class="sxs-lookup"><span data-stu-id="b69c7-175">It's **important** to understand that:</span></span>

* <span data-ttu-id="b69c7-176">Атрибут `[Authorize]` в этом контроллере API является единственным, который защищает этот API от несанкционированного доступа.</span><span class="sxs-lookup"><span data-stu-id="b69c7-176">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="b69c7-177">Атрибут `[Authorize]`, используемый в приложении Blazor сборки, служит указанием для приложения, которое должно быть проверено для правильной работы приложения.</span><span class="sxs-lookup"><span data-stu-id="b69c7-177">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

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

## <a name="client-app-configuration"></a><span data-ttu-id="b69c7-178">Конфигурация клиентского приложения</span><span class="sxs-lookup"><span data-stu-id="b69c7-178">Client app configuration</span></span>

<span data-ttu-id="b69c7-179">*Этот раздел относится к **клиентскому** приложению решения.*</span><span class="sxs-lookup"><span data-stu-id="b69c7-179">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="b69c7-180">Пакет проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="b69c7-180">Authentication package</span></span>

<span data-ttu-id="b69c7-181">Когда приложение создается для использования рабочих или учебных учетных записей (`SingleOrg`), приложение автоматически получает ссылку на пакет для [библиотеки проверки подлинности Майкрософт](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="b69c7-181">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="b69c7-182">Пакет предоставляет набор примитивов, которые помогают приложению проверять подлинность пользователей и получать маркеры для вызова защищенных интерфейсов API.</span><span class="sxs-lookup"><span data-stu-id="b69c7-182">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="b69c7-183">При добавлении проверки подлинности в приложение вручную добавьте пакет в файл проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="b69c7-183">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="b69c7-184">Замените `{VERSION}` в предыдущей ссылке на пакет версией пакета `Microsoft.AspNetCore.Blazor.Templates`, показанного в статье <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="b69c7-184">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="b69c7-185">Пакет `Microsoft.Authentication.WebAssembly.Msal` транзитивно добавляет пакет `Microsoft.AspNetCore.Components.WebAssembly.Authentication` в приложение.</span><span class="sxs-lookup"><span data-stu-id="b69c7-185">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="b69c7-186">Поддержка службы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="b69c7-186">Authentication service support</span></span>

<span data-ttu-id="b69c7-187">Поддержка проверки подлинности пользователей регистрируется в контейнере службы с помощью метода расширения `AddMsalAuthentication`, предоставленного пакетом `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="b69c7-187">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="b69c7-188">Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком удостоверений (IP).</span><span class="sxs-lookup"><span data-stu-id="b69c7-188">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="b69c7-189">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b69c7-189">*Program.cs*:</span></span>

<span data-ttu-id="b69c7-190">При создании *клиентского приложения* область маркера доступа по умолчанию имеет формат `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`.</span><span class="sxs-lookup"><span data-stu-id="b69c7-190">When the *Client app* is generated, the default access token scope is of the format `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`.</span></span> <span data-ttu-id="b69c7-191">**Удалите `api://`ную часть значения области.**</span><span class="sxs-lookup"><span data-stu-id="b69c7-191">**Remove the `api://` portion of the scope value.**</span></span> <span data-ttu-id="b69c7-192">Эта проблема будет устранена в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="b69c7-192">This issue will be addressed in a future preview release.</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> <span data-ttu-id="b69c7-193">Область маркера доступа по умолчанию должна иметь формат `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (например, `11111111-1111-1111-1111-111111111111/API.Access`).</span><span class="sxs-lookup"><span data-stu-id="b69c7-193">The default access token scope must be in the format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (for example, `11111111-1111-1111-1111-111111111111/API.Access`).</span></span> <span data-ttu-id="b69c7-194">Если для параметра области (как показано на портале Azure) предоставлена схема или схема и узел, то *клиентское приложение* создает необработанное исключение при получении от *приложения API сервера*ответа *401 с несанкционированным* доступом.</span><span class="sxs-lookup"><span data-stu-id="b69c7-194">If a scheme or scheme and host is provided to the scope setting (as shown in the Azure Portal), the *Client app* throws an unhandled exception when it receives a *401 Unauthorized* response from the *Server API app*.</span></span>

<span data-ttu-id="b69c7-195">Метод `AddMsalAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения.</span><span class="sxs-lookup"><span data-stu-id="b69c7-195">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="b69c7-196">Значения, необходимые для настройки приложения, можно получить из конфигурации AAD на портале Azure при регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="b69c7-196">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="b69c7-197">Области токена доступа по умолчанию представляют собой список областей токенов доступа.</span><span class="sxs-lookup"><span data-stu-id="b69c7-197">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="b69c7-198">Включается по умолчанию в запросе на вход.</span><span class="sxs-lookup"><span data-stu-id="b69c7-198">Included by default in the sign in request.</span></span>
* <span data-ttu-id="b69c7-199">Используется для предоставления маркера доступа сразу после проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b69c7-199">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="b69c7-200">Все области должны принадлежать одному и тому же приложению для правил Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b69c7-200">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="b69c7-201">При необходимости можно добавить дополнительные области для дополнительных приложений API:</span><span class="sxs-lookup"><span data-stu-id="b69c7-201">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{SCOPE}");
});
```

### <a name="index-page"></a><span data-ttu-id="b69c7-202">Страница индексации</span><span class="sxs-lookup"><span data-stu-id="b69c7-202">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a><span data-ttu-id="b69c7-203">Компонент приложения</span><span class="sxs-lookup"><span data-stu-id="b69c7-203">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="b69c7-204">Компонент Редиректтологин</span><span class="sxs-lookup"><span data-stu-id="b69c7-204">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="b69c7-205">Компонент Логиндисплай</span><span class="sxs-lookup"><span data-stu-id="b69c7-205">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="b69c7-206">Компонент проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="b69c7-206">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="b69c7-207">Компонент FetchData</span><span class="sxs-lookup"><span data-stu-id="b69c7-207">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="b69c7-208">Запустите приложение</span><span class="sxs-lookup"><span data-stu-id="b69c7-208">Run the app</span></span>

<span data-ttu-id="b69c7-209">Запустите приложение из серверного проекта.</span><span class="sxs-lookup"><span data-stu-id="b69c7-209">Run the app from the Server project.</span></span> <span data-ttu-id="b69c7-210">При использовании Visual Studio выберите серверный проект в **Обозреватель решений** и нажмите кнопку **выполнить** на панели инструментов или запустите приложение из меню **Отладка** .</span><span class="sxs-lookup"><span data-stu-id="b69c7-210">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="b69c7-211">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b69c7-211">Additional resources</span></span>

* <xref:security/authentication/azure-active-directory/index>
