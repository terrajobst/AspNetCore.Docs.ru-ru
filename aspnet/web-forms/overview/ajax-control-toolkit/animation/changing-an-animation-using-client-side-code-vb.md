---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Изменения анимации с помощью кода на стороне клиента (Visual Basic) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Можно также анимации...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f9b72576cc3a9e91827cfb40983821704621060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879157"
---
<a name="changing-an-animation-using-client-side-code-vb"></a><span data-ttu-id="69ccd-104">Изменения анимации с помощью кода на стороне клиента (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="69ccd-104">Changing an Animation Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="69ccd-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="69ccd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="69ccd-106">[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="69ccd-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span></span>

> <span data-ttu-id="69ccd-107">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="69ccd-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="69ccd-108">Также можно изменить анимации с помощью настраиваемый код JavaScript на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="69ccd-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="69ccd-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="69ccd-109">Overview</span></span>

<span data-ttu-id="69ccd-110">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="69ccd-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="69ccd-111">Также можно изменить анимации с помощью настраиваемый код JavaScript на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="69ccd-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="69ccd-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="69ccd-112">Steps</span></span>

<span data-ttu-id="69ccd-113">Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="69ccd-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="69ccd-114">Анимация применяется панель текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="69ccd-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="69ccd-115">В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:</span><span class="sxs-lookup"><span data-stu-id="69ccd-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="69ccd-116">Фактический анимации, запускаемого по HTML-кнопок:</span><span class="sxs-lookup"><span data-stu-id="69ccd-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="69ccd-117">Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="69ccd-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

<span data-ttu-id="69ccd-118">Следует отметить, что не `<Animations>` узла в `AnimationExtender` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="69ccd-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="69ccd-119">Настраиваемый код JavaScript используется для предоставления анимации для использования с элементом управления.</span><span class="sxs-lookup"><span data-stu-id="69ccd-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="69ccd-120">Как и в случае с сервером API `AnimationExtender`, невозможно просто присвоить расширителя анимации еще.</span><span class="sxs-lookup"><span data-stu-id="69ccd-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="69ccd-121">Однако расширителя предоставлять несколько методов для чтения и записи анимации регистрации различных событий (`OnClick`, `OnLoad`и так далее).</span><span class="sxs-lookup"><span data-stu-id="69ccd-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="69ccd-122">Далее приводятся некоторые примеры.</span><span class="sxs-lookup"><span data-stu-id="69ccd-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="69ccd-123">Формат возвращаемого значения `get_*()` функции и формат аргумента для `set_*()` функции представляет собой строку JSON, предоставляя объектное представление XML-разметку, которые бы.</span><span class="sxs-lookup"><span data-stu-id="69ccd-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="69ccd-124">В настоящее время невозможно передать объект в, но это можно считать объект из данной анимации (`get_OnXXXBehavior()` методов).</span><span class="sxs-lookup"><span data-stu-id="69ccd-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="69ccd-125">Вот строка JSON (без разделителя кавычек и форматировать хорошо) представляющий анимации запуска по щелчку кнопки, но анимации панели, его размера и исчезновение одновременно:</span><span class="sxs-lookup"><span data-stu-id="69ccd-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

<span data-ttu-id="69ccd-126">Следующий код JavaScript назначает это descripting JSON для `OnClick` анимации текущего расширителя и запускает ее:</span><span class="sxs-lookup"><span data-stu-id="69ccd-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


<span data-ttu-id="69ccd-127">[![Анимация выполняется немедленно, без щелчка мыши (и с минимальными разметкой)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="69ccd-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="69ccd-128">Анимация выполняется немедленно, без щелчок мыши (и с минимальными разметкой) ([Просмотр полноразмерное изображение](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="69ccd-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="69ccd-129">[Назад](executing-animations-using-client-side-code-vb.md)
> [Вперед](animating-an-updatepanel-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="69ccd-129">[Previous](executing-animations-using-client-side-code-vb.md)
[Next](animating-an-updatepanel-control-vb.md)</span></span>
