---
title: Настройка внешнего входа в Twitter с помощью ASP.NET Core
author: rick-anderson
description: В этом руководстве демонстрируется интеграция аутентификации пользователя учетной записи Twitter с существующим ASP.NET Core приложением.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 5182f1647acb664bf35f086fcddbe909559a62f7
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082303"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="13eb4-103">Настройка внешнего входа в Twitter с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13eb4-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="13eb4-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="13eb4-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="13eb4-105">В этом примере показано, как разрешить пользователям [входить с помощью учетной записи Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) , используя пример проекта ASP.NET Core 2,2, созданный на [предыдущей странице](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="13eb4-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="13eb4-106">Создание приложения в Twitter</span><span class="sxs-lookup"><span data-stu-id="13eb4-106">Create the app in Twitter</span></span>

* <span data-ttu-id="13eb4-107">Перейдите к [ https://apps.twitter.com/ ](https://apps.twitter.com/) и войдите в систему.</span><span class="sxs-lookup"><span data-stu-id="13eb4-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="13eb4-108">Если у вас еще нет учетной записи Twitter, используйте ссылку **[зарегистрироваться сейчас](https://twitter.com/signup)** , чтобы создать ее.</span><span class="sxs-lookup"><span data-stu-id="13eb4-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="13eb4-109">Коснитесь элемента **создать новое приложение** и введите URI **имени**приложения, **описания** и общедоступного **веб-сайта** (это может быть временным, пока не будет зарегистрировано доменное имя):</span><span class="sxs-lookup"><span data-stu-id="13eb4-109">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="13eb4-110">Введите URI разработки с `/signin-twitter` добавлением в **допустимое поле URI перенаправления OAuth** (например: `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="13eb4-110">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="13eb4-111">Схема проверки подлинности Twitter, настроенная далее в этом примере, `/signin-twitter` автоматически обрабатывает запросы по маршруту для реализации потока OAuth.</span><span class="sxs-lookup"><span data-stu-id="13eb4-111">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="13eb4-112">Сегмент `/signin-twitter` URI задается в качестве обратного вызова по умолчанию для поставщика проверки подлинности Twitter.</span><span class="sxs-lookup"><span data-stu-id="13eb4-112">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="13eb4-113">URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Twitter с помощью унаследованного свойства [ремотеаусентикатионоптионс. каллбаккпас](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) класса [твиттероптионс](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="13eb4-113">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="13eb4-114">Заполните оставшуюся часть формы и коснитесь пункта **создать приложение Twitter**.</span><span class="sxs-lookup"><span data-stu-id="13eb4-114">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="13eb4-115">Отобразятся сведения о новом приложении:</span><span class="sxs-lookup"><span data-stu-id="13eb4-115">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="13eb4-116">Хранение ключа и секрета API потребителя Twitter</span><span class="sxs-lookup"><span data-stu-id="13eb4-116">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="13eb4-117">Выполните следующие команды для безопасного хранения `ClientId` и `ClientSecret` использования [диспетчера секретов](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="13eb4-117">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

<span data-ttu-id="13eb4-118">Свяжите конфиденциальные параметры, `Consumer Key` такие `Consumer Secret` как Twitter, и конфигурацию приложения с помощью [диспетчера секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="13eb4-118">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="13eb4-119">Для целей этого примера назовите маркеры `Authentication:Twitter:ConsumerKey` и. `Authentication:Twitter:ConsumerSecret`</span><span class="sxs-lookup"><span data-stu-id="13eb4-119">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="13eb4-120">Эти маркеры можно найти на вкладке **ключи и маркеры доступа** после создания нового приложения Twitter.</span><span class="sxs-lookup"><span data-stu-id="13eb4-120">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="13eb4-121">Настройка проверки подлинности Twitter</span><span class="sxs-lookup"><span data-stu-id="13eb4-121">Configure Twitter Authentication</span></span>

<span data-ttu-id="13eb4-122">Добавьте службу Twitter в `ConfigureServices` метод в файле *Startup.CS* :</span><span class="sxs-lookup"><span data-stu-id="13eb4-122">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="13eb4-123">Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности в Twitter, см. в справочнике по API [твиттероптионс](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="13eb4-123">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="13eb4-124">Это может использоваться для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="13eb4-124">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="13eb4-125">Вход с помощью Twitter</span><span class="sxs-lookup"><span data-stu-id="13eb4-125">Sign in with Twitter</span></span>

<span data-ttu-id="13eb4-126">Запустите приложение и выберите **Вход**.</span><span class="sxs-lookup"><span data-stu-id="13eb4-126">Run the app and select **Log in**.</span></span> <span data-ttu-id="13eb4-127">Появится возможность входа с помощью Twitter:</span><span class="sxs-lookup"><span data-stu-id="13eb4-127">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="13eb4-128">При щелчке **Twitter** в Twitter перенаправления для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="13eb4-128">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="13eb4-129">После ввода учетных данных Twitter вы будете перенаправлены на веб-сайт, на котором можно задать свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="13eb4-129">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="13eb4-130">Теперь вы выполнили вход с использованием учетных данных Twitter:</span><span class="sxs-lookup"><span data-stu-id="13eb4-130">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="13eb4-131">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="13eb4-131">Troubleshooting</span></span>

* <span data-ttu-id="13eb4-132">**Только ASP.NET Core 2. x:** Если удостоверение не настроено `services.AddIdentity` с `ConfigureServices`помощью вызова в, попытка проверки подлинности приведет *к появлению исключения ArgumentException: Необходимо указать*параметр "сигнинсчеме".</span><span class="sxs-lookup"><span data-stu-id="13eb4-132">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="13eb4-133">Шаблон проекта, используемый в этом образце, гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="13eb4-133">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="13eb4-134">Если база данных сайта не был создан путем применения первоначальной миграции, вы получите *сбой операции из базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="13eb4-134">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="13eb4-135">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="13eb4-135">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="13eb4-136">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="13eb4-136">Next steps</span></span>

* <span data-ttu-id="13eb4-137">В этой статье показано, как можно пройти проверку подлинности в Twitter.</span><span class="sxs-lookup"><span data-stu-id="13eb4-137">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="13eb4-138">Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="13eb4-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="13eb4-139">После публикации веб-сайта в веб-приложение Azure необходимо сбросить настройки `ConsumerSecret` на портале разработчика Twitter.</span><span class="sxs-lookup"><span data-stu-id="13eb4-139">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="13eb4-140">Задайте `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="13eb4-140">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="13eb4-141">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="13eb4-141">The configuration system is set up to read keys from environment variables.</span></span>
