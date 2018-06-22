---
title: Программа установки внешнее имя входа учетной записи Майкрософт с ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграции проверки подлинности пользователя учетной записи Майкрософт в существующее приложение ASP.NET Core.
ms.author: riande
ms.date: 08/24/2017
uid: security/authentication/microsoft-logins
ms.openlocfilehash: cc4fe8c71b97d29cc6697e2aebf04694afb753ec
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272780"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="7dc63-103">Программа установки внешнее имя входа учетной записи Майкрософт с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7dc63-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="7dc63-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="7dc63-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7dc63-105">Этого учебника показано, как предоставить пользователям возможность войти в учетную запись Майкрософт с помощью ASP.NET 2.0 основной пример проекта создан на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7dc63-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="7dc63-106">Создать приложение на портале для разработчиков Microsoft</span><span class="sxs-lookup"><span data-stu-id="7dc63-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="7dc63-107">Перейдите к [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) и создать или войти в учетную запись Майкрософт:</span><span class="sxs-lookup"><span data-stu-id="7dc63-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![В диалоговом окне входа](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="7dc63-109">Если у вас нет учетной записи Майкрософт, коснитесь  **[создать!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="7dc63-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="7dc63-110">После входа вы попадете на **Мои приложения** страницы:</span><span class="sxs-lookup"><span data-stu-id="7dc63-110">After signing in you are redirected to **My applications** page:</span></span>

![Портал разработчиков Microsoft открыть в Microsoft Edge](index/_static/MicrosoftDev.png)

* <span data-ttu-id="7dc63-112">Коснитесь **Добавление приложения** в правом верхнем углу, а затем введите ваш **имя_приложения** и **адрес электронной почты**:</span><span class="sxs-lookup"><span data-stu-id="7dc63-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Новое диалоговое окно регистрации приложения](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="7dc63-114">Для целей этого учебника, снимите **интерактивной установки** флажок.</span><span class="sxs-lookup"><span data-stu-id="7dc63-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="7dc63-115">Коснитесь **создать** продолжать **регистрации** страницы.</span><span class="sxs-lookup"><span data-stu-id="7dc63-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="7dc63-116">Укажите **имя** и обратите внимание на значение **идентификатор приложения**, который используется в качестве `ClientId` далее в этом учебнике:</span><span class="sxs-lookup"><span data-stu-id="7dc63-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Страница регистрации](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="7dc63-118">Коснитесь **добавить платформу** в **платформы** , выберите **Web** платформа:</span><span class="sxs-lookup"><span data-stu-id="7dc63-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Добавить диалоговое окно платформы](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="7dc63-120">В новом **Web** платформы введите URL-адрес разработки с `/signin-microsoft` добавляется в **URL-адреса перенаправления** поля (например: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="7dc63-120">In the new **Web** platform section, enter your development URL with `/signin-microsoft` appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="7dc63-121">Схема проверки подлинности Microsoft настроен далее в этом учебнике автоматически будет обрабатывать запросы на `/signin-microsoft` маршрута для реализации потока OAuth:</span><span class="sxs-lookup"><span data-stu-id="7dc63-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow:</span></span>

![Веб-платформы раздела](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> <span data-ttu-id="7dc63-123">Сегмент URI `/signin-microsoft` задан в качестве обратного вызова по умолчанию поставщика проверки подлинности Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="7dc63-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="7dc63-124">URI обратного вызова по умолчанию можно изменить во время настройки по промежуточного слоя проверки подлинности Майкрософт через наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) класса.</span><span class="sxs-lookup"><span data-stu-id="7dc63-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

* <span data-ttu-id="7dc63-125">Коснитесь **Добавление URL-адреса** чтобы URL-адрес был добавлен.</span><span class="sxs-lookup"><span data-stu-id="7dc63-125">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="7dc63-126">Заполните все прочие параметры приложения, при необходимости и коснитесь **Сохранить** в нижней части страницы, чтобы сохранить изменения в конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="7dc63-126">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="7dc63-127">При развертывании узла необходимо будет повторно **регистрации** и задайте новый общедоступный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="7dc63-127">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="7dc63-128">Сохранить код приложения Майкрософт и пароль</span><span class="sxs-lookup"><span data-stu-id="7dc63-128">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="7dc63-129">Примечание `Application Id` отображаются на **регистрации** страницы.</span><span class="sxs-lookup"><span data-stu-id="7dc63-129">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="7dc63-130">Коснитесь **создать новый пароль** в **секреты приложения** раздела.</span><span class="sxs-lookup"><span data-stu-id="7dc63-130">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="7dc63-131">Откроется окно, где можно скопировать пароль приложения:</span><span class="sxs-lookup"><span data-stu-id="7dc63-131">This displays a box where you can copy the application password:</span></span>

![Новый пароль, созданный диалоговое окно](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="7dc63-133">Связать конфиденциальные параметры, такие как Microsoft `Application ID` и `Password` в конфигурации приложения с помощью [секрет диспетчер](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7dc63-133">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="7dc63-134">В целях этого учебника назовите токены `Authentication:Microsoft:ApplicationId` и `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="7dc63-134">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="7dc63-135">Настройка проверки подлинности учетной записи Майкрософт</span><span class="sxs-lookup"><span data-stu-id="7dc63-135">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="7dc63-136">Шаблон проекта, используемые в этом учебнике гарантирует, что [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) пакет уже установлен.</span><span class="sxs-lookup"><span data-stu-id="7dc63-136">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="7dc63-137">Чтобы установить этот пакет с 2017 г. для Visual Studio, щелкните правой кнопкой мыши проект и выберите пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7dc63-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="7dc63-138">Чтобы установить с .NET Core CLI, выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="7dc63-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7dc63-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7dc63-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="7dc63-140">Добавление службы учетной записи Майкрософт в `ConfigureServices` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="7dc63-140">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7dc63-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7dc63-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="7dc63-142">Добавьте учетную запись Майкрософт по промежуточного слоя в `Configure` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="7dc63-142">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

<span data-ttu-id="7dc63-143">Несмотря на то, что эти токены имена терминологию, используемую на портал разработчиков Microsoft `ApplicationId` и `Password`, они виде `ClientId` и `ClientSecret` для API-Интерфейс настройки.</span><span class="sxs-lookup"><span data-stu-id="7dc63-143">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="7dc63-144">В разделе [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Справочник по API для дополнительных сведений о параметрах конфигурации, поддерживается проверка подлинности учетной записи Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="7dc63-144">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="7dc63-145">Это можно использовать для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="7dc63-145">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="7dc63-146">Войдите с учетной записью Майкрософт</span><span class="sxs-lookup"><span data-stu-id="7dc63-146">Sign in with Microsoft Account</span></span>

<span data-ttu-id="7dc63-147">Запустите приложение и нажмите кнопку **входа**.</span><span class="sxs-lookup"><span data-stu-id="7dc63-147">Run your application and click **Log in**.</span></span> <span data-ttu-id="7dc63-148">Появляется возможность войти в Microsoft:</span><span class="sxs-lookup"><span data-stu-id="7dc63-148">An option to sign in with Microsoft appears:</span></span>

![Веб-приложение журнала страницы: пользователь не прошел проверку подлинности](index/_static/DoneMicrosoft.png)

<span data-ttu-id="7dc63-150">При нажатии кнопки на Microsoft, вы будете перенаправлены в корпорацию Майкрософт для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="7dc63-150">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="7dc63-151">После входа под учетной записью Майкрософт (если он еще не выполнили вход) будет предложено разрешить приложению доступ к вашим данным:</span><span class="sxs-lookup"><span data-stu-id="7dc63-151">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Диалоговое окно проверки подлинности Майкрософт](index/_static/MicrosoftLogin.png)

<span data-ttu-id="7dc63-153">Коснитесь **Да** вы будете перенаправлены к веб-сайта, где вы можете задать ваш адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7dc63-153">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="7dc63-154">Вы вошли в, используя учетные данные Microsoft:</span><span class="sxs-lookup"><span data-stu-id="7dc63-154">You are now logged in using your Microsoft credentials:</span></span>

![Веб-приложения: пользователь прошел проверку подлинности](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="7dc63-156">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="7dc63-156">Troubleshooting</span></span>

* <span data-ttu-id="7dc63-157">Если поставщик учетной записи Майкрософт вы будете перенаправлены на страницу ошибки входа, обратите внимание, ошибка заголовок и описание параметров строки запроса непосредственно за `#` (хэштегом) в Uri.</span><span class="sxs-lookup"><span data-stu-id="7dc63-157">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="7dc63-158">Несмотря на то, что сообщение об ошибке, кажется указывают на проблему с использованием проверки подлинности, наиболее распространенной причиной является Uri, не соответствует ни одному из приложения **URI перенаправления** указано **Web** платформы .</span><span class="sxs-lookup"><span data-stu-id="7dc63-158">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="7dc63-159">**ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*.</span><span class="sxs-lookup"><span data-stu-id="7dc63-159">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="7dc63-160">Шаблон проекта, используемые в этом учебнике гарантирует, что происходит.</span><span class="sxs-lookup"><span data-stu-id="7dc63-160">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="7dc63-161">Если не был создан путем применения первоначальной миграции базы данных сайта, вы получите *не удалось выполнить операцию базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="7dc63-161">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="7dc63-162">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="7dc63-162">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7dc63-163">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="7dc63-163">Next steps</span></span>

* <span data-ttu-id="7dc63-164">В этой статье показано, как можно выполнить проверку подлинности с помощью Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7dc63-164">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="7dc63-165">Можно выполнить подобный подход для проверки подлинности для других поставщиков на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7dc63-165">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="7dc63-166">После публикации веб-узла веб-приложение Azure, следует создать новый `Password` на портале разработчиков Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="7dc63-166">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="7dc63-167">Задать `Authentication:Microsoft:ApplicationId` и `Authentication:Microsoft:Password` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="7dc63-167">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="7dc63-168">Система конфигурации настраивается для чтения ключи из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="7dc63-168">The configuration system is set up to read keys from environment variables.</span></span>
