---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Вход с использованием внешних сайтов в ASP.NET Web Pages сайта (Razor) | Документы Microsoft
author: tfitzmac
description: В этой статье объясняется, как выполнить вход на сайт веб-страниц ASP.NET (Razor), используя Facebook, Google, Twitter, Yahoo и других сайтов, то есть, как обеспечить поддержку...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530173"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="ef204-103">Вход с использованием внешних сайтов в веб-страницы (Razor) узла ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ef204-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="ef204-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ef204-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ef204-105">В этой статье объясняется, как выполнить вход на сайт веб-страниц ASP.NET (Razor), используя Facebook, Google, Twitter, Yahoo и других сайтов — то есть, как обеспечить поддержку OAuth и OpenID в веб-узла.</span><span class="sxs-lookup"><span data-stu-id="ef204-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="ef204-106">Что вы узнаете следующее.</span><span class="sxs-lookup"><span data-stu-id="ef204-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="ef204-107">Как включить имя входа с других узлов при использовании шаблона WebMatrix начального сайта.</span><span class="sxs-lookup"><span data-stu-id="ef204-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="ef204-108">Это функция ASP.NET, представленные в статье:</span><span class="sxs-lookup"><span data-stu-id="ef204-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="ef204-109">`OAuthWebSecurity` Вспомогательные.</span><span class="sxs-lookup"><span data-stu-id="ef204-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ef204-110">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="ef204-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ef204-111">Веб-страниц ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="ef204-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="ef204-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="ef204-112">WebMatrix 3</span></span>

<span data-ttu-id="ef204-113">Веб-страниц ASP.NET включает поддержку [OAuth](http://oauth.net/) и [OpenID](http://openid.net/) поставщиков.</span><span class="sxs-lookup"><span data-stu-id="ef204-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="ef204-114">С помощью этих поставщиков, можно позволить пользователям входить на сайт с помощью существующих учетных данных от Facebook, Twitter, корпорация Майкрософт и Google.</span><span class="sxs-lookup"><span data-stu-id="ef204-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="ef204-115">Например выполнить вход с использованием учетной записи Facebook, пользователи так же можно выбрать значок Facebook, который перенаправляет их на страницу входа в Facebook, которой требуется ввести учетные данные.</span><span class="sxs-lookup"><span data-stu-id="ef204-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="ef204-116">Их можно связать имя входа Facebook с помощью учетной записи на узле.</span><span class="sxs-lookup"><span data-stu-id="ef204-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="ef204-117">Улучшение для функции членства веб-страницы –, пользователи могут связать несколько учетных записей (в том числе имена входа из сайты социальных сетей), использовать одну учетную запись на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="ef204-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="ef204-118">На данном рисунке показан на страницу входа из **начального сайта** шаблона, где пользователь сможет выбирать значок, Facebook, Twitter, Google или Microsoft, чтобы включить вход внешнюю учетную запись:</span><span class="sxs-lookup"><span data-stu-id="ef204-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![внешних поставщиков](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="ef204-120">Можно включить членство OAuth и OpenID Раскомментировать несколько строк кода в **начального сайта** шаблона.</span><span class="sxs-lookup"><span data-stu-id="ef204-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="ef204-121">Методы и свойства, которые позволяют работать с OAuth и OpenID поставщики находятся в `WebMatrix.Security.OAuthWebSecurity` класса.</span><span class="sxs-lookup"><span data-stu-id="ef204-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="ef204-122">**Начального сайта** шаблон включает в себя инфраструктуры полнофункционального членства, со страницы входа, база данных членства и весь код необходимо разрешить пользователям входить на сайт, используя учетные данные локального или из другого сайта .</span><span class="sxs-lookup"><span data-stu-id="ef204-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="ef204-123">В этом разделе приведен пример того, как разрешить пользователям входить в систему с внешних сайтов с сайтом, на основе **начального сайта** шаблона.</span><span class="sxs-lookup"><span data-stu-id="ef204-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="ef204-124">После создания начального сайта, выполните (выполните подробные сведения):</span><span class="sxs-lookup"><span data-stu-id="ef204-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="ef204-125">Для узлов, использующих поставщик OAuth (Facebook, Twitter и Microsoft) создать приложение на внешние веб-узле.</span><span class="sxs-lookup"><span data-stu-id="ef204-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="ef204-126">Этот метод позволяет приложения ключи, необходимые для вызова компонента имени входа для этих сайтов.</span><span class="sxs-lookup"><span data-stu-id="ef204-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="ef204-127">Для узлов, использующих поставщик OpenID (Google) у вас для создания приложения.</span><span class="sxs-lookup"><span data-stu-id="ef204-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="ef204-128">Все эти веб-сайты необходимо настроить учетную запись для входа и создавать приложения для разработчиков.</span><span class="sxs-lookup"><span data-stu-id="ef204-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ef204-129">Приложения Майкрософт только принимает динамической URL-адрес для работающего веб-сайта, поэтому нельзя использовать URL-адрес локального веб-сайта для тестирования имена входа.</span><span class="sxs-lookup"><span data-stu-id="ef204-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="ef204-130">Изменение нескольких файлов веб-сайта для отправки на сайт, который вы хотите использовать имя входа и задайте поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="ef204-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="ef204-131">В этой статье приведены отдельные инструкции для выполнения следующих задач:</span><span class="sxs-lookup"><span data-stu-id="ef204-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="ef204-132">Включение входа Google</span><span class="sxs-lookup"><span data-stu-id="ef204-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="ef204-133">Включение входа Facebook</span><span class="sxs-lookup"><span data-stu-id="ef204-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="ef204-134">Включение входа Twitter</span><span class="sxs-lookup"><span data-stu-id="ef204-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="ef204-135">Включение входа Google</span><span class="sxs-lookup"><span data-stu-id="ef204-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="ef204-136">Создайте или откройте узел веб-страниц ASP.NET, который основан на шаблоне начального сайта WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="ef204-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="ef204-137">Откройте  *\_AppStart.cshtml* страницы и раскомментируйте следующую строку кода.</span><span class="sxs-lookup"><span data-stu-id="ef204-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="ef204-138">Тестирование имя входа Google</span><span class="sxs-lookup"><span data-stu-id="ef204-138">Testing Google login</span></span>

1. <span data-ttu-id="ef204-139">Запустите *default.cshtml* страницы веб-узла и выберите **вход** кнопки.</span><span class="sxs-lookup"><span data-stu-id="ef204-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="ef204-140">На *входа* страницы в **используется другой службой для входа в** выберите либо **Google** или **Yahoo** кнопка отправки.</span><span class="sxs-lookup"><span data-stu-id="ef204-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="ef204-141">В этом примере используется имя входа Google.</span><span class="sxs-lookup"><span data-stu-id="ef204-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="ef204-142">Веб-страница перенаправляет запрос на страницу входа Google.</span><span class="sxs-lookup"><span data-stu-id="ef204-142">The web page redirects the request to the Google login page.</span></span>

    ![Вход Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="ef204-144">Введите учетные данные существующей учетной записи Google.</span><span class="sxs-lookup"><span data-stu-id="ef204-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="ef204-145">Если Google вопросом, следует ли разрешить *Localhost* использовать информацию из учетной записи, нажмите кнопку **Разрешить**.</span><span class="sxs-lookup"><span data-stu-id="ef204-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="ef204-146">Код проверки подлинности пользователя с помощью токена Google и возвращается на эту страницу на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="ef204-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="ef204-147">Эта страница позволяет связать с существующей учетной записи на веб-сайте своего имени входа Google или они могут зарегистрировать новую учетную запись на сайте, связываемый с внешнее имя входа.</span><span class="sxs-lookup"><span data-stu-id="ef204-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="ef204-149">Выберите **связать** кнопки.</span><span class="sxs-lookup"><span data-stu-id="ef204-149">Choose the **Associate** button.</span></span> <span data-ttu-id="ef204-150">Браузер возвращается на домашней странице приложения.</span><span class="sxs-lookup"><span data-stu-id="ef204-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="ef204-151">Включение входа Facebook</span><span class="sxs-lookup"><span data-stu-id="ef204-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="ef204-152">Последовательно выберите пункты [сайт разработчиков Facebook](https://developers.facebook.com/apps) (вход, если вы еще не вошли).</span><span class="sxs-lookup"><span data-stu-id="ef204-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="ef204-153">Выберите **создать новое приложение** кнопку, а затем следуйте инструкциям на экране, чтобы задать имя и создать новое приложение.</span><span class="sxs-lookup"><span data-stu-id="ef204-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="ef204-154">В разделе **выберите интеграции приложения с Facebook**, выберите **веб-сайт** раздела.</span><span class="sxs-lookup"><span data-stu-id="ef204-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="ef204-155">Заполните **URL-адрес сайта** поле URL-адрес веб-узла (например, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="ef204-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="ef204-156">**Домена** поле является необязательным; это можно использовать для проверки подлинности для всего домена (например, *example.com*).</span><span class="sxs-lookup"><span data-stu-id="ef204-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="ef204-157">При выполнении на локальном компьютере с URL-адрес сайта, например `http://localhost:12345` (где номер является номером локального порта), можно добавить это значение, чтобы **URL-адрес сайта** для тестирования веб-узла.</span><span class="sxs-lookup"><span data-stu-id="ef204-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="ef204-158">Однако каждый раз номер порта изменения локального узла, будет необходимо обновить **URL-адрес сайта** поле приложения.</span><span class="sxs-lookup"><span data-stu-id="ef204-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="ef204-159">Выберите **сохранить изменения** кнопки.</span><span class="sxs-lookup"><span data-stu-id="ef204-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="ef204-160">Выберите **приложений** вкладку еще раз, а затем просмотрите начальную страницу для приложения.</span><span class="sxs-lookup"><span data-stu-id="ef204-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="ef204-161">Копировать **идентификатор приложения** и **секрет приложения** значения для вашего приложения и вставьте их в временного текстового файла.</span><span class="sxs-lookup"><span data-stu-id="ef204-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="ef204-162">Вы передаете эти значения к поставщику Facebook в коде веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="ef204-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="ef204-163">Выйдите из сайта разработчика Facebook.</span><span class="sxs-lookup"><span data-stu-id="ef204-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="ef204-164">Теперь внесении изменений на две страницы веб-сайта, чтобы пользователи смогут войти на сайт, используя учетную запись Facebook.</span><span class="sxs-lookup"><span data-stu-id="ef204-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="ef204-165">Создайте или откройте узел веб-страниц ASP.NET, который основан на шаблоне начального сайта WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="ef204-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="ef204-166">Откройте  *\_AppStart.cshtml* страницы и раскомментируйте код для поставщика Facebook OAuth.</span><span class="sxs-lookup"><span data-stu-id="ef204-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="ef204-167">Блок uncommented кода выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ef204-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="ef204-168">Копировать **идентификатор приложения** значение из приложения Facebook в качестве значения `appId` параметра (внутри кавычек).</span><span class="sxs-lookup"><span data-stu-id="ef204-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="ef204-169">Копировать **секрет приложения** значение из приложения Facebook в качестве `appSecret` значение параметра.</span><span class="sxs-lookup"><span data-stu-id="ef204-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="ef204-170">Сохраните и закройте файл.</span><span class="sxs-lookup"><span data-stu-id="ef204-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="ef204-171">Проверка имени входа Facebook</span><span class="sxs-lookup"><span data-stu-id="ef204-171">Testing Facebook login</span></span>

1. <span data-ttu-id="ef204-172">Запустите на сайте *default.cshtml* странице и выберите **входа** кнопки.</span><span class="sxs-lookup"><span data-stu-id="ef204-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="ef204-173">На *входа* страницы в **используется другой службой для входа в** выберите **Facebook** значок.</span><span class="sxs-lookup"><span data-stu-id="ef204-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="ef204-174">Веб-страница перенаправляет запрос на страницу входа в Facebook.</span><span class="sxs-lookup"><span data-stu-id="ef204-174">The web page redirects the request to the Facebook login page.</span></span>

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="ef204-176">Войдите в учетную запись Facebook.</span><span class="sxs-lookup"><span data-stu-id="ef204-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="ef204-177">Код использует маркер Facebook для проверки подлинности пользователя и затем возвращает страницы, где можно связать имя входа Facebook с именем входа веб-узла.</span><span class="sxs-lookup"><span data-stu-id="ef204-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="ef204-178">Адрес электронной почты пользователя заполняется в **электронной почты** поля в форме.</span><span class="sxs-lookup"><span data-stu-id="ef204-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="ef204-180">Выберите **связать** кнопки.</span><span class="sxs-lookup"><span data-stu-id="ef204-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="ef204-181">Вы вошли в браузере возвращает на домашнюю страницу</span><span class="sxs-lookup"><span data-stu-id="ef204-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="ef204-182">Включение входа Twitter</span><span class="sxs-lookup"><span data-stu-id="ef204-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="ef204-183">Перейдите к [сайт разработчиков Twitter](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="ef204-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="ef204-184">Выберите **создать приложение** связь, а затем войдите на сайт.</span><span class="sxs-lookup"><span data-stu-id="ef204-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="ef204-185">На **создать приложение** форме, заполните **имя** и **описание** поля.</span><span class="sxs-lookup"><span data-stu-id="ef204-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="ef204-186">В **веб-сайт** введите URL-адрес веб-узла (например, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="ef204-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="ef204-187">Если вы тестируете веб-узла локально (с помощью URL-адрес: `http://localhost:12345`), Twitter, не может принимать URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="ef204-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="ef204-188">Тем не менее, можно будет использовать локальный петлевой IP-адрес (например `http://127.0.0.1:12345`).</span><span class="sxs-lookup"><span data-stu-id="ef204-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="ef204-189">Это упрощает процесс тестирования приложения локально.</span><span class="sxs-lookup"><span data-stu-id="ef204-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="ef204-190">Однако каждый раз при изменении номера порта на локальный сайт, необходимо будет обновить **веб-сайт** поле приложения.</span><span class="sxs-lookup"><span data-stu-id="ef204-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="ef204-191">В **URL-адрес обратного вызова** введите URL-адрес для страницы в веб-сайта, будет отображаться для возвращения к после входа в Twitter.</span><span class="sxs-lookup"><span data-stu-id="ef204-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="ef204-192">Например, чтобы перенаправить пользователей на домашней странице начального узла (который распознает их состояние вошедшего в систему), введите же URL-адреса, введенного в **веб-сайт** поля.</span><span class="sxs-lookup"><span data-stu-id="ef204-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="ef204-193">Принять условия соглашения и выбрать **создания приложения Twitter** кнопки.</span><span class="sxs-lookup"><span data-stu-id="ef204-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="ef204-194">На **Мои приложения** целевой страницы, выберите приложение, вы создали.</span><span class="sxs-lookup"><span data-stu-id="ef204-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="ef204-195">На **сведения** , прокрутите вниз и выберите **создать токен доступа Мой** кнопки.</span><span class="sxs-lookup"><span data-stu-id="ef204-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="ef204-196">На **сведения** вкладки, скопируйте **ключ потребителя** и **секрет пользователя** значения для вашего приложения и вставьте их в временного текстового файла.</span><span class="sxs-lookup"><span data-stu-id="ef204-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="ef204-197">В коде веб-сайт будет передачи этих значений для поставщика Twitter.</span><span class="sxs-lookup"><span data-stu-id="ef204-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="ef204-198">Выйдите из сайта Twitter.</span><span class="sxs-lookup"><span data-stu-id="ef204-198">Exit the Twitter site.</span></span>

<span data-ttu-id="ef204-199">Теперь можно внести изменения две страницы в веб-сайта, чтобы пользователи смогут Войдите на сайт, используя учетную запись Twitter.</span><span class="sxs-lookup"><span data-stu-id="ef204-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="ef204-200">Создайте или откройте узел веб-страниц ASP.NET, который основан на шаблоне начального сайта WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="ef204-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="ef204-201">Откройте  *\_AppStart.cshtml* страницы и раскомментируйте код для поставщика Twitter OAuth.</span><span class="sxs-lookup"><span data-stu-id="ef204-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="ef204-202">Блок uncommented кода выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ef204-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="ef204-203">Копировать **ключ потребителя** значение из приложения Twitter в качестве значения `consumerKey` параметра (внутри кавычек).</span><span class="sxs-lookup"><span data-stu-id="ef204-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="ef204-204">Копировать **секрет пользователя** значение из приложения Twitter в качестве значения `consumerSecret` параметра.</span><span class="sxs-lookup"><span data-stu-id="ef204-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="ef204-205">Сохраните и закройте файл.</span><span class="sxs-lookup"><span data-stu-id="ef204-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="ef204-206">Тестирование входа Twitter</span><span class="sxs-lookup"><span data-stu-id="ef204-206">Testing Twitter login</span></span>

1. <span data-ttu-id="ef204-207">Запустите *default.cshtml* страницы веб-узла и выберите **входа** кнопки.</span><span class="sxs-lookup"><span data-stu-id="ef204-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="ef204-208">На *входа* страницы в **используется другой службой для входа в** выберите **Twitter** значок.</span><span class="sxs-lookup"><span data-stu-id="ef204-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="ef204-209">Веб-страница перенаправляет запрос на страницу входа для приложения, которое вы создали Twitter.</span><span class="sxs-lookup"><span data-stu-id="ef204-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![OAuth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="ef204-211">Войдите в учетную запись Twitter.</span><span class="sxs-lookup"><span data-stu-id="ef204-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="ef204-212">Код использует маркер Twitter для проверки подлинности пользователя и последующий возврат на страницу можно связать имя входа с учетной записью веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="ef204-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="ef204-213">Ваш адрес электронной почты будет заполняться в **электронной почты** поля в форме.</span><span class="sxs-lookup"><span data-stu-id="ef204-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="ef204-215">Выберите **связать** кнопки.</span><span class="sxs-lookup"><span data-stu-id="ef204-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="ef204-216">Вы вошли в браузере возвращает на домашнюю страницу</span><span class="sxs-lookup"><span data-stu-id="ef204-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="ef204-217">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ef204-217">Additional Resources</span></span>


- [<span data-ttu-id="ef204-218">Настройка поведения сайта</span><span class="sxs-lookup"><span data-stu-id="ef204-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="ef204-219">Добавление безопасности и членство в веб-страниц сайта ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ef204-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
