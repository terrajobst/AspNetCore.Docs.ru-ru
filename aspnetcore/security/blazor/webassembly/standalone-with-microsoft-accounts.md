---
title: Защищайте Blazor автономное приложение ASP.NET Core WebAssembly с учетными записями Майкрософт
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: 8c409651b3338c2baeae497bef43b994823a20f9
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977084"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a><span data-ttu-id="47831-102">Защищайте Blazor автономное приложение ASP.NET Core WebAssembly с учетными записями Майкрософт</span><span class="sxs-lookup"><span data-stu-id="47831-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Microsoft Accounts</span></span>

<span data-ttu-id="47831-103">[Хавьер Кальварро Нельсон](https://github.com/javiercn) и Люк [Лэйтам](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="47831-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="47831-104">Для создания Blazor автономного приложения WebAssembly, использующее [учетные записи Майкрософт с помощью Active Directory (AAD) для](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="47831-104">To create a Blazor WebAssembly standalone app that uses [Microsoft Accounts with Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) for authentication:</span></span>

1. [<span data-ttu-id="47831-105">Создание арендатора AAD и веб-приложения</span><span class="sxs-lookup"><span data-stu-id="47831-105">Create an AAD tenant and web application</span></span>](/azure/active-directory/develop/v2-overview)

   <span data-ttu-id="47831-106">Зарегистрируйте приложение AAD в зоне**регистрации приложений** **Azure Active Directory** > на портале Azure:</span><span class="sxs-lookup"><span data-stu-id="47831-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

   <span data-ttu-id="47831-107">1\.</span><span class="sxs-lookup"><span data-stu-id="47831-107">1\.</span></span> <span data-ttu-id="47831-108">Укажите **имя** приложения (например, \*\* Blazor Client AAD).\*\*</span><span class="sxs-lookup"><span data-stu-id="47831-108">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span><br>
   <span data-ttu-id="47831-109">2\.</span><span class="sxs-lookup"><span data-stu-id="47831-109">2\.</span></span> <span data-ttu-id="47831-110">В **поддерживаемых типах учетных записей**выберите **учетные записи в любом каталоге организации.**</span><span class="sxs-lookup"><span data-stu-id="47831-110">In **Supported account types**, select **Accounts in any organizational directory**.</span></span><br>
   <span data-ttu-id="47831-111">3\.</span><span class="sxs-lookup"><span data-stu-id="47831-111">3\.</span></span> <span data-ttu-id="47831-112">Оставьте **перенаправить URI** упасть набор в **Интернет**, `https://localhost:5001/authentication/login-callback`и обеспечить перенаправление URI .</span><span class="sxs-lookup"><span data-stu-id="47831-112">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span><br>
   <span data-ttu-id="47831-113">4\.</span><span class="sxs-lookup"><span data-stu-id="47831-113">4\.</span></span> <span data-ttu-id="47831-114">Отключите > **админ-концентратор** **Разрешений**Гранта для проверки открытых и offline_access разрешений.</span><span class="sxs-lookup"><span data-stu-id="47831-114">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span><br>
   <span data-ttu-id="47831-115">5\.</span><span class="sxs-lookup"><span data-stu-id="47831-115">5\.</span></span> <span data-ttu-id="47831-116">Выберите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="47831-116">Select **Register**.</span></span>

   <span data-ttu-id="47831-117">В**настройках платформы** >  **аутентификации** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="47831-117">In **Authentication** > **Platform configurations** > **Web**:</span></span>

   <span data-ttu-id="47831-118">1\.</span><span class="sxs-lookup"><span data-stu-id="47831-118">1\.</span></span> <span data-ttu-id="47831-119">Подтвердите, `https://localhost:5001/authentication/login-callback` что **redirect URI** присутствует.</span><span class="sxs-lookup"><span data-stu-id="47831-119">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span><br>
   <span data-ttu-id="47831-120">2\.</span><span class="sxs-lookup"><span data-stu-id="47831-120">2\.</span></span> <span data-ttu-id="47831-121">Для **неявного гранта**выберите флажки для **токенов Access** и **токенов ID.**</span><span class="sxs-lookup"><span data-stu-id="47831-121">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span><br>
   <span data-ttu-id="47831-122">3\.</span><span class="sxs-lookup"><span data-stu-id="47831-122">3\.</span></span> <span data-ttu-id="47831-123">Остальные по умолчанию для приложения приемлемы для этого опыта.</span><span class="sxs-lookup"><span data-stu-id="47831-123">The remaining defaults for the app are acceptable for this experience.</span></span><br>
   <span data-ttu-id="47831-124">4\.</span><span class="sxs-lookup"><span data-stu-id="47831-124">4\.</span></span> <span data-ttu-id="47831-125">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="47831-125">Select the **Save** button.</span></span>

   <span data-ttu-id="47831-126">Запись идентификатора приложения (например, `11111111-1111-1111-1111-111111111111`id клиента).</span><span class="sxs-lookup"><span data-stu-id="47831-126">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

1. <span data-ttu-id="47831-127">Замените заполнителей в следующей команде информацией, записанной ранее, и выполните команду в командном корпусе:</span><span class="sxs-lookup"><span data-stu-id="47831-127">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   <span data-ttu-id="47831-128">Чтобы указать местона вывода, которое создает папку проекта, если она не существует, включите опцию вывода в команду с путем (например, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="47831-128">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="47831-129">Имя папки также становится частью названия проекта.</span><span class="sxs-lookup"><span data-stu-id="47831-129">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="47831-130">После создания приложения, вы должны быть в состоянии:</span><span class="sxs-lookup"><span data-stu-id="47831-130">After creating the app, you should be able to:</span></span>

* <span data-ttu-id="47831-131">Войти в приложение с помощью учетной записи Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="47831-131">Log into the app using a Microsoft Account.</span></span>
* <span data-ttu-id="47831-132">Запрос токенов доступа для AI Microsoft с Blazor использованием того же подхода, что и для автономных приложений, при условии, что вы правильно настроили приложение.</span><span class="sxs-lookup"><span data-stu-id="47831-132">Request access tokens for Microsoft APIs using the same approach as for standalone Blazor apps provided that you have configured the app correctly.</span></span> <span data-ttu-id="47831-133">Для получения дополнительной информации [см.](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)</span><span class="sxs-lookup"><span data-stu-id="47831-133">For more information, see [Quickstart: Configure an application to expose web APIs](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span></span>

## <a name="authentication-package"></a><span data-ttu-id="47831-134">Пакет аутентификации</span><span class="sxs-lookup"><span data-stu-id="47831-134">Authentication package</span></span>

<span data-ttu-id="47831-135">Когда приложение создается для использования рабочих`SingleOrg`или школьных учетных записей ( ),`Microsoft.Authentication.WebAssembly.Msal`приложение автоматически получает ссылку на пакет для [библиотеки подлинности Майкрософт](/azure/active-directory/develop/msal-overview) ().</span><span class="sxs-lookup"><span data-stu-id="47831-135">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="47831-136">Пакет предоставляет набор примитивов, которые помогают приложению аутентифицировать пользователей и получать токены для вызова защищенных AIS.</span><span class="sxs-lookup"><span data-stu-id="47831-136">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="47831-137">При добавлении аутентификации в приложение вручную добавьте пакет в файл проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="47831-137">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="47831-138">Замените `{VERSION}` в предыдущем пакете ссылку на версию `Microsoft.AspNetCore.Blazor.Templates` пакета, показанную <xref:blazor/get-started> в статье.</span><span class="sxs-lookup"><span data-stu-id="47831-138">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="47831-139">Пакет `Microsoft.Authentication.WebAssembly.Msal` транзитно добавляет `Microsoft.AspNetCore.Components.WebAssembly.Authentication` пакет в приложение.</span><span class="sxs-lookup"><span data-stu-id="47831-139">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="47831-140">Поддержка службы аутентификации</span><span class="sxs-lookup"><span data-stu-id="47831-140">Authentication service support</span></span>

<span data-ttu-id="47831-141">Поддержка аутентификации пользователей регистрируется `AddMsalAuthentication` в сервисном `Microsoft.Authentication.WebAssembly.Msal` контейнере с методом расширения, предусмотренным пакетом.</span><span class="sxs-lookup"><span data-stu-id="47831-141">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="47831-142">Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком идентификационных данных (IP).</span><span class="sxs-lookup"><span data-stu-id="47831-142">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="47831-143">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="47831-143">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="47831-144">Метод `AddMsalAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения.</span><span class="sxs-lookup"><span data-stu-id="47831-144">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="47831-145">Значения, необходимые для настройки приложения, могут быть получены из конфигурации учетных записей Майкрософт при регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="47831-145">The values required for configuring the app can be obtained from the Microsoft Accounts configuration when you register the app.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="47831-146">Области маркеров доступа</span><span class="sxs-lookup"><span data-stu-id="47831-146">Access token scopes</span></span>

<span data-ttu-id="47831-147">Шаблон Blazor WebAssembly не настраивает приложение автоматически, чтобы запросить токен доступа для защищенного API.</span><span class="sxs-lookup"><span data-stu-id="47831-147">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="47831-148">Чтобы предоставить токен как часть потока ввоза, добавьте область к области `MsalProviderOptions`маркеров доступа по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="47831-148">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="47831-149">Если портал Azure предоставляет область URI и **приложение выбрасывает необработанное исключение,** когда получает *401 несанкционированный* ответ от API, попробуйте использовать область URI, которая не включает схему и узел.</span><span class="sxs-lookup"><span data-stu-id="47831-149">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="47831-150">Например, портал Azure может предоставить один из следующих форматов URI:</span><span class="sxs-lookup"><span data-stu-id="47831-150">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="47831-151">Поставка области URI без схемы и хозяина:</span><span class="sxs-lookup"><span data-stu-id="47831-151">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="47831-152">Для получения дополнительной информации см. <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span><span class="sxs-lookup"><span data-stu-id="47831-152">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

## <a name="imports-file"></a><span data-ttu-id="47831-153">Файл импорта</span><span class="sxs-lookup"><span data-stu-id="47831-153">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="47831-154">Страница индексации</span><span class="sxs-lookup"><span data-stu-id="47831-154">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="47831-155">Компонент приложения</span><span class="sxs-lookup"><span data-stu-id="47831-155">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="47831-156">ПеренаправлениеКомпонентToLogin</span><span class="sxs-lookup"><span data-stu-id="47831-156">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="47831-157">Компонент LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="47831-157">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="47831-158">Компонент аутентификации</span><span class="sxs-lookup"><span data-stu-id="47831-158">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="47831-159">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="47831-159">Additional resources</span></span>

* [<span data-ttu-id="47831-160">Запрос дополнительных токенов доступа</span><span class="sxs-lookup"><span data-stu-id="47831-160">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="47831-161">Быстрый запуск: Зарегистрируйте приложение на платформе идентификации Майкрософт</span><span class="sxs-lookup"><span data-stu-id="47831-161">Quickstart: Register an application with the Microsoft identity platform</span></span>](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [<span data-ttu-id="47831-162">Быстрый запуск: Настройка приложения для разоблачения веб-AIS</span><span class="sxs-lookup"><span data-stu-id="47831-162">Quickstart: Configure an application to expose web APIs</span></span>](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
