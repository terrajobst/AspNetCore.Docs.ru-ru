---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Создание MVC 5 приложения с помощью Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on (C#) | Документация Майкрософт
author: Rick-Anderson
description: Этот учебник показывает, как создавать веб-приложения ASP.NET MVC 5, который позволяет пользователям выполнять вход с помощью OAuth 2.0 с учетными данными из внешних проверка...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 330cb290668ae951e822b95990ed92100b790cd5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835726"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="cc3a8-103">Создать приложение ASP.NET MVC 5 с помощью Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on (C#)</span><span class="sxs-lookup"><span data-stu-id="cc3a8-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="cc3a8-104">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="cc3a8-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="cc3a8-105">Этом руководстве показано, как построить веб-приложение ASP.NET MVC 5, которая позволяет пользователям входить в систему с помощью [OAuth 2.0](http://oauth.net/2/) с учетными данными от поставщика внешней проверки подлинности, таких как Facebook, Twitter, LinkedIn, Microsoft или Google.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="cc3a8-106">Для простоты это руководство посвящено работе с учетными данными с Facebook и Google.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="cc3a8-107">Включение эти учетные данные в веб-сайтов обеспечивает значительное преимущество, так как миллионам пользователей уже есть учетные записи с помощью этих внешних поставщиков.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="cc3a8-108">Эти пользователи могут быть более необходимо, чтобы зарегистрироваться для веб-узла, если они не нужно создавать и запоминать новый набор учетных данных.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="cc3a8-109">См. также [приложения ASP.NET MVC 5 с помощью SMS и электронной почты двухфакторной проверки подлинности](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="cc3a8-110">В руководстве также показано, как добавить данные профиля пользователя, а также как использовать API членства для добавления ролей.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="cc3a8-111">Это руководство было написано с [Рик Андерсон](https://blogs.msdn.com/rickAndy) (выполните мне в Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="cc3a8-112">Начало работы</span><span class="sxs-lookup"><span data-stu-id="cc3a8-112">Getting Started</span></span>

<span data-ttu-id="cc3a8-113">Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="cc3a8-114">Установка Visual Studio [2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390521) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="cc3a8-115">Получить справку по Dropbox, GitHub, Linkedin, Instagram, буфер, Salesforce, поток данных, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! и многое другое, см. в этом [пример проекта](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="cc3a8-116">Необходимо установить Visual Studio [2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390521) или более поздней версии с помощью Google OAuth 2 и отлаживать локально без предупреждений SSL.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="cc3a8-117">Нажмите кнопку **новый проект** из **запустить** страницы, или можно использовать меню и выберите **файл**, а затем **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="cc3a8-118">Создание первого приложения</span><span class="sxs-lookup"><span data-stu-id="cc3a8-118">Creating Your First Application</span></span>

<span data-ttu-id="cc3a8-119">Нажмите кнопку **новый проект**, а затем выберите **Visual C#** слева, затем **Web** , а затем выберите **веб-приложение ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="cc3a8-120">Имя проекта «MvcAuth» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="cc3a8-121">В **новый проект ASP.NET** диалоговое окно, нажмите кнопку **MVC**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="cc3a8-122">Если проверка подлинности не **учетные записи отдельных пользователей**, нажмите кнопку **изменить способ проверки подлинности** и выберите **учетные записи отдельных пользователей**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="cc3a8-123">Проверив **разместить в облаке**, приложение будет очень легко разместить в Azure.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="cc3a8-124">Если вы выбрали **разместить в облаке**, заполните поля диалогового окна настройки.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="cc3a8-125">Используйте NuGet для обновления до последней по промежуточного слоя OWIN</span><span class="sxs-lookup"><span data-stu-id="cc3a8-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="cc3a8-126">Используйте диспетчер пакетов NuGet для обновления [по промежуточного слоя OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="cc3a8-127">Выберите **обновления** в меню слева.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="cc3a8-128">Вы можете щелкнуть **обновить все** кнопку можно искать только пакеты OWIN (показано на рисунке ниже):</span><span class="sxs-lookup"><span data-stu-id="cc3a8-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="cc3a8-129">В приведенном ниже рисунке показаны только пакеты OWIN.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="cc3a8-130">Из консоли диспетчера пакетов (PMC), можно ввести `Update-Package` команду, которая будет обновить все пакеты.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="cc3a8-131">Нажмите клавишу **F5** или **Ctrl + F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="cc3a8-132">На рисунке ниже номер порта — 1234.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="cc3a8-133">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="cc3a8-134">В зависимости от размера окна браузера может потребоваться щелкнуть значок навигации, чтобы см. в разделе **Главная**, **о**, **контакт**, **зарегистрировать**и **вход** ссылки.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="cc3a8-135">Настройка SSL в проекте</span><span class="sxs-lookup"><span data-stu-id="cc3a8-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="cc3a8-136">Чтобы подключиться к поставщикам проверки подлинности, таких как Google и Facebook, необходимо настроить IIS Express для использования протокола SSL.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="cc3a8-137">Необходимо продолжать использовать SSL после входа в систему и не удалять обратно к HTTP, ваш файл cookie входа — так же, как секрет, как имя пользователя и пароль и без использования SSL, вы отправляете его в открытый текст по каналу связи.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="cc3a8-138">Кроме того, вы уже сделали время для выполнения установки соединения и обеспечьте безопасность канала (это основная часть делает HTTPS медленнее, чем HTTP) перед запуском конвейера MVC, поэтому перенаправление обратно HTTP после входа не текущего запроса или сделать будущее запросы гораздо быстрее.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="cc3a8-139">В **обозревателе решений**, нажмите кнопку **MvcAuth** проекта.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="cc3a8-140">Нажмите клавишу F4, чтобы показать свойства проекта.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="cc3a8-141">Кроме того, из **представление** меню можно выбрать **окно "Свойства"**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="cc3a8-142">Изменение **SSL включен** значение true.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="cc3a8-143">Скопируйте URL-адрес SSL (который может быть `https://localhost:44300/` Если вы создали другие проекты SSL).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="cc3a8-144">В **обозревателе решений**, щелкните правой кнопкой мыши **MvcAuth** проекта и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="cc3a8-145">Выберите **Web** вкладке, а затем вставьте URL-адрес SSL в **URL-адрес проекта** поле.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="cc3a8-146">Сохраните файл (Ctl + S).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="cc3a8-147">Вам потребуется этот URL-адрес для настройки проверки подлинности приложения Facebook и Google.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="cc3a8-148">Добавить [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) атрибут `Home` контроллера для всех запросов необходимо использовать протокол HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="cc3a8-149">Более безопасный подход заключается в добавлении [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) фильтр к приложению.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="cc3a8-150">См. в разделе &quot;защиты приложения с помощью SSL и атрибута авторизации&quot; в моей tutoral [Создание приложения ASP.NET MVC с проверкой подлинности и база данных SQL и развертывание в службе приложений Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="cc3a8-151">Ниже приведен фрагмент контроллера Home.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="cc3a8-152">Нажмите CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="cc3a8-153">Если вы установили сертификат в прошлом, вы можете пропустить оставшейся части этого раздела и перейти к [Создание приложения Google для oauth2 и подключение приложения к проекту](#goog), в противном случае следуйте инструкциям, чтобы доверия к самозаверяющему сертификат, созданный IIS Express.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="cc3a8-154">Чтение **предупреждение системы безопасности** диалоговое окно, а затем нажмите кнопку **Да** Если вы хотите установить сертификат, представляющий localhost.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="cc3a8-155">IE откроется *Главная* страницы и без предупреждений SSL.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="cc3a8-156">Google Chrome также принимает сертификат и будет показано содержимое HTTPS без предупреждения.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="cc3a8-157">Firefox использует собственное хранилище сертификатов, поэтому он будет отображаться предупреждение.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="cc3a8-158">Для нашего приложения могут безопасно щелкните **я понимаю последствия**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="cc3a8-159">Создание приложения Google для oauth2 и подключение приложения к проекту</span><span class="sxs-lookup"><span data-stu-id="cc3a8-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="cc3a8-160">Актуальные инструкции Google OAuth, см. в разделе [Google Настройка проверки подлинности в ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="cc3a8-161">Перейдите к [Google Developers Console](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="cc3a8-162">Если вы еще не создали проект перед, выберите **учетные данные** на левой вкладке, а затем выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="cc3a8-163">На левой вкладке щелкните **учетные данные**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="cc3a8-164">Нажмите кнопку **создайте учетные данные** затем **идентификатор клиента OAuth**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="cc3a8-165">В **Создание идентификатора клиента** диалоговое окно, оставьте значение по умолчанию **веб-приложение** для типа приложения.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="cc3a8-166">Задайте **авторизованных JavaScript** источников на приведенном выше URL-адрес SSL (`https://localhost:44300/` Если вы создали другие проекты SSL)</span><span class="sxs-lookup"><span data-stu-id="cc3a8-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="cc3a8-167">Задайте **авторизованные URI перенаправления** для:</span><span class="sxs-lookup"><span data-stu-id="cc3a8-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="cc3a8-168">Щелкните пункт меню экран согласия OAuth, а затем задайте адрес и продукта имени электронной почты.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="cc3a8-169">После завершения щелкните форму **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="cc3a8-170">Пункт меню библиотеки, поиска **Google + API**, щелкните его, а затем нажмите клавишу Enable.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="cc3a8-171">На рисунке ниже показана включенных API-интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="cc3a8-172">Посетите из API-интерфейсов API диспетчера Google, **учетные данные** tab, чтобы получить **идентификатор клиента**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="cc3a8-173">Загрузки, чтобы сохранить файл JSON с секретными данными приложения.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="cc3a8-174">Скопируйте и вставьте **ClientId** и **ClientSecret** в `UseGoogleAuthentication` найти метод в *Startup.Auth.cs* файл *App_Start* папки.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="cc3a8-175">**ClientId** и **ClientSecret** значений, приведенных ниже примеров и не работают.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="cc3a8-176">Безопасность — никогда не сохраняйте конфиденциальные данные в исходном коде.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="cc3a8-177">Запись и учетные данные добавляются в код выше для простоты в примере.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="cc3a8-178">См. в разделе [советы и рекомендации по развертыванию паролей и других конфиденциальных данных в ASP.NET и службы приложений Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="cc3a8-179">Нажмите клавишу **CTRL + F5** для сборки и запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="cc3a8-180">Нажмите кнопку **вход** ссылку.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="cc3a8-181">В разделе **вход с помощью другой службы**, нажмите кнопку **Google**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="cc3a8-182">Если вы упустите какие-либо шаги выше вы получите ошибку HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="cc3a8-183">Проверьте выполненные действия выше.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-183">Recheck your steps above.</span></span> <span data-ttu-id="cc3a8-184">Если вы пропустите обязательный параметр (например **название продукта**), чтобы добавить отсутствующий элемент и сохраните; может занять несколько минут для проверки подлинности для работы.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="cc3a8-185">Вы будете перенаправлены на сайт Google, где можно будет ввести свои учетные данные.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="cc3a8-186">После того как вы введете свои учетные данные, вам будет предложено предоставить разрешения на доступ к веб-приложение, которое вы только что создали:</span><span class="sxs-lookup"><span data-stu-id="cc3a8-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="cc3a8-187">Нажмите кнопку **принимать**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-187">Click **Accept**.</span></span> <span data-ttu-id="cc3a8-188">Вы будете перенаправлены обратно **зарегистрировать** страница MvcAuth приложения, где можно зарегистрировать учетную запись Google.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="cc3a8-189">Вы можете изменить имя регистрации локальной электронной почты для учетной записи Gmail, но обычно требуется сохранить псевдоним электронной почты по умолчанию (то есть той, которая используется для проверки подлинности).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="cc3a8-190">Щелкните ссылку **Регистрация**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="cc3a8-191">Создать приложение в Facebook и подключить приложение к проекту</span><span class="sxs-lookup"><span data-stu-id="cc3a8-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="cc3a8-192">Актуальные инструкции для проверки подлинности Facebook OAuth2, см. в разделе [проверки подлинности Facebook, Настройка](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="cc3a8-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>


<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="cc3a8-193">Изучите данные членства</span><span class="sxs-lookup"><span data-stu-id="cc3a8-193">Examine the Membership Data</span></span>

<span data-ttu-id="cc3a8-194">В **представление** меню, щелкните **обозревателя серверов**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-194">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="cc3a8-195">Разверните **DefaultConnection (MvcAuth)**, разверните **таблиц**, щелкните правой кнопкой мыши **AspNetUsers** и нажмите кнопку **Показать таблицу данных**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-195">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![данные таблицы aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="cc3a8-197">Добавление данных профиля в класс пользователя</span><span class="sxs-lookup"><span data-stu-id="cc3a8-197">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="cc3a8-198">В этом разделе вы добавите Дата рождения и родного города в пользовательских данных во время регистрации, как показано на следующем рисунке.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-198">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![REG родного города и д.рожд.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="cc3a8-200">Откройте *Models\IdentityModels.cs* файл и добавьте свойства Город Домашняя страница и даты рождения:</span><span class="sxs-lookup"><span data-stu-id="cc3a8-200">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="cc3a8-201">Откройте *Models\AccountViewModels.cs* файл и задайте рождения даты и домашней свойства города в `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-201">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="cc3a8-202">Откройте *файл Controllers\AccountController.cs* файл и добавьте код города Домашняя страница и даты рождения в `ExternalLoginConfirmation` метода действия, как показано:</span><span class="sxs-lookup"><span data-stu-id="cc3a8-202">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="cc3a8-203">Добавление даты рождения и родного города для *Views\Account\ExternalLoginConfirmation.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="cc3a8-203">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="cc3a8-204">Удаление базы данных членства, чтобы можно было снова зарегистрировать учетную запись Facebook в приложения и убедитесь, что можно добавить новую дату рождения и сведения о профиле родного города.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-204">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="cc3a8-205">Из **обозревателе решений**, нажмите кнопку **Показать все файлы** значок, а затем щелкните правой кнопкой мыши *добавить\_Data\aspnet-MvcAuth -&lt;метки даты&gt;.mdf* и нажмите кнопку **удалить**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-205">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="cc3a8-206">Из **средства** меню, щелкните **диспетчера пакетов NuGet**, затем нажмите кнопку **консоль диспетчера пакетов** (PMC).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-206">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="cc3a8-207">Введите следующие команды в PMC.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-207">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="cc3a8-208">Enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="cc3a8-208">Enable-Migrations</span></span>
2. <span data-ttu-id="cc3a8-209">Add-Migration Init</span><span class="sxs-lookup"><span data-stu-id="cc3a8-209">Add-Migration Init</span></span>
3. <span data-ttu-id="cc3a8-210">Обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="cc3a8-210">Update-Database</span></span>

<span data-ttu-id="cc3a8-211">Запустите приложение и используйте FaceBook и Google для входа и регистрации некоторых пользователей.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-211">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="cc3a8-212">Изучите данные членства</span><span class="sxs-lookup"><span data-stu-id="cc3a8-212">Examine the Membership Data</span></span>

<span data-ttu-id="cc3a8-213">В **представление** меню, щелкните **обозревателя серверов**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-213">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="cc3a8-214">Щелкните правой кнопкой мыши **AspNetUsers** и нажмите кнопку **Показать таблицу данных**.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-214">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="cc3a8-215">`HomeTown` И `BirthDate` поля, показаны ниже.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-215">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="cc3a8-216">Входа в систему с другой учетной записи и выход из приложения</span><span class="sxs-lookup"><span data-stu-id="cc3a8-216">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="cc3a8-217">Если вход в приложение с помощью Facebook и затем выйдите и попробуйте войти снова использовать другую учетную запись Facebook (используя тот же самый браузер), вам будет немедленно войти в предыдущей учетной записи Facebook, которая использовалась.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-217">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="cc3a8-218">Чтобы использовать другую учетную запись, необходимо перейти к Facebook и выхода в Facebook.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-218">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="cc3a8-219">То же правило применяется к любой другой сторонний поставщик проверки подлинности субъекта.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-219">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="cc3a8-220">Кроме того можно войти с использованием другой учетной записи, используя другой браузер.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-220">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc3a8-221">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="cc3a8-221">Next Steps</span></span>

<span data-ttu-id="cc3a8-222">См. в разделе [введение поставщиков безопасности Yahoo и LinkedIn OAuth для OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) по Pelser Джерри инструкции Yahoo и LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-222">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="cc3a8-223">См. в разделе Джерри довольно кнопки входа из социальных сетей для ASP.NET MVC 5, чтобы получить Включение кнопки входа из социальных сетей.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-223">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="cc3a8-224">Следуйте указаниям руководства, Мой [Создание приложения ASP.NET MVC с проверкой подлинности и база данных SQL и развертывание в службе приложений Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), который продолжает этом руководстве и отображаются следующие:</span><span class="sxs-lookup"><span data-stu-id="cc3a8-224">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="cc3a8-225">Как развернуть приложение в Azure.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-225">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="cc3a8-226">Как защитить, это приложение с помощью ролей.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-226">How to secure you app with roles.</span></span>
3. <span data-ttu-id="cc3a8-227">Как защитить приложения с помощью [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) и [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) фильтры.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-227">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="cc3a8-228">Как использовать API членства для добавления пользователей и ролей.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-228">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="cc3a8-229">Оставьте свои отзывы на том, как вам понравилось, и этот учебник, и что можно улучшить.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-229">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="cc3a8-230">Можно также запросить новые темы на [показать мне как с помощью кода](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-230">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="cc3a8-231">Можно запросить и проголосовать за новые функции, добавляемых к ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-231">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="cc3a8-232">Например, вы можете проголосовать за это средство для [создания и управления пользователями и ролями.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="cc3a8-232">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="cc3a8-233">Хорошее объяснение принципов работы внешние службы проверки подлинности ASP.NET, см. в разделе Robert McMurray [внешние службы проверки подлинности](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="cc3a8-233">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="cc3a8-234">Статью Роберта также подробно реализации функции аутентификации Microsoft и Twitter.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-234">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="cc3a8-235">Том Дайкстра отличную [руководство по EF и MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) показано, как работать с платформой Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cc3a8-235">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
