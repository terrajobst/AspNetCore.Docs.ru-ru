---
title: Настройка внешнего входа в Twitter с помощью ASP.NET Core
author: rick-anderson
description: В этом руководстве демонстрируется интеграция аутентификации пользователя учетной записи Twitter с существующим ASP.NET Core приложением.
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: b848486415fd72ce6180b4cf8fc1ba00410d694a
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989738"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="57b1c-103">Настройка внешнего входа в Twitter с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="57b1c-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="57b1c-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="57b1c-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="57b1c-105">В этом примере показано, как разрешить пользователям [входить с помощью учетной записи Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) , используя пример проекта ASP.NET Core 3,0, созданный на [предыдущей странице](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="57b1c-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="57b1c-106">Создание приложения в Twitter</span><span class="sxs-lookup"><span data-stu-id="57b1c-106">Create the app in Twitter</span></span>

* <span data-ttu-id="57b1c-107">Добавьте в проект пакет NuGet [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) .</span><span class="sxs-lookup"><span data-stu-id="57b1c-107">Add the [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet package to the project.</span></span>

* <span data-ttu-id="57b1c-108">Перейдите к [https://apps.twitter.com/](https://apps.twitter.com/) и выполните вход.</span><span class="sxs-lookup"><span data-stu-id="57b1c-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="57b1c-109">Если у вас еще нет учетной записи Twitter, используйте ссылку **[зарегистрироваться сейчас](https://twitter.com/signup)** , чтобы создать ее.</span><span class="sxs-lookup"><span data-stu-id="57b1c-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="57b1c-110">Выберите **Create an app** (Создать приложение).</span><span class="sxs-lookup"><span data-stu-id="57b1c-110">Select **Create an app**.</span></span> <span data-ttu-id="57b1c-111">Укажите **имя приложения**, **Описание приложения** и универсальный код ресурса (URI) общедоступного **веб-сайта** (это может быть временным, пока не будет зарегистрировано доменное имя):</span><span class="sxs-lookup"><span data-stu-id="57b1c-111">Fill out the **App name**, **Application description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="57b1c-112">Установите флажок **включить вход с помощью Twitter** .</span><span class="sxs-lookup"><span data-stu-id="57b1c-112">Check the box next to **Enable Sign in with Twitter**</span></span>

* <span data-ttu-id="57b1c-113">Microsoft. AspNetCore. Identity требует, чтобы по умолчанию у пользователей был адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="57b1c-113">Microsoft.AspNetCore.Identity requires users to have an email address by default.</span></span> <span data-ttu-id="57b1c-114">Перейдите на вкладку **разрешения** , нажмите кнопку " **изменить** " и установите флажок " **запросить адрес электронной почты у пользователей**".</span><span class="sxs-lookup"><span data-stu-id="57b1c-114">Go to the **Permissions** tab, click the **Edit** button and check the box next to **Request email address from users**.</span></span>

* <span data-ttu-id="57b1c-115">Введите универсальный код ресурса (URI) для разработки `/signin-twitter` добавлен в поле **URL-адреса обратного вызова** (например: `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="57b1c-115">Enter your development URI with `/signin-twitter` appended into the **Callback URLs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="57b1c-116">Схема проверки подлинности Twitter, настроенная далее в этом примере, автоматически обрабатывает запросы на `/signin-twitter` маршруте для реализации потока OAuth.</span><span class="sxs-lookup"><span data-stu-id="57b1c-116">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="57b1c-117">Сегмент URI `/signin-twitter` задается в качестве обратного вызова по умолчанию для поставщика проверки подлинности Twitter.</span><span class="sxs-lookup"><span data-stu-id="57b1c-117">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="57b1c-118">URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Twitter с помощью унаследованного свойства [ремотеаусентикатионоптионс. каллбаккпас](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) класса [твиттероптионс](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="57b1c-118">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="57b1c-119">Заполните оставшуюся часть формы и выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="57b1c-119">Fill out the rest of the form and select **Create**.</span></span> <span data-ttu-id="57b1c-120">Отобразятся сведения о новом приложении:</span><span class="sxs-lookup"><span data-stu-id="57b1c-120">New application details are displayed:</span></span>

## <a name="store-the-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="57b1c-121">Хранение ключа и секрета API потребителя Twitter</span><span class="sxs-lookup"><span data-stu-id="57b1c-121">Store the Twitter consumer API key and secret</span></span>

<span data-ttu-id="57b1c-122">Храните конфиденциальные параметры, такие как ключ API потребителя Twitter и секретный код, с помощью [диспетчера секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="57b1c-122">Store sensitive settings such as the Twitter consumer API key and secret with [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="57b1c-123">Для этого примера выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="57b1c-123">For this sample, use the following steps:</span></span>

1. <span data-ttu-id="57b1c-124">Инициализируйте проект для хранения секретных данных согласно инструкциям в разделе [Включение секретного хранилища](xref:security/app-secrets#enable-secret-storage).</span><span class="sxs-lookup"><span data-stu-id="57b1c-124">Initialize the project for secret storage per the instructions at [Enable secret storage](xref:security/app-secrets#enable-secret-storage).</span></span>
1. <span data-ttu-id="57b1c-125">Храните конфиденциальные параметры в локальном хранилище секретов с помощью ключей секретов `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret`:</span><span class="sxs-lookup"><span data-stu-id="57b1c-125">Store the sensitive settings in the local secret store with the secrets keys `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`:</span></span>

    ```dotnetcli
    dotnet user-secrets set "Authentication:Twitter:ConsumerAPIKey" "<consumer-api-key>"
    dotnet user-secrets set "Authentication:Twitter:ConsumerSecret" "<consumer-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="57b1c-126">Эти маркеры можно найти на вкладке **ключи и маркеры доступа** после создания нового приложения Twitter.</span><span class="sxs-lookup"><span data-stu-id="57b1c-126">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="57b1c-127">Настройка проверки подлинности Twitter</span><span class="sxs-lookup"><span data-stu-id="57b1c-127">Configure Twitter Authentication</span></span>

<span data-ttu-id="57b1c-128">Добавьте службу Twitter в метод `ConfigureServices` в файле *Startup.CS* :</span><span class="sxs-lookup"><span data-stu-id="57b1c-128">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-15)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="57b1c-129">Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности в Twitter, см. в справочнике по API [твиттероптионс](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="57b1c-129">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="57b1c-130">Это может использоваться для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="57b1c-130">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="57b1c-131">Вход с помощью Twitter</span><span class="sxs-lookup"><span data-stu-id="57b1c-131">Sign in with Twitter</span></span>

<span data-ttu-id="57b1c-132">Запустите приложение и выберите **Вход**.</span><span class="sxs-lookup"><span data-stu-id="57b1c-132">Run the app and select **Log in**.</span></span> <span data-ttu-id="57b1c-133">Появится возможность входа с помощью Twitter:</span><span class="sxs-lookup"><span data-stu-id="57b1c-133">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="57b1c-134">При щелчке **Twitter** в Twitter перенаправления для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="57b1c-134">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="57b1c-135">После ввода учетных данных Twitter вы будете перенаправлены на веб-сайт, на котором можно задать свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="57b1c-135">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="57b1c-136">Теперь вы выполнили вход с использованием учетных данных Twitter:</span><span class="sxs-lookup"><span data-stu-id="57b1c-136">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="57b1c-137">Диагностика</span><span class="sxs-lookup"><span data-stu-id="57b1c-137">Troubleshooting</span></span>

* <span data-ttu-id="57b1c-138">**Только ASP.NET Core 2. x:** Если удостоверение не настроено путем вызова `services.AddIdentity` в `ConfigureServices`, попытка проверки подлинности приведет к появлению *исключения ArgumentException: необходимо указать параметр "сигнинсчеме"* .</span><span class="sxs-lookup"><span data-stu-id="57b1c-138">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="57b1c-139">Шаблон проекта, используемый в этом образце, гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="57b1c-139">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="57b1c-140">Если база данных сайта не была создана с применением первоначальной миграции, то при обработке ошибки запроса возникнет *Ошибка операции с базой данных* .</span><span class="sxs-lookup"><span data-stu-id="57b1c-140">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="57b1c-141">Выберите **Применить миграции** , чтобы создать базу данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="57b1c-141">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57b1c-142">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="57b1c-142">Next steps</span></span>

* <span data-ttu-id="57b1c-143">В этой статье показано, как можно пройти проверку подлинности в Twitter.</span><span class="sxs-lookup"><span data-stu-id="57b1c-143">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="57b1c-144">Аналогичный подход можно использовать для проверки подлинности с другими поставщиками, перечисленными на [предыдущей странице](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="57b1c-144">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="57b1c-145">После публикации веб-сайта в веб-приложение Azure необходимо сбросить `ConsumerSecret` на портале разработчика Twitter.</span><span class="sxs-lookup"><span data-stu-id="57b1c-145">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="57b1c-146">Задайте `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret` в качестве параметров приложения в портал Azure.</span><span class="sxs-lookup"><span data-stu-id="57b1c-146">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="57b1c-147">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="57b1c-147">The configuration system is set up to read keys from environment variables.</span></span>
