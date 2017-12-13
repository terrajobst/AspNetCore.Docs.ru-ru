---
title: "Программа установки внешнее имя входа учетной записи Майкрософт"
author: rick-anderson
description: "В этом учебнике показано интеграции проверки подлинности пользователя учетной записи Майкрософт в существующее приложение ASP.NET Core."
keywords: "ASP.NET Core, учетной записи Майкрософт, имя входа, проверка подлинности"
ms.author: riande
manager: wpickett
ms.date: 08/24/2017
ms.topic: article
ms.assetid: 66DB4B94-C78C-4005-BA03-3D982B87C268
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 77c16e3ae93c9bfe1f569d0a5888c5b765d04241
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="configuring-microsoft-account-authentication"></a><span data-ttu-id="c0d77-104">Настройка проверки подлинности учетной записи Майкрософт</span><span class="sxs-lookup"><span data-stu-id="c0d77-104">Configuring Microsoft Account authentication</span></span>

<span data-ttu-id="c0d77-105">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="c0d77-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c0d77-106">Этого учебника показано, как предоставить пользователям возможность войти в учетную запись Майкрософт с помощью ASP.NET 2.0 основной пример проекта создан на [предыдущую страницу](index.md).</span><span class="sxs-lookup"><span data-stu-id="c0d77-106">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="c0d77-107">Создать приложение на портале для разработчиков Microsoft</span><span class="sxs-lookup"><span data-stu-id="c0d77-107">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="c0d77-108">Перейдите к [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) и создать или войти в учетную запись Майкрософт:</span><span class="sxs-lookup"><span data-stu-id="c0d77-108">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![В диалоговом окне входа](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="c0d77-110">Если у вас нет учетной записи Майкрософт, коснитесь  **[создать!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="c0d77-110">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="c0d77-111">После входа вы попадете на **Мои приложения** страницы:</span><span class="sxs-lookup"><span data-stu-id="c0d77-111">After signing in you are redirected to **My applications** page:</span></span>

![Портал разработчиков Microsoft открыть в Microsoft Edge](index/_static/MicrosoftDev.png)

* <span data-ttu-id="c0d77-113">Коснитесь **Добавление приложения** в правом верхнем углу, а затем введите ваш **имя_приложения** и **адрес электронной почты**:</span><span class="sxs-lookup"><span data-stu-id="c0d77-113">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Новое диалоговое окно регистрации приложения](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="c0d77-115">Для целей этого учебника, снимите **интерактивной установки** флажок.</span><span class="sxs-lookup"><span data-stu-id="c0d77-115">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="c0d77-116">Коснитесь **создать** продолжать **регистрации** страницы.</span><span class="sxs-lookup"><span data-stu-id="c0d77-116">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="c0d77-117">Укажите **имя** и обратите внимание на значение **идентификатор приложения**, который используется в качестве `ClientId` далее в этом учебнике:</span><span class="sxs-lookup"><span data-stu-id="c0d77-117">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Страница регистрации](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="c0d77-119">Коснитесь **добавить платформу** в **платформы** , выберите **Web** платформа:</span><span class="sxs-lookup"><span data-stu-id="c0d77-119">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Добавить диалоговое окно платформы](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="c0d77-121">В новом **Web** платформы введите URL-адрес разработки с */signin-microsoft* добавляется в **URL-адреса перенаправления** поля (например: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="c0d77-121">In the new **Web** platform section, enter your development URL with */signin-microsoft* appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="c0d77-122">Схема проверки подлинности Microsoft настроен далее в этом учебнике автоматически будет обрабатывать запросы на */signin-microsoft* маршрута для реализации потока OAuth:</span><span class="sxs-lookup"><span data-stu-id="c0d77-122">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at */signin-microsoft* route to implement the OAuth flow:</span></span>

![Веб-платформы раздела](index/_static/MicrosoftRedirectUri.png)

* <span data-ttu-id="c0d77-124">Коснитесь **Добавление URL-адреса** чтобы URL-адрес был добавлен.</span><span class="sxs-lookup"><span data-stu-id="c0d77-124">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="c0d77-125">Заполните все прочие параметры приложения, при необходимости и коснитесь **Сохранить** в нижней части страницы, чтобы сохранить изменения в конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="c0d77-125">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="c0d77-126">При развертывании узла необходимо будет повторно **регистрации** и задайте новый общедоступный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="c0d77-126">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="c0d77-127">Сохранить код приложения Майкрософт и пароль</span><span class="sxs-lookup"><span data-stu-id="c0d77-127">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="c0d77-128">Примечание `Application Id` отображаются на **регистрации** страницы.</span><span class="sxs-lookup"><span data-stu-id="c0d77-128">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="c0d77-129">Коснитесь **создать новый пароль** в **секреты приложения** раздела.</span><span class="sxs-lookup"><span data-stu-id="c0d77-129">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="c0d77-130">Откроется окно, где можно скопировать пароль приложения:</span><span class="sxs-lookup"><span data-stu-id="c0d77-130">This displays a box where you can copy the application password:</span></span>

![Новый пароль, созданный диалоговое окно](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="c0d77-132">Связать конфиденциальные параметры, такие как Microsoft `Application ID` и `Password` в конфигурации приложения с помощью [секрет диспетчер](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="c0d77-132">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="c0d77-133">В целях этого учебника назовите токены `Authentication:Microsoft:ApplicationId` и `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="c0d77-133">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="c0d77-134">Настройка проверки подлинности учетной записи Майкрософт</span><span class="sxs-lookup"><span data-stu-id="c0d77-134">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="c0d77-135">Шаблон проекта, используемые в этом учебнике гарантирует, что [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) пакет уже установлен.</span><span class="sxs-lookup"><span data-stu-id="c0d77-135">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="c0d77-136">Чтобы установить этот пакет с 2017 г. для Visual Studio, щелкните правой кнопкой мыши проект и выберите пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c0d77-136">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="c0d77-137">Чтобы установить с .NET Core CLI, выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="c0d77-137">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0d77-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0d77-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c0d77-139">Добавление службы учетной записи Майкрософт в `ConfigureServices` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="c0d77-139">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0d77-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0d77-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0d77-141">Добавьте учетную запись Майкрософт по промежуточного слоя в `Configure` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="c0d77-141">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

<span data-ttu-id="c0d77-142">Несмотря на то, что эти токены имена терминологию, используемую на портал разработчиков Microsoft `ApplicationId` и `Password`, они представляются как `ClientId` и `ClientSecret` для API-Интерфейс настройки.</span><span class="sxs-lookup"><span data-stu-id="c0d77-142">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they are exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="c0d77-143">В разделе [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Справочник по API для дополнительных сведений о параметрах конфигурации, поддерживается проверка подлинности учетной записи Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="c0d77-143">See the [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="c0d77-144">Это можно использовать для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="c0d77-144">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="c0d77-145">Войдите с учетной записью Майкрософт</span><span class="sxs-lookup"><span data-stu-id="c0d77-145">Sign in with Microsoft Account</span></span>

<span data-ttu-id="c0d77-146">Запустите приложение и нажмите кнопку **входа**.</span><span class="sxs-lookup"><span data-stu-id="c0d77-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="c0d77-147">Появляется возможность войти в Microsoft:</span><span class="sxs-lookup"><span data-stu-id="c0d77-147">An option to sign in with Microsoft appears:</span></span>

![Веб-приложение журнала страницы: пользователь не прошел проверку подлинности](index/_static/DoneMicrosoft.png)

<span data-ttu-id="c0d77-149">При нажатии кнопки на Microsoft, вы будете перенаправлены в корпорацию Майкрософт для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="c0d77-149">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="c0d77-150">После входа под учетной записью Майкрософт (если он еще не выполнили вход) будет предложено разрешить приложению доступ к вашим данным:</span><span class="sxs-lookup"><span data-stu-id="c0d77-150">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Диалоговое окно проверки подлинности Майкрософт](index/_static/MicrosoftLogin.png)

<span data-ttu-id="c0d77-152">Коснитесь **Да** вы будете перенаправлены к веб-сайта, где вы можете задать ваш адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="c0d77-152">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="c0d77-153">Вы вошли в, используя учетные данные Microsoft:</span><span class="sxs-lookup"><span data-stu-id="c0d77-153">You are now logged in using your Microsoft credentials:</span></span>

![Веб-приложения: пользователь прошел проверку подлинности](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="c0d77-155">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="c0d77-155">Troubleshooting</span></span>

* <span data-ttu-id="c0d77-156">Если поставщик учетной записи Майкрософт вы будете перенаправлены на страницу ошибки входа, обратите внимание, ошибка заголовок и описание параметров строки запроса непосредственно за `#` (хэштегом) в Uri.</span><span class="sxs-lookup"><span data-stu-id="c0d77-156">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="c0d77-157">Несмотря на то, что сообщение об ошибке, кажется указывают на проблему с использованием проверки подлинности, наиболее распространенной причиной является Uri, не соответствует ни одному из приложения **URI перенаправления** указано **Web** платформы .</span><span class="sxs-lookup"><span data-stu-id="c0d77-157">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="c0d77-158">**ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*.</span><span class="sxs-lookup"><span data-stu-id="c0d77-158">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="c0d77-159">Шаблон проекта, используемые в этом учебнике гарантирует, что происходит.</span><span class="sxs-lookup"><span data-stu-id="c0d77-159">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="c0d77-160">Если не был создан путем применения первоначальной миграции базы данных сайта, вы получите *не удалось выполнить операцию базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="c0d77-160">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="c0d77-161">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="c0d77-161">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0d77-162">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="c0d77-162">Next steps</span></span>

* <span data-ttu-id="c0d77-163">В этой статье показано, как можно выполнить проверку подлинности с помощью Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c0d77-163">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="c0d77-164">Можно выполнить подобный подход для проверки подлинности для других поставщиков на [предыдущую страницу](index.md).</span><span class="sxs-lookup"><span data-stu-id="c0d77-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="c0d77-165">После публикации веб-узла веб-приложение Azure, следует создать новый `Password` на портале разработчиков Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="c0d77-165">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="c0d77-166">Задать `Authentication:Microsoft:ApplicationId` и `Authentication:Microsoft:Password` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="c0d77-166">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="c0d77-167">Система конфигурации настраивается для чтения ключи из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="c0d77-167">The configuration system is set up to read keys from environment variables.</span></span>
