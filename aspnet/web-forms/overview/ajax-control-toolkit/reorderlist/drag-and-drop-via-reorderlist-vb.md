---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Перетаскивание с помощью элемента управления ReorderList (VB) | Документация Майкрософт
author: wenz
description: /Data-Access/tutorials/master-Detail-using-a-bulleted-List-of-master-Records-with-a-Details-DataList-VB
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: d8b10ecd4c061082aba5cefe9d92b8616e9f40e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389167"
---
<a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="b0c7a-103">Перетаскивание с помощью элемента управления ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="b0c7a-103">Drag and Drop via ReorderList (VB)</span></span>
====================
<span data-ttu-id="b0c7a-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b0c7a-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b0c7a-105">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b0c7a-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="b0c7a-106">Элемент управления ReorderList в AJAX Control Toolkit предоставляет список, можно ли изменять расположение пользователем с помощью операции перетаскивания.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="b0c7a-107">Текущий порядок элементов в списке должны быть сохранены на сервере.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-107">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="b0c7a-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="b0c7a-108">Overview</span></span>

<span data-ttu-id="b0c7a-109">`ReorderList` Элемент управления в AJAX Control Toolkit предоставляет список, можно ли изменять расположение пользователем с помощью операции перетаскивания.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="b0c7a-110">Текущий порядок элементов в списке должны быть сохранены на сервере.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="b0c7a-111">Шаги</span><span class="sxs-lookup"><span data-stu-id="b0c7a-111">Steps</span></span>

<span data-ttu-id="b0c7a-112">`ReorderList` Элемент управления поддерживает привязку данных из базы данных в список.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="b0c7a-113">Самое главное она также поддерживает запись изменений порядку элемент списка в хранилище данных.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="b0c7a-114">Этот образец использует Microsoft SQL Server 2005 Express Edition в качестве хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="b0c7a-115">Базы данных является необязательным (и бесплатной) часть установки Visual Studio, включая экспресс-выпуск.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="b0c7a-116">Эта схема также доступна для загрузки в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="b0c7a-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="b0c7a-117">В этом примере мы предполагаем, что экземпляр SQL Server 2005 Express Edition называется `SQLEXPRESS` и находится на одном компьютере с веб-сервера; это также настройки по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="b0c7a-118">Если настройки отличаются, вам нужно адаптировать сведения о соединении для базы данных.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="b0c7a-119">Самый простой способ установить базу данных, — использовать Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="b0c7a-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="b0c7a-120">Подключитесь к серверу, дважды щелкните `Databases` и создать новую базу данных (щелкните правой кнопкой мыши и выберите `New Database`) вызывается `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="b0c7a-121">В этой базе данных, создать новую таблицу с именем `AJAX` с следующие четыре столбца:</span><span class="sxs-lookup"><span data-stu-id="b0c7a-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="b0c7a-122">`id` (первичный ключ, integer, удостоверение, not NULL)</span><span class="sxs-lookup"><span data-stu-id="b0c7a-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="b0c7a-123">`char` (char(1), NULL)</span><span class="sxs-lookup"><span data-stu-id="b0c7a-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="b0c7a-124">`description` (varchar(50), NULL)</span><span class="sxs-lookup"><span data-stu-id="b0c7a-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="b0c7a-125">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="b0c7a-125">`position` (int, NULL)</span></span>


<span data-ttu-id="b0c7a-126">[![Макет таблицы AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b0c7a-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="b0c7a-127">Макет таблицы AJAX ([Просмотр полноразмерного изображения](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b0c7a-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="b0c7a-128">Затем введите таблицы с несколькими значениями.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="b0c7a-129">Обратите внимание, что `position` столбец содержит порядок сортировки элементов.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-129">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="b0c7a-130">[![Начальные данные в таблице AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="b0c7a-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="b0c7a-131">Начальные данные в таблице AJAX ([Просмотр полноразмерного изображения](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b0c7a-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="b0c7a-132">Для следующего шага требуется для создания `SqlDataSource` управления для взаимодействия с новой базы данных и соответствующей таблицей.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="b0c7a-133">Источник данных должен поддерживать `SELECT` и `UPDATE` команды SQL.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="b0c7a-134">При последующем изменении порядок элементов списка `ReorderList` элемент управления автоматически отправляет два значения в источник данных `Update` команда: новое положение и идентификатор элемента.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="b0c7a-135">Таким образом, источника данных требованиям `<UpdateParameters>` раздел для этих двух значений:</span><span class="sxs-lookup"><span data-stu-id="b0c7a-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="b0c7a-136">`ReorderList` Управления необходимо задать следующие атрибуты:</span><span class="sxs-lookup"><span data-stu-id="b0c7a-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="b0c7a-137">`AllowReorder`: Ли можно упорядочить элементы списка</span><span class="sxs-lookup"><span data-stu-id="b0c7a-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="b0c7a-138">`DataSourceID`: Идентификатор источника данных</span><span class="sxs-lookup"><span data-stu-id="b0c7a-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="b0c7a-139">`DataKeyField`: Имя столбца первичного ключа в источнике данных</span><span class="sxs-lookup"><span data-stu-id="b0c7a-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="b0c7a-140">`SortOrderField`: Исходный столбец данных, представляющий порядок сортировки для элементов списка</span><span class="sxs-lookup"><span data-stu-id="b0c7a-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="b0c7a-141">В `<DragHandleTemplate>` и `<ItemTemplate>` разделы, макет списка может быть настроена.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="b0c7a-142">Кроме того, возможна привязка данных с помощью `Eval()` метод, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="b0c7a-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="b0c7a-143">Сведения о стиле следующий код CSS (на который указывает ссылка в `<DragHandleTemplate>` раздел `ReorderList` управления) гарантирует, что указатель мыши изменяется соответствующим образом при его наведении маркер перетаскивания:</span><span class="sxs-lookup"><span data-stu-id="b0c7a-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="b0c7a-144">Наконец `ScriptManager` управления инициализирует ASP.NET AJAX для страницы:</span><span class="sxs-lookup"><span data-stu-id="b0c7a-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="b0c7a-145">Выполнить этот пример в браузере и немного изменить порядок элементов списка.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="b0c7a-146">Затем перезагрузите страницу и/или посмотрите на базе данных.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="b0c7a-147">Измененная позиций вестись и также отражаются по значениям в `position` столбца в базе данных, которые все без кода, вы можете просто с помощью разметки.</span><span class="sxs-lookup"><span data-stu-id="b0c7a-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="b0c7a-148">[![Изменения данных в базе данных в соответствии с новой порядок элементов списка](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="b0c7a-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="b0c7a-149">Элемент данных при изменениях базы данных в соответствии со списком нового заказа ([Просмотр полноразмерного изображения](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="b0c7a-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b0c7a-150">Назад</span><span class="sxs-lookup"><span data-stu-id="b0c7a-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
