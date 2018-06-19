---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Сайт с помощью CAPTCHA, чтобы предотвратить использование вашего веб-ASP.NET Razor программы-роботы) | Документы Microsoft
author: microsoft
description: В этой статье объясняется, как использовать ReCaptcha (меры безопасности), чтобы предотвратить выполнение задач в веб-страниц ASP.NET (Razor) автоматические программы (программы-роботы) мы...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529923"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="a8e44-103">Чтобы предотвратить использование вашего веб-ASP.NET Razor программы-роботы с помощью CAPTCHA) сайта</span><span class="sxs-lookup"><span data-stu-id="a8e44-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>
====================
<span data-ttu-id="a8e44-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a8e44-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a8e44-105">В этой статье описывается использование ReCaptcha (меры безопасности), чтобы предотвратить выполнение задач в на веб-сайт ASP.NET Web Pages (Razor) автоматических программ (программы-роботы).</span><span class="sxs-lookup"><span data-stu-id="a8e44-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="a8e44-106">**Что вы узнаете следующее.**</span><span class="sxs-lookup"><span data-stu-id="a8e44-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="a8e44-107">Как добавить тест CAPTCHA в веб-узла.</span><span class="sxs-lookup"><span data-stu-id="a8e44-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="a8e44-108">Существуют следующие функции ASP.NET, представленные в статье:</span><span class="sxs-lookup"><span data-stu-id="a8e44-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="a8e44-109">`ReCaptcha` Вспомогательные.</span><span class="sxs-lookup"><span data-stu-id="a8e44-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="a8e44-110">Информация в этой статье относится к веб-страниц ASP.NET 1.0 и 2 веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="a8e44-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>


## <a name="about-captchas"></a><span data-ttu-id="a8e44-111">О CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="a8e44-111">About CAPTCHAs</span></span>

<span data-ttu-id="a8e44-112">Каждый раз, предоставляется возможность регистрации пользователей на сайте или даже просто введите имя и URL-адрес (как для блог комментарий), может появиться наводнения фиктивное имен.</span><span class="sxs-lookup"><span data-stu-id="a8e44-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="a8e44-113">Они часто влево на автоматические программы (программы-роботы), пытающиеся оставьте URL-адреса в каждом веб-сайта, можно найти.</span><span class="sxs-lookup"><span data-stu-id="a8e44-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="a8e44-114">(Общие причины является для учета URL-адреса товаров для продажи).</span><span class="sxs-lookup"><span data-stu-id="a8e44-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="a8e44-115">Вы можете помочь убедитесь в том, что пользователь является человек, а не программой компьютера с помощью *CAPTCHA* для проверки пользователей при их регистрации или в противном случае введите свое имя и сайта.</span><span class="sxs-lookup"><span data-stu-id="a8e44-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="a8e44-116">CAPTCHA означает теста полностью автоматизировать открытый Тьюринга о компьютерах и люди расстоянии друг от друга.</span><span class="sxs-lookup"><span data-stu-id="a8e44-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="a8e44-117">— CAPTCHA *запрос ответ* теста, в котором пользователю будет предложено сделать что-нибудь легко лицу для выполнения, но трудно автоматизированной программой для выполнения.</span><span class="sxs-lookup"><span data-stu-id="a8e44-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="a8e44-118">Наиболее распространенный тип CAPTCHA, где некоторые искаженный буквы в разделе и будет предложено ввести их.</span><span class="sxs-lookup"><span data-stu-id="a8e44-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="a8e44-119">(Искажение должен усложняют программы-роботы расшифровать буквы.)</span><span class="sxs-lookup"><span data-stu-id="a8e44-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="a8e44-120">Добавление теста ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="a8e44-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="a8e44-121">На страницах ASP.NET, можно использовать `ReCaptcha` вспомогательный метод для отображения тест CAPTCHA, который основан на службе ReCaptcha ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="a8e44-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="a8e44-122">`ReCaptcha` Вспомогательный объект отображает изображение два искаженный слов, которые пользователи должны ввести правильно перед проверкой страницы.</span><span class="sxs-lookup"><span data-stu-id="a8e44-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="a8e44-123">Ответ пользователя проверяется службой ReCaptcha.Net.</span><span class="sxs-lookup"><span data-stu-id="a8e44-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="a8e44-124">Регистрация веб-сайта в ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="a8e44-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="a8e44-125">После завершения регистрации, вы сможете получить открытый ключ и закрытый ключ.</span><span class="sxs-lookup"><span data-stu-id="a8e44-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="a8e44-126">Добавить библиотека вспомогательных методов ASP.NET Web веб-сайта, как описано в [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если это еще не сделано.</span><span class="sxs-lookup"><span data-stu-id="a8e44-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="a8e44-127">Если у вас еще нет  *\_AppStart.cshtml* в корневой папке веб-сайта, создайте файл с именем  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a8e44-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="a8e44-128">Добавьте следующие `Recaptcha` вспомогательный параметры в  *\_AppStart.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="a8e44-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="a8e44-129">Задать `PublicKey` и `PrivateKey` свойства с помощью собственных открытый и закрытый ключи.</span><span class="sxs-lookup"><span data-stu-id="a8e44-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="a8e44-130">Сохранить  *\_AppStart.cshtml* файл и закройте его.</span><span class="sxs-lookup"><span data-stu-id="a8e44-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="a8e44-131">В корневой папке веб-сайта, создайте новую страницу с именем *Recaptcha.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a8e44-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="a8e44-132">Замените существующее содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a8e44-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="a8e44-133">Запустите *Recaptcha.cshtml* страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="a8e44-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="a8e44-134">Если `PrivateKey` значение допустимо, на странице отображается элемент управления ReCaptcha и кнопки.</span><span class="sxs-lookup"><span data-stu-id="a8e44-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="a8e44-135">Если вы не задали ключи глобально доступны в  *\_AppStart.html*, страница может отображаться ошибка.</span><span class="sxs-lookup"><span data-stu-id="a8e44-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="a8e44-136">Введите слова для теста.</span><span class="sxs-lookup"><span data-stu-id="a8e44-136">Enter the words for the test.</span></span> <span data-ttu-id="a8e44-137">Если вы прошли проверку ReCaptcha, появится сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="a8e44-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="a8e44-138">В противном случае отображается сообщение об ошибке, и отобразится ReCaptcha элемента управления.</span><span class="sxs-lookup"><span data-stu-id="a8e44-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="a8e44-139">Если компьютер находится в домене, которая использует прокси-сервер, может потребоваться настроить `defaultproxy` элемент *Web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="a8e44-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="a8e44-140">В следующем примере показан *Web.config* файл с `defaultproxy` настроены для включения должна работать служба ReCaptcha элемента.</span><span class="sxs-lookup"><span data-stu-id="a8e44-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="a8e44-141">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a8e44-141">Additional Resources</span></span>


- [<span data-ttu-id="a8e44-142">Настройка поведения сайта для веб-страниц узлов ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a8e44-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="a8e44-143">ReCaptcha сайта</span><span class="sxs-lookup"><span data-stu-id="a8e44-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
