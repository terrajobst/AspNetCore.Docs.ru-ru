---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Использование CAPTCHA, чтобы предотвратить использование веб-ASP.NET Razor программами-роботами) сайта | Документация Майкрософт
author: microsoft
description: В этой статье описывается использование ReCaptcha (меры безопасности), чтобы предотвратить выполнение задач в веб-страниц ASP.NET (Razor) автоматическими программами (программами-роботами) мы...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: cfdc8561f8ee4b97cc1ef2111113846a1c579d5c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389076"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="98ea8-103">Использование CAPTCHA, чтобы предотвратить использование веб-ASP.NET Razor программами-роботами) сайта</span><span class="sxs-lookup"><span data-stu-id="98ea8-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>
====================
<span data-ttu-id="98ea8-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="98ea8-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="98ea8-105">В этой статье описываются способы использования ReCaptcha (меры безопасности), чтобы предотвратить выполнение задач в на веб-сайте ASP.NET Web Pages (Razor) автоматическими программами (программами-роботами).</span><span class="sxs-lookup"><span data-stu-id="98ea8-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="98ea8-106">**Вы узнаете, как:**</span><span class="sxs-lookup"><span data-stu-id="98ea8-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="98ea8-107">Как добавить на сайт тест CAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="98ea8-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="98ea8-108">Ниже приведены функции ASP.NET, представленные в этой статье.</span><span class="sxs-lookup"><span data-stu-id="98ea8-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="98ea8-109">`ReCaptcha` Вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="98ea8-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="98ea8-110">Сведения в этой статье относятся к веб-страниц ASP.NET 1.0 и Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="98ea8-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>


## <a name="about-captchas"></a><span data-ttu-id="98ea8-111">О CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="98ea8-111">About CAPTCHAs</span></span>

<span data-ttu-id="98ea8-112">Любое время, вы сотрудники могут легко зарегистрировать на сайте, или даже просто введите имя и URL-адрес (как и для блог комментарий), то можно получить поток ненастоящие.</span><span class="sxs-lookup"><span data-stu-id="98ea8-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="98ea8-113">Они часто остаются с автоматическими программами (программами-роботами), которые пытаются привести каждого веб-узла, который можно найти URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="98ea8-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="98ea8-114">(Распространенный мотив — post URL-адреса продуктов для продажи.)</span><span class="sxs-lookup"><span data-stu-id="98ea8-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="98ea8-115">Помочь убедитесь в том, что пользователь является человек, а не программой компьютера с помощью *CAPTCHA* для проверки подлинности пользователей при регистрации или в противном случае введите свое имя и сайта.</span><span class="sxs-lookup"><span data-stu-id="98ea8-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="98ea8-116">CAPTCHA означает теста полностью автоматизировать общих Тьюринга о компьютерах и люди друг от друга.</span><span class="sxs-lookup"><span data-stu-id="98ea8-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="98ea8-117">— Тест CAPTCHA *ответ на запрос* теста, в котором пользователю предлагается сделать нечто, легко лицу для выполнения, но трудно сделать автоматизированной программой.</span><span class="sxs-lookup"><span data-stu-id="98ea8-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="98ea8-118">Это наиболее распространенный тип CAPTCHA, где некоторые искаженные буквы см. в разделе и будет предложено ввести их.</span><span class="sxs-lookup"><span data-stu-id="98ea8-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="98ea8-119">(Искажение предполагается затруднить программ-роботов расшифровать буквы.)</span><span class="sxs-lookup"><span data-stu-id="98ea8-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="98ea8-120">Добавление теста ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="98ea8-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="98ea8-121">На страницах ASP.NET, можно использовать `ReCaptcha` вспомогательный метод для отрисовки тест CAPTCHA, основанный на службе ReCaptcha ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="98ea8-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="98ea8-122">`ReCaptcha` Вспомогательный объект отображает изображение два искаженные слов, которые пользователям нужно правильно ввести перед проверкой страницы.</span><span class="sxs-lookup"><span data-stu-id="98ea8-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="98ea8-123">Ответ пользователя проверяется службой ReCaptcha.Net.</span><span class="sxs-lookup"><span data-stu-id="98ea8-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="98ea8-124">Регистрация веб-сайта в ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="98ea8-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="98ea8-125">После завершения регистрации вы получите открытый ключ и закрытый ключ.</span><span class="sxs-lookup"><span data-stu-id="98ea8-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="98ea8-126">Добавьте библиотеку ASP.NET на веб-сайт, как описано в [Установка вспомогательных функций на сайте веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если у вас еще нет.</span><span class="sxs-lookup"><span data-stu-id="98ea8-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="98ea8-127">Если у вас нет  *\_AppStart.cshtml* в корневой папке веб-сайта, создайте файл с именем  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="98ea8-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="98ea8-128">Добавьте следующий `Recaptcha` вспомогательные параметры в  *\_AppStart.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="98ea8-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="98ea8-129">Задайте `PublicKey` и `PrivateKey` свойства с помощью собственных открытых и закрытых ключей.</span><span class="sxs-lookup"><span data-stu-id="98ea8-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="98ea8-130">Сохранить  *\_AppStart.cshtml* файл и закройте его.</span><span class="sxs-lookup"><span data-stu-id="98ea8-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="98ea8-131">В корневой папке веб-сайта, создайте новую страницу с именем *Recaptcha.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="98ea8-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="98ea8-132">Замените существующее содержимое следующим:</span><span class="sxs-lookup"><span data-stu-id="98ea8-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="98ea8-133">Запустите *Recaptcha.cshtml* страницу в браузере.</span><span class="sxs-lookup"><span data-stu-id="98ea8-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="98ea8-134">Если `PrivateKey` значение является допустимым, на странице отображается элемент управления ReCaptcha и кнопки.</span><span class="sxs-lookup"><span data-stu-id="98ea8-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="98ea8-135">Если вы не задали ключи глобально в  *\_AppStart.html*, на странице будет отображаться ошибка.</span><span class="sxs-lookup"><span data-stu-id="98ea8-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="98ea8-136">Введите слова для теста.</span><span class="sxs-lookup"><span data-stu-id="98ea8-136">Enter the words for the test.</span></span> <span data-ttu-id="98ea8-137">Если вы прошли Испытание на ReCaptcha, вы увидите сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="98ea8-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="98ea8-138">В противном случае отображается сообщение об ошибке и отобразится элемент управления ReCaptcha.</span><span class="sxs-lookup"><span data-stu-id="98ea8-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="98ea8-139">Если компьютер находится в домене, который использует прокси-сервер, может потребоваться настроить `defaultproxy` элемент *Web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="98ea8-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="98ea8-140">В следующем примере показан *Web.config* файл с `defaultproxy` элемент, настроены для включения ReCaptcha для работы службы.</span><span class="sxs-lookup"><span data-stu-id="98ea8-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="98ea8-141">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="98ea8-141">Additional Resources</span></span>


- [<span data-ttu-id="98ea8-142">Настройка поведения сайта для веб-страниц узлов ASP.NET</span><span class="sxs-lookup"><span data-stu-id="98ea8-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="98ea8-143">ReCaptcha сайта</span><span class="sxs-lookup"><span data-stu-id="98ea8-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
