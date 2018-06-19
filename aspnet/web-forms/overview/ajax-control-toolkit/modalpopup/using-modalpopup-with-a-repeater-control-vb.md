---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
title: Использование ModalPopup с элементом управления повторителем (VB) | Документы Microsoft
author: wenz
description: Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Можно также использовать этот контракту...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0c8e74f1-b3ba-4ca9-a1c5-f5c4831a359a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 04e3b132d1de2f42ba5de113dfbc22c85c3b198e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870925"
---
<a name="using-modalpopup-with-a-repeater-control-vb"></a><span data-ttu-id="4eaac-104">Использование ModalPopup с элементом управления повторителем (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="4eaac-104">Using ModalPopup with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="4eaac-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4eaac-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4eaac-106">[Загрузить код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4eaac-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)</span></span>

> <span data-ttu-id="4eaac-107">Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств.</span><span class="sxs-lookup"><span data-stu-id="4eaac-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="4eaac-108">Можно также использовать этот элемент управления в пределах повторитель.</span><span class="sxs-lookup"><span data-stu-id="4eaac-108">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="4eaac-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="4eaac-109">Overview</span></span>

<span data-ttu-id="4eaac-110">Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств.</span><span class="sxs-lookup"><span data-stu-id="4eaac-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="4eaac-111">Можно также использовать этот элемент управления в пределах повторитель.</span><span class="sxs-lookup"><span data-stu-id="4eaac-111">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="4eaac-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="4eaac-112">Steps</span></span>

<span data-ttu-id="4eaac-113">Во-первых источник данных является обязательным.</span><span class="sxs-lookup"><span data-stu-id="4eaac-113">First of all, a data source is required.</span></span> <span data-ttu-id="4eaac-114">В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="4eaac-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="4eaac-115">Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна как отдельный загружаемый в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="4eaac-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="4eaac-116">Базы данных AdventureWorks входит в состав образцов баз данных и образцы SQL Server 2005 (загрузить в [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="4eaac-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="4eaac-117">Самый простой способ настроить базы данных будет использовать Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.</span><span class="sxs-lookup"><span data-stu-id="4eaac-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span> <span data-ttu-id="4eaac-118">Для данного образца предполагается, что экземпляр SQL Server 2005 Express Edition вызывается `SQLEXPRESS` и находится на том же компьютере, что веб-сервер; Кроме того, это настройки по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4eaac-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="4eaac-119">Если настройки отличаются, необходимо адаптировать сведения о соединении для базы данных.</span><span class="sxs-lookup"><span data-stu-id="4eaac-119">If your setup differs, you have to adapt the connection information for the database.</span></span> <span data-ttu-id="4eaac-120">Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):</span><span class="sxs-lookup"><span data-stu-id="4eaac-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="4eaac-121">Затем добавьте источник данных на страницу.</span><span class="sxs-lookup"><span data-stu-id="4eaac-121">Then, add a data source to the page.</span></span> <span data-ttu-id="4eaac-122">Чтобы использовать ограниченный объем данных, мы выбрать только первых пяти записей в таблице поставщиков базы данных AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="4eaac-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="4eaac-123">При использовании помощника по Visual Studio для создания источника данных, помните, что ошибки в текущей версии не предшествует имени таблицы (`Vendor`) с `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="4eaac-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="4eaac-124">В следующем примере показана правильный синтаксис:</span><span class="sxs-lookup"><span data-stu-id="4eaac-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="4eaac-125">Добавьте панель, который служит в качестве модальное окно.</span><span class="sxs-lookup"><span data-stu-id="4eaac-125">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="4eaac-126">Он содержит `Button` элемента управления, чтобы закрыть всплывающее окно еще раз:</span><span class="sxs-lookup"><span data-stu-id="4eaac-126">It contains a `Button` control to close the popup again:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="4eaac-127">Чтобы работать в повторителя всплывающее окно `ModalPopupExtender` управления необходимо поместить в `<ItemTemplate>` разделе повторителя.</span><span class="sxs-lookup"><span data-stu-id="4eaac-127">In order to make the popup work within the repeater, the `ModalPopupExtender` control must be put within the `<ItemTemplate>` section of the repeater.</span></span> <span data-ttu-id="4eaac-128">Поэтому панель находится за пределами повторителя, но расширителя находится внутри.</span><span class="sxs-lookup"><span data-stu-id="4eaac-128">So the panel is outside the repeater, but the extender is inside.</span></span> <span data-ttu-id="4eaac-129">Вот разметку для повторителя.</span><span class="sxs-lookup"><span data-stu-id="4eaac-129">Here is the markup for the repeater:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="4eaac-130">Затем с рядом с кнопкой, которая запускает модальное окно отображается каждый элемент в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="4eaac-130">Then, every item in the data source is displayed with a button next to it that triggers the modal popup.</span></span>


<span data-ttu-id="4eaac-131">[![Модальное окно можно запустить для каждой записи источника данных](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4eaac-131">[![The modal popup can be triggered for every data source entry](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="4eaac-132">Модальное окно можно запустить для каждой записи источника данных ([Просмотр полноразмерное изображение](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4eaac-132">The modal popup can be triggered for every data source entry ([Click to view full-size image](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4eaac-133">[Назад](launching-a-modal-popup-window-from-server-code-vb.md)
> [Вперед](handling-postbacks-from-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4eaac-133">[Previous](launching-a-modal-popup-window-from-server-code-vb.md)
[Next](handling-postbacks-from-a-modalpopup-vb.md)</span></span>
