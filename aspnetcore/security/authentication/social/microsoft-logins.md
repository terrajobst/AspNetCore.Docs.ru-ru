---
title: Настройка внешнего входа учетной записи Майкрософт с ASP.NET Core
author: rick-anderson
description: В этом примере демонстрируется интеграция учетная запись Майкрософт проверки подлинности пользователей в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 91ace293fd16cd180b3d5c183c637af6db1d08c3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082343"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="5006c-103">Настройка внешнего входа учетной записи Майкрософт с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5006c-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="5006c-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="5006c-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5006c-105">В этом примере показано, как разрешить пользователям входить с помощью учетная запись Майкрософт с помощью проекта ASP.NET Core 2,2, созданного на [предыдущей странице](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="5006c-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="5006c-106">Создание приложения на портале разработчика Майкрософт</span><span class="sxs-lookup"><span data-stu-id="5006c-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="5006c-107">Перейдите на страницу [портал Azure-регистрация приложений](https://go.microsoft.com/fwlink/?linkid=2083908) и создайте или войдите в учетная запись Майкрософт:</span><span class="sxs-lookup"><span data-stu-id="5006c-107">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="5006c-108">Если у вас нет учетная запись Майкрософт, выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="5006c-108">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="5006c-109">После входа вы будете перенаправлены на **Регистрация приложений** страницу:</span><span class="sxs-lookup"><span data-stu-id="5006c-109">After signing in you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="5006c-110">Выбрать **новую регистрацию**</span><span class="sxs-lookup"><span data-stu-id="5006c-110">Select **New registration**</span></span>
* <span data-ttu-id="5006c-111">Введите **имя**.</span><span class="sxs-lookup"><span data-stu-id="5006c-111">Enter a **Name**.</span></span>
* <span data-ttu-id="5006c-112">Выберите вариант для **поддерживаемых типов учетных записей**.</span><span class="sxs-lookup"><span data-stu-id="5006c-112">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="5006c-113">В разделе **URI перенаправления**введите URL-адрес `/signin-microsoft` разработки с добавлением.</span><span class="sxs-lookup"><span data-stu-id="5006c-113">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="5006c-114">Например, `https://localhost:44389/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="5006c-114">For example, `https://localhost:44389/signin-microsoft`.</span></span> <span data-ttu-id="5006c-115">Схема проверки подлинности Майкрософт, настроенная далее в этом примере, `/signin-microsoft` автоматически обрабатывает запросы по маршруту для реализации потока OAuth.</span><span class="sxs-lookup"><span data-stu-id="5006c-115">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="5006c-116">Выбор **регистра**</span><span class="sxs-lookup"><span data-stu-id="5006c-116">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="5006c-117">Создать секрет клиента</span><span class="sxs-lookup"><span data-stu-id="5006c-117">Create client secret</span></span>

* <span data-ttu-id="5006c-118">В левой области выберите **сертификаты & секреты**.</span><span class="sxs-lookup"><span data-stu-id="5006c-118">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="5006c-119">В разделе **секреты клиента**выберите **новый секрет клиента** .</span><span class="sxs-lookup"><span data-stu-id="5006c-119">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="5006c-120">Добавьте описание секрета клиента.</span><span class="sxs-lookup"><span data-stu-id="5006c-120">Add a description for the client secret.</span></span>
  * <span data-ttu-id="5006c-121">Нажмите кнопку **Добавить** .</span><span class="sxs-lookup"><span data-stu-id="5006c-121">Select the **Add** button.</span></span>

* <span data-ttu-id="5006c-122">В разделе **секреты клиента**скопируйте значение секрета клиента.</span><span class="sxs-lookup"><span data-stu-id="5006c-122">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="5006c-123">Сегмент `/signin-microsoft` URI задается в качестве обратного вызова по умолчанию для поставщика проверки подлинности Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="5006c-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="5006c-124">URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Майкрософт с помощью унаследованного свойства [ремотеаусентикатионоптионс. каллбаккпас](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) класса [микрософтаккаунтоптионс](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) .</span><span class="sxs-lookup"><span data-stu-id="5006c-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="5006c-125">Хранение идентификатора клиента и секрета клиента Майкрософт</span><span class="sxs-lookup"><span data-stu-id="5006c-125">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="5006c-126">Выполните следующие команды для безопасного хранения `ClientId` и `ClientSecret` использования [диспетчера секретов](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="5006c-126">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="5006c-127">Свяжите конфиденциальные параметры, `ClientId` такие `ClientSecret` как Майкрософт и с конфигурацией приложения, с помощью [диспетчера секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="5006c-127">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="5006c-128">Для целей этого примера назовите маркеры `Authentication:Microsoft:ClientId` и. `Authentication:Microsoft:ClientSecret`</span><span class="sxs-lookup"><span data-stu-id="5006c-128">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="5006c-129">Настройка проверки подлинности учетной записи Майкрософт</span><span class="sxs-lookup"><span data-stu-id="5006c-129">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="5006c-130">Добавьте службу учетных записей Майкрософт `Startup.ConfigureServices`в:</span><span class="sxs-lookup"><span data-stu-id="5006c-130">Add the Microsoft Account service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="5006c-131">Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности учетной записи Майкрософт, см. в справочнике по API [микрософтаккаунтоптионс](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) .</span><span class="sxs-lookup"><span data-stu-id="5006c-131">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="5006c-132">Это может использоваться для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="5006c-132">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="5006c-133">Учетная запись Войдите с помощью учетной записи Майкрософт</span><span class="sxs-lookup"><span data-stu-id="5006c-133">Sign in with Microsoft Account</span></span>

<span data-ttu-id="5006c-134">Запустите и щелкните **Вход**.</span><span class="sxs-lookup"><span data-stu-id="5006c-134">Run the and click **Log in**.</span></span> <span data-ttu-id="5006c-135">Появится возможность войти в систему с помощью Microsoft.</span><span class="sxs-lookup"><span data-stu-id="5006c-135">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="5006c-136">Если щелкнуть Майкрософт, вы будете перенаправлены в корпорацию Майкрософт для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5006c-136">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="5006c-137">После входа в систему с помощью учетной записи Майкрософт (если она еще не выполнена) вам будет предложено предоставить приложению доступ к вашим сведениям:</span><span class="sxs-lookup"><span data-stu-id="5006c-137">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="5006c-138">Коснитесь кнопки **Да** , и вы будете перенаправлены на веб-сайт, на котором можно задать свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="5006c-138">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="5006c-139">Теперь вы выполнили вход с использованием учетных данных Майкрософт:</span><span class="sxs-lookup"><span data-stu-id="5006c-139">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="5006c-140">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="5006c-140">Troubleshooting</span></span>

* <span data-ttu-id="5006c-141">Если поставщик учетных записей Майкрософт перенаправит вас на страницу ошибки входа, обратите внимание на заголовок ошибки и описание параметры строки запроса непосредственно после `#` (хэш-кода) в универсальном коде ресурса (URI).</span><span class="sxs-lookup"><span data-stu-id="5006c-141">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="5006c-142">Несмотря на то, что сообщение об ошибке указывает на проблему с проверкой подлинности Майкрософт, наиболее распространенной причиной является URI приложения, не совпадающий ни с одним из **URI перенаправления** , указанных для **веб-** платформы.</span><span class="sxs-lookup"><span data-stu-id="5006c-142">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="5006c-143">Если удостоверение не настроено `services.AddIdentity` с `ConfigureServices`помощью вызова в, попытка проверки подлинности приведет *к появлению исключения ArgumentException: Необходимо указать*параметр "сигнинсчеме".</span><span class="sxs-lookup"><span data-stu-id="5006c-143">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="5006c-144">Шаблон проекта, используемый в этом образце, гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="5006c-144">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="5006c-145">Если база данных сайта не был создан путем применения первоначальной миграции, вы получите *сбой операции из базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="5006c-145">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="5006c-146">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="5006c-146">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5006c-147">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="5006c-147">Next steps</span></span>

* <span data-ttu-id="5006c-148">В этой статье показано, как можно пройти проверку подлинности в Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="5006c-148">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="5006c-149">Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="5006c-149">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="5006c-150">После публикации веб-сайта в веб-приложение Azure создайте секретные данные клиента на портале разработчика Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="5006c-150">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="5006c-151">Задайте `Authentication:Microsoft:ClientId` и `Authentication:Microsoft:ClientSecret` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="5006c-151">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="5006c-152">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="5006c-152">The configuration system is set up to read keys from environment variables.</span></span>