---
title: "Настройка внешнего входа Facebook в ASP.NET Core"
author: rick-anderson
description: "В этом учебнике показано интеграции проверки подлинности пользователя учетной записи Facebook в существующее приложение ASP.NET Core."
keywords: "ASP.NET Core, Facebook, имя входа, проверка подлинности"
ms.author: riande
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.assetid: 8c65179b-688c-4af1-8f5e-1862920cda95
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/facebook-logins
ms.openlocfilehash: 826ac826c22dae81e5dbea08a11a62cac0b1068a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="configuring-facebook-authentication"></a><span data-ttu-id="eb98a-104">Настройка проверки подлинности Facebook</span><span class="sxs-lookup"><span data-stu-id="eb98a-104">Configuring Facebook authentication</span></span>

<span data-ttu-id="eb98a-105">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="eb98a-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eb98a-106">Этого учебника показано, как предоставить пользователям возможность войти в свою учетную запись Facebook с помощью ASP.NET 2.0 основной пример проекта создан на [предыдущую страницу](index.md).</span><span class="sxs-lookup"><span data-stu-id="eb98a-106">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="eb98a-107">Мы начнем с создания код приложения Facebook, следуя [официальный действия](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="eb98a-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="eb98a-108">Создать приложение на Facebook</span><span class="sxs-lookup"><span data-stu-id="eb98a-108">Create the app in Facebook</span></span>

*  <span data-ttu-id="eb98a-109">Перейдите к [Facebook для разработчиков](https://developers.facebook.com) страницы и выполните вход.</span><span class="sxs-lookup"><span data-stu-id="eb98a-109">Navigate to the [Facebook for Developers](https://developers.facebook.com) page and sign in.</span></span> <span data-ttu-id="eb98a-110">Если у вас еще нет учетной записи Facebook, используйте **зарегистрироваться для Facebook** ссылку на страницу входа для ее создания.</span><span class="sxs-lookup"><span data-stu-id="eb98a-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="eb98a-111">Коснитесь **Создание приложения** кнопку в правом верхнем углу, чтобы создать новый идентификатор приложения.</span><span class="sxs-lookup"><span data-stu-id="eb98a-111">Tap the **Create App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook для портала разработчиков открыть в Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="eb98a-113">Заполните форму и коснитесь **создать идентификатор приложения** кнопки.</span><span class="sxs-lookup"><span data-stu-id="eb98a-113">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Создайте форму новый идентификатор приложения](index/_static/FBNewAppId.png)

* <span data-ttu-id="eb98a-115">В появившемся **выберите продукт** запрос, нажмите кнопку **Set Up** на **входа Facebook** карты.</span><span class="sxs-lookup"><span data-stu-id="eb98a-115">When presented with **Select a product** prompt, Click **Set Up** on the **Facebook Login** card.</span></span>

   ![Страница установки продукта](index/_static/FBProductSetup.png)

* <span data-ttu-id="eb98a-117">**Краткое руководство** запустит мастер **выберите платформу** первая страница.</span><span class="sxs-lookup"><span data-stu-id="eb98a-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="eb98a-118">Сейчас пропустить мастер, нажав кнопку **параметры** ссылку в меню слева:</span><span class="sxs-lookup"><span data-stu-id="eb98a-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Пропустить быстрый запуск](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="eb98a-120">Появится **параметры клиента OAuth** страницы:</span><span class="sxs-lookup"><span data-stu-id="eb98a-120">You are presented with the **Client OAuth Settings** page:</span></span>

![Страница параметров OAuth клиента](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="eb98a-122">Введите URI разработки с */signin-facebook* добавляется в **допустимый URI перенаправления OAuth** поля (например: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="eb98a-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="eb98a-123">Проверка подлинности Facebook далее в этом учебнике автоматически будет обрабатывать запросы на */signin-facebook* маршрута для реализации потока OAuth.</span><span class="sxs-lookup"><span data-stu-id="eb98a-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="eb98a-124">Нажмите кнопку **сохранить изменения**.</span><span class="sxs-lookup"><span data-stu-id="eb98a-124">Click **Save Changes**.</span></span>

* <span data-ttu-id="eb98a-125">Нажмите кнопку **мониторинга** ссылку в левой области навигации.</span><span class="sxs-lookup"><span data-stu-id="eb98a-125">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="eb98a-126">На этой странице, запишите вашей `App ID` и `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="eb98a-126">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="eb98a-127">В следующем разделе будет добавлен и в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eb98a-127">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Панели мониторинга разработчика Facebook](index/_static/FBDashboard.png)

* <span data-ttu-id="eb98a-129">При развертывании сайта необходимо повторно **входа Facebook** страница установки и зарегистрировать новый открытый универсальный код Ресурса.</span><span class="sxs-lookup"><span data-stu-id="eb98a-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="eb98a-130">Сохранить код приложения Facebook и секрет приложения</span><span class="sxs-lookup"><span data-stu-id="eb98a-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="eb98a-131">Связать конфиденциальные параметры, например Facebook `App ID` и `App Secret` в конфигурации приложения с помощью [секрет диспетчер](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="eb98a-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="eb98a-132">В целях этого учебника назовите токены `Authentication:Facebook:AppId` и `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="eb98a-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="eb98a-133">Выполните следующие команды для безопасного хранения `App ID` и `App Secret` с помощью диспетчера секрет:</span><span class="sxs-lookup"><span data-stu-id="eb98a-133">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="eb98a-134">Настройка проверки подлинности Facebook</span><span class="sxs-lookup"><span data-stu-id="eb98a-134">Configure Facebook Authentication</span></span>

<span data-ttu-id="eb98a-135">Шаблон проекта, используемые в этом учебнике гарантирует, что [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) пакет уже установлен.</span><span class="sxs-lookup"><span data-stu-id="eb98a-135">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package is already installed.</span></span>

* <span data-ttu-id="eb98a-136">Чтобы установить этот пакет с 2017 г. для Visual Studio, щелкните правой кнопкой мыши проект и выберите пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="eb98a-136">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="eb98a-137">Чтобы установить с .NET Core CLI, выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="eb98a-137">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="eb98a-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="eb98a-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="eb98a-139">Добавьте службу Facebook в `ConfigureServices` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="eb98a-139">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="eb98a-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="eb98a-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="eb98a-141">Добавить по промежуточного слоя Facebook в `Configure` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="eb98a-141">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="eb98a-142">В разделе [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) Справочник по API для дополнительных сведений о параметрах конфигурации, поддерживаемых проверкой подлинности Facebook.</span><span class="sxs-lookup"><span data-stu-id="eb98a-142">See the [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="eb98a-143">Параметры конфигурации можно использовать для:</span><span class="sxs-lookup"><span data-stu-id="eb98a-143">Configuration options can be used to:</span></span>

* <span data-ttu-id="eb98a-144">Запрос различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="eb98a-144">Request different information about the user.</span></span>
* <span data-ttu-id="eb98a-145">Добавьте аргументы строки запроса, чтобы настроить имя входа в систему.</span><span class="sxs-lookup"><span data-stu-id="eb98a-145">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="eb98a-146">Войдите с помощью Facebook</span><span class="sxs-lookup"><span data-stu-id="eb98a-146">Sign in with Facebook</span></span>

<span data-ttu-id="eb98a-147">Запустите приложение и нажмите кнопку **входа**.</span><span class="sxs-lookup"><span data-stu-id="eb98a-147">Run your application and click **Log in**.</span></span> <span data-ttu-id="eb98a-148">Появится возможность войти в Facebook.</span><span class="sxs-lookup"><span data-stu-id="eb98a-148">You see an option to sign in with Facebook.</span></span>

![Веб-приложения: пользователь не прошел проверку подлинности](index/_static/DoneFacebook.png)

<span data-ttu-id="eb98a-150">При нажатии кнопки на **Facebook**, вы будете перенаправлены на Facebook для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="eb98a-150">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Страница проверки подлинности Facebook](index/_static/FBLogin.png)

<span data-ttu-id="eb98a-152">Открытый профиль и адрес электронной почты запросы проверки подлинности Facebook по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="eb98a-152">Facebook authentication requests public profile and email address by default:</span></span>

![Страница проверки подлинности Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="eb98a-154">После ввода учетных данных Facebook вы будете перенаправлены обратно на сайт, где вы можете задать ваш адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="eb98a-154">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="eb98a-155">Вы вошли в Facebook учетные данные с помощью:</span><span class="sxs-lookup"><span data-stu-id="eb98a-155">You are now logged in using your Facebook credentials:</span></span>

![Веб-приложения: пользователь прошел проверку подлинности](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="eb98a-157">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="eb98a-157">Troubleshooting</span></span>

* <span data-ttu-id="eb98a-158">**ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*.</span><span class="sxs-lookup"><span data-stu-id="eb98a-158">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="eb98a-159">Шаблон проекта, используемые в этом учебнике гарантирует, что происходит.</span><span class="sxs-lookup"><span data-stu-id="eb98a-159">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="eb98a-160">Если не был создан путем применения первоначальной миграции базы данных сайта, вы получаете *не удалось выполнить операцию базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="eb98a-160">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="eb98a-161">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="eb98a-161">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eb98a-162">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="eb98a-162">Next steps</span></span>

* <span data-ttu-id="eb98a-163">В этой статье показано, как можно выполнить проверку подлинности с Facebook.</span><span class="sxs-lookup"><span data-stu-id="eb98a-163">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="eb98a-164">Можно выполнить подобный подход для проверки подлинности для других поставщиков на [предыдущую страницу](index.md).</span><span class="sxs-lookup"><span data-stu-id="eb98a-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="eb98a-165">После публикации веб-сайте Azure веб-приложения, необходимо переустановить `AppSecret` на портале разработчика Facebook.</span><span class="sxs-lookup"><span data-stu-id="eb98a-165">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="eb98a-166">Задать `Authentication:Facebook:AppId` и `Authentication:Facebook:AppSecret` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="eb98a-166">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="eb98a-167">Система конфигурации настраивается для чтения ключи из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="eb98a-167">The configuration system is set up to read keys from environment variables.</span></span>
