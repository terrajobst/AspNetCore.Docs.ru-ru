---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: "Приложения ASP.NET MVC 5 с помощью SMS и двухфакторная проверка подлинности электронной почты | Документы Microsoft"
author: Rick-Anderson
description: "Этого учебника показано, как построить веб-приложение ASP.NET MVC 5 с двухфакторной проверки подлинности. Необходимо выполнить создание безопасных ASP.NET MVC 5 веб-приложения с..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: db57b8fe44f41d65d27964f45e0884138629f92b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="995ba-104">Приложения ASP.NET MVC 5 с помощью SMS и электронной почты двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="995ba-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="995ba-105">По [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="995ba-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="995ba-106">Этого учебника показано, как построить веб-приложение ASP.NET MVC 5 с двухфакторной проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="995ba-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="995ba-107">Следует выполнить [создания безопасного веб-приложения ASP.NET MVC 5 с журналом в сбросить пароль и подтверждение по электронной почте](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) перед продолжением.</span><span class="sxs-lookup"><span data-stu-id="995ba-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="995ba-108">Завершенное приложение можно загрузить [здесь](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="995ba-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="995ba-109">Загружаемый файл содержит отладки вспомогательные методы, которые позволяют тестировать подтверждения электронной почты и SMS без настройки электронной почты или поставщика SMS.</span><span class="sxs-lookup"><span data-stu-id="995ba-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="995ba-110">Это руководство было написано с [Рик Андерсон](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="995ba-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="995ba-111">Создание приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="995ba-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="995ba-112">Настройка SMS для двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="995ba-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="995ba-113">Включение двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="995ba-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="995ba-114">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="995ba-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="995ba-115">Создание приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="995ba-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="995ba-116">Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="995ba-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="995ba-117">Установка [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="995ba-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="995ba-118">Предупреждение: Необходимо выполнить [создания безопасного веб-приложения ASP.NET MVC 5 с журналом в сбросить пароль и подтверждение по электронной почте](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) перед продолжением.</span><span class="sxs-lookup"><span data-stu-id="995ba-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="995ba-119">Необходимо установить [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии для выполнения заданий данного учебника.</span><span class="sxs-lookup"><span data-stu-id="995ba-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="995ba-120">Создайте новый проект веб-ASP.NET и выберите шаблон MVC.</span><span class="sxs-lookup"><span data-stu-id="995ba-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="995ba-121">Web Forms также поддерживает ASP.NET Identity, так что можно выполнить аналогичные шаги в приложении web forms.</span><span class="sxs-lookup"><span data-stu-id="995ba-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="995ba-122">Оставить как проверку подлинности по умолчанию **индивидуальные учетные записи**.</span><span class="sxs-lookup"><span data-stu-id="995ba-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="995ba-123">Если вы хотите разместить приложение в Azure, оставьте флажок установлен.</span><span class="sxs-lookup"><span data-stu-id="995ba-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="995ba-124">Далее в этом учебнике описывается развертывание в Azure.</span><span class="sxs-lookup"><span data-stu-id="995ba-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="995ba-125">Вы можете [открыть учетную запись Azure бесплатно](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="995ba-125">You can [open an Azure account for free](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="995ba-126">Задать [проекта для использования SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="995ba-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="995ba-127">Настройка SMS для двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="995ba-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="995ba-128">Этот учебник содержит инструкции по использованию Twilio или ASPSMS, но можно использовать любой другой поставщик SMS.</span><span class="sxs-lookup"><span data-stu-id="995ba-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="995ba-129">**Создание учетной записи пользователя с помощью поставщика SMS**</span><span class="sxs-lookup"><span data-stu-id="995ba-129">**Creating a User Account with an SMS provider**</span></span>  
  
 <span data-ttu-id="995ba-130">Создание [Twilio](https://www.twilio.com/try-twilio) или [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) учетной записи.</span><span class="sxs-lookup"><span data-stu-id="995ba-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="995ba-131">**Установка дополнительных пакетов или Добавление ссылки на службу**</span><span class="sxs-lookup"><span data-stu-id="995ba-131">**Installing additional packages or adding service references**</span></span>  
  
 <span data-ttu-id="995ba-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="995ba-132">Twilio:</span></span>  
 <span data-ttu-id="995ba-133">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="995ba-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
 <span data-ttu-id="995ba-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="995ba-134">ASPSMS:</span></span>  
 <span data-ttu-id="995ba-135">Следующие ссылки на службу необходимо добавить:</span><span class="sxs-lookup"><span data-stu-id="995ba-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
 <span data-ttu-id="995ba-136">Адрес:</span><span class="sxs-lookup"><span data-stu-id="995ba-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 <span data-ttu-id="995ba-137">Пространство имен:</span><span class="sxs-lookup"><span data-stu-id="995ba-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="995ba-138">**Изучив учетные данные пользователя поставщика SMS**</span><span class="sxs-lookup"><span data-stu-id="995ba-138">**Figuring out SMS Provider User credentials**</span></span>  
  
 <span data-ttu-id="995ba-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="995ba-139">Twilio:</span></span>  
 <span data-ttu-id="995ba-140">Из **мониторинга** вкладка учетной записи Twilio, Копировать **ИД безопасности учетной записи** и **маркер проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="995ba-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
 <span data-ttu-id="995ba-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="995ba-141">ASPSMS:</span></span>  
 <span data-ttu-id="995ba-142">Параметры учетной записи, перейдите к **использованием ключа пользователя** и скопируйте его вместе с самостоятельно определенных **пароль**.</span><span class="sxs-lookup"><span data-stu-id="995ba-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
 <span data-ttu-id="995ba-143">Позже он будет храниться эти значения в *web.config* файла в ключи `"SMSAccountIdentification"` и `"SMSAccountPassword"` .</span><span class="sxs-lookup"><span data-stu-id="995ba-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="995ba-144">**Указав идентификатор отправителя / инициатора**</span><span class="sxs-lookup"><span data-stu-id="995ba-144">**Specifying SenderID / Originator**</span></span>  
  
 <span data-ttu-id="995ba-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="995ba-145">Twilio:</span></span>  
 <span data-ttu-id="995ba-146">Из **номера** вкладки, скопируйте Twilio номер телефона.</span><span class="sxs-lookup"><span data-stu-id="995ba-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
 <span data-ttu-id="995ba-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="995ba-147">ASPSMS:</span></span>  
 <span data-ttu-id="995ba-148">В пределах **разблокировать инициаторы** меню разблокирования одного или нескольких источников или выберите инициатор буквенно-цифровых (поддерживаются не все сети).</span><span class="sxs-lookup"><span data-stu-id="995ba-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
 <span data-ttu-id="995ba-149">Позже он будет храниться в это значение *web.config* файла в ключе `"SMSAccountFrom"` .</span><span class="sxs-lookup"><span data-stu-id="995ba-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="995ba-150">**Передача учетных данных поставщика SMS в приложение**</span><span class="sxs-lookup"><span data-stu-id="995ba-150">**Transferring SMS provider credentials into app**</span></span>  
  
 <span data-ttu-id="995ba-151">Предоставления учетных данных и номер телефона отправителя для приложения.</span><span class="sxs-lookup"><span data-stu-id="995ba-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="995ba-152">Чтобы не усложнять будут храниться эти значения в *web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="995ba-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="995ba-153">При развертывании в Azure, мы может хранить значения как обеспечить безопасность при **параметры приложения** вкладка настройки раздел на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="995ba-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="995ba-154">Безопасность — никогда не конфиденциальных данных в хранилище в исходном коде.</span><span class="sxs-lookup"><span data-stu-id="995ba-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="995ba-155">Учетная запись и учетные данные добавляются выше, чтобы не усложнять в образце кода.</span><span class="sxs-lookup"><span data-stu-id="995ba-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="995ba-156">В разделе [советы и рекомендации по развертыванию пароли и другие конфиденциальные данные в ASP.NET и Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="995ba-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="995ba-157">**Реализация передачи данных для поставщика SMS**</span><span class="sxs-lookup"><span data-stu-id="995ba-157">**Implementation of data transfer to SMS provider**</span></span>  
  
 <span data-ttu-id="995ba-158">Настройка `SmsService` класса в *приложения\_Start\IdentityConfig.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="995ba-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
 <span data-ttu-id="995ba-159">В зависимости от используемых поставщика SMS активировать **Twilio** или **ASPSMS** раздела:</span><span class="sxs-lookup"><span data-stu-id="995ba-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="995ba-160">Обновление *Views\Manage\Index.cshtml* представления Razor: (Примечание: не просто удалить все комментарии в существующий код, используйте приведенный ниже код.)</span><span class="sxs-lookup"><span data-stu-id="995ba-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="995ba-161">Проверьте `EnableTwoFactorAuthentication` и `DisableTwoFactorAuthentication` методы действий в `ManageController` имеют[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) атрибута:</span><span class="sxs-lookup"><span data-stu-id="995ba-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="995ba-162">Запустите приложение и войти с использованием учетной записи, которую вы ранее зарегистрированного.</span><span class="sxs-lookup"><span data-stu-id="995ba-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="995ba-163">Щелкните идентификатор пользователя, который активирует `Index` метода действия в `Manage` контроллера.</span><span class="sxs-lookup"><span data-stu-id="995ba-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="995ba-164">Нажмите кнопку Добавить.</span><span class="sxs-lookup"><span data-stu-id="995ba-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="995ba-165">`AddPhoneNumber` Метод действия отображает диалоговое окно, введите номер телефона, который может получить SMS-сообщений.</span><span class="sxs-lookup"><span data-stu-id="995ba-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="995ba-166">Через несколько секунд вы получите SMS с кодом проверки.</span><span class="sxs-lookup"><span data-stu-id="995ba-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="995ba-167">Введите его и нажмите клавишу **отправить**.</span><span class="sxs-lookup"><span data-stu-id="995ba-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="995ba-168">Представления управление показывает, что был добавлен в ваш номер телефона.</span><span class="sxs-lookup"><span data-stu-id="995ba-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="995ba-169">Включение двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="995ba-169">Enable two-factor authentication</span></span>

<span data-ttu-id="995ba-170">В приложении создается шаблон необходимо включить двухфакторную проверку подлинности (2FA) с помощью пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="995ba-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="995ba-171">Чтобы включить 2FA, щелкните свой идентификатор пользователя (псевдоним электронной почты) на панели навигации.</span><span class="sxs-lookup"><span data-stu-id="995ba-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="995ba-172">Щелкните Включить 2FA.</span><span class="sxs-lookup"><span data-stu-id="995ba-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="995ba-173">Выхода из системы, затем войдите снова.</span><span class="sxs-lookup"><span data-stu-id="995ba-173">Logout, then log back in.</span></span> <span data-ttu-id="995ba-174">Если вы включили функцию электронной почты (см. Мои [с предыдущим учебником](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), можно выбрать SMS или по электронной почте для 2FA.</span><span class="sxs-lookup"><span data-stu-id="995ba-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="995ba-175">Проверьте кодовая страница отображается, где можно ввести код (от SMS или по электронной почте).</span><span class="sxs-lookup"><span data-stu-id="995ba-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="995ba-176">Щелкнув **Запомнить этот браузер** флажок Исключить вас от необходимости использовать 2FA для входа при использовании браузера и устройства, где установлен флажок.</span><span class="sxs-lookup"><span data-stu-id="995ba-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="995ba-177">До тех пор, пока пользователи-злоумышленники не может получить доступ к устройству, включение 2FA и щелкнув **Запомнить этот браузер** обеспечивают удобный доступ пароль один шаг, сохранив строгого 2FA защиты для всех видов доступа из ненадежных устройства.</span><span class="sxs-lookup"><span data-stu-id="995ba-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="995ba-178">Это можно сделать на любом устройстве закрытого регулярно используемых.</span><span class="sxs-lookup"><span data-stu-id="995ba-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="995ba-179">Этот учебник содержит краткие сведения о включении 2FA на новое приложение ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="995ba-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="995ba-180">Мои учебника [двухфакторной проверки подлинности с помощью SMS и электронной почты с ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) попадают в подробности кода программной части образца.</span><span class="sxs-lookup"><span data-stu-id="995ba-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="995ba-181">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="995ba-181">Additional Resources</span></span>

- <span data-ttu-id="995ba-182">[Двухфакторная проверка подлинности с помощью SMS и электронной почты с ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) переходит в сведения о двухфакторной проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="995ba-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="995ba-183">Ссылки на ASP.NET Identity, рекомендуется использовать ресурсы</span><span class="sxs-lookup"><span data-stu-id="995ba-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="995ba-184">[Учетной записи и подтверждение пароля восстановления с ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) приведены более подробные сведения на подтверждение пароля восстановления и учетной записи.</span><span class="sxs-lookup"><span data-stu-id="995ba-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="995ba-185">[Приложение MVC 5 с Facebook, Twitter, LinkedIn и входа в Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) этого учебника показано, как написать приложение ASP.NET MVC 5 с Facebook и Google OAuth 2 авторизации.</span><span class="sxs-lookup"><span data-stu-id="995ba-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="995ba-186">Также показано, как добавить дополнительные данные в базы данных удостоверений.</span><span class="sxs-lookup"><span data-stu-id="995ba-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="995ba-187">[Развертывание приложения безопасного ASP.NET MVC с членством, OAuth и базы данных SQL Azure веб-](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="995ba-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="995ba-188">Этот учебник добавляет развертывания Azure, как защищать приложения с ролями, как использовать API членства для добавления пользователей и ролей и дополнительные функции безопасности.</span><span class="sxs-lookup"><span data-stu-id="995ba-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="995ba-189">Создание приложения Google OAuth 2 и подключение приложения в проект</span><span class="sxs-lookup"><span data-stu-id="995ba-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="995ba-190">Создать приложение в Facebook и подключить приложения в проект</span><span class="sxs-lookup"><span data-stu-id="995ba-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="995ba-191">Настройка SSL в проекте</span><span class="sxs-lookup"><span data-stu-id="995ba-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
