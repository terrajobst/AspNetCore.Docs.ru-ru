---
title: "Программа установки внешней учетной записи Google в ASP.NET Core"
author: rick-anderson
description: "Программа установки внешней учетной записи Google в ASP.NET Core"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/google-logins
ms.openlocfilehash: 8723a74250ff1b0a63139057bfc17fdd31dd169e
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="configuring-google-authentication-in-aspnet-core"></a><span data-ttu-id="70015-104">Настройка проверки подлинности Google в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70015-104">Configuring Google authentication in ASP.NET Core</span></span>

<a name=security-authentication-google-logins></a>

<span data-ttu-id="70015-105">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="70015-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="70015-106">Этого учебника показано, как предоставить пользователям возможность войти в их Google + учетную запись с помощью ASP.NET 2.0 основной пример проекта создан на [предыдущую страницу](index.md).</span><span class="sxs-lookup"><span data-stu-id="70015-106">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="70015-107">Мы начать со следующих [официальный действия](https://developers.google.com/identity/sign-in/web/devconsole-project) для создания нового приложения в консоли Google API.</span><span class="sxs-lookup"><span data-stu-id="70015-107">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="70015-108">Создание приложения в консоли Google API</span><span class="sxs-lookup"><span data-stu-id="70015-108">Create the app in Google API Console</span></span>

* <span data-ttu-id="70015-109">Перейдите к [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) и выполните вход.</span><span class="sxs-lookup"><span data-stu-id="70015-109">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="70015-110">Если у вас еще нет учетной записи Google, используйте **Дополнительные параметры** > **[создать учетную запись](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  ссылку, чтобы создать один:</span><span class="sxs-lookup"><span data-stu-id="70015-110">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Консоли Google API](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="70015-112">Вы будете перенаправлены на **библиотека API диспетчера** страницы:</span><span class="sxs-lookup"><span data-stu-id="70015-112">You are redirected to **API Manager Library** page:</span></span>

![Библиотека API диспетчера страницы](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="70015-114">Коснитесь **создать** и введите ваш **имя проекта**:</span><span class="sxs-lookup"><span data-stu-id="70015-114">Tap **Create** and enter your **Project name**:</span></span>

![Диалоговое окно создания нового проекта](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="70015-116">После принятия диалоговое окно, вы будете перенаправлены на страницу библиотеки, в которой можно выбрать компоненты для нового приложения.</span><span class="sxs-lookup"><span data-stu-id="70015-116">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="70015-117">Найти **API Google +** в списке и щелкните ссылку на нее, чтобы добавить компонент API:</span><span class="sxs-lookup"><span data-stu-id="70015-117">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Библиотека API диспетчера страницы](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="70015-119">Откроется страница для вновь добавленного API.</span><span class="sxs-lookup"><span data-stu-id="70015-119">The page for the newly added API is displayed.</span></span> <span data-ttu-id="70015-120">Коснитесь **включить** Добавление приложения Google + знак в функции:</span><span class="sxs-lookup"><span data-stu-id="70015-120">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![Страница диспетчера API Google + API](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="70015-122">После включения API, коснитесь **создать учетные данные** Настройка секреты:</span><span class="sxs-lookup"><span data-stu-id="70015-122">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![Страница диспетчера API Google + API](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="70015-124">Выберите:</span><span class="sxs-lookup"><span data-stu-id="70015-124">Choose:</span></span>
   * <span data-ttu-id="70015-125">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="70015-125">**Google+ API**</span></span>
   * <span data-ttu-id="70015-126">**Веб-сервера (например node.js, Tomcat)**, и</span><span class="sxs-lookup"><span data-stu-id="70015-126">**Web server (e.g. node.js, Tomcat)**, and</span></span>
   * <span data-ttu-id="70015-127">**Данные пользователя**:</span><span class="sxs-lookup"><span data-stu-id="70015-127">**User data**:</span></span>

![Страница API диспетчера учетных данных: Узнайте, что тип учетных данных вы должны панели](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="70015-129">Коснитесь **какие учетные данные нужны?** которой осуществляется переход ко второму шагу Конфигурация приложения **создать идентификатор клиента OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="70015-129">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![Страница API диспетчера учетных данных: создать идентификатор клиента OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="70015-131">Поскольку мы создаем с какой-либо функции (вход), мы можете ввести тот же проект Google + **имя** для идентификатор клиента OAuth 2.0, что мы использовали для проекта.</span><span class="sxs-lookup"><span data-stu-id="70015-131">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="70015-132">Введите URI разработки с */signin-google* добавляется в **авторизованные URI перенаправления** поля (например: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="70015-132">Enter your development URI with */signin-google* appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="70015-133">Проверка подлинности Google далее в этом учебнике автоматически будет обрабатывать запросы на */signin-google* маршрута для реализации потока OAuth.</span><span class="sxs-lookup"><span data-stu-id="70015-133">The Google authentication configured later in this tutorial will automatically handle requests at */signin-google* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="70015-134">Нажмите клавишу TAB, чтобы добавить **авторизованные URI перенаправления** входа.</span><span class="sxs-lookup"><span data-stu-id="70015-134">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="70015-135">Коснитесь **создать идентификатор клиента**, которой осуществляется переход к третьему этапу **настроить экран согласия OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="70015-135">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![Страница API диспетчера учетных данных: настроить экран согласия OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="70015-137">Введите ваш общедоступным **адрес электронной почты** и **название продукта** показано для своего приложения Google + пользователю войти в систему по запросу.</span><span class="sxs-lookup"><span data-stu-id="70015-137">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="70015-138">Дополнительные параметры доступны в разделе **Дополнительные параметры настройки**.</span><span class="sxs-lookup"><span data-stu-id="70015-138">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="70015-139">Коснитесь **Продолжить** для перейдите к последнему шагу, **загрузить учетные данные**:</span><span class="sxs-lookup"><span data-stu-id="70015-139">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![Страница API диспетчера учетных данных: загрузить учетные данные](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="70015-141">Коснитесь **загрузки** для сохранения файла JSON с секретами приложения и **сделать** для завершения создания нового приложения.</span><span class="sxs-lookup"><span data-stu-id="70015-141">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="70015-142">При развертывании узла необходимо будет повторно **консоли Google** и зарегистрировать новый общедоступный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="70015-142">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="70015-143">Магазин Google ClientID и ClientSecret</span><span class="sxs-lookup"><span data-stu-id="70015-143">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="70015-144">Связать конфиденциальные параметры, например Google `Client ID` и `Client Secret` в конфигурации приложения с помощью [секрет диспетчер](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="70015-144">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="70015-145">В целях этого учебника назовите токены `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="70015-145">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="70015-146">Значения для этих маркеров можно найти в JSON-файл, загруженный в предыдущем шаге, в `web.client_id` и `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="70015-146">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="70015-147">Настройка проверки подлинности Google</span><span class="sxs-lookup"><span data-stu-id="70015-147">Configure Google Authentication</span></span>

<span data-ttu-id="70015-148">Шаблон проекта, используемые в этом учебнике гарантирует, что [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) пакет установлен.</span><span class="sxs-lookup"><span data-stu-id="70015-148">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

 * <span data-ttu-id="70015-149">Чтобы установить этот пакет с 2017 г. для Visual Studio, щелкните правой кнопкой мыши проект и выберите пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="70015-149">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
 * <span data-ttu-id="70015-150">Чтобы установить с .NET Core CLI, выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="70015-150">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="70015-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="70015-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="70015-152">Добавление службы Google в `ConfigureServices` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="70015-152">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="70015-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="70015-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="70015-154">Добавить по промежуточного слоя Google в `Configure` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="70015-154">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

<span data-ttu-id="70015-155">В разделе [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) Справочник по API для дополнительных сведений о параметрах конфигурации, поддерживаемых проверкой подлинности Google.</span><span class="sxs-lookup"><span data-stu-id="70015-155">See the [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="70015-156">Это можно использовать для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="70015-156">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="70015-157">Вход Google</span><span class="sxs-lookup"><span data-stu-id="70015-157">Sign in with Google</span></span>

<span data-ttu-id="70015-158">Запустите приложение и нажмите кнопку **входа**.</span><span class="sxs-lookup"><span data-stu-id="70015-158">Run your application and click **Log in**.</span></span> <span data-ttu-id="70015-159">Появляется возможность войти с помощью Google:</span><span class="sxs-lookup"><span data-stu-id="70015-159">An option to sign in with Google appears:</span></span>

![Веб-приложение работает в Microsoft Edge: пользователь не прошел проверку подлинности](index/_static/DoneGoogle.png)

<span data-ttu-id="70015-161">При нажатии кнопки на Google, вы будете перенаправлены на Google для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="70015-161">When you click on Google, you are redirected to Google for authentication:</span></span>

![Диалоговое окно проверки подлинности Google](index/_static/GoogleLogin.png)

<span data-ttu-id="70015-163">После ввода учетных данных Google, затем вы будете перенаправлены к веб-сайта, где вы можете задать ваш адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="70015-163">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="70015-164">Будет выполнен вход с помощью учетных данных Google:</span><span class="sxs-lookup"><span data-stu-id="70015-164">You are now logged in using your Google credentials:</span></span>

![Веб-приложение работает в Microsoft Edge: пользователь прошел проверку подлинности](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="70015-166">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="70015-166">Troubleshooting</span></span>

* <span data-ttu-id="70015-167">При получении `403 (Forbidden)` страницы ошибки из вашего собственного приложения, когда работает в режиме разработки (или приостановки выполнения в отладчике с той же ошибки), убедитесь, что **API Google +** была включена в **библиотека API диспетчера** , выполнив следующие действия [ранее на этой странице](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="70015-167">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="70015-168">Если вход не работает, а не получают все ошибки, переключитесь в режим разработки для упрощения процесса отладки проблему.</span><span class="sxs-lookup"><span data-stu-id="70015-168">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="70015-169">**ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*.</span><span class="sxs-lookup"><span data-stu-id="70015-169">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="70015-170">Шаблон проекта, используемые в этом учебнике гарантирует, что происходит.</span><span class="sxs-lookup"><span data-stu-id="70015-170">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="70015-171">Если не был создан путем применения первоначальной миграции базы данных сайта, вы получите *не удалось выполнить операцию базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="70015-171">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="70015-172">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="70015-172">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="70015-173">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="70015-173">Next steps</span></span>

* <span data-ttu-id="70015-174">В этой статье показано, как можно выполнить проверку подлинности с помощью Google.</span><span class="sxs-lookup"><span data-stu-id="70015-174">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="70015-175">Можно выполнить подобный подход для проверки подлинности для других поставщиков на [предыдущую страницу](index.md).</span><span class="sxs-lookup"><span data-stu-id="70015-175">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="70015-176">После публикации веб-сайте Azure веб-приложения, необходимо переустановить `ClientSecret` в консоли Google API.</span><span class="sxs-lookup"><span data-stu-id="70015-176">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="70015-177">Задать `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="70015-177">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="70015-178">Система конфигурации настраивается для чтения ключи из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="70015-178">The configuration system is set up to read keys from environment variables.</span></span>
