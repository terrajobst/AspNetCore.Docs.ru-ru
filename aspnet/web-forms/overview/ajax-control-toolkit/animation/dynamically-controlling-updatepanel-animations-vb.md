---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: "Динамическому управлению UpdatePanel анимации (VB) | Документы Microsoft"
author: wenz
description: "Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Для содержимого..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 75b1f169724d8d3f8bcbc46198d320eeb8e7260c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a><span data-ttu-id="1114d-104">Динамическому управлению UpdatePanel анимации (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="1114d-104">Dynamically Controlling UpdatePanel Animations (VB)</span></span>
====================
<span data-ttu-id="1114d-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1114d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1114d-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1114d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span></span>

> <span data-ttu-id="1114d-107">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="1114d-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1114d-108">Для содержимого элемента управления UpdatePanel, существует специальные расширения, в большой степени основывается на платформе анимации: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="1114d-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="1114d-109">Она также может работать вместе с триггерами UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="1114d-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="1114d-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="1114d-110">Overview</span></span>

<span data-ttu-id="1114d-111">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="1114d-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1114d-112">Для содержимого `UpdatePanel`, специальные расширения существует, в большой степени основывается на платформе анимации: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="1114d-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="1114d-113">Можно также работать вместе с `UpdatePanel` триггеров.</span><span class="sxs-lookup"><span data-stu-id="1114d-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="1114d-114">Шаги</span><span class="sxs-lookup"><span data-stu-id="1114d-114">Steps</span></span>

<span data-ttu-id="1114d-115">Первым шагом является обычным образом, чтобы включить `ScriptManager` на странице, чтобы загрузить библиотеки ASP.NET AJAX и может использоваться набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="1114d-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

<span data-ttu-id="1114d-116">Анимация в этом сценарии будет применяться для отображения текущего времени.</span><span class="sxs-lookup"><span data-stu-id="1114d-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="1114d-117">Эти сведения могут записываться в метку с использованием `Page_Load()` метода, или (для простоты) используется следующий встроенный код:</span><span class="sxs-lookup"><span data-stu-id="1114d-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

<span data-ttu-id="1114d-118">Кроме того создается кнопка для активации, обновляя время:</span><span class="sxs-lookup"><span data-stu-id="1114d-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

<span data-ttu-id="1114d-119">Этот код затем помещаются в `<ContentTemplate>` раздел `UpdatePanel` элемента.</span><span class="sxs-lookup"><span data-stu-id="1114d-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="1114d-120">Панель `UpdateMode` атрибуту должно быть присвоено `"Conditional"`, поскольку только триггеры могут обновлять ее содержимого.</span><span class="sxs-lookup"><span data-stu-id="1114d-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="1114d-121">В `<Triggers>` раздел `UpdatePanel`, создается триггер асинхронной обратной передачи и привязан к `Click` события кнопки.</span><span class="sxs-lookup"><span data-stu-id="1114d-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="1114d-122">Таким образом, если пользователь нажимает кнопку, `UpdatePanel` не будет обновлен.</span><span class="sxs-lookup"><span data-stu-id="1114d-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="1114d-123">Разметка для `UpdatePanel` управления:</span><span class="sxs-lookup"><span data-stu-id="1114d-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

<span data-ttu-id="1114d-124">Наконец `UpdatePanelAnimationExtender` должны быть настроены: задать `TargetControlID` в идентификатор панели и определении анимации в модуле.</span><span class="sxs-lookup"><span data-stu-id="1114d-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="1114d-125">Эффект постепенного увеличения делает смысле, создающий хорошо visual акцент на время обновления.</span><span class="sxs-lookup"><span data-stu-id="1114d-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="1114d-126">Расширения разметки может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1114d-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

<span data-ttu-id="1114d-127">Запустите файл в браузере.</span><span class="sxs-lookup"><span data-stu-id="1114d-127">Run the file in the browser.</span></span> <span data-ttu-id="1114d-128">Всякий раз при нажатии на кнопку на панели всегда эффект постепенного увеличения в течение одной секунды отображается текущее время.</span><span class="sxs-lookup"><span data-stu-id="1114d-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="1114d-129">[![Эффект постепенного увеличения текущего времени](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1114d-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span></span>

<span data-ttu-id="1114d-130">Эффект постепенного увеличения текущего времени ([Просмотр полноразмерное изображение](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1114d-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="1114d-131">Назад</span><span class="sxs-lookup"><span data-stu-id="1114d-131">Previous</span></span>](animating-an-updatepanel-control-vb.md)
