---
title: "Двухфакторная проверка подлинности с помощью SMS"
author: rick-anderson
description: "Показано, как настроить двухфакторную проверку подлинности (2FA) с ASP.NET Core"
keywords: "ASP.NET Core, SMS, проверку подлинности, 2FA, двухфакторной проверки подлинности, двухфакторной проверки подлинности"
ms.author: riande
manager: wpickett
ms.date: 8/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/2fa
ms.openlocfilehash: 77404de2f367cb12ba25433899198b69f9e5a7f2
ms.sourcegitcommit: 9a22c64759a7285ba788a37039bea5fe95f45f21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/15/2017
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="7975b-104">Двухфакторная проверка подлинности с помощью SMS</span><span class="sxs-lookup"><span data-stu-id="7975b-104">Two-factor authentication with SMS</span></span>

<span data-ttu-id="7975b-105">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [разработчики швейцарский](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="7975b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="7975b-106">Этот учебник относится только к ASP.NET Core только 1.x.</span><span class="sxs-lookup"><span data-stu-id="7975b-106">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="7975b-107">В разделе [Создание Включение QR-код для приложения для проверки подлинности в ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) Core ASP.NET 2.0 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="7975b-107">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="7975b-108">Этого учебника показано, как настроить двухфакторную проверку подлинности (2FA) с помощью SMS.</span><span class="sxs-lookup"><span data-stu-id="7975b-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="7975b-109">Инструкции, приведенные для [twilio](https://www.twilio.com/) и [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), но можно использовать любой другой поставщик SMS.</span><span class="sxs-lookup"><span data-stu-id="7975b-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="7975b-110">Рекомендуется [подтверждение учетной записи и пароль восстановления](accconfirm.md) перед запуском этого учебника.</span><span class="sxs-lookup"><span data-stu-id="7975b-110">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="7975b-111">Представление [полного примера](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="7975b-111">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="7975b-112">[Загрузка](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="7975b-112">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="7975b-113">Создайте новый проект ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7975b-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="7975b-114">Создать новый веб-приложение ASP.NET Core с именем `Web2FA` для каждой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="7975b-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="7975b-115">Следуйте инструкциям в [реализации SSL в приложении ASP.NET Core](xref:security/enforcing-ssl) для настройки и требовать SSL.</span><span class="sxs-lookup"><span data-stu-id="7975b-115">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="7975b-116">Создание учетной записи SMS</span><span class="sxs-lookup"><span data-stu-id="7975b-116">Create an SMS account</span></span>

<span data-ttu-id="7975b-117">Создание учетной записи SMS, например, из [twilio](https://www.twilio.com/) или [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="7975b-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="7975b-118">Запишите учетные данные проверки подлинности (для twilio: accountSid и authToken для ASPSMS: использованием ключа пользователя и пароль).</span><span class="sxs-lookup"><span data-stu-id="7975b-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="7975b-119">Изучив учетные данные поставщика SMS</span><span class="sxs-lookup"><span data-stu-id="7975b-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="7975b-120">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="7975b-120">**Twilio:**</span></span>  
<span data-ttu-id="7975b-121">На вкладке панели мониторинга учетной записи Twilio, скопируйте **ИД безопасности учетной записи** и **маркер проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="7975b-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="7975b-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="7975b-122">**ASPSMS:**</span></span>  
<span data-ttu-id="7975b-123">Параметры учетной записи, перейдите к **использованием ключа пользователя** и скопируйте его вместе с вашей **пароль**.</span><span class="sxs-lookup"><span data-stu-id="7975b-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="7975b-124">Позже он будет храниться эти значения с помощью секрет диспетчера в ключи `SMSAccountIdentification` и `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="7975b-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="7975b-125">Указав идентификатор отправителя / инициатора</span><span class="sxs-lookup"><span data-stu-id="7975b-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="7975b-126">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="7975b-126">**Twilio:**</span></span>  
<span data-ttu-id="7975b-127">На вкладке номера скопируйте вашей Twilio **номер телефона**.</span><span class="sxs-lookup"><span data-stu-id="7975b-127">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="7975b-128">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="7975b-128">**ASPSMS:**</span></span>  
<span data-ttu-id="7975b-129">В меню разблокирования инициаторы разблокировать одного или нескольких источников или выберите инициатор буквенно-цифровых (поддерживаются не все сети).</span><span class="sxs-lookup"><span data-stu-id="7975b-129">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="7975b-130">Это значение с помощью средства диспетчера секрет раздел позже будет храниться `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="7975b-130">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="7975b-131">Укажите учетные данные для службы SMS</span><span class="sxs-lookup"><span data-stu-id="7975b-131">Provide credentials for the SMS service</span></span>

<span data-ttu-id="7975b-132">Мы будем использовать [параметры шаблона](xref:fundamentals/configuration#options-config-objects) для доступа к параметрам учетной записи и ключа пользователя.</span><span class="sxs-lookup"><span data-stu-id="7975b-132">We'll use the [Options pattern](xref:fundamentals/configuration#options-config-objects) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="7975b-133">Создание класса для выборки защищенный ключ SMS.</span><span class="sxs-lookup"><span data-stu-id="7975b-133">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="7975b-134">Для этого образца `SMSoptions` класса создается в *Services/SMSoptions.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="7975b-134">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

<span data-ttu-id="7975b-135">[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="7975b-135">[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]</span></span>

<span data-ttu-id="7975b-136">Задать `SMSAccountIdentification`, `SMSAccountPassword` и `SMSAccountFrom` с [секрет диспетчера](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7975b-136">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7975b-137">Пример:</span><span class="sxs-lookup"><span data-stu-id="7975b-137">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="7975b-138">Добавьте пакет NuGet для поставщика SMS.</span><span class="sxs-lookup"><span data-stu-id="7975b-138">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="7975b-139">Из пакета Диспетчер консоли (PMC) запуска:</span><span class="sxs-lookup"><span data-stu-id="7975b-139">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="7975b-140">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="7975b-140">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="7975b-141">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="7975b-141">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="7975b-142">Добавьте код в *Services/MessageServices.cs* файл, чтобы включить SMS.</span><span class="sxs-lookup"><span data-stu-id="7975b-142">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="7975b-143">Используйте Twilio или разделе ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="7975b-143">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="7975b-144">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="7975b-144">**Twilio:**</span></span>  
<span data-ttu-id="7975b-145">[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span><span class="sxs-lookup"><span data-stu-id="7975b-145">[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span></span>

<span data-ttu-id="7975b-146">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="7975b-146">**ASPSMS:**</span></span>  
<span data-ttu-id="7975b-147">[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span><span class="sxs-lookup"><span data-stu-id="7975b-147">[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span></span>

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="7975b-148">Настройка запуска для использования`SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="7975b-148">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="7975b-149">Добавить `SMSoptions` в контейнер службы в `ConfigureServices` метод в *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7975b-149">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

<span data-ttu-id="7975b-150">[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="7975b-150">[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]</span></span>

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="7975b-151">Включение двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="7975b-151">Enable two-factor authentication</span></span>

<span data-ttu-id="7975b-152">Откройте *Views/Manage/Index.cshtml* файл представления Razor и удалите символы комментариев (поэтому разметка не закомментированного).</span><span class="sxs-lookup"><span data-stu-id="7975b-152">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="7975b-153">Войдите с помощью двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="7975b-153">Log in with two-factor authentication</span></span>

* <span data-ttu-id="7975b-154">Запустите приложение и регистрации нового пользователя</span><span class="sxs-lookup"><span data-stu-id="7975b-154">Run the app and register a new user</span></span>

![Веб-приложение регистра, откройте представление в Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="7975b-156">Нажмите на имя пользователя, который активирует `Index` метод действия в контроллере управление.</span><span class="sxs-lookup"><span data-stu-id="7975b-156">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="7975b-157">Выберите номер телефона **добавить** ссылку.</span><span class="sxs-lookup"><span data-stu-id="7975b-157">Then tap the phone number **Add** link.</span></span>

![Управление представления](2fa/_static/login2fa2.png)

* <span data-ttu-id="7975b-159">Добавить номер телефона, получать код проверки и коснитесь **отправить код проверки**.</span><span class="sxs-lookup"><span data-stu-id="7975b-159">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Добавление номера страницы](2fa/_static/login2fa3.png)

* <span data-ttu-id="7975b-161">Вы получите SMS с кодом проверки.</span><span class="sxs-lookup"><span data-stu-id="7975b-161">You will get a text message with the verification code.</span></span> <span data-ttu-id="7975b-162">Введите его и нажмите кнопку **отправки**</span><span class="sxs-lookup"><span data-stu-id="7975b-162">Enter it and tap **Submit**</span></span>

![Проверить номер телефона страницу](2fa/_static/login2fa4.png)

<span data-ttu-id="7975b-164">Если не получен текстовое сообщение, посетите страницу twilio журнала.</span><span class="sxs-lookup"><span data-stu-id="7975b-164">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="7975b-165">Представления управление показывает, что ваш номер телефона успешно добавлен.</span><span class="sxs-lookup"><span data-stu-id="7975b-165">The Manage view shows your phone number was added successfully.</span></span>

![Управление представления](2fa/_static/login2fa5.png)

* <span data-ttu-id="7975b-167">Коснитесь **включить** Включение двухфакторной проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="7975b-167">Tap **Enable** to enable two-factor authentication.</span></span>

![Управление представления](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="7975b-169">Тестирование двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="7975b-169">Test two-factor authentication</span></span>

* <span data-ttu-id="7975b-170">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="7975b-170">Log off.</span></span>

* <span data-ttu-id="7975b-171">Войти.</span><span class="sxs-lookup"><span data-stu-id="7975b-171">Log in.</span></span>

* <span data-ttu-id="7975b-172">Учетная запись пользователя включена двухфакторной проверки подлинности, поэтому вы должны предоставить второй фактор проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="7975b-172">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="7975b-173">В этом учебнике вы включили Проверка телефона.</span><span class="sxs-lookup"><span data-stu-id="7975b-173">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="7975b-174">Встроенные шаблоны также можно настроить электронную почту в качестве второго фактора.</span><span class="sxs-lookup"><span data-stu-id="7975b-174">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="7975b-175">Можно настроить дополнительные факторы проверки подлинности, такие как QR-коды, второй.</span><span class="sxs-lookup"><span data-stu-id="7975b-175">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="7975b-176">Коснитесь **отправить**.</span><span class="sxs-lookup"><span data-stu-id="7975b-176">Tap **Submit**.</span></span>

![Отправить в представлении кода проверки](2fa/_static/login2fa7.png)

* <span data-ttu-id="7975b-178">Введите код, который вы получаете в SMS-сообщения.</span><span class="sxs-lookup"><span data-stu-id="7975b-178">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="7975b-179">Щелкнув **Запомнить этот браузер** флажок Исключить вас от необходимости использовать 2FA вход при использовании одного устройства и браузера.</span><span class="sxs-lookup"><span data-stu-id="7975b-179">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="7975b-180">Включение 2FA и щелкнув **Запомнить этот браузер** обеспечивают надежный 2FA защиты от злоумышленников, пытается получить доступ к вашей учетной записи, пока они не имеют доступа к устройству.</span><span class="sxs-lookup"><span data-stu-id="7975b-180">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="7975b-181">Это можно сделать на любом устройстве закрытого регулярно используемых.</span><span class="sxs-lookup"><span data-stu-id="7975b-181">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="7975b-182">Установив **Запомнить этот браузер**, получить дополнительную защиту 2FA с устройств, которые вы не используете регулярно и получения удобства на отсутствие необходимости проходить через 2FA на собственных устройствах.</span><span class="sxs-lookup"><span data-stu-id="7975b-182">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Проверить представление](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="7975b-184">Блокировки учетных записей для защиты от атак методом подбора</span><span class="sxs-lookup"><span data-stu-id="7975b-184">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="7975b-185">Мы рекомендуем использовать блокировки учетных записей с 2FA.</span><span class="sxs-lookup"><span data-stu-id="7975b-185">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="7975b-186">Как только пользователь вошел в систему (с помощью локальной учетной записи или учетной записи социальных сетей), сохранения каждого Неудачная попытка 2FA и если достигается максимальное попыток, (по умолчанию — 5), пользователь будет заблокирован на пять минут (можно задать блокировка времени с `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="7975b-186">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="7975b-187">Следующие настраивает учетную запись будет заблокирован на 10 минут после 10 неудачных попыток входа.</span><span class="sxs-lookup"><span data-stu-id="7975b-187">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

<span data-ttu-id="7975b-188">[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]</span><span class="sxs-lookup"><span data-stu-id="7975b-188">[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]</span></span> 
