---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Создание MVC 5 приложения с помощью Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on (C#) | Документация Майкрософт
author: Rick-Anderson
description: Этот учебник показывает, как создавать веб-приложения ASP.NET MVC 5, который позволяет пользователям выполнять вход с помощью OAuth 2.0 с учетными данными из внешних проверка...
ms.author: aspnetcontent
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 6af4990f726bfcd0c45eb6991418661f9b8ccbf6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824710"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="bf8f2-103">Создать приложение ASP.NET MVC 5 с помощью Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on (C#)</span><span class="sxs-lookup"><span data-stu-id="bf8f2-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="bf8f2-104">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="bf8f2-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="bf8f2-105">Этом руководстве показано, как построить веб-приложение ASP.NET MVC 5, которая позволяет пользователям входить в систему с помощью [OAuth 2.0](http://oauth.net/2/) с учетными данными от поставщика внешней проверки подлинности, таких как Facebook, Twitter, LinkedIn, Microsoft или Google.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="bf8f2-106">Для простоты это руководство посвящено работе с учетными данными с Facebook и Google.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="bf8f2-107">Включение эти учетные данные в веб-сайтов обеспечивает значительное преимущество, так как миллионам пользователей уже есть учетные записи с помощью этих внешних поставщиков.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="bf8f2-108">Эти пользователи могут быть более необходимо, чтобы зарегистрироваться для веб-узла, если они не нужно создавать и запоминать новый набор учетных данных.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="bf8f2-109">См. также [приложения ASP.NET MVC 5 с помощью SMS и электронной почты двухфакторной проверки подлинности](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="bf8f2-110">В руководстве также показано, как добавить данные профиля пользователя, а также как использовать API членства для добавления ролей.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="bf8f2-111">Это руководство было написано с [Рик Андерсон](https://blogs.msdn.com/rickAndy) (выполните мне в Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="bf8f2-112">Начало работы</span><span class="sxs-lookup"><span data-stu-id="bf8f2-112">Getting Started</span></span>

<span data-ttu-id="bf8f2-113">Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="bf8f2-114">Установка Visual Studio [2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390521) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="bf8f2-115">Получить справку по Dropbox, GitHub, Linkedin, Instagram, буфер, Salesforce, поток данных, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! и многое другое, см. в этом [пример проекта](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="bf8f2-116">Необходимо установить Visual Studio [2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390521) или более поздней версии с помощью Google OAuth 2 и отлаживать локально без предупреждений SSL.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="bf8f2-117">Нажмите кнопку **новый проект** из **запустить** страницы, или можно использовать меню и выберите **файл**, а затем **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="bf8f2-118">Создание первого приложения</span><span class="sxs-lookup"><span data-stu-id="bf8f2-118">Creating Your First Application</span></span>

<span data-ttu-id="bf8f2-119">Нажмите кнопку **новый проект**, а затем выберите **Visual C#** слева, затем **Web** , а затем выберите **веб-приложение ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="bf8f2-120">Имя проекта «MvcAuth» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="bf8f2-121">В **новый проект ASP.NET** диалоговое окно, нажмите кнопку **MVC**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="bf8f2-122">Если проверка подлинности не **учетные записи отдельных пользователей**, нажмите кнопку **изменить способ проверки подлинности** и выберите **учетные записи отдельных пользователей**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="bf8f2-123">Проверив **разместить в облаке**, приложение будет очень легко разместить в Azure.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="bf8f2-124">Если вы выбрали **разместить в облаке**, заполните поля диалогового окна настройки.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="bf8f2-125">Используйте NuGet для обновления до последней по промежуточного слоя OWIN</span><span class="sxs-lookup"><span data-stu-id="bf8f2-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="bf8f2-126">Используйте диспетчер пакетов NuGet для обновления [по промежуточного слоя OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="bf8f2-127">Выберите **обновления** в меню слева.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="bf8f2-128">Вы можете щелкнуть **обновить все** кнопку можно искать только пакеты OWIN (показано на рисунке ниже):</span><span class="sxs-lookup"><span data-stu-id="bf8f2-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="bf8f2-129">В приведенном ниже рисунке показаны только пакеты OWIN.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="bf8f2-130">Из консоли диспетчера пакетов (PMC), можно ввести `Update-Package` команду, которая будет обновить все пакеты.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="bf8f2-131">Нажмите клавишу **F5** или **Ctrl + F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="bf8f2-132">На рисунке ниже номер порта — 1234.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="bf8f2-133">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="bf8f2-134">В зависимости от размера окна браузера может потребоваться щелкнуть значок навигации, чтобы см. в разделе **Главная**, **о**, **контакт**, **зарегистрировать**и **вход** ссылки.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="bf8f2-135">Настройка SSL в проекте</span><span class="sxs-lookup"><span data-stu-id="bf8f2-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="bf8f2-136">Чтобы подключиться к поставщикам проверки подлинности, таких как Google и Facebook, необходимо настроить IIS Express для использования протокола SSL.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="bf8f2-137">Необходимо продолжать использовать SSL после входа в систему и не удалять обратно к HTTP, ваш файл cookie входа — так же, как секрет, как имя пользователя и пароль и без использования SSL, вы отправляете его в открытый текст по каналу связи.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="bf8f2-138">Кроме того, вы уже сделали время для выполнения установки соединения и обеспечьте безопасность канала (это основная часть делает HTTPS медленнее, чем HTTP) перед запуском конвейера MVC, поэтому перенаправление обратно HTTP после входа не текущего запроса или сделать будущее запросы гораздо быстрее.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="bf8f2-139">В **обозревателе решений**, нажмите кнопку **MvcAuth** проекта.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="bf8f2-140">Нажмите клавишу F4, чтобы показать свойства проекта.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="bf8f2-141">Кроме того, из **представление** меню можно выбрать **окно "Свойства"**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="bf8f2-142">Изменение **SSL включен** значение true.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="bf8f2-143">Скопируйте URL-адрес SSL (который может быть `https://localhost:44300/` Если вы создали другие проекты SSL).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="bf8f2-144">В **обозревателе решений**, щелкните правой кнопкой мыши **MvcAuth** проекта и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="bf8f2-145">Выберите **Web** вкладке, а затем вставьте URL-адрес SSL в **URL-адрес проекта** поле.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="bf8f2-146">Сохраните файл (Ctl + S).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="bf8f2-147">Вам потребуется этот URL-адрес для настройки проверки подлинности приложения Facebook и Google.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="bf8f2-148">Добавить [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) атрибут `Home` контроллера для всех запросов необходимо использовать протокол HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="bf8f2-149">Более безопасный подход заключается в добавлении [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) фильтр к приложению.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="bf8f2-150">См. в разделе &quot;защиты приложения с помощью SSL и атрибута авторизации&quot; в моей tutoral [Создание приложения ASP.NET MVC с проверкой подлинности и база данных SQL и развертывание в службе приложений Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="bf8f2-151">Ниже приведен фрагмент контроллера Home.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="bf8f2-152">Нажмите CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="bf8f2-153">Если вы установили сертификат в прошлом, вы можете пропустить оставшейся части этого раздела и перейти к [Создание приложения Google для oauth2 и подключение приложения к проекту](#goog), в противном случае следуйте инструкциям, чтобы доверия к самозаверяющему сертификат, созданный IIS Express.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="bf8f2-154">Чтение **предупреждение системы безопасности** диалоговое окно, а затем нажмите кнопку **Да** Если вы хотите установить сертификат, представляющий localhost.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="bf8f2-155">IE откроется *Главная* страницы и без предупреждений SSL.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="bf8f2-156">Google Chrome также принимает сертификат и будет показано содержимое HTTPS без предупреждения.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="bf8f2-157">Firefox использует собственное хранилище сертификатов, поэтому он будет отображаться предупреждение.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="bf8f2-158">Для нашего приложения могут безопасно щелкните **я понимаю последствия**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="bf8f2-159">Создание приложения Google для oauth2 и подключение приложения к проекту</span><span class="sxs-lookup"><span data-stu-id="bf8f2-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="bf8f2-160">Актуальные инструкции Google OAuth, см. в разделе [Google Настройка проверки подлинности в ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="bf8f2-161">Перейдите к [Google Developers Console](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="bf8f2-162">Если вы еще не создали проект перед, выберите **учетные данные** на левой вкладке, а затем выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="bf8f2-163">На левой вкладке щелкните **учетные данные**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="bf8f2-164">Нажмите кнопку **создайте учетные данные** затем **идентификатор клиента OAuth**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="bf8f2-165">В **Создание идентификатора клиента** диалоговое окно, оставьте значение по умолчанию **веб-приложение** для типа приложения.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="bf8f2-166">Задайте **авторизованных JavaScript** источников на приведенном выше URL-адрес SSL (`https://localhost:44300/` Если вы создали другие проекты SSL)</span><span class="sxs-lookup"><span data-stu-id="bf8f2-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="bf8f2-167">Задайте **авторизованные URI перенаправления** для:</span><span class="sxs-lookup"><span data-stu-id="bf8f2-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="bf8f2-168">Щелкните пункт меню экран согласия OAuth, а затем задайте адрес и продукта имени электронной почты.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="bf8f2-169">После завершения щелкните форму **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="bf8f2-170">Пункт меню библиотеки, поиска **Google + API**, щелкните его, а затем нажмите клавишу Enable.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="bf8f2-171">На рисунке ниже показана включенных API-интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="bf8f2-172">Посетите из API-интерфейсов API диспетчера Google, **учетные данные** tab, чтобы получить **идентификатор клиента**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="bf8f2-173">Загрузки, чтобы сохранить файл JSON с секретными данными приложения.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="bf8f2-174">Скопируйте и вставьте **ClientId** и **ClientSecret** в `UseGoogleAuthentication` найти метод в *Startup.Auth.cs* файл *App_Start* папки.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="bf8f2-175">**ClientId** и **ClientSecret** значений, приведенных ниже примеров и не работают.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="bf8f2-176">Безопасность — никогда не сохраняйте конфиденциальные данные в исходном коде.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="bf8f2-177">Запись и учетные данные добавляются в код выше для простоты в примере.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="bf8f2-178">См. в разделе [советы и рекомендации по развертыванию паролей и других конфиденциальных данных в ASP.NET и службы приложений Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="bf8f2-179">Нажмите клавишу **CTRL + F5** для сборки и запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="bf8f2-180">Нажмите кнопку **вход** ссылку.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="bf8f2-181">В разделе **вход с помощью другой службы**, нажмите кнопку **Google**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="bf8f2-182">Если вы упустите какие-либо шаги выше вы получите ошибку HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="bf8f2-183">Проверьте выполненные действия выше.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-183">Recheck your steps above.</span></span> <span data-ttu-id="bf8f2-184">Если вы пропустите обязательный параметр (например **название продукта**), чтобы добавить отсутствующий элемент и сохраните; может занять несколько минут для проверки подлинности для работы.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="bf8f2-185">Вы будете перенаправлены на сайт Google, где можно будет ввести свои учетные данные.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="bf8f2-186">После того как вы введете свои учетные данные, вам будет предложено предоставить разрешения на доступ к веб-приложение, которое вы только что создали:</span><span class="sxs-lookup"><span data-stu-id="bf8f2-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="bf8f2-187">Нажмите кнопку **принимать**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-187">Click **Accept**.</span></span> <span data-ttu-id="bf8f2-188">Вы будете перенаправлены обратно **зарегистрировать** страница MvcAuth приложения, где можно зарегистрировать учетную запись Google.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="bf8f2-189">Вы можете изменить имя регистрации локальной электронной почты для учетной записи Gmail, но обычно требуется сохранить псевдоним электронной почты по умолчанию (то есть той, которая используется для проверки подлинности).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="bf8f2-190">Щелкните ссылку **Регистрация**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="bf8f2-191">Создать приложение в Facebook и подключить приложение к проекту</span><span class="sxs-lookup"><span data-stu-id="bf8f2-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="bf8f2-192">Актуальные инструкции для проверки подлинности Facebook OAuth2, см. в разделе [проверки подлинности Facebook, Настройка](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="bf8f2-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<span data-ttu-id="bf8f2-193">Для проверки подлинности Facebook OAuth2 вам нужно скопировать в свой проект некоторые параметры из приложения, создаваемые в Facebook.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-193">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="bf8f2-194">В окне браузера перейдите к [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) и войдите в систему, введя свои учетные данные Facebook.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-194">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="bf8f2-195">Если вы еще не зарегистрировался в качестве разработчика Facebook, щелкните **регистрация в качестве разработчика** и следуйте инструкциям для регистрации.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-195">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="bf8f2-196">На **приложений** щелкните **создать новое приложение**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-196">On the **Apps** tab, click **Create New App**.</span></span>

    ![Создание нового приложения](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="bf8f2-198">Введите **имя_приложения** и **категории**, затем нажмите кнопку **создать приложение**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-198">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="bf8f2-199"><strong>Пространство имен приложения</strong> является частью URL-адрес, приложение будет использовать для доступа к приложению Facebook для проверки подлинности (например, https\://apps.facebook.com/{App пространство имен}).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-199">The <strong>App Namespace</strong> is the part of the URL that your App will use to access the Facebook application for authentication (for example, https\://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="bf8f2-200">Если вы не укажете <strong>пространство имен приложения</strong>, <strong>идентификатор приложения</strong> будет использоваться в качестве URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-200">If you don't specify an <strong>App Namespace</strong>, the <strong>App ID</strong> will be used for the URL.</span></span> <span data-ttu-id="bf8f2-201"><strong>Идентификатор приложения</strong> долго системой номер, который вы увидите на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-201">The <strong>App ID</strong> is a long system-generated number that you will see in the next step.</span></span>

    ![Создать новое приложение](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="bf8f2-203">Отправьте проверку безопасности standard.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-203">Submit the standard security check.</span></span>

    ![Проверка безопасности](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="bf8f2-205">Выберите **параметры** для панели меню слева![ Строка меню разработчика Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="bf8f2-205">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="bf8f2-206">На **основные** страницы «Параметры» выберите **Добавление платформы** для указания, что при добавлении приложения веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-206">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="bf8f2-207">![Основные параметры](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="bf8f2-207">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="bf8f2-208">Выберите **веб-сайт** из вариантов платформы.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-208">Select **Website** from the platform choices.</span></span>  
  
    ![Выбор платформы](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="bf8f2-210">Запомните или запишите вашей **идентификатор приложения** и **секрет приложения** таким образом, чтобы можно было добавить оба сертификата в приложении MVC далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-210">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="bf8f2-211">Кроме того, добавьте URL-адрес сайта (`https://localhost:44300/`) для тестирования приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-211">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="bf8f2-212">Кроме того, добавьте **контактный адрес электронной почты**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-212">Also, add a **Contact Email**.</span></span> <span data-ttu-id="bf8f2-213">Выберите **сохранить изменения**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-213">Then, select **Save Changes**.</span></span>   

    ![Странице сведений о простое приложение](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="bf8f2-215">Обратите внимание, что вы только сможете пройти проверку подлинности с помощью зарегистрированного псевдоним электронной почты.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-215">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="bf8f2-216">Не будет возможности регистрации других пользователей и тестовых учетных записей.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-216">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="bf8f2-217">Вы можете предоставить доступ учетным записям других Facebook для приложения на Facebook **роль разработчика** вкладки.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-217">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="bf8f2-218">В Visual Studio откройте *приложения\_Start\Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-218">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="bf8f2-219">Скопируйте и вставьте **AppId** и **секрет приложения** в `UseFacebookAuthentication` метод.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-219">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="bf8f2-220">**AppId** и **секрет приложения** значений, приведенных ниже примеров и не будет работать.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-220">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="bf8f2-221">Нажмите кнопку **сохранить изменения**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-221">Click **Save Changes**.</span></span>
13. <span data-ttu-id="bf8f2-222">Нажмите клавишу **CTRL + F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-222">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="bf8f2-223">Выберите **вход** на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-223">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="bf8f2-224">Нажмите кнопку **Facebook** под **вход с помощью другой службы.**</span><span class="sxs-lookup"><span data-stu-id="bf8f2-224">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="bf8f2-225">Введите свои учетные данные Facebook.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-225">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="bf8f2-226">Вам будет предложено предоставить разрешение для приложения доступ к открытый профиль и списков друзей.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-226">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Сведения о приложении Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="bf8f2-228">Теперь вы вошли.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-228">You are now logged in.</span></span> <span data-ttu-id="bf8f2-229">Теперь вы можете зарегистрировать эту учетную запись с приложением.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-229">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="bf8f2-230">При регистрации, добавляется запись *пользователей* таблицы в базе данных членства.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-230">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="bf8f2-231">Изучите данные членства</span><span class="sxs-lookup"><span data-stu-id="bf8f2-231">Examine the Membership Data</span></span>

<span data-ttu-id="bf8f2-232">В **представление** меню, щелкните **обозревателя серверов**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-232">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="bf8f2-233">Разверните **DefaultConnection (MvcAuth)**, разверните **таблиц**, щелкните правой кнопкой мыши **AspNetUsers** и нажмите кнопку **Показать таблицу данных**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-233">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![данные таблицы aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="bf8f2-235">Добавление данных профиля в класс пользователя</span><span class="sxs-lookup"><span data-stu-id="bf8f2-235">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="bf8f2-236">В этом разделе вы добавите Дата рождения и родного города в пользовательских данных во время регистрации, как показано на следующем рисунке.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-236">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![REG родного города и д.рожд.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="bf8f2-238">Откройте *Models\IdentityModels.cs* файл и добавьте свойства Город Домашняя страница и даты рождения:</span><span class="sxs-lookup"><span data-stu-id="bf8f2-238">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="bf8f2-239">Откройте *Models\AccountViewModels.cs* файл и задайте рождения даты и домашней свойства города в `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-239">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="bf8f2-240">Откройте *файл Controllers\AccountController.cs* файл и добавьте код города Домашняя страница и даты рождения в `ExternalLoginConfirmation` метода действия, как показано:</span><span class="sxs-lookup"><span data-stu-id="bf8f2-240">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="bf8f2-241">Добавление даты рождения и родного города для *Views\Account\ExternalLoginConfirmation.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="bf8f2-241">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="bf8f2-242">Удаление базы данных членства, чтобы можно было снова зарегистрировать учетную запись Facebook в приложения и убедитесь, что можно добавить новую дату рождения и сведения о профиле родного города.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-242">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="bf8f2-243">Из **обозревателе решений**, нажмите кнопку **Показать все файлы** значок, а затем щелкните правой кнопкой мыши *добавить\_Data\aspnet-MvcAuth -&lt;метки даты&gt;.mdf* и нажмите кнопку **удалить**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-243">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="bf8f2-244">Из **средства** меню, щелкните **диспетчера пакетов NuGet**, затем нажмите кнопку **консоль диспетчера пакетов** (PMC).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-244">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="bf8f2-245">Введите следующие команды в PMC.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-245">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="bf8f2-246">Enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="bf8f2-246">Enable-Migrations</span></span>
2. <span data-ttu-id="bf8f2-247">Add-Migration Init</span><span class="sxs-lookup"><span data-stu-id="bf8f2-247">Add-Migration Init</span></span>
3. <span data-ttu-id="bf8f2-248">Обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="bf8f2-248">Update-Database</span></span>

<span data-ttu-id="bf8f2-249">Запустите приложение и используйте FaceBook и Google для входа и регистрации некоторых пользователей.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-249">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="bf8f2-250">Изучите данные членства</span><span class="sxs-lookup"><span data-stu-id="bf8f2-250">Examine the Membership Data</span></span>

<span data-ttu-id="bf8f2-251">В **представление** меню, щелкните **обозревателя серверов**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-251">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="bf8f2-252">Щелкните правой кнопкой мыши **AspNetUsers** и нажмите кнопку **Показать таблицу данных**.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-252">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="bf8f2-253">`HomeTown` И `BirthDate` поля, показаны ниже.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-253">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="bf8f2-254">Входа в систему с другой учетной записи и выход из приложения</span><span class="sxs-lookup"><span data-stu-id="bf8f2-254">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="bf8f2-255">Если вход в приложение с помощью Facebook и затем выйдите и попробуйте войти снова использовать другую учетную запись Facebook (используя тот же самый браузер), вам будет немедленно войти в предыдущей учетной записи Facebook, которая использовалась.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-255">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="bf8f2-256">Чтобы использовать другую учетную запись, необходимо перейти к Facebook и выхода в Facebook.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-256">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="bf8f2-257">То же правило применяется к любой другой сторонний поставщик проверки подлинности субъекта.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-257">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="bf8f2-258">Кроме того можно войти с использованием другой учетной записи, используя другой браузер.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-258">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf8f2-259">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="bf8f2-259">Next Steps</span></span>

<span data-ttu-id="bf8f2-260">См. в разделе [введение поставщиков безопасности Yahoo и LinkedIn OAuth для OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) по Pelser Джерри инструкции Yahoo и LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-260">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="bf8f2-261">См. в разделе Джерри довольно кнопки входа из социальных сетей для ASP.NET MVC 5, чтобы получить Включение кнопки входа из социальных сетей.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-261">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="bf8f2-262">Следуйте указаниям руководства, Мой [Создание приложения ASP.NET MVC с проверкой подлинности и база данных SQL и развертывание в службе приложений Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), который продолжает этом руководстве и отображаются следующие:</span><span class="sxs-lookup"><span data-stu-id="bf8f2-262">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="bf8f2-263">Как развернуть приложение в Azure.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-263">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="bf8f2-264">Как защитить, это приложение с помощью ролей.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-264">How to secure you app with roles.</span></span>
3. <span data-ttu-id="bf8f2-265">Как защитить приложения с помощью [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) и [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) фильтры.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-265">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="bf8f2-266">Как использовать API членства для добавления пользователей и ролей.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-266">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="bf8f2-267">Оставьте свои отзывы на том, как вам понравилось, и этот учебник, и что можно улучшить.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-267">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="bf8f2-268">Можно также запросить новые темы на [показать мне как с помощью кода](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-268">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="bf8f2-269">Можно запросить и проголосовать за новые функции, добавляемых к ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-269">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="bf8f2-270">Например, вы можете проголосовать за это средство для [создания и управления пользователями и ролями.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="bf8f2-270">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="bf8f2-271">Хорошее объяснение принципов работы внешние службы проверки подлинности ASP.NET, см. в разделе Robert McMurray [внешние службы проверки подлинности](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="bf8f2-271">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="bf8f2-272">Статью Роберта также подробно реализации функции аутентификации Microsoft и Twitter.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-272">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="bf8f2-273">Том Дайкстра отличную [руководство по EF и MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) показано, как работать с платформой Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bf8f2-273">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
