---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Размещение ModalPopup (C#) | Документация Майкрософт
author: wenz
description: Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Тем не менее не предлагает элемент управления...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 4f94c0a473b88c0155847d700c48faa34b84c940
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807068"
---
<a name="positioning-a-modalpopup-c"></a><span data-ttu-id="2ddbc-104">Размещение ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="2ddbc-104">Positioning a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="2ddbc-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2ddbc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2ddbc-106">[Скачать код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2ddbc-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span></span>

> <span data-ttu-id="2ddbc-107">Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="2ddbc-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="2ddbc-108">Однако элемент управления не предоставляет встроенные функциональные возможности для размещения всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="2ddbc-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="2ddbc-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="2ddbc-109">Overview</span></span>

<span data-ttu-id="2ddbc-110">Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="2ddbc-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="2ddbc-111">Однако элемент управления не предоставляет встроенные функциональные возможности для размещения всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="2ddbc-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="2ddbc-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="2ddbc-112">Steps</span></span>

<span data-ttu-id="2ddbc-113">Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="2ddbc-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="2ddbc-114">элемент управления необходимо поместить в любом месте на странице (но в `<form>` элемента):</span><span class="sxs-lookup"><span data-stu-id="2ddbc-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="2ddbc-115">Добавьте панель, который служит в качестве модального всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="2ddbc-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="2ddbc-116">Кнопка используется для закрытия всплывающего окна:</span><span class="sxs-lookup"><span data-stu-id="2ddbc-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="2ddbc-117">Каждый раз, когда всплывающее окно отображается, он должен размещаться в определенное место на странице.</span><span class="sxs-lookup"><span data-stu-id="2ddbc-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="2ddbc-118">Для выполнения этой задачи будет создана функция JavaScript на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="2ddbc-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="2ddbc-119">Сначала выполняется попытка доступа к панели.</span><span class="sxs-lookup"><span data-stu-id="2ddbc-119">It first tries to access the panel.</span></span> <span data-ttu-id="2ddbc-120">Выполняется успешно, положение панели задается с помощью CSS и JavaScript (изменение будет положение всплывающего окна в).</span><span class="sxs-lookup"><span data-stu-id="2ddbc-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="2ddbc-121">Тем не менее `ModalPopupExtender` управления также пытается положение всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="2ddbc-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="2ddbc-122">Таким образом код JavaScript многократно всплывающее окно, каждую десятую долю секунды.</span><span class="sxs-lookup"><span data-stu-id="2ddbc-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

<span data-ttu-id="2ddbc-123">Как вы видите, возвращаемое значение `setTimeout()` метод JavaScript сохраняется в глобальной переменной.</span><span class="sxs-lookup"><span data-stu-id="2ddbc-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="2ddbc-124">Это позволяет остановить повторное размещение всплывающего окна по требованию, с помощью `clearTimeout()` метод:</span><span class="sxs-lookup"><span data-stu-id="2ddbc-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

<span data-ttu-id="2ddbc-125">Теперь осталось сделать лишь для вызова этих функций всякий раз, когда соответствующие браузера.</span><span class="sxs-lookup"><span data-stu-id="2ddbc-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="2ddbc-126">`movePanel()` Функция JavaScript должен вызываться при нажатии кнопки, который запускает панели:</span><span class="sxs-lookup"><span data-stu-id="2ddbc-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

<span data-ttu-id="2ddbc-127">И `stopMoving()` функции вступает в действие, когда всплывающее окно закрывается, это может быть предприняты в `ModalPopupExtender` управления:</span><span class="sxs-lookup"><span data-stu-id="2ddbc-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


<span data-ttu-id="2ddbc-128">[![Появится модальное всплывающее окно в указанной позиции](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2ddbc-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="2ddbc-129">Появится модальное всплывающее окно в указанной позиции ([Просмотр полноразмерного изображения](positioning-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2ddbc-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2ddbc-130">[Назад](handling-postbacks-from-a-modalpopup-cs.md)
> [Вперед](launching-a-modal-popup-window-from-server-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2ddbc-130">[Previous](handling-postbacks-from-a-modalpopup-cs.md)
[Next](launching-a-modal-popup-window-from-server-code-vb.md)</span></span>
