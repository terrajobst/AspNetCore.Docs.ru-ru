---
title: Подтверждение учетной записи и восстановление пароля в ASP.NET Core
author: rick-anderson
description: Сведения о создании приложения ASP.NET Core с помощью по электронной почте подтверждение и сброс пароля.
ms.author: riande
ms.date: 3/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 59041bcf11f7deb351a2f0bb075ed80c8af5e12b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891681"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="4d5b6-103">Подтверждение учетной записи и восстановление пароля в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d5b6-103">Account confirmation and password recovery in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="4d5b6-104">См. в разделе [этот PDF-файл](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) для ASP.NET Core 1.1 и версия 2.1.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-104">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4d5b6-105">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), и [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="4d5b6-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="4d5b6-106">Этом руководстве описывается создание приложения ASP.NET Core с помощью по электронной почте подтверждение и сброс пароля.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="4d5b6-107">Это руководство представляет собой **не** начало раздела.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="4d5b6-108">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-108">You should be familiar with:</span></span>

* [<span data-ttu-id="4d5b6-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d5b6-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="4d5b6-110">Authentication</span><span class="sxs-lookup"><span data-stu-id="4d5b6-110">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="4d5b6-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4d5b6-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="4d5b6-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="4d5b6-112">Prerequisites</span></span>

[<span data-ttu-id="4d5b6-113">Пакет SDK для .NET core 2.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="4d5b6-113">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="4d5b6-114">Создание веб-приложения и сформировать шаблон удостоверений</span><span class="sxs-lookup"><span data-stu-id="4d5b6-114">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="4d5b6-115">Выполните следующие команды для создания веб-приложения с помощью проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-115">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="4d5b6-116">Регистрация нового пользователя для тестирования</span><span class="sxs-lookup"><span data-stu-id="4d5b6-116">Test new user registration</span></span>

<span data-ttu-id="4d5b6-117">Запустите приложение, выберите **зарегистрировать** связать и регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-117">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="4d5b6-118">На этом этапе является только проверка на адрес электронной почты с [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) атрибута.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-118">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="4d5b6-119">После отправки регистрации, вы вошли в приложение.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-119">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="4d5b6-120">Далее в этом руководстве код обновляется, поэтому новые пользователи не могут войти до окончания проверки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-120">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="4d5b6-121">Обратите внимание, таблицы `EmailConfirmed` поле является `False`.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-121">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="4d5b6-122">Вы можете использовать этот адрес электронной почты снова на следующем шаге, когда приложение отправляет сообщение электронной почты с подтверждением.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-122">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="4d5b6-123">Щелкните правой кнопкой мыши в строке и выберите **удалить**.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-123">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="4d5b6-124">Удаление псевдонима электронной почты упрощает в следующих шагах.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-124">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="4d5b6-125">Требуется подтверждение по электронной почте</span><span class="sxs-lookup"><span data-stu-id="4d5b6-125">Require email confirmation</span></span>

<span data-ttu-id="4d5b6-126">Рекомендуется подтвердить адрес электронной почты новой регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-126">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="4d5b6-127">Отправить по электронной почте подтверждение помогает проверить, они не олицетворения кто-то другой (то есть они еще не зарегистрировано другого пользователя по электронной почте).</span><span class="sxs-lookup"><span data-stu-id="4d5b6-127">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="4d5b6-128">Предположим, что имеется дискуссионный форум и избежать "yli@example.com«из регистрации в качестве»nolivetto@contoso.com«.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-128">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="4d5b6-129">Без подтверждение по электронной почте "nolivetto@contoso.com" могли получать нежелательные сообщения электронной почты из вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-129">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="4d5b6-130">Предположим, что пользователь случайно зарегистрирован как "ylo@example.com" и не заметили опечатку «yli».</span><span class="sxs-lookup"><span data-stu-id="4d5b6-130">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="4d5b6-131">Они не смогут использовать пароль восстановления, так как у приложения нет их правильный адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-131">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="4d5b6-132">Подтверждение по электронной почте предоставляет ограниченную защиту от программ-роботов.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-132">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="4d5b6-133">Подтверждение по электронной почте не обеспечивают защиту от пользователей-злоумышленников с многих учетных записей электронной почты.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-133">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="4d5b6-134">Обычно требуется запретить новым пользователям из учета данных для веб-сайт, прежде чем они получат подтвержден по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-134">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="4d5b6-135">Обновление `Startup.ConfigureServices` требуется подтвержден по электронной почте:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-135">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="4d5b6-136">`config.SignIn.RequireConfirmedEmail = true;` запрещает вход до подтверждения их по электронной почте зарегистрированным пользователям.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-136">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="4d5b6-137">Настройка поставщика услуг электронной почты</span><span class="sxs-lookup"><span data-stu-id="4d5b6-137">Configure email provider</span></span>

<span data-ttu-id="4d5b6-138">В этом руководстве [SendGrid](https://sendgrid.com) используется для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-138">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="4d5b6-139">Требуется учетная запись SendGrid и ключ для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-139">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="4d5b6-140">Можно использовать другие поставщики электронной почты.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-140">You can use other email providers.</span></span> <span data-ttu-id="4d5b6-141">ASP.NET Core 2.x включает `System.Net.Mail`, который позволяет отправлять электронную почту из приложения.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-141">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="4d5b6-142">Мы рекомендуем использовать SendGrid или другая служба электронной почты для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-142">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="4d5b6-143">Для защиты и правильно настроить сложно SMTP.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-143">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="4d5b6-144">Создание класса для извлечения ключа защиты электронной почты.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-144">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="4d5b6-145">Для этого примера создайте *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-145">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="4d5b6-146">Настройка SendGrid секреты пользователя</span><span class="sxs-lookup"><span data-stu-id="4d5b6-146">Configure SendGrid user secrets</span></span>

<span data-ttu-id="4d5b6-147">Задайте `SendGridUser` и `SendGridKey` с [средство secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="4d5b6-147">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="4d5b6-148">Пример:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-148">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="4d5b6-149">На Windows, Secret Manager хранит пары ключей и значений в *secrets.json* файл `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` каталог.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-149">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="4d5b6-150">Содержание *secrets.json* файл не зашифрован.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-150">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="4d5b6-151">Следующая разметка показывает *secrets.json* файла.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-151">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="4d5b6-152">`SendGridKey` Значение удаляется.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-152">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="4d5b6-153">Дополнительные сведения см. в разделе [шаблон параметров](xref:fundamentals/configuration/options) и [конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="4d5b6-153">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="4d5b6-154">Установка SendGrid</span><span class="sxs-lookup"><span data-stu-id="4d5b6-154">Install SendGrid</span></span>

<span data-ttu-id="4d5b6-155">Этот учебник демонстрирует добавление уведомлений по электронной почте через [SendGrid](https://sendgrid.com/), но вы можете отправлять электронную почту с помощью SMTP и другие механизмы.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-155">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="4d5b6-156">Установить `SendGrid` пакет NuGet:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-156">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4d5b6-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4d5b6-157">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4d5b6-158">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-158">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4d5b6-159">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="4d5b6-159">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4d5b6-160">В консоли введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-160">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="4d5b6-161">См. в разделе [приступить к работе с SendGrid бесплатно](https://sendgrid.com/free/) зарегистрироваться для получения бесплатной учетной записи SendGrid.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-161">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="4d5b6-162">Реализовать IEmailSender</span><span class="sxs-lookup"><span data-stu-id="4d5b6-162">Implement IEmailSender</span></span>

<span data-ttu-id="4d5b6-163">Для реализации `IEmailSender`, создание *Services/EmailSender.cs* с кодом, аналогичную следующей:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-163">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="4d5b6-164">Настройка запуска для поддержки по электронной почте</span><span class="sxs-lookup"><span data-stu-id="4d5b6-164">Configure startup to support email</span></span>

<span data-ttu-id="4d5b6-165">Добавьте следующий код, чтобы `ConfigureServices` метод в *Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-165">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="4d5b6-166">Добавление `EmailSender` как временной ошибкой службы.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-166">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="4d5b6-167">Зарегистрировать `AuthMessageSenderOptions` конфигурации экземпляра.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-167">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="4d5b6-168">Включить учетную запись, пароль и Подтверждение восстановления</span><span class="sxs-lookup"><span data-stu-id="4d5b6-168">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="4d5b6-169">Шаблон содержит код для восстановления и подтверждение пароля учетной записи.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-169">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="4d5b6-170">Найти `OnPostAsync` метод в *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-170">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="4d5b6-171">Запретить новым зарегистрированным пользователям, чтобы автоматически входить, закомментируйте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-171">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="4d5b6-172">С помощью измененные строки выделены показан полный метод:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-172">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="4d5b6-173">Регистрация, подтверждение электронной почты и сброс пароля</span><span class="sxs-lookup"><span data-stu-id="4d5b6-173">Register, confirm email, and reset password</span></span>

<span data-ttu-id="4d5b6-174">Запустите веб-приложение и протестируйте подтверждение учетной записи и процесс восстановления пароля.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-174">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="4d5b6-175">Запустите приложение и зарегистрируйте нового пользователя</span><span class="sxs-lookup"><span data-stu-id="4d5b6-175">Run the app and register a new user</span></span>
* <span data-ttu-id="4d5b6-176">Проверьте свой адрес электронной почты для ссылку для подтверждения учетной записи.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-176">Check your email for the account confirmation link.</span></span> <span data-ttu-id="4d5b6-177">См. в разделе [отладки электронной почты](#debug) Если вы не получили сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-177">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="4d5b6-178">Щелкните ссылку, чтобы подтвердить свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-178">Click the link to confirm your email.</span></span>
* <span data-ttu-id="4d5b6-179">Войдите, используя свой адрес электронной почты и пароль.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-179">Sign in with your email and password.</span></span>
* <span data-ttu-id="4d5b6-180">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-180">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="4d5b6-181">Просмотреть страницу управление</span><span class="sxs-lookup"><span data-stu-id="4d5b6-181">View the manage page</span></span>

<span data-ttu-id="4d5b6-182">Выберите свое имя пользователя в браузере: ![окно браузера с именем пользователя](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="4d5b6-182">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="4d5b6-183">Управление страница отображается с **профиль** выделенной вкладки.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-183">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="4d5b6-184">**Электронной почты** показано типа "флажок", указывающее, адрес электронной почты будет подтверждена.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-184">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="4d5b6-185">Сброс пароля теста</span><span class="sxs-lookup"><span data-stu-id="4d5b6-185">Test password reset</span></span>

* <span data-ttu-id="4d5b6-186">Если вы уже вошли в, выберите **выхода**.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-186">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="4d5b6-187">Выберите **вход** ссылку и выберите **забыли пароль?** ссылку.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-187">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="4d5b6-188">Введите адрес электронной почты, которая использовалась для регистрации учетной записи.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-188">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="4d5b6-189">Отправляется сообщение электронной почты со ссылкой для сброса пароля.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-189">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="4d5b6-190">Проверьте свой адрес электронной почты и щелкните ссылку, чтобы сбросить пароль.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-190">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="4d5b6-191">После успешного сброса пароля вы можете войти с использованием электронной почты и новый пароль.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-191">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="4d5b6-192">Изменить время ожидания сообщения электронной почты и действия</span><span class="sxs-lookup"><span data-stu-id="4d5b6-192">Change email and activity timeout</span></span>

<span data-ttu-id="4d5b6-193">Время ожидания бездействия по умолчанию — 14 дней.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-193">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="4d5b6-194">Следующий код задает время ожидания в бездействии до 5 дней:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-194">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="4d5b6-195">Изменить все самозаверяющиеся маркеров защиты данных</span><span class="sxs-lookup"><span data-stu-id="4d5b6-195">Change all data protection token lifespans</span></span>

<span data-ttu-id="4d5b6-196">Следующий код изменяет все ожидания маркеры защиты данных на 3 часа:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-196">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="4d5b6-197">Встроенные в маркерах удостоверения пользователя (см. в разделе [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) имеют [одного дня время ожидания](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="4d5b6-197">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="4d5b6-198">Изменить время существования токена электронной почты</span><span class="sxs-lookup"><span data-stu-id="4d5b6-198">Change the email token lifespan</span></span>

<span data-ttu-id="4d5b6-199">По умолчанию время существования токена доступа из [маркеров идентификации пользователя](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) — [один день](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="4d5b6-199">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="4d5b6-200">В этом разделе показано, как изменить время существования токена электронной почты.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-200">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="4d5b6-201">Добавить пользовательское [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) и <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-201">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="4d5b6-202">Добавьте пользовательский поставщик в контейнер службы:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-202">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="4d5b6-203">Повторно отправить подтверждение по электронной почте</span><span class="sxs-lookup"><span data-stu-id="4d5b6-203">Resend email confirmation</span></span>

<span data-ttu-id="4d5b6-204">См. в разделе [проблема GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="4d5b6-204">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="4d5b6-205">Отладка по электронной почте</span><span class="sxs-lookup"><span data-stu-id="4d5b6-205">Debug email</span></span>

<span data-ttu-id="4d5b6-206">Если не удается получить рабочий адрес электронной почты:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-206">If you can't get email working:</span></span>

* <span data-ttu-id="4d5b6-207">Установите точку останова в `EmailSender.Execute` для проверки `SendGridClient.SendEmailAsync` вызывается.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-207">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="4d5b6-208">Создание [консольное приложение для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) аналогичный код, используя `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-208">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="4d5b6-209">Просмотрите [действия "сообщения"](https://sendgrid.com/docs/User_Guide/email_activity.html) страницы.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-209">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="4d5b6-210">Проверьте папку нежелательной почты.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-210">Check your spam folder.</span></span>
* <span data-ttu-id="4d5b6-211">Попробуйте другой псевдоним электронной почты на другой поставщик электронной почты (Microsoft, Yahoo, Gmail, и т.д.)</span><span class="sxs-lookup"><span data-stu-id="4d5b6-211">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="4d5b6-212">Попробуйте отправить учетными записями электронной почты.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-212">Try sending to different email accounts.</span></span>

<span data-ttu-id="4d5b6-213">**Рекомендации по безопасности** — **не** используйте секреты производства в разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-213">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="4d5b6-214">При публикации приложения в Azure, секреты SendGrid можно задать как параметры приложения на портале веб-приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-214">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="4d5b6-215">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-215">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="4d5b6-216">Объединить учетные записи социальных сетей и локального имени входа</span><span class="sxs-lookup"><span data-stu-id="4d5b6-216">Combine social and local login accounts</span></span>

<span data-ttu-id="4d5b6-217">Для завершения этого раздела необходимо сначала включить внешний поставщик аутентификации.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-217">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="4d5b6-218">См. в разделе [Facebook, Google и внешнего поставщика проверки подлинности](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="4d5b6-218">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="4d5b6-219">Можно объединить учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-219">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="4d5b6-220">В следующей последовательности «RickAndMSFT@gmail.com"сначала создается как локальное имя входа; тем не менее, можно сначала создать учетную запись как имя для входа социальных сетей, а затем добавить локальное имя входа.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-220">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Веб-приложения: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

<span data-ttu-id="4d5b6-222">Щелкните **управление** ссылку.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-222">Click on the **Manage** link.</span></span> <span data-ttu-id="4d5b6-223">Примечание внешний 0 (имена входа социальных сетей), связанное с этой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-223">Note the 0 external (social logins) associated with this account.</span></span>

![Управления представлением](accconfirm/_static/manage.png)

<span data-ttu-id="4d5b6-225">Щелкните ссылку на другую службу входа в систему и принимать запросы приложений.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-225">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="4d5b6-226">На следующем рисунке Facebook, — это внешнего поставщика проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-226">In the following image, Facebook is the external authentication provider:</span></span>

![Управление ваш список внешних имен входа Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="4d5b6-228">Две учетные записи были объединены.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-228">The two accounts have been combined.</span></span> <span data-ttu-id="4d5b6-229">Вы сможете выполнить вход с использованием либо учетной записи.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-229">You are able to sign in with either account.</span></span> <span data-ttu-id="4d5b6-230">Вы можете добавить локальные учетные записи, в случае их учетные записи социальных сетей службы проверки подлинности не работает, или более вероятно, они потеряли доступ к своей учетной записи социальной сети пользователей.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-230">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="4d5b6-231">Включить подтверждение учетной записи, после узла имеет пользователей</span><span class="sxs-lookup"><span data-stu-id="4d5b6-231">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="4d5b6-232">Включение подтверждение учетной записи на сайте с пользователями блокирует работу всех существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-232">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="4d5b6-233">Существующие пользователи заблокированы, так как учетные записи не подтверждены.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-233">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="4d5b6-234">Чтобы обойти существующие блокировки пользователя, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="4d5b6-234">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="4d5b6-235">Обновите базу данных для пометки всех существующих пользователей в качестве подтверждения.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-235">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="4d5b6-236">Подтвердите существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-236">Confirm existing users.</span></span> <span data-ttu-id="4d5b6-237">Например пакетная Отправка сообщения электронной почты с подтверждением ссылки.</span><span class="sxs-lookup"><span data-stu-id="4d5b6-237">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
