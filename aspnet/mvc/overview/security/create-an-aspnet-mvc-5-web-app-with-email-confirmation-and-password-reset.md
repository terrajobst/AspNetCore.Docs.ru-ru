---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Создание безопасного веб-приложения ASP.NET MVC 5 со входом, отправить по электронной почте подтверждение и сброс пароля (C#) | Документация Майкрософт
author: Rick-Anderson
description: Этом руководстве показано, как создавать веб-приложение ASP.NET MVC 5 с подтверждение по электронной почте и сбрасывать пароль с помощью системы членства ASP.NET Identity. ЦС...
ms.author: aspnetcontent
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 1387c3e9c03e011b610a070aa0c273ded23b463e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823551"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="762cd-104">Создание безопасного веб-приложения ASP.NET MVC 5 со входом, отправить по электронной почте подтверждение и сброс пароля (C#)</span><span class="sxs-lookup"><span data-stu-id="762cd-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="762cd-105">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="762cd-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="762cd-106">Этом руководстве показано, как создавать веб-приложение ASP.NET MVC 5 с подтверждение по электронной почте и сбрасывать пароль с помощью системы членства ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="762cd-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="762cd-107">Можно загрузить готовое приложение [здесь](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="762cd-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="762cd-108">Загружаемый файл содержит отладки вспомогательные методы, которые позволяют тестировать подтверждение по электронной почте и SMS без настройки электронной почты или поставщика SMS.</span><span class="sxs-lookup"><span data-stu-id="762cd-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="762cd-109">Это руководство было написано с [Рик Андерсон](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="762cd-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="762cd-110">Создание приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="762cd-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="762cd-111">Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="762cd-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="762cd-112">Установка [Visual Studio 2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="762cd-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="762cd-113">Предупреждение: Необходимо установить [Visual Studio 2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии для работы с этим руководством.</span><span class="sxs-lookup"><span data-stu-id="762cd-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="762cd-114">Создайте новый проект веб-ASP.NET и выберите шаблон MVC.</span><span class="sxs-lookup"><span data-stu-id="762cd-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="762cd-115">Веб-форм также поддерживает ASP.NET Identity, поэтому можно выполнить аналогичные действия в приложении web forms.</span><span class="sxs-lookup"><span data-stu-id="762cd-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="762cd-116">Оставьте проверку подлинности по умолчанию как **учетные записи отдельных пользователей**.</span><span class="sxs-lookup"><span data-stu-id="762cd-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="762cd-117">Если вы хотите разместить приложение в Azure, этот флажок установлен.</span><span class="sxs-lookup"><span data-stu-id="762cd-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="762cd-118">Далее в этом руководстве описывается развертывание в Azure.</span><span class="sxs-lookup"><span data-stu-id="762cd-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="762cd-119">Вы можете [открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="762cd-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="762cd-120">Задайте [проект для использования SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="762cd-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="762cd-121">Запустите приложение, нажмите кнопку **зарегистрировать** ссылку и регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="762cd-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="762cd-122">На этом этапе является только проверка на адрес электронной почты с [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) атрибута.</span><span class="sxs-lookup"><span data-stu-id="762cd-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="762cd-123">В обозревателе сервера перейдите к **Connections\DefaultConnection\Tables\AspNetUsers данных**, щелкните правой кнопкой мыши и выберите **откройте определение таблицы**.</span><span class="sxs-lookup"><span data-stu-id="762cd-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="762cd-124">На следующем рисунке показана `AspNetUsers` схемы:</span><span class="sxs-lookup"><span data-stu-id="762cd-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="762cd-125">Щелкните правой кнопкой мыши **AspNetUsers** таблицы и выберите **Показать таблицу данных**.</span><span class="sxs-lookup"><span data-stu-id="762cd-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="762cd-126">На этом этапе сообщение электронной почты не подтвержден.</span><span class="sxs-lookup"><span data-stu-id="762cd-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="762cd-127">Щелкните строку и выберите Удалить.</span><span class="sxs-lookup"><span data-stu-id="762cd-127">Click on the row and select delete.</span></span> <span data-ttu-id="762cd-128">Вы добавите этот адрес электронной почты еще раз на следующем шаге и отправить электронное сообщение с подтверждением.</span><span class="sxs-lookup"><span data-stu-id="762cd-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="762cd-129">Подтверждение по электронной почте</span><span class="sxs-lookup"><span data-stu-id="762cd-129">Email confirmation</span></span>

<span data-ttu-id="762cd-130">Это рекомендуется, чтобы подтвердить регистрацию новых пользователей, чтобы проверить, они не выполняется олицетворение кто-то другой адрес электронной почты (то есть они еще не зарегистрировано другого пользователя по электронной почте).</span><span class="sxs-lookup"><span data-stu-id="762cd-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="762cd-131">Предположим, что у вас есть дискуссионный форум, следует предотвратить `"bob@example.com"` из регистрации в качестве `"joe@contoso.com"`.</span><span class="sxs-lookup"><span data-stu-id="762cd-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="762cd-132">Без подтверждение по электронной почте `"joe@contoso.com"` удалось получить нежелательные сообщения электронной почты из приложения.</span><span class="sxs-lookup"><span data-stu-id="762cd-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="762cd-133">Предположим, что Боб случайно зарегистрирован как `"bib@example.com"` и не заметили, он не сможет использовать пароль восстановления, так как приложение не имеет правильное сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="762cd-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="762cd-134">Подтверждение по электронной почте предоставляет ограниченную защиту от программ-роботов и не предоставляет защиту от определено отправителей нежелательной почты, они имеют много псевдонимы рабочий электронной почты, которые они могут использовать для регистрации.</span><span class="sxs-lookup"><span data-stu-id="762cd-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="762cd-135">Обычно требуется запретить новым пользователям из учета данных для веб-сайт, прежде чем они подтверждения по электронной почте, SMS-сообщение или другой механизм.</span><span class="sxs-lookup"><span data-stu-id="762cd-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="762cd-136">В следующих разделах мы включим подтверждение по электронной почте и измените код, чтобы предотвратить только что зарегистрированным пользователям войти в систему, пока не будет подтверждена доступ к электронной почте.</span><span class="sxs-lookup"><span data-stu-id="762cd-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="762cd-137">Подключить SendGrid</span><span class="sxs-lookup"><span data-stu-id="762cd-137">Hook up SendGrid</span></span>

<span data-ttu-id="762cd-138">Несмотря на то, что этот учебник только показано, как добавить уведомление по электронной почте через [SendGrid](http://sendgrid.com/), вы можете отправить электронную почту с помощью SMTP и другие механизмы (см. в разделе [дополнительные ресурсы](#addRes)).</span><span class="sxs-lookup"><span data-stu-id="762cd-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="762cd-139">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="762cd-139">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="762cd-140">Перейдите к [страницу регистрации Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) и зарегистрировать для получения бесплатной учетной записи SendGrid.</span><span class="sxs-lookup"><span data-stu-id="762cd-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="762cd-141">Настройка SendGrid, добавив код, аналогичный следующему *App_Start/IdentityConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="762cd-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="762cd-142">Необходимо добавить включает в себя следующее:</span><span class="sxs-lookup"><span data-stu-id="762cd-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="762cd-143">Для простоты в этом примере мы будем хранить параметры приложения в *web.config* файла:</span><span class="sxs-lookup"><span data-stu-id="762cd-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="762cd-144">Безопасность — никогда не сохраняйте конфиденциальные данные в исходном коде.</span><span class="sxs-lookup"><span data-stu-id="762cd-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="762cd-145">Запись и учетные данные хранятся в параметр appSetting.</span><span class="sxs-lookup"><span data-stu-id="762cd-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="762cd-146">В Azure, вы можете безопасно хранить эти значения на **[Настройка](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** вкладка на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="762cd-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="762cd-147">См. в разделе [советы и рекомендации по развертыванию паролей и других конфиденциальных данных в ASP.NET и Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="762cd-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="762cd-148">Включить подтверждение по электронной почте в контроллере Account</span><span class="sxs-lookup"><span data-stu-id="762cd-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="762cd-149">Проверьте *Views\Account\ConfirmEmail.cshtml* файл имеет синтаксис правильный razor.</span><span class="sxs-lookup"><span data-stu-id="762cd-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="762cd-150">(@ Символа в первой строке могут отсутствовать.</span><span class="sxs-lookup"><span data-stu-id="762cd-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="762cd-151">)</span><span class="sxs-lookup"><span data-stu-id="762cd-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="762cd-152">Запустите приложение и щелкните ссылку регистрации.</span><span class="sxs-lookup"><span data-stu-id="762cd-152">Run the app and click the Register link.</span></span> <span data-ttu-id="762cd-153">После отправки формы регистрации вы вошли.</span><span class="sxs-lookup"><span data-stu-id="762cd-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="762cd-154">Проверьте учетную запись электронной почты и щелкните ссылку, чтобы подтвердить свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="762cd-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="762cd-155">Требуется подтверждение по электронной почте до входа</span><span class="sxs-lookup"><span data-stu-id="762cd-155">Require email confirmation before log in</span></span>

<span data-ttu-id="762cd-156">В настоящее время в том случае, когда пользователь завершает регистрационную форму, они вошли.</span><span class="sxs-lookup"><span data-stu-id="762cd-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="762cd-157">Обычно требуется подтвердить доступ к электронной почте перед их вход.</span><span class="sxs-lookup"><span data-stu-id="762cd-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="762cd-158">В разделе ниже мы будем изменять код, чтобы требовать новых пользователей, иметь подтвержден по электронной почте, прежде чем они вошли (с проверкой подлинности).</span><span class="sxs-lookup"><span data-stu-id="762cd-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="762cd-159">Обновление `HttpPost Register` метод с следующие выделенные изменения:</span><span class="sxs-lookup"><span data-stu-id="762cd-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="762cd-160">При комментировании `SignInAsync` метод, пользователь не будет подписано путем регистрации.</span><span class="sxs-lookup"><span data-stu-id="762cd-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="762cd-161">`TempData["ViewBagLink"] = callbackUrl;` Строки могут быть использованы для [отладку приложения](#dbg) и проверка регистрации без отправки по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="762cd-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="762cd-162">`ViewBag.Message` используется для отображения инструкции подтверждение.</span><span class="sxs-lookup"><span data-stu-id="762cd-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="762cd-163">[Загрузить образец](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) содержит код для тестирования без настройки по электронной почте подтверждение по электронной почте, а также можно использовать для отладки приложения.</span><span class="sxs-lookup"><span data-stu-id="762cd-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="762cd-164">Создание `Views\Shared\Info.cshtml` файл и добавьте следующую разметку razor:</span><span class="sxs-lookup"><span data-stu-id="762cd-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="762cd-165">Добавить [атрибут Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) для `Contact` метод действия контроллера Home.</span><span class="sxs-lookup"><span data-stu-id="762cd-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="762cd-166">Вы можете щелкнуть **контакт** ссылку для проверки анонимных пользователей нет доступа и прошедшие проверку подлинности пользователи имеют доступ.</span><span class="sxs-lookup"><span data-stu-id="762cd-166">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="762cd-167">Вы также должны обновить `HttpPost Login` метод действия:</span><span class="sxs-lookup"><span data-stu-id="762cd-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="762cd-168">Обновление *Views\Shared\Error.cshtml* представление, чтобы отобразить сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="762cd-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="762cd-169">Удалить все учетные записи в **AspNetUsers** таблица, которая содержит псевдоним электронной почты, который вы хотите протестировать.</span><span class="sxs-lookup"><span data-stu-id="762cd-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="762cd-170">Запустите приложение и убедитесь, что не удается войти до подтверждения ваш адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="762cd-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="762cd-171">Когда вы подтвердите адрес электронной почты, нажмите кнопку **контакт** ссылку.</span><span class="sxs-lookup"><span data-stu-id="762cd-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="762cd-172">Восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="762cd-172">Password recovery/reset</span></span>

<span data-ttu-id="762cd-173">Удалите символы комментария из `HttpPost ForgotPassword` метода действия в контроллере account:</span><span class="sxs-lookup"><span data-stu-id="762cd-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="762cd-174">Удалите символы комментария из `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) в *Views\Account\Login.cshtml* файл представления razor:</span><span class="sxs-lookup"><span data-stu-id="762cd-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="762cd-175">На страницу входа, теперь будет отображать ссылку для сброса пароля.</span><span class="sxs-lookup"><span data-stu-id="762cd-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="762cd-176">Повторно отправить ссылку для подтверждения по электронной почте</span><span class="sxs-lookup"><span data-stu-id="762cd-176">Resend email confirmation link</span></span>

<span data-ttu-id="762cd-177">Когда пользователь создает новую учетную запись локального, по электронной почте ссылку для подтверждения, они необходимы для использования, прежде чем они могут войти на.</span><span class="sxs-lookup"><span data-stu-id="762cd-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="762cd-178">Если пользователь случайно удалит сообщение подтверждения или сообщение никогда не будет доставлено, им потребуется повторная отправка ссылку для подтверждения.</span><span class="sxs-lookup"><span data-stu-id="762cd-178">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="762cd-179">Следующие изменения кода показано, как для этого.</span><span class="sxs-lookup"><span data-stu-id="762cd-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="762cd-180">Добавьте следующий вспомогательный метод в нижнюю часть *файл Controllers\AccountController.cs* файл:</span><span class="sxs-lookup"><span data-stu-id="762cd-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="762cd-181">Обновите метод регистрации для использования нового модуля поддержки:</span><span class="sxs-lookup"><span data-stu-id="762cd-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="762cd-182">Обновите метод входа для повторной отправки пароля, если учетная запись пользователя не имеет подтверждения:</span><span class="sxs-lookup"><span data-stu-id="762cd-182">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="762cd-183">Объединить учетные записи социальных сетей и локального имени входа</span><span class="sxs-lookup"><span data-stu-id="762cd-183">Combine social and local login accounts</span></span>

<span data-ttu-id="762cd-184">Можно объединить учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="762cd-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="762cd-185">В следующей последовательности **RickAndMSFT@gmail.com** сначала создается как локальное имя входа, но можно создать учетную запись как журнал социальных сетей в первой, а затем добавить локальное имя входа.</span><span class="sxs-lookup"><span data-stu-id="762cd-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="762cd-186">Щелкните **управление** ссылку.</span><span class="sxs-lookup"><span data-stu-id="762cd-186">Click on the **Manage** link.</span></span> <span data-ttu-id="762cd-187">Примечание **внешних имен входа: 0** связанные с этой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="762cd-187">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="762cd-188">Щелкните ссылку, чтобы другой журнал в службе и принимать запросы приложений.</span><span class="sxs-lookup"><span data-stu-id="762cd-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="762cd-189">Были объединены две учетные записи, вы сможете выполнить вход с использованием либо учетной записи.</span><span class="sxs-lookup"><span data-stu-id="762cd-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="762cd-190">Вы можете добавить локальные учетные записи, в случае их социальных сетей журнала в службе проверки подлинности не работает, или более вероятно, они потеряли доступ к своей учетной записи социальной сети пользователей.</span><span class="sxs-lookup"><span data-stu-id="762cd-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="762cd-191">На следующем рисунке, Tom является журналом социальных сетей в (который также можно увидеть из **внешних имен входа: 1** на странице).</span><span class="sxs-lookup"><span data-stu-id="762cd-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="762cd-192">Щелкнув **выберите пароль** позволяет добавлять в локальный журнал на связанные с той же учетной записи.</span><span class="sxs-lookup"><span data-stu-id="762cd-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="762cd-193">Подтверждение по электронной почте Подробнее</span><span class="sxs-lookup"><span data-stu-id="762cd-193">Email confirmation in more depth</span></span>

<span data-ttu-id="762cd-194">Мои руководстве [подтверждение учетной записи и восстановление пароля в ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) переходит в этом разделе с более подробными сведениями.</span><span class="sxs-lookup"><span data-stu-id="762cd-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="762cd-195">Отладка приложения</span><span class="sxs-lookup"><span data-stu-id="762cd-195">Debugging the app</span></span>

<span data-ttu-id="762cd-196">Если вы не получили сообщение электронной почты, содержащее ссылку:</span><span class="sxs-lookup"><span data-stu-id="762cd-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="762cd-197">Проверьте папку нежелательной или Нежелательная почта.</span><span class="sxs-lookup"><span data-stu-id="762cd-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="762cd-198">Войдите в учетную запись SendGrid и выберите [действия по электронной почте ссылку](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="762cd-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="762cd-199">Чтобы проверить ссылку для проверки без электронной почты, загрузите [готовый пример](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="762cd-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="762cd-200">Ссылку для подтверждения и коды подтверждения будет отображаться на странице.</span><span class="sxs-lookup"><span data-stu-id="762cd-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="762cd-201">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="762cd-201">Additional Resources</span></span>

- [<span data-ttu-id="762cd-202">Рекомендуемые ресурсы по ссылкам в ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="762cd-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="762cd-203">[Подтверждение учетной записи и восстановление пароля в ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) содержит более подробные сведения на подтверждение пароля восстановления и учетную запись.</span><span class="sxs-lookup"><span data-stu-id="762cd-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="762cd-204">[Приложение MVC 5 с Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) этом руководстве показано, как писать приложения ASP.NET MVC 5 с авторизацией на Facebook и Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="762cd-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="762cd-205">Также показано, как добавить дополнительные данные для базы данных удостоверений.</span><span class="sxs-lookup"><span data-stu-id="762cd-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="762cd-206">[Развертывание безопасного приложения ASP.NET MVC с членством, OAuth и базой данных SQL в Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="762cd-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="762cd-207">В этом руководстве добавляется развертывания Azure, как защитить приложения с помощью ролей, как использовать API членства для добавления пользователей и ролей и дополнительные функции безопасности.</span><span class="sxs-lookup"><span data-stu-id="762cd-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="762cd-208">Создание приложения Google для oauth2 и подключение приложения к проекту</span><span class="sxs-lookup"><span data-stu-id="762cd-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="762cd-209">Создать приложение в Facebook и подключить приложение к проекту</span><span class="sxs-lookup"><span data-stu-id="762cd-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="762cd-210">Настройка SSL в проекте</span><span class="sxs-lookup"><span data-stu-id="762cd-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
