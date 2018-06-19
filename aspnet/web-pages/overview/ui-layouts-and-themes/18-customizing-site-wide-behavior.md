---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Настройка поведения сайта для веб-страниц ASP.NET (Razor) узлов | Документы Microsoft
author: tfitzmac
description: В этой главе объясняется, как сделать параметры вашего веб-сайты или всю папку, а не только страницы.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: 4457318bcf1d2886eb8ed68fdd795eea7905368b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899205"
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a><span data-ttu-id="f90f1-103">Настройка поведения сайта для веб-страниц (Razor) узлов ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f90f1-103">Customizing Site-Wide Behavior for ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="f90f1-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f90f1-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f90f1-105">В этой статье объясняется, как делать параметров на стороне узла для страниц веб-сайте ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="f90f1-105">This article explains how to make site-side settings for pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="f90f1-106">Что вы узнаете следующее.</span><span class="sxs-lookup"><span data-stu-id="f90f1-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="f90f1-107">Как для выполнения кода, которая позволяет вам набор значений (глобальные переменные или параметры поддержки) для всех страниц на сайте.</span><span class="sxs-lookup"><span data-stu-id="f90f1-107">How to run code that lets you set values (global values or helper settings) for all pages in a site.</span></span>
> - <span data-ttu-id="f90f1-108">Как выполнять код, который позволяет задавать значения для всех страниц в папке.</span><span class="sxs-lookup"><span data-stu-id="f90f1-108">How to run code that lets you set values for all pages in a folder.</span></span>
> - <span data-ttu-id="f90f1-109">Загружает выполнение кода до и после страницы.</span><span class="sxs-lookup"><span data-stu-id="f90f1-109">How to run code before and after a page loads.</span></span>
> - <span data-ttu-id="f90f1-110">Как отправить ошибки на странице центра ошибки.</span><span class="sxs-lookup"><span data-stu-id="f90f1-110">How to send errors to a central error page.</span></span>
> - <span data-ttu-id="f90f1-111">Инструкции по добавлению проверки подлинности для всех страниц в папке.</span><span class="sxs-lookup"><span data-stu-id="f90f1-111">How to add authentication to all pages in a folder.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f90f1-112">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="f90f1-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f90f1-113">Веб-страниц ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="f90f1-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="f90f1-114">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="f90f1-114">WebMatrix 3</span></span>
> - <span data-ttu-id="f90f1-115">Библиотека вспомогательных методов ASP.NET Web (пакет NuGet)</span><span class="sxs-lookup"><span data-stu-id="f90f1-115">ASP.NET Web Helpers Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="f90f1-116">Этот учебник также работает с 3 веб-страниц ASP.NET и Visual Studio 2013 (или Visual Studio Express 2013 для веб-сайта), за исключением того, вы не может использовать библиотека вспомогательных методов ASP.NET Web.</span><span class="sxs-lookup"><span data-stu-id="f90f1-116">This tutorial also works with ASP.NET Web Pages 3 and Visual Studio 2013 (or Visual Studio Express 2013 for Web), except you cannot use the ASP.NET Web Helpers Library.</span></span>


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a><span data-ttu-id="f90f1-117">Добавление кода для запуска веб-сайт веб-страниц ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f90f1-117">Adding Website Startup Code for ASP.NET Web Pages</span></span>

<span data-ttu-id="f90f1-118">Большую часть кода, написанного в веб-страницах ASP.NET, для отдельной страницы может содержать весь код, необходимый для данной страницы.</span><span class="sxs-lookup"><span data-stu-id="f90f1-118">For much of the code that you write in ASP.NET Web Pages, an individual page can contain all the code that's required for that page.</span></span> <span data-ttu-id="f90f1-119">Например если страница отправляет сообщение электронной почты, можно поместить весь код для этой операции на одну страницу.</span><span class="sxs-lookup"><span data-stu-id="f90f1-119">For example, if a page sends an email message, it's possible to put all the code for that operation in a single page.</span></span> <span data-ttu-id="f90f1-120">Это может включать код, чтобы инициализировать параметры для отправки электронной почты (то есть для SMTP-сервера) и для отправки сообщений электронной почты.</span><span class="sxs-lookup"><span data-stu-id="f90f1-120">This can include the code to initialize the settings for sending email (that is, for the SMTP server) and for sending the email message.</span></span>

<span data-ttu-id="f90f1-121">Однако в некоторых ситуациях может потребоваться выполнять определенный код перед выполнением любой страницы на сайте.</span><span class="sxs-lookup"><span data-stu-id="f90f1-121">However, in some situations, you might want to run some code before any page on the site runs.</span></span> <span data-ttu-id="f90f1-122">Это полезно при задании значения, которые могут использоваться в любом месте на сайте (называют *глобальные значения*.) Например некоторые вспомогательные методы требуют предоставления значения, такие как параметры электронной почты или ключи учетной записи.</span><span class="sxs-lookup"><span data-stu-id="f90f1-122">This is useful for setting values that can be used anywhere in the site (referred to as *global values*.) For example, some helpers require you to provide values like email settings or account keys.</span></span> <span data-ttu-id="f90f1-123">Он может оказаться удобным для сохранения этих параметров в глобальные значения.</span><span class="sxs-lookup"><span data-stu-id="f90f1-123">It can be handy to keep these settings in global values.</span></span>

<span data-ttu-id="f90f1-124">Это можно сделать путем создания страницы с именем  *\_AppStart.cshtml* в корневом узле.</span><span class="sxs-lookup"><span data-stu-id="f90f1-124">You can do this by creating a page named *\_AppStart.cshtml* in the root of the site.</span></span> <span data-ttu-id="f90f1-125">Если эта страница существует, оно выполняется при первом запросе любой страницы на сайте.</span><span class="sxs-lookup"><span data-stu-id="f90f1-125">If this page exists, it runs the first time any page in the site is requested.</span></span> <span data-ttu-id="f90f1-126">Таким образом это хорошая возможность запустить код, чтобы задать глобальные значения.</span><span class="sxs-lookup"><span data-stu-id="f90f1-126">Therefore, it's a good place to run code to set global values.</span></span> <span data-ttu-id="f90f1-127">(Поскольку  *\_AppStart.cshtml* имеют префикс подчеркивания ASP.NET не будет отправлять страницы в браузер, даже если он напрямую запрашивать пользователей.)</span><span class="sxs-lookup"><span data-stu-id="f90f1-127">(Because *\_AppStart.cshtml* has an underscore prefix, ASP.NET won't send the page to a browser even if users request it directly.)</span></span>

<span data-ttu-id="f90f1-128">На следующей схеме показано как  *\_AppStart.cshtml* страница работает.</span><span class="sxs-lookup"><span data-stu-id="f90f1-128">The following diagram shows how the *\_AppStart.cshtml* page works.</span></span> <span data-ttu-id="f90f1-129">При поступлении запроса на странице, и если это первый запрос любой страницы на сайте ASP.NET сначала проверяет ли  *\_AppStart.cshtml* страница существует.</span><span class="sxs-lookup"><span data-stu-id="f90f1-129">When a request comes in for a page, and if this is the first request for any page in the site, ASP.NET first checks whether a *\_AppStart.cshtml* page exists.</span></span> <span data-ttu-id="f90f1-130">Если Да, какой-либо код  *\_AppStart.cshtml* выполняется страница, а затем запускает запрошенной страницы.</span><span class="sxs-lookup"><span data-stu-id="f90f1-130">If so, any code in the *\_AppStart.cshtml* page runs, and then the requested page runs.</span></span>

![[image]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a><span data-ttu-id="f90f1-132">Установка глобальных значений для веб-сайта</span><span class="sxs-lookup"><span data-stu-id="f90f1-132">Setting Global Values for Your Website</span></span>

1. <span data-ttu-id="f90f1-133">В корневой папке веб-сайта в WebMatrix, создайте файл с именем  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f90f1-133">In the root folder of a WebMatrix website, create a file named *\_AppStart.cshtml*.</span></span> <span data-ttu-id="f90f1-134">Файл должен быть в корневом узле.</span><span class="sxs-lookup"><span data-stu-id="f90f1-134">The file must be in the root of the site.</span></span>
2. <span data-ttu-id="f90f1-135">Замените существующее содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="f90f1-135">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    <span data-ttu-id="f90f1-136">Этот код сохраняет значение в `AppState` словаря, который автоматически доступны для всех страниц на сайте.</span><span class="sxs-lookup"><span data-stu-id="f90f1-136">This code stores a value in the `AppState` dictionary, which is automatically available to all pages in the site.</span></span> <span data-ttu-id="f90f1-137">Обратите внимание, что  *\_AppStart.cshtml* файл имеет разметку в нем.</span><span class="sxs-lookup"><span data-stu-id="f90f1-137">Notice that the *\_AppStart.cshtml* file does not have any markup in it.</span></span> <span data-ttu-id="f90f1-138">Запускается код и затем перенаправление на страницу, первоначально запрошенной страницы.</span><span class="sxs-lookup"><span data-stu-id="f90f1-138">The page will run the code and then redirect to the page that was originally requested.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f90f1-139">Следует соблюдать осторожность при добавлении кода в  *\_AppStart.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="f90f1-139">Be careful when you put code in the *\_AppStart.cshtml* file.</span></span> <span data-ttu-id="f90f1-140">При появлении ошибок в коде в  *\_AppStart.cshtml* файла, веб-сайт не запускается.</span><span class="sxs-lookup"><span data-stu-id="f90f1-140">If any errors occur in code in the *\_AppStart.cshtml* file, the website won't start.</span></span>
3. <span data-ttu-id="f90f1-141">В корневой папке создайте новую страницу с именем *AppName.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f90f1-141">In the root folder, create a new page named *AppName.cshtml*.</span></span>
4. <span data-ttu-id="f90f1-142">Замените по умолчанию разметка и код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="f90f1-142">Replace the default markup and code with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    <span data-ttu-id="f90f1-143">Этот код извлекает значение из `AppState` объекта, который задается в  *\_AppStart.cshtml* страницы.</span><span class="sxs-lookup"><span data-stu-id="f90f1-143">This code extracts the value from the `AppState` object that you set in the *\_AppStart.cshtml* page.</span></span>
5. <span data-ttu-id="f90f1-144">Запустите *AppName.cshtml* страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="f90f1-144">Run the *AppName.cshtml* page in a browser.</span></span> <span data-ttu-id="f90f1-145">(Убедитесь, что выбран страницы **файлы** рабочей области, прежде чем запускать его.) На странице отображается глобальное значение.</span><span class="sxs-lookup"><span data-stu-id="f90f1-145">(Make sure the page is selected in the **Files** workspace before you run it.) The page displays the global value.</span></span> 

    ![[image]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a><span data-ttu-id="f90f1-147">Значения параметра для помощников</span><span class="sxs-lookup"><span data-stu-id="f90f1-147">Setting Values for Helpers</span></span>

<span data-ttu-id="f90f1-148">Рекомендуется использовать для  *\_AppStart.cshtml* файла является установка значений для вспомогательных методов, которые используется в веб-узла и который нужно инициализировать.</span><span class="sxs-lookup"><span data-stu-id="f90f1-148">A good use for the *\_AppStart.cshtml* file is to set values for helpers that you use in your site and that have to be initialized.</span></span> <span data-ttu-id="f90f1-149">Типичными примерами являются параметры электронной почты для `WebMail` вспомогательный метод и открытые и закрытые ключи для `ReCaptcha` модуля поддержки.</span><span class="sxs-lookup"><span data-stu-id="f90f1-149">Typical examples are email settings for the `WebMail` helper and the private and public keys for the `ReCaptcha` helper.</span></span> <span data-ttu-id="f90f1-150">В подобных случаях можно задать значения в один раз в  *\_AppStart.cshtml* и затем они всегда уже можно изменить для всех страниц веб-узла.</span><span class="sxs-lookup"><span data-stu-id="f90f1-150">In cases like these, you can set the values once in the *\_AppStart.cshtml* and then they're already set for all the pages in your site.</span></span>

<span data-ttu-id="f90f1-151">Эта процедура показывает, как задать `WebMail` параметры глобально.</span><span class="sxs-lookup"><span data-stu-id="f90f1-151">This procedure shows you how to set `WebMail` settings globally.</span></span> <span data-ttu-id="f90f1-152">(Дополнительные сведения об использовании `WebMail` вспомогательного приложения, в разделе [Добавление электронной почты для узла веб-страниц ASP.NET](../getting-started/11-adding-email-to-your-web-site.md).)</span><span class="sxs-lookup"><span data-stu-id="f90f1-152">(For more information about using the `WebMail` helper, see [Adding Email to an ASP.NET Web Pages Site](../getting-started/11-adding-email-to-your-web-site.md).)</span></span>

1. <span data-ttu-id="f90f1-153">Добавить библиотека вспомогательных методов ASP.NET Web веб-сайта, как описано в [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если не было добавлено.</span><span class="sxs-lookup"><span data-stu-id="f90f1-153">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="f90f1-154">Если у вас еще нет  *\_AppStart.cshtml* в корневой папке веб-сайта, создайте файл с именем  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f90f1-154">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
3. <span data-ttu-id="f90f1-155">Добавьте следующие `WebMail` параметры  *\_AppStart.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="f90f1-155">Add the following `WebMail` settings to the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    <span data-ttu-id="f90f1-156">Изменить связанные параметры в коде, электронной почты следующие:</span><span class="sxs-lookup"><span data-stu-id="f90f1-156">Modify the following email related settings in the code:</span></span>

   - <span data-ttu-id="f90f1-157">Задать `your-SMTP-host` имя SMTP-сервера, у вас есть право.</span><span class="sxs-lookup"><span data-stu-id="f90f1-157">Set `your-SMTP-host` to the name of the SMTP server that you have access to.</span></span>
   - <span data-ttu-id="f90f1-158">Задать `your-user-name-here` к имени пользователя для учетной записи сервера SMTP.</span><span class="sxs-lookup"><span data-stu-id="f90f1-158">Set `your-user-name-here` to the user name for your SMTP server account.</span></span>
   - <span data-ttu-id="f90f1-159">Задать `your-account-password` пароль пользователя для учетной записи сервера SMTP.</span><span class="sxs-lookup"><span data-stu-id="f90f1-159">Set `your-account-password` to the password for your SMTP server account.</span></span>
   - <span data-ttu-id="f90f1-160">Задать `your-email-address-here` на собственный адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="f90f1-160">Set `your-email-address-here` to your own email address.</span></span> <span data-ttu-id="f90f1-161">Это адрес электронной почты отправителя сообщения.</span><span class="sxs-lookup"><span data-stu-id="f90f1-161">This is the email address that the message is sent from.</span></span> <span data-ttu-id="f90f1-162">(Некоторые поставщики электронной почты не позволяют задать другую `From` адреса и будет использовать имя пользователя как `From` адрес.)</span><span class="sxs-lookup"><span data-stu-id="f90f1-162">(Some email providers don't let you specify a different `From` address and will use your user name as the `From` address.)</span></span>

     <span data-ttu-id="f90f1-163">Дополнительные сведения о параметрах SMTP см. в разделе [Настройка параметров электронной почты](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) в статье [отправки электронной почты из веб-узла ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) и [проблемы с отправкой сообщения электронной почты](https://go.microsoft.com/fwlink/?LinkId=253001#email)в [веб-страниц ASP.NET (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).</span><span class="sxs-lookup"><span data-stu-id="f90f1-163">For more information about SMTP settings, see [Configuring Email Settings](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) in the article [Sending Email from an ASP.NET Web Pages (Razor) Site](https://go.microsoft.com/fwlink/?LinkID=202899) and [Issues with Sending Email](https://go.microsoft.com/fwlink/?LinkId=253001#email) in the [ASP.NET Web Pages (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).</span></span>
4. <span data-ttu-id="f90f1-164">Сохранить  *\_AppStart.cshtml* файл и закройте его.</span><span class="sxs-lookup"><span data-stu-id="f90f1-164">Save the *\_AppStart.cshtml* file and close it.</span></span>
5. <span data-ttu-id="f90f1-165">В корневой папке веб-сайта, создайте новую страницу с именем *TestEmail.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f90f1-165">In the root folder of a website, create new page named *TestEmail.cshtml*.</span></span>
6. <span data-ttu-id="f90f1-166">Замените существующее содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="f90f1-166">Replace the existing content with the following:</span></span> 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. <span data-ttu-id="f90f1-167">Запустите *TestEmail.cshtml* страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="f90f1-167">Run the *TestEmail.cshtml* page in a browser.</span></span>
8. <span data-ttu-id="f90f1-168">Заполните поля Отправьте себе электронное сообщение, а затем нажмите кнопку **отправки**.</span><span class="sxs-lookup"><span data-stu-id="f90f1-168">Fill in the fields to send yourself an email message and then click **Send**.</span></span>
9. <span data-ttu-id="f90f1-169">Проверьте свою электронную почту, чтобы убедиться в том, что вы получили сообщение.</span><span class="sxs-lookup"><span data-stu-id="f90f1-169">Check your email to make sure you've gotten the message.</span></span>

<span data-ttu-id="f90f1-170">Важной частью в этом примере это параметры, которые не изменяют обычно — имя SMTP-сервера и учетные данные электронной почты, например — устанавливаются в  *\_AppStart.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="f90f1-170">The important part of this example is that the settings that you don't usually change — like the name of your SMTP server and your email credentials — are set in the *\_AppStart.cshtml* file.</span></span> <span data-ttu-id="f90f1-171">В этом случае не нужно повторно устанавливать их на каждой странице, куда отправить по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="f90f1-171">That way you don't need to set them again in each page where you send email.</span></span> <span data-ttu-id="f90f1-172">(Несмотря на то, что если какой-либо причине необходимо изменить эти параметры, можно задать их по отдельности на странице.) На странице только задания значений, которые обычно изменяется каждый раз, как и получателе, а текст сообщения электронной почты.</span><span class="sxs-lookup"><span data-stu-id="f90f1-172">(Although if for some reason you need to change those settings, you can set them individually in a page.) In the page, you only set the values that typically change each time, like the recipient and the body of the email message.</span></span>

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a><span data-ttu-id="f90f1-173">Выполнение кода до и после файлов в папке</span><span class="sxs-lookup"><span data-stu-id="f90f1-173">Running Code Before and After Files in a Folder</span></span>

<span data-ttu-id="f90f1-174">Так же, как можно использовать  *\_AppStart.cshtml* написания кода до запуска страницы веб-узла, можно написать код, выполняемый до (и после) любой страницы в определенной папке запущена.</span><span class="sxs-lookup"><span data-stu-id="f90f1-174">Just like you can use *\_AppStart.cshtml* to write code before pages in the site run, you can write code that runs before (and after) any page in a particular folder run.</span></span> <span data-ttu-id="f90f1-175">Это полезно для таких вещей, как установка одной страницы макета для всех страниц в папке или для проверки, пользователь вошел перед запуском страницы в папке.</span><span class="sxs-lookup"><span data-stu-id="f90f1-175">This is useful for things like setting the same layout page for all the pages in a folder, or for checking that a user is logged in before running a page in the folder.</span></span>

<span data-ttu-id="f90f1-176">Для страниц, в частности папок, вы можете создать код в файле с именем  *\_PageStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f90f1-176">For pages in particular folders, you can create code in a file named *\_PageStart.cshtml*.</span></span> <span data-ttu-id="f90f1-177">На следующей схеме показано как  *\_PageStart.cshtml* страница работает.</span><span class="sxs-lookup"><span data-stu-id="f90f1-177">The following diagram shows how the *\_PageStart.cshtml* page works.</span></span> <span data-ttu-id="f90f1-178">При поступлении запроса для страницы ASP.NET сначала проверяет  *\_AppStart.cshtml* страницы и работает под управлением.</span><span class="sxs-lookup"><span data-stu-id="f90f1-178">When a request comes in for a page, ASP.NET first checks for a *\_AppStart.cshtml* page and runs that.</span></span> <span data-ttu-id="f90f1-179">ASP.NET проверяет, существует ли  *\_PageStart.cshtml* страницы, и если да, работает под управлением.</span><span class="sxs-lookup"><span data-stu-id="f90f1-179">Then ASP.NET checks whether there's a *\_PageStart.cshtml* page, and if so, runs that.</span></span> <span data-ttu-id="f90f1-180">Затем он выполняет запрошенную страницу.</span><span class="sxs-lookup"><span data-stu-id="f90f1-180">It then runs the requested page.</span></span>

<span data-ttu-id="f90f1-181">Внутри  *\_PageStart.cshtml* страницы, где можно указать во время обработки требуется для запуска, включая запрошенную страницу `RunPage` метод.</span><span class="sxs-lookup"><span data-stu-id="f90f1-181">Inside the *\_PageStart.cshtml* page, you can specify where during processing you want the requested page to run by including a `RunPage` method.</span></span> <span data-ttu-id="f90f1-182">Это позволяет выполнять код перед выполнением запрошенной страницы и затем снова после него.</span><span class="sxs-lookup"><span data-stu-id="f90f1-182">This lets you run code before the requested page runs and then again after it.</span></span> <span data-ttu-id="f90f1-183">Если не включить `RunPage`, весь код в  *\_PageStart.cshtml* запуски и Запрашиваемая страница запускается автоматически.</span><span class="sxs-lookup"><span data-stu-id="f90f1-183">If you don't include `RunPage`, all the code in *\_PageStart.cshtml* runs, and then the requested page runs automatically.</span></span>

![[image]](18-customizing-site-wide-behavior/_static/image3.jpg)

<span data-ttu-id="f90f1-185">ASP.NET позволяет создать иерархию  *\_PageStart.cshtml* файлов.</span><span class="sxs-lookup"><span data-stu-id="f90f1-185">ASP.NET lets you create a hierarchy of *\_PageStart.cshtml* files.</span></span> <span data-ttu-id="f90f1-186">Можно поместить  *\_PageStart.cshtml* файл в корневой каталог сайта и в любой вложенной.</span><span class="sxs-lookup"><span data-stu-id="f90f1-186">You can put a *\_PageStart.cshtml* file in the root of the site and in any subfolder.</span></span> <span data-ttu-id="f90f1-187">При запросе страницы  *\_PageStart.cshtml* файл в запусков самого верхнего уровня (ближайшего к корню сайта), за которым следует  *\_PageStart.cshtml* файла в следующем Вложенная папка, и так далее вниз структура вложенных, пока запрос не достигнет папку, содержащую запрошенной страницы.</span><span class="sxs-lookup"><span data-stu-id="f90f1-187">When a page is requested, the *\_PageStart.cshtml* file at the top-most level (nearest to the site root) runs, followed by the *\_PageStart.cshtml* file in the next subfolder, and so on down the subfolder structure until the request reaches the folder that contains the requested page.</span></span> <span data-ttu-id="f90f1-188">После всех применимых  *\_PageStart.cshtml* запуска файлов, выполняет запрошенную страницу.</span><span class="sxs-lookup"><span data-stu-id="f90f1-188">After all the applicable *\_PageStart.cshtml* files have run, the requested page runs.</span></span>

<span data-ttu-id="f90f1-189">Например, может потребоваться следующее сочетание  *\_PageStart.cshtml* файлы и *Default.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="f90f1-189">For example, you might have the following combination of *\_PageStart.cshtml* files and *Default.cshtml* file:</span></span>

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

<span data-ttu-id="f90f1-190">При запуске */myfolder/default.cshtml*, вы увидите следующее:</span><span class="sxs-lookup"><span data-stu-id="f90f1-190">When you run */myfolder/default.cshtml*, you'll see the following:</span></span>

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a><span data-ttu-id="f90f1-191">Выполнение кода инициализации для всех страниц в папке</span><span class="sxs-lookup"><span data-stu-id="f90f1-191">Running Initialization Code for All Pages in a Folder</span></span>

<span data-ttu-id="f90f1-192">Рекомендуется использовать для  *\_PageStart.cshtml* файлов: для инициализации одну страницу макета для всех файлов в одной папке.</span><span class="sxs-lookup"><span data-stu-id="f90f1-192">A good use for *\_PageStart.cshtml* files is to initialize the same layout page for all files in a single folder.</span></span>

1. <span data-ttu-id="f90f1-193">В корневой папке создайте новую папку с именем *InitPages*.</span><span class="sxs-lookup"><span data-stu-id="f90f1-193">In the root folder, create a new folder named *InitPages*.</span></span>
2. <span data-ttu-id="f90f1-194">В *InitPages* папку веб-сайта, создайте файл с именем  *\_PageStart.cshtml* и замените разметку и код следующим:</span><span class="sxs-lookup"><span data-stu-id="f90f1-194">In the *InitPages* folder of your website, create a file named *\_PageStart.cshtml* and replace the default markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. <span data-ttu-id="f90f1-195">В корневом веб-сайта, создайте папку с именем *Shared*.</span><span class="sxs-lookup"><span data-stu-id="f90f1-195">In the root of the website, create a folder named *Shared*.</span></span>
4. <span data-ttu-id="f90f1-196">В *Shared* папки, создайте файл с именем  *\_Layout1.cshtml* и замените разметку и код следующим:</span><span class="sxs-lookup"><span data-stu-id="f90f1-196">In the *Shared* folder, create a file named *\_Layout1.cshtml* and replace the default markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. <span data-ttu-id="f90f1-197">В *InitPages* папки, создайте файл с именем *Content1.cshtml* и замените существующее содержимое следующим:</span><span class="sxs-lookup"><span data-stu-id="f90f1-197">In the *InitPages* folder, create a file named *Content1.cshtml* and replace the existing content with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. <span data-ttu-id="f90f1-198">В *InitPages* папки, создайте другой файл с именем *Content2.cshtml* и замените существующую разметку следующим:</span><span class="sxs-lookup"><span data-stu-id="f90f1-198">In the *InitPages* folder, create another file named *Content2.cshtml* and replace the default markup with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. <span data-ttu-id="f90f1-199">Запустите *Content1.cshtml* в браузере.</span><span class="sxs-lookup"><span data-stu-id="f90f1-199">Run *Content1.cshtml* in a browser.</span></span> 

    ![[image]](18-customizing-site-wide-behavior/_static/image4.jpg)

    <span data-ttu-id="f90f1-201">Когда *Content1.cshtml* выполняется, страница  *\_PageStart.cshtml* файл задает `Layout` , а также задается `PageData["MyBackground"]` цвет.</span><span class="sxs-lookup"><span data-stu-id="f90f1-201">When the *Content1.cshtml* page runs, the *\_PageStart.cshtml* file sets `Layout` and also sets `PageData["MyBackground"]` to a color.</span></span> <span data-ttu-id="f90f1-202">В *Content1.cshtml*, применения макета и цвет.</span><span class="sxs-lookup"><span data-stu-id="f90f1-202">In *Content1.cshtml*, the layout and color are applied.</span></span>
8. <span data-ttu-id="f90f1-203">Отображение *Content2.cshtml* в браузере.</span><span class="sxs-lookup"><span data-stu-id="f90f1-203">Display *Content2.cshtml* in a browser.</span></span> 

    <span data-ttu-id="f90f1-204">Макет не изменяется, так как обе страницы использовать те же страницы макета и цвет, как инициализировать в  *\_PageStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f90f1-204">The layout is the same, because both pages use the same layout page and color as initialized in *\_PageStart.cshtml*.</span></span>

## <a name="using-pagestartcshtml-to-handle-errors"></a><span data-ttu-id="f90f1-205">С помощью \_PageStart.cshtml для обработки ошибок</span><span class="sxs-lookup"><span data-stu-id="f90f1-205">Using \_PageStart.cshtml to Handle Errors</span></span>

<span data-ttu-id="f90f1-206">Использовать другой подходит для  *\_PageStart.cshtml* файла является создание способ обработки программирования ошибок (исключения), которые могут возникнуть в любой *.cshtml* страница в папке.</span><span class="sxs-lookup"><span data-stu-id="f90f1-206">Another good use for the *\_PageStart.cshtml* file is to create a way to handle programming errors (exceptions) that might occur in any *.cshtml* page in a folder.</span></span> <span data-ttu-id="f90f1-207">В этом примере показано, как это сделать.</span><span class="sxs-lookup"><span data-stu-id="f90f1-207">This example shows you one way to do this.</span></span>

1. <span data-ttu-id="f90f1-208">В корневой папке создайте папку с именем *InitCatch*.</span><span class="sxs-lookup"><span data-stu-id="f90f1-208">In the root folder, create a folder named *InitCatch*.</span></span>
2. <span data-ttu-id="f90f1-209">В *InitCatch* папку веб-сайта, создайте файл с именем  *\_PageStart.cshtml* и замените существующую разметку и код следующим:</span><span class="sxs-lookup"><span data-stu-id="f90f1-209">In the *InitCatch* folder of your website, create a file named *\_PageStart.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    <span data-ttu-id="f90f1-210">В этом коде повторите запуск Запрашиваемая страница явным образом с помощью вызова `RunPage` метод внутри `try` блока.</span><span class="sxs-lookup"><span data-stu-id="f90f1-210">In this code, you try running the requested page explicitly by calling the `RunPage` method inside a `try` block.</span></span> <span data-ttu-id="f90f1-211">При появлении ошибок программирования в запрошенной страницы, код внутри `catch` блок.</span><span class="sxs-lookup"><span data-stu-id="f90f1-211">If any programming errors occur in the requested page, the code inside the `catch` block runs.</span></span> <span data-ttu-id="f90f1-212">В этом случае код выполняет перенаправление на страницу (*Error.cshtml*) и передает имя файла, в которой возникла ошибка как часть URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="f90f1-212">In this case, the code redirects to a page (*Error.cshtml*) and passes the name of the file that experienced the error as part of the URL.</span></span> <span data-ttu-id="f90f1-213">(Вы создадите страницы вскоре.)</span><span class="sxs-lookup"><span data-stu-id="f90f1-213">(You'll create the page shortly.)</span></span>
3. <span data-ttu-id="f90f1-214">В *InitCatch* папку веб-сайта, создайте файл с именем *Exception.cshtml* и замените существующую разметку и код следующим:</span><span class="sxs-lookup"><span data-stu-id="f90f1-214">In the *InitCatch* folder of your website, create a file named *Exception.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    <span data-ttu-id="f90f1-215">Для этого примера вы всего лишь на этой странице намеренно создает ошибку при попытке открыть файл базы данных, которая не существует.</span><span class="sxs-lookup"><span data-stu-id="f90f1-215">For purposes of this example, what you're doing in this page is deliberately creating an error by trying to open a database file that doesn't exist.</span></span>
4. <span data-ttu-id="f90f1-216">В корневой папке создайте файл с именем *Error.cshtml* и замените существующую разметку и код следующим:</span><span class="sxs-lookup"><span data-stu-id="f90f1-216">In the root folder, create a file named *Error.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    <span data-ttu-id="f90f1-217">На этой странице выражение `@Request["source"]` возвращает значение из URL-адрес и отображает его.</span><span class="sxs-lookup"><span data-stu-id="f90f1-217">In this page, the expression `@Request["source"]` gets the value out of the URL and displays it.</span></span>
5. <span data-ttu-id="f90f1-218">На панели инструментов нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="f90f1-218">In the toolbar, click **Save**.</span></span>
6. <span data-ttu-id="f90f1-219">Запустите *Exception.cshtml* в браузере.</span><span class="sxs-lookup"><span data-stu-id="f90f1-219">Run *Exception.cshtml* in a browser.</span></span> 

    ![[image]](18-customizing-site-wide-behavior/_static/image5.jpg)

    <span data-ttu-id="f90f1-221">Из-за ошибки в *Exception.cshtml*,  *\_PageStart.cshtml* страница выполняет перенаправление на *Error.cshtml* файл, который выводит сообщение.</span><span class="sxs-lookup"><span data-stu-id="f90f1-221">Because an error occurs in *Exception.cshtml*, the *\_PageStart.cshtml* page redirects to the *Error.cshtml* file, which displays the message.</span></span>

    <span data-ttu-id="f90f1-222">Дополнительные сведения об исключениях см. в разделе [введение в ASP.NET Web Pages программирование с использованием синтаксиса Razor](https://go.microsoft.com/fwlink/?LinkID=251587).</span><span class="sxs-lookup"><span data-stu-id="f90f1-222">For more information about exceptions, see [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkID=251587).</span></span>

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a><span data-ttu-id="f90f1-223">С помощью \_PageStart.cshtml, чтобы ограничить доступ к папке</span><span class="sxs-lookup"><span data-stu-id="f90f1-223">Using \_PageStart.cshtml to Restrict Folder Access</span></span>

<span data-ttu-id="f90f1-224">Можно также использовать  *\_PageStart.cshtml* файл, чтобы ограничить доступ ко всем файлам в папке.</span><span class="sxs-lookup"><span data-stu-id="f90f1-224">You can also use the *\_PageStart.cshtml* file to restrict access to all the files in a folder.</span></span>

1. <span data-ttu-id="f90f1-225">В WebMatrix, создайте новый веб-сайт с помощью **сайта из шаблона** параметр.</span><span class="sxs-lookup"><span data-stu-id="f90f1-225">In WebMatrix, create a new website using the **Site From Template** option.</span></span>
2. <span data-ttu-id="f90f1-226">В списке доступных шаблонов выберите **начального сайта**.</span><span class="sxs-lookup"><span data-stu-id="f90f1-226">From the available templates, select **Starter Site**.</span></span>
3. <span data-ttu-id="f90f1-227">В корневой папке создайте папку с именем *AuthenticatedContent*.</span><span class="sxs-lookup"><span data-stu-id="f90f1-227">In the root folder, create a folder named *AuthenticatedContent*.</span></span>
4. <span data-ttu-id="f90f1-228">В *AuthenticatedContent* папки, создайте файл с именем  *\_PageStart.cshtml* и замените существующую разметку и код следующим:</span><span class="sxs-lookup"><span data-stu-id="f90f1-228">In the *AuthenticatedContent* folder, create a file named *\_PageStart.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    <span data-ttu-id="f90f1-229">Начинается код, так как все файлы в папке кэширования.</span><span class="sxs-lookup"><span data-stu-id="f90f1-229">The code starts by preventing all files in the folder from being cached.</span></span> <span data-ttu-id="f90f1-230">(Это требуется для сценариев, таких как общедоступные компьютеры, где вы не хотите кэшированных страниц одного пользователя будут доступны для следующего пользователя.) Затем код определяет ли пользователь вход в систему сайта, прежде чем они могут просматривать какие-либо страницы в папке.</span><span class="sxs-lookup"><span data-stu-id="f90f1-230">(This is required for scenarios like public computers, where you don't want one user's cached pages to be available to the next user.) Next, the code determines whether the user has signed in to the site before they can view any of the pages in the folder.</span></span> <span data-ttu-id="f90f1-231">Если пользователь не выполнил вход, код выполняет перенаправление на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="f90f1-231">If the user is not signed in, the code redirects to the login page.</span></span> <span data-ttu-id="f90f1-232">Страница входа можно вернуться на страницу, первоначально запрошенной при включении значение строки запроса с именем пользователя `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="f90f1-232">The login page can return the user to the page that was originally requested if you include a query string value named `ReturnUrl`.</span></span>
5. <span data-ttu-id="f90f1-233">Создание новой страницы в *AuthenticatedContent* папку с именем *Page.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f90f1-233">Create a new page in the *AuthenticatedContent* folder named *Page.cshtml*.</span></span>
6. <span data-ttu-id="f90f1-234">Замените существующую разметку следующим:</span><span class="sxs-lookup"><span data-stu-id="f90f1-234">Replace the default markup with the following:</span></span>  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. <span data-ttu-id="f90f1-235">Запустите *Page.cshtml* в браузере.</span><span class="sxs-lookup"><span data-stu-id="f90f1-235">Run *Page.cshtml* in a browser.</span></span> <span data-ttu-id="f90f1-236">Код выполняет перенаправление на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="f90f1-236">The code redirects you to a login page.</span></span> <span data-ttu-id="f90f1-237">Перед входом необходимо зарегистрировать.</span><span class="sxs-lookup"><span data-stu-id="f90f1-237">You must register before logging in.</span></span> <span data-ttu-id="f90f1-238">После регистрации и входа, можно перейти на страницу и просмотреть его содержимое.</span><span class="sxs-lookup"><span data-stu-id="f90f1-238">After you've registered and logged in, you can navigate to the page and view its contents.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="f90f1-239">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f90f1-239">Additional Resources</span></span>

[<span data-ttu-id="f90f1-240">Введение в программирование с использованием синтаксиса Razor веб-страниц ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f90f1-240">Introduction to ASP.NET Web Pages Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=251587)
