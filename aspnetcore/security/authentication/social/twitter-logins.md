---
title: "Программа установки внешнее имя входа Twitter"
author: rick-anderson
description: "В этом учебнике показано интеграции Twitter проверки подлинности учетной записи пользователя в существующее приложение ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: d92f9b1f55c03018f88cf9298e981fc4b2c29f41
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="configuring-twitter-authentication"></a><span data-ttu-id="d9dd8-103">Настройка проверки подлинности Twitter</span><span class="sxs-lookup"><span data-stu-id="d9dd8-103">Configuring Twitter authentication</span></span>

<span data-ttu-id="d9dd8-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="d9dd8-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d9dd8-105">Этого учебника показано, как предоставить пользователям возможность [войти в свою учетную запись Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) с помощью проекта ASP.NET 2.0 основной пример создан на [предыдущую страницу](index.md).</span><span class="sxs-lookup"><span data-stu-id="d9dd8-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="d9dd8-106">Создать приложение на Twitter</span><span class="sxs-lookup"><span data-stu-id="d9dd8-106">Create the app in Twitter</span></span>

* <span data-ttu-id="d9dd8-107">Перейдите к [https://apps.twitter.com/](https://apps.twitter.com/) и выполните вход.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="d9dd8-108">Если у вас еще нет учетной записи Twitter, используйте  **[Зарегистрируйтесь сейчас](https://twitter.com/signup)**  ссылку, чтобы создать его.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="d9dd8-109">После входа в, **управление приложениями** показана страница:</span><span class="sxs-lookup"><span data-stu-id="d9dd8-109">After signing in, the **Application Management** page is shown:</span></span>

![Управление приложениями Twitter открыть в Microsoft Edge](index/_static/TwitterAppManage.png)

* <span data-ttu-id="d9dd8-111">Коснитесь **создать новое приложение** и заполнить приложения **имя**, **описание** и открытые **веб-сайт** URI (это может быть временной пока не будет Зарегистрируйте доменное имя):</span><span class="sxs-lookup"><span data-stu-id="d9dd8-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![Создание страницы приложения](index/_static/TwitterCreate.png)

* <span data-ttu-id="d9dd8-113">Введите URI разработки с */signin-twitter* добавляется в **допустимый URI перенаправления OAuth** поля (например: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="d9dd8-113">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="d9dd8-114">Схема проверки подлинности Twitter настроен далее в этом учебнике автоматически будет обрабатывать запросы на */signin-twitter* маршрута для реализации потока OAuth.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="d9dd8-115">Заполните другие формы и коснитесь **создания приложения Twitter**.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-115">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="d9dd8-116">Отображаются сведения о новом приложение.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-116">New application details are displayed:</span></span>

![Вкладка "Подробности" на странице приложения](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="d9dd8-118">При развертывании узла необходимо будет повторно **управление приложениями** страницы и зарегистрировать новый открытый универсальный код Ресурса.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-118">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="d9dd8-119">Хранение Twitter ConsumerKey и ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="d9dd8-119">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="d9dd8-120">Связать конфиденциальные параметры, такие как Twitter `Consumer Key` и `Consumer Secret` в конфигурации приложения с помощью [секрет диспетчер](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="d9dd8-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="d9dd8-121">В целях этого учебника назовите токены `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-121">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="d9dd8-122">Эти токены можно найти на **ключей и маркеры доступа** вкладку после создания нового приложения Twitter:</span><span class="sxs-lookup"><span data-stu-id="d9dd8-122">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Вкладка ключей и маркеры доступа](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="d9dd8-124">Настройка проверки подлинности Twitter</span><span class="sxs-lookup"><span data-stu-id="d9dd8-124">Configure Twitter Authentication</span></span>

<span data-ttu-id="d9dd8-125">Шаблон проекта, используемые в этом учебнике гарантирует, что [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) пакет уже установлен.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-125">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="d9dd8-126">Чтобы установить этот пакет с 2017 г. для Visual Studio, щелкните правой кнопкой мыши проект и выберите пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-126">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="d9dd8-127">Чтобы установить с .NET Core CLI, выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="d9dd8-127">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d9dd8-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d9dd8-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d9dd8-129">Добавление службы Twitter в `ConfigureServices` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="d9dd8-129">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d9dd8-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d9dd8-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d9dd8-131">Добавить по промежуточного слоя Twitter в `Configure` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="d9dd8-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="d9dd8-132">В разделе [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) Справочник по API для дополнительных сведений о параметрах конфигурации, поддерживаемых проверкой подлинности Twitter.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-132">See the [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="d9dd8-133">Это можно использовать для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="d9dd8-134">Войдите с помощью Twitter</span><span class="sxs-lookup"><span data-stu-id="d9dd8-134">Sign in with Twitter</span></span>

<span data-ttu-id="d9dd8-135">Запустите приложение и нажмите кнопку **входа**.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="d9dd8-136">Появляется возможность войти в Twitter:</span><span class="sxs-lookup"><span data-stu-id="d9dd8-136">An option to sign in with Twitter appears:</span></span>

![Веб-приложения: пользователь не прошел проверку подлинности](index/_static/DoneTwitter.png)

<span data-ttu-id="d9dd8-138">Щелкнув **Twitter** перенаправляет Twitter для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="d9dd8-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Страница проверки подлинности Twitter](index/_static/TwitterLogin.png)

<span data-ttu-id="d9dd8-140">После ввода учетных данных Twitter, вы будете перенаправлены к веб-сайта, где вы можете задать ваш адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="d9dd8-141">Будет выполнен вход с помощью учетных данных Twitter:</span><span class="sxs-lookup"><span data-stu-id="d9dd8-141">You are now logged in using your Twitter credentials:</span></span>

![Веб-приложения: пользователь прошел проверку подлинности](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="d9dd8-143">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="d9dd8-143">Troubleshooting</span></span>

* <span data-ttu-id="d9dd8-144">**ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-144">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="d9dd8-145">Шаблон проекта, используемые в этом учебнике гарантирует, что происходит.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="d9dd8-146">Если не был создан путем применения первоначальной миграции базы данных сайта, вы получите *не удалось выполнить операцию базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="d9dd8-147">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9dd8-148">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="d9dd8-148">Next steps</span></span>

* <span data-ttu-id="d9dd8-149">В этой статье показано, как можно выполнить проверку подлинности с Twitter.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="d9dd8-150">Можно выполнить подобный подход для проверки подлинности для других поставщиков на [предыдущую страницу](index.md).</span><span class="sxs-lookup"><span data-stu-id="d9dd8-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="d9dd8-151">После публикации веб-сайте Azure веб-приложения, необходимо переустановить `ConsumerSecret` на портале разработчика Twitter.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="d9dd8-152">Задать `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="d9dd8-153">Система конфигурации настраивается для чтения ключи из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="d9dd8-153">The configuration system is set up to read keys from environment variables.</span></span>
