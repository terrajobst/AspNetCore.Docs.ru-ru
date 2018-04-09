---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Открыв окно модальное окно из серверного кода (VB) | Документы Microsoft
author: wenz
description: Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако в некоторых сценариях требуется, t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 46554dae60ad9cd13e97e5755e95cb2125d1fed9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="974ca-104">Открыв окно модальное окно из серверного кода (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="974ca-104">Launching a Modal Popup Window from Server Code (VB)</span></span>
====================
<span data-ttu-id="974ca-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="974ca-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="974ca-106">[Загрузить код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="974ca-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="974ca-107">Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств.</span><span class="sxs-lookup"><span data-stu-id="974ca-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="974ca-108">Однако в некоторых сценариях требуется запуск открывающий модальное окно на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="974ca-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="974ca-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="974ca-109">Overview</span></span>

<span data-ttu-id="974ca-110">Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств.</span><span class="sxs-lookup"><span data-stu-id="974ca-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="974ca-111">Однако в некоторых сценариях требуется запуск открывающий модальное окно на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="974ca-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="974ca-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="974ca-112">Steps</span></span>

<span data-ttu-id="974ca-113">Во-первых веб-элемента управления кнопки ASP.NET требуется продемонстрировать, как работает элемент управления ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="974ca-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="974ca-114">Добавление кнопки в &lt;формы&gt; элемент на новую страницу:</span><span class="sxs-lookup"><span data-stu-id="974ca-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="974ca-115">Затем необходимо разметку для контекстного меню, который требуется создать.</span><span class="sxs-lookup"><span data-stu-id="974ca-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="974ca-116">Определить ее как `<asp:Panel>` управления и убедитесь, что он включает элемент управления Button.</span><span class="sxs-lookup"><span data-stu-id="974ca-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="974ca-117">Элемент управления ModalPopup предоставляет функциональные возможности для создания таких кнопки Закрыть всплывающее окно; в противном случае нет никакого простого способа запускать его удалить.</span><span class="sxs-lookup"><span data-stu-id="974ca-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="974ca-118">Затем добавьте элемент управления ModalPopup из набора средств ASP.NET AJAX на страницу.</span><span class="sxs-lookup"><span data-stu-id="974ca-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="974ca-119">Задание свойств для кнопку, которая загружается элемент управления, кнопки, что делает его исчезают и идентификатор фактическое всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="974ca-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="974ca-120">Как и все веб-страницы, на базе ASP.NET AJAX; Диспетчера скриптов, необходимой для загрузки необходимые библиотеки JavaScript для различных целевых браузеров:</span><span class="sxs-lookup"><span data-stu-id="974ca-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="974ca-121">В браузере Запустите пример.</span><span class="sxs-lookup"><span data-stu-id="974ca-121">Run the example in the browser.</span></span> <span data-ttu-id="974ca-122">При нажатии на кнопке отображается модальное окно.</span><span class="sxs-lookup"><span data-stu-id="974ca-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="974ca-123">Для достижения такого же эффекта, с помощью кода на стороне сервера, требуется новая кнопка:</span><span class="sxs-lookup"><span data-stu-id="974ca-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="974ca-124">Как видно, нажмите кнопку создает обратную передачу и выполняет `ServerButton_Click()` метод на сервере.</span><span class="sxs-lookup"><span data-stu-id="974ca-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="974ca-125">Этот метод вызывается функция JavaScript `launchModal()` выполняется быть точным, функция JavaScript, которая выполняется после загрузки страницы:</span><span class="sxs-lookup"><span data-stu-id="974ca-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="974ca-126">Задача `launchModal()` — отображение ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="974ca-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="974ca-127">`launchModal()` Функция выполняется после загрузки страницы HTML.</span><span class="sxs-lookup"><span data-stu-id="974ca-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="974ca-128">В этот момент тем не менее, платформа AJAX для ASP.NET не был полностью загружен еще.</span><span class="sxs-lookup"><span data-stu-id="974ca-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="974ca-129">Таким образом `launchModal()` функция просто задает переменную, которая управления ModalPopup должно быть показано ниже на:</span><span class="sxs-lookup"><span data-stu-id="974ca-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="974ca-130">`pageLoad()` Функцию JavaScript — это специальная функция, который исполняется после полной загрузки ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="974ca-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="974ca-131">Поэтому мы добавьте код для этой функции для отображения элемента управления ModalPopup, но только если `launchModal()` был вызван до:</span><span class="sxs-lookup"><span data-stu-id="974ca-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="974ca-132">`$find()` Функция ищет именованного элемента на странице и ожидает в качестве параметра идентификатор серверные.</span><span class="sxs-lookup"><span data-stu-id="974ca-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="974ca-133">Таким образом `$find("mpe")` возвращает представление клиента управления ModalPopup; его `show()` метод позволяет отображается всплывающее окно.</span><span class="sxs-lookup"><span data-stu-id="974ca-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="974ca-134">[![Модальное окно появляется при нажатии любой кнопки](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="974ca-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="974ca-135">Модальное окно появляется при нажатии любой кнопки ([Просмотр полноразмерное изображение](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="974ca-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="974ca-136">[Назад](positioning-a-modalpopup-cs.md)
> [Вперед](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="974ca-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
