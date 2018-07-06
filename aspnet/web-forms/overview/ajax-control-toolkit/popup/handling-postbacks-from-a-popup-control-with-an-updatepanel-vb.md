---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Обработка операций обратной передачи из элемента управления всплывающего окна с помощью UpdatePanel (VB) | Документация Майкрософт
author: wenz
description: Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления. Особое внимание не надо...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 2cd6f03b805ce209b736ab9c105fadd3ba6ac26b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826148"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="7b0a3-104">Обработка операций обратной передачи из элемента управления всплывающего окна с помощью UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="7b0a3-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="7b0a3-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7b0a3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7b0a3-106">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7b0a3-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="7b0a3-107">Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="7b0a3-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7b0a3-108">Особое внимание приходится принимать при обратной передаче в таких всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="7b0a3-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="7b0a3-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="7b0a3-109">Overview</span></span>

<span data-ttu-id="7b0a3-110">Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="7b0a3-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7b0a3-111">Особое внимание приходится принимать при обратной передаче в таких всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="7b0a3-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="7b0a3-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="7b0a3-112">Steps</span></span>

<span data-ttu-id="7b0a3-113">При использовании `PopupControl` при обратной передаче, `UpdatePanel` можно предотвратить обновление страницы, из-за обратной передачи.</span><span class="sxs-lookup"><span data-stu-id="7b0a3-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="7b0a3-114">Следующая разметка определяет несколько важных элементов:</span><span class="sxs-lookup"><span data-stu-id="7b0a3-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="7b0a3-115">Объект `ScriptManager` управления, чтобы обеспечить работу ASP.NET AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="7b0a3-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="7b0a3-116">Два `TextBox` элементов управления, которые будут активировать всплывающее окно</span><span class="sxs-lookup"><span data-stu-id="7b0a3-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="7b0a3-117">Объект `Panel` элемент управления, который будет использоваться в качестве всплывающего окна</span><span class="sxs-lookup"><span data-stu-id="7b0a3-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="7b0a3-118">На панели `Calendar` управления, встроенный в `UpdatePanel` элемента управления</span><span class="sxs-lookup"><span data-stu-id="7b0a3-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="7b0a3-119">Два `PopupControlExtender` элементов управления, назначить панель текстовые поля</span><span class="sxs-lookup"><span data-stu-id="7b0a3-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="7b0a3-120">Обратите внимание, что `OnSelectionChanged` атрибут `Calendar` элемента управления задано значение.</span><span class="sxs-lookup"><span data-stu-id="7b0a3-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="7b0a3-121">Поэтому когда пользователь выбирает дату в календаре, происходит обратная передача и серверного метода `c1_SelectionChanged()` выполняется.</span><span class="sxs-lookup"><span data-stu-id="7b0a3-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="7b0a3-122">В этом методе необходимо извлечь текущую дату и записана в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="7b0a3-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="7b0a3-123">Синтаксис, выглядит следующим образом: во-первых, прокси-сервер для объекта `PopupControlExtender` на странице должен быть создан.</span><span class="sxs-lookup"><span data-stu-id="7b0a3-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="7b0a3-124">ASP.NET AJAX Control Toolkit предлагает `GetProxyForCurrentPopup()` метод.</span><span class="sxs-lookup"><span data-stu-id="7b0a3-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="7b0a3-125">Этот метод возвращает объект поддерживает `Commit()` метод, который отправляет значение элемента управления, являющегося всплывающее окно (не элемента управления, являющегося вызова метода!).</span><span class="sxs-lookup"><span data-stu-id="7b0a3-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="7b0a3-126">В следующем коде представлен выбранную дату в качестве аргумента для `Commit()` методом, выполняемым кодом для обратной записи выбранную дату в текстовом поле:</span><span class="sxs-lookup"><span data-stu-id="7b0a3-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="7b0a3-127">Теперь всякий раз при нажатии на дату, отображается выбранную дату в соответствующем текстовом поле, создание элемента выбора даты, в настоящее время находятся на многих веб-сайтах.</span><span class="sxs-lookup"><span data-stu-id="7b0a3-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="7b0a3-128">[![Календарь появляется, когда пользователь нажимает кнопку в текстовое поле](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7b0a3-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="7b0a3-129">Календарь появляется, когда пользователь нажимает кнопку в текстовое поле ([Просмотр полноразмерного изображения](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7b0a3-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="7b0a3-130">[![Щелкните дату помещает его в текстовое поле](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="7b0a3-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="7b0a3-131">Щелкните дату помещает его в текстовое поле ([Просмотр полноразмерного изображения](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="7b0a3-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7b0a3-132">[Назад](using-multiple-popup-controls-vb.md)
> [Вперед](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7b0a3-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
