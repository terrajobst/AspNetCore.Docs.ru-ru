---
title: Настройка внешней учетной записи Twitter с помощью ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграции проверки подлинности пользователя учетной записи Twitter в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 43a5ea59d8853d297ae2c1ec3f4b1c0c14ec80c3
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708430"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="25aa3-103">Настройка внешней учетной записи Twitter с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25aa3-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="25aa3-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="25aa3-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="25aa3-105">Этом руководстве показано, как разрешить пользователям [войдите с помощью своей учетной записью Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) используя образец проекта ASP.NET Core 2.0 создан на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="25aa3-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="25aa3-106">Создайте приложение в Twitter</span><span class="sxs-lookup"><span data-stu-id="25aa3-106">Create the app in Twitter</span></span>

* <span data-ttu-id="25aa3-107">Перейдите к [ https://apps.twitter.com/ ](https://apps.twitter.com/) и войдите в систему.</span><span class="sxs-lookup"><span data-stu-id="25aa3-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="25aa3-108">Если у вас нет учетной записи Twitter, используйте **[Зарегистрируйтесь сейчас](https://twitter.com/signup)** ссылку, чтобы создать его.</span><span class="sxs-lookup"><span data-stu-id="25aa3-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="25aa3-109">После входа, **управление приложениями** отобразится страница с:</span><span class="sxs-lookup"><span data-stu-id="25aa3-109">After signing in, the **Application Management** page is shown:</span></span>

  ![Управление приложениями Twitter можно открыть в Microsoft Edge](index/_static/TwitterAppManage.png)

* <span data-ttu-id="25aa3-111">Коснитесь **создать новое приложение** и заполните приложения **имя**, **описание** и общедоступных **веб-сайт** URI (это могут быть временными пока не будут Зарегистрируйте доменное имя):</span><span class="sxs-lookup"><span data-stu-id="25aa3-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

  ![Создание страницы приложения](index/_static/TwitterCreate.png)

* <span data-ttu-id="25aa3-113">Введите URI разработки с `/signin-twitter` добавляется в **допустимый URI перенаправления OAuth** поле (например: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="25aa3-113">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="25aa3-114">Схема проверки подлинности Twitter, Настройка описывается далее в этом руководстве автоматически будет обрабатывать запросы на `/signin-twitter` маршрута, чтобы реализовать поток OAuth.</span><span class="sxs-lookup"><span data-stu-id="25aa3-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="25aa3-115">Сегмент URI `/signin-twitter` задан в качестве обратного вызова по умолчанию поставщика проверки подлинности Twitter.</span><span class="sxs-lookup"><span data-stu-id="25aa3-115">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="25aa3-116">URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Twitter с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) класс.</span><span class="sxs-lookup"><span data-stu-id="25aa3-116">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="25aa3-117">Заполните оставшиеся поля формы и коснитесь **создать приложение Twitter**.</span><span class="sxs-lookup"><span data-stu-id="25aa3-117">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="25aa3-118">Отображаются сведения о новом приложения:</span><span class="sxs-lookup"><span data-stu-id="25aa3-118">New application details are displayed:</span></span>

  ![Вкладке "Сведения" на странице "приложения"](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="25aa3-120">При развертывании на сайте необходимо будет пересмотреть **управление приложениями** странице и зарегистрировать новый открытый универсальный код Ресурса.</span><span class="sxs-lookup"><span data-stu-id="25aa3-120">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="25aa3-121">Хранение Twitter ConsumerKey и ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="25aa3-121">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="25aa3-122">Связать конфиденциальные параметры, такие как Twitter `Consumer Key` и `Consumer Secret` для конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="25aa3-122">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="25aa3-123">В целях этого учебника назовите токены `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="25aa3-123">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="25aa3-124">Эти маркеры можно найти на **ключи и токены доступа** вкладке после создания нового приложения Twitter:</span><span class="sxs-lookup"><span data-stu-id="25aa3-124">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Вкладка "ключи и маркеры доступа"](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="25aa3-126">Настройка проверки подлинности Twitter</span><span class="sxs-lookup"><span data-stu-id="25aa3-126">Configure Twitter Authentication</span></span>

<span data-ttu-id="25aa3-127">Шаблон проекта, используемый в этом руководстве гарантирует, что [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) пакет уже установлен.</span><span class="sxs-lookup"><span data-stu-id="25aa3-127">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="25aa3-128">Установить этот пакет с помощью Visual Studio 2017, щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="25aa3-128">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="25aa3-129">Чтобы установить с помощью интерфейса командной строки .NET Core, выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="25aa3-129">To install with .NET Core CLI, execute the following in your project directory:</span></span>

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="25aa3-130">Добавление службы Twitter в `ConfigureServices` метод в *Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="25aa3-130">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="25aa3-131">Добавьте по промежуточного слоя Twitter в `Configure` метод в *Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="25aa3-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

<span data-ttu-id="25aa3-132">См. в разделе [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) Справочник по API, Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности Twitter.</span><span class="sxs-lookup"><span data-stu-id="25aa3-132">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="25aa3-133">Это может использоваться для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="25aa3-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="25aa3-134">Войдите с помощью Twitter</span><span class="sxs-lookup"><span data-stu-id="25aa3-134">Sign in with Twitter</span></span>

<span data-ttu-id="25aa3-135">Запустите приложение и нажмите кнопку **вход**.</span><span class="sxs-lookup"><span data-stu-id="25aa3-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="25aa3-136">Появится возможность войти с помощью Twitter:</span><span class="sxs-lookup"><span data-stu-id="25aa3-136">An option to sign in with Twitter appears:</span></span>

![Веб-приложения: пользователь не прошел проверку подлинности](index/_static/DoneTwitter.png)

<span data-ttu-id="25aa3-138">Щелкнув **Twitter** перенаправляет Twitter для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="25aa3-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Страница проверки подлинности Twitter](index/_static/TwitterLogin.png)

<span data-ttu-id="25aa3-140">После ввода учетных данных Twitter, вы будете перенаправлены обратно на веб-узел, где вы можете задать свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="25aa3-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="25aa3-141">Теперь вы вошли с использованием учетных данных Twitter:</span><span class="sxs-lookup"><span data-stu-id="25aa3-141">You are now logged in using your Twitter credentials:</span></span>

![Веб-приложения: пользователь прошел проверку подлинности](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="25aa3-143">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="25aa3-143">Troubleshooting</span></span>

* <span data-ttu-id="25aa3-144">**ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*.</span><span class="sxs-lookup"><span data-stu-id="25aa3-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="25aa3-145">Шаблон проекта, используемый в этом руководстве гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="25aa3-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="25aa3-146">Если база данных сайта не был создан путем применения первоначальной миграции, вы получите *сбой операции из базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="25aa3-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="25aa3-147">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="25aa3-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25aa3-148">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="25aa3-148">Next steps</span></span>

* <span data-ttu-id="25aa3-149">В этой статье объясняется, как можно выполнить проверку подлинности с помощью Twitter.</span><span class="sxs-lookup"><span data-stu-id="25aa3-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="25aa3-150">Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="25aa3-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="25aa3-151">После публикации веб-сайт веб-приложение Azure, необходимо сбросить `ConsumerSecret` на портале разработчика Twitter.</span><span class="sxs-lookup"><span data-stu-id="25aa3-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="25aa3-152">Задайте `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="25aa3-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="25aa3-153">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="25aa3-153">The configuration system is set up to read keys from environment variables.</span></span>
