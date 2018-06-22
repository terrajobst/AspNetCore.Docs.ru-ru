---
title: Программа установки внешней учетной записи Google в ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграции проверки подлинности пользователя учетной записи Google в существующее приложение ASP.NET Core.
ms.author: riande
ms.date: 08/02/2017
uid: security/authentication/google-logins
ms.openlocfilehash: c5b6c992e134a2c4f0314d9d6e0465e6228c54ee
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274915"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="5eac8-103">Программа установки внешней учетной записи Google в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5eac8-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="5eac8-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="5eac8-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5eac8-105">Этого учебника показано, как предоставить пользователям возможность войти в их Google + учетную запись с помощью ASP.NET 2.0 основной пример проекта создан на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="5eac8-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="5eac8-106">Мы начать со следующих [официальный действия](https://developers.google.com/identity/sign-in/web/devconsole-project) для создания нового приложения в консоли Google API.</span><span class="sxs-lookup"><span data-stu-id="5eac8-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="5eac8-107">Создание приложения в консоли Google API</span><span class="sxs-lookup"><span data-stu-id="5eac8-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="5eac8-108">Перейдите к [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) и выполните вход.</span><span class="sxs-lookup"><span data-stu-id="5eac8-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="5eac8-109">Если у вас еще нет учетной записи Google, используйте **Дополнительные параметры** > **[создать учетную запись](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  ссылку, чтобы создать один:</span><span class="sxs-lookup"><span data-stu-id="5eac8-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Консоли Google API](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="5eac8-111">Вы будете перенаправлены на **библиотека API диспетчера** страницы:</span><span class="sxs-lookup"><span data-stu-id="5eac8-111">You are redirected to **API Manager Library** page:</span></span>

![Библиотека API диспетчера страницы](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="5eac8-113">Коснитесь **создать** и введите ваш **имя проекта**:</span><span class="sxs-lookup"><span data-stu-id="5eac8-113">Tap **Create** and enter your **Project name**:</span></span>

![Диалоговое окно создания нового проекта](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="5eac8-115">После принятия диалоговое окно, вы будете перенаправлены на страницу библиотеки, в которой можно выбрать компоненты для нового приложения.</span><span class="sxs-lookup"><span data-stu-id="5eac8-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="5eac8-116">Найти **API Google +** в списке и щелкните ссылку на нее, чтобы добавить компонент API:</span><span class="sxs-lookup"><span data-stu-id="5eac8-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Библиотека API диспетчера страницы](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="5eac8-118">Откроется страница для вновь добавленного API.</span><span class="sxs-lookup"><span data-stu-id="5eac8-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="5eac8-119">Коснитесь **включить** Добавление приложения Google + знак в функции:</span><span class="sxs-lookup"><span data-stu-id="5eac8-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![Страница диспетчера API Google + API](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="5eac8-121">После включения API, коснитесь **создать учетные данные** Настройка секреты:</span><span class="sxs-lookup"><span data-stu-id="5eac8-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![Страница диспетчера API Google + API](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="5eac8-123">Выберите:</span><span class="sxs-lookup"><span data-stu-id="5eac8-123">Choose:</span></span>
   * <span data-ttu-id="5eac8-124">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="5eac8-124">**Google+ API**</span></span>
   * <span data-ttu-id="5eac8-125">**Веб-сервера (например node.js, Tomcat)**, и</span><span class="sxs-lookup"><span data-stu-id="5eac8-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
   * <span data-ttu-id="5eac8-126">**Данные пользователя**:</span><span class="sxs-lookup"><span data-stu-id="5eac8-126">**User data**:</span></span>

![Страница API диспетчера учетных данных: Узнайте, что тип учетных данных вы должны панели](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="5eac8-128">Коснитесь **какие учетные данные нужны?** которой осуществляется переход ко второму шагу Конфигурация приложения **создать идентификатор клиента OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="5eac8-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![Страница API диспетчера учетных данных: создать идентификатор клиента OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="5eac8-130">Поскольку мы создаем с какой-либо функции (вход), мы можете ввести тот же проект Google + **имя** для идентификатор клиента OAuth 2.0, что мы использовали для проекта.</span><span class="sxs-lookup"><span data-stu-id="5eac8-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="5eac8-131">Введите URI разработки с `/signin-google` добавляется в **авторизованные URI перенаправления** поля (например: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="5eac8-131">Enter your development URI with `/signin-google` appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="5eac8-132">Проверка подлинности Google далее в этом учебнике автоматически будет обрабатывать запросы на `/signin-google` маршрута для реализации потока OAuth.</span><span class="sxs-lookup"><span data-stu-id="5eac8-132">The Google authentication configured later in this tutorial will automatically handle requests at `/signin-google` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="5eac8-133">Сегмент URI `/signin-google` задан в качестве обратного вызова по умолчанию поставщика проверки подлинности Google.</span><span class="sxs-lookup"><span data-stu-id="5eac8-133">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="5eac8-134">URI обратного вызова по умолчанию можно изменить во время настройки по промежуточного слоя проверки подлинности Google через наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) класса.</span><span class="sxs-lookup"><span data-stu-id="5eac8-134">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

* <span data-ttu-id="5eac8-135">Нажмите клавишу TAB, чтобы добавить **авторизованные URI перенаправления** входа.</span><span class="sxs-lookup"><span data-stu-id="5eac8-135">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="5eac8-136">Коснитесь **создать идентификатор клиента**, которой осуществляется переход к третьему этапу **настроить экран согласия OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="5eac8-136">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![Страница API диспетчера учетных данных: настроить экран согласия OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="5eac8-138">Введите ваш общедоступным **адрес электронной почты** и **название продукта** показано для своего приложения Google + пользователю войти в систему по запросу.</span><span class="sxs-lookup"><span data-stu-id="5eac8-138">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="5eac8-139">Дополнительные параметры доступны в разделе **Дополнительные параметры настройки**.</span><span class="sxs-lookup"><span data-stu-id="5eac8-139">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="5eac8-140">Коснитесь **Продолжить** для перейдите к последнему шагу, **загрузить учетные данные**:</span><span class="sxs-lookup"><span data-stu-id="5eac8-140">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![Страница API диспетчера учетных данных: загрузить учетные данные](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="5eac8-142">Коснитесь **загрузки** для сохранения файла JSON с секретами приложения и **сделать** для завершения создания нового приложения.</span><span class="sxs-lookup"><span data-stu-id="5eac8-142">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="5eac8-143">При развертывании узла необходимо будет повторно **консоли Google** и зарегистрировать новый общедоступный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="5eac8-143">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="5eac8-144">Магазин Google ClientID и ClientSecret</span><span class="sxs-lookup"><span data-stu-id="5eac8-144">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="5eac8-145">Связать конфиденциальные параметры, например Google `Client ID` и `Client Secret` в конфигурации приложения с помощью [секрет диспетчер](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="5eac8-145">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="5eac8-146">В целях этого учебника назовите токены `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="5eac8-146">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="5eac8-147">Значения для этих маркеров можно найти в JSON-файл, загруженный в предыдущем шаге, в `web.client_id` и `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="5eac8-147">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="5eac8-148">Настройка проверки подлинности Google</span><span class="sxs-lookup"><span data-stu-id="5eac8-148">Configure Google Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5eac8-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5eac8-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="5eac8-150">Добавление службы Google в `ConfigureServices` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="5eac8-150">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5eac8-151">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5eac8-151">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="5eac8-152">Шаблон проекта, используемые в этом учебнике гарантирует, что [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) пакет установлен.</span><span class="sxs-lookup"><span data-stu-id="5eac8-152">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

* <span data-ttu-id="5eac8-153">Чтобы установить этот пакет с 2017 г. для Visual Studio, щелкните правой кнопкой мыши проект и выберите пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5eac8-153">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="5eac8-154">Чтобы установить с .NET Core CLI, выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="5eac8-154">To install with .NET Core CLI, execute the following in your project directory:</span></span>

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="5eac8-155">Добавить по промежуточного слоя Google в `Configure` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="5eac8-155">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

<span data-ttu-id="5eac8-156">В разделе [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) Справочник по API для дополнительных сведений о параметрах конфигурации, поддерживаемых проверкой подлинности Google.</span><span class="sxs-lookup"><span data-stu-id="5eac8-156">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="5eac8-157">Это можно использовать для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="5eac8-157">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="5eac8-158">Вход Google</span><span class="sxs-lookup"><span data-stu-id="5eac8-158">Sign in with Google</span></span>

<span data-ttu-id="5eac8-159">Запустите приложение и нажмите кнопку **входа**.</span><span class="sxs-lookup"><span data-stu-id="5eac8-159">Run your application and click **Log in**.</span></span> <span data-ttu-id="5eac8-160">Появляется возможность войти с помощью Google:</span><span class="sxs-lookup"><span data-stu-id="5eac8-160">An option to sign in with Google appears:</span></span>

![Веб-приложение работает в Microsoft Edge: пользователь не прошел проверку подлинности](index/_static/DoneGoogle.png)

<span data-ttu-id="5eac8-162">При нажатии кнопки на Google, вы будете перенаправлены на Google для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="5eac8-162">When you click on Google, you are redirected to Google for authentication:</span></span>

![Диалоговое окно проверки подлинности Google](index/_static/GoogleLogin.png)

<span data-ttu-id="5eac8-164">После ввода учетных данных Google, затем вы будете перенаправлены к веб-сайта, где вы можете задать ваш адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="5eac8-164">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="5eac8-165">Будет выполнен вход с помощью учетных данных Google:</span><span class="sxs-lookup"><span data-stu-id="5eac8-165">You are now logged in using your Google credentials:</span></span>

![Веб-приложение работает в Microsoft Edge: пользователь прошел проверку подлинности](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="5eac8-167">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="5eac8-167">Troubleshooting</span></span>

* <span data-ttu-id="5eac8-168">При получении `403 (Forbidden)` страницы ошибки из вашего собственного приложения, когда работает в режиме разработки (или приостановки выполнения в отладчике с той же ошибки), убедитесь, что **API Google +** была включена в **библиотека API диспетчера** , выполнив следующие действия [ранее на этой странице](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="5eac8-168">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="5eac8-169">Если вход не работает, а не получают все ошибки, переключитесь в режим разработки для упрощения процесса отладки проблему.</span><span class="sxs-lookup"><span data-stu-id="5eac8-169">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="5eac8-170">**ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*.</span><span class="sxs-lookup"><span data-stu-id="5eac8-170">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="5eac8-171">Шаблон проекта, используемые в этом учебнике гарантирует, что происходит.</span><span class="sxs-lookup"><span data-stu-id="5eac8-171">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="5eac8-172">Если не был создан путем применения первоначальной миграции базы данных сайта, вы получите *не удалось выполнить операцию базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="5eac8-172">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="5eac8-173">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="5eac8-173">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5eac8-174">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="5eac8-174">Next steps</span></span>

* <span data-ttu-id="5eac8-175">В этой статье показано, как можно выполнить проверку подлинности с помощью Google.</span><span class="sxs-lookup"><span data-stu-id="5eac8-175">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="5eac8-176">Можно выполнить подобный подход для проверки подлинности для других поставщиков на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="5eac8-176">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="5eac8-177">После публикации веб-сайте Azure веб-приложения, необходимо переустановить `ClientSecret` в консоли Google API.</span><span class="sxs-lookup"><span data-stu-id="5eac8-177">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="5eac8-178">Задать `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="5eac8-178">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="5eac8-179">Система конфигурации настраивается для чтения ключи из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="5eac8-179">The configuration system is set up to read keys from environment variables.</span></span>
