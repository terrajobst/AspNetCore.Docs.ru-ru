---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Позиционирование ModalPopup (VB) | Документы Microsoft
author: wenz
description: Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако элемент управления не предлагает...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 2d20888674dfedee7a7af85efd8df118c8394c6c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="ae3f3-104">Позиционирование ModalPopup (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="ae3f3-104">Positioning a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="ae3f3-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ae3f3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ae3f3-106">[Загрузить код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ae3f3-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="ae3f3-107">Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств.</span><span class="sxs-lookup"><span data-stu-id="ae3f3-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="ae3f3-108">Однако элемент управления не предоставляет встроенные функциональные возможности для размещения всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="ae3f3-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="ae3f3-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="ae3f3-109">Overview</span></span>

<span data-ttu-id="ae3f3-110">Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств.</span><span class="sxs-lookup"><span data-stu-id="ae3f3-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="ae3f3-111">Однако элемент управления не предоставляет встроенные функциональные возможности для размещения всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="ae3f3-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="ae3f3-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="ae3f3-112">Steps</span></span>

<span data-ttu-id="ae3f3-113">Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="ae3f3-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="ae3f3-114">элемент управления необходимо поместить в любом месте на странице (но в `<form>` элемент):</span><span class="sxs-lookup"><span data-stu-id="ae3f3-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="ae3f3-115">Добавьте панель, который служит в качестве модальное окно.</span><span class="sxs-lookup"><span data-stu-id="ae3f3-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="ae3f3-116">Кнопки используется, чтобы закрыть всплывающее окно:</span><span class="sxs-lookup"><span data-stu-id="ae3f3-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="ae3f3-117">Каждый раз, когда отображается всплывающее окно, его должны размещаться в определенное место на странице.</span><span class="sxs-lookup"><span data-stu-id="ae3f3-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="ae3f3-118">Эта задача создается клиентские функции JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ae3f3-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="ae3f3-119">Сначала пытается получить доступ к панели.</span><span class="sxs-lookup"><span data-stu-id="ae3f3-119">It first tries to access the panel.</span></span> <span data-ttu-id="ae3f3-120">В случае успеха положение панели устанавливается с помощью CSS и JavaScript (изменение будет положение во всплывающем окне).</span><span class="sxs-lookup"><span data-stu-id="ae3f3-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="ae3f3-121">Однако `ModalPopupExtender` управления также пытается положение всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="ae3f3-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="ae3f3-122">Таким образом код JavaScript многократно всплывающее окно, каждые десять раз меньше секунды.</span><span class="sxs-lookup"><span data-stu-id="ae3f3-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="ae3f3-123">Как видите, возвращаемое значение `setTimeout()` метода JavaScript сохраняется в глобальной переменной.</span><span class="sxs-lookup"><span data-stu-id="ae3f3-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="ae3f3-124">Это позволяет остановить повторное размещение всплывающего окна по требованию, с помощью `clearTimeout()` метод:</span><span class="sxs-lookup"><span data-stu-id="ae3f3-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="ae3f3-125">Теперь осталось сделать — сделать вызывать эти функции каждый раз, когда соответствующие браузер.</span><span class="sxs-lookup"><span data-stu-id="ae3f3-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="ae3f3-126">`movePanel()` Функцию JavaScript должен вызываться при нажатии кнопки, которое запускает панели:</span><span class="sxs-lookup"><span data-stu-id="ae3f3-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="ae3f3-127">И `stopMoving()` функции вступает в действие, когда всплывающее окно закрывается, это может быть предприняты в `ModalPopupExtender` управления:</span><span class="sxs-lookup"><span data-stu-id="ae3f3-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="ae3f3-128">[![Модальное окно отображается в указанной позиции](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ae3f3-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="ae3f3-129">Модальное окно отображается в указанной позиции ([Просмотр полноразмерное изображение](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ae3f3-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ae3f3-130">Назад</span><span class="sxs-lookup"><span data-stu-id="ae3f3-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
