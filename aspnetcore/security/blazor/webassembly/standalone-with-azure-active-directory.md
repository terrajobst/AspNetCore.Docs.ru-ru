---
title: Защищайте Blazor автономное приложение ASP.NET Core WebAssembly с помощью Active Directory Azure
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: 7e132723657b7e12803b67ec12c3a33f1945baa3
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977004"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a><span data-ttu-id="dac8d-102">Защищайте Blazor автономное приложение ASP.NET Core WebAssembly с помощью Active Directory Azure</span><span class="sxs-lookup"><span data-stu-id="dac8d-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory</span></span>

<span data-ttu-id="dac8d-103">[Хавьер Кальварро Нельсон](https://github.com/javiercn) и Люк [Лэйтам](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="dac8d-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="dac8d-104">Для создания Blazor автономного приложения WebAssembly, использующему [Active Directory (AAD) Azure Для](https://azure.microsoft.com/services/active-directory/) проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="dac8d-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication:</span></span>

<span data-ttu-id="dac8d-105">[Создание арендатора AAD и веб-приложения:](/azure/active-directory/develop/v2-overview)</span><span class="sxs-lookup"><span data-stu-id="dac8d-105">[Create an AAD tenant and web application](/azure/active-directory/develop/v2-overview):</span></span>

<span data-ttu-id="dac8d-106">Зарегистрируйте приложение AAD в зоне**регистрации приложений** **Azure Active Directory** > на портале Azure:</span><span class="sxs-lookup"><span data-stu-id="dac8d-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="dac8d-107">Укажите **имя** приложения (например, \*\* Blazor Client AAD).\*\*</span><span class="sxs-lookup"><span data-stu-id="dac8d-107">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="dac8d-108">Выберите **типы учетной записи Поддержки.**</span><span class="sxs-lookup"><span data-stu-id="dac8d-108">Choose a **Supported account types**.</span></span> <span data-ttu-id="dac8d-109">Вы можете выбрать **учетные записи в этом каталоге организации только** для этого опыта.</span><span class="sxs-lookup"><span data-stu-id="dac8d-109">You may select **Accounts in this organizational directory only** for this experience.</span></span>
1. <span data-ttu-id="dac8d-110">Оставьте **перенаправить URI** упасть набор в **Интернет**, `https://localhost:5001/authentication/login-callback`и обеспечить перенаправление URI .</span><span class="sxs-lookup"><span data-stu-id="dac8d-110">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="dac8d-111">Отключите > **админ-концентратор** **Разрешений**Гранта для проверки открытых и offline_access разрешений.</span><span class="sxs-lookup"><span data-stu-id="dac8d-111">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="dac8d-112">Выберите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="dac8d-112">Select **Register**.</span></span>

<span data-ttu-id="dac8d-113">В**настройках платформы** >  **аутентификации** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="dac8d-113">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="dac8d-114">Подтвердите, `https://localhost:5001/authentication/login-callback` что **redirect URI** присутствует.</span><span class="sxs-lookup"><span data-stu-id="dac8d-114">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="dac8d-115">Для **неявного гранта**выберите флажки для **токенов Access** и **токенов ID.**</span><span class="sxs-lookup"><span data-stu-id="dac8d-115">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="dac8d-116">Остальные по умолчанию для приложения приемлемы для этого опыта.</span><span class="sxs-lookup"><span data-stu-id="dac8d-116">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="dac8d-117">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="dac8d-117">Select the **Save** button.</span></span>

<span data-ttu-id="dac8d-118">Запишите следующую информацию:</span><span class="sxs-lookup"><span data-stu-id="dac8d-118">Record the following information:</span></span>

* <span data-ttu-id="dac8d-119">Идентификатор приложения (идентификатор клиента) (например, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="dac8d-119">Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="dac8d-120">Идентификатор каталога (идентификатор арендатора) (например, `22222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="dac8d-120">Directory ID (Tenant ID) (for example, `22222222-2222-2222-2222-222222222222`)</span></span>

<span data-ttu-id="dac8d-121">Замените заполнителей в следующей команде информацией, записанной ранее, и выполните команду в командном корпусе:</span><span class="sxs-lookup"><span data-stu-id="dac8d-121">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="dac8d-122">Чтобы указать местона вывода, которое создает папку проекта, если она не существует, включите опцию вывода в команду с путем (например, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="dac8d-122">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="dac8d-123">Имя папки также становится частью названия проекта.</span><span class="sxs-lookup"><span data-stu-id="dac8d-123">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="dac8d-124">Пакет аутентификации</span><span class="sxs-lookup"><span data-stu-id="dac8d-124">Authentication package</span></span>

<span data-ttu-id="dac8d-125">Когда приложение создается для использования рабочих`SingleOrg`или школьных учетных записей ( ),`Microsoft.Authentication.WebAssembly.Msal`приложение автоматически получает ссылку на пакет для [библиотеки подлинности Майкрософт](/azure/active-directory/develop/msal-overview) ().</span><span class="sxs-lookup"><span data-stu-id="dac8d-125">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="dac8d-126">Пакет предоставляет набор примитивов, которые помогают приложению аутентифицировать пользователей и получать токены для вызова защищенных AIS.</span><span class="sxs-lookup"><span data-stu-id="dac8d-126">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="dac8d-127">При добавлении аутентификации в приложение вручную добавьте пакет в файл проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="dac8d-127">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="dac8d-128">Замените `{VERSION}` в предыдущем пакете ссылку на версию `Microsoft.AspNetCore.Blazor.Templates` пакета, показанную <xref:blazor/get-started> в статье.</span><span class="sxs-lookup"><span data-stu-id="dac8d-128">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="dac8d-129">Пакет `Microsoft.Authentication.WebAssembly.Msal` транзитно добавляет `Microsoft.AspNetCore.Components.WebAssembly.Authentication` пакет в приложение.</span><span class="sxs-lookup"><span data-stu-id="dac8d-129">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="dac8d-130">Поддержка службы аутентификации</span><span class="sxs-lookup"><span data-stu-id="dac8d-130">Authentication service support</span></span>

<span data-ttu-id="dac8d-131">Поддержка аутентификации пользователей регистрируется `AddMsalAuthentication` в сервисном `Microsoft.Authentication.WebAssembly.Msal` контейнере с методом расширения, предусмотренным пакетом.</span><span class="sxs-lookup"><span data-stu-id="dac8d-131">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="dac8d-132">Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком идентификационных данных (IP).</span><span class="sxs-lookup"><span data-stu-id="dac8d-132">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="dac8d-133">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="dac8d-133">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="dac8d-134">Метод `AddMsalAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения.</span><span class="sxs-lookup"><span data-stu-id="dac8d-134">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="dac8d-135">Значения, необходимые для настройки приложения, можно получить из конфигурации Azure Portal AAD при регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="dac8d-135">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="dac8d-136">Области маркеров доступа</span><span class="sxs-lookup"><span data-stu-id="dac8d-136">Access token scopes</span></span>

<span data-ttu-id="dac8d-137">Шаблон Blazor WebAssembly не настраивает приложение автоматически, чтобы запросить токен доступа для защищенного API.</span><span class="sxs-lookup"><span data-stu-id="dac8d-137">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="dac8d-138">Чтобы предоставить токен как часть потока ввоза, добавьте область к области `MsalProviderOptions`маркеров доступа по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="dac8d-138">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="dac8d-139">Если портал Azure предоставляет область URI и **приложение выбрасывает необработанное исключение,** когда получает *401 несанкционированный* ответ от API, попробуйте использовать область URI, которая не включает схему и узел.</span><span class="sxs-lookup"><span data-stu-id="dac8d-139">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="dac8d-140">Например, портал Azure может предоставить один из следующих форматов URI:</span><span class="sxs-lookup"><span data-stu-id="dac8d-140">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="dac8d-141">Поставка области URI без схемы и хозяина:</span><span class="sxs-lookup"><span data-stu-id="dac8d-141">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="dac8d-142">Для получения дополнительной информации см. <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span><span class="sxs-lookup"><span data-stu-id="dac8d-142">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

## <a name="imports-file"></a><span data-ttu-id="dac8d-143">Файл импорта</span><span class="sxs-lookup"><span data-stu-id="dac8d-143">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="dac8d-144">Страница индексации</span><span class="sxs-lookup"><span data-stu-id="dac8d-144">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="dac8d-145">Компонент приложения</span><span class="sxs-lookup"><span data-stu-id="dac8d-145">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="dac8d-146">ПеренаправлениеКомпонентToLogin</span><span class="sxs-lookup"><span data-stu-id="dac8d-146">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="dac8d-147">Компонент LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="dac8d-147">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="dac8d-148">Компонент аутентификации</span><span class="sxs-lookup"><span data-stu-id="dac8d-148">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="dac8d-149">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="dac8d-149">Additional resources</span></span>

* [<span data-ttu-id="dac8d-150">Запрос дополнительных токенов доступа</span><span class="sxs-lookup"><span data-stu-id="dac8d-150">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* <xref:security/authentication/azure-active-directory/index>
* [<span data-ttu-id="dac8d-151">Документация по платформе удостоверений Майкрософт</span><span class="sxs-lookup"><span data-stu-id="dac8d-151">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
