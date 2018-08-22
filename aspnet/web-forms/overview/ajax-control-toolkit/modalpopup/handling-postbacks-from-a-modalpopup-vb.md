---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Обработка обратных передач из ModalPopup (VB) | Документация Майкрософт
author: wenz
description: Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Особое внимание следует принимать при терминалом...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f81a2bf1866d026046fdba815fdbae1b162c1dd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837023"
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="f8a47-104">Обработка обратных передач из ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="f8a47-104">Handling Postbacks from a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="f8a47-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f8a47-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f8a47-106">[Скачать код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f8a47-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="f8a47-107">Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="f8a47-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="f8a47-108">Специальные необходимо соблюдать осторожность при обратной передачи из в контекстное меню.</span><span class="sxs-lookup"><span data-stu-id="f8a47-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="f8a47-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="f8a47-109">Overview</span></span>

<span data-ttu-id="f8a47-110">Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="f8a47-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="f8a47-111">Специальные необходимо соблюдать осторожность при обратной передачи из в контекстное меню.</span><span class="sxs-lookup"><span data-stu-id="f8a47-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="f8a47-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="f8a47-112">Steps</span></span>

<span data-ttu-id="f8a47-113">Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):</span><span class="sxs-lookup"><span data-stu-id="f8a47-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="f8a47-114">Добавьте панель, который служит в качестве модального всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="f8a47-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="f8a47-115">Существует пользователь может ввести имя и адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="f8a47-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="f8a47-116">Кнопка позволяет закрыть всплывающее окно и сохранить их.</span><span class="sxs-lookup"><span data-stu-id="f8a47-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="f8a47-117">Обратите внимание, что `OnClick` атрибут имеет значение, что обратная передача происходит при нажатии этой кнопки:</span><span class="sxs-lookup"><span data-stu-id="f8a47-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="f8a47-118">Сама страница состоит из двух меток для точно те же данные: имя и адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="f8a47-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="f8a47-119">Кнопка используется для запуска модального всплывающего окна:</span><span class="sxs-lookup"><span data-stu-id="f8a47-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="f8a47-120">Чтобы сделать всплывающее окно отображается, добавьте `ModalPopupExtender` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="f8a47-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="f8a47-121">Задайте `PopupControlID` атрибут ID панели и `TargetControlID` идентификатору кнопки:</span><span class="sxs-lookup"><span data-stu-id="f8a47-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="f8a47-122">Теперь всякий раз, когда `Save` нажатии кнопки внутри модального всплывающего окна на стороне сервера `SaveData()` выполнения метода.</span><span class="sxs-lookup"><span data-stu-id="f8a47-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="f8a47-123">Здесь вы можете сохранить введенные данные в хранилище данных.</span><span class="sxs-lookup"><span data-stu-id="f8a47-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="f8a47-124">Для простоты новые данные, просто вывести в метке:</span><span class="sxs-lookup"><span data-stu-id="f8a47-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="f8a47-125">Кроме того элементы управления textbox внутри модального всплывающего окна должен быть заполнен действующие имя и адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="f8a47-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="f8a47-126">Тем не менее это требуется только при отсутствии обратной передачи.</span><span class="sxs-lookup"><span data-stu-id="f8a47-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="f8a47-127">Если обратная передача, функция viewstate ASP.NET автоматически заполнят текстовых полей с соответствующими значениями.</span><span class="sxs-lookup"><span data-stu-id="f8a47-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


<span data-ttu-id="f8a47-128">[![Модальное всплывающее окно вызывает обратную передачу](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f8a47-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="f8a47-129">Модальное всплывающее окно вызывает обратную передачу ([Просмотр полноразмерного изображения](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f8a47-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f8a47-130">[Назад](using-modalpopup-with-a-repeater-control-vb.md)
> [Вперед](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f8a47-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
