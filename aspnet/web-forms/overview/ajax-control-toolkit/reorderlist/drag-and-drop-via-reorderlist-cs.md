---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: Перетаскивание через ReorderList (C#) | Документы Microsoft
author: wenz
description: Элемент управления ReorderList в наборе элементов управления AJAX предоставляет список, который можно переупорядочить пользователем с помощью операции перетаскивания. Текущий порядок элементов в списке должны...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 42464d10f119e0ba51d5eebf2a67e76e3e419bda
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="drag-and-drop-via-reorderlist-c"></a><span data-ttu-id="45606-104">Перетаскивание через ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="45606-104">Drag and Drop via ReorderList (C#)</span></span>
====================
<span data-ttu-id="45606-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="45606-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="45606-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="45606-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span></span>

> <span data-ttu-id="45606-107">Элемент управления ReorderList в наборе элементов управления AJAX предоставляет список, который можно переупорядочить пользователем с помощью операции перетаскивания.</span><span class="sxs-lookup"><span data-stu-id="45606-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="45606-108">Текущий порядок элементов в списке должны быть сохранены на сервере.</span><span class="sxs-lookup"><span data-stu-id="45606-108">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="45606-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="45606-109">Overview</span></span>

<span data-ttu-id="45606-110">`ReorderList` Список элемента управления в наборе элементов управления AJAX можно переупорядочить пользователем с помощью операции перетаскивания.</span><span class="sxs-lookup"><span data-stu-id="45606-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="45606-111">Текущий порядок элементов в списке должны быть сохранены на сервере.</span><span class="sxs-lookup"><span data-stu-id="45606-111">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="45606-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="45606-112">Steps</span></span>

<span data-ttu-id="45606-113">`ReorderList` Элемент управления поддерживает привязку данных из базы данных в список.</span><span class="sxs-lookup"><span data-stu-id="45606-113">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="45606-114">Лучше всего оно также поддерживает запись изменений порядку элемент списка в хранилище данных.</span><span class="sxs-lookup"><span data-stu-id="45606-114">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="45606-115">Этот образец использует в качестве хранилища данных Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="45606-115">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="45606-116">Базы данных является частью необязательно (и бесплатные) установки Visual Studio, включая экспресс-выпуск.</span><span class="sxs-lookup"><span data-stu-id="45606-116">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="45606-117">Эта схема также доступна как отдельный загружаемый под [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="45606-117">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="45606-118">Для данного образца предполагается, что экземпляр SQL Server 2005 Express Edition вызывается `SQLEXPRESS` и находится на том же компьютере, что веб-сервер; Кроме того, это настройки по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="45606-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="45606-119">Если настройки отличаются, необходимо адаптировать сведения о соединении для базы данных.</span><span class="sxs-lookup"><span data-stu-id="45606-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="45606-120">Самый простой способ настроить базу данных будет использовать Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="45606-120">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="45606-121">Подключиться к серверу, дважды щелкните `Databases` и создать новую базу данных (щелкните правой кнопкой мыши и выберите `New Database`) вызывается `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="45606-121">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="45606-122">В этой базе данных, создайте новую таблицу с именем `AJAX` следующие четыре столбца:</span><span class="sxs-lookup"><span data-stu-id="45606-122">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="45606-123">`id` (первичный ключ, целое число со знаком, удостоверение, не ОПРЕДЕЛЕНО)</span><span class="sxs-lookup"><span data-stu-id="45606-123">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="45606-124">`char` (char(1) NULL)</span><span class="sxs-lookup"><span data-stu-id="45606-124">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="45606-125">`description` (varchar(50) NULL)</span><span class="sxs-lookup"><span data-stu-id="45606-125">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="45606-126">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="45606-126">`position` (int, NULL)</span></span>


<span data-ttu-id="45606-127">[![Макет таблицы AJAX](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="45606-127">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="45606-128">Макет таблицы AJAX ([Просмотр полноразмерное изображение](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="45606-128">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span></span>


<span data-ttu-id="45606-129">После этого заполните таблицу с помощью нескольких значений.</span><span class="sxs-lookup"><span data-stu-id="45606-129">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="45606-130">Обратите внимание, что `position` столбец содержит порядок сортировки элементов.</span><span class="sxs-lookup"><span data-stu-id="45606-130">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="45606-131">[![Начальные данные в таблице AJAX](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="45606-131">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span></span>

<span data-ttu-id="45606-132">Начальные данные в таблице AJAX ([Просмотр полноразмерное изображение](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="45606-132">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span></span>


<span data-ttu-id="45606-133">Следующий шаг необходим для создания `SqlDataSource` управления для связи с новой базы данных и соответствующей таблицей.</span><span class="sxs-lookup"><span data-stu-id="45606-133">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="45606-134">Источник данных должен поддерживать `SELECT` и `UPDATE` команды SQL.</span><span class="sxs-lookup"><span data-stu-id="45606-134">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="45606-135">При последующем изменении порядок элементов списка `ReorderList` управления автоматически отправляет двух значений в источник данных `Update` команды: новое положение и идентификатор элемента.</span><span class="sxs-lookup"><span data-stu-id="45606-135">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="45606-136">Таким образом, источника данных требованиям `<UpdateParameters>` раздел для этих двух значений:</span><span class="sxs-lookup"><span data-stu-id="45606-136">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="45606-137">`ReorderList` Элемент управления должен задать следующие атрибуты:</span><span class="sxs-lookup"><span data-stu-id="45606-137">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="45606-138">`AllowReorder`: Ли можно упорядочить элементы списка</span><span class="sxs-lookup"><span data-stu-id="45606-138">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="45606-139">`DataSourceID`: Идентификатор источника данных</span><span class="sxs-lookup"><span data-stu-id="45606-139">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="45606-140">`DataKeyField`: Имя столбца первичного ключа в источнике данных</span><span class="sxs-lookup"><span data-stu-id="45606-140">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="45606-141">`SortOrderField`: Столбца источника данных, представляющий порядок сортировки для элементов списка</span><span class="sxs-lookup"><span data-stu-id="45606-141">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="45606-142">В `<DragHandleTemplate>` и `<ItemTemplate>` разделах макета списка может быть настроена.</span><span class="sxs-lookup"><span data-stu-id="45606-142">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="45606-143">Кроме того, возможна привязка данных с помощью `Eval()` метода, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="45606-143">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="45606-144">Сведения о стиле следующий код CSS (на который ссылается `<DragHandleTemplate>` раздел `ReorderList` управления) гарантирует, что указатель мыши изменяется соответствующим образом при наведении курсора на маркер перетаскивания:</span><span class="sxs-lookup"><span data-stu-id="45606-144">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

<span data-ttu-id="45606-145">Наконец `ScriptManager` элемент управления инициализирует ASP.NET AJAX для страницы:</span><span class="sxs-lookup"><span data-stu-id="45606-145">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="45606-146">Этот пример можно выполнить в браузере и немного изменить порядок элементов списка.</span><span class="sxs-lookup"><span data-stu-id="45606-146">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="45606-147">Затем перезагрузите страницу и/или посмотрите на базе данных.</span><span class="sxs-lookup"><span data-stu-id="45606-147">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="45606-148">Измененная позиций вестись и также отражаются по значениям в `position` столбца в базе данных, которые все без кода, только с помощью разметки.</span><span class="sxs-lookup"><span data-stu-id="45606-148">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="45606-149">[![Данные в базы данных изменяется в соответствии с новой порядок элементов списка](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="45606-149">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span></span>

<span data-ttu-id="45606-150">Данные в базы данных изменяется в соответствии с новый список элементов заказа ([Просмотр полноразмерное изображение](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="45606-150">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="45606-151">[Назад](using-postbacks-with-reorderlist-cs.md)
> [Вперед](using-postbacks-with-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="45606-151">[Previous](using-postbacks-with-reorderlist-cs.md)
[Next](using-postbacks-with-reorderlist-vb.md)</span></span>
