---
title: Настройка внешнего входа Facebook в ASP.NET Core
author: rick-anderson
description: Руководство с примерами кода, демонстрирующими интеграцию аутентификации пользователя с учетной записью Facebook с существующим ASP.NET Core приложением.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 12/02/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: 2e4cc04c6e7ff8e5f5701cc7f9ede73dbc1b4685
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717140"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="7d808-103">Настройка внешнего входа Facebook в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d808-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="7d808-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="7d808-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7d808-105">В этом учебнике с примерами кода показано, как включить вход пользователей с помощью учетной записи Facebook, используя пример проекта ASP.NET Core 3,0, созданный на [предыдущей странице](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7d808-105">This tutorial with code examples shows how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="7d808-106">Начнем с создания идентификатора приложения Facebook, следуя [официальным действиям](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="7d808-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="7d808-107">Создание приложения в Facebook</span><span class="sxs-lookup"><span data-stu-id="7d808-107">Create the app in Facebook</span></span>

* <span data-ttu-id="7d808-108">Добавьте в проект пакет NuGet [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) .</span><span class="sxs-lookup"><span data-stu-id="7d808-108">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to the project.</span></span>

* <span data-ttu-id="7d808-109">Перейдите на страницу [приложения Facebook Developers](https://developers.facebook.com/apps/) и выполните вход.</span><span class="sxs-lookup"><span data-stu-id="7d808-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="7d808-110">Если у вас еще нет учетной записи Facebook, используйте ссылку **зарегистрироваться для Facebook** на странице входа, чтобы создать ее.</span><span class="sxs-lookup"><span data-stu-id="7d808-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>  <span data-ttu-id="7d808-111">После получения учетной записи Facebook следуйте инструкциям по регистрации в качестве разработчика Facebook.</span><span class="sxs-lookup"><span data-stu-id="7d808-111">Once you have a Facebook account, follow the instructions to register as a Facebook Developer.</span></span>

* <span data-ttu-id="7d808-112">В меню **Мои приложения** выберите пункт **создать приложение** , чтобы создать новый идентификатор приложения.</span><span class="sxs-lookup"><span data-stu-id="7d808-112">From the **My Apps** menu select **Create App** to create a new App ID.</span></span>

   ![Портал Facebook для разработчиков открыт в Microsoft ребр](index/_static/FBMyApps.png)

* <span data-ttu-id="7d808-114">Заполните форму и нажмите кнопку **CREATE App ID (создать идентификатор приложения** ).</span><span class="sxs-lookup"><span data-stu-id="7d808-114">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Создание новой формы идентификатора приложения](index/_static/FBNewAppId.png)

* <span data-ttu-id="7d808-116">В карточке нового приложения выберите **Добавить продукт**.</span><span class="sxs-lookup"><span data-stu-id="7d808-116">On the new App card, select **Add a Product**.</span></span>  <span data-ttu-id="7d808-117">На карточке **входа Facebook** щелкните **настроить** .</span><span class="sxs-lookup"><span data-stu-id="7d808-117">On the **Facebook Login** card, click **Set Up**</span></span> 

  ![Страница «Настройка продукта»](index/_static/FBProductSetup.png)

* <span data-ttu-id="7d808-119">Мастер **быстрого** запуска запускается с параметром **выбрать платформу** в качестве первой страницы.</span><span class="sxs-lookup"><span data-stu-id="7d808-119">The **Quickstart** wizard launches with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="7d808-120">Пропустите мастер, щелкнув ссылку **Параметры** **входа Facebook** в меню в левом нижнем углу экрана:</span><span class="sxs-lookup"><span data-stu-id="7d808-120">Bypass the wizard for now by clicking the **FaceBook Login** **Settings** link in the menu on the lower left:</span></span>

  ![Пропустить Быстрое начало](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="7d808-122">Отобразится страница **параметров OAuth клиента** :</span><span class="sxs-lookup"><span data-stu-id="7d808-122">You are presented with the **Client OAuth Settings** page:</span></span>

  ![Страница параметров OAuth клиента](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="7d808-124">Введите универсальный код ресурса (URI) для разработки с */сигнин-фацебук* , добавленным в **допустимое поле URI перенаправления OAuth** (например: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="7d808-124">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="7d808-125">Проверка подлинности Facebook, настроенная далее в этом руководстве, автоматически обрабатывает запросы по маршруту */сигнин-фацебук* для реализации потока OAuth.</span><span class="sxs-lookup"><span data-stu-id="7d808-125">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="7d808-126">URI */сигнин-фацебук* задается в качестве обратного вызова по умолчанию для поставщика проверки подлинности Facebook.</span><span class="sxs-lookup"><span data-stu-id="7d808-126">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="7d808-127">URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Facebook с помощью унаследованного свойства [ремотеаусентикатионоптионс. каллбаккпас](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) класса [фацебукоптионс](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) .</span><span class="sxs-lookup"><span data-stu-id="7d808-127">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="7d808-128">Нажмите кнопку **сохранить изменения**.</span><span class="sxs-lookup"><span data-stu-id="7d808-128">Click **Save Changes**.</span></span>

* <span data-ttu-id="7d808-129">В левой области навигации щелкните **параметры** > **Обычная** ссылка.</span><span class="sxs-lookup"><span data-stu-id="7d808-129">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="7d808-130">На этой странице запишите `App ID` и `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="7d808-130">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="7d808-131">Вы добавите оба варианта в приложение ASP.NET Core в следующем разделе:</span><span class="sxs-lookup"><span data-stu-id="7d808-131">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="7d808-132">При развертывании сайта необходимо повторно посетить страницу настройки **имени входа Facebook** и зарегистрировать новый общедоступный URI.</span><span class="sxs-lookup"><span data-stu-id="7d808-132">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="7d808-133">Хранение идентификатора приложения Facebook и секрета приложения</span><span class="sxs-lookup"><span data-stu-id="7d808-133">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="7d808-134">Свяжите конфиденциальные параметры, такие как Facebook `App ID` и `App Secret` в конфигурации приложения с помощью [диспетчера секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7d808-134">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="7d808-135">В рамках этого руководства назовите маркеры `Authentication:Facebook:AppId` и `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="7d808-135">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="7d808-136">Выполните следующие команды для безопасного хранения `App ID` и `App Secret` с помощью диспетчера секретов:</span><span class="sxs-lookup"><span data-stu-id="7d808-136">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="7d808-137">Настройка проверки подлинности Facebook</span><span class="sxs-lookup"><span data-stu-id="7d808-137">Configure Facebook Authentication</span></span>

<span data-ttu-id="7d808-138">Добавьте службу Facebook в метод `ConfigureServices` в файле *Startup.CS* :</span><span class="sxs-lookup"><span data-stu-id="7d808-138">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="7d808-139">Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности Facebook, см. в справочнике по API [фацебукоптионс](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) .</span><span class="sxs-lookup"><span data-stu-id="7d808-139">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="7d808-140">Параметры конфигурации можно использовать для:</span><span class="sxs-lookup"><span data-stu-id="7d808-140">Configuration options can be used to:</span></span>

* <span data-ttu-id="7d808-141">Запросите другие сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="7d808-141">Request different information about the user.</span></span>
* <span data-ttu-id="7d808-142">Добавьте аргументы строки запроса, чтобы настроить процедуру входа.</span><span class="sxs-lookup"><span data-stu-id="7d808-142">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="7d808-143">Вход с помощью Facebook</span><span class="sxs-lookup"><span data-stu-id="7d808-143">Sign in with Facebook</span></span>

<span data-ttu-id="7d808-144">Запустите приложение и нажмите кнопку **войти**.</span><span class="sxs-lookup"><span data-stu-id="7d808-144">Run your application and click **Log in**.</span></span> <span data-ttu-id="7d808-145">Вы увидите параметр для входа с помощью Facebook.</span><span class="sxs-lookup"><span data-stu-id="7d808-145">You see an option to sign in with Facebook.</span></span>

![Веб-приложение: пользователь не прошел проверку подлинности](index/_static/DoneFacebook.png)

<span data-ttu-id="7d808-147">Если щелкнуть **Facebook**, вы будете перенаправлены на Facebook для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="7d808-147">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Страница проверки подлинности Facebook](index/_static/FBLogin.png)

<span data-ttu-id="7d808-149">По умолчанию запросы проверки подлинности Facebook запрашивают открытый профиль и адрес электронной почты:</span><span class="sxs-lookup"><span data-stu-id="7d808-149">Facebook authentication requests public profile and email address by default:</span></span>

![Экран согласия проверки подлинности страницы Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="7d808-151">После ввода учетных данных Facebook вы перенаправляетесь обратно на сайт, где можно задать свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7d808-151">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="7d808-152">Теперь вы выполнили вход с использованием учетных данных Facebook:</span><span class="sxs-lookup"><span data-stu-id="7d808-152">You are now logged in using your Facebook credentials:</span></span>

![Веб-приложение: проверка подлинности пользователя](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="7d808-154">Диагностика</span><span class="sxs-lookup"><span data-stu-id="7d808-154">Troubleshooting</span></span>

* <span data-ttu-id="7d808-155">**Только ASP.NET Core 2. x:** Если удостоверение не настроено путем вызова `services.AddIdentity` в `ConfigureServices`, попытка проверки подлинности приведет к появлению *исключения ArgumentException: необходимо указать параметр "сигнинсчеме"* .</span><span class="sxs-lookup"><span data-stu-id="7d808-155">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="7d808-156">Шаблон проекта, используемый в этом руководстве, гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="7d808-156">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="7d808-157">Если база данных сайта не была создана путем применения первоначальной миграции, то при обработке ошибки запроса возникнет *Ошибка операции с базой данных* .</span><span class="sxs-lookup"><span data-stu-id="7d808-157">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="7d808-158">Выберите **Применить миграции** , чтобы создать базу данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="7d808-158">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d808-159">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="7d808-159">Next steps</span></span>

* <span data-ttu-id="7d808-160">В этой статье показано, как можно пройти проверку подлинности с помощью Facebook.</span><span class="sxs-lookup"><span data-stu-id="7d808-160">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="7d808-161">Аналогичный подход можно использовать для проверки подлинности с другими поставщиками, перечисленными на [предыдущей странице](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7d808-161">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="7d808-162">После публикации веб-сайта в веб-приложение Azure необходимо сбросить `AppSecret` на портале разработчика Facebook.</span><span class="sxs-lookup"><span data-stu-id="7d808-162">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="7d808-163">Задайте `Authentication:Facebook:AppId` и `Authentication:Facebook:AppSecret` в качестве параметров приложения в портал Azure.</span><span class="sxs-lookup"><span data-stu-id="7d808-163">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="7d808-164">Система конфигурации настроена на чтение ключей из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="7d808-164">The configuration system is set up to read keys from environment variables.</span></span>
