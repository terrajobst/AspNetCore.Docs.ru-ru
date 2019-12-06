---
title: Подтверждение учетной записи и восстановление пароля в ASP.NET Core
author: rick-anderson
description: Узнайте, как создать приложение ASP.NET Core с подтверждением электронной почты и сбросом пароля.
ms.author: riande
ms.date: 03/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: a4ecc2d91fb72915703dfaa146260f0c1360bded
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880771"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="41215-103">Подтверждение учетной записи и восстановление пароля в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41215-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="41215-104">[Рик Андерсон (](https://twitter.com/RickAndMSFT), [Понант](https://github.com/Ponant)и [Джо аудетте](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="41215-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="41215-105">В этом руководстве показано, как создать приложение ASP.NET Core с подтверждением электронной почты и сбросом пароля.</span><span class="sxs-lookup"><span data-stu-id="41215-105">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="41215-106">Это руководство **не** является началом статьи.</span><span class="sxs-lookup"><span data-stu-id="41215-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="41215-107">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="41215-107">You should be familiar with:</span></span>

* [<span data-ttu-id="41215-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41215-108">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="41215-109">Authentication</span><span class="sxs-lookup"><span data-stu-id="41215-109">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="41215-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="41215-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="41215-111">См. [этот PDF-файл](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) для ASP.NET Core версии 1,1.</span><span class="sxs-lookup"><span data-stu-id="41215-111">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 version.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="41215-112">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="41215-112">Prerequisites</span></span>

[<span data-ttu-id="41215-113">Пакет SDK для .NET Core 3.0 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="41215-113">.NET Core 3.0 SDK or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a><span data-ttu-id="41215-114">Создание и тестирование веб-приложения с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="41215-114">Create and test a web app with authentication</span></span>

<span data-ttu-id="41215-115">Выполните следующие команды, чтобы создать веб-приложение с проверкой подлинности.</span><span class="sxs-lookup"><span data-stu-id="41215-115">Run the following commands to create a web app with authentication.</span></span>

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

<span data-ttu-id="41215-116">Запустите приложение, выберите ссылку **Register** и зарегистрируйте пользователя.</span><span class="sxs-lookup"><span data-stu-id="41215-116">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="41215-117">После регистрации вы будете перенаправлены на страницу `/Identity/Account/RegisterConfirmation`, содержащую ссылку для имитации подтверждения по электронной почте:</span><span class="sxs-lookup"><span data-stu-id="41215-117">Once registered, you are redirected to the to `/Identity/Account/RegisterConfirmation` page which contains a link to simulate email confirmation:</span></span>

* <span data-ttu-id="41215-118">Выберите ссылку `Click here to confirm your account`.</span><span class="sxs-lookup"><span data-stu-id="41215-118">Select the `Click here to confirm your account` link.</span></span>
* <span data-ttu-id="41215-119">Выберите ссылку для **входа** и выполните вход с теми же учетными данными.</span><span class="sxs-lookup"><span data-stu-id="41215-119">Select the **Login** link and sign-in with the same credentials.</span></span>
* <span data-ttu-id="41215-120">Выберите ссылку `Hello YourEmail@provider.com!`, которая перенаправит вас на страницу `/Identity/Account/Manage/PersonalData`.</span><span class="sxs-lookup"><span data-stu-id="41215-120">Select the `Hello YourEmail@provider.com!` link, which redirects you to the `/Identity/Account/Manage/PersonalData` page.</span></span>
* <span data-ttu-id="41215-121">Выберите вкладку **личные данные** слева, а затем щелкните **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="41215-121">Select the **Personal data** tab on the left, and then select **Delete**.</span></span>

### <a name="configure-an-email-provider"></a><span data-ttu-id="41215-122">Настройка поставщика электронной почты</span><span class="sxs-lookup"><span data-stu-id="41215-122">Configure an email provider</span></span>

<span data-ttu-id="41215-123">В этом руководстве [SendGrid](https://sendgrid.com) используется для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-123">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="41215-124">Для отправки электронной почты требуется учетная запись SendGrid и ключ.</span><span class="sxs-lookup"><span data-stu-id="41215-124">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="41215-125">Вы можете использовать другие поставщики электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-125">You can use other email providers.</span></span> <span data-ttu-id="41215-126">Для отправки электронной почты рекомендуется использовать SendGrid или другую почтовую службу.</span><span class="sxs-lookup"><span data-stu-id="41215-126">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="41215-127">Протокол SMTP трудно защитить и настроить правильно.</span><span class="sxs-lookup"><span data-stu-id="41215-127">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="41215-128">Создайте класс для выборки ключа защищенной электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-128">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="41215-129">Для этого примера создайте *Services/аусмессажесендероптионс. CS*:</span><span class="sxs-lookup"><span data-stu-id="41215-129">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="41215-130">Настройка секретов пользователя SendGrid</span><span class="sxs-lookup"><span data-stu-id="41215-130">Configure SendGrid user secrets</span></span>

<span data-ttu-id="41215-131">Задайте `SendGridUser` и `SendGridKey` с помощью [средства Secret-Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="41215-131">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="41215-132">Например:</span><span class="sxs-lookup"><span data-stu-id="41215-132">For example:</span></span>

```dotnetcli
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="41215-133">В Windows Диспетчер секретов сохраняет пары "ключ-значение" в файле *секреты. JSON* в каталоге `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.</span><span class="sxs-lookup"><span data-stu-id="41215-133">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="41215-134">Содержимое файла *секреты. JSON* не шифруется.</span><span class="sxs-lookup"><span data-stu-id="41215-134">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="41215-135">В следующей разметке показан файл *секреты. JSON* .</span><span class="sxs-lookup"><span data-stu-id="41215-135">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="41215-136">Значение `SendGridKey` было удалено.</span><span class="sxs-lookup"><span data-stu-id="41215-136">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="41215-137">Дополнительные сведения см. в разделе [шаблон](xref:fundamentals/configuration/options) и [Конфигурация](xref:fundamentals/configuration/index)параметров.</span><span class="sxs-lookup"><span data-stu-id="41215-137">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="41215-138">Установка SendGrid</span><span class="sxs-lookup"><span data-stu-id="41215-138">Install SendGrid</span></span>

<span data-ttu-id="41215-139">В этом руководстве показано, как добавлять уведомления по электронной почте через [SendGrid](https://sendgrid.com/), но можно отправлять сообщения электронной почты с помощью SMTP и других механизмов.</span><span class="sxs-lookup"><span data-stu-id="41215-139">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="41215-140">Установите `SendGrid` пакет NuGet:</span><span class="sxs-lookup"><span data-stu-id="41215-140">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="41215-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41215-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="41215-142">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="41215-142">From the Package Manager Console, enter the following command:</span></span>

```powershell
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="41215-143">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="41215-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="41215-144">В консоли введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="41215-144">From the console, enter the following command:</span></span>

```dotnetcli
dotnet add package SendGrid
```

---

<span data-ttu-id="41215-145">Ознакомьтесь [со статьей начало работы с SendGrid](https://sendgrid.com/free/) , чтобы зарегистрироваться для получения бесплатной учетной записи SendGrid.</span><span class="sxs-lookup"><span data-stu-id="41215-145">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="41215-146">Реализация Иемаилсендер</span><span class="sxs-lookup"><span data-stu-id="41215-146">Implement IEmailSender</span></span>

<span data-ttu-id="41215-147">Чтобы реализовать `IEmailSender`, создайте *службы/емаилсендер. CS* с кодом, аналогичным следующему:</span><span class="sxs-lookup"><span data-stu-id="41215-147">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="41215-148">Настройка запуска для поддержки электронной почты</span><span class="sxs-lookup"><span data-stu-id="41215-148">Configure startup to support email</span></span>

<span data-ttu-id="41215-149">Добавьте следующий код в метод `ConfigureServices` в файле *Startup.CS* :</span><span class="sxs-lookup"><span data-stu-id="41215-149">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="41215-150">Добавьте `EmailSender` в качестве временной службы.</span><span class="sxs-lookup"><span data-stu-id="41215-150">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="41215-151">Зарегистрируйте экземпляр конфигурации `AuthMessageSenderOptions`.</span><span class="sxs-lookup"><span data-stu-id="41215-151">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="41215-152">Регистрация, подтверждение электронной почты и сброс пароля</span><span class="sxs-lookup"><span data-stu-id="41215-152">Register, confirm email, and reset password</span></span>

<span data-ttu-id="41215-153">Запустите веб-приложение и протестируйте поток подтверждения учетной записи и восстановления пароля.</span><span class="sxs-lookup"><span data-stu-id="41215-153">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="41215-154">Запустите приложение и зарегистрируйте нового пользователя</span><span class="sxs-lookup"><span data-stu-id="41215-154">Run the app and register a new user</span></span>
* <span data-ttu-id="41215-155">Проверьте свою электронную почту на наличие ссылки для подтверждения учетной записи.</span><span class="sxs-lookup"><span data-stu-id="41215-155">Check your email for the account confirmation link.</span></span> <span data-ttu-id="41215-156">Если вы не получаете электронное письмо, см. раздел [Отладка электронной почты](#debug) .</span><span class="sxs-lookup"><span data-stu-id="41215-156">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="41215-157">Щелкните ссылку, чтобы подтвердить свою электронную почту.</span><span class="sxs-lookup"><span data-stu-id="41215-157">Click the link to confirm your email.</span></span>
* <span data-ttu-id="41215-158">Выполните вход, используя свой адрес электронной почты и пароль.</span><span class="sxs-lookup"><span data-stu-id="41215-158">Sign in with your email and password.</span></span>
* <span data-ttu-id="41215-159">Выполните выход.</span><span class="sxs-lookup"><span data-stu-id="41215-159">Sign out.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="41215-160">Проверка сброса пароля</span><span class="sxs-lookup"><span data-stu-id="41215-160">Test password reset</span></span>

* <span data-ttu-id="41215-161">Если вы вошли в этот компьютер, выберите **выход**.</span><span class="sxs-lookup"><span data-stu-id="41215-161">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="41215-162">Щелкните ссылку **войти** и выберите ссылку **забыли пароль?** .</span><span class="sxs-lookup"><span data-stu-id="41215-162">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="41215-163">Введите адрес электронной почты, использованный для регистрации учетной записи.</span><span class="sxs-lookup"><span data-stu-id="41215-163">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="41215-164">Отправляется сообщение электронной почты со ссылкой для сброса пароля.</span><span class="sxs-lookup"><span data-stu-id="41215-164">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="41215-165">Проверьте свой адрес электронной почты и щелкните ссылку, чтобы сбросить пароль.</span><span class="sxs-lookup"><span data-stu-id="41215-165">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="41215-166">После успешного сброса пароля можно выполнить вход с помощью электронной почты и нового пароля.</span><span class="sxs-lookup"><span data-stu-id="41215-166">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="41215-167">Изменить время ожидания для электронной почты и активности</span><span class="sxs-lookup"><span data-stu-id="41215-167">Change email and activity timeout</span></span>

<span data-ttu-id="41215-168">Время ожидания бездействия по умолчанию составляет 14 дней.</span><span class="sxs-lookup"><span data-stu-id="41215-168">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="41215-169">Следующий код задает время ожидания простоя в 5 дней:</span><span class="sxs-lookup"><span data-stu-id="41215-169">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="41215-170">Изменить все продолжительность существования маркеров защиты данных</span><span class="sxs-lookup"><span data-stu-id="41215-170">Change all data protection token lifespans</span></span>

<span data-ttu-id="41215-171">Следующий код изменяет все маркеры защиты данных в течение времени ожидания до 3 часов:</span><span class="sxs-lookup"><span data-stu-id="41215-171">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

<span data-ttu-id="41215-172">Встроенные маркеры пользователей удостоверений (см. [AspNetCore/src/Identity/Extensions. Core/src/токеноптионс. CS](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) имеют [время ожидания в один день](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="41215-172">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="41215-173">Изменение срока существования маркера электронной почты</span><span class="sxs-lookup"><span data-stu-id="41215-173">Change the email token lifespan</span></span>

<span data-ttu-id="41215-174">Срок существования токена [пользователя](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) по умолчанию — [один день](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="41215-174">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="41215-175">В этом разделе показано, как изменить срок существования маркера электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-175">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="41215-176">Добавьте настраиваемую [датапротектортокенпровидер\<тусер >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) и <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="41215-176">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="41215-177">Добавьте настраиваемый поставщик в контейнер службы:</span><span class="sxs-lookup"><span data-stu-id="41215-177">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="41215-178">Подтверждение повторной отправки электронной почты</span><span class="sxs-lookup"><span data-stu-id="41215-178">Resend email confirmation</span></span>

<span data-ttu-id="41215-179">Также см. [эту проблему в GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="41215-179">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="41215-180">Отладка электронной почты</span><span class="sxs-lookup"><span data-stu-id="41215-180">Debug email</span></span>

<span data-ttu-id="41215-181">Если вы не можете работать с электронной почтой:</span><span class="sxs-lookup"><span data-stu-id="41215-181">If you can't get email working:</span></span>

* <span data-ttu-id="41215-182">Установите точку останова в `EmailSender.Execute` для проверки вызова `SendGridClient.SendEmailAsync`.</span><span class="sxs-lookup"><span data-stu-id="41215-182">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="41215-183">Создание [консольного приложения для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) с помощью аналогичного кода для `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="41215-183">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="41215-184">Ознакомьтесь со страницей [действие по электронной почте](https://sendgrid.com/docs/User_Guide/email_activity.html) .</span><span class="sxs-lookup"><span data-stu-id="41215-184">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="41215-185">Проверьте папку спама.</span><span class="sxs-lookup"><span data-stu-id="41215-185">Check your spam folder.</span></span>
* <span data-ttu-id="41215-186">Попробуйте использовать другой псевдоним электронной почты в другом поставщике электронной почты (Microsoft, Yahoo, Gmail и т. д.).</span><span class="sxs-lookup"><span data-stu-id="41215-186">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="41215-187">Попробуйте отправить их в другую учетную запись электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-187">Try sending to different email accounts.</span></span>

<span data-ttu-id="41215-188">В целях **безопасности** рекомендуется **не** использовать рабочие секреты в тестировании и разработке.</span><span class="sxs-lookup"><span data-stu-id="41215-188">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="41215-189">Если приложение публикуется в Azure, задайте секреты SendGrid как параметры приложения на портале веб-приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="41215-189">If you publish the app to Azure, set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="41215-190">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="41215-190">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="41215-191">Объединение социальных и местных учетных записей входа</span><span class="sxs-lookup"><span data-stu-id="41215-191">Combine social and local login accounts</span></span>

<span data-ttu-id="41215-192">Для выполнения этого раздела необходимо сначала включить внешний поставщик проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="41215-192">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="41215-193">См. статью [Проверка подлинности в Facebook, Google и внешнем поставщике](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="41215-193">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="41215-194">Вы можете объединить локальные и социальные учетные записи, щелкнув ссылку на электронную почту.</span><span class="sxs-lookup"><span data-stu-id="41215-194">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="41215-195">В следующей последовательности «RickAndMSFT@gmail.com» сначала создается как локальное имя входа. Тем не менее можно сначала создать учетную запись в качестве имени для входа в социальных сетях, а затем добавить локальное имя входа.</span><span class="sxs-lookup"><span data-stu-id="41215-195">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Веб-приложение: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

<span data-ttu-id="41215-197">Щелкните ссылку **Управление** .</span><span class="sxs-lookup"><span data-stu-id="41215-197">Click on the **Manage** link.</span></span> <span data-ttu-id="41215-198">Обратите внимание на 0 внешних (социальных) имен, связанных с этой учетной записью.</span><span class="sxs-lookup"><span data-stu-id="41215-198">Note the 0 external (social logins) associated with this account.</span></span>

![Управление представлением](accconfirm/_static/manage.png)

<span data-ttu-id="41215-200">Щелкните ссылку на другую службу входа и примите запросы приложения.</span><span class="sxs-lookup"><span data-stu-id="41215-200">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="41215-201">На следующем рисунке Facebook является внешним поставщиком проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="41215-201">In the following image, Facebook is the external authentication provider:</span></span>

![Управление внешними именами входа Просмотр списка Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="41215-203">Две учетные записи были объединены.</span><span class="sxs-lookup"><span data-stu-id="41215-203">The two accounts have been combined.</span></span> <span data-ttu-id="41215-204">Вы можете войти с помощью любой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="41215-204">You are able to sign in with either account.</span></span> <span data-ttu-id="41215-205">Пользователям может потребоваться добавить локальные учетные записи на случай, если служба проверки подлинности в социальных сетях не работает, или более вероятно, что они потеряют доступ к учетной записи социальных сетей.</span><span class="sxs-lookup"><span data-stu-id="41215-205">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="41215-206">Включить подтверждение учетной записи после того, как у сайта есть пользователи</span><span class="sxs-lookup"><span data-stu-id="41215-206">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="41215-207">Включение подтверждения учетной записи на сайте с пользователями блокирует всех существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="41215-207">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="41215-208">Существующие пользователи заблокированы, так как их учетные записи не подтверждены.</span><span class="sxs-lookup"><span data-stu-id="41215-208">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="41215-209">Чтобы обойти существующую блокировку пользователей, используйте один из следующих подходов.</span><span class="sxs-lookup"><span data-stu-id="41215-209">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="41215-210">Обновите базу данных, чтобы пометить все существующие пользователи как подтвержденные.</span><span class="sxs-lookup"><span data-stu-id="41215-210">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="41215-211">Подтвердите существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="41215-211">Confirm existing users.</span></span> <span data-ttu-id="41215-212">Например, пакетная отправка сообщений электронной почты с ссылками для подтверждения.</span><span class="sxs-lookup"><span data-stu-id="41215-212">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0 < aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="41215-213">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="41215-213">Prerequisites</span></span>

[<span data-ttu-id="41215-214">Пакет SDK для .NET Core 2,2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="41215-214">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="41215-215">Создание веб-приложения и удостоверение шаблона</span><span class="sxs-lookup"><span data-stu-id="41215-215">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="41215-216">Выполните следующие команды, чтобы создать веб-приложение с проверкой подлинности.</span><span class="sxs-lookup"><span data-stu-id="41215-216">Run the following commands to create a web app with authentication.</span></span>

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="41215-217">Тестирование регистрации нового пользователя</span><span class="sxs-lookup"><span data-stu-id="41215-217">Test new user registration</span></span>

<span data-ttu-id="41215-218">Запустите приложение, выберите ссылку **Register** и зарегистрируйте пользователя.</span><span class="sxs-lookup"><span data-stu-id="41215-218">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="41215-219">На этом этапе единственной проверкой адреса электронной почты является атрибут [`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) .</span><span class="sxs-lookup"><span data-stu-id="41215-219">At this point, the only validation on the email is with the [`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="41215-220">После отправки регистрации вы вошли в приложение.</span><span class="sxs-lookup"><span data-stu-id="41215-220">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="41215-221">Далее в этом руководстве код обновляется, поэтому новые пользователи не смогут войти в систему, пока не будет проверена электронная почта.</span><span class="sxs-lookup"><span data-stu-id="41215-221">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="41215-222">Обратите внимание, что поле `EmailConfirmed` таблицы `False`.</span><span class="sxs-lookup"><span data-stu-id="41215-222">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="41215-223">Вы можете снова использовать это сообщение на следующем шаге, когда приложение отправит сообщение электронной почты с подтверждением.</span><span class="sxs-lookup"><span data-stu-id="41215-223">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="41215-224">Щелкните строку правой кнопкой мыши и выберите команду **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="41215-224">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="41215-225">Удаление псевдонима электронной почты упрощает следующие шаги.</span><span class="sxs-lookup"><span data-stu-id="41215-225">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="41215-226">Требовать подтверждение по электронной почте</span><span class="sxs-lookup"><span data-stu-id="41215-226">Require email confirmation</span></span>

<span data-ttu-id="41215-227">Рекомендуется подтвердить сообщение электронной почты о новой регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="41215-227">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="41215-228">Подтверждение по электронной почте помогает проверить, что они не олицетворяют другого пользователя (т. е. они не зарегистрированы в сообщении электронной почты другого пользователя).</span><span class="sxs-lookup"><span data-stu-id="41215-228">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="41215-229">Предположим, у вас есть дискуссионный форум, и вы хотите предотвратить регистрацию "yli@example.com" как "nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="41215-229">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="41215-230">Без подтверждения по электронной почте "nolivetto@contoso.com" может получить от вашего приложения нежелательное сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-230">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="41215-231">Предположим, что пользователь случайно зарегистрировался как "ylo@example.com" и не заметил опечатку "или".</span><span class="sxs-lookup"><span data-stu-id="41215-231">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="41215-232">Они не смогут использовать восстановление пароля, так как у приложения нет нужной электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-232">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="41215-233">Подтверждение по электронной почте обеспечивает ограниченную защиту от программы-роботы.</span><span class="sxs-lookup"><span data-stu-id="41215-233">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="41215-234">Подтверждение по электронной почте не обеспечивает защиту от вредоносных пользователей со многими учетными записями электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-234">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="41215-235">Обычно требуется запретить новым пользователям отправлять данные на ваш веб-сайт, прежде чем они будут иметь подтвержденное сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-235">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="41215-236">Обновите `Startup.ConfigureServices`, чтобы требовать подтверждения по электронной почте:</span><span class="sxs-lookup"><span data-stu-id="41215-236">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="41215-237">`config.SignIn.RequireConfirmedEmail = true;` запрещает зарегистрированным пользователям входить в систему, пока не будет подтверждено их электронная почта.</span><span class="sxs-lookup"><span data-stu-id="41215-237">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="41215-238">Настройка поставщика электронной почты</span><span class="sxs-lookup"><span data-stu-id="41215-238">Configure email provider</span></span>

<span data-ttu-id="41215-239">В этом руководстве [SendGrid](https://sendgrid.com) используется для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-239">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="41215-240">Для отправки электронной почты требуется учетная запись SendGrid и ключ.</span><span class="sxs-lookup"><span data-stu-id="41215-240">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="41215-241">Вы можете использовать другие поставщики электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-241">You can use other email providers.</span></span> <span data-ttu-id="41215-242">ASP.NET Core 2. x включает `System.Net.Mail`, что позволяет отправлять электронную почту из приложения.</span><span class="sxs-lookup"><span data-stu-id="41215-242">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="41215-243">Для отправки электронной почты рекомендуется использовать SendGrid или другую почтовую службу.</span><span class="sxs-lookup"><span data-stu-id="41215-243">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="41215-244">Протокол SMTP трудно защитить и настроить правильно.</span><span class="sxs-lookup"><span data-stu-id="41215-244">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="41215-245">Создайте класс для выборки ключа защищенной электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-245">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="41215-246">Для этого примера создайте *Services/аусмессажесендероптионс. CS*:</span><span class="sxs-lookup"><span data-stu-id="41215-246">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="41215-247">Настройка секретов пользователя SendGrid</span><span class="sxs-lookup"><span data-stu-id="41215-247">Configure SendGrid user secrets</span></span>

<span data-ttu-id="41215-248">Задайте `SendGridUser` и `SendGridKey` с помощью [средства Secret-Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="41215-248">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="41215-249">Например:</span><span class="sxs-lookup"><span data-stu-id="41215-249">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="41215-250">В Windows Диспетчер секретов сохраняет пары "ключ-значение" в файле *секреты. JSON* в каталоге `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.</span><span class="sxs-lookup"><span data-stu-id="41215-250">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="41215-251">Содержимое файла *секреты. JSON* не шифруется.</span><span class="sxs-lookup"><span data-stu-id="41215-251">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="41215-252">В следующей разметке показан файл *секреты. JSON* .</span><span class="sxs-lookup"><span data-stu-id="41215-252">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="41215-253">Значение `SendGridKey` было удалено.</span><span class="sxs-lookup"><span data-stu-id="41215-253">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="41215-254">Дополнительные сведения см. в разделе [шаблон](xref:fundamentals/configuration/options) и [Конфигурация](xref:fundamentals/configuration/index)параметров.</span><span class="sxs-lookup"><span data-stu-id="41215-254">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="41215-255">Установка SendGrid</span><span class="sxs-lookup"><span data-stu-id="41215-255">Install SendGrid</span></span>

<span data-ttu-id="41215-256">В этом руководстве показано, как добавлять уведомления по электронной почте через [SendGrid](https://sendgrid.com/), но можно отправлять сообщения электронной почты с помощью SMTP и других механизмов.</span><span class="sxs-lookup"><span data-stu-id="41215-256">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="41215-257">Установите `SendGrid` пакет NuGet:</span><span class="sxs-lookup"><span data-stu-id="41215-257">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="41215-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41215-258">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="41215-259">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="41215-259">From the Package Manager Console, enter the following command:</span></span>

```powershell
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="41215-260">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="41215-260">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="41215-261">В консоли введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="41215-261">From the console, enter the following command:</span></span>

```dotnetcli
dotnet add package SendGrid
```

---

<span data-ttu-id="41215-262">Ознакомьтесь [со статьей начало работы с SendGrid](https://sendgrid.com/free/) , чтобы зарегистрироваться для получения бесплатной учетной записи SendGrid.</span><span class="sxs-lookup"><span data-stu-id="41215-262">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="41215-263">Реализация Иемаилсендер</span><span class="sxs-lookup"><span data-stu-id="41215-263">Implement IEmailSender</span></span>

<span data-ttu-id="41215-264">Чтобы реализовать `IEmailSender`, создайте *службы/емаилсендер. CS* с кодом, аналогичным следующему:</span><span class="sxs-lookup"><span data-stu-id="41215-264">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="41215-265">Настройка запуска для поддержки электронной почты</span><span class="sxs-lookup"><span data-stu-id="41215-265">Configure startup to support email</span></span>

<span data-ttu-id="41215-266">Добавьте следующий код в метод `ConfigureServices` в файле *Startup.CS* :</span><span class="sxs-lookup"><span data-stu-id="41215-266">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="41215-267">Добавьте `EmailSender` в качестве временной службы.</span><span class="sxs-lookup"><span data-stu-id="41215-267">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="41215-268">Зарегистрируйте экземпляр конфигурации `AuthMessageSenderOptions`.</span><span class="sxs-lookup"><span data-stu-id="41215-268">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="41215-269">Включение подтверждения учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="41215-269">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="41215-270">Шаблон содержит код для подтверждения учетной записи и восстановления пароля.</span><span class="sxs-lookup"><span data-stu-id="41215-270">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="41215-271">Найдите метод `OnPostAsync` в *области/удостоверение/страницы/учетная запись/Register. cshtml. CS*.</span><span class="sxs-lookup"><span data-stu-id="41215-271">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="41215-272">Запретите автоматический вход новых зарегистрированных пользователей с помощью комментария к следующей строке:</span><span class="sxs-lookup"><span data-stu-id="41215-272">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="41215-273">Показан полный метод с выделенной измененной строкой:</span><span class="sxs-lookup"><span data-stu-id="41215-273">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="41215-274">Регистрация, подтверждение электронной почты и сброс пароля</span><span class="sxs-lookup"><span data-stu-id="41215-274">Register, confirm email, and reset password</span></span>

<span data-ttu-id="41215-275">Запустите веб-приложение и протестируйте поток подтверждения учетной записи и восстановления пароля.</span><span class="sxs-lookup"><span data-stu-id="41215-275">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="41215-276">Запустите приложение и зарегистрируйте нового пользователя</span><span class="sxs-lookup"><span data-stu-id="41215-276">Run the app and register a new user</span></span>
* <span data-ttu-id="41215-277">Проверьте свою электронную почту на наличие ссылки для подтверждения учетной записи.</span><span class="sxs-lookup"><span data-stu-id="41215-277">Check your email for the account confirmation link.</span></span> <span data-ttu-id="41215-278">Если вы не получаете электронное письмо, см. раздел [Отладка электронной почты](#debug) .</span><span class="sxs-lookup"><span data-stu-id="41215-278">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="41215-279">Щелкните ссылку, чтобы подтвердить свою электронную почту.</span><span class="sxs-lookup"><span data-stu-id="41215-279">Click the link to confirm your email.</span></span>
* <span data-ttu-id="41215-280">Выполните вход, используя свой адрес электронной почты и пароль.</span><span class="sxs-lookup"><span data-stu-id="41215-280">Sign in with your email and password.</span></span>
* <span data-ttu-id="41215-281">Выполните выход.</span><span class="sxs-lookup"><span data-stu-id="41215-281">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="41215-282">Просмотр страницы "Управление"</span><span class="sxs-lookup"><span data-stu-id="41215-282">View the manage page</span></span>

<span data-ttu-id="41215-283">Выберите имя пользователя в браузере: ![окно браузера с именем пользователя](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="41215-283">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="41215-284">Страница Управление отображается с выбранной вкладкой **профиль** .</span><span class="sxs-lookup"><span data-stu-id="41215-284">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="41215-285">В **сообщении электронной почты** отображается флажок, указывающий, что сообщение электронной почты подтверждено.</span><span class="sxs-lookup"><span data-stu-id="41215-285">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="41215-286">Проверка сброса пароля</span><span class="sxs-lookup"><span data-stu-id="41215-286">Test password reset</span></span>

* <span data-ttu-id="41215-287">Если вы вошли в этот компьютер, выберите **выход**.</span><span class="sxs-lookup"><span data-stu-id="41215-287">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="41215-288">Щелкните ссылку **войти** и выберите ссылку **забыли пароль?** .</span><span class="sxs-lookup"><span data-stu-id="41215-288">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="41215-289">Введите адрес электронной почты, использованный для регистрации учетной записи.</span><span class="sxs-lookup"><span data-stu-id="41215-289">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="41215-290">Отправляется сообщение электронной почты со ссылкой для сброса пароля.</span><span class="sxs-lookup"><span data-stu-id="41215-290">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="41215-291">Проверьте свой адрес электронной почты и щелкните ссылку, чтобы сбросить пароль.</span><span class="sxs-lookup"><span data-stu-id="41215-291">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="41215-292">После успешного сброса пароля можно выполнить вход с помощью электронной почты и нового пароля.</span><span class="sxs-lookup"><span data-stu-id="41215-292">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="41215-293">Изменить время ожидания для электронной почты и активности</span><span class="sxs-lookup"><span data-stu-id="41215-293">Change email and activity timeout</span></span>

<span data-ttu-id="41215-294">Время ожидания бездействия по умолчанию составляет 14 дней.</span><span class="sxs-lookup"><span data-stu-id="41215-294">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="41215-295">Следующий код задает время ожидания простоя в 5 дней:</span><span class="sxs-lookup"><span data-stu-id="41215-295">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="41215-296">Изменить все продолжительность существования маркеров защиты данных</span><span class="sxs-lookup"><span data-stu-id="41215-296">Change all data protection token lifespans</span></span>

<span data-ttu-id="41215-297">Следующий код изменяет все маркеры защиты данных в течение времени ожидания до 3 часов:</span><span class="sxs-lookup"><span data-stu-id="41215-297">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="41215-298">Встроенные маркеры пользователей удостоверений (см. [AspNetCore/src/Identity/Extensions. Core/src/токеноптионс. CS](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) имеют [время ожидания в один день](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="41215-298">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="41215-299">Изменение срока существования маркера электронной почты</span><span class="sxs-lookup"><span data-stu-id="41215-299">Change the email token lifespan</span></span>

<span data-ttu-id="41215-300">Срок существования токена [пользователя](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) по умолчанию — [один день](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="41215-300">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="41215-301">В этом разделе показано, как изменить срок существования маркера электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-301">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="41215-302">Добавьте настраиваемую [датапротектортокенпровидер\<тусер >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) и <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="41215-302">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="41215-303">Добавьте настраиваемый поставщик в контейнер службы:</span><span class="sxs-lookup"><span data-stu-id="41215-303">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="41215-304">Подтверждение повторной отправки электронной почты</span><span class="sxs-lookup"><span data-stu-id="41215-304">Resend email confirmation</span></span>

<span data-ttu-id="41215-305">Также см. [эту проблему в GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="41215-305">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="41215-306">Отладка электронной почты</span><span class="sxs-lookup"><span data-stu-id="41215-306">Debug email</span></span>

<span data-ttu-id="41215-307">Если вы не можете работать с электронной почтой:</span><span class="sxs-lookup"><span data-stu-id="41215-307">If you can't get email working:</span></span>

* <span data-ttu-id="41215-308">Установите точку останова в `EmailSender.Execute` для проверки вызова `SendGridClient.SendEmailAsync`.</span><span class="sxs-lookup"><span data-stu-id="41215-308">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="41215-309">Создание [консольного приложения для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) с помощью аналогичного кода для `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="41215-309">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="41215-310">Ознакомьтесь со страницей [действие по электронной почте](https://sendgrid.com/docs/User_Guide/email_activity.html) .</span><span class="sxs-lookup"><span data-stu-id="41215-310">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="41215-311">Проверьте папку спама.</span><span class="sxs-lookup"><span data-stu-id="41215-311">Check your spam folder.</span></span>
* <span data-ttu-id="41215-312">Попробуйте использовать другой псевдоним электронной почты в другом поставщике электронной почты (Microsoft, Yahoo, Gmail и т. д.).</span><span class="sxs-lookup"><span data-stu-id="41215-312">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="41215-313">Попробуйте отправить их в другую учетную запись электронной почты.</span><span class="sxs-lookup"><span data-stu-id="41215-313">Try sending to different email accounts.</span></span>

<span data-ttu-id="41215-314">В целях **безопасности** рекомендуется **не** использовать рабочие секреты в тестировании и разработке.</span><span class="sxs-lookup"><span data-stu-id="41215-314">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="41215-315">При публикации приложения в Azure можно задать секреты SendGrid как параметры приложения на портале веб-приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="41215-315">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="41215-316">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="41215-316">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="41215-317">Объединение социальных и местных учетных записей входа</span><span class="sxs-lookup"><span data-stu-id="41215-317">Combine social and local login accounts</span></span>

<span data-ttu-id="41215-318">Для выполнения этого раздела необходимо сначала включить внешний поставщик проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="41215-318">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="41215-319">См. статью [Проверка подлинности в Facebook, Google и внешнем поставщике](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="41215-319">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="41215-320">Вы можете объединить локальные и социальные учетные записи, щелкнув ссылку на электронную почту.</span><span class="sxs-lookup"><span data-stu-id="41215-320">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="41215-321">В следующей последовательности «RickAndMSFT@gmail.com» сначала создается как локальное имя входа. Тем не менее можно сначала создать учетную запись в качестве имени для входа в социальных сетях, а затем добавить локальное имя входа.</span><span class="sxs-lookup"><span data-stu-id="41215-321">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Веб-приложение: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

<span data-ttu-id="41215-323">Щелкните ссылку **Управление** .</span><span class="sxs-lookup"><span data-stu-id="41215-323">Click on the **Manage** link.</span></span> <span data-ttu-id="41215-324">Обратите внимание на 0 внешних (социальных) имен, связанных с этой учетной записью.</span><span class="sxs-lookup"><span data-stu-id="41215-324">Note the 0 external (social logins) associated with this account.</span></span>

![Управление представлением](accconfirm/_static/manage.png)

<span data-ttu-id="41215-326">Щелкните ссылку на другую службу входа и примите запросы приложения.</span><span class="sxs-lookup"><span data-stu-id="41215-326">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="41215-327">На следующем рисунке Facebook является внешним поставщиком проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="41215-327">In the following image, Facebook is the external authentication provider:</span></span>

![Управление внешними именами входа Просмотр списка Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="41215-329">Две учетные записи были объединены.</span><span class="sxs-lookup"><span data-stu-id="41215-329">The two accounts have been combined.</span></span> <span data-ttu-id="41215-330">Вы можете войти с помощью любой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="41215-330">You are able to sign in with either account.</span></span> <span data-ttu-id="41215-331">Пользователям может потребоваться добавить локальные учетные записи на случай, если служба проверки подлинности в социальных сетях не работает, или более вероятно, что они потеряют доступ к учетной записи социальных сетей.</span><span class="sxs-lookup"><span data-stu-id="41215-331">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="41215-332">Включить подтверждение учетной записи после того, как у сайта есть пользователи</span><span class="sxs-lookup"><span data-stu-id="41215-332">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="41215-333">Включение подтверждения учетной записи на сайте с пользователями блокирует всех существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="41215-333">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="41215-334">Существующие пользователи заблокированы, так как их учетные записи не подтверждены.</span><span class="sxs-lookup"><span data-stu-id="41215-334">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="41215-335">Чтобы обойти существующую блокировку пользователей, используйте один из следующих подходов.</span><span class="sxs-lookup"><span data-stu-id="41215-335">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="41215-336">Обновите базу данных, чтобы пометить все существующие пользователи как подтвержденные.</span><span class="sxs-lookup"><span data-stu-id="41215-336">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="41215-337">Подтвердите существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="41215-337">Confirm existing users.</span></span> <span data-ttu-id="41215-338">Например, пакетная отправка сообщений электронной почты с ссылками для подтверждения.</span><span class="sxs-lookup"><span data-stu-id="41215-338">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
