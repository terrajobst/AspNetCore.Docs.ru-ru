---
title: Подтверждение учетной записи и восстановление пароля в ASP.NET Core
author: rick-anderson
description: Сведения о создании приложения ASP.NET Core с помощью по электронной почте подтверждение и сброс пароля.
ms.author: riande
ms.date: 7/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 4c2e62335bc7dd004829dbc2a8c1f62ea91f334f
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2018
ms.locfileid: "50253043"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="822ce-103">Подтверждение учетной записи и восстановление пароля в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="822ce-103">Account confirmation and password recovery in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="822ce-104">См. в разделе [этот PDF-файл](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) для ASP.NET Core 1.1 и версия 2.1.</span><span class="sxs-lookup"><span data-stu-id="822ce-104">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="822ce-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="822ce-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="822ce-106">Этом руководстве описывается создание приложения ASP.NET Core с помощью по электронной почте подтверждение и сброс пароля.</span><span class="sxs-lookup"><span data-stu-id="822ce-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="822ce-107">Это руководство представляет собой **не** начало раздела.</span><span class="sxs-lookup"><span data-stu-id="822ce-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="822ce-108">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="822ce-108">You should be familiar with:</span></span>

* [<span data-ttu-id="822ce-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="822ce-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="822ce-110">Authentication</span><span class="sxs-lookup"><span data-stu-id="822ce-110">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="822ce-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="822ce-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="822ce-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="822ce-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="822ce-113">Создание веб-приложения и сформировать шаблон удостоверений</span><span class="sxs-lookup"><span data-stu-id="822ce-113">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="822ce-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="822ce-114">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="822ce-115">В Visual Studio создайте новое **веб-приложение** проект с именем **WebPWrecover**.</span><span class="sxs-lookup"><span data-stu-id="822ce-115">In Visual Studio, create a new **Web Application** project named **WebPWrecover**.</span></span>
* <span data-ttu-id="822ce-116">Выберите **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="822ce-116">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="822ce-117">Сохраните значение по умолчанию **проверки подлинности** присвоено **без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="822ce-117">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="822ce-118">На следующем шаге добавляется проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="822ce-118">Authentication is added in the next step.</span></span>

<span data-ttu-id="822ce-119">На следующем шаге:</span><span class="sxs-lookup"><span data-stu-id="822ce-119">In the next step:</span></span>

* <span data-ttu-id="822ce-120">Задайте страницу макета *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="822ce-120">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="822ce-121">Выберите *учетной записи: регистрация*</span><span class="sxs-lookup"><span data-stu-id="822ce-121">Select *Account/Register*</span></span>
* <span data-ttu-id="822ce-122">Создайте новый **класс контекста данных**</span><span class="sxs-lookup"><span data-stu-id="822ce-122">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="822ce-123">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="822ce-123">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet new webapp -o WebPWrecover
cd WebPWrecover
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -fi Account.Register -dc WebPWrecover.Models.WebPWrecoverContext
dotnet ef migrations add CreateIdentitySchema
dotnet ef database drop -f
dotnet ef database update
dotnet build
```

<span data-ttu-id="822ce-124">Запустите `dotnet aspnet-codegenerator identity --help` для получения справки о средстве формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="822ce-124">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="822ce-125">Следуйте инструкциям в [включить проверку подлинности](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="822ce-125">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="822ce-126">Добавление `app.UseAuthentication();` для `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="822ce-126">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="822ce-127">Добавление `<partial name="_LoginPartial" />` с файлами макетов.</span><span class="sxs-lookup"><span data-stu-id="822ce-127">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="822ce-128">Регистрация нового пользователя для тестирования</span><span class="sxs-lookup"><span data-stu-id="822ce-128">Test new user registration</span></span>

<span data-ttu-id="822ce-129">Запустите приложение, выберите **зарегистрировать** связать и регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="822ce-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="822ce-130">На этом этапе является только проверка на адрес электронной почты с [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) атрибута.</span><span class="sxs-lookup"><span data-stu-id="822ce-130">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="822ce-131">После отправки регистрации, вы вошли в приложение.</span><span class="sxs-lookup"><span data-stu-id="822ce-131">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="822ce-132">Далее в этом руководстве код обновляется, поэтому новые пользователи не могут войти, пока не проверяется их по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="822ce-132">Later in the tutorial, the code is updated so new users can't log in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="822ce-133">Обратите внимание, таблицы `EmailConfirmed` поле является `False`.</span><span class="sxs-lookup"><span data-stu-id="822ce-133">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="822ce-134">Вы можете использовать этот адрес электронной почты снова на следующем шаге, когда приложение отправляет сообщение электронной почты с подтверждением.</span><span class="sxs-lookup"><span data-stu-id="822ce-134">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="822ce-135">Щелкните правой кнопкой мыши в строке и выберите **удалить**.</span><span class="sxs-lookup"><span data-stu-id="822ce-135">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="822ce-136">Удаление псевдонима электронной почты упрощает в следующих шагах.</span><span class="sxs-lookup"><span data-stu-id="822ce-136">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="822ce-137">Требуется подтверждение по электронной почте</span><span class="sxs-lookup"><span data-stu-id="822ce-137">Require email confirmation</span></span>

<span data-ttu-id="822ce-138">Рекомендуется подтвердить адрес электронной почты новой регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="822ce-138">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="822ce-139">Отправить по электронной почте подтверждение помогает проверить, они не олицетворения кто-то другой (то есть они еще не зарегистрировано другого пользователя по электронной почте).</span><span class="sxs-lookup"><span data-stu-id="822ce-139">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="822ce-140">Предположим, что имеется дискуссионный форум и избежать "yli@example.com«из регистрации в качестве»nolivetto@contoso.com«.</span><span class="sxs-lookup"><span data-stu-id="822ce-140">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="822ce-141">Без подтверждение по электронной почте "nolivetto@contoso.com" могли получать нежелательные сообщения электронной почты из вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="822ce-141">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="822ce-142">Предположим, что пользователь случайно зарегистрирован как "ylo@example.com" и не заметили опечатку «yli».</span><span class="sxs-lookup"><span data-stu-id="822ce-142">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="822ce-143">Они не смогут использовать пароль восстановления, так как у приложения нет их правильный адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="822ce-143">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="822ce-144">Подтверждение по электронной почте предоставляет ограниченную защиту от программ-роботов.</span><span class="sxs-lookup"><span data-stu-id="822ce-144">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="822ce-145">Подтверждение по электронной почте не обеспечивают защиту от пользователей-злоумышленников с многих учетных записей электронной почты.</span><span class="sxs-lookup"><span data-stu-id="822ce-145">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="822ce-146">Обычно требуется запретить новым пользователям из учета данных для веб-сайт, прежде чем они получат подтвержден по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="822ce-146">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="822ce-147">Обновление *Areas/Identity/IdentityHostingStartup.cs* требуется подтвержден по электронной почте:</span><span class="sxs-lookup"><span data-stu-id="822ce-147">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="822ce-148">`config.SignIn.RequireConfirmedEmail = true;` запрещает вход до подтверждения их по электронной почте зарегистрированным пользователям.</span><span class="sxs-lookup"><span data-stu-id="822ce-148">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="822ce-149">Настройка поставщика услуг электронной почты</span><span class="sxs-lookup"><span data-stu-id="822ce-149">Configure email provider</span></span>

<span data-ttu-id="822ce-150">В этом руководстве [SendGrid](https://sendgrid.com) используется для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="822ce-150">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="822ce-151">Требуется учетная запись SendGrid и ключ для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="822ce-151">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="822ce-152">Можно использовать другие поставщики электронной почты.</span><span class="sxs-lookup"><span data-stu-id="822ce-152">You can use other email providers.</span></span> <span data-ttu-id="822ce-153">ASP.NET Core 2.x включает `System.Net.Mail`, который позволяет отправлять электронную почту из приложения.</span><span class="sxs-lookup"><span data-stu-id="822ce-153">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="822ce-154">Мы рекомендуем использовать SendGrid или другая служба электронной почты для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="822ce-154">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="822ce-155">Для защиты и правильно настроить сложно SMTP.</span><span class="sxs-lookup"><span data-stu-id="822ce-155">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="822ce-156">Создание класса для извлечения ключа защиты электронной почты.</span><span class="sxs-lookup"><span data-stu-id="822ce-156">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="822ce-157">Для этого примера создайте *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="822ce-157">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="822ce-158">Настройка SendGrid секреты пользователя</span><span class="sxs-lookup"><span data-stu-id="822ce-158">Configure SendGrid user secrets</span></span>

<span data-ttu-id="822ce-159">Добавьте уникальный `<UserSecretsId>` значение `<PropertyGroup>` элемент файла проекта:</span><span class="sxs-lookup"><span data-stu-id="822ce-159">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="822ce-160">Задайте `SendGridUser` и `SendGridKey` с [средство secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="822ce-160">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="822ce-161">Пример:</span><span class="sxs-lookup"><span data-stu-id="822ce-161">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="822ce-162">На Windows, Secret Manager хранит пары ключей и значений в *secrets.json* файл `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` каталог.</span><span class="sxs-lookup"><span data-stu-id="822ce-162">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="822ce-163">Содержание *secrets.json* файл не зашифрован.</span><span class="sxs-lookup"><span data-stu-id="822ce-163">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="822ce-164">*Secrets.json* ниже приведен файл ( `SendGridKey` значение удаляется.)</span><span class="sxs-lookup"><span data-stu-id="822ce-164">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```
 
<span data-ttu-id="822ce-165">Дополнительные сведения см. в разделе [шаблон параметров](xref:fundamentals/configuration/options) и [конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="822ce-165">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="822ce-166">Установка SendGrid</span><span class="sxs-lookup"><span data-stu-id="822ce-166">Install SendGrid</span></span>

<span data-ttu-id="822ce-167">Этот учебник демонстрирует добавление уведомлений по электронной почте через [SendGrid](https://sendgrid.com/), но вы можете отправлять электронную почту с помощью SMTP и другие механизмы.</span><span class="sxs-lookup"><span data-stu-id="822ce-167">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="822ce-168">Установить `SendGrid` пакет NuGet:</span><span class="sxs-lookup"><span data-stu-id="822ce-168">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="822ce-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="822ce-169">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="822ce-170">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="822ce-170">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="822ce-171">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="822ce-171">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="822ce-172">В консоли введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="822ce-172">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="822ce-173">См. в разделе [приступить к работе с SendGrid бесплатно](https://sendgrid.com/free/) зарегистрироваться для получения бесплатной учетной записи SendGrid.</span><span class="sxs-lookup"><span data-stu-id="822ce-173">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="822ce-174">Реализовать IEmailSender</span><span class="sxs-lookup"><span data-stu-id="822ce-174">Implement IEmailSender</span></span>

<span data-ttu-id="822ce-175">Для реализации `IEmailSender`, создание *Services/EmailSender.cs* с кодом, аналогичную следующей:</span><span class="sxs-lookup"><span data-stu-id="822ce-175">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="822ce-176">Настройка запуска для поддержки по электронной почте</span><span class="sxs-lookup"><span data-stu-id="822ce-176">Configure startup to support email</span></span>

<span data-ttu-id="822ce-177">Добавьте следующий код, чтобы `ConfigureServices` метод в *Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="822ce-177">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="822ce-178">Добавление `EmailSender` как отдельная служба.</span><span class="sxs-lookup"><span data-stu-id="822ce-178">Add `EmailSender` as a singleton service.</span></span>
* <span data-ttu-id="822ce-179">Зарегистрировать `AuthMessageSenderOptions` конфигурации экземпляра.</span><span class="sxs-lookup"><span data-stu-id="822ce-179">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="822ce-180">Включить учетную запись, пароль и Подтверждение восстановления</span><span class="sxs-lookup"><span data-stu-id="822ce-180">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="822ce-181">Шаблон содержит код для восстановления и подтверждение пароля учетной записи.</span><span class="sxs-lookup"><span data-stu-id="822ce-181">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="822ce-182">Найти `OnPostAsync` метод в *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="822ce-182">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="822ce-183">Запретить пользователям только что зарегистрированное автоматически входит в систему, закомментируйте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="822ce-183">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="822ce-184">С помощью измененные строки выделены показан полный метод:</span><span class="sxs-lookup"><span data-stu-id="822ce-184">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="822ce-185">Регистрация, подтверждение электронной почты и сброс пароля</span><span class="sxs-lookup"><span data-stu-id="822ce-185">Register, confirm email, and reset password</span></span>

<span data-ttu-id="822ce-186">Запустите веб-приложение и протестируйте подтверждение учетной записи и процесс восстановления пароля.</span><span class="sxs-lookup"><span data-stu-id="822ce-186">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="822ce-187">Запустите приложение и зарегистрируйте нового пользователя</span><span class="sxs-lookup"><span data-stu-id="822ce-187">Run the app and register a new user</span></span>

  ![Веб-приложения, зарегистрируйте учетную запись-представление](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="822ce-189">Проверьте свой адрес электронной почты для ссылку для подтверждения учетной записи.</span><span class="sxs-lookup"><span data-stu-id="822ce-189">Check your email for the account confirmation link.</span></span> <span data-ttu-id="822ce-190">См. в разделе [отладки электронной почты](#debug) Если вы не получили сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="822ce-190">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="822ce-191">Щелкните ссылку, чтобы подтвердить свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="822ce-191">Click the link to confirm your email.</span></span>
* <span data-ttu-id="822ce-192">Войдите с помощью электронной почты и пароль.</span><span class="sxs-lookup"><span data-stu-id="822ce-192">Log in with your email and password.</span></span>
* <span data-ttu-id="822ce-193">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="822ce-193">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="822ce-194">Просмотреть страницу управление</span><span class="sxs-lookup"><span data-stu-id="822ce-194">View the manage page</span></span>

<span data-ttu-id="822ce-195">Выберите свое имя пользователя в браузере: ![окно браузера с именем пользователя](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="822ce-195">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="822ce-196">Может потребоваться развернуть панель навигации, чтобы увидеть имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="822ce-196">You might need to expand the navbar to see user name.</span></span>

![панель переходов](accconfirm/_static/x.png)

<span data-ttu-id="822ce-198">Управление страница отображается с **профиль** выделенной вкладки.</span><span class="sxs-lookup"><span data-stu-id="822ce-198">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="822ce-199">**Электронной почты** показано типа "флажок", указывающее, адрес электронной почты будет подтверждена.</span><span class="sxs-lookup"><span data-stu-id="822ce-199">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="822ce-200">Сброс пароля теста</span><span class="sxs-lookup"><span data-stu-id="822ce-200">Test password reset</span></span>

* <span data-ttu-id="822ce-201">Если вы выполнили вход в, выберите **выхода**.</span><span class="sxs-lookup"><span data-stu-id="822ce-201">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="822ce-202">Выберите **вход** ссылку и выберите **забыли пароль?** ссылку.</span><span class="sxs-lookup"><span data-stu-id="822ce-202">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="822ce-203">Введите адрес электронной почты, которая использовалась для регистрации учетной записи.</span><span class="sxs-lookup"><span data-stu-id="822ce-203">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="822ce-204">Отправляется сообщение электронной почты со ссылкой для сброса пароля.</span><span class="sxs-lookup"><span data-stu-id="822ce-204">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="822ce-205">Проверьте свой адрес электронной почты и щелкните ссылку, чтобы сбросить пароль.</span><span class="sxs-lookup"><span data-stu-id="822ce-205">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="822ce-206">После успешного сброса пароля вы можете войти с использованием электронной почты и новый пароль.</span><span class="sxs-lookup"><span data-stu-id="822ce-206">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="822ce-207">Отладка по электронной почте</span><span class="sxs-lookup"><span data-stu-id="822ce-207">Debug email</span></span>

<span data-ttu-id="822ce-208">Если не удается получить рабочий адрес электронной почты:</span><span class="sxs-lookup"><span data-stu-id="822ce-208">If you can't get email working:</span></span>

* <span data-ttu-id="822ce-209">Установите точку останова в `EmailSender.Execute` для проверки `SendGridClient.SendEmailAsync` вызывается.</span><span class="sxs-lookup"><span data-stu-id="822ce-209">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="822ce-210">Создание [консольное приложение для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) аналогичный код, используя `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="822ce-210">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="822ce-211">Просмотрите [действия "сообщения"](https://sendgrid.com/docs/User_Guide/email_activity.html) страницы.</span><span class="sxs-lookup"><span data-stu-id="822ce-211">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="822ce-212">Проверьте папку нежелательной почты.</span><span class="sxs-lookup"><span data-stu-id="822ce-212">Check your spam folder.</span></span>
* <span data-ttu-id="822ce-213">Попробуйте другой псевдоним электронной почты на другой поставщик электронной почты (Microsoft, Yahoo, Gmail, и т.д.)</span><span class="sxs-lookup"><span data-stu-id="822ce-213">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="822ce-214">Попробуйте отправить учетными записями электронной почты.</span><span class="sxs-lookup"><span data-stu-id="822ce-214">Try sending to different email accounts.</span></span>

<span data-ttu-id="822ce-215">**Рекомендации по безопасности** — **не** используйте секреты производства в разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="822ce-215">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="822ce-216">При публикации приложения в Azure, секреты SendGrid можно задать как параметры приложения на портале веб-приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="822ce-216">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="822ce-217">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="822ce-217">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="822ce-218">Объединить учетные записи социальных сетей и локального имени входа</span><span class="sxs-lookup"><span data-stu-id="822ce-218">Combine social and local login accounts</span></span>

<span data-ttu-id="822ce-219">Для завершения этого раздела необходимо сначала включить внешний поставщик аутентификации.</span><span class="sxs-lookup"><span data-stu-id="822ce-219">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="822ce-220">См. в разделе [Facebook, Google и внешнего поставщика проверки подлинности](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="822ce-220">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="822ce-221">Можно объединить учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="822ce-221">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="822ce-222">В следующей последовательности «RickAndMSFT@gmail.com"сначала создается как локальное имя входа; тем не менее, можно сначала создать учетную запись как имя для входа социальных сетей, а затем добавить локальное имя входа.</span><span class="sxs-lookup"><span data-stu-id="822ce-222">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Веб-приложения: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

<span data-ttu-id="822ce-224">Щелкните **управление** ссылку.</span><span class="sxs-lookup"><span data-stu-id="822ce-224">Click on the **Manage** link.</span></span> <span data-ttu-id="822ce-225">Примечание внешний 0 (имена входа социальных сетей), связанное с этой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="822ce-225">Note the 0 external (social logins) associated with this account.</span></span>

![Управления представлением](accconfirm/_static/manage.png)

<span data-ttu-id="822ce-227">Щелкните ссылку на другую службу входа в систему и принимать запросы приложений.</span><span class="sxs-lookup"><span data-stu-id="822ce-227">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="822ce-228">На следующем рисунке Facebook, — это внешнего поставщика проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="822ce-228">In the following image, Facebook is the external authentication provider:</span></span>

![Управление ваш список внешних имен входа Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="822ce-230">Две учетные записи были объединены.</span><span class="sxs-lookup"><span data-stu-id="822ce-230">The two accounts have been combined.</span></span> <span data-ttu-id="822ce-231">Вы сможете выполнить вход с использованием либо учетной записи.</span><span class="sxs-lookup"><span data-stu-id="822ce-231">You are able to log on with either account.</span></span> <span data-ttu-id="822ce-232">Вы можете добавить локальные учетные записи, в случае их учетные записи социальных сетей службы проверки подлинности не работает, или более вероятно, они потеряли доступ к своей учетной записи социальной сети пользователей.</span><span class="sxs-lookup"><span data-stu-id="822ce-232">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="822ce-233">Включить подтверждение учетной записи, после узла имеет пользователей</span><span class="sxs-lookup"><span data-stu-id="822ce-233">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="822ce-234">Включение подтверждение учетной записи на сайте с пользователями блокирует работу всех существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="822ce-234">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="822ce-235">Существующие пользователи заблокированы, так как учетные записи не подтверждены.</span><span class="sxs-lookup"><span data-stu-id="822ce-235">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="822ce-236">Чтобы обойти существующие блокировки пользователя, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="822ce-236">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="822ce-237">Обновите базу данных для пометки всех существующих пользователей в качестве подтверждения.</span><span class="sxs-lookup"><span data-stu-id="822ce-237">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="822ce-238">Подтвердите существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="822ce-238">Confirm exiting users.</span></span> <span data-ttu-id="822ce-239">Например пакетная Отправка сообщения электронной почты с подтверждением ссылки.</span><span class="sxs-lookup"><span data-stu-id="822ce-239">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
