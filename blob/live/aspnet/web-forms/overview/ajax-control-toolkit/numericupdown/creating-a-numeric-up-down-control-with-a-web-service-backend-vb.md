---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: "Создание числового вверх или вниз элемента управления с серверной службы Web (Visual Basic) | Документы Microsoft"
author: wenz
description: "Вместо позволить пользователю ввести значение в поле Проверка, числовой вверх и вниз (то есть в Windows и других операционных системах) может оказаться как дополнительные c..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ceefd6c18761c2abe3f3a4298d340642a0951d6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a><span data-ttu-id="79ee5-103">Создание числового элемента управления вверх/вниз с серверной службы Web (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="79ee5-103">Creating a Numeric Up/Down Control with a Web Service Backend (VB)</span></span>
====================
<span data-ttu-id="79ee5-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="79ee5-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="79ee5-105">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="79ee5-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span></span>

> <span data-ttu-id="79ee5-106">Вместо позволить пользователю ввести значение в поле проверьте, может оказаться как дополнительные числовой для увеличения или уменьшения управления (который существует в Windows и других операционных системах) все возможности.</span><span class="sxs-lookup"><span data-stu-id="79ee5-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="79ee5-107">По умолчанию элемент управления NumericUpDown всегда увеличивает или уменьшает значение на 1, но веб-службы подтверждает большую гибкость.</span><span class="sxs-lookup"><span data-stu-id="79ee5-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="79ee5-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="79ee5-108">Overview</span></span>

<span data-ttu-id="79ee5-109">Вместо позволить пользователю ввести значение в поле проверьте, может оказаться как дополнительные числовой для увеличения или уменьшения управления (который существует в Windows и других операционных системах) все возможности.</span><span class="sxs-lookup"><span data-stu-id="79ee5-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="79ee5-110">По умолчанию `NumericUpDown` управления всегда увеличивает или уменьшает значение на 1, но веб-службы подтверждает большую гибкость.</span><span class="sxs-lookup"><span data-stu-id="79ee5-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="79ee5-111">Шаги</span><span class="sxs-lookup"><span data-stu-id="79ee5-111">Steps</span></span>

<span data-ttu-id="79ee5-112">Содержит набор элементов управления ASP.NET AJAX `NumericUpDown` расширителя, который автоматически добавляет две кнопки в текстовое поле: одно для увеличения количества его значения, один для его уменьшения.</span><span class="sxs-lookup"><span data-stu-id="79ee5-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="79ee5-113">Однако элемент управления также поддерживает вызов веб-службы (или вызов метода страницы).</span><span class="sxs-lookup"><span data-stu-id="79ee5-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="79ee5-114">При каждом щелчке вверх или вниз, JavaScript, код подключается к веб-сервер и выполняет метод существует.</span><span class="sxs-lookup"><span data-stu-id="79ee5-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="79ee5-115">Сигнатура метода является приведенному ниже:</span><span class="sxs-lookup"><span data-stu-id="79ee5-115">The method signature is the following one:</span></span>

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

<span data-ttu-id="79ee5-116">`current` Аргумент — это текущее значение в текстовом поле; `tag` атрибут имеет дополнительный контекст данных, можно задать в качестве свойства `NumericUpDown` расширителя (но не обязательно).</span><span class="sxs-lookup"><span data-stu-id="79ee5-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="79ee5-117">Для этого образца числовые вверх или вниз элемента управления должны разрешать только значения, которые представляют собой степени двух: 1, 2, 4, 8, 16, 32, 64 и т. д.</span><span class="sxs-lookup"><span data-stu-id="79ee5-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="79ee5-118">Таким образом метод выполняется, когда пользователь хочет увеличить значение необходимо double старое значение; другой метод должны деления значения на два.</span><span class="sxs-lookup"><span data-stu-id="79ee5-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="79ee5-119">Вот полный веб-службы:</span><span class="sxs-lookup"><span data-stu-id="79ee5-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

<span data-ttu-id="79ee5-120">Наконец создайте новую страницу ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="79ee5-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="79ee5-121">Как обычно требуется `ScriptManager` управления `TextBox` управления и `NumericUpDownExtender` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="79ee5-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="79ee5-122">Для второго вы должны предоставить информации веб-службы:</span><span class="sxs-lookup"><span data-stu-id="79ee5-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="79ee5-123">`ServiceDownMethod`Имя списка веб-метод или метод страницы</span><span class="sxs-lookup"><span data-stu-id="79ee5-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="79ee5-124">`ServiceDownPath`путь к веб-службы с вниз метода службы; пропустить, если вы используете метод страницы</span><span class="sxs-lookup"><span data-stu-id="79ee5-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="79ee5-125">`ServiceUpMethod`Имя вверх веб-метод или метод страницы</span><span class="sxs-lookup"><span data-stu-id="79ee5-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="79ee5-126">`ServiceUpPath`путь к веб-службы с вверх метода службы; пропустить, если вы используете метод страницы</span><span class="sxs-lookup"><span data-stu-id="79ee5-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="79ee5-127">Ниже приведен полный разметку страницы.</span><span class="sxs-lookup"><span data-stu-id="79ee5-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

<span data-ttu-id="79ee5-128">При запуске страницы, обратите внимание на то, как значение в текстовом поле всегда удваивается при щелчке на верхнюю кнопку и уменьшается вдвое при нажатии на кнопку ниже.</span><span class="sxs-lookup"><span data-stu-id="79ee5-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


<span data-ttu-id="79ee5-129">[![Отображаются только те числа, которые являются степенью числа 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="79ee5-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span></span>

<span data-ttu-id="79ee5-130">Отображаются только те числа, которые являются степенью числа 2 ([Просмотр полноразмерное изображение](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="79ee5-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="79ee5-131">Назад</span><span class="sxs-lookup"><span data-stu-id="79ee5-131">Previous</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
