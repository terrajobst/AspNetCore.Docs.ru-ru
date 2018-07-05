---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: Динамическое управление анимациями UpdatePanel (C#) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Для содержимого...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: f7b363a23c05490522705648fabfe0e7d0856b55
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400685"
---
<a name="dynamically-controlling-updatepanel-animations-c"></a><span data-ttu-id="a312a-104">Динамическое управление анимациями UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="a312a-104">Dynamically Controlling UpdatePanel Animations (C#)</span></span>
====================
<span data-ttu-id="a312a-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a312a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a312a-106">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a312a-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span></span>

> <span data-ttu-id="a312a-107">Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="a312a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a312a-108">Для содержимого элемента управления UpdatePanel, существует специальные расширения, которая основывается на использовании framework анимации: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="a312a-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="a312a-109">Он также может работать вместе с триггерах UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="a312a-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="a312a-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="a312a-110">Overview</span></span>

<span data-ttu-id="a312a-111">Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="a312a-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a312a-112">Для содержимого `UpdatePanel`, существует специальные расширения, которая основывается на использовании framework анимации: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="a312a-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="a312a-113">Он также может работать вместе с `UpdatePanel` триггеров.</span><span class="sxs-lookup"><span data-stu-id="a312a-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="a312a-114">Шаги</span><span class="sxs-lookup"><span data-stu-id="a312a-114">Steps</span></span>

<span data-ttu-id="a312a-115">Первым шагом является обычным образом, чтобы включить `ScriptManager` на странице, ASP.NET AJAX library загружается и может использоваться набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="a312a-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

<span data-ttu-id="a312a-116">Анимация в этом сценарии будет применяться для отображения в виде текущего времени.</span><span class="sxs-lookup"><span data-stu-id="a312a-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="a312a-117">Эти сведения можно записать в метку с помощью `Page_Load()` метод, или (для простоты) используется следующий встроенный код:</span><span class="sxs-lookup"><span data-stu-id="a312a-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

<span data-ttu-id="a312a-118">Кроме того создается кнопка для активации, обновляя время:</span><span class="sxs-lookup"><span data-stu-id="a312a-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

<span data-ttu-id="a312a-119">Этот код помещается в переменную `<ContentTemplate>` раздел `UpdatePanel` элемент.</span><span class="sxs-lookup"><span data-stu-id="a312a-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="a312a-120">Панели `UpdateMode` атрибута должно быть присвоено `"Conditional"`, поскольку только триггеры могут обновлять содержимое панели.</span><span class="sxs-lookup"><span data-stu-id="a312a-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="a312a-121">В `<Triggers>` раздел `UpdatePanel`, создается триггер асинхронной обратной передачи и привязаны к `Click` события кнопки.</span><span class="sxs-lookup"><span data-stu-id="a312a-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="a312a-122">Таким образом, если пользователь нажимает кнопку, `UpdatePanel` обновляется.</span><span class="sxs-lookup"><span data-stu-id="a312a-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="a312a-123">Далее приведена разметка для `UpdatePanel` управления:</span><span class="sxs-lookup"><span data-stu-id="a312a-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

<span data-ttu-id="a312a-124">Наконец `UpdatePanelAnimationExtender` должен быть настроен: задать `TargetControlID` атрибут с идентификатором панели и определите анимацию в модуле.</span><span class="sxs-lookup"><span data-stu-id="a312a-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="a312a-125">Плавный переход в делает смысле, который создает удобный визуальное выделение важных фрагментов, на время обновления.</span><span class="sxs-lookup"><span data-stu-id="a312a-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="a312a-126">Расширения разметки может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a312a-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

<span data-ttu-id="a312a-127">Запустите файл в браузере.</span><span class="sxs-lookup"><span data-stu-id="a312a-127">Run the file in the browser.</span></span> <span data-ttu-id="a312a-128">При каждом нажатии кнопки текущее время отображается на панели, всегда плавный переход течение одной секунды.</span><span class="sxs-lookup"><span data-stu-id="a312a-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="a312a-129">[![Текущее время плавный переход](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a312a-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span></span>

<span data-ttu-id="a312a-130">Текущее время плавный переход ([Просмотр полноразмерного изображения](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a312a-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a312a-131">[Назад](animating-an-updatepanel-control-cs.md)
> [Вперед](adding-animation-to-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a312a-131">[Previous](animating-an-updatepanel-control-cs.md)
[Next](adding-animation-to-a-control-vb.md)</span></span>
