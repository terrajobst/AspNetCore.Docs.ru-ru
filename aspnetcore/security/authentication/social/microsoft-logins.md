---
title: Настройка внешней учетной записи учетной записи Майкрософт с помощью ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграции проверки подлинности пользователя учетной записи Майкрософт в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 4909a0084994654777ad7a6ebda866ac727f0528
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735756"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="8efb5-103">Настройка внешней учетной записи учетной записи Майкрософт с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8efb5-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="8efb5-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="8efb5-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8efb5-105">Этом руководстве показано, как включить пользователей выполнить вход с использованием своей учетной записью Майкрософт, используя образец проекта ASP.NET Core 2.0, созданные на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8efb5-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="8efb5-106">Создание приложения на портале разработчика Майкрософт</span><span class="sxs-lookup"><span data-stu-id="8efb5-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="8efb5-107">Перейдите к [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) и создать или войти в учетную запись Майкрософт:</span><span class="sxs-lookup"><span data-stu-id="8efb5-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![Диалоговое окно входа](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="8efb5-109">Если у вас нет учетной записи Майкрософт, коснитесь  **[создайте ее!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="8efb5-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="8efb5-110">После входа вы будете перенаправлены на **моих приложений** страницы:</span><span class="sxs-lookup"><span data-stu-id="8efb5-110">After signing in you are redirected to **My applications** page:</span></span>

![Портал разработчиков Microsoft открыть в Microsoft Edge](index/_static/MicrosoftDev.png)

* <span data-ttu-id="8efb5-112">Коснитесь **Добавление веб-приложения** в верхнем правом углу, а затем введите ваш **имя_приложения** и **контактный адрес электронной почты**:</span><span class="sxs-lookup"><span data-stu-id="8efb5-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Диалоговое окно создания регистрации приложения](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="8efb5-114">Для целей данного учебника, снимите **руководство по настройке** "флажок".</span><span class="sxs-lookup"><span data-stu-id="8efb5-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="8efb5-115">Коснитесь **создать** продолжать **регистрации** страницы.</span><span class="sxs-lookup"><span data-stu-id="8efb5-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="8efb5-116">Укажите **имя** и обратите внимание на значение **идентификатор приложения**, который можно использовать в качестве `ClientId` далее в этом руководстве:</span><span class="sxs-lookup"><span data-stu-id="8efb5-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Страница регистрации](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="8efb5-118">Коснитесь **Добавление платформы** в **платформ** и выберите **Web** платформы:</span><span class="sxs-lookup"><span data-stu-id="8efb5-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Добавить диалоговое окно платформы](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="8efb5-120">В новом **Web** платформы введите свой URL-адрес разработки с `/signin-microsoft` добавляется в **URL-адреса перенаправления** поле (например: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="8efb5-120">In the new **Web** platform section, enter your development URL with `/signin-microsoft` appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="8efb5-121">Схема проверки подлинности Майкрософт, Настройка описывается далее в этом руководстве автоматически будет обрабатывать запросы на `/signin-microsoft` маршрута, чтобы реализовать поток OAuth:</span><span class="sxs-lookup"><span data-stu-id="8efb5-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow:</span></span>

![Веб-платформы раздел](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> <span data-ttu-id="8efb5-123">Сегмент URI `/signin-microsoft` задан в качестве обратного вызова по умолчанию поставщика проверки подлинности Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="8efb5-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="8efb5-124">URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Майкрософт с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) класса.</span><span class="sxs-lookup"><span data-stu-id="8efb5-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

* <span data-ttu-id="8efb5-125">Коснитесь **Добавление URL-адреса** чтобы URL-адрес был добавлен.</span><span class="sxs-lookup"><span data-stu-id="8efb5-125">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="8efb5-126">Заполните все остальные параметры приложения, при необходимости и коснитесь **Сохранить** в нижней части страницы, чтобы сохранить изменения в конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="8efb5-126">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="8efb5-127">При развертывании на сайте необходимо будет пересмотреть **регистрации** странице и задайте новый общедоступный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="8efb5-127">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="8efb5-128">Store Microsoft Application Id и пароль</span><span class="sxs-lookup"><span data-stu-id="8efb5-128">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="8efb5-129">Примечание `Application Id` на **регистрации** страницы.</span><span class="sxs-lookup"><span data-stu-id="8efb5-129">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="8efb5-130">Коснитесь **создать новый пароль** в **секретов приложения** раздел.</span><span class="sxs-lookup"><span data-stu-id="8efb5-130">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="8efb5-131">Откроется окно, где можно скопировать пароль приложения:</span><span class="sxs-lookup"><span data-stu-id="8efb5-131">This displays a box where you can copy the application password:</span></span>

![Диалоговое окно создания пароля, созданных](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="8efb5-133">Связать конфиденциальные параметры, такие как Microsoft `Application ID` и `Password` для конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="8efb5-133">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="8efb5-134">В целях этого учебника назовите токены `Authentication:Microsoft:ApplicationId` и `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="8efb5-134">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="8efb5-135">Настройка проверки подлинности учетной записи Майкрософт</span><span class="sxs-lookup"><span data-stu-id="8efb5-135">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="8efb5-136">Шаблон проекта, используемый в этом руководстве гарантирует, что [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) пакет уже установлен.</span><span class="sxs-lookup"><span data-stu-id="8efb5-136">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="8efb5-137">Установить этот пакет с помощью Visual Studio 2017, щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8efb5-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="8efb5-138">Чтобы установить с помощью интерфейса командной строки .NET Core, выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="8efb5-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8efb5-139">Добавление службы учетной записи Майкрософт в `ConfigureServices` метод в *Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="8efb5-139">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8efb5-140">Добавьте по промежуточного слоя учетной записи Майкрософт в `Configure` метод в *Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="8efb5-140">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

<span data-ttu-id="8efb5-141">Несмотря на то, что терминология, используемая на портале разработчиков Майкрософт называет эти маркеры `ApplicationId` и `Password`, они виде `ClientId` и `ClientSecret` в конфигурацию API.</span><span class="sxs-lookup"><span data-stu-id="8efb5-141">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="8efb5-142">См. в разделе [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Справочник по API, Дополнительные сведения о параметрах конфигурации, поддерживается проверка подлинности учетной записи Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="8efb5-142">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="8efb5-143">Это может использоваться для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="8efb5-143">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="8efb5-144">Войдите с помощью учетной записи Майкрософт</span><span class="sxs-lookup"><span data-stu-id="8efb5-144">Sign in with Microsoft Account</span></span>

<span data-ttu-id="8efb5-145">Запустите приложение и нажмите кнопку **вход**.</span><span class="sxs-lookup"><span data-stu-id="8efb5-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="8efb5-146">Появится возможность входа с учетной записью Майкрософт:</span><span class="sxs-lookup"><span data-stu-id="8efb5-146">An option to sign in with Microsoft appears:</span></span>

![Веб-приложение журнала на странице: Пользователь не прошел проверку подлинности](index/_static/DoneMicrosoft.png)

<span data-ttu-id="8efb5-148">Если щелкнуть on Microsoft, вы будете перенаправлены в корпорацию Майкрософт для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="8efb5-148">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="8efb5-149">После входа под учетной записью Майкрософт (если это не сделано) вам будет предложено разрешить приложению доступ к вашим сведениям:</span><span class="sxs-lookup"><span data-stu-id="8efb5-149">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Диалоговое окно проверки подлинности Майкрософт](index/_static/MicrosoftLogin.png)

<span data-ttu-id="8efb5-151">Коснитесь **Да** и вы будете перенаправлены обратно на веб-узел, где вы можете задать свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="8efb5-151">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="8efb5-152">Теперь вы вошли с использованием учетных данных Майкрософт:</span><span class="sxs-lookup"><span data-stu-id="8efb5-152">You are now logged in using your Microsoft credentials:</span></span>

![Веб-приложение: Пользователь прошел проверку подлинности](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="8efb5-154">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="8efb5-154">Troubleshooting</span></span>

* <span data-ttu-id="8efb5-155">Если поставщик учетной записи Майкрософт вы будете перенаправлены к странице ошибки входа, обратите внимание, ошибка заголовок и описание параметров строки запроса сразу после `#` (хэштег) в Uri.</span><span class="sxs-lookup"><span data-stu-id="8efb5-155">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="8efb5-156">Несмотря на то, что сообщение об ошибке кажется указывают на проблему с использованием проверки подлинности, наиболее распространенной причиной является Uri, не соответствующих ни одному из приложения **URI перенаправления** для **Web** платформы .</span><span class="sxs-lookup"><span data-stu-id="8efb5-156">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="8efb5-157">**ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: Необходимо указать параметр «SignInScheme»*.</span><span class="sxs-lookup"><span data-stu-id="8efb5-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="8efb5-158">Шаблон проекта, используемый в этом руководстве гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="8efb5-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="8efb5-159">Если база данных сайта не был создан путем применения первоначальной миграции, вы получите *сбой операции из базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="8efb5-159">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="8efb5-160">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="8efb5-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8efb5-161">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="8efb5-161">Next steps</span></span>

* <span data-ttu-id="8efb5-162">В этой статье объясняется, как можно выполнить проверку подлинности с корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="8efb5-162">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="8efb5-163">Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8efb5-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="8efb5-164">После публикации веб-сайт веб-приложение Azure, необходимо создать новый `Password` на портале разработчика Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8efb5-164">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="8efb5-165">Задайте `Authentication:Microsoft:ApplicationId` и `Authentication:Microsoft:Password` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="8efb5-165">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="8efb5-166">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="8efb5-166">The configuration system is set up to read keys from environment variables.</span></span>
