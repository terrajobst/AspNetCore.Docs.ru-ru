---
title: Настройка внешней учетной записи Google в ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграцию Google учетной записи пользователя и проверки подлинности в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: 372504eb4f6fea412b5b160e0d5e9251dafe0d56
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284498"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="53722-103">Настройка внешней учетной записи Google в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53722-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="53722-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="53722-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="53722-105">Этом руководстве показано, как разрешить пользователям вход с помощью своей учетной записи Google + используя образец проекта ASP.NET Core 2.0, созданные на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="53722-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="53722-106">Мы начнем, следуя [официальный действия](https://developers.google.com/identity/sign-in/web/devconsole-project) для создания нового приложения в консоли Google API.</span><span class="sxs-lookup"><span data-stu-id="53722-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="53722-107">Создание приложения в консоли Google API</span><span class="sxs-lookup"><span data-stu-id="53722-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="53722-108">Перейдите к [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) и войдите в систему.</span><span class="sxs-lookup"><span data-stu-id="53722-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="53722-109">Если у вас нет учетной записи Google, используйте **Дополнительные параметры** > **[создать учетную запись](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  ссылку, чтобы создать его:</span><span class="sxs-lookup"><span data-stu-id="53722-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Консоли Google API](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="53722-111">Вы будете перенаправлены на **библиотека API диспетчера** страницы:</span><span class="sxs-lookup"><span data-stu-id="53722-111">You are redirected to **API Manager Library** page:</span></span>

![Целевая на странице библиотеки API диспетчера](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="53722-113">Коснитесь **создать** и введите ваш **имя_проекта**:</span><span class="sxs-lookup"><span data-stu-id="53722-113">Tap **Create** and enter your **Project name**:</span></span>

![Диалоговое окно создания нового проекта](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="53722-115">После принятия диалогового окна, вы будете перенаправлены на страницу библиотеки, в которой можно выбрать функции для нового приложения.</span><span class="sxs-lookup"><span data-stu-id="53722-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="53722-116">Найти **Google + API** в списке и щелкните ссылку на нее, чтобы добавить компонент API:</span><span class="sxs-lookup"><span data-stu-id="53722-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Поиск «Google + API» на странице библиотеки API диспетчера](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="53722-118">Откроется страница для вновь добавленного API.</span><span class="sxs-lookup"><span data-stu-id="53722-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="53722-119">Коснитесь **включить** Добавление Google + знак в компонент приложения:</span><span class="sxs-lookup"><span data-stu-id="53722-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![Целевая на странице диспетчера API Google + API](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="53722-121">После включения API, коснитесь **Создание учетных данных** Настройка секреты:</span><span class="sxs-lookup"><span data-stu-id="53722-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![Создание кнопки учетные данные на странице диспетчера API Google + API](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="53722-123">Выберите:</span><span class="sxs-lookup"><span data-stu-id="53722-123">Choose:</span></span>
  * <span data-ttu-id="53722-124">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="53722-124">**Google+ API**</span></span>
  * <span data-ttu-id="53722-125">**Веб-сервера (например node.js, Tomcat)**, и</span><span class="sxs-lookup"><span data-stu-id="53722-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
  * <span data-ttu-id="53722-126">**Данные пользователя**:</span><span class="sxs-lookup"><span data-stu-id="53722-126">**User data**:</span></span>

![Страница API диспетчера учетных данных: Узнайте также довольно учетные данные, вы должны панели](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="53722-128">Коснитесь **какие учетные данные нужны?** , чтобы перейти на втором шаге Конфигурация приложения **создать идентификатор клиента OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="53722-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![Страница API диспетчера учетных данных: Создайте идентификатор клиента OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="53722-130">Поскольку мы создаем проект Google + со всего одной функцией (вход), мы можете ввести тот же **имя** для идентификатора клиента OAuth 2.0, который мы использовали для проекта.</span><span class="sxs-lookup"><span data-stu-id="53722-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="53722-131">Введите URI разработки с `/signin-google` добавляется в **авторизованные URI перенаправления** поле (например: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="53722-131">Enter your development URI with `/signin-google` appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="53722-132">Проверка подлинности Google настроена далее в этом руководстве автоматически будет обрабатывать запросы на `/signin-google` маршрута, чтобы реализовать поток OAuth.</span><span class="sxs-lookup"><span data-stu-id="53722-132">The Google authentication configured later in this tutorial will automatically handle requests at `/signin-google` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="53722-133">Сегмент URI `/signin-google` задан в качестве обратного вызова по умолчанию поставщик проверки подлинности Google.</span><span class="sxs-lookup"><span data-stu-id="53722-133">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="53722-134">URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Google с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) класса.</span><span class="sxs-lookup"><span data-stu-id="53722-134">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

* <span data-ttu-id="53722-135">Нажмите клавишу TAB, чтобы добавить **авторизованные URI перенаправления** запись.</span><span class="sxs-lookup"><span data-stu-id="53722-135">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="53722-136">Коснитесь **создать идентификатор клиента**, которой осуществляется переход к третьему этапу — **настроить экран согласия OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="53722-136">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![Страница API диспетчера учетных данных: Настроить экран согласия OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="53722-138">Введите ваш общедоступны **адрес электронной почты** и **название продукта** показано для вашего приложения, когда Google + предлагает пользователю войти в систему.</span><span class="sxs-lookup"><span data-stu-id="53722-138">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="53722-139">Доступны дополнительные параметры в разделе **возможностей настройки**.</span><span class="sxs-lookup"><span data-stu-id="53722-139">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="53722-140">Коснитесь **Продолжить** для перейдите к последнему шагу, **скачать учетные данные**:</span><span class="sxs-lookup"><span data-stu-id="53722-140">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![Страница API диспетчера учетных данных: Скачивание учетных данных](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="53722-142">Коснитесь **загрузить** сохранить файл JSON с помощью секретов приложения и **сделать** чтобы завершить создание нового приложения.</span><span class="sxs-lookup"><span data-stu-id="53722-142">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="53722-143">При развертывании на сайте необходимо будет пересмотреть **консоли Google** и зарегистрировать новый общедоступный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="53722-143">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="53722-144">Store Google ClientID и ClientSecret</span><span class="sxs-lookup"><span data-stu-id="53722-144">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="53722-145">Связать конфиденциальные параметры, такие как Google `Client ID` и `Client Secret` для конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="53722-145">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="53722-146">В целях этого учебника назовите токены `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="53722-146">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="53722-147">Значения для этих маркеров можно найти в JSON-файл, Скачанный на предыдущем шаге, в `web.client_id` и `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="53722-147">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="53722-148">Настройка проверки подлинности Google</span><span class="sxs-lookup"><span data-stu-id="53722-148">Configure Google Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="53722-149">Добавить службу Google в `ConfigureServices` метод в *Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="53722-149">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="53722-150">Шаблон проекта, используемый в этом руководстве гарантирует, что [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) установки пакета.</span><span class="sxs-lookup"><span data-stu-id="53722-150">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

* <span data-ttu-id="53722-151">Установить этот пакет с помощью Visual Studio 2017, щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="53722-151">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="53722-152">Чтобы установить с помощью интерфейса командной строки .NET Core, выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="53722-152">To install with .NET Core CLI, execute the following in your project directory:</span></span>

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="53722-153">Добавьте по промежуточного слоя Google в `Configure` метод в *Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="53722-153">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

<span data-ttu-id="53722-154">См. в разделе [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) Справочник по API, Дополнительные сведения о параметрах конфигурации, поддерживается проверка подлинности Google.</span><span class="sxs-lookup"><span data-stu-id="53722-154">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="53722-155">Это может использоваться для запроса различные сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="53722-155">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="53722-156">Войдите с помощью Google</span><span class="sxs-lookup"><span data-stu-id="53722-156">Sign in with Google</span></span>

<span data-ttu-id="53722-157">Запустите приложение и нажмите кнопку **вход**.</span><span class="sxs-lookup"><span data-stu-id="53722-157">Run your application and click **Log in**.</span></span> <span data-ttu-id="53722-158">Появится возможность войти с помощью Google:</span><span class="sxs-lookup"><span data-stu-id="53722-158">An option to sign in with Google appears:</span></span>

![Веб-приложения, работающего в Microsoft Edge: Пользователь не прошел проверку подлинности](index/_static/DoneGoogle.png)

<span data-ttu-id="53722-160">При выборе Google, вы будете перенаправлены на Google для проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="53722-160">When you click on Google, you are redirected to Google for authentication:</span></span>

![Диалоговое окно проверки подлинности Google](index/_static/GoogleLogin.png)

<span data-ttu-id="53722-162">После ввода учетных данных Google, затем вы будете перенаправлены обратно на веб-узел, где вы можете задать свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="53722-162">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="53722-163">Теперь вы вошли с использованием учетных данных Google:</span><span class="sxs-lookup"><span data-stu-id="53722-163">You are now logged in using your Google credentials:</span></span>

![Веб-приложения, работающего в Microsoft Edge: Пользователь прошел проверку подлинности](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="53722-165">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="53722-165">Troubleshooting</span></span>

* <span data-ttu-id="53722-166">Если появится `403 (Forbidden)` страницу ошибки из собственного приложения, когда работает в режиме разработки (или приостановки выполнения в отладчике с той же ошибкой), убедитесь, что **Google + API** была включена в **библиотека API диспетчера** , выполнив действия, описанные [более ранних версий на этой странице](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="53722-166">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="53722-167">Если вход не работает, а не получают все ошибки, переключитесь в режим разработки для упрощения процесса отладки проблемы.</span><span class="sxs-lookup"><span data-stu-id="53722-167">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="53722-168">**ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: Необходимо указать параметр «SignInScheme»*.</span><span class="sxs-lookup"><span data-stu-id="53722-168">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="53722-169">Шаблон проекта, используемый в этом руководстве гарантирует, что это будет сделано.</span><span class="sxs-lookup"><span data-stu-id="53722-169">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="53722-170">Если база данных сайта не был создан путем применения первоначальной миграции, вы получите *сбой операции из базы данных при обработке запроса* ошибки.</span><span class="sxs-lookup"><span data-stu-id="53722-170">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="53722-171">Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.</span><span class="sxs-lookup"><span data-stu-id="53722-171">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="53722-172">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="53722-172">Next steps</span></span>

* <span data-ttu-id="53722-173">В этой статье объясняется, как можно выполнить проверку подлинности с помощью Google.</span><span class="sxs-lookup"><span data-stu-id="53722-173">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="53722-174">Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="53722-174">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="53722-175">После публикации веб-сайт веб-приложение Azure, необходимо сбросить `ClientSecret` в консоли Google API.</span><span class="sxs-lookup"><span data-stu-id="53722-175">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="53722-176">Задайте `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret` как параметры приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="53722-176">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="53722-177">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="53722-177">The configuration system is set up to read keys from environment variables.</span></span>
