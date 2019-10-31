---
title: Настройка внешнего входа Google в ASP.NET Core
author: rick-anderson
description: В этом руководстве демонстрируется Интеграция проверки подлинности пользователя учетной записи Google с существующим ASP.NET Core приложением.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/30/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 83f45143eca1be43410880bfd875a3fce1d2e9c9
ms.sourcegitcommit: de0fc77487a4d342bcc30965ec5c142d10d22c03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2019
ms.locfileid: "73143455"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="d56f7-103">Настройка внешнего входа Google в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d56f7-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="d56f7-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="d56f7-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d56f7-105">В этом руководстве показано, как разрешить пользователям входить в систему с помощью учетной записи Google, используя проект ASP.NET Core 3,0, созданный на [предыдущей странице](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="d56f7-105">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="d56f7-106">Создание проекта консоли Google API и идентификатора клиента</span><span class="sxs-lookup"><span data-stu-id="d56f7-106">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="d56f7-107">Установите [Microsoft. AspNetCore. Authentication. Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google).</span><span class="sxs-lookup"><span data-stu-id="d56f7-107">Install [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google).</span></span>
* <span data-ttu-id="d56f7-108">Перейдите к разделу [Интеграция Google-входа в веб-приложение](https://developers.google.com/identity/sign-in/web/devconsole-project) и выберите **Настройка проекта**.</span><span class="sxs-lookup"><span data-stu-id="d56f7-108">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="d56f7-109">В диалоговом окне **Настройка клиента OAuth** выберите **веб-сервер**.</span><span class="sxs-lookup"><span data-stu-id="d56f7-109">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="d56f7-110">В текстовом поле " **зарегистрированные URI перенаправления** " задайте URI перенаправления.</span><span class="sxs-lookup"><span data-stu-id="d56f7-110">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="d56f7-111">Например: `https://localhost:44312/signin-google`</span><span class="sxs-lookup"><span data-stu-id="d56f7-111">For example, `https://localhost:44312/signin-google`</span></span>
* <span data-ttu-id="d56f7-112">Сохраните **идентификатор клиента** и **секрет клиента**.</span><span class="sxs-lookup"><span data-stu-id="d56f7-112">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="d56f7-113">При развертывании сайта зарегистрируйте новый общедоступный URL-адрес из **консоли Google**.</span><span class="sxs-lookup"><span data-stu-id="d56f7-113">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="d56f7-114">Хранение Google ClientID и ClientSecret</span><span class="sxs-lookup"><span data-stu-id="d56f7-114">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="d56f7-115">Храните конфиденциальные параметры, такие как Google `Client ID` и `Client Secret` с [диспетчером секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d56f7-115">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="d56f7-116">В рамках этого руководства назовите маркеры `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="d56f7-116">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "<client id>"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="d56f7-117">Вы можете управлять учетными данными и использованием API в [консоли API](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="d56f7-117">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="d56f7-118">Настройка проверки подлинности Google</span><span class="sxs-lookup"><span data-stu-id="d56f7-118">Configure Google authentication</span></span>

<span data-ttu-id="d56f7-119">Добавьте службу Google в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d56f7-119">Add the Google service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupGoogle3x.cs?highlight=11-19)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="d56f7-120">Вход с помощью Google</span><span class="sxs-lookup"><span data-stu-id="d56f7-120">Sign in with Google</span></span>

* <span data-ttu-id="d56f7-121">Запустите приложение и нажмите кнопку **войти**.</span><span class="sxs-lookup"><span data-stu-id="d56f7-121">Run the app and click **Log in**.</span></span> <span data-ttu-id="d56f7-122">Появится возможность войти в систему с помощью Google.</span><span class="sxs-lookup"><span data-stu-id="d56f7-122">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="d56f7-123">Нажмите кнопку **Google** , которая перенаправляет на Google для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d56f7-123">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="d56f7-124">После ввода учетных данных Google вы будете перенаправлены обратно на веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="d56f7-124">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="d56f7-125">Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности Google, см. в справочнике по API <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>.</span><span class="sxs-lookup"><span data-stu-id="d56f7-125">See the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="d56f7-126">Это можно использовать для запроса различных сведений о пользователе.</span><span class="sxs-lookup"><span data-stu-id="d56f7-126">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="d56f7-127">Изменение URI обратного вызова по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d56f7-127">Change the default callback URI</span></span>

<span data-ttu-id="d56f7-128">Сегмент URI `/signin-google` задается в качестве обратного вызова по умолчанию для поставщика проверки подлинности Google.</span><span class="sxs-lookup"><span data-stu-id="d56f7-128">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="d56f7-129">Вы можете изменить URI обратного вызова по умолчанию при настройке по промежуточного слоя для проверки подлинности Google с помощью унаследованного свойства [ремотеаусентикатионоптионс. каллбаккпас](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) класса [гуглеоптионс](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) .</span><span class="sxs-lookup"><span data-stu-id="d56f7-129">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="d56f7-130">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="d56f7-130">Troubleshooting</span></span>

* <span data-ttu-id="d56f7-131">Если вход не работает и вы не получаете никаких ошибок, переключитесь в режим разработки, чтобы упростить отладку.</span><span class="sxs-lookup"><span data-stu-id="d56f7-131">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="d56f7-132">Если удостоверение не настроено путем вызова `services.AddIdentity` в `ConfigureServices`, попытка проверить подлинность результатов в *ArgumentException: необходимо указать параметр "сигнинсчеме"* .</span><span class="sxs-lookup"><span data-stu-id="d56f7-132">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="d56f7-133">Шаблон проекта, используемый в этом руководстве, гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="d56f7-133">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="d56f7-134">Если база данных сайта не была создана путем применения первоначальной миграции, то при обработке ошибки запроса возникнет *Ошибка операции с базой данных* .</span><span class="sxs-lookup"><span data-stu-id="d56f7-134">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="d56f7-135">Выберите **Применить миграции** , чтобы создать базу данных, и обновите страницу, чтобы продолжить работу после ошибки.</span><span class="sxs-lookup"><span data-stu-id="d56f7-135">Select **Apply Migrations** to create the database, and refresh the page to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d56f7-136">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="d56f7-136">Next steps</span></span>

* <span data-ttu-id="d56f7-137">В этой статье показано, как можно пройти проверку подлинности в Google.</span><span class="sxs-lookup"><span data-stu-id="d56f7-137">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="d56f7-138">Аналогичный подход можно использовать для проверки подлинности с другими поставщиками, перечисленными на [предыдущей странице](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="d56f7-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="d56f7-139">После публикации приложения в Azure сбросьте `ClientSecret` в консоли Google API.</span><span class="sxs-lookup"><span data-stu-id="d56f7-139">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="d56f7-140">Задайте `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret` в качестве параметров приложения в портал Azure.</span><span class="sxs-lookup"><span data-stu-id="d56f7-140">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="d56f7-141">Система конфигурации настроена на чтение ключей из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="d56f7-141">The configuration system is set up to read keys from environment variables.</span></span>
