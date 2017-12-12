---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: "Обработка операций обратной передачи из контекстного меню элемента управления с помощью элемента управления UpdatePanel (Visual Basic) | Документы Microsoft"
author: wenz
description: "Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. Особое внимание не надо..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 4445437f25925429d240b7fe2d019855afc52fe0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="e04a2-104">Обработка операций обратной передачи из контекстного меню элемента управления с помощью элемента управления UpdatePanel (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="e04a2-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="e04a2-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e04a2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e04a2-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e04a2-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="e04a2-107">Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="e04a2-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="e04a2-108">Особое внимание должен принимать при обратной передаче в таких всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="e04a2-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="e04a2-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="e04a2-109">Overview</span></span>

<span data-ttu-id="e04a2-110">Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="e04a2-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="e04a2-111">Особое внимание должен принимать при обратной передаче в таких всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="e04a2-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="e04a2-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="e04a2-112">Steps</span></span>

<span data-ttu-id="e04a2-113">При использовании `PopupControl` при обратной передаче, `UpdatePanel` может предотвратить обновление страницы, вызванные обратной передачи.</span><span class="sxs-lookup"><span data-stu-id="e04a2-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="e04a2-114">Приведенная ниже разметка определяет ряд важных элементов:</span><span class="sxs-lookup"><span data-stu-id="e04a2-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="e04a2-115">Объект `ScriptManager` управления, что набор элементов управления ASP.NET AJAX</span><span class="sxs-lookup"><span data-stu-id="e04a2-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="e04a2-116">Два `TextBox` элементов управления, которые будут активировать всплывающего окна</span><span class="sxs-lookup"><span data-stu-id="e04a2-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="e04a2-117">Объект `Panel` элемент управления, который будет использоваться в качестве всплывающего окна</span><span class="sxs-lookup"><span data-stu-id="e04a2-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="e04a2-118">На панели `Calendar` управления, встроенный в `UpdatePanel` управления</span><span class="sxs-lookup"><span data-stu-id="e04a2-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="e04a2-119">Два `PopupControlExtender` элементов управления, назначьте панели текстовые поля</span><span class="sxs-lookup"><span data-stu-id="e04a2-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="e04a2-120">Обратите внимание, что `OnSelectionChanged` атрибут `Calendar` элемента управления задано.</span><span class="sxs-lookup"><span data-stu-id="e04a2-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="e04a2-121">Так происходит, когда пользователь выбирает дату в календаре, обратная передача и серверного метода `c1_SelectionChanged()` выполняется.</span><span class="sxs-lookup"><span data-stu-id="e04a2-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="e04a2-122">В этом методе необходимо извлечь текущую дату и записана в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="e04a2-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="e04a2-123">Используется следующий синтаксис для этого: во-первых, прокси-объект для `PopupControlExtender` на странице должен быть создан.</span><span class="sxs-lookup"><span data-stu-id="e04a2-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="e04a2-124">Предоставляет набор элементов управления ASP.NET AJAX `GetProxyForCurrentPopup()` метод.</span><span class="sxs-lookup"><span data-stu-id="e04a2-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="e04a2-125">Этот метод возвращает объект поддерживает `Commit()` метод, который отправляет значение элемента управления, являющегося контекстное меню (не элемента управления, являющегося вызова метода!).</span><span class="sxs-lookup"><span data-stu-id="e04a2-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="e04a2-126">В следующем коде представлен в качестве аргумента для выбранной даты `Commit()` метода, вынуждая кода на обратную запись выбранную дату в текстовом поле:</span><span class="sxs-lookup"><span data-stu-id="e04a2-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="e04a2-127">Теперь по щелчку календарная дата, выбранная дата отображается в соответствующем текстовом поле Создание управляющий элемент выбора даты, в настоящее время находятся на многих веб-сайтах.</span><span class="sxs-lookup"><span data-stu-id="e04a2-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="e04a2-128">[![Календарь появляется, когда пользователь щелкает в текстовое поле](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e04a2-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="e04a2-129">Календарь появляется, когда пользователь щелкает в текстовое поле ([Просмотр полноразмерное изображение](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e04a2-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="e04a2-130">[![Щелкните дату помещает его в текстовое поле](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="e04a2-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="e04a2-131">Щелкните дату помещает его в текстовое поле ([Просмотр полноразмерное изображение](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="e04a2-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e04a2-132">[Назад](using-multiple-popup-controls-vb.md)
[Вперед](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e04a2-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
