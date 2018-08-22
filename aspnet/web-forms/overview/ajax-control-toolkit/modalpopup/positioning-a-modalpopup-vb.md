---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Размещение ModalPopup (VB) | Документация Майкрософт
author: wenz
description: Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Тем не менее не предлагает элемент управления...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: a507cb173b6dba96ca532a249fa9d495ded7d4e8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837662"
---
<a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="cef35-104">Размещение ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="cef35-104">Positioning a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="cef35-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="cef35-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="cef35-106">[Скачать код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="cef35-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="cef35-107">Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="cef35-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="cef35-108">Однако элемент управления не предоставляет встроенные функциональные возможности для размещения всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="cef35-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="cef35-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="cef35-109">Overview</span></span>

<span data-ttu-id="cef35-110">Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="cef35-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="cef35-111">Однако элемент управления не предоставляет встроенные функциональные возможности для размещения всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="cef35-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="cef35-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="cef35-112">Steps</span></span>

<span data-ttu-id="cef35-113">Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="cef35-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="cef35-114">элемент управления необходимо поместить в любом месте на странице (но в `<form>` элемента):</span><span class="sxs-lookup"><span data-stu-id="cef35-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="cef35-115">Добавьте панель, который служит в качестве модального всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="cef35-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="cef35-116">Кнопка используется для закрытия всплывающего окна:</span><span class="sxs-lookup"><span data-stu-id="cef35-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="cef35-117">Каждый раз, когда всплывающее окно отображается, он должен размещаться в определенное место на странице.</span><span class="sxs-lookup"><span data-stu-id="cef35-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="cef35-118">Для выполнения этой задачи будет создана функция JavaScript на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="cef35-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="cef35-119">Сначала выполняется попытка доступа к панели.</span><span class="sxs-lookup"><span data-stu-id="cef35-119">It first tries to access the panel.</span></span> <span data-ttu-id="cef35-120">Выполняется успешно, положение панели задается с помощью CSS и JavaScript (изменение будет положение всплывающего окна в).</span><span class="sxs-lookup"><span data-stu-id="cef35-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="cef35-121">Тем не менее `ModalPopupExtender` управления также пытается положение всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="cef35-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="cef35-122">Таким образом код JavaScript многократно всплывающее окно, каждую десятую долю секунды.</span><span class="sxs-lookup"><span data-stu-id="cef35-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="cef35-123">Как вы видите, возвращаемое значение `setTimeout()` метод JavaScript сохраняется в глобальной переменной.</span><span class="sxs-lookup"><span data-stu-id="cef35-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="cef35-124">Это позволяет остановить повторное размещение всплывающего окна по требованию, с помощью `clearTimeout()` метод:</span><span class="sxs-lookup"><span data-stu-id="cef35-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="cef35-125">Теперь осталось сделать лишь для вызова этих функций всякий раз, когда соответствующие браузера.</span><span class="sxs-lookup"><span data-stu-id="cef35-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="cef35-126">`movePanel()` Функция JavaScript должен вызываться при нажатии кнопки, который запускает панели:</span><span class="sxs-lookup"><span data-stu-id="cef35-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="cef35-127">И `stopMoving()` функции вступает в действие, когда всплывающее окно закрывается, это может быть предприняты в `ModalPopupExtender` управления:</span><span class="sxs-lookup"><span data-stu-id="cef35-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="cef35-128">[![Появится модальное всплывающее окно в указанной позиции](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cef35-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="cef35-129">Появится модальное всплывающее окно в указанной позиции ([Просмотр полноразмерного изображения](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="cef35-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="cef35-130">Назад</span><span class="sxs-lookup"><span data-stu-id="cef35-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
