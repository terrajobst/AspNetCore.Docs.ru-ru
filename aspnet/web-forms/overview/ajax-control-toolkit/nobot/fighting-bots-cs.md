---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Борьба с программами-роботами (C#) | Документация Майкрософт
author: wenz
description: Автоматические программы-роботы Гипс блогов и других веб-сайтов с нежелательной почтой, отправка форм комментарий без вмешательства пользователя. Элемент управления NoBot в ASP.NET AJAX Con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 41e8fda5138e4a94e7b8c4af0a5c2bd68e50e9e1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402724"
---
<a name="fighting-bots-c"></a><span data-ttu-id="759c1-104">Борьба с программами-роботами (C#)</span><span class="sxs-lookup"><span data-stu-id="759c1-104">Fighting Bots (C#)</span></span>
====================
<span data-ttu-id="759c1-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="759c1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="759c1-106">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="759c1-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span></span>

> <span data-ttu-id="759c1-107">Автоматические программы-роботы Гипс блогов и других веб-сайтов с нежелательной почтой, отправка форм комментарий без вмешательства пользователя.</span><span class="sxs-lookup"><span data-stu-id="759c1-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="759c1-108">Элемент управления NoBot в ASP.NET AJAX Control Toolkit, помогающих бороться с этими программами-роботами.</span><span class="sxs-lookup"><span data-stu-id="759c1-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="759c1-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="759c1-109">Overview</span></span>

<span data-ttu-id="759c1-110">Автоматические программы-роботы Гипс блогов и других веб-сайтов с нежелательной почтой, отправка форм комментарий без вмешательства пользователя.</span><span class="sxs-lookup"><span data-stu-id="759c1-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="759c1-111">Элемент управления NoBot в ASP.NET AJAX Control Toolkit, помогающих бороться с этими программами-роботами.</span><span class="sxs-lookup"><span data-stu-id="759c1-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="759c1-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="759c1-112">Steps</span></span>

<span data-ttu-id="759c1-113">Распространенный подход к вообще отменить эффект действия программ-роботов является использование test CAPTCHAs полностью автоматизировать общих Тьюринга о компьютерах и люди друг от друга.</span><span class="sxs-lookup"><span data-stu-id="759c1-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="759c1-114">Тест Тьюринга изначально теста кто-то необходимости принятии решений о связи партнера человек или компьютере.</span><span class="sxs-lookup"><span data-stu-id="759c1-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="759c1-115">В веб-тест CAPTCHA обычно состоит из образа на некоторые искаженные буквы на нем.</span><span class="sxs-lookup"><span data-stu-id="759c1-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="759c1-116">Суть в том что только человек может читать буквы на изображение, в то время как алгоритмы OCR завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="759c1-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="759c1-117">Существует ряд преимуществ и недостатков этого подхода, но обсуждение этого вопроса выходит за рамки данного руководства.</span><span class="sxs-lookup"><span data-stu-id="759c1-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="759c1-118">Однако есть элемент управления в наборе элементов управления AJAX ASP.NET, который предоставляет подобный подход: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="759c1-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="759c1-119">Это проще, чем тест CAPTCHA преодолеть, но очень прост в использовании и платежах чрезвычайно эффективно на веб-сайтах, такие как блоги, где он считается успешной, если большинство нежелательной почты попыток нарушается, который `NoBot` управления звонками.</span><span class="sxs-lookup"><span data-stu-id="759c1-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="759c1-120">`NoBot` перехватывает обратной передачи текущего веб-формы ASP.NET, если соблюдается по крайней мере одно из следующих условий:</span><span class="sxs-lookup"><span data-stu-id="759c1-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="759c1-121">Браузер не сможет решить головоломку JavaScript (например если JavaScript отключен)</span><span class="sxs-lookup"><span data-stu-id="759c1-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="759c1-122">Пользователь отправил форму, чтобы быстро</span><span class="sxs-lookup"><span data-stu-id="759c1-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="759c1-123">IP-адрес клиента отправки формы Сотрудник слишком часто в определенный период времени.</span><span class="sxs-lookup"><span data-stu-id="759c1-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="759c1-124">Чтобы проверить выполнение этих условий `NoBot` элемент управления требует эти атрибуты (все из них необязательно):</span><span class="sxs-lookup"><span data-stu-id="759c1-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="759c1-125">`ResponseMinimumDelaySeconds` Минимальное количество секунд между обратными передачами</span><span class="sxs-lookup"><span data-stu-id="759c1-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="759c1-126">`CutoffWindowSeconds` Продолжительность интервала времени, в котором обратные передачи из одного IP-адреса являются мерами</span><span class="sxs-lookup"><span data-stu-id="759c1-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="759c1-127">`CutoffMaximumInstances` Максимальное количество секунд в интервале времени</span><span class="sxs-lookup"><span data-stu-id="759c1-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="759c1-128">Следующие требования разметки, по крайней мере две секунды должно пройти между обратными передачами, и существует только пять обратных передач или меньше, в пределах 30-секундного интервала:</span><span class="sxs-lookup"><span data-stu-id="759c1-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

<span data-ttu-id="759c1-129">Затем обычным образом Убедитесь, что для включения `ScriptManager` на странице, ASP.NET AJAX library загружается и может использоваться набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="759c1-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

<span data-ttu-id="759c1-130">Так как большинство проверок `NoBot` делает происходят на стороне сервера, необходимо проверить результат этих проверок.</span><span class="sxs-lookup"><span data-stu-id="759c1-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="759c1-131">Это можно сделать, вызвав `NoBot` `IsValid()` метод.</span><span class="sxs-lookup"><span data-stu-id="759c1-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="759c1-132">Он имеет один аргумент (как `out` параметр /`ByRef` параметров) типа `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="759c1-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="759c1-133">Строковое представление содержит причину, если проверка завершается неудачно и `Valid` в противном случае.</span><span class="sxs-lookup"><span data-stu-id="759c1-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="759c1-134">Следующий код выводит сообщение в соответствии с `NoBot`в результате:</span><span class="sxs-lookup"><span data-stu-id="759c1-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

<span data-ttu-id="759c1-135">Наконец вам потребуется формы для отправки и элемента label для вывода сообщения и все готово!</span><span class="sxs-lookup"><span data-stu-id="759c1-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

<span data-ttu-id="759c1-136">При выполнении этого скрипта и деактивировать JavaScript или отправить форму в течение первых двух секунд или отправить форму семь раз в течение 30 секунд, вы получите сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="759c1-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="759c1-137">Тем не менее этот элемент управления использовать осмотрительно, поскольку только около 90-95% пользователи активировали JavaScript, таким образом 5 – 10% пользователей не удастся `NoBot`элемента теста.</span><span class="sxs-lookup"><span data-stu-id="759c1-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="759c1-138">[![Это сообщение об ошибке может быть вызвано программы-робота](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="759c1-138">[![This error message could have been caused by a bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span></span>

<span data-ttu-id="759c1-139">Это сообщение об ошибке может быть вызвано программы-робота ([Просмотр полноразмерного изображения](fighting-bots-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="759c1-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="759c1-140">Вперед</span><span class="sxs-lookup"><span data-stu-id="759c1-140">Next</span></span>](fighting-bots-vb.md)
