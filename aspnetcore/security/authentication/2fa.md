---
title: Двухфакторная проверка подлинности с помощью SMS в ASP.NET Core
author: rick-anderson
description: Узнайте, как настроить двухфакторную проверку подлинности (2FA) с помощью приложения ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 68219579be9b7a7b25da6e348054e1ff2015cf5f
ms.sourcegitcommit: e54672f5c493258dc449fac5b98faf47eb123b28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2019
ms.locfileid: "71248384"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="adcd2-103">Двухфакторная проверка подлинности с помощью SMS в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="adcd2-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="adcd2-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [разработчики Швейцария](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="adcd2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

>[!WARNING]
> <span data-ttu-id="adcd2-105">Два фактора проверки подлинности (2FA) проверки подлинности приложения с помощью по времени одноразовый пароль алгоритм (TOTP), являются отрасли, рекомендуемый подход для 2FA.</span><span class="sxs-lookup"><span data-stu-id="adcd2-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="adcd2-106">2FA с помощью TOTP предпочтительнее SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="adcd2-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="adcd2-107">Дополнительные сведения см. в разделе [создания включить QR-кода для приложений TOTP проверки подлинности в ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) ASP.NET Core 2.0 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="adcd2-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="adcd2-108">Этом руководстве показано, как настроить двухфакторную проверку подлинности (2FA) с помощью SMS.</span><span class="sxs-lookup"><span data-stu-id="adcd2-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="adcd2-109">Инструкции, приведенные для [twilio](https://www.twilio.com/) и [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), но вы можете использовать любой другой поставщик SMS.</span><span class="sxs-lookup"><span data-stu-id="adcd2-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="adcd2-110">Мы рекомендуем вам выполнить [подтверждение учетной записи и восстановление пароля](xref:security/authentication/accconfirm) этим руководством.</span><span class="sxs-lookup"><span data-stu-id="adcd2-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="adcd2-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="adcd2-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="adcd2-112">[Как скачать](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="adcd2-112">[How to download](xref:index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="adcd2-113">Создание проекта ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="adcd2-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="adcd2-114">Создание нового веб-приложения ASP.NET Core с именем `Web2FA` с учетными записями отдельных пользователей.</span><span class="sxs-lookup"><span data-stu-id="adcd2-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="adcd2-115">Следуйте инструкциям в <xref:security/enforcing-ssl> , чтобы настроить и требовать протокол HTTPS.</span><span class="sxs-lookup"><span data-stu-id="adcd2-115">Follow the instructions in <xref:security/enforcing-ssl> to set up and require HTTPS.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="adcd2-116">Создание учетной записи SMS</span><span class="sxs-lookup"><span data-stu-id="adcd2-116">Create an SMS account</span></span>

<span data-ttu-id="adcd2-117">Создание учетной записи SMS, например, из [twilio](https://www.twilio.com/) или [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="adcd2-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="adcd2-118">Запишите учетные данные для проверки подлинности (для twilio: accountSid и authToken, для АСПСМС: Userkey и Password).</span><span class="sxs-lookup"><span data-stu-id="adcd2-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="adcd2-119">Выяснение учетные данные поставщика SMS</span><span class="sxs-lookup"><span data-stu-id="adcd2-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="adcd2-120">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="adcd2-120">**Twilio:**</span></span>

<span data-ttu-id="adcd2-121">На вкладке Панель мониторинга учетной записи Twilio скопируйте **SID учетной записи** и **токен проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="adcd2-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="adcd2-122">**АСПСМС:**</span><span class="sxs-lookup"><span data-stu-id="adcd2-122">**ASPSMS:**</span></span>

<span data-ttu-id="adcd2-123">В параметрах учетной записи перейдите по адресу **Userkey** и скопируйте его вместе с **паролем**.</span><span class="sxs-lookup"><span data-stu-id="adcd2-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="adcd2-124">Позже мы сохраним эти значения с помощью диспетчера секретов, в ключи `SMSAccountIdentification` и `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="adcd2-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="adcd2-125">Указав идентификатор SenderID / инициатора</span><span class="sxs-lookup"><span data-stu-id="adcd2-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="adcd2-126">**Twilio** На вкладке числа скопируйте **номер телефона**Twilio.</span><span class="sxs-lookup"><span data-stu-id="adcd2-126">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="adcd2-127">**АСПСМС:** В меню Источник разблокировки Разблокируйте один или несколько источник или выберите буквенно-цифровой инициатор (не поддерживается всеми сетями).</span><span class="sxs-lookup"><span data-stu-id="adcd2-127">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="adcd2-128">Позже мы Сохраним это значение с помощью диспетчера секретов, раздел `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="adcd2-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>

### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="adcd2-129">Укажите учетные данные для службы SMS</span><span class="sxs-lookup"><span data-stu-id="adcd2-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="adcd2-130">Мы будем использовать [шаблон параметров](xref:fundamentals/configuration/options) для доступа к параметрам и ключ учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="adcd2-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

* <span data-ttu-id="adcd2-131">Создание класса для извлечения ключа безопасности SMS.</span><span class="sxs-lookup"><span data-stu-id="adcd2-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="adcd2-132">В этом примере `SMSoptions` класс создается в *Services/SMSoptions.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="adcd2-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="adcd2-133">Задайте `SMSAccountIdentification`, `SMSAccountPassword` и `SMSAccountFrom` с [средство secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="adcd2-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="adcd2-134">Пример:</span><span class="sxs-lookup"><span data-stu-id="adcd2-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* <span data-ttu-id="adcd2-135">Добавьте пакет NuGet для поставщика SMS.</span><span class="sxs-lookup"><span data-stu-id="adcd2-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="adcd2-136">Из консоли диспетчера пакетов (PMC) выполните:</span><span class="sxs-lookup"><span data-stu-id="adcd2-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="adcd2-137">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="adcd2-137">**Twilio:**</span></span>

`Install-Package Twilio`

<span data-ttu-id="adcd2-138">**АСПСМС:**</span><span class="sxs-lookup"><span data-stu-id="adcd2-138">**ASPSMS:**</span></span>

`Install-Package ASPSMS`

* <span data-ttu-id="adcd2-139">Добавьте код в *Services/MessageServices.cs* файл, чтобы включить SMS.</span><span class="sxs-lookup"><span data-stu-id="adcd2-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="adcd2-140">Использование Twilio или разделе ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="adcd2-140">Use either the Twilio or the ASPSMS section:</span></span>

<span data-ttu-id="adcd2-141">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="adcd2-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="adcd2-142">**АСПСМС:**</span><span class="sxs-lookup"><span data-stu-id="adcd2-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="adcd2-143">Настройка запуска для использования `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="adcd2-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="adcd2-144">Добавить `SMSoptions` в контейнер службы в `ConfigureServices` метод в *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="adcd2-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="adcd2-145">Включение двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="adcd2-145">Enable two-factor authentication</span></span>

<span data-ttu-id="adcd2-146">Откройте файл представления Razor views */Manage/index. cshtml* и удалите символы комментария (поэтому разметка не записывается в комментарий).</span><span class="sxs-lookup"><span data-stu-id="adcd2-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commented out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="adcd2-147">Войдите с помощью двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="adcd2-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="adcd2-148">Запустите приложение и зарегистрируйте нового пользователя</span><span class="sxs-lookup"><span data-stu-id="adcd2-148">Run the app and register a new user</span></span>

![Веб-приложение Register, откройте представление в Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="adcd2-150">Выберите свое имя пользователя, который активирует `Index` метода действия в контроллере управление.</span><span class="sxs-lookup"><span data-stu-id="adcd2-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="adcd2-151">Выберите номер телефона **добавить** ссылку.</span><span class="sxs-lookup"><span data-stu-id="adcd2-151">Then tap the phone number **Add** link.</span></span>

![Управление представления – нажать ссылку «Добавить»](2fa/_static/login2fa2.png)

* <span data-ttu-id="adcd2-153">Добавить номер телефона, который будет получать код проверки и коснитесь **отправить проверочный код**.</span><span class="sxs-lookup"><span data-stu-id="adcd2-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Добавить страницу номер телефона](2fa/_static/login2fa3.png)

* <span data-ttu-id="adcd2-155">Вы получите текстовое сообщение с кодом проверки.</span><span class="sxs-lookup"><span data-stu-id="adcd2-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="adcd2-156">Введите его и коснитесь **отправки**</span><span class="sxs-lookup"><span data-stu-id="adcd2-156">Enter it and tap **Submit**</span></span>

![Проверить номер телефона страницу](2fa/_static/login2fa4.png)

<span data-ttu-id="adcd2-158">Если вы не получили текстовое сообщение, см. в разделе страницы журнала twilio.</span><span class="sxs-lookup"><span data-stu-id="adcd2-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="adcd2-159">Представления управление показывает, что ваш номер телефона успешно добавлен.</span><span class="sxs-lookup"><span data-stu-id="adcd2-159">The Manage view shows your phone number was added successfully.</span></span>

![Управление представление — номер телефона успешно добавлен](2fa/_static/login2fa5.png)

* <span data-ttu-id="adcd2-161">Коснитесь **включить** включить двухфакторную проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="adcd2-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Управления представлением — Включение двухфакторной проверки подлинности](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="adcd2-163">Тестирование двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="adcd2-163">Test two-factor authentication</span></span>

* <span data-ttu-id="adcd2-164">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="adcd2-164">Log off.</span></span>

* <span data-ttu-id="adcd2-165">Вход.</span><span class="sxs-lookup"><span data-stu-id="adcd2-165">Log in.</span></span>

* <span data-ttu-id="adcd2-166">Учетной записи пользователя включена двухфакторная проверка подлинности, поэтому вы должны предоставить второй фактор проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="adcd2-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="adcd2-167">В этом руководстве вы включили Проверка номера телефона.</span><span class="sxs-lookup"><span data-stu-id="adcd2-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="adcd2-168">Встроенные шаблоны позволяют настроить почту в качестве второго фактора.</span><span class="sxs-lookup"><span data-stu-id="adcd2-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="adcd2-169">Можно настроить дополнительные факторы проверки подлинности, например QR-коды, второй.</span><span class="sxs-lookup"><span data-stu-id="adcd2-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="adcd2-170">Коснитесь **отправить**.</span><span class="sxs-lookup"><span data-stu-id="adcd2-170">Tap **Submit**.</span></span>

![Отправить код проверки представления](2fa/_static/login2fa7.png)

* <span data-ttu-id="adcd2-172">Введите код, полученный в SMS-сообщения.</span><span class="sxs-lookup"><span data-stu-id="adcd2-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="adcd2-173">Щелкнув **запомнить браузер** флажок будет исключить от необходимости использовать 2FA для входа при использовании же устройства и браузера.</span><span class="sxs-lookup"><span data-stu-id="adcd2-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="adcd2-174">Включение 2FA и щелкнув **запомнить браузер** предоставит вам строгого 2FA защиты от злоумышленников, пытается получить доступ к вашей учетной записи, до тех пор, пока они не имеют доступа к устройству.</span><span class="sxs-lookup"><span data-stu-id="adcd2-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="adcd2-175">Это можно сделать на любом устройстве закрытого, которые регулярно использовать.</span><span class="sxs-lookup"><span data-stu-id="adcd2-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="adcd2-176">Установив **запомнить браузер**, вы получаете дополнительную защиту 2FA с устройств, вы не используете регулярно, и вы получаете удобства на не нужно изучать через 2FA на собственных устройствах.</span><span class="sxs-lookup"><span data-stu-id="adcd2-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Проверить представление](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="adcd2-178">Блокировка учетной записи, обеспечивающие защиту от атак методом подбора</span><span class="sxs-lookup"><span data-stu-id="adcd2-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="adcd2-179">Блокировка учетной записи рекомендуется с 2FA.</span><span class="sxs-lookup"><span data-stu-id="adcd2-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="adcd2-180">Когда пользователь войдет в приложение через локальную учетную запись или учетную запись социальной сети, каждая неудачная попытка 2FA хранится.</span><span class="sxs-lookup"><span data-stu-id="adcd2-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="adcd2-181">Если достигнуто максимальное число неудачных попыток доступа, пользователь блокируется (по умолчанию: 5-минутная блокировка после 5 неудачных попыток доступа).</span><span class="sxs-lookup"><span data-stu-id="adcd2-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="adcd2-182">Сбрасывает счетчик попыток неудачные попытки доступа успешной проверки подлинности и сбрасывает часы.</span><span class="sxs-lookup"><span data-stu-id="adcd2-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="adcd2-183">Максимально неудачных попыток доступа и время блокировки может быть задано с помощью [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) и [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="adcd2-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="adcd2-184">Следующие настраивает блокировки учетных записей для 10 минут после десяти неудачных попыток доступа:</span><span class="sxs-lookup"><span data-stu-id="adcd2-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="adcd2-185">Убедитесь, что [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) задает `lockoutOnFailure` для `true`:</span><span class="sxs-lookup"><span data-stu-id="adcd2-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
