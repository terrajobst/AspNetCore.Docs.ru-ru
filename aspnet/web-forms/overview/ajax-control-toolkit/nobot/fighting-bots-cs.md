---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Борьбе с программы-роботы (C#) | Документы Microsoft
author: wenz
description: Автоматические программы-роботы Гипс блоги и другие веб-сайты с нежелательной почты, отправки форм комментарий без вмешательства пользователя. Элемент управления NoBot в ASP.NET AJAX Con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 1ea3aaa5461c2f58a927ae975568f18a34a4729b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872017"
---
<a name="fighting-bots-c"></a><span data-ttu-id="0e26f-104">Борьбе с программы-роботы (C#)</span><span class="sxs-lookup"><span data-stu-id="0e26f-104">Fighting Bots (C#)</span></span>
====================
<span data-ttu-id="0e26f-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0e26f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0e26f-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0e26f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span></span>

> <span data-ttu-id="0e26f-107">Автоматические программы-роботы Гипс блоги и другие веб-сайты с нежелательной почты, отправки форм комментарий без вмешательства пользователя.</span><span class="sxs-lookup"><span data-stu-id="0e26f-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="0e26f-108">Элемент управления NoBot в наборе элементов управления ASP.NET AJAX борьбы эти программы-роботы.</span><span class="sxs-lookup"><span data-stu-id="0e26f-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="0e26f-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="0e26f-109">Overview</span></span>

<span data-ttu-id="0e26f-110">Автоматические программы-роботы Гипс блоги и другие веб-сайты с нежелательной почты, отправки форм комментарий без вмешательства пользователя.</span><span class="sxs-lookup"><span data-stu-id="0e26f-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="0e26f-111">Элемент управления NoBot в наборе элементов управления ASP.NET AJAX борьбы эти программы-роботы.</span><span class="sxs-lookup"><span data-stu-id="0e26f-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="0e26f-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="0e26f-112">Steps</span></span>

<span data-ttu-id="0e26f-113">Распространенный подход для предотвращения программы-роботы является использование теста CAPTCHAs полностью автоматизировать открытый Тьюринга о компьютерах и люди расстоянии друг от друга.</span><span class="sxs-lookup"><span data-stu-id="0e26f-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="0e26f-114">Тест Тьюринга изначально теста где кто-то требуются решить, верна ли связь партнера человек или на компьютере.</span><span class="sxs-lookup"><span data-stu-id="0e26f-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="0e26f-115">В веб-тест CAPTCHA обычно состоит из изображения на нем некоторые искаженный буквами.</span><span class="sxs-lookup"><span data-stu-id="0e26f-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="0e26f-116">Идея состоит, что только человек может прочитать буквы в образе, в то время как алгоритмы OCR завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="0e26f-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="0e26f-117">Некоторые преимущества и недостатки этого подхода, но с обсуждением выходит за рамки данного руководства.</span><span class="sxs-lookup"><span data-stu-id="0e26f-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="0e26f-118">Однако элемент управления в набор элементов управления ASP.NET AJAX, который предоставляет подобный подход: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="0e26f-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="0e26f-119">Это проще, чем тест CAPTCHA преодолеть, но очень прост в использовании и цен очень хорошо на веб-сайты, такие как блоги, где он считается пройденной, если большинство нежелательной почты попыток нарушается, который `NoBot` можно сделать элемент управления.</span><span class="sxs-lookup"><span data-stu-id="0e26f-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="0e26f-120">`NoBot` перехватывает обратной передачи текущего веб-формы ASP.NET, если соблюдается по крайней мере одно из следующих условий:</span><span class="sxs-lookup"><span data-stu-id="0e26f-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="0e26f-121">Браузер не удается решить головоломку JavaScript (например при JavaScript отключена)</span><span class="sxs-lookup"><span data-stu-id="0e26f-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="0e26f-122">Форму, чтобы быстро отправленные пользователем</span><span class="sxs-lookup"><span data-stu-id="0e26f-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="0e26f-123">IP-адрес клиента отправки формы слишком часто в течение определенного периода времени.</span><span class="sxs-lookup"><span data-stu-id="0e26f-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="0e26f-124">Чтобы проверить выполнение этих условий `NoBot` управления требует этих атрибутов (все они необязательно):</span><span class="sxs-lookup"><span data-stu-id="0e26f-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="0e26f-125">`ResponseMinimumDelaySeconds` Минимальное количество секунд между обратными передачами</span><span class="sxs-lookup"><span data-stu-id="0e26f-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="0e26f-126">`CutoffWindowSeconds` Продолжительность интервала времени, в котором обратных передач с одного IP-адреса являются мерами</span><span class="sxs-lookup"><span data-stu-id="0e26f-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="0e26f-127">`CutoffMaximumInstances` Максимальное количество секунд в интервал времени</span><span class="sxs-lookup"><span data-stu-id="0e26f-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="0e26f-128">Следующие требования разметки, по крайней мере двух секунд должно пройти между обратными передачами и существуют только пять обратной передачи или меньше в пределах 30-секундного интервала:</span><span class="sxs-lookup"><span data-stu-id="0e26f-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

<span data-ttu-id="0e26f-129">Затем обычным образом Убедитесь, что для включения `ScriptManager` на странице, чтобы загрузить библиотеки ASP.NET AJAX и может использоваться набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="0e26f-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

<span data-ttu-id="0e26f-130">Поскольку большинство проверок `NoBot` выполняет возникают на стороне сервера, необходимо проверить результат этих проверок.</span><span class="sxs-lookup"><span data-stu-id="0e26f-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="0e26f-131">Это можно сделать путем вызова `NoBot` `IsValid()` метод.</span><span class="sxs-lookup"><span data-stu-id="0e26f-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="0e26f-132">Он имеет один аргумент (как `out` параметр /`ByRef` параметров) типа `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="0e26f-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="0e26f-133">Строковое представление содержит по причине, если проверка завершается неудачно и `Valid` в противном случае.</span><span class="sxs-lookup"><span data-stu-id="0e26f-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="0e26f-134">Следующий код выводит сообщение в соответствии с `NoBot`в результате:</span><span class="sxs-lookup"><span data-stu-id="0e26f-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

<span data-ttu-id="0e26f-135">Наконец требуются формы для отправки и элемент label для вывода сообщения, и все готово!</span><span class="sxs-lookup"><span data-stu-id="0e26f-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

<span data-ttu-id="0e26f-136">При выполнении этого сценария и деактивировать JavaScript или отправить форму в течение первых двух секунд или отправки формы семь раз в течение 30 секунд, вы получите сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="0e26f-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="0e26f-137">Однако рационально использовать этот элемент управления, поскольку лишь около 90-95% пользователей JavaScript активирован, 5 – 10% от пользователей не будут применены `NoBot`этого теста.</span><span class="sxs-lookup"><span data-stu-id="0e26f-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="0e26f-138">[![Это сообщение об ошибке вызвано программа-робот](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0e26f-138">[![This error message could have been caused by a bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span></span>

<span data-ttu-id="0e26f-139">Это сообщение об ошибке вызвано программа-робот ([Просмотр полноразмерное изображение](fighting-bots-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0e26f-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0e26f-140">Вперед</span><span class="sxs-lookup"><span data-stu-id="0e26f-140">Next</span></span>](fighting-bots-vb.md)
