---
title: Настройка внешней учетной записи Facebook в ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграцию Facebook учетной записи пользователя и проверки подлинности в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/facebook-logins
ms.openlocfilehash: 8bb22dc6df9879e827ff9a5ac11e9e3ad5346dc2
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121509"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="5b336-103">Настройка внешней учетной записи Facebook в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b336-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="5b336-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="5b336-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5b336-105">Этом руководстве показано, как включить пользователей выполнить вход с использованием учетной записи Facebook, используя образец проекта ASP.NET Core 2.0, созданные на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="5b336-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="5b336-106">Требует проверки подлинности Facebook [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="5b336-106">Facebook authentication requires the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package.</span></span> <span data-ttu-id="5b336-107">Мы начнем с создания код приложения Facebook, следуя [официальный действия](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="5b336-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="5b336-108">Создать приложение в Facebook</span><span class="sxs-lookup"><span data-stu-id="5b336-108">Create the app in Facebook</span></span>

* <span data-ttu-id="5b336-109">Перейдите к [приложений разработчиков для Facebook](https://developers.facebook.com/apps/) странице и войдите в систему.</span><span class="sxs-lookup"><span data-stu-id="5b336-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="5b336-110">Если у вас нет учетной записи Facebook, используйте **зарегистрироваться для Facebook** ссылку на странице входа, чтобы создать его.</span><span class="sxs-lookup"><span data-stu-id="5b336-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="5b336-111">Коснитесь **добавить новое приложение** кнопки в правом верхнем углу, чтобы создать новый идентификатор приложения.</span><span class="sxs-lookup"><span data-stu-id="5b336-111">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook для портала разработчиков открыть в Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="5b336-113">Заполните форму и коснитесь **создать идентификатор приложения** кнопки.</span><span class="sxs-lookup"><span data-stu-id="5b336-113">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Создание формы новый идентификатор приложения](index/_static/FBNewAppId.png)

* <span data-ttu-id="5b336-115">На **выбрать продукт** щелкните **Настройка** на **имени для входа Facebook** карты.</span><span class="sxs-lookup"><span data-stu-id="5b336-115">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

  ![Страница установки продукта](index/_static/FBProductSetup.png)

* <span data-ttu-id="5b336-117">**Быстрого запуска** будет запущен мастер с **выберите платформу** первая страница.</span><span class="sxs-lookup"><span data-stu-id="5b336-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="5b336-118">Сейчас пропустить мастера, нажав кнопку **параметры** ссылку в меню слева:</span><span class="sxs-lookup"><span data-stu-id="5b336-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

  ![Пропустить быстрый запуск](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="5b336-120">Появится **клиентские настройки OAuth** страницы:</span><span class="sxs-lookup"><span data-stu-id="5b336-120">You are presented with the **Client OAuth Settings** page:</span></span>

  ![Параметры OAuth клиента](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="5b336-122">Введите URI разработки с */signin-facebook* добавляется в **допустимый URI перенаправления OAuth** поле (например: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="5b336-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="5b336-123">Проверка подлинности Facebook, Настройка описывается далее в этом руководстве автоматически будет обрабатывать запросы на */signin-facebook* маршрут, чтобы реализовать поток OAuth.</span><span class="sxs-lookup"><span data-stu-id="5b336-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="5b336-124">URI */signin-facebook* задан в качестве обратного вызова по умолчанию поставщика проверки подлинности Facebook.</span><span class="sxs-lookup"><span data-stu-id="5b336-124">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="5b336-125">URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Facebook с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) класс.</span><span class="sxs-lookup"><span data-stu-id="5b336-125">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="5b336-126">Нажмите кнопку **сохранить изменения**.</span><span class="sxs-lookup"><span data-stu-id="5b336-126">Click **Save Changes**.</span></span>

* <span data-ttu-id="5b336-127">Нажмите кнопку **параметры** > **основные** ссылку в левой области навигации.</span><span class="sxs-lookup"><span data-stu-id="5b336-127">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="5b336-128">На этой странице, запомните или запишите вашей `App ID` и `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="5b336-128">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="5b336-129">Оба сертификата в приложении ASP.NET Core будет добавлена в следующем разделе:</span><span class="sxs-lookup"><span data-stu-id="5b336-129">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="5b336-130">При развертывании на сайте необходимо пересмотреть **имени для входа Facebook** установки: страница и зарегистрировать новый открытый универсальный код Ресурса.</span><span class="sxs-lookup"><span data-stu-id="5b336-130">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="5b336-131">Идентификатор приложения Facebook Store и секрет приложения</span><span class="sxs-lookup"><span data-stu-id="5b336-131">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="5b336-132">Связать конфиденциальные параметры, такие как Facebook `App ID` и `App Secret` для конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="5b336-132">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="5b336-133">В целях этого учебника назовите токены `Authentication:Facebook:AppId` и `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="5b336-133">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="5b336-134">Выполните следующие команды, чтобы обеспечить безопасное хранение `App ID` и `App Secret` с помощью диспетчера секретов:</span><span class="sxs-lookup"><span data-stu-id="5b336-134">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="5b336-135">Настройка проверки подлинности Facebook</span><span class="sxs-lookup"><span data-stu-id="5b336-135">Configure Facebook Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b336-136">Добавьте службу Facebook в `ConfigureServices` метод в *Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="5b336-136">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

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

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5b336-137">Установка [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) пакета.</span><span class="sxs-lookup"><span data-stu-id="5b336-137">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="5b336-138">Установить этот пакет с помощью Visual Studio 2017, щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5b336-138">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="5b336-139">Чтобы установить с помощью интерфейса командной строки .NET Core, выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="5b336-139">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="5b336-140">Добавьте по промежуточного слоя Facebook в `Configure` метод в *Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="5b336-140">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="5b336-141">См. в разделе [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) Справочник по API, Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности Facebook.</span><span class="sxs-lookup"><span data-stu-id="5b336-141">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="5b336-142">Параметры конфигурации может использоваться для:</span><span class="sxs-lookup"><span data-stu-id="5b336-142">Configuration options can be used to:</span></span>

* <span data-ttu-id="5b336-143">Запросить различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="5b336-143">Request different information about the user.</span></span>
* <span data-ttu-id="5b336-144">Добавление аргументов строки запроса, настраивать параметры имени входа.</span><span class="sxs-lookup"><span data-stu-id="5b336-144">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="5b336-145">Вход с помощью Facebook</span><span class="sxs-lookup"><span data-stu-id="5b336-145">Sign in with Facebook</span></span>

<span data-ttu-id="5b336-146">Запустите приложение и нажмите кнопку **вход**.</span><span class="sxs-lookup"><span data-stu-id="5b336-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="5b336-147">Отображается параметр выполнить вход с использованием Facebook.</span><span class="sxs-lookup"><span data-stu-id="5b336-147">You see an option to sign in with Facebook.</span></span>

![Веб-приложения: пользователь не прошел проверку подлинности](index/_static/DoneFacebook.png)

<span data-ttu-id="5b336-149">Когда вы щелкаете **Facebook**, вы будете перенаправлены на Facebook для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="5b336-149">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Страница проверки подлинности Facebook](index/_static/FBLogin.png)

<span data-ttu-id="5b336-151">Проверка подлинности Facebook запрашивает открытый профиль и адрес электронной почты по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="5b336-151">Facebook authentication requests public profile and email address by default:</span></span>

![Экран согласия страницу проверки подлинности Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="5b336-153">После ввода учетных данных Facebook вы будете перенаправлены обратно на сайт, где вы можете задать свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="5b336-153">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="5b336-154">Теперь вы вошли с использованием учетных данных Facebook:</span><span class="sxs-lookup"><span data-stu-id="5b336-154">You are now logged in using your Facebook credentials:</span></span>

![Веб-приложения: пользователь прошел проверку подлинности](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="5b336-156">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="5b336-156">Troubleshooting</span></span>

* <span data-ttu-id="5b336-157">**ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*.</span><span class="sxs-lookup"><span data-stu-id="5b336-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="5b336-158">Шаблон проекта, используемый в этом руководстве гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="5b336-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="5b336-159">Если база данных сайта не был создан путем применения первоначальной миграции, вы получаете *сбой операции из базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="5b336-159">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="5b336-160">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="5b336-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b336-161">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="5b336-161">Next steps</span></span>

* <span data-ttu-id="5b336-162">В этой статье объясняется, как можно выполнить проверку подлинности с помощью Facebook.</span><span class="sxs-lookup"><span data-stu-id="5b336-162">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="5b336-163">Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="5b336-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="5b336-164">После публикации веб-сайт веб-приложение Azure, необходимо сбросить `AppSecret` на портале разработчика Facebook.</span><span class="sxs-lookup"><span data-stu-id="5b336-164">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="5b336-165">Задайте `Authentication:Facebook:AppId` и `Authentication:Facebook:AppSecret` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="5b336-165">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="5b336-166">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="5b336-166">The configuration system is set up to read keys from environment variables.</span></span>
