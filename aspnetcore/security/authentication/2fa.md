---
title: Двухфакторная проверка подлинности с помощью SMS в ASP.NET Core
author: rick-anderson
description: Узнайте, как настроить двухфакторную проверку подлинности (2FA) с приложением ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
uid: security/authentication/2fa
ms.openlocfilehash: 335edfd5cd4dfbb9d223ba0ae888a6d2386cd4a5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272313"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="72cd1-103">Двухфакторная проверка подлинности с помощью SMS в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72cd1-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="72cd1-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [разработчики швейцарский](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="72cd1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="72cd1-105">В разделе [Создание включить QR-код для приложения для проверки подлинности в ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) Core ASP.NET 2.0 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="72cd1-105">See [Enable QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="72cd1-106">Этого учебника показано, как настроить двухфакторную проверку подлинности (2FA) с помощью SMS.</span><span class="sxs-lookup"><span data-stu-id="72cd1-106">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="72cd1-107">Инструкции, приведенные для [twilio](https://www.twilio.com/) и [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), но можно использовать любой другой поставщик SMS.</span><span class="sxs-lookup"><span data-stu-id="72cd1-107">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="72cd1-108">Рекомендуется [подтверждение учетной записи и пароль восстановления](xref:security/authentication/accconfirm) перед запуском этого учебника.</span><span class="sxs-lookup"><span data-stu-id="72cd1-108">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="72cd1-109">Представление [полного примера](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="72cd1-109">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="72cd1-110">[Загрузка](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="72cd1-110">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="72cd1-111">Создайте новый проект ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72cd1-111">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="72cd1-112">Создать новый веб-приложение ASP.NET Core с именем `Web2FA` для каждой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="72cd1-112">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="72cd1-113">Следуйте инструкциям в [применять SSL в приложении ASP.NET Core](xref:security/enforcing-ssl) для настройки и требовать SSL.</span><span class="sxs-lookup"><span data-stu-id="72cd1-113">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="72cd1-114">Создание учетной записи SMS</span><span class="sxs-lookup"><span data-stu-id="72cd1-114">Create an SMS account</span></span>

<span data-ttu-id="72cd1-115">Создание учетной записи SMS, например, из [twilio](https://www.twilio.com/) или [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="72cd1-115">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="72cd1-116">Запишите учетные данные проверки подлинности (для twilio: accountSid и authToken для ASPSMS: использованием ключа пользователя и пароль).</span><span class="sxs-lookup"><span data-stu-id="72cd1-116">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="72cd1-117">Изучив учетные данные поставщика SMS</span><span class="sxs-lookup"><span data-stu-id="72cd1-117">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="72cd1-118">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="72cd1-118">**Twilio:**</span></span>  
<span data-ttu-id="72cd1-119">На вкладке панели мониторинга учетной записи Twilio, скопируйте **ИД безопасности учетной записи** и **маркер проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="72cd1-119">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="72cd1-120">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="72cd1-120">**ASPSMS:**</span></span>  
<span data-ttu-id="72cd1-121">Параметры учетной записи, перейдите к **использованием ключа пользователя** и скопируйте его вместе с вашей **пароль**.</span><span class="sxs-lookup"><span data-stu-id="72cd1-121">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="72cd1-122">Позже он будет храниться эти значения с помощью секрет диспетчера в ключи `SMSAccountIdentification` и `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="72cd1-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="72cd1-123">Указав идентификатор отправителя / инициатора</span><span class="sxs-lookup"><span data-stu-id="72cd1-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="72cd1-124">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="72cd1-124">**Twilio:**</span></span>  
<span data-ttu-id="72cd1-125">На вкладке номера скопируйте вашей Twilio **номер телефона**.</span><span class="sxs-lookup"><span data-stu-id="72cd1-125">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="72cd1-126">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="72cd1-126">**ASPSMS:**</span></span>  
<span data-ttu-id="72cd1-127">В меню разблокирования инициаторы разблокировать одного или нескольких источников или выберите инициатор буквенно-цифровых (поддерживаются не все сети).</span><span class="sxs-lookup"><span data-stu-id="72cd1-127">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="72cd1-128">Это значение с помощью средства диспетчера секрет раздел позже будет храниться `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="72cd1-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="72cd1-129">Укажите учетные данные для службы SMS</span><span class="sxs-lookup"><span data-stu-id="72cd1-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="72cd1-130">Мы будем использовать [параметры шаблона](xref:fundamentals/configuration/options) для доступа к параметрам учетной записи и ключа пользователя.</span><span class="sxs-lookup"><span data-stu-id="72cd1-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="72cd1-131">Создание класса для выборки защищенный ключ SMS.</span><span class="sxs-lookup"><span data-stu-id="72cd1-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="72cd1-132">Для этого образца `SMSoptions` класса создается в *Services/SMSoptions.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="72cd1-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="72cd1-133">Задать `SMSAccountIdentification`, `SMSAccountPassword` и `SMSAccountFrom` с [секрет диспетчера](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="72cd1-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="72cd1-134">Пример:</span><span class="sxs-lookup"><span data-stu-id="72cd1-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="72cd1-135">Добавьте пакет NuGet для поставщика SMS.</span><span class="sxs-lookup"><span data-stu-id="72cd1-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="72cd1-136">Из пакета Диспетчер консоли (PMC) запуска:</span><span class="sxs-lookup"><span data-stu-id="72cd1-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="72cd1-137">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="72cd1-137">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="72cd1-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="72cd1-138">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="72cd1-139">Добавьте код в *Services/MessageServices.cs* файл, чтобы включить SMS.</span><span class="sxs-lookup"><span data-stu-id="72cd1-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="72cd1-140">Используйте Twilio или разделе ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="72cd1-140">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="72cd1-141">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="72cd1-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="72cd1-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="72cd1-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="72cd1-143">Настройка запуска для использования `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="72cd1-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="72cd1-144">Добавить `SMSoptions` в контейнер службы в `ConfigureServices` метод в *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="72cd1-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="72cd1-145">Включение двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="72cd1-145">Enable two-factor authentication</span></span>

<span data-ttu-id="72cd1-146">Откройте *Views/Manage/Index.cshtml* файл представления Razor и удалите символы комментариев (поэтому разметка не закомментированного).</span><span class="sxs-lookup"><span data-stu-id="72cd1-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="72cd1-147">Войдите с помощью двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="72cd1-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="72cd1-148">Запустите приложение и регистрации нового пользователя</span><span class="sxs-lookup"><span data-stu-id="72cd1-148">Run the app and register a new user</span></span>

![Веб-приложение регистра, откройте представление в Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="72cd1-150">Нажмите на имя пользователя, который активирует `Index` метод действия в контроллере управление.</span><span class="sxs-lookup"><span data-stu-id="72cd1-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="72cd1-151">Выберите номер телефона **добавить** ссылку.</span><span class="sxs-lookup"><span data-stu-id="72cd1-151">Then tap the phone number **Add** link.</span></span>

![Управление представления](2fa/_static/login2fa2.png)

* <span data-ttu-id="72cd1-153">Добавить номер телефона, получать код проверки и коснитесь **отправить код проверки**.</span><span class="sxs-lookup"><span data-stu-id="72cd1-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Добавление номера страницы](2fa/_static/login2fa3.png)

* <span data-ttu-id="72cd1-155">Вы получите SMS с кодом проверки.</span><span class="sxs-lookup"><span data-stu-id="72cd1-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="72cd1-156">Введите его и нажмите кнопку **отправки**</span><span class="sxs-lookup"><span data-stu-id="72cd1-156">Enter it and tap **Submit**</span></span>

![Проверить номер телефона страницу](2fa/_static/login2fa4.png)

<span data-ttu-id="72cd1-158">Если не получен текстовое сообщение, посетите страницу twilio журнала.</span><span class="sxs-lookup"><span data-stu-id="72cd1-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="72cd1-159">Представления управление показывает, что ваш номер телефона успешно добавлен.</span><span class="sxs-lookup"><span data-stu-id="72cd1-159">The Manage view shows your phone number was added successfully.</span></span>

![Управление представления](2fa/_static/login2fa5.png)

* <span data-ttu-id="72cd1-161">Коснитесь **включить** Включение двухфакторной проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="72cd1-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Управление представления](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="72cd1-163">Тестирование двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="72cd1-163">Test two-factor authentication</span></span>

* <span data-ttu-id="72cd1-164">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="72cd1-164">Log off.</span></span>

* <span data-ttu-id="72cd1-165">Вход.</span><span class="sxs-lookup"><span data-stu-id="72cd1-165">Log in.</span></span>

* <span data-ttu-id="72cd1-166">Учетная запись пользователя включена двухфакторной проверки подлинности, поэтому вы должны предоставить второй фактор проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="72cd1-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="72cd1-167">В этом учебнике вы включили Проверка телефона.</span><span class="sxs-lookup"><span data-stu-id="72cd1-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="72cd1-168">Встроенные шаблоны также можно настроить электронную почту в качестве второго фактора.</span><span class="sxs-lookup"><span data-stu-id="72cd1-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="72cd1-169">Можно настроить дополнительные факторы проверки подлинности, такие как QR-коды, второй.</span><span class="sxs-lookup"><span data-stu-id="72cd1-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="72cd1-170">Коснитесь **отправить**.</span><span class="sxs-lookup"><span data-stu-id="72cd1-170">Tap **Submit**.</span></span>

![Отправить в представлении кода проверки](2fa/_static/login2fa7.png)

* <span data-ttu-id="72cd1-172">Введите код, который вы получаете в SMS-сообщения.</span><span class="sxs-lookup"><span data-stu-id="72cd1-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="72cd1-173">Щелкнув **Запомнить этот браузер** флажок Исключить вас от необходимости использовать 2FA вход при использовании одного устройства и браузера.</span><span class="sxs-lookup"><span data-stu-id="72cd1-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="72cd1-174">Включение 2FA и щелкнув **Запомнить этот браузер** обеспечивают надежный 2FA защиты от злоумышленников, пытается получить доступ к вашей учетной записи, пока они не имеют доступа к устройству.</span><span class="sxs-lookup"><span data-stu-id="72cd1-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="72cd1-175">Это можно сделать на любом устройстве закрытого регулярно используемых.</span><span class="sxs-lookup"><span data-stu-id="72cd1-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="72cd1-176">Установив **Запомнить этот браузер**, получить дополнительную защиту 2FA с устройств, которые вы не используете регулярно и получения удобства на отсутствие необходимости проходить через 2FA на собственных устройствах.</span><span class="sxs-lookup"><span data-stu-id="72cd1-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Проверить представление](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="72cd1-178">Блокировки учетных записей для защиты от атак методом подбора</span><span class="sxs-lookup"><span data-stu-id="72cd1-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="72cd1-179">С 2FA рекомендуется блокировки учетной записи.</span><span class="sxs-lookup"><span data-stu-id="72cd1-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="72cd1-180">Когда пользователь входит через локальной учетной записи или учетной записи социальных сетей, каждого Неудачная попытка 2FA сохраняется.</span><span class="sxs-lookup"><span data-stu-id="72cd1-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="72cd1-181">При достижении максимального неудачных попыток доступа пользователя блокируется (по умолчанию: 5-минутного блокировки после 5 неудачных попыток доступа).</span><span class="sxs-lookup"><span data-stu-id="72cd1-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="72cd1-182">Сбрасывает счетчик неудачных доступов попыток прошли проверку подлинности и сбрасывает часы.</span><span class="sxs-lookup"><span data-stu-id="72cd1-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="72cd1-183">Максимально неудачных попыток доступа и время блокировки может быть задано с [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) и [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="72cd1-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="72cd1-184">Следующие настраивает блокировки учетных записей на 10 минут после 10 неудачных попыток доступа:</span><span class="sxs-lookup"><span data-stu-id="72cd1-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="72cd1-185">Убедитесь, что [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) задает `lockoutOnFailure` для `true`:</span><span class="sxs-lookup"><span data-stu-id="72cd1-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
