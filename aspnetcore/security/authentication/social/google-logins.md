---
title: Настройка внешней учетной записи Google в ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграцию Google учетной записи пользователя и проверки подлинности в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: e12d831d2e0a5c9acae5ea41fb4187ad4ca6b0ea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082484"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="8b3a7-103">Настройка внешней учетной записи Google в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b3a7-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="8b3a7-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="8b3a7-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8b3a7-105">[Устаревшие API-интерфейсы Google + были закрыты до 7 марта 2019 г](https://developers.google.com/+/api-shutdown).</span><span class="sxs-lookup"><span data-stu-id="8b3a7-105">[Legacy Google+ APIs have been shut down as of March 7, 2019](https://developers.google.com/+/api-shutdown).</span></span> <span data-ttu-id="8b3a7-106">Вход в Google + и разработчики должны перейти на новую систему входа Google.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-106">Google+ sign in and developers must move to a new Google sign in system.</span></span> <span data-ttu-id="8b3a7-107">Пакеты ASP.NET Core 2,1 и 2,2 для проверки подлинности Google обновлены в соответствии с изменениями.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-107">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication have be updated to accommodate the changes.</span></span> <span data-ttu-id="8b3a7-108">Дополнительные сведения и временные меры для ASP.NET Core см. в [этой статье о проблемах GitHub](https://github.com/aspnet/AspNetCore/issues/6486).</span><span class="sxs-lookup"><span data-stu-id="8b3a7-108">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="8b3a7-109">Этот учебник был обновлен с учетом нового процесса установки.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-109">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="8b3a7-110">В этом руководстве показано, как разрешить пользователям входить в систему с помощью учетной записи Google, используя проект ASP.NET Core 2,2, созданный на [предыдущей странице](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8b3a7-110">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="8b3a7-111">Создание проекта консоли Google API и идентификатора клиента</span><span class="sxs-lookup"><span data-stu-id="8b3a7-111">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="8b3a7-112">Перейдите к разделу [Интеграция Google-входа в веб-приложение](https://developers.google.com/identity/sign-in/web/devconsole-project) и выберите **Настройка проекта**.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-112">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="8b3a7-113">В диалоговом окне **Настройка клиента OAuth** выберите **веб-сервер**.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-113">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="8b3a7-114">В текстовом поле " **зарегистрированные URI перенаправления** " задайте URI перенаправления.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-114">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="8b3a7-115">Например: `https://localhost:5001/signin-google`</span><span class="sxs-lookup"><span data-stu-id="8b3a7-115">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="8b3a7-116">Сохраните **идентификатор клиента** и **секрет клиента**.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-116">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="8b3a7-117">При развертывании сайта зарегистрируйте новый общедоступный URL-адрес из **консоли Google**.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-117">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="8b3a7-118">Store Google ClientID и ClientSecret</span><span class="sxs-lookup"><span data-stu-id="8b3a7-118">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="8b3a7-119">Храните конфиденциальные параметры, такие как `Client ID` Google `Client Secret` и [Диспетчер секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="8b3a7-119">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="8b3a7-120">В рамках этого руководства назовите маркеры `Authentication:Google:ClientId` и: `Authentication:Google:ClientSecret`</span><span class="sxs-lookup"><span data-stu-id="8b3a7-120">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="8b3a7-121">Вы можете управлять учетными данными и использованием API в [консоли API](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="8b3a7-121">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="8b3a7-122">Настройка проверки подлинности Google</span><span class="sxs-lookup"><span data-stu-id="8b3a7-122">Configure Google authentication</span></span>

<span data-ttu-id="8b3a7-123">Добавьте службу Google в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8b3a7-123">Add the Google service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="8b3a7-124">Войдите с помощью Google</span><span class="sxs-lookup"><span data-stu-id="8b3a7-124">Sign in with Google</span></span>

* <span data-ttu-id="8b3a7-125">Запустите приложение и нажмите кнопку **войти**.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-125">Run the app and click **Log in**.</span></span> <span data-ttu-id="8b3a7-126">Появится возможность войти в систему с помощью Google.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-126">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="8b3a7-127">Нажмите кнопку **Google** , которая перенаправляет на Google для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-127">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="8b3a7-128">После ввода учетных данных Google вы будете перенаправлены обратно на веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-128">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="8b3a7-129">Дополнительные сведения <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> о параметрах конфигурации, поддерживаемых проверкой подлинности Google, см. в справочнике по API.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-129">See the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="8b3a7-130">Это может использоваться для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-130">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="8b3a7-131">Изменение URI обратного вызова по умолчанию</span><span class="sxs-lookup"><span data-stu-id="8b3a7-131">Change the default callback URI</span></span>

<span data-ttu-id="8b3a7-132">Сегмент URI `/signin-google` задан в качестве обратного вызова по умолчанию поставщик проверки подлинности Google.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-132">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="8b3a7-133">URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Google с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) класса.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-133">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="8b3a7-134">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="8b3a7-134">Troubleshooting</span></span>

* <span data-ttu-id="8b3a7-135">Если вход не работает и вы не получаете никаких ошибок, переключитесь в режим разработки, чтобы упростить отладку.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-135">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="8b3a7-136">Если удостоверение не настроено `services.AddIdentity` с `ConfigureServices`помощью вызова в, попытка проверить *подлинность результатов в ArgumentException: Необходимо указать*параметр "сигнинсчеме".</span><span class="sxs-lookup"><span data-stu-id="8b3a7-136">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="8b3a7-137">Шаблон проекта, используемый в этом руководстве гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-137">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="8b3a7-138">Если база данных сайта не был создан путем применения первоначальной миграции, вы получаете *сбой операции из базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-138">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="8b3a7-139">Выберите **Применить миграции** , чтобы создать базу данных, и обновите страницу, чтобы продолжить работу после ошибки.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-139">Select **Apply Migrations** to create the database, and refresh the page to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8b3a7-140">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="8b3a7-140">Next steps</span></span>

* <span data-ttu-id="8b3a7-141">В этой статье объясняется, как можно выполнить проверку подлинности с помощью Google.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-141">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="8b3a7-142">Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8b3a7-142">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="8b3a7-143">После публикации приложения в Azure сбросьте его `ClientSecret` в консоли Google API.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-143">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="8b3a7-144">Задайте `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-144">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="8b3a7-145">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="8b3a7-145">The configuration system is set up to read keys from environment variables.</span></span>
