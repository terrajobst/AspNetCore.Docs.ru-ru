---
title: Настройка внешнего входа в Twitter с помощью ASP.NET Core
author: rick-anderson
description: В этом руководстве демонстрируется интеграция аутентификации пользователя учетной записи Twitter с существующим ASP.NET Core приложением.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: 4710c033018710ce3620f8d7221ae2253b2c0b69
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172523"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="e56ab-103">Настройка внешнего входа в Twitter с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e56ab-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="e56ab-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="e56ab-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e56ab-105">В этом примере показано, как разрешить пользователям [входить с помощью учетной записи Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) , используя пример проекта ASP.NET Core 3,0, созданный на [предыдущей странице](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="e56ab-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="e56ab-106">Создание приложения в Twitter</span><span class="sxs-lookup"><span data-stu-id="e56ab-106">Create the app in Twitter</span></span>

* <span data-ttu-id="e56ab-107">Добавьте в проект пакет NuGet [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) .</span><span class="sxs-lookup"><span data-stu-id="e56ab-107">Add the [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet package to the project.</span></span>

* <span data-ttu-id="e56ab-108">Перейдите к [https://apps.twitter.com/](https://apps.twitter.com/) и выполните вход.</span><span class="sxs-lookup"><span data-stu-id="e56ab-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="e56ab-109">Если у вас еще нет учетной записи Twitter, используйте ссылку **[зарегистрироваться сейчас](https://twitter.com/signup)** , чтобы создать ее.</span><span class="sxs-lookup"><span data-stu-id="e56ab-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="e56ab-110">Выберите **Create an app** (Создать приложение).</span><span class="sxs-lookup"><span data-stu-id="e56ab-110">Select **Create an app**.</span></span> <span data-ttu-id="e56ab-111">Укажите **имя приложения**, **Описание приложения** и универсальный код ресурса (URI) общедоступного **веб-сайта** (это может быть временным, пока не будет зарегистрировано доменное имя):</span><span class="sxs-lookup"><span data-stu-id="e56ab-111">Fill out the **App name**, **Application description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="e56ab-112">Установите флажок **включить вход с помощью Twitter** .</span><span class="sxs-lookup"><span data-stu-id="e56ab-112">Check the box next to **Enable Sign in with Twitter**</span></span>

* <span data-ttu-id="e56ab-113">Microsoft. AspNetCore. Identity требует, чтобы по умолчанию у пользователей был адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="e56ab-113">Microsoft.AspNetCore.Identity requires users to have an email address by default.</span></span> <span data-ttu-id="e56ab-114">Перейдите на вкладку **разрешения** , нажмите кнопку " **изменить** " и установите флажок " **запросить адрес электронной почты у пользователей**".</span><span class="sxs-lookup"><span data-stu-id="e56ab-114">Go to the **Permissions** tab, click the **Edit** button and check the box next to **Request email address from users**.</span></span>

* <span data-ttu-id="e56ab-115">Введите универсальный код ресурса (URI) для разработки `/signin-twitter` добавлен в поле **URL-адреса обратного вызова** (например: `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="e56ab-115">Enter your development URI with `/signin-twitter` appended into the **Callback URLs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="e56ab-116">Схема проверки подлинности Twitter, настроенная далее в этом примере, автоматически обрабатывает запросы на `/signin-twitter` маршруте для реализации потока OAuth.</span><span class="sxs-lookup"><span data-stu-id="e56ab-116">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="e56ab-117">Сегмент URI `/signin-twitter` задается в качестве обратного вызова по умолчанию для поставщика проверки подлинности Twitter.</span><span class="sxs-lookup"><span data-stu-id="e56ab-117">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="e56ab-118">URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Twitter с помощью унаследованного свойства [ремотеаусентикатионоптионс. каллбаккпас](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) класса [твиттероптионс](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="e56ab-118">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="e56ab-119">Заполните оставшуюся часть формы и выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="e56ab-119">Fill out the rest of the form and select **Create**.</span></span> <span data-ttu-id="e56ab-120">Отобразятся сведения о новом приложении:</span><span class="sxs-lookup"><span data-stu-id="e56ab-120">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="e56ab-121">Хранение ключа и секрета API потребителя Twitter</span><span class="sxs-lookup"><span data-stu-id="e56ab-121">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="e56ab-122">Выполните следующие команды для безопасного хранения `ClientId` и `ClientSecret` с помощью [диспетчера секретов](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="e56ab-122">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

<span data-ttu-id="e56ab-123">Свяжите конфиденциальные параметры, такие как Twitter `Consumer Key` и `Consumer Secret` в конфигурации приложения с помощью [диспетчера секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="e56ab-123">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="e56ab-124">Для целей этого примера назовите маркеры `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="e56ab-124">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="e56ab-125">Эти маркеры можно найти на вкладке **ключи и маркеры доступа** после создания нового приложения Twitter.</span><span class="sxs-lookup"><span data-stu-id="e56ab-125">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="e56ab-126">Настройка проверки подлинности Twitter</span><span class="sxs-lookup"><span data-stu-id="e56ab-126">Configure Twitter Authentication</span></span>

<span data-ttu-id="e56ab-127">Добавьте службу Twitter в метод `ConfigureServices` в файле *Startup.CS* :</span><span class="sxs-lookup"><span data-stu-id="e56ab-127">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-15)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="e56ab-128">Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности в Twitter, см. в справочнике по API [твиттероптионс](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="e56ab-128">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="e56ab-129">Это может использоваться для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="e56ab-129">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="e56ab-130">Вход с помощью Twitter</span><span class="sxs-lookup"><span data-stu-id="e56ab-130">Sign in with Twitter</span></span>

<span data-ttu-id="e56ab-131">Запустите приложение и выберите **Вход**.</span><span class="sxs-lookup"><span data-stu-id="e56ab-131">Run the app and select **Log in**.</span></span> <span data-ttu-id="e56ab-132">Появится возможность входа с помощью Twitter:</span><span class="sxs-lookup"><span data-stu-id="e56ab-132">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="e56ab-133">При щелчке **Twitter** в Twitter перенаправления для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="e56ab-133">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="e56ab-134">После ввода учетных данных Twitter вы будете перенаправлены на веб-сайт, на котором можно задать свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="e56ab-134">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="e56ab-135">Теперь вы выполнили вход с использованием учетных данных Twitter:</span><span class="sxs-lookup"><span data-stu-id="e56ab-135">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="e56ab-136">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="e56ab-136">Troubleshooting</span></span>

* <span data-ttu-id="e56ab-137">**Только ASP.NET Core 2. x:** Если удостоверение не настроено путем вызова `services.AddIdentity` в `ConfigureServices`, попытка проверки подлинности приведет к появлению *исключения ArgumentException: необходимо указать параметр "сигнинсчеме"* .</span><span class="sxs-lookup"><span data-stu-id="e56ab-137">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="e56ab-138">Шаблон проекта, используемый в этом образце, гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="e56ab-138">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="e56ab-139">Если база данных сайта не была создана с применением первоначальной миграции, то при обработке ошибки запроса возникнет *Ошибка операции с базой данных* .</span><span class="sxs-lookup"><span data-stu-id="e56ab-139">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="e56ab-140">Выберите **Применить миграции** , чтобы создать базу данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="e56ab-140">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e56ab-141">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="e56ab-141">Next steps</span></span>

* <span data-ttu-id="e56ab-142">В этой статье показано, как можно пройти проверку подлинности в Twitter.</span><span class="sxs-lookup"><span data-stu-id="e56ab-142">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="e56ab-143">Аналогичный подход можно использовать для проверки подлинности с другими поставщиками, перечисленными на [предыдущей странице](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="e56ab-143">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="e56ab-144">После публикации веб-сайта в веб-приложение Azure необходимо сбросить `ConsumerSecret` на портале разработчика Twitter.</span><span class="sxs-lookup"><span data-stu-id="e56ab-144">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="e56ab-145">Задайте `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret` в качестве параметров приложения в портал Azure.</span><span class="sxs-lookup"><span data-stu-id="e56ab-145">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="e56ab-146">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="e56ab-146">The configuration system is set up to read keys from environment variables.</span></span>
