---
title: Настройка внешней учетной записи учетной записи Майкрософт с помощью ASP.NET Core
author: rick-anderson
description: В этом примере демонстрируется интеграция проверки подлинности пользователя учетной записи Майкрософт в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 2c690e5bd8465806d42091616917cfdd747ef8f0
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815575"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="ce7f8-103">Настройка внешней учетной записи учетной записи Майкрософт с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce7f8-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="ce7f8-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="ce7f8-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ce7f8-105">В этом примере показано, как разрешить пользователям выполнить вход с использованием своей учетной записью Майкрософт, с помощью ASP.NET Core 2.2 проект, созданный на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="ce7f8-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="ce7f8-106">Создание приложения на портале разработчика Майкрософт</span><span class="sxs-lookup"><span data-stu-id="ce7f8-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="ce7f8-107">Перейдите к [Azure портал регистрации приложений](https://go.microsoft.com/fwlink/?linkid=2083908) странице и создавать или входить в учетную запись Майкрософт:</span><span class="sxs-lookup"><span data-stu-id="ce7f8-107">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="ce7f8-108">Если у вас нет учетной записи Майкрософт, выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-108">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="ce7f8-109">После входа вы будете перенаправлены на **регистрация приложений** страницы:</span><span class="sxs-lookup"><span data-stu-id="ce7f8-109">After signing in you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="ce7f8-110">Выберите **Новая регистрация**</span><span class="sxs-lookup"><span data-stu-id="ce7f8-110">Select **New registration**</span></span>
* <span data-ttu-id="ce7f8-111">Введите **имя**.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-111">Enter a **Name**.</span></span>
* <span data-ttu-id="ce7f8-112">Выберите параметр для **поддерживаемые типы учетных записей**.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-112">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="ce7f8-113">В разделе **URI перенаправления**, введите URL-адрес разработки с `/signin-microsoft` добавляется.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-113">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="ce7f8-114">Например, `https://localhost:44389/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-114">For example, `https://localhost:44389/signin-microsoft`.</span></span> <span data-ttu-id="ce7f8-115">Схема проверки подлинности Майкрософт, Настройка описывается далее в этом образце автоматически будет обрабатывать запросы на `/signin-microsoft` маршрута, чтобы реализовать поток OAuth.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-115">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="ce7f8-116">Выберите **регистрации**</span><span class="sxs-lookup"><span data-stu-id="ce7f8-116">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="ce7f8-117">Создайте секрет клиента</span><span class="sxs-lookup"><span data-stu-id="ce7f8-117">Create client secret</span></span>

* <span data-ttu-id="ce7f8-118">В левой области выберите **сертификаты и секреты**.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-118">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="ce7f8-119">В разделе **секреты клиента**выберите **новый секрет клиента**</span><span class="sxs-lookup"><span data-stu-id="ce7f8-119">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="ce7f8-120">Добавьте описание секрет клиента.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-120">Add a description for the client secret.</span></span>
  * <span data-ttu-id="ce7f8-121">Выберите **добавить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-121">Select the **Add** button.</span></span>

* <span data-ttu-id="ce7f8-122">В разделе **секреты клиента**, скопируйте значение секрета клиента.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-122">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="ce7f8-123">Сегмент URI `/signin-microsoft` задан в качестве обратного вызова по умолчанию поставщика проверки подлинности Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="ce7f8-124">URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Майкрософт с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) класса.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="ce7f8-125">Store Microsoft клиента идентификатор и секрет клиента</span><span class="sxs-lookup"><span data-stu-id="ce7f8-125">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="ce7f8-126">Выполните следующие команды, чтобы обеспечить безопасное хранение `ClientId` и `ClientSecret` с помощью [Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="ce7f8-126">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="ce7f8-127">Связать конфиденциальные параметры, такие как Microsoft `ClientId` и `ClientSecret` для конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="ce7f8-127">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="ce7f8-128">В целях этого примера назовите токены `Authentication:Microsoft:ClientId` и `Authentication:Microsoft:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-128">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="ce7f8-129">Настройка проверки подлинности учетной записи Майкрософт</span><span class="sxs-lookup"><span data-stu-id="ce7f8-129">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="ce7f8-130">Добавить учетную запись Майкрософт службу `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ce7f8-130">Add the Microsoft Account service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="ce7f8-131">См. в разделе [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Справочник по API, Дополнительные сведения о параметрах конфигурации, поддерживается проверка подлинности учетной записи Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-131">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="ce7f8-132">Это может использоваться для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-132">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="ce7f8-133">Войдите с помощью учетной записи Майкрософт</span><span class="sxs-lookup"><span data-stu-id="ce7f8-133">Sign in with Microsoft Account</span></span>

<span data-ttu-id="ce7f8-134">Запустите и нажмите кнопку **вход**.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-134">Run the and click **Log in**.</span></span> <span data-ttu-id="ce7f8-135">Появится возможность входа с учетной записью Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-135">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="ce7f8-136">Если щелкнуть on Microsoft, вы будете перенаправлены в корпорацию Майкрософт для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-136">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="ce7f8-137">После входа под учетной записью Майкрософт (если это не сделано) вам будет предложено разрешить приложению доступ к вашим сведениям:</span><span class="sxs-lookup"><span data-stu-id="ce7f8-137">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="ce7f8-138">Коснитесь **Да** и вы будете перенаправлены обратно на веб-узел, где вы можете задать свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-138">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="ce7f8-139">Теперь вы вошли с использованием учетных данных Майкрософт:</span><span class="sxs-lookup"><span data-stu-id="ce7f8-139">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="ce7f8-140">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="ce7f8-140">Troubleshooting</span></span>

* <span data-ttu-id="ce7f8-141">Если поставщик учетной записи Майкрософт вы будете перенаправлены к странице ошибки входа, обратите внимание, ошибка заголовок и описание параметров строки запроса сразу после `#` (хэштег) в Uri.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-141">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="ce7f8-142">Несмотря на то, что сообщение об ошибке кажется указывают на проблему с использованием проверки подлинности, наиболее распространенной причиной является Uri, не соответствующих ни одному из приложения **URI перенаправления** для **Web** платформы .</span><span class="sxs-lookup"><span data-stu-id="ce7f8-142">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="ce7f8-143">Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: Необходимо указать параметр «SignInScheme»* .</span><span class="sxs-lookup"><span data-stu-id="ce7f8-143">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="ce7f8-144">Шаблон проекта, используемый в этом примере гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-144">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="ce7f8-145">Если база данных сайта не был создан путем применения первоначальной миграции, вы получите *сбой операции из базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-145">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="ce7f8-146">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-146">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ce7f8-147">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="ce7f8-147">Next steps</span></span>

* <span data-ttu-id="ce7f8-148">В этой статье объясняется, как можно выполнить проверку подлинности с корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-148">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="ce7f8-149">Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="ce7f8-149">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="ce7f8-150">После публикации веб-сайт веб-приложение Azure, создание нового клиента секретов на портале разработчика Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-150">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="ce7f8-151">Задайте `Authentication:Microsoft:ClientId` и `Authentication:Microsoft:ClientSecret` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-151">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="ce7f8-152">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="ce7f8-152">The configuration system is set up to read keys from environment variables.</span></span>