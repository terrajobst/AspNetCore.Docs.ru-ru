---
title: Защита ASP.NET Core автономного приложения Blazor сборки с помощью Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: bb03ef1e6d216cfc06e2b91919c64f92f2ef634e
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219276"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a><span data-ttu-id="a33ad-102">Защита ASP.NET Core автономного приложения Blazor сборки с помощью Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="a33ad-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory B2C</span></span>

<span data-ttu-id="a33ad-103">[Хавьер Калварро Воронков](https://github.com/javiercn) и [Люк ЛаСаМ](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a33ad-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="a33ad-104">Чтобы создать изолированное приложение Blazor сборки, использующее [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="a33ad-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication:</span></span>

1. <span data-ttu-id="a33ad-105">Следуйте указаниям в следующих разделах, чтобы создать клиент и зарегистрировать веб-приложение на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="a33ad-105">Follow the guidance in the following topics to create a tenant and register a web app in the Azure Portal:</span></span>

   * <span data-ttu-id="a33ad-106">[Создайте клиент AAD B2C](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; запишите следующие сведения:</span><span class="sxs-lookup"><span data-stu-id="a33ad-106">[Create an AAD B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; Record the following information:</span></span>

     <span data-ttu-id="a33ad-107">1\.</span><span class="sxs-lookup"><span data-stu-id="a33ad-107">1\.</span></span> <span data-ttu-id="a33ad-108">AAD B2C экземпляр (например, `https://contoso.b2clogin.com/`, который включает замыкающую косую черту)</span><span class="sxs-lookup"><span data-stu-id="a33ad-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span><br>
     <span data-ttu-id="a33ad-109">2\.</span><span class="sxs-lookup"><span data-stu-id="a33ad-109">2\.</span></span> <span data-ttu-id="a33ad-110">Домен клиента AAD B2C (например, `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="a33ad-110">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

   * <span data-ttu-id="a33ad-111">[Зарегистрируйте веб-приложение](/azure/active-directory-b2c/tutorial-register-applications) &ndash; сделайте следующее при регистрации приложения:</span><span class="sxs-lookup"><span data-stu-id="a33ad-111">[Register a web application](/azure/active-directory-b2c/tutorial-register-applications) &ndash; Make the following selections during app registration:</span></span>

     <span data-ttu-id="a33ad-112">1\.</span><span class="sxs-lookup"><span data-stu-id="a33ad-112">1\.</span></span> <span data-ttu-id="a33ad-113">Задайте для параметра **веб-приложение или веб-API** значение **Да**.</span><span class="sxs-lookup"><span data-stu-id="a33ad-113">Set **Web App / Web API** to **Yes**.</span></span><br>
     <span data-ttu-id="a33ad-114">2\.</span><span class="sxs-lookup"><span data-stu-id="a33ad-114">2\.</span></span> <span data-ttu-id="a33ad-115">Установите для параметра **Разрешить неявный поток** значение **Да**.</span><span class="sxs-lookup"><span data-stu-id="a33ad-115">Set **Allow implicit flow** to **Yes**.</span></span><br>
     <span data-ttu-id="a33ad-116">3\.</span><span class="sxs-lookup"><span data-stu-id="a33ad-116">3\.</span></span> <span data-ttu-id="a33ad-117">Добавьте **URL-адрес ответа** `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="a33ad-117">Add a **Reply URL** of `https://localhost:5001/authentication/login-callback`.</span></span>

     <span data-ttu-id="a33ad-118">Запишите идентификатор приложения (идентификатор клиента) (например, `11111111-1111-1111-1111-111111111111`).</span><span class="sxs-lookup"><span data-stu-id="a33ad-118">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

   * <span data-ttu-id="a33ad-119">[Создание потоков пользователя](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; создание потока пользователя регистрации и входа в систему.</span><span class="sxs-lookup"><span data-stu-id="a33ad-119">[Create user flows](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; Create a sign-up and sign-in user flow.</span></span>

     <span data-ttu-id="a33ad-120">Чтобы заполнить `context.User.Identity.Name` в компоненте `LoginDisplay` (*Shared/логиндисплай. Razor*), как минимум, выберите **Application Claims (заявка на приложение** ) > **Отображаемое имя** пользовательский атрибут.</span><span class="sxs-lookup"><span data-stu-id="a33ad-120">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

     <span data-ttu-id="a33ad-121">Запишите имя потока пользователя для регистрации и входа, созданное для приложения (например, `B2C_1_signupsignin`).</span><span class="sxs-lookup"><span data-stu-id="a33ad-121">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

1. <span data-ttu-id="a33ad-122">Замените заполнители в следующей команде на записанные ранее сведения и выполните команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="a33ad-122">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   <span data-ttu-id="a33ad-123">Чтобы указать расположение выходных данных, которое создает папку проекта, если она не существует, включите параметр OUTPUT в команду с путем (например, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="a33ad-123">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="a33ad-124">Имя папки также станет частью имени проекта.</span><span class="sxs-lookup"><span data-stu-id="a33ad-124">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="a33ad-125">Пакет проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="a33ad-125">Authentication package</span></span>

<span data-ttu-id="a33ad-126">При создании приложения для использования отдельной учетной записи B2C (`IndividualB2C`) приложение автоматически получает ссылку на пакет для [библиотеки проверки подлинности Майкрософт](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="a33ad-126">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="a33ad-127">Пакет предоставляет набор примитивов, которые помогают приложению проверять подлинность пользователей и получать маркеры для вызова защищенных интерфейсов API.</span><span class="sxs-lookup"><span data-stu-id="a33ad-127">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="a33ad-128">При добавлении проверки подлинности в приложение вручную добавьте пакет в файл проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="a33ad-128">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="a33ad-129">Замените `{VERSION}` в предыдущей ссылке на пакет версией пакета `Microsoft.AspNetCore.Blazor.Templates`, показанного в статье <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="a33ad-129">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="a33ad-130">Пакет `Microsoft.Authentication.WebAssembly.Msal` транзитивно добавляет пакет `Microsoft.AspNetCore.Components.WebAssembly.Authentication` в приложение.</span><span class="sxs-lookup"><span data-stu-id="a33ad-130">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="a33ad-131">Поддержка службы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="a33ad-131">Authentication service support</span></span>

<span data-ttu-id="a33ad-132">Поддержка проверки подлинности пользователей регистрируется в контейнере службы с помощью метода расширения `AddMsalAuthentication`, предоставленного пакетом `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="a33ad-132">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="a33ad-133">Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком удостоверений (IP).</span><span class="sxs-lookup"><span data-stu-id="a33ad-133">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="a33ad-134">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a33ad-134">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
});
```

<span data-ttu-id="a33ad-135">Метод `AddMsalAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения.</span><span class="sxs-lookup"><span data-stu-id="a33ad-135">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="a33ad-136">Значения, необходимые для настройки приложения, можно получить из конфигурации AAD на портале Azure при регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="a33ad-136">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="a33ad-137">Шаблон Blazor-сборки не автоматически настраивает приложение для запроса маркера доступа для безопасного API.</span><span class="sxs-lookup"><span data-stu-id="a33ad-137">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="a33ad-138">Чтобы создать маркер в рамках потока входа, добавьте область в область маркера доступа по умолчанию `MsalProviderOptions`:</span><span class="sxs-lookup"><span data-stu-id="a33ad-138">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{API ID URI}/{SCOPE}");
});
```

## <a name="index-page"></a><span data-ttu-id="a33ad-139">Страница индексации</span><span class="sxs-lookup"><span data-stu-id="a33ad-139">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="a33ad-140">Компонент приложения</span><span class="sxs-lookup"><span data-stu-id="a33ad-140">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="a33ad-141">Компонент Редиректтологин</span><span class="sxs-lookup"><span data-stu-id="a33ad-141">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="a33ad-142">Компонент Логиндисплай</span><span class="sxs-lookup"><span data-stu-id="a33ad-142">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="a33ad-143">Компонент проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="a33ad-143">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="a33ad-144">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a33ad-144">Additional resources</span></span>

* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="a33ad-145">Руководство по созданию клиента Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="a33ad-145">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
