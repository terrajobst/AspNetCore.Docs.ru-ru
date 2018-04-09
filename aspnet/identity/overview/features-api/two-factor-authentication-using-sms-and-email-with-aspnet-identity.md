---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Двухфакторная проверка подлинности с помощью SMS и электронной почты с ASP.NET Identity | Документы Microsoft
author: HaoK
description: Этого учебника показано, как настроить двухфакторную проверку подлинности (2FA) с помощью SMS и адрес электронной почты. В этой статье было написано с Рик Андерсон ( @RickAndMSFT ), счетам...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c8f628d177004a8569dde2651469ed591e48591e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="9c0d5-104">Двухфакторная проверка подлинности с помощью SMS и электронной почты с ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="9c0d5-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="9c0d5-105">по [поздравить Hao](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Рик Андерсон](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="9c0d5-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="9c0d5-106">Этого учебника показано, как настроить двухфакторную проверку подлинности (2FA) с помощью SMS и адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="9c0d5-107">В этой статье было написано с Рик Андерсон ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), поздравить Hao и Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="9c0d5-108">NuGet образец был написан главным образом Hao поздравить.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="9c0d5-109">В этом разделе объясняется следующее.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-109">This topic covers the following:</span></span>

- [<span data-ttu-id="9c0d5-110">Построение образца удостоверений</span><span class="sxs-lookup"><span data-stu-id="9c0d5-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="9c0d5-111">Настройка SMS для двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9c0d5-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="9c0d5-112">Включение двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9c0d5-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="9c0d5-113">Как зарегистрировать поставщик двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9c0d5-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="9c0d5-114">Объединение социальных сетей и локальных учетных записей</span><span class="sxs-lookup"><span data-stu-id="9c0d5-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="9c0d5-115">Блокировка учетной записи, от атак методом подбора</span><span class="sxs-lookup"><span data-stu-id="9c0d5-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="9c0d5-116">Построение образца удостоверений</span><span class="sxs-lookup"><span data-stu-id="9c0d5-116">Building the Identity sample</span></span>

<span data-ttu-id="9c0d5-117">В этом разделе будет использоваться NuGet для загрузки образца, который мы работаем над.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="9c0d5-118">Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="9c0d5-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="9c0d5-119">Установка Visual Studio [2013 обновление 2](https://go.microsoft.com/fwlink/?LinkId=390521) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="9c0d5-120">Предупреждение: Необходимо установить Visual Studio [2013 обновление 2](https://go.microsoft.com/fwlink/?LinkId=390521) для завершения этого учебника.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="9c0d5-121">Создайте новый ***пустой*** веб-проекте ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="9c0d5-122">В консоли диспетчера пакетов введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   <span data-ttu-id="9c0d5-123">В этом учебнике мы будем использовать [SendGrid](http://sendgrid.com/) для отправки электронной почты и [Twilio](https://www.twilio.com/) или [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) для sms-сообщения.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="9c0d5-124">`Identity.Samples` Пакет устанавливает код, мы будем работать с.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="9c0d5-125">Задать [проекта для использования SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="9c0d5-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="9c0d5-126">*Необязательный*: следуйте инструкциям в моей [учебник по электронной почте подтверждение](account-confirmation-and-password-recovery-with-aspnet-identity.md) подключить SendGrid и запустить приложение и зарегистрировать учетную запись электронной почты.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="9c0d5-127">* Необязательно: * удалите Демонстрация по электронной почте ссылку подтверждения код из примера ( `ViewBag.Link` кода в контроллера учетных записей.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="9c0d5-128">В разделе `DisplayEmail` и `ForgotPasswordConfirmation` методы действий и представления razor).</span><span class="sxs-lookup"><span data-stu-id="9c0d5-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="9c0d5-129"><em>Необязательно: * удалите `ViewBag.Status` код из контроллеров учетной записи и управление ими и *Views\Account\VerifyCode.cshtml</em> и <em>Views\Manage\VerifyPhoneNumber.cshtml</em> представлений razor.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-129"><em>Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml</em> and <em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor views.</span></span> <span data-ttu-id="9c0d5-130">Кроме того, можно сохранить `ViewBag.Status` отображение, чтобы проверить, как работает это приложение локально, без необходимости подключать и отправки электронной почты и SMS-сообщений.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="9c0d5-131">Предупреждение: При изменении параметров безопасности в этом образце производства приложения потребуется проходить аудит безопасности, которая явно вызывает изменения.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="9c0d5-132">Настройка SMS для двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9c0d5-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="9c0d5-133">Этот учебник содержит инструкции по использованию Twilio или ASPSMS, но можно использовать любой другой поставщик SMS.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="9c0d5-134">**Создание учетной записи пользователя с помощью поставщика SMS**</span><span class="sxs-lookup"><span data-stu-id="9c0d5-134">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="9c0d5-135">Создание [Twilio](https://www.twilio.com/try-twilio) или [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) учетной записи.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="9c0d5-136">**Установка дополнительных пакетов или Добавление ссылки на службу**</span><span class="sxs-lookup"><span data-stu-id="9c0d5-136">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="9c0d5-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-137">Twilio:</span></span>  
   <span data-ttu-id="9c0d5-138">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="9c0d5-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-139">ASPSMS:</span></span>  
   <span data-ttu-id="9c0d5-140">Следующие ссылки на службу необходимо добавить:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   <span data-ttu-id="9c0d5-141">Адрес:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="9c0d5-142">Пространство имен:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="9c0d5-143">**Изучив учетные данные пользователя поставщика SMS**</span><span class="sxs-lookup"><span data-stu-id="9c0d5-143">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="9c0d5-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-144">Twilio:</span></span>  
   <span data-ttu-id="9c0d5-145">Из **мониторинга** вкладка учетной записи Twilio, Копировать **ИД безопасности учетной записи** и **маркер проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="9c0d5-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-146">ASPSMS:</span></span>  
   <span data-ttu-id="9c0d5-147">Параметры учетной записи, перейдите к **использованием ключа пользователя** и скопируйте его вместе с самостоятельно определенных **пароль**.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="9c0d5-148">Мы позже хранит эти значения в переменных `SMSAccountIdentification` и `SMSAccountPassword` .</span><span class="sxs-lookup"><span data-stu-id="9c0d5-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="9c0d5-149">**Указав идентификатор отправителя / инициатора**</span><span class="sxs-lookup"><span data-stu-id="9c0d5-149">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="9c0d5-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-150">Twilio:</span></span>  
   <span data-ttu-id="9c0d5-151">Из **номера** вкладки, скопируйте Twilio номер телефона.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="9c0d5-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-152">ASPSMS:</span></span>  
   <span data-ttu-id="9c0d5-153">В пределах **разблокировать инициаторы** меню разблокирования одного или нескольких источников или выберите инициатор буквенно-цифровых (поддерживаются не все сети).</span><span class="sxs-lookup"><span data-stu-id="9c0d5-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="9c0d5-154">Это значение будет позже хранятся в переменной `SMSAccountFrom` .</span><span class="sxs-lookup"><span data-stu-id="9c0d5-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="9c0d5-155">**Передача учетных данных поставщика SMS в приложение**</span><span class="sxs-lookup"><span data-stu-id="9c0d5-155">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="9c0d5-156">Предоставьте доступ к приложению учетные данные и номер телефона отправителя.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="9c0d5-157">Безопасность — никогда не конфиденциальных данных в хранилище в исходном коде.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="9c0d5-158">Учетная запись и учетные данные добавляются выше, чтобы не усложнять в образце кода.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="9c0d5-159">. В разделе Jon Atten [ASP.NET MVC: сохранить частные параметры из системы управления версиями](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c0d5-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="9c0d5-160">**Реализация передачи данных для поставщика SMS**</span><span class="sxs-lookup"><span data-stu-id="9c0d5-160">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="9c0d5-161">Настройка `SmsService` класса в *приложения\_Start\IdentityConfig.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="9c0d5-162">В зависимости от используемых поставщика SMS активировать **Twilio** или **ASPSMS** раздела:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="9c0d5-163">Запустите приложение и войти с использованием учетной записи, которую вы ранее зарегистрированного.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="9c0d5-164">Щелкните идентификатор пользователя, который активирует `Index` метода действия в `Manage` контроллера.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="9c0d5-165">Нажмите кнопку Добавить.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="9c0d5-166">Через несколько секунд вы получите SMS с кодом проверки.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="9c0d5-167">Введите его и нажмите клавишу **отправить**.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="9c0d5-168">Представления управление показывает, что был добавлен в ваш номер телефона.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="9c0d5-169">Анализ кода</span><span class="sxs-lookup"><span data-stu-id="9c0d5-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="9c0d5-170">`Index` Метода действия в `Manage` задает сообщение о состоянии в зависимости от предыдущего действия контроллера, а также ссылки на добавить локальную учетную запись или изменить локальный пароль.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="9c0d5-171">`Index` Метод также отображает состояние или вашей 2FA телефонный номер, внешних имен входа, 2FA включен и запоминать, метод 2FA для этого браузера (рассматривается позже).</span><span class="sxs-lookup"><span data-stu-id="9c0d5-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="9c0d5-172">Щелкнув код пользователя (электронной почты) в строке заголовка не передает сообщение.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="9c0d5-173">Щелкнув **номер телефона: удалите** связать передает `Message=RemovePhoneSuccess` как строка запроса.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="9c0d5-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="9c0d5-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="9c0d5-175">`AddPhoneNumber` Метод действия отображает диалоговое окно, введите номер телефона, который может получить SMS-сообщений.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="9c0d5-176">Щелкнув **отправить код проверки** кнопка инициирует передачу номер телефона для HTTP POST `AddPhoneNumber` метода действия.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="9c0d5-177">`GenerateChangePhoneNumberTokenAsync` Метод создает маркер безопасности, в которой будут устанавливаться в SMS-сообщения.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="9c0d5-178">Если служба SMS была настроена, маркер отправляется в качестве строки &quot;ваш код безопасности &lt;маркера&gt;&quot;.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="9c0d5-179">`SmsService.SendAsync` Метод вызывается асинхронно, а затем перенаправляется приложение `VerifyPhoneNumber` метода действия (в котором отображается следующее диалоговое окно), где можно ввести код проверки.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="9c0d5-180">Введите код и нажмите кнопку Отправить, HTTP POST отправляется код `VerifyPhoneNumber` метода действия.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="9c0d5-181">`ChangePhoneNumberAsync` Метод проверяет, отправленное защитный код.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="9c0d5-182">Если правильности кода, номер телефона добавляется `PhoneNumber` поле `AspNetUsers` таблицы.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="9c0d5-183">Если этот вызов был успешным, `SignInAsync` вызывается метод:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="9c0d5-184">`isPersistent` Параметр задает ли сеанс проверки подлинности сохраняется в нескольких запросах.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="9c0d5-185">При изменении профиля безопасности новой метки безопасности создается и сохраняется в `SecurityStamp` поле *AspNetUsers* таблицы.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="9c0d5-186">Следует отметить, что `SecurityStamp` поля отличается от безопасности cookie.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="9c0d5-187">Файл cookie безопасности не хранятся в `AspNetUsers` таблицы (или любом другом месте в базе данных удостоверений).</span><span class="sxs-lookup"><span data-stu-id="9c0d5-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="9c0d5-188">Маркер безопасности cookie самостоятельно подписывается с помощью [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) и создается `UserId, SecurityStamp` и сведения о времени истечения срока действия.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="9c0d5-189">По промежуточного слоя файлов cookie проверяет куки-файл для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="9c0d5-190">`SecurityStampValidator` Метод в `Startup` класс обращений к базе данных и периодически проверяет метку безопасности в соответствии с `validateInterval`.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="9c0d5-191">Это происходит каждые 30 минут (в нашем примере) только изменения профиля безопасности.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="9c0d5-192">Чтобы свести к минимуму обращений к базе данных был выбран интервал 30 минут.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="9c0d5-193">`SignInAsync` Метод должен вызываться при любом изменении профиля безопасности.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="9c0d5-194">При изменении профиля безопасности, база данных находится в обновлений `SecurityStamp` поле и без вызова `SignInAsync` будет находиться в системе метод *только* до следующего запуска конвейер OWIN обращается к базе данных ( `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="9c0d5-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="9c0d5-195">Можно проверить это, изменив `SignInAsync` метод для немедленный возврат, а также установка файла cookie `validateInterval` свойство за 30 минут до 5 секунд:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="9c0d5-196">Выше изменений кода, можно изменить профиль безопасности (например, при изменении состояния **включены два фактора**) и будет выхода в течение 5 секунд при `SecurityStampValidator.OnValidateIdentity` метод завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="9c0d5-197">Удалить строки, возврата в `SignInAsync` метода, сделать другой профиль безопасности изменить, и вам будет не вышел из. `SignInAsync` Метод создает новый файл cookie безопасности.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="9c0d5-198">Включение двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9c0d5-198">Enable two-factor authentication</span></span>

<span data-ttu-id="9c0d5-199">В примере приложения необходимо включить двухфакторную проверку подлинности (2FA) с помощью пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="9c0d5-200">Чтобы включить 2FA, щелкните свой идентификатор пользователя (псевдоним электронной почты) на панели навигации.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="9c0d5-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="9c0d5-201">Щелкните Включить 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="9c0d5-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="9c0d5-202">Выйдите из системы, а затем войдите снова.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-202">Log out, then log back in.</span></span> <span data-ttu-id="9c0d5-203">Если вы включили функцию электронной почты (см. Мои [с предыдущим учебником](account-confirmation-and-password-recovery-with-aspnet-identity.md)), можно выбрать SMS или по электронной почте для 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="9c0d5-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="9c0d5-204">Проверьте кодовая страница отображается, где можно ввести код (от SMS или по электронной почте).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="9c0d5-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="9c0d5-205">Щелкнув **Запомнить этот браузер** флажок Исключить вас от необходимости использовать 2FA войти в систему с этого компьютера и обозреватель.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="9c0d5-206">Включение 2FA и щелкнув **Запомнить этот браузер** обеспечивают надежный 2FA защиты от злоумышленников, пытается получить доступ к вашей учетной записи, пока они не имеют доступа к компьютеру.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="9c0d5-207">Это можно сделать на любом компьютере закрытый, которые регулярно использовать.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="9c0d5-208">Установив **Запомнить этот браузер**, получить дополнительную защиту 2FA на компьютерах, не пользуетесь регулярно и получения удобства на отсутствие необходимости проходить через 2FA на собственных компьютерах.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="9c0d5-209">Как зарегистрировать поставщик двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9c0d5-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="9c0d5-210">При создании нового проекта MVC *IdentityConfig.cs* файл содержит следующий код, чтобы зарегистрировать поставщика двухфакторной проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="9c0d5-211">Добавить номер телефона для 2FA</span><span class="sxs-lookup"><span data-stu-id="9c0d5-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="9c0d5-212">`AddPhoneNumber` Метода действия в `Manage` контроллера создает маркер безопасности и отправляет его на телефоне число, вы предоставили.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="9c0d5-213">После отправки маркер, он перенаправляет `VerifyPhoneNumber` метод действия, где можно ввести код для регистрации 2FA SMS.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="9c0d5-214">SMS 2FA не используется, пока вы проверили номер телефона.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="9c0d5-215">Включение 2FA</span><span class="sxs-lookup"><span data-stu-id="9c0d5-215">Enabling 2FA</span></span>

<span data-ttu-id="9c0d5-216">`EnableTFA` Метод действия включает 2FA:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="9c0d5-217">Примечание `SignInAsync` должен вызываться так, как включить 2FA изменение профиля безопасности.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="9c0d5-218">При включении 2FA пользователю могут потребоваться 2FA для ведения журнала, способами 2FA регистрации (SMS и электронной почты в образце).</span><span class="sxs-lookup"><span data-stu-id="9c0d5-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="9c0d5-219">Можно добавить дополнительные 2FA поставщиков, например генераторы QR-кода или можно написать вы являетесь владельцем (см. [с помощью Google Authenticator с ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span><span class="sxs-lookup"><span data-stu-id="9c0d5-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="9c0d5-220">Коды 2FA создаются с помощью [время одноразовый пароль алгоритм на основе](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) и коды действительны в течение шести минут.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="9c0d5-221">Если более шести минут, введите код, вы получите сообщение об ошибке Недопустимый код.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="9c0d5-222">Объединение социальных сетей и локальных учетных записей</span><span class="sxs-lookup"><span data-stu-id="9c0d5-222">Combine social and local login accounts</span></span>

<span data-ttu-id="9c0d5-223">Можно объединять учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="9c0d5-224">В следующей последовательности &quot; RickAndMSFT@gmail.com &quot; сначала создается как локальное имя входа, но можно создать учетную запись в качестве социальных журналов в первой, а затем добавить локальный вход.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="9c0d5-225">Щелкните **управление** ссылку.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-225">Click on the **Manage** link.</span></span> <span data-ttu-id="9c0d5-226">Примечание внешних 0 (социальных имена входа), связанные с этой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="9c0d5-227">Щелкните ссылку, чтобы другой журнал в службе, которая принимает запросы приложения.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="9c0d5-228">Были объединены две учетные записи, вы сможете войти в систему с любой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="9c0d5-229">Может потребоваться пользователям добавлять локальные учетные записи, в случае их социальных вход службы проверки подлинности не работает, или что более вероятно, они утрачен доступ к учетной записи социальных сетей.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="9c0d5-230">На следующем рисунке Tom является социальных вход (что можно узнать из **внешних имен входа: 1** отображаться на странице).</span><span class="sxs-lookup"><span data-stu-id="9c0d5-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="9c0d5-231">Щелкнув **подбора пароля** можно добавить в локальный журнал на связанные с той же учетной записи.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="9c0d5-232">Блокировка учетной записи, от атак методом подбора</span><span class="sxs-lookup"><span data-stu-id="9c0d5-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="9c0d5-233">Учетные записи можно защитить приложения от атак методом подбора путем включения блокировки пользователя.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="9c0d5-234">В следующем примере кода в `ApplicationUserManager Create` метод настраивает блокировка:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="9c0d5-235">Приведенный выше код позволяет блокировки для двухфакторной проверки подлинности только.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="9c0d5-236">Хотя блокировка для имен входа, можно включить, изменив `shouldLockout` значение true в `Login` метод контроллера учетных записей, мы рекомендуем не включена блокировка для имен входа, потому что позволяет учетной записи подвержены [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) входа атаки.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="9c0d5-237">В образце кода, блокировка отключена для учетной записи администратора, созданной в `ApplicationDbInitializer Seed` метод:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="9c0d5-238">Требуется учетная запись электронной почты проверенного пользователя</span><span class="sxs-lookup"><span data-stu-id="9c0d5-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="9c0d5-239">Следующий код требует, чтобы пользователь имел проверенные электронной почты учетной записи для входа:</span><span class="sxs-lookup"><span data-stu-id="9c0d5-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="9c0d5-240">Как SignInManager проверяет для 2FA требования</span><span class="sxs-lookup"><span data-stu-id="9c0d5-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="9c0d5-241">Локальный вход и проверьте, включена ли 2FA социальных вход.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="9c0d5-242">При включении 2FA `SignInManager` входа возвращает `SignInStatus.RequiresVerification`, и пользователь будет перенаправлен на `SendCode` метод действия, где их придется вводить код для завершения журнала в последовательности.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="9c0d5-243">Если у пользователя есть RememberMe задается в файле cookie локальных пользователей `SignInManager` вернет `SignInStatus.Success` и им не нужно проходить через 2FA.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="9c0d5-244">В следующем коде показано `SendCode` метода действия.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="9c0d5-245">Объект [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) создается со всеми методами 2FA, разрешенных для текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="9c0d5-246">[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) передается [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) вспомогательного приложения, который позволяет пользователю выбрать подход 2FA (обычно электронной почты и SMS).</span><span class="sxs-lookup"><span data-stu-id="9c0d5-246">The [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="9c0d5-247">Когда пользователь отправляет подход 2FA `HTTP POST SendCode` вызывается метод действия, `SignInManager` отправляет 2FA кода и пользователь перенаправляется на `VerifyCode` метод действия, где можно ввести код, чтобы завершить вход.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="9c0d5-248">2FA блокировки</span><span class="sxs-lookup"><span data-stu-id="9c0d5-248">2FA Lockout</span></span>

<span data-ttu-id="9c0d5-249">Несмотря на то, что при сбое попытки пароль имени входа можно задать блокировки учетной записи, такой подход делает ваше имя входа подвержен атакам с [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) блокировки.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="9c0d5-250">Мы рекомендуем использовать только с 2FA блокировки учетной записи.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="9c0d5-251">При `ApplicationUserManager` будет создан, образец кода задает 2FA блокировки и `MaxFailedAccessAttemptsBeforeLockout` до 5.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="9c0d5-252">После пользователь вошел в систему (с помощью локальной учетной записи или учетной записи социальных сетей), каждая неудачная попытка 2FA хранится и при достижении максимального попыток пользователь заблокирован на пять минут (можно задать блокировка времени с `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="9c0d5-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="9c0d5-253">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9c0d5-253">Additional Resources</span></span>

- <span data-ttu-id="9c0d5-254">[ASP.NET Identity, рекомендуется использовать ресурсы](../getting-started/aspnet-identity-recommended-resources.md) полный список идентификаторов блоги, видеоролики, учебники и высококачественных таким ОБРАЗОМ ссылки.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="9c0d5-255">[Приложение MVC 5 с Facebook, Twitter, LinkedIn и входа в Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) также показано, как добавить данные профиля в таблицу пользователей.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="9c0d5-256">[ASP.NET MVC и удостоверение 2.0: представление об основах](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) , Джон Atten.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="9c0d5-257">Подтверждение учетной записи и пароль восстановления в ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="9c0d5-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="9c0d5-258">Введение в ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="9c0d5-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="9c0d5-259">[Объявляет о RTM ASP.NET удостоверения 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) по Pranav Rastogi.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="9c0d5-260">[Удостоверение ASP.NET 2.0: Настройки учетной записи проверки авторизации двухфакторной](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) , Джон Atten.</span><span class="sxs-lookup"><span data-stu-id="9c0d5-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
