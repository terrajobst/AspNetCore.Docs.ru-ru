---
title: Двухфакторная проверка подлинности с помощью SMS в ASP.NET Core
author: rick-anderson
description: Узнайте, как настроить двухфакторную проверку подлинности (2FA) с помощью приложения ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 6f20928b0dec9b235fa17c1b44c81a48d031e9e0
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121665"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="36b73-103">Двухфакторная проверка подлинности с помощью SMS в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="36b73-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="36b73-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [разработчики Швейцария](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="36b73-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

>[!WARNING]
> <span data-ttu-id="36b73-105">Два фактора проверки подлинности (2FA) проверки подлинности приложения с помощью по времени одноразовый пароль алгоритм (TOTP), являются отрасли, рекомендуемый подход для 2FA.</span><span class="sxs-lookup"><span data-stu-id="36b73-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="36b73-106">2FA с помощью TOTP предпочтительнее SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="36b73-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="36b73-107">Дополнительные сведения см. в разделе [создания включить QR-кода для приложений TOTP проверки подлинности в ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) ASP.NET Core 2.0 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="36b73-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="36b73-108">Этом руководстве показано, как настроить двухфакторную проверку подлинности (2FA) с помощью SMS.</span><span class="sxs-lookup"><span data-stu-id="36b73-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="36b73-109">Инструкции, приведенные для [twilio](https://www.twilio.com/) и [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), но вы можете использовать любой другой поставщик SMS.</span><span class="sxs-lookup"><span data-stu-id="36b73-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="36b73-110">Мы рекомендуем вам выполнить [подтверждение учетной записи и восстановление пароля](xref:security/authentication/accconfirm) этим руководством.</span><span class="sxs-lookup"><span data-stu-id="36b73-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="36b73-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="36b73-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="36b73-112">[Как скачать](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="36b73-112">[How to download](xref:index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="36b73-113">Создание проекта ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="36b73-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="36b73-114">Создание нового веб-приложения ASP.NET Core с именем `Web2FA` с учетными записями отдельных пользователей.</span><span class="sxs-lookup"><span data-stu-id="36b73-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="36b73-115">Следуйте инструкциям в [принудительно использовать SSL-в приложении ASP.NET Core](xref:security/enforcing-ssl) для настройки и обязательное использование SSL.</span><span class="sxs-lookup"><span data-stu-id="36b73-115">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="36b73-116">Создание учетной записи SMS</span><span class="sxs-lookup"><span data-stu-id="36b73-116">Create an SMS account</span></span>

<span data-ttu-id="36b73-117">Создание учетной записи SMS, например, из [twilio](https://www.twilio.com/) или [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="36b73-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="36b73-118">Запишите учетные данные проверки подлинности (для twilio: accountSid и authToken для ASPSMS: Userkey и пароль).</span><span class="sxs-lookup"><span data-stu-id="36b73-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="36b73-119">Выяснение учетные данные поставщика SMS</span><span class="sxs-lookup"><span data-stu-id="36b73-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="36b73-120">**Twilio:** вкладки панели мониторинга учетной записи Twilio, скопируйте **ИД безопасности учетной записи** и **маркер проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="36b73-120">**Twilio:** From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="36b73-121">**ASPSMS:** из параметров учетной записи, перейдите к **Userkey** и скопируйте его вместе с вашей **пароль**.</span><span class="sxs-lookup"><span data-stu-id="36b73-121">**ASPSMS:** From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="36b73-122">Позже мы сохраним эти значения с помощью диспетчера секретов, в ключи `SMSAccountIdentification` и `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="36b73-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="36b73-123">Указав идентификатор SenderID / инициатора</span><span class="sxs-lookup"><span data-stu-id="36b73-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="36b73-124">**Twilio:** на вкладке «числа» скопируйте twilio и значениями **номер телефона**.</span><span class="sxs-lookup"><span data-stu-id="36b73-124">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="36b73-125">**ASPSMS:** инициаторы меню разблокирования разблокировать один или несколько авторства или выберите инициатор буквенно-цифровых (не поддерживаются все сети).</span><span class="sxs-lookup"><span data-stu-id="36b73-125">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="36b73-126">Позже мы Сохраним это значение с помощью диспетчера секретов, раздел `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="36b73-126">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="36b73-127">Укажите учетные данные для службы SMS</span><span class="sxs-lookup"><span data-stu-id="36b73-127">Provide credentials for the SMS service</span></span>

<span data-ttu-id="36b73-128">Мы будем использовать [шаблон параметров](xref:fundamentals/configuration/options) для доступа к параметрам и ключ учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="36b73-128">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

   * <span data-ttu-id="36b73-129">Создание класса для извлечения ключа безопасности SMS.</span><span class="sxs-lookup"><span data-stu-id="36b73-129">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="36b73-130">В этом примере `SMSoptions` класс создается в *Services/SMSoptions.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="36b73-130">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="36b73-131">Задайте `SMSAccountIdentification`, `SMSAccountPassword` и `SMSAccountFrom` с [средство secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="36b73-131">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="36b73-132">Пример:</span><span class="sxs-lookup"><span data-stu-id="36b73-132">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="36b73-133">Добавьте пакет NuGet для поставщика SMS.</span><span class="sxs-lookup"><span data-stu-id="36b73-133">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="36b73-134">Из консоли диспетчера пакетов (PMC) выполните:</span><span class="sxs-lookup"><span data-stu-id="36b73-134">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="36b73-135">**Twilio:**
`Install-Package Twilio`</span><span class="sxs-lookup"><span data-stu-id="36b73-135">**Twilio:**
`Install-Package Twilio`</span></span>

<span data-ttu-id="36b73-136">**ASPSMS:**
`Install-Package ASPSMS`</span><span class="sxs-lookup"><span data-stu-id="36b73-136">**ASPSMS:**
`Install-Package ASPSMS`</span></span>


* <span data-ttu-id="36b73-137">Добавьте код в *Services/MessageServices.cs* файл, чтобы включить SMS.</span><span class="sxs-lookup"><span data-stu-id="36b73-137">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="36b73-138">Использование Twilio или разделе ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="36b73-138">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="36b73-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span><span class="sxs-lookup"><span data-stu-id="36b73-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span></span>

<span data-ttu-id="36b73-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span><span class="sxs-lookup"><span data-stu-id="36b73-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span></span>

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="36b73-141">Настройка запуска для использования `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="36b73-141">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="36b73-142">Добавить `SMSoptions` в контейнер службы в `ConfigureServices` метод в *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="36b73-142">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="36b73-143">Включение двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="36b73-143">Enable two-factor authentication</span></span>

<span data-ttu-id="36b73-144">Откройте *Views/Manage/Index.cshtml* файл представления Razor и удалите символы комментария (поэтому разметка не закомментированного).</span><span class="sxs-lookup"><span data-stu-id="36b73-144">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="36b73-145">Войдите с помощью двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="36b73-145">Log in with two-factor authentication</span></span>

* <span data-ttu-id="36b73-146">Запустите приложение и зарегистрируйте нового пользователя</span><span class="sxs-lookup"><span data-stu-id="36b73-146">Run the app and register a new user</span></span>

![Веб-приложение Register, откройте представление в Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="36b73-148">Выберите свое имя пользователя, который активирует `Index` метода действия в контроллере управление.</span><span class="sxs-lookup"><span data-stu-id="36b73-148">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="36b73-149">Выберите номер телефона **добавить** ссылку.</span><span class="sxs-lookup"><span data-stu-id="36b73-149">Then tap the phone number **Add** link.</span></span>

![Управление представления – нажать ссылку «Добавить»](2fa/_static/login2fa2.png)

* <span data-ttu-id="36b73-151">Добавить номер телефона, который будет получать код проверки и коснитесь **отправить проверочный код**.</span><span class="sxs-lookup"><span data-stu-id="36b73-151">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Добавить страницу номер телефона](2fa/_static/login2fa3.png)

* <span data-ttu-id="36b73-153">Вы получите текстовое сообщение с кодом проверки.</span><span class="sxs-lookup"><span data-stu-id="36b73-153">You will get a text message with the verification code.</span></span> <span data-ttu-id="36b73-154">Введите его и коснитесь **отправки**</span><span class="sxs-lookup"><span data-stu-id="36b73-154">Enter it and tap **Submit**</span></span>

![Проверить номер телефона страницу](2fa/_static/login2fa4.png)

<span data-ttu-id="36b73-156">Если вы не получили текстовое сообщение, см. в разделе страницы журнала twilio.</span><span class="sxs-lookup"><span data-stu-id="36b73-156">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="36b73-157">Представления управление показывает, что ваш номер телефона успешно добавлен.</span><span class="sxs-lookup"><span data-stu-id="36b73-157">The Manage view shows your phone number was added successfully.</span></span>

![Управление представление — номер телефона успешно добавлен](2fa/_static/login2fa5.png)

* <span data-ttu-id="36b73-159">Коснитесь **включить** включить двухфакторную проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="36b73-159">Tap **Enable** to enable two-factor authentication.</span></span>

![Управления представлением — Включение двухфакторной проверки подлинности](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="36b73-161">Тестирование двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="36b73-161">Test two-factor authentication</span></span>

* <span data-ttu-id="36b73-162">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="36b73-162">Log off.</span></span>

* <span data-ttu-id="36b73-163">Вход.</span><span class="sxs-lookup"><span data-stu-id="36b73-163">Log in.</span></span>

* <span data-ttu-id="36b73-164">Учетной записи пользователя включена двухфакторная проверка подлинности, поэтому вы должны предоставить второй фактор проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="36b73-164">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="36b73-165">В этом руководстве вы включили Проверка номера телефона.</span><span class="sxs-lookup"><span data-stu-id="36b73-165">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="36b73-166">Встроенные шаблоны позволяют настроить почту в качестве второго фактора.</span><span class="sxs-lookup"><span data-stu-id="36b73-166">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="36b73-167">Можно настроить дополнительные факторы проверки подлинности, например QR-коды, второй.</span><span class="sxs-lookup"><span data-stu-id="36b73-167">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="36b73-168">Коснитесь **отправить**.</span><span class="sxs-lookup"><span data-stu-id="36b73-168">Tap **Submit**.</span></span>

![Отправить код проверки представления](2fa/_static/login2fa7.png)

* <span data-ttu-id="36b73-170">Введите код, полученный в SMS-сообщения.</span><span class="sxs-lookup"><span data-stu-id="36b73-170">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="36b73-171">Щелкнув **запомнить браузер** флажок будет исключить от необходимости использовать 2FA для входа при использовании же устройства и браузера.</span><span class="sxs-lookup"><span data-stu-id="36b73-171">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="36b73-172">Включение 2FA и щелкнув **запомнить браузер** предоставит вам строгого 2FA защиты от злоумышленников, пытается получить доступ к вашей учетной записи, до тех пор, пока они не имеют доступа к устройству.</span><span class="sxs-lookup"><span data-stu-id="36b73-172">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="36b73-173">Это можно сделать на любом устройстве закрытого, которые регулярно использовать.</span><span class="sxs-lookup"><span data-stu-id="36b73-173">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="36b73-174">Установив **запомнить браузер**, вы получаете дополнительную защиту 2FA с устройств, вы не используете регулярно, и вы получаете удобства на не нужно изучать через 2FA на собственных устройствах.</span><span class="sxs-lookup"><span data-stu-id="36b73-174">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Проверить представление](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="36b73-176">Блокировка учетной записи, обеспечивающие защиту от атак методом подбора</span><span class="sxs-lookup"><span data-stu-id="36b73-176">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="36b73-177">Блокировка учетной записи рекомендуется с 2FA.</span><span class="sxs-lookup"><span data-stu-id="36b73-177">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="36b73-178">Когда пользователь войдет в приложение через локальную учетную запись или учетную запись социальной сети, каждая неудачная попытка 2FA хранится.</span><span class="sxs-lookup"><span data-stu-id="36b73-178">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="36b73-179">После достижения максимального неудачных попыток доступа пользователя блокируется (по умолчанию: 5-минутного блокировки после 5 неудачных попыток доступа).</span><span class="sxs-lookup"><span data-stu-id="36b73-179">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="36b73-180">Сбрасывает счетчик попыток неудачные попытки доступа успешной проверки подлинности и сбрасывает часы.</span><span class="sxs-lookup"><span data-stu-id="36b73-180">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="36b73-181">Максимально неудачных попыток доступа и время блокировки может быть задано с помощью [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) и [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="36b73-181">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="36b73-182">Следующие настраивает блокировки учетных записей для 10 минут после десяти неудачных попыток доступа:</span><span class="sxs-lookup"><span data-stu-id="36b73-182">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="36b73-183">Убедитесь, что [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) задает `lockoutOnFailure` для `true`:</span><span class="sxs-lookup"><span data-stu-id="36b73-183">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
