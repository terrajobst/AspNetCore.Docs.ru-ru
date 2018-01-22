---
title: "Двухфакторная проверка подлинности с помощью SMS"
author: rick-anderson
description: "Показано, как настроить двухфакторную проверку подлинности (2FA) с ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/2fa
ms.openlocfilehash: 0970b7e95b116ceab1d17502d2f8aee9cd821715
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="fd411-103">Двухфакторная проверка подлинности с помощью SMS</span><span class="sxs-lookup"><span data-stu-id="fd411-103">Two-factor authentication with SMS</span></span>

<span data-ttu-id="fd411-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [разработчики швейцарский](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="fd411-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="fd411-105">Этот учебник относится только к ASP.NET Core только 1.x.</span><span class="sxs-lookup"><span data-stu-id="fd411-105">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="fd411-106">В разделе [Создание Включение QR-код для приложения для проверки подлинности в ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) Core ASP.NET 2.0 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="fd411-106">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="fd411-107">Этого учебника показано, как настроить двухфакторную проверку подлинности (2FA) с помощью SMS.</span><span class="sxs-lookup"><span data-stu-id="fd411-107">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="fd411-108">Инструкции, приведенные для [twilio](https://www.twilio.com/) и [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), но можно использовать любой другой поставщик SMS.</span><span class="sxs-lookup"><span data-stu-id="fd411-108">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="fd411-109">Рекомендуется [подтверждение учетной записи и пароль восстановления](accconfirm.md) перед запуском этого учебника.</span><span class="sxs-lookup"><span data-stu-id="fd411-109">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="fd411-110">Представление [полного примера](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="fd411-110">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="fd411-111">[Загрузка](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="fd411-111">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="fd411-112">Создайте новый проект ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd411-112">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="fd411-113">Создать новый веб-приложение ASP.NET Core с именем `Web2FA` для каждой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="fd411-113">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="fd411-114">Следуйте инструкциям в [реализации SSL в приложении ASP.NET Core](xref:security/enforcing-ssl) для настройки и требовать SSL.</span><span class="sxs-lookup"><span data-stu-id="fd411-114">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="fd411-115">Создание учетной записи SMS</span><span class="sxs-lookup"><span data-stu-id="fd411-115">Create an SMS account</span></span>

<span data-ttu-id="fd411-116">Создание учетной записи SMS, например, из [twilio](https://www.twilio.com/) или [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="fd411-116">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="fd411-117">Запишите учетные данные проверки подлинности (для twilio: accountSid и authToken для ASPSMS: использованием ключа пользователя и пароль).</span><span class="sxs-lookup"><span data-stu-id="fd411-117">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="fd411-118">Изучив учетные данные поставщика SMS</span><span class="sxs-lookup"><span data-stu-id="fd411-118">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="fd411-119">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="fd411-119">**Twilio:**</span></span>  
<span data-ttu-id="fd411-120">На вкладке панели мониторинга учетной записи Twilio, скопируйте **ИД безопасности учетной записи** и **маркер проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="fd411-120">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="fd411-121">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="fd411-121">**ASPSMS:**</span></span>  
<span data-ttu-id="fd411-122">Параметры учетной записи, перейдите к **использованием ключа пользователя** и скопируйте его вместе с вашей **пароль**.</span><span class="sxs-lookup"><span data-stu-id="fd411-122">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="fd411-123">Позже он будет храниться эти значения с помощью секрет диспетчера в ключи `SMSAccountIdentification` и `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="fd411-123">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="fd411-124">Указав идентификатор отправителя / инициатора</span><span class="sxs-lookup"><span data-stu-id="fd411-124">Specifying SenderID / Originator</span></span>

<span data-ttu-id="fd411-125">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="fd411-125">**Twilio:**</span></span>  
<span data-ttu-id="fd411-126">На вкладке номера скопируйте вашей Twilio **номер телефона**.</span><span class="sxs-lookup"><span data-stu-id="fd411-126">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="fd411-127">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="fd411-127">**ASPSMS:**</span></span>  
<span data-ttu-id="fd411-128">В меню разблокирования инициаторы разблокировать одного или нескольких источников или выберите инициатор буквенно-цифровых (поддерживаются не все сети).</span><span class="sxs-lookup"><span data-stu-id="fd411-128">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="fd411-129">Это значение с помощью средства диспетчера секрет раздел позже будет храниться `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="fd411-129">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="fd411-130">Укажите учетные данные для службы SMS</span><span class="sxs-lookup"><span data-stu-id="fd411-130">Provide credentials for the SMS service</span></span>

<span data-ttu-id="fd411-131">Мы будем использовать [параметры шаблона](xref:fundamentals/configuration/options) для доступа к параметрам учетной записи и ключа пользователя.</span><span class="sxs-lookup"><span data-stu-id="fd411-131">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="fd411-132">Создание класса для выборки защищенный ключ SMS.</span><span class="sxs-lookup"><span data-stu-id="fd411-132">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="fd411-133">Для этого образца `SMSoptions` класса создается в *Services/SMSoptions.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="fd411-133">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="fd411-134">Задать `SMSAccountIdentification`, `SMSAccountPassword` и `SMSAccountFrom` с [секрет диспетчера](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="fd411-134">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="fd411-135">Пример:</span><span class="sxs-lookup"><span data-stu-id="fd411-135">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="fd411-136">Добавьте пакет NuGet для поставщика SMS.</span><span class="sxs-lookup"><span data-stu-id="fd411-136">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="fd411-137">Из пакета Диспетчер консоли (PMC) запуска:</span><span class="sxs-lookup"><span data-stu-id="fd411-137">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="fd411-138">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="fd411-138">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="fd411-139">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="fd411-139">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="fd411-140">Добавьте код в *Services/MessageServices.cs* файл, чтобы включить SMS.</span><span class="sxs-lookup"><span data-stu-id="fd411-140">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="fd411-141">Используйте Twilio или разделе ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="fd411-141">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="fd411-142">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="fd411-142">**Twilio:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="fd411-143">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="fd411-143">**ASPSMS:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="fd411-144">Настройка запуска для использования`SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="fd411-144">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="fd411-145">Добавить `SMSoptions` в контейнер службы в `ConfigureServices` метод в *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="fd411-145">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="fd411-146">Включение двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="fd411-146">Enable two-factor authentication</span></span>

<span data-ttu-id="fd411-147">Откройте *Views/Manage/Index.cshtml* файл представления Razor и удалите символы комментариев (поэтому разметка не закомментированного).</span><span class="sxs-lookup"><span data-stu-id="fd411-147">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="fd411-148">Войдите с помощью двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="fd411-148">Log in with two-factor authentication</span></span>

* <span data-ttu-id="fd411-149">Запустите приложение и регистрации нового пользователя</span><span class="sxs-lookup"><span data-stu-id="fd411-149">Run the app and register a new user</span></span>

![Веб-приложение регистра, откройте представление в Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="fd411-151">Нажмите на имя пользователя, который активирует `Index` метод действия в контроллере управление.</span><span class="sxs-lookup"><span data-stu-id="fd411-151">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="fd411-152">Выберите номер телефона **добавить** ссылку.</span><span class="sxs-lookup"><span data-stu-id="fd411-152">Then tap the phone number **Add** link.</span></span>

![Управление представления](2fa/_static/login2fa2.png)

* <span data-ttu-id="fd411-154">Добавить номер телефона, получать код проверки и коснитесь **отправить код проверки**.</span><span class="sxs-lookup"><span data-stu-id="fd411-154">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Добавление номера страницы](2fa/_static/login2fa3.png)

* <span data-ttu-id="fd411-156">Вы получите SMS с кодом проверки.</span><span class="sxs-lookup"><span data-stu-id="fd411-156">You will get a text message with the verification code.</span></span> <span data-ttu-id="fd411-157">Введите его и нажмите кнопку **отправки**</span><span class="sxs-lookup"><span data-stu-id="fd411-157">Enter it and tap **Submit**</span></span>

![Проверить номер телефона страницу](2fa/_static/login2fa4.png)

<span data-ttu-id="fd411-159">Если не получен текстовое сообщение, посетите страницу twilio журнала.</span><span class="sxs-lookup"><span data-stu-id="fd411-159">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="fd411-160">Представления управление показывает, что ваш номер телефона успешно добавлен.</span><span class="sxs-lookup"><span data-stu-id="fd411-160">The Manage view shows your phone number was added successfully.</span></span>

![Управление представления](2fa/_static/login2fa5.png)

* <span data-ttu-id="fd411-162">Коснитесь **включить** Включение двухфакторной проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="fd411-162">Tap **Enable** to enable two-factor authentication.</span></span>

![Управление представления](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="fd411-164">Тестирование двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="fd411-164">Test two-factor authentication</span></span>

* <span data-ttu-id="fd411-165">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="fd411-165">Log off.</span></span>

* <span data-ttu-id="fd411-166">Войти.</span><span class="sxs-lookup"><span data-stu-id="fd411-166">Log in.</span></span>

* <span data-ttu-id="fd411-167">Учетная запись пользователя включена двухфакторной проверки подлинности, поэтому вы должны предоставить второй фактор проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="fd411-167">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="fd411-168">В этом учебнике вы включили Проверка телефона.</span><span class="sxs-lookup"><span data-stu-id="fd411-168">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="fd411-169">Встроенные шаблоны также можно настроить электронную почту в качестве второго фактора.</span><span class="sxs-lookup"><span data-stu-id="fd411-169">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="fd411-170">Можно настроить дополнительные факторы проверки подлинности, такие как QR-коды, второй.</span><span class="sxs-lookup"><span data-stu-id="fd411-170">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="fd411-171">Коснитесь **отправить**.</span><span class="sxs-lookup"><span data-stu-id="fd411-171">Tap **Submit**.</span></span>

![Отправить в представлении кода проверки](2fa/_static/login2fa7.png)

* <span data-ttu-id="fd411-173">Введите код, который вы получаете в SMS-сообщения.</span><span class="sxs-lookup"><span data-stu-id="fd411-173">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="fd411-174">Щелкнув **Запомнить этот браузер** флажок Исключить вас от необходимости использовать 2FA вход при использовании одного устройства и браузера.</span><span class="sxs-lookup"><span data-stu-id="fd411-174">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="fd411-175">Включение 2FA и щелкнув **Запомнить этот браузер** обеспечивают надежный 2FA защиты от злоумышленников, пытается получить доступ к вашей учетной записи, пока они не имеют доступа к устройству.</span><span class="sxs-lookup"><span data-stu-id="fd411-175">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="fd411-176">Это можно сделать на любом устройстве закрытого регулярно используемых.</span><span class="sxs-lookup"><span data-stu-id="fd411-176">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="fd411-177">Установив **Запомнить этот браузер**, получить дополнительную защиту 2FA с устройств, которые вы не используете регулярно и получения удобства на отсутствие необходимости проходить через 2FA на собственных устройствах.</span><span class="sxs-lookup"><span data-stu-id="fd411-177">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Проверить представление](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="fd411-179">Блокировки учетных записей для защиты от атак методом подбора</span><span class="sxs-lookup"><span data-stu-id="fd411-179">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="fd411-180">Мы рекомендуем использовать блокировки учетных записей с 2FA.</span><span class="sxs-lookup"><span data-stu-id="fd411-180">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="fd411-181">Как только пользователь вошел в систему (с помощью локальной учетной записи или учетной записи социальных сетей), сохранения каждого Неудачная попытка 2FA и если достигается максимальное попыток, (по умолчанию — 5), пользователь будет заблокирован на пять минут (можно задать блокировка времени с `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="fd411-181">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="fd411-182">Следующие настраивает учетную запись будет заблокирован на 10 минут после 10 неудачных попыток входа.</span><span class="sxs-lookup"><span data-stu-id="fd411-182">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
