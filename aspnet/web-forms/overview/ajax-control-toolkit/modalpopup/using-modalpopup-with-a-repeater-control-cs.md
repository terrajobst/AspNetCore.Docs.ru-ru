---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: Использование ModalPopup с элементом управления Repeater (C#) | Документация Майкрософт
author: wenz
description: Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Можно также использовать этот контракту...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7e6136bac48b7c1b0931cada66501d303be8e927
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371081"
---
<a name="using-modalpopup-with-a-repeater-control-c"></a><span data-ttu-id="aca6d-104">Использование ModalPopup с элементом управления Repeater (C#)</span><span class="sxs-lookup"><span data-stu-id="aca6d-104">Using ModalPopup with a Repeater Control (C#)</span></span>
====================
<span data-ttu-id="aca6d-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="aca6d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="aca6d-106">[Скачать код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="aca6d-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span></span>

> <span data-ttu-id="aca6d-107">Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="aca6d-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="aca6d-108">Можно также использовать этот элемент управления в элементе управления repeater.</span><span class="sxs-lookup"><span data-stu-id="aca6d-108">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="aca6d-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="aca6d-109">Overview</span></span>

<span data-ttu-id="aca6d-110">Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="aca6d-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="aca6d-111">Можно также использовать этот элемент управления в элементе управления repeater.</span><span class="sxs-lookup"><span data-stu-id="aca6d-111">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="aca6d-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="aca6d-112">Steps</span></span>

<span data-ttu-id="aca6d-113">Во-первых источником данных является обязательным.</span><span class="sxs-lookup"><span data-stu-id="aca6d-113">First of all, a data source is required.</span></span> <span data-ttu-id="aca6d-114">В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="aca6d-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="aca6d-115">Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна для загрузки в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="aca6d-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="aca6d-116">Базы данных AdventureWorks является частью образцы SQL Server 2005 и образцов баз данных (загрузить в [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="aca6d-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="aca6d-117">Самый простой способ настроить базы данных является использование Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.</span><span class="sxs-lookup"><span data-stu-id="aca6d-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span> <span data-ttu-id="aca6d-118">В этом примере мы предполагаем, что экземпляр SQL Server 2005 Express Edition называется `SQLEXPRESS` и находится на одном компьютере с веб-сервера; это также настройки по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="aca6d-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="aca6d-119">Если настройки отличаются, вам нужно адаптировать сведения о соединении для базы данных.</span><span class="sxs-lookup"><span data-stu-id="aca6d-119">If your setup differs, you have to adapt the connection information for the database.</span></span> <span data-ttu-id="aca6d-120">Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):</span><span class="sxs-lookup"><span data-stu-id="aca6d-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

<span data-ttu-id="aca6d-121">Затем добавьте источник данных на страницу.</span><span class="sxs-lookup"><span data-stu-id="aca6d-121">Then, add a data source to the page.</span></span> <span data-ttu-id="aca6d-122">Чтобы использовать ограниченный объем данных, мы выбрать только первые пять записей в таблице поставщиков базы данных AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="aca6d-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="aca6d-123">Если вы используете этот помощник используется Visual Studio для создания источника данных, виду, что ошибки в текущей версии не префикса имени таблицы (`Vendor`) с `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="aca6d-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="aca6d-124">В следующей разметке показан правильный синтаксис:</span><span class="sxs-lookup"><span data-stu-id="aca6d-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

<span data-ttu-id="aca6d-125">Добавьте панель, который служит в качестве модального всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="aca6d-125">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="aca6d-126">Он содержит `Button` элемента управления, чтобы закрыть всплывающее окно еще раз:</span><span class="sxs-lookup"><span data-stu-id="aca6d-126">It contains a `Button` control to close the popup again:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

<span data-ttu-id="aca6d-127">Чтобы работать в элементе repeater всплывающее окно `ModalPopupExtender` управления должны быть размещены в `<ItemTemplate>` разделе повторителя.</span><span class="sxs-lookup"><span data-stu-id="aca6d-127">In order to make the popup work within the repeater, the `ModalPopupExtender` control must be put within the `<ItemTemplate>` section of the repeater.</span></span> <span data-ttu-id="aca6d-128">Поэтому панели находится за пределами элемента управления repeater, но расширителя находится внутри.</span><span class="sxs-lookup"><span data-stu-id="aca6d-128">So the panel is outside the repeater, but the extender is inside.</span></span> <span data-ttu-id="aca6d-129">Далее приведена разметка для элемента управления repeater:</span><span class="sxs-lookup"><span data-stu-id="aca6d-129">Here is the markup for the repeater:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

<span data-ttu-id="aca6d-130">Затем с рядом с ним кнопка, инициирующая модальное всплывающее окно отображается каждый элемент в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="aca6d-130">Then, every item in the data source is displayed with a button next to it that triggers the modal popup.</span></span>


<span data-ttu-id="aca6d-131">[![Модального всплывающего окна можно активировать для каждой записи источника данных](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="aca6d-131">[![The modal popup can be triggered for every data source entry](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="aca6d-132">Модального всплывающего окна можно активировать для каждой записи источника данных ([Просмотр полноразмерного изображения](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="aca6d-132">The modal popup can be triggered for every data source entry ([Click to view full-size image](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aca6d-133">[Назад](launching-a-modal-popup-window-from-server-code-cs.md)
> [Вперед](handling-postbacks-from-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="aca6d-133">[Previous](launching-a-modal-popup-window-from-server-code-cs.md)
[Next](handling-postbacks-from-a-modalpopup-cs.md)</span></span>
