---
title: Настройка внешней учетной записи Google в ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграцию Google учетной записи пользователя и проверки подлинности в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 5b6bfaafba68eaf15a60b7c512a9e7406e3112ee
ms.sourcegitcommit: 6d8c362a3cb4c634e64cbb3d28069c36bc973581
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418670"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="d50be-103">Настройка внешней учетной записи Google в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d50be-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="d50be-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="d50be-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d50be-105">В января 2019 Google начал [завершение работы](https://developers.google.com/+/api-shutdown) Google + вход и разработчикам необходимо переместить в новый входа Google в системе, марта.</span><span class="sxs-lookup"><span data-stu-id="d50be-105">In January 2019 Google started to [shut down](https://developers.google.com/+/api-shutdown) Google+ sign in and developers must move to a new Google sign in system by March.</span></span> <span data-ttu-id="d50be-106">ASP.NET Core 2.1 и 2.2 пакетов для проверки подлинности Google будут обновлены в феврале в соответствии с изменениями.</span><span class="sxs-lookup"><span data-stu-id="d50be-106">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication will be updated in February to accommodate the changes.</span></span> <span data-ttu-id="d50be-107">Дополнительные сведения и временные способы устранения рисков для ASP.NET Core, см. в разделе [проблема GitHub](https://github.com/aspnet/AspNetCore/issues/6486).</span><span class="sxs-lookup"><span data-stu-id="d50be-107">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="d50be-108">Этот учебник был обновлен с помощью нового процесса установки.</span><span class="sxs-lookup"><span data-stu-id="d50be-108">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="d50be-109">Этом руководстве показано, как разрешить пользователям выполнить вход с использованием своей учетной записью Google, с помощью ASP.NET Core 2.2 проект, созданный на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="d50be-109">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="d50be-110">Создать идентификатор проекта и клиент консоли Google API</span><span class="sxs-lookup"><span data-stu-id="d50be-110">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="d50be-111">Перейдите к [интеграция Google Sign-In в веб-приложения](https://developers.google.com/identity/sign-in/web/devconsole-project) и выберите **НАСТРОИТЬ ПРОЕКТ**.</span><span class="sxs-lookup"><span data-stu-id="d50be-111">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="d50be-112">В **настроить клиент OAuth** диалоговом окне выберите **веб-сервере**.</span><span class="sxs-lookup"><span data-stu-id="d50be-112">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="d50be-113">В **авторизованные URI перенаправления** текстовом поле, значение URI перенаправления.</span><span class="sxs-lookup"><span data-stu-id="d50be-113">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="d50be-114">Например `https://localhost:5001/signin-google` </span><span class="sxs-lookup"><span data-stu-id="d50be-114">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="d50be-115">Сохранить **идентификатор клиента** и **секрет клиента**.</span><span class="sxs-lookup"><span data-stu-id="d50be-115">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="d50be-116">При развертывании на сайте, зарегистрировать новый общедоступный URL-адрес из **консоли Google**.</span><span class="sxs-lookup"><span data-stu-id="d50be-116">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="d50be-117">Store Google ClientID и ClientSecret</span><span class="sxs-lookup"><span data-stu-id="d50be-117">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="d50be-118">Конфиденциальные параметры, такие как Google Store `Client ID` и `Client Secret` с [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d50be-118">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="d50be-119">В целях этого учебника назовите токены `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="d50be-119">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

<span data-ttu-id="d50be-120">Вы можете управлять ваши учетные данные API и использования в [консоль API](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="d50be-120">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="d50be-121">Настройка проверки подлинности Google</span><span class="sxs-lookup"><span data-stu-id="d50be-121">Configure Google authentication</span></span>

<span data-ttu-id="d50be-122">Добавить службу Google `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d50be-122">Add the Google service to `Startup.ConfigureServices`.</span></span>

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="d50be-123">Войдите с помощью Google</span><span class="sxs-lookup"><span data-stu-id="d50be-123">Sign in with Google</span></span>

* <span data-ttu-id="d50be-124">Запустите приложение и нажмите кнопку **вход**.</span><span class="sxs-lookup"><span data-stu-id="d50be-124">Run the app and click **Log in**.</span></span> <span data-ttu-id="d50be-125">Появится возможность войти с помощью Google.</span><span class="sxs-lookup"><span data-stu-id="d50be-125">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="d50be-126">Нажмите кнопку **Google** кнопку, которая перенаправляет Google для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d50be-126">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="d50be-127">После ввода учетных данных Google, вы будете перенаправлены к веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="d50be-127">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="d50be-128">См. в разделе [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) Справочник по API, Дополнительные сведения о параметрах конфигурации, поддерживается проверка подлинности Google.</span><span class="sxs-lookup"><span data-stu-id="d50be-128">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="d50be-129">Это может использоваться для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="d50be-129">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="d50be-130">Изменить URI обратного вызова по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d50be-130">Change the default callback URI</span></span>

<span data-ttu-id="d50be-131">Сегмент URI `/signin-google` задан в качестве обратного вызова по умолчанию поставщик проверки подлинности Google.</span><span class="sxs-lookup"><span data-stu-id="d50be-131">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="d50be-132">URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Google с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) класса.</span><span class="sxs-lookup"><span data-stu-id="d50be-132">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="d50be-133">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="d50be-133">Troubleshooting</span></span>

* <span data-ttu-id="d50be-134">Если вход не работает, а не получают все ошибки, переключитесь в режим разработки для упрощения процесса отладки проблемы.</span><span class="sxs-lookup"><span data-stu-id="d50be-134">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="d50be-135">Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, попытка проверки подлинности приводит к *ArgumentException: Необходимо указать параметр «SignInScheme»*.</span><span class="sxs-lookup"><span data-stu-id="d50be-135">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="d50be-136">Шаблон проекта, используемый в этом руководстве гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="d50be-136">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="d50be-137">Если база данных сайта не был создан путем применения первоначальной миграции, вы получаете *сбой операции из базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="d50be-137">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="d50be-138">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="d50be-138">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d50be-139">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="d50be-139">Next steps</span></span>

* <span data-ttu-id="d50be-140">В этой статье объясняется, как можно выполнить проверку подлинности с помощью Google.</span><span class="sxs-lookup"><span data-stu-id="d50be-140">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="d50be-141">Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="d50be-141">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="d50be-142">После того, как опубликовать приложение в Azure, сбросить `ClientSecret` в консоли Google API.</span><span class="sxs-lookup"><span data-stu-id="d50be-142">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="d50be-143">Задайте `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="d50be-143">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="d50be-144">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="d50be-144">The configuration system is set up to read keys from environment variables.</span></span>
