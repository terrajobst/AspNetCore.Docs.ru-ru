---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Обработка обратных передач из ModalPopup (C#) | Документация Майкрософт
author: wenz
description: Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Особое внимание следует принимать при терминалом...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 0f27d28b9851f8e26c207a6bc495e98ad70577a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838126"
---
<a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="879ea-104">Обработка обратных передач из ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="879ea-104">Handling Postbacks from a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="879ea-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="879ea-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="879ea-106">[Скачать код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="879ea-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="879ea-107">Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="879ea-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="879ea-108">Специальные необходимо соблюдать осторожность при обратной передачи из в контекстное меню.</span><span class="sxs-lookup"><span data-stu-id="879ea-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="879ea-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="879ea-109">Overview</span></span>

<span data-ttu-id="879ea-110">Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="879ea-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="879ea-111">Специальные необходимо соблюдать осторожность при обратной передачи из в контекстное меню.</span><span class="sxs-lookup"><span data-stu-id="879ea-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="879ea-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="879ea-112">Steps</span></span>

<span data-ttu-id="879ea-113">Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):</span><span class="sxs-lookup"><span data-stu-id="879ea-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="879ea-114">Добавьте панель, который служит в качестве модального всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="879ea-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="879ea-115">Существует пользователь может ввести имя и адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="879ea-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="879ea-116">Кнопка позволяет закрыть всплывающее окно и сохранить их.</span><span class="sxs-lookup"><span data-stu-id="879ea-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="879ea-117">Обратите внимание, что `OnClick` атрибут имеет значение, что обратная передача происходит при нажатии этой кнопки:</span><span class="sxs-lookup"><span data-stu-id="879ea-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="879ea-118">Сама страница состоит из двух меток для точно те же данные: имя и адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="879ea-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="879ea-119">Кнопка используется для запуска модального всплывающего окна:</span><span class="sxs-lookup"><span data-stu-id="879ea-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="879ea-120">Чтобы сделать всплывающее окно отображается, добавьте `ModalPopupExtender` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="879ea-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="879ea-121">Задайте `PopupControlID` атрибут ID панели и `TargetControlID` идентификатору кнопки:</span><span class="sxs-lookup"><span data-stu-id="879ea-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="879ea-122">Теперь всякий раз, когда `Save` нажатии кнопки внутри модального всплывающего окна на стороне сервера `SaveData()` выполнения метода.</span><span class="sxs-lookup"><span data-stu-id="879ea-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="879ea-123">Здесь вы можете сохранить введенные данные в хранилище данных.</span><span class="sxs-lookup"><span data-stu-id="879ea-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="879ea-124">Для простоты новые данные, просто вывести в метке:</span><span class="sxs-lookup"><span data-stu-id="879ea-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="879ea-125">Кроме того элементы управления textbox внутри модального всплывающего окна должен быть заполнен действующие имя и адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="879ea-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="879ea-126">Тем не менее это требуется только при отсутствии обратной передачи.</span><span class="sxs-lookup"><span data-stu-id="879ea-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="879ea-127">Если обратная передача, функция viewstate ASP.NET автоматически заполнят текстовых полей с соответствующими значениями.</span><span class="sxs-lookup"><span data-stu-id="879ea-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


<span data-ttu-id="879ea-128">[![Модальное всплывающее окно вызывает обратную передачу](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="879ea-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="879ea-129">Модальное всплывающее окно вызывает обратную передачу ([Просмотр полноразмерного изображения](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="879ea-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="879ea-130">[Назад](using-modalpopup-with-a-repeater-control-cs.md)
> [Вперед](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="879ea-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
