---
title: Подтверждение учетной записи и восстановление пароля в ASP.NET Core
author: rick-anderson
description: Сведения о создании приложения ASP.NET Core с помощью по электронной почте подтверждение и сброс пароля.
ms.author: riande
ms.date: 03/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 802ba446af04df6a35ac73187ad693b8ec80c654
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814839"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="d55cb-103">Подтверждение учетной записи и восстановление пароля в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d55cb-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="d55cb-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), и [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="d55cb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="d55cb-105">Этом руководстве описывается создание приложения ASP.NET Core с помощью по электронной почте подтверждение и сброс пароля.</span><span class="sxs-lookup"><span data-stu-id="d55cb-105">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="d55cb-106">Это руководство представляет собой **не** начало раздела.</span><span class="sxs-lookup"><span data-stu-id="d55cb-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="d55cb-107">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="d55cb-107">You should be familiar with:</span></span>

* [<span data-ttu-id="d55cb-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d55cb-108">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="d55cb-109">Authentication</span><span class="sxs-lookup"><span data-stu-id="d55cb-109">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="d55cb-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d55cb-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="d55cb-111">См. в разделе [этот PDF-файл](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) для версии ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="d55cb-111">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 version.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="d55cb-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d55cb-112">Prerequisites</span></span>

[<span data-ttu-id="d55cb-113">Пакет SDK для .NET core 3.0 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="d55cb-113">.NET Core 3.0 SDK or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a><span data-ttu-id="d55cb-114">Создание и тестирование веб-приложения с помощью проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="d55cb-114">Create and test a web app with authentication</span></span>

<span data-ttu-id="d55cb-115">Выполните следующие команды для создания веб-приложения с помощью проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d55cb-115">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

<span data-ttu-id="d55cb-116">Запустите приложение, выберите **зарегистрировать** связать и регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="d55cb-116">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="d55cb-117">После регистрации вы будете перенаправлены к `/Identity/Account/RegisterConfirmation` страницу, которая содержит ссылку, чтобы имитировать подтверждение по электронной почте:</span><span class="sxs-lookup"><span data-stu-id="d55cb-117">Once registered, you are redirected to the to `/Identity/Account/RegisterConfirmation` page which contains a link to simulate email confirmation:</span></span>

* <span data-ttu-id="d55cb-118">Выберите `Click here to confirm your account` ссылку.</span><span class="sxs-lookup"><span data-stu-id="d55cb-118">Select the `Click here to confirm your account` link.</span></span>
* <span data-ttu-id="d55cb-119">Выберите **входа** ссылку и вход в систему с теми же учетными данными.</span><span class="sxs-lookup"><span data-stu-id="d55cb-119">Select the **Login** link and sign-in with the same credentials.</span></span>
* <span data-ttu-id="d55cb-120">Выберите `Hello YourEmail@provider.com!` ссылку, которую вы будете перенаправлены на `/Identity/Account/Manage/PersonalData` страницы.</span><span class="sxs-lookup"><span data-stu-id="d55cb-120">Select the `Hello YourEmail@provider.com!` link, which redirects you to the `/Identity/Account/Manage/PersonalData` page.</span></span>
* <span data-ttu-id="d55cb-121">Выберите **персональные данные** слева, а затем выберите **удалить**.</span><span class="sxs-lookup"><span data-stu-id="d55cb-121">Select the **Personal data** tab on the left, and then select **Delete**.</span></span>

### <a name="configure-an-email-provider"></a><span data-ttu-id="d55cb-122">Настройка поставщика электронной почты</span><span class="sxs-lookup"><span data-stu-id="d55cb-122">Configure an email provider</span></span>

<span data-ttu-id="d55cb-123">В этом руководстве [SendGrid](https://sendgrid.com) используется для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-123">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="d55cb-124">Требуется учетная запись SendGrid и ключ для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-124">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="d55cb-125">Можно использовать другие поставщики электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-125">You can use other email providers.</span></span> <span data-ttu-id="d55cb-126">Мы рекомендуем использовать SendGrid или другая служба электронной почты для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-126">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="d55cb-127">Для защиты и правильно настроить сложно SMTP.</span><span class="sxs-lookup"><span data-stu-id="d55cb-127">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="d55cb-128">Создание класса для извлечения ключа защиты электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-128">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="d55cb-129">Для этого примера создайте *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="d55cb-129">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="d55cb-130">Настройка SendGrid секреты пользователя</span><span class="sxs-lookup"><span data-stu-id="d55cb-130">Configure SendGrid user secrets</span></span>

<span data-ttu-id="d55cb-131">Задайте `SendGridUser` и `SendGridKey` с [средство secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d55cb-131">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="d55cb-132">Например:</span><span class="sxs-lookup"><span data-stu-id="d55cb-132">For example:</span></span>

```console
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="d55cb-133">На Windows, Secret Manager хранит пары ключей и значений в *secrets.json* файл `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` каталог.</span><span class="sxs-lookup"><span data-stu-id="d55cb-133">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="d55cb-134">Содержание *secrets.json* файл не зашифрован.</span><span class="sxs-lookup"><span data-stu-id="d55cb-134">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="d55cb-135">Следующая разметка показывает *secrets.json* файла.</span><span class="sxs-lookup"><span data-stu-id="d55cb-135">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="d55cb-136">`SendGridKey` Значение удаляется.</span><span class="sxs-lookup"><span data-stu-id="d55cb-136">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="d55cb-137">Дополнительные сведения см. в разделе [шаблон параметров](xref:fundamentals/configuration/options) и [конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d55cb-137">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="d55cb-138">Установка SendGrid</span><span class="sxs-lookup"><span data-stu-id="d55cb-138">Install SendGrid</span></span>

<span data-ttu-id="d55cb-139">Этот учебник демонстрирует добавление уведомлений по электронной почте через [SendGrid](https://sendgrid.com/), но вы можете отправлять электронную почту с помощью SMTP и другие механизмы.</span><span class="sxs-lookup"><span data-stu-id="d55cb-139">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="d55cb-140">Установить `SendGrid` пакет NuGet:</span><span class="sxs-lookup"><span data-stu-id="d55cb-140">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d55cb-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d55cb-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d55cb-142">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d55cb-142">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d55cb-143">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d55cb-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d55cb-144">В консоли введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d55cb-144">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="d55cb-145">См. в разделе [приступить к работе с SendGrid бесплатно](https://sendgrid.com/free/) зарегистрироваться для получения бесплатной учетной записи SendGrid.</span><span class="sxs-lookup"><span data-stu-id="d55cb-145">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="d55cb-146">Реализовать IEmailSender</span><span class="sxs-lookup"><span data-stu-id="d55cb-146">Implement IEmailSender</span></span>

<span data-ttu-id="d55cb-147">Для реализации `IEmailSender`, создание *Services/EmailSender.cs* с кодом, аналогичную следующей:</span><span class="sxs-lookup"><span data-stu-id="d55cb-147">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="d55cb-148">Настройка запуска для поддержки по электронной почте</span><span class="sxs-lookup"><span data-stu-id="d55cb-148">Configure startup to support email</span></span>

<span data-ttu-id="d55cb-149">Добавьте следующий код, чтобы `ConfigureServices` метод в *Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="d55cb-149">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="d55cb-150">Добавление `EmailSender` как временной ошибкой службы.</span><span class="sxs-lookup"><span data-stu-id="d55cb-150">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="d55cb-151">Зарегистрировать `AuthMessageSenderOptions` конфигурации экземпляра.</span><span class="sxs-lookup"><span data-stu-id="d55cb-151">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="d55cb-152">Регистрация, подтверждение электронной почты и сброс пароля</span><span class="sxs-lookup"><span data-stu-id="d55cb-152">Register, confirm email, and reset password</span></span>

<span data-ttu-id="d55cb-153">Запустите веб-приложение и протестируйте подтверждение учетной записи и процесс восстановления пароля.</span><span class="sxs-lookup"><span data-stu-id="d55cb-153">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="d55cb-154">Запустите приложение и зарегистрируйте нового пользователя</span><span class="sxs-lookup"><span data-stu-id="d55cb-154">Run the app and register a new user</span></span>
* <span data-ttu-id="d55cb-155">Проверьте свой адрес электронной почты для ссылку для подтверждения учетной записи.</span><span class="sxs-lookup"><span data-stu-id="d55cb-155">Check your email for the account confirmation link.</span></span> <span data-ttu-id="d55cb-156">См. в разделе [отладки электронной почты](#debug) Если вы не получили сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-156">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="d55cb-157">Щелкните ссылку, чтобы подтвердить свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-157">Click the link to confirm your email.</span></span>
* <span data-ttu-id="d55cb-158">Войдите, используя свой адрес электронной почты и пароль.</span><span class="sxs-lookup"><span data-stu-id="d55cb-158">Sign in with your email and password.</span></span>
* <span data-ttu-id="d55cb-159">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="d55cb-159">Sign out.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="d55cb-160">Сброс пароля теста</span><span class="sxs-lookup"><span data-stu-id="d55cb-160">Test password reset</span></span>

* <span data-ttu-id="d55cb-161">Если вы уже вошли в, выберите **выхода**.</span><span class="sxs-lookup"><span data-stu-id="d55cb-161">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="d55cb-162">Выберите **вход** ссылку и выберите **забыли пароль?** ссылку.</span><span class="sxs-lookup"><span data-stu-id="d55cb-162">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="d55cb-163">Введите адрес электронной почты, которая использовалась для регистрации учетной записи.</span><span class="sxs-lookup"><span data-stu-id="d55cb-163">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="d55cb-164">Отправляется сообщение электронной почты со ссылкой для сброса пароля.</span><span class="sxs-lookup"><span data-stu-id="d55cb-164">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="d55cb-165">Проверьте свой адрес электронной почты и щелкните ссылку, чтобы сбросить пароль.</span><span class="sxs-lookup"><span data-stu-id="d55cb-165">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="d55cb-166">После успешного сброса пароля вы можете войти с использованием электронной почты и новый пароль.</span><span class="sxs-lookup"><span data-stu-id="d55cb-166">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="d55cb-167">Изменить время ожидания сообщения электронной почты и действия</span><span class="sxs-lookup"><span data-stu-id="d55cb-167">Change email and activity timeout</span></span>

<span data-ttu-id="d55cb-168">Время ожидания бездействия по умолчанию — 14 дней.</span><span class="sxs-lookup"><span data-stu-id="d55cb-168">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="d55cb-169">Следующий код задает время ожидания в бездействии до 5 дней:</span><span class="sxs-lookup"><span data-stu-id="d55cb-169">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="d55cb-170">Изменить все самозаверяющиеся маркеров защиты данных</span><span class="sxs-lookup"><span data-stu-id="d55cb-170">Change all data protection token lifespans</span></span>

<span data-ttu-id="d55cb-171">Следующий код изменяет все ожидания маркеры защиты данных на 3 часа:</span><span class="sxs-lookup"><span data-stu-id="d55cb-171">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

<span data-ttu-id="d55cb-172">Встроенные в маркерах удостоверения пользователя (см. в разделе [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) имеют [одного дня время ожидания](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d55cb-172">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="d55cb-173">Изменить время существования токена электронной почты</span><span class="sxs-lookup"><span data-stu-id="d55cb-173">Change the email token lifespan</span></span>

<span data-ttu-id="d55cb-174">По умолчанию время существования токена доступа из [маркеров идентификации пользователя](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) — [один день](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d55cb-174">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="d55cb-175">В этом разделе показано, как изменить время существования токена электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-175">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="d55cb-176">Добавить пользовательское [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) и <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="d55cb-176">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="d55cb-177">Добавьте пользовательский поставщик в контейнер службы:</span><span class="sxs-lookup"><span data-stu-id="d55cb-177">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="d55cb-178">Повторно отправить подтверждение по электронной почте</span><span class="sxs-lookup"><span data-stu-id="d55cb-178">Resend email confirmation</span></span>

<span data-ttu-id="d55cb-179">См. в разделе [проблема GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="d55cb-179">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="d55cb-180">Отладка по электронной почте</span><span class="sxs-lookup"><span data-stu-id="d55cb-180">Debug email</span></span>

<span data-ttu-id="d55cb-181">Если не удается получить рабочий адрес электронной почты:</span><span class="sxs-lookup"><span data-stu-id="d55cb-181">If you can't get email working:</span></span>

* <span data-ttu-id="d55cb-182">Установите точку останова в `EmailSender.Execute` для проверки `SendGridClient.SendEmailAsync` вызывается.</span><span class="sxs-lookup"><span data-stu-id="d55cb-182">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="d55cb-183">Создание [консольное приложение для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) аналогичный код, используя `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="d55cb-183">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="d55cb-184">Просмотрите [действия "сообщения"](https://sendgrid.com/docs/User_Guide/email_activity.html) страницы.</span><span class="sxs-lookup"><span data-stu-id="d55cb-184">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="d55cb-185">Проверьте папку нежелательной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-185">Check your spam folder.</span></span>
* <span data-ttu-id="d55cb-186">Попробуйте другой псевдоним электронной почты на другой поставщик электронной почты (Microsoft, Yahoo, Gmail, и т.д.)</span><span class="sxs-lookup"><span data-stu-id="d55cb-186">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="d55cb-187">Попробуйте отправить учетными записями электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-187">Try sending to different email accounts.</span></span>

<span data-ttu-id="d55cb-188">**Рекомендации по безопасности** — **не** используйте секреты производства в разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="d55cb-188">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="d55cb-189">Если опубликовать приложение в Azure, задайте параметры приложения на портале веб-приложения Azure секреты SendGrid.</span><span class="sxs-lookup"><span data-stu-id="d55cb-189">If you publish the app to Azure, set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="d55cb-190">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="d55cb-190">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="d55cb-191">Объединить учетные записи социальных сетей и локального имени входа</span><span class="sxs-lookup"><span data-stu-id="d55cb-191">Combine social and local login accounts</span></span>

<span data-ttu-id="d55cb-192">Для завершения этого раздела необходимо сначала включить внешний поставщик аутентификации.</span><span class="sxs-lookup"><span data-stu-id="d55cb-192">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="d55cb-193">См. в разделе [Facebook, Google и внешнего поставщика проверки подлинности](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="d55cb-193">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="d55cb-194">Можно объединить учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="d55cb-194">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="d55cb-195">В следующей последовательности «RickAndMSFT@gmail.com"сначала создается как локальное имя входа; тем не менее, можно сначала создать учетную запись как имя для входа социальных сетей, а затем добавить локальное имя входа.</span><span class="sxs-lookup"><span data-stu-id="d55cb-195">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Веб-приложения: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

<span data-ttu-id="d55cb-197">Щелкните **управление** ссылку.</span><span class="sxs-lookup"><span data-stu-id="d55cb-197">Click on the **Manage** link.</span></span> <span data-ttu-id="d55cb-198">Примечание внешний 0 (имена входа социальных сетей), связанное с этой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="d55cb-198">Note the 0 external (social logins) associated with this account.</span></span>

![Управления представлением](accconfirm/_static/manage.png)

<span data-ttu-id="d55cb-200">Щелкните ссылку на другую службу входа в систему и принимать запросы приложений.</span><span class="sxs-lookup"><span data-stu-id="d55cb-200">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="d55cb-201">На следующем рисунке Facebook, — это внешнего поставщика проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="d55cb-201">In the following image, Facebook is the external authentication provider:</span></span>

![Управление ваш список внешних имен входа Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="d55cb-203">Две учетные записи были объединены.</span><span class="sxs-lookup"><span data-stu-id="d55cb-203">The two accounts have been combined.</span></span> <span data-ttu-id="d55cb-204">Вы сможете выполнить вход с использованием либо учетной записи.</span><span class="sxs-lookup"><span data-stu-id="d55cb-204">You are able to sign in with either account.</span></span> <span data-ttu-id="d55cb-205">Вы можете добавить локальные учетные записи, в случае их учетные записи социальных сетей службы проверки подлинности не работает, или более вероятно, они потеряли доступ к своей учетной записи социальной сети пользователей.</span><span class="sxs-lookup"><span data-stu-id="d55cb-205">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="d55cb-206">Включить подтверждение учетной записи, после узла имеет пользователей</span><span class="sxs-lookup"><span data-stu-id="d55cb-206">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="d55cb-207">Включение подтверждение учетной записи на сайте с пользователями блокирует работу всех существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="d55cb-207">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="d55cb-208">Существующие пользователи заблокированы, так как учетные записи не подтверждены.</span><span class="sxs-lookup"><span data-stu-id="d55cb-208">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="d55cb-209">Чтобы обойти существующие блокировки пользователя, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="d55cb-209">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="d55cb-210">Обновите базу данных для пометки всех существующих пользователей в качестве подтверждения.</span><span class="sxs-lookup"><span data-stu-id="d55cb-210">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="d55cb-211">Подтвердите существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="d55cb-211">Confirm existing users.</span></span> <span data-ttu-id="d55cb-212">Например пакетная Отправка сообщения электронной почты с подтверждением ссылки.</span><span class="sxs-lookup"><span data-stu-id="d55cb-212">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0 < aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="d55cb-213">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d55cb-213">Prerequisites</span></span>

[<span data-ttu-id="d55cb-214">Пакет SDK для .NET core 2.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="d55cb-214">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="d55cb-215">Создание веб-приложения и сформировать шаблон удостоверений</span><span class="sxs-lookup"><span data-stu-id="d55cb-215">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="d55cb-216">Выполните следующие команды для создания веб-приложения с помощью проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d55cb-216">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="d55cb-217">Регистрация нового пользователя для тестирования</span><span class="sxs-lookup"><span data-stu-id="d55cb-217">Test new user registration</span></span>

<span data-ttu-id="d55cb-218">Запустите приложение, выберите **зарегистрировать** связать и регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="d55cb-218">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="d55cb-219">На этом этапе является только проверка на адрес электронной почты с [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) атрибута.</span><span class="sxs-lookup"><span data-stu-id="d55cb-219">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="d55cb-220">После отправки регистрации, вы вошли в приложение.</span><span class="sxs-lookup"><span data-stu-id="d55cb-220">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="d55cb-221">Далее в этом руководстве код обновляется, поэтому новые пользователи не могут войти до окончания проверки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-221">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="d55cb-222">Обратите внимание, таблицы `EmailConfirmed` поле является `False`.</span><span class="sxs-lookup"><span data-stu-id="d55cb-222">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="d55cb-223">Вы можете использовать этот адрес электронной почты снова на следующем шаге, когда приложение отправляет сообщение электронной почты с подтверждением.</span><span class="sxs-lookup"><span data-stu-id="d55cb-223">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="d55cb-224">Щелкните правой кнопкой мыши в строке и выберите **удалить**.</span><span class="sxs-lookup"><span data-stu-id="d55cb-224">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="d55cb-225">Удаление псевдонима электронной почты упрощает в следующих шагах.</span><span class="sxs-lookup"><span data-stu-id="d55cb-225">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="d55cb-226">Требуется подтверждение по электронной почте</span><span class="sxs-lookup"><span data-stu-id="d55cb-226">Require email confirmation</span></span>

<span data-ttu-id="d55cb-227">Рекомендуется подтвердить адрес электронной почты новой регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="d55cb-227">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="d55cb-228">Отправить по электронной почте подтверждение помогает проверить, они не олицетворения кто-то другой (то есть они еще не зарегистрировано другого пользователя по электронной почте).</span><span class="sxs-lookup"><span data-stu-id="d55cb-228">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="d55cb-229">Предположим, что имеется дискуссионный форум и избежать "yli@example.com«из регистрации в качестве»nolivetto@contoso.com«.</span><span class="sxs-lookup"><span data-stu-id="d55cb-229">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="d55cb-230">Без подтверждение по электронной почте "nolivetto@contoso.com" могли получать нежелательные сообщения электронной почты из вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="d55cb-230">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="d55cb-231">Предположим, что пользователь случайно зарегистрирован как "ylo@example.com" и не заметили опечатку «yli».</span><span class="sxs-lookup"><span data-stu-id="d55cb-231">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="d55cb-232">Они не смогут использовать пароль восстановления, так как у приложения нет их правильный адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-232">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="d55cb-233">Подтверждение по электронной почте предоставляет ограниченную защиту от программ-роботов.</span><span class="sxs-lookup"><span data-stu-id="d55cb-233">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="d55cb-234">Подтверждение по электронной почте не обеспечивают защиту от пользователей-злоумышленников с многих учетных записей электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-234">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="d55cb-235">Обычно требуется запретить новым пользователям из учета данных для веб-сайт, прежде чем они получат подтвержден по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="d55cb-235">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="d55cb-236">Обновление `Startup.ConfigureServices` требуется подтвержден по электронной почте:</span><span class="sxs-lookup"><span data-stu-id="d55cb-236">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="d55cb-237">`config.SignIn.RequireConfirmedEmail = true;` запрещает вход до подтверждения их по электронной почте зарегистрированным пользователям.</span><span class="sxs-lookup"><span data-stu-id="d55cb-237">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="d55cb-238">Настройка поставщика услуг электронной почты</span><span class="sxs-lookup"><span data-stu-id="d55cb-238">Configure email provider</span></span>

<span data-ttu-id="d55cb-239">В этом руководстве [SendGrid](https://sendgrid.com) используется для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-239">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="d55cb-240">Требуется учетная запись SendGrid и ключ для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-240">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="d55cb-241">Можно использовать другие поставщики электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-241">You can use other email providers.</span></span> <span data-ttu-id="d55cb-242">ASP.NET Core 2.x включает `System.Net.Mail`, который позволяет отправлять электронную почту из приложения.</span><span class="sxs-lookup"><span data-stu-id="d55cb-242">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="d55cb-243">Мы рекомендуем использовать SendGrid или другая служба электронной почты для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-243">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="d55cb-244">Для защиты и правильно настроить сложно SMTP.</span><span class="sxs-lookup"><span data-stu-id="d55cb-244">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="d55cb-245">Создание класса для извлечения ключа защиты электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-245">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="d55cb-246">Для этого примера создайте *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="d55cb-246">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="d55cb-247">Настройка SendGrid секреты пользователя</span><span class="sxs-lookup"><span data-stu-id="d55cb-247">Configure SendGrid user secrets</span></span>

<span data-ttu-id="d55cb-248">Задайте `SendGridUser` и `SendGridKey` с [средство secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d55cb-248">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="d55cb-249">Например:</span><span class="sxs-lookup"><span data-stu-id="d55cb-249">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="d55cb-250">На Windows, Secret Manager хранит пары ключей и значений в *secrets.json* файл `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` каталог.</span><span class="sxs-lookup"><span data-stu-id="d55cb-250">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="d55cb-251">Содержание *secrets.json* файл не зашифрован.</span><span class="sxs-lookup"><span data-stu-id="d55cb-251">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="d55cb-252">Следующая разметка показывает *secrets.json* файла.</span><span class="sxs-lookup"><span data-stu-id="d55cb-252">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="d55cb-253">`SendGridKey` Значение удаляется.</span><span class="sxs-lookup"><span data-stu-id="d55cb-253">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="d55cb-254">Дополнительные сведения см. в разделе [шаблон параметров](xref:fundamentals/configuration/options) и [конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d55cb-254">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="d55cb-255">Установка SendGrid</span><span class="sxs-lookup"><span data-stu-id="d55cb-255">Install SendGrid</span></span>

<span data-ttu-id="d55cb-256">Этот учебник демонстрирует добавление уведомлений по электронной почте через [SendGrid](https://sendgrid.com/), но вы можете отправлять электронную почту с помощью SMTP и другие механизмы.</span><span class="sxs-lookup"><span data-stu-id="d55cb-256">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="d55cb-257">Установить `SendGrid` пакет NuGet:</span><span class="sxs-lookup"><span data-stu-id="d55cb-257">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d55cb-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d55cb-258">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d55cb-259">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d55cb-259">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d55cb-260">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d55cb-260">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d55cb-261">В консоли введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d55cb-261">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="d55cb-262">См. в разделе [приступить к работе с SendGrid бесплатно](https://sendgrid.com/free/) зарегистрироваться для получения бесплатной учетной записи SendGrid.</span><span class="sxs-lookup"><span data-stu-id="d55cb-262">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="d55cb-263">Реализовать IEmailSender</span><span class="sxs-lookup"><span data-stu-id="d55cb-263">Implement IEmailSender</span></span>

<span data-ttu-id="d55cb-264">Для реализации `IEmailSender`, создание *Services/EmailSender.cs* с кодом, аналогичную следующей:</span><span class="sxs-lookup"><span data-stu-id="d55cb-264">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="d55cb-265">Настройка запуска для поддержки по электронной почте</span><span class="sxs-lookup"><span data-stu-id="d55cb-265">Configure startup to support email</span></span>

<span data-ttu-id="d55cb-266">Добавьте следующий код, чтобы `ConfigureServices` метод в *Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="d55cb-266">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="d55cb-267">Добавление `EmailSender` как временной ошибкой службы.</span><span class="sxs-lookup"><span data-stu-id="d55cb-267">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="d55cb-268">Зарегистрировать `AuthMessageSenderOptions` конфигурации экземпляра.</span><span class="sxs-lookup"><span data-stu-id="d55cb-268">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="d55cb-269">Включить учетную запись, пароль и Подтверждение восстановления</span><span class="sxs-lookup"><span data-stu-id="d55cb-269">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="d55cb-270">Шаблон содержит код для восстановления и подтверждение пароля учетной записи.</span><span class="sxs-lookup"><span data-stu-id="d55cb-270">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="d55cb-271">Найти `OnPostAsync` метод в *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="d55cb-271">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="d55cb-272">Запретить новым зарегистрированным пользователям, чтобы автоматически входить, закомментируйте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="d55cb-272">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="d55cb-273">С помощью измененные строки выделены показан полный метод:</span><span class="sxs-lookup"><span data-stu-id="d55cb-273">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="d55cb-274">Регистрация, подтверждение электронной почты и сброс пароля</span><span class="sxs-lookup"><span data-stu-id="d55cb-274">Register, confirm email, and reset password</span></span>

<span data-ttu-id="d55cb-275">Запустите веб-приложение и протестируйте подтверждение учетной записи и процесс восстановления пароля.</span><span class="sxs-lookup"><span data-stu-id="d55cb-275">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="d55cb-276">Запустите приложение и зарегистрируйте нового пользователя</span><span class="sxs-lookup"><span data-stu-id="d55cb-276">Run the app and register a new user</span></span>
* <span data-ttu-id="d55cb-277">Проверьте свой адрес электронной почты для ссылку для подтверждения учетной записи.</span><span class="sxs-lookup"><span data-stu-id="d55cb-277">Check your email for the account confirmation link.</span></span> <span data-ttu-id="d55cb-278">См. в разделе [отладки электронной почты](#debug) Если вы не получили сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-278">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="d55cb-279">Щелкните ссылку, чтобы подтвердить свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-279">Click the link to confirm your email.</span></span>
* <span data-ttu-id="d55cb-280">Войдите, используя свой адрес электронной почты и пароль.</span><span class="sxs-lookup"><span data-stu-id="d55cb-280">Sign in with your email and password.</span></span>
* <span data-ttu-id="d55cb-281">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="d55cb-281">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="d55cb-282">Просмотреть страницу управление</span><span class="sxs-lookup"><span data-stu-id="d55cb-282">View the manage page</span></span>

<span data-ttu-id="d55cb-283">Выберите свое имя пользователя в браузере: ![окно браузера с именем пользователя](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="d55cb-283">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="d55cb-284">Управление страница отображается с **профиль** выделенной вкладки.</span><span class="sxs-lookup"><span data-stu-id="d55cb-284">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="d55cb-285">**Электронной почты** показано типа "флажок", указывающее, адрес электронной почты будет подтверждена.</span><span class="sxs-lookup"><span data-stu-id="d55cb-285">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="d55cb-286">Сброс пароля теста</span><span class="sxs-lookup"><span data-stu-id="d55cb-286">Test password reset</span></span>

* <span data-ttu-id="d55cb-287">Если вы уже вошли в, выберите **выхода**.</span><span class="sxs-lookup"><span data-stu-id="d55cb-287">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="d55cb-288">Выберите **вход** ссылку и выберите **забыли пароль?** ссылку.</span><span class="sxs-lookup"><span data-stu-id="d55cb-288">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="d55cb-289">Введите адрес электронной почты, которая использовалась для регистрации учетной записи.</span><span class="sxs-lookup"><span data-stu-id="d55cb-289">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="d55cb-290">Отправляется сообщение электронной почты со ссылкой для сброса пароля.</span><span class="sxs-lookup"><span data-stu-id="d55cb-290">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="d55cb-291">Проверьте свой адрес электронной почты и щелкните ссылку, чтобы сбросить пароль.</span><span class="sxs-lookup"><span data-stu-id="d55cb-291">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="d55cb-292">После успешного сброса пароля вы можете войти с использованием электронной почты и новый пароль.</span><span class="sxs-lookup"><span data-stu-id="d55cb-292">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="d55cb-293">Изменить время ожидания сообщения электронной почты и действия</span><span class="sxs-lookup"><span data-stu-id="d55cb-293">Change email and activity timeout</span></span>

<span data-ttu-id="d55cb-294">Время ожидания бездействия по умолчанию — 14 дней.</span><span class="sxs-lookup"><span data-stu-id="d55cb-294">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="d55cb-295">Следующий код задает время ожидания в бездействии до 5 дней:</span><span class="sxs-lookup"><span data-stu-id="d55cb-295">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="d55cb-296">Изменить все самозаверяющиеся маркеров защиты данных</span><span class="sxs-lookup"><span data-stu-id="d55cb-296">Change all data protection token lifespans</span></span>

<span data-ttu-id="d55cb-297">Следующий код изменяет все ожидания маркеры защиты данных на 3 часа:</span><span class="sxs-lookup"><span data-stu-id="d55cb-297">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="d55cb-298">Встроенные в маркерах удостоверения пользователя (см. в разделе [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) имеют [одного дня время ожидания](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d55cb-298">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="d55cb-299">Изменить время существования токена электронной почты</span><span class="sxs-lookup"><span data-stu-id="d55cb-299">Change the email token lifespan</span></span>

<span data-ttu-id="d55cb-300">По умолчанию время существования токена доступа из [маркеров идентификации пользователя](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) — [один день](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d55cb-300">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="d55cb-301">В этом разделе показано, как изменить время существования токена электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-301">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="d55cb-302">Добавить пользовательское [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) и <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="d55cb-302">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="d55cb-303">Добавьте пользовательский поставщик в контейнер службы:</span><span class="sxs-lookup"><span data-stu-id="d55cb-303">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="d55cb-304">Повторно отправить подтверждение по электронной почте</span><span class="sxs-lookup"><span data-stu-id="d55cb-304">Resend email confirmation</span></span>

<span data-ttu-id="d55cb-305">См. в разделе [проблема GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="d55cb-305">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="d55cb-306">Отладка по электронной почте</span><span class="sxs-lookup"><span data-stu-id="d55cb-306">Debug email</span></span>

<span data-ttu-id="d55cb-307">Если не удается получить рабочий адрес электронной почты:</span><span class="sxs-lookup"><span data-stu-id="d55cb-307">If you can't get email working:</span></span>

* <span data-ttu-id="d55cb-308">Установите точку останова в `EmailSender.Execute` для проверки `SendGridClient.SendEmailAsync` вызывается.</span><span class="sxs-lookup"><span data-stu-id="d55cb-308">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="d55cb-309">Создание [консольное приложение для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) аналогичный код, используя `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="d55cb-309">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="d55cb-310">Просмотрите [действия "сообщения"](https://sendgrid.com/docs/User_Guide/email_activity.html) страницы.</span><span class="sxs-lookup"><span data-stu-id="d55cb-310">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="d55cb-311">Проверьте папку нежелательной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-311">Check your spam folder.</span></span>
* <span data-ttu-id="d55cb-312">Попробуйте другой псевдоним электронной почты на другой поставщик электронной почты (Microsoft, Yahoo, Gmail, и т.д.)</span><span class="sxs-lookup"><span data-stu-id="d55cb-312">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="d55cb-313">Попробуйте отправить учетными записями электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d55cb-313">Try sending to different email accounts.</span></span>

<span data-ttu-id="d55cb-314">**Рекомендации по безопасности** — **не** используйте секреты производства в разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="d55cb-314">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="d55cb-315">При публикации приложения в Azure, секреты SendGrid можно задать как параметры приложения на портале веб-приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="d55cb-315">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="d55cb-316">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="d55cb-316">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="d55cb-317">Объединить учетные записи социальных сетей и локального имени входа</span><span class="sxs-lookup"><span data-stu-id="d55cb-317">Combine social and local login accounts</span></span>

<span data-ttu-id="d55cb-318">Для завершения этого раздела необходимо сначала включить внешний поставщик аутентификации.</span><span class="sxs-lookup"><span data-stu-id="d55cb-318">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="d55cb-319">См. в разделе [Facebook, Google и внешнего поставщика проверки подлинности](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="d55cb-319">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="d55cb-320">Можно объединить учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="d55cb-320">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="d55cb-321">В следующей последовательности «RickAndMSFT@gmail.com"сначала создается как локальное имя входа; тем не менее, можно сначала создать учетную запись как имя для входа социальных сетей, а затем добавить локальное имя входа.</span><span class="sxs-lookup"><span data-stu-id="d55cb-321">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Веб-приложения: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

<span data-ttu-id="d55cb-323">Щелкните **управление** ссылку.</span><span class="sxs-lookup"><span data-stu-id="d55cb-323">Click on the **Manage** link.</span></span> <span data-ttu-id="d55cb-324">Примечание внешний 0 (имена входа социальных сетей), связанное с этой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="d55cb-324">Note the 0 external (social logins) associated with this account.</span></span>

![Управления представлением](accconfirm/_static/manage.png)

<span data-ttu-id="d55cb-326">Щелкните ссылку на другую службу входа в систему и принимать запросы приложений.</span><span class="sxs-lookup"><span data-stu-id="d55cb-326">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="d55cb-327">На следующем рисунке Facebook, — это внешнего поставщика проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="d55cb-327">In the following image, Facebook is the external authentication provider:</span></span>

![Управление ваш список внешних имен входа Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="d55cb-329">Две учетные записи были объединены.</span><span class="sxs-lookup"><span data-stu-id="d55cb-329">The two accounts have been combined.</span></span> <span data-ttu-id="d55cb-330">Вы сможете выполнить вход с использованием либо учетной записи.</span><span class="sxs-lookup"><span data-stu-id="d55cb-330">You are able to sign in with either account.</span></span> <span data-ttu-id="d55cb-331">Вы можете добавить локальные учетные записи, в случае их учетные записи социальных сетей службы проверки подлинности не работает, или более вероятно, они потеряли доступ к своей учетной записи социальной сети пользователей.</span><span class="sxs-lookup"><span data-stu-id="d55cb-331">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="d55cb-332">Включить подтверждение учетной записи, после узла имеет пользователей</span><span class="sxs-lookup"><span data-stu-id="d55cb-332">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="d55cb-333">Включение подтверждение учетной записи на сайте с пользователями блокирует работу всех существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="d55cb-333">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="d55cb-334">Существующие пользователи заблокированы, так как учетные записи не подтверждены.</span><span class="sxs-lookup"><span data-stu-id="d55cb-334">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="d55cb-335">Чтобы обойти существующие блокировки пользователя, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="d55cb-335">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="d55cb-336">Обновите базу данных для пометки всех существующих пользователей в качестве подтверждения.</span><span class="sxs-lookup"><span data-stu-id="d55cb-336">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="d55cb-337">Подтвердите существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="d55cb-337">Confirm existing users.</span></span> <span data-ttu-id="d55cb-338">Например пакетная Отправка сообщения электронной почты с подтверждением ссылки.</span><span class="sxs-lookup"><span data-stu-id="d55cb-338">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
