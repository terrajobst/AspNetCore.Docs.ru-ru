---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Обработка обратные передачи из ModalPopup (C#) | Документы Microsoft
author: wenz
description: Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Специальные необходимо соблюдать осторожность при pos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 183725db62ba8b4037f368ed9d87d5059e3f1bcb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873736"
---
<a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="c3b48-104">Обработка обратные передачи из ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="c3b48-104">Handling Postbacks from a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="c3b48-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c3b48-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c3b48-106">[Загрузить код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c3b48-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="c3b48-107">Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств.</span><span class="sxs-lookup"><span data-stu-id="c3b48-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c3b48-108">Необходимо уделить особое, во время обратной передачи из в контекстное меню.</span><span class="sxs-lookup"><span data-stu-id="c3b48-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="c3b48-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="c3b48-109">Overview</span></span>

<span data-ttu-id="c3b48-110">Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств.</span><span class="sxs-lookup"><span data-stu-id="c3b48-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c3b48-111">Необходимо уделить особое, во время обратной передачи из в контекстное меню.</span><span class="sxs-lookup"><span data-stu-id="c3b48-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="c3b48-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="c3b48-112">Steps</span></span>

<span data-ttu-id="c3b48-113">Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):</span><span class="sxs-lookup"><span data-stu-id="c3b48-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="c3b48-114">Добавьте панель, который служит в качестве модальное окно.</span><span class="sxs-lookup"><span data-stu-id="c3b48-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="c3b48-115">Нет пользователь может ввести имя и адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="c3b48-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="c3b48-116">Чтобы закрыть всплывающее окно и сохраните используется кнопка.</span><span class="sxs-lookup"><span data-stu-id="c3b48-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="c3b48-117">Обратите внимание, что `OnClick` атрибут имеет значение, чтобы обратной передачи возникает при нажатии этой кнопки:</span><span class="sxs-lookup"><span data-stu-id="c3b48-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="c3b48-118">Сама страница состоит из двух меток для точно те же данные: имя и адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="c3b48-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="c3b48-119">Кнопки используется для запуска модальное окно:</span><span class="sxs-lookup"><span data-stu-id="c3b48-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="c3b48-120">Для упрощения всплывающее окно отображается, добавьте `ModalPopupExtender` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="c3b48-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="c3b48-121">Задать `PopupControlID` атрибут ID панели и `TargetControlID` кнопки с идентификатором:</span><span class="sxs-lookup"><span data-stu-id="c3b48-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="c3b48-122">Теперь каждый раз, когда `Save` в модальное окно кнопки, сервере `SaveData()` выполнения метода.</span><span class="sxs-lookup"><span data-stu-id="c3b48-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="c3b48-123">Нет может сохранить введенные данные в хранилище данных.</span><span class="sxs-lookup"><span data-stu-id="c3b48-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="c3b48-124">Для простоты новые данные просто вывести в метке:</span><span class="sxs-lookup"><span data-stu-id="c3b48-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="c3b48-125">Кроме того элементы управления textbox в модальное окно должен быть заполнен текущего имени и по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="c3b48-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="c3b48-126">Однако это требуется только при возникновении без обратной передачи.</span><span class="sxs-lookup"><span data-stu-id="c3b48-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="c3b48-127">Если обратную передачу, функция viewstate ASP.NET автоматически вводится текстовых полей с соответствующими значениями.</span><span class="sxs-lookup"><span data-stu-id="c3b48-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


<span data-ttu-id="c3b48-128">[![Модальное окно вызывает обратную передачу](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c3b48-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="c3b48-129">Модальное окно вызывает обратную передачу ([Просмотр полноразмерное изображение](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c3b48-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c3b48-130">[Назад](using-modalpopup-with-a-repeater-control-cs.md)
> [Вперед](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="c3b48-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
