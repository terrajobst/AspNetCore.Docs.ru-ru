---
title: Настройка внешнего входа учетной записи Майкрософт с ASP.NET Core
author: rick-anderson
description: В этом примере демонстрируется интеграция учетная запись Майкрософт проверки подлинности пользователей в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/4/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/microsoft-logins
ms.openlocfilehash: ddaae1a25a1dcf167ffae0f24b480e2cde6aca5b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652072"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="a9335-103">Настройка внешнего входа учетной записи Майкрософт с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9335-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="a9335-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="a9335-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a9335-105">В этом примере показано, как разрешить пользователям входить с помощью учетная запись Майкрософт с помощью проекта ASP.NET Core 3,0, созданного на [предыдущей странице](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="a9335-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="a9335-106">Создание приложения на портале разработчика Майкрософт</span><span class="sxs-lookup"><span data-stu-id="a9335-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="a9335-107">Добавьте в проект пакет NuGet [Microsoft. AspNetCore. Authentication. MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) .</span><span class="sxs-lookup"><span data-stu-id="a9335-107">Add the [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) NuGet package to the project.</span></span>
* <span data-ttu-id="a9335-108">Перейдите на страницу [портал Azure-регистрация приложений](https://go.microsoft.com/fwlink/?linkid=2083908) и создайте или войдите в учетная запись Майкрософт:</span><span class="sxs-lookup"><span data-stu-id="a9335-108">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="a9335-109">Если у вас нет учетная запись Майкрософт, выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="a9335-109">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="a9335-110">После входа вы будете перенаправлены на **Регистрация приложений** страницу:</span><span class="sxs-lookup"><span data-stu-id="a9335-110">After signing in, you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="a9335-111">Выбрать **новую регистрацию**</span><span class="sxs-lookup"><span data-stu-id="a9335-111">Select **New registration**</span></span>
* <span data-ttu-id="a9335-112">Введите **Имя**.</span><span class="sxs-lookup"><span data-stu-id="a9335-112">Enter a **Name**.</span></span>
* <span data-ttu-id="a9335-113">Выберите вариант для **поддерживаемых типов учетных записей**.</span><span class="sxs-lookup"><span data-stu-id="a9335-113">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="a9335-114">В разделе **URI перенаправления**введите URL-адрес разработки с добавленным `/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="a9335-114">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="a9335-115">Например, `https://localhost:5001/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="a9335-115">For example, `https://localhost:5001/signin-microsoft`.</span></span> <span data-ttu-id="a9335-116">Схема проверки подлинности Майкрософт, настроенная далее в этом примере, автоматически обрабатывает запросы на `/signin-microsoft` маршруте для реализации потока OAuth.</span><span class="sxs-lookup"><span data-stu-id="a9335-116">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="a9335-117">Выбор **регистра**</span><span class="sxs-lookup"><span data-stu-id="a9335-117">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="a9335-118">Создать секрет клиента</span><span class="sxs-lookup"><span data-stu-id="a9335-118">Create client secret</span></span>

* <span data-ttu-id="a9335-119">В левой области выберите **сертификаты & секреты**.</span><span class="sxs-lookup"><span data-stu-id="a9335-119">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="a9335-120">В разделе **секреты клиента**выберите **новый секрет клиента** .</span><span class="sxs-lookup"><span data-stu-id="a9335-120">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="a9335-121">Добавьте описание секрета клиента.</span><span class="sxs-lookup"><span data-stu-id="a9335-121">Add a description for the client secret.</span></span>
  * <span data-ttu-id="a9335-122">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a9335-122">Select the **Add** button.</span></span>

* <span data-ttu-id="a9335-123">В разделе **секреты клиента**скопируйте значение секрета клиента.</span><span class="sxs-lookup"><span data-stu-id="a9335-123">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="a9335-124">Сегмент URI `/signin-microsoft` задается в качестве обратного вызова по умолчанию для поставщика проверки подлинности Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="a9335-124">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="a9335-125">URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Майкрософт с помощью унаследованного свойства [ремотеаусентикатионоптионс. каллбаккпас](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) класса [микрософтаккаунтоптионс](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) .</span><span class="sxs-lookup"><span data-stu-id="a9335-125">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="a9335-126">Хранение идентификатора клиента и секрета клиента Майкрософт</span><span class="sxs-lookup"><span data-stu-id="a9335-126">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="a9335-127">Выполните следующие команды для безопасного хранения `ClientId` и `ClientSecret` с помощью [диспетчера секретов](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="a9335-127">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="a9335-128">Свяжите конфиденциальные параметры, такие как Microsoft `ClientId` и `ClientSecret` с конфигурацией приложения с помощью [диспетчера секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="a9335-128">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="a9335-129">Для целей этого примера назовите маркеры `Authentication:Microsoft:ClientId` и `Authentication:Microsoft:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="a9335-129">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="a9335-130">Настройка проверки подлинности учетной записи Майкрософт</span><span class="sxs-lookup"><span data-stu-id="a9335-130">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="a9335-131">Добавьте службу учетных записей Майкрософт в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a9335-131">Add the Microsoft Account service to the `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupMS3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="a9335-132">Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности учетной записи Майкрософт, см. в справочнике по API [микрософтаккаунтоптионс](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) .</span><span class="sxs-lookup"><span data-stu-id="a9335-132">For more information about configuration options supported by Microsoft Account authentication, see the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference.</span></span> <span data-ttu-id="a9335-133">Это может использоваться для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="a9335-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="a9335-134">Учетная запись Войдите с помощью учетной записи Майкрософт</span><span class="sxs-lookup"><span data-stu-id="a9335-134">Sign in with Microsoft Account</span></span>

<span data-ttu-id="a9335-135">Запустите приложение и нажмите кнопку **войти**.</span><span class="sxs-lookup"><span data-stu-id="a9335-135">Run the app and click **Log in**.</span></span> <span data-ttu-id="a9335-136">Появится возможность войти в систему с помощью Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a9335-136">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="a9335-137">Если щелкнуть Майкрософт, вы будете перенаправлены в корпорацию Майкрософт для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a9335-137">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="a9335-138">После входа с помощью учетной записи Майкрософт вам будет предложено предоставить приложению доступ к вашим сведениям:</span><span class="sxs-lookup"><span data-stu-id="a9335-138">After signing in with your Microsoft Account, you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="a9335-139">Коснитесь кнопки **Да** , и вы будете перенаправлены на веб-сайт, на котором можно задать свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="a9335-139">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="a9335-140">Теперь вы выполнили вход с использованием учетных данных Майкрософт:</span><span class="sxs-lookup"><span data-stu-id="a9335-140">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="a9335-141">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="a9335-141">Troubleshooting</span></span>

* <span data-ttu-id="a9335-142">Если поставщик учетных записей Майкрософт перенаправит вас на страницу ошибки входа, обратите внимание на заголовок ошибки и описание параметры строки запроса непосредственно после `#` (хэш-кода) в универсальном коде ресурса (URI).</span><span class="sxs-lookup"><span data-stu-id="a9335-142">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="a9335-143">Несмотря на то, что сообщение об ошибке указывает на проблему с проверкой подлинности Майкрософт, наиболее распространенной причиной является URI приложения, не совпадающий ни с одним из **URI перенаправления** , указанных для **веб-** платформы.</span><span class="sxs-lookup"><span data-stu-id="a9335-143">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="a9335-144">Если удостоверение не настроено путем вызова `services.AddIdentity` в `ConfigureServices`, попытка проверки подлинности приведет к появлению *исключения ArgumentException: необходимо указать параметр "сигнинсчеме"* .</span><span class="sxs-lookup"><span data-stu-id="a9335-144">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="a9335-145">Шаблон проекта, используемый в этом образце, гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="a9335-145">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="a9335-146">Если база данных сайта не была создана с применением первоначальной миграции, то при обработке ошибки запроса возникнет *Ошибка операции с базой данных* .</span><span class="sxs-lookup"><span data-stu-id="a9335-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="a9335-147">Выберите **Применить миграции** , чтобы создать базу данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="a9335-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9335-148">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="a9335-148">Next steps</span></span>

* <span data-ttu-id="a9335-149">В этой статье показано, как можно пройти проверку подлинности в Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="a9335-149">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="a9335-150">Аналогичный подход можно использовать для проверки подлинности с другими поставщиками, перечисленными на [предыдущей странице](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="a9335-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="a9335-151">После публикации веб-сайта в веб-приложение Azure создайте секретные данные клиента на портале разработчика Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="a9335-151">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="a9335-152">Задайте `Authentication:Microsoft:ClientId` и `Authentication:Microsoft:ClientSecret` в качестве параметров приложения в портал Azure.</span><span class="sxs-lookup"><span data-stu-id="a9335-152">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="a9335-153">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="a9335-153">The configuration system is set up to read keys from environment variables.</span></span>
