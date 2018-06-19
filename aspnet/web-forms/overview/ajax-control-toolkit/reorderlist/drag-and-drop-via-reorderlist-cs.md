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
ms.locfileid: "30873460"
---
<a name="drag-and-drop-via-reorderlist-c"></a>Перетаскивание через ReorderList (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)

> Элемент управления ReorderList в наборе элементов управления AJAX предоставляет список, который можно переупорядочить пользователем с помощью операции перетаскивания. Текущий порядок элементов в списке должны быть сохранены на сервере.


## <a name="overview"></a>Обзор

`ReorderList` Список элемента управления в наборе элементов управления AJAX можно переупорядочить пользователем с помощью операции перетаскивания. Текущий порядок элементов в списке должны быть сохранены на сервере.

## <a name="steps"></a>Шаги

`ReorderList` Элемент управления поддерживает привязку данных из базы данных в список. Лучше всего оно также поддерживает запись изменений порядку элемент списка в хранилище данных.

Этот образец использует в качестве хранилища данных Microsoft SQL Server 2005 Express Edition. Базы данных является частью необязательно (и бесплатные) установки Visual Studio, включая экспресс-выпуск. Эта схема также доступна как отдельный загружаемый под [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Для данного образца предполагается, что экземпляр SQL Server 2005 Express Edition вызывается `SQLEXPRESS` и находится на том же компьютере, что веб-сервер; Кроме того, это настройки по умолчанию. Если настройки отличаются, необходимо адаптировать сведения о соединении для базы данных.

Самый простой способ настроить базу данных будет использовать Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Подключиться к серверу, дважды щелкните `Databases` и создать новую базу данных (щелкните правой кнопкой мыши и выберите `New Database`) вызывается `Tutorials`.

В этой базе данных, создайте новую таблицу с именем `AJAX` следующие четыре столбца:

- `id` (первичный ключ, целое число со знаком, удостоверение, не ОПРЕДЕЛЕНО)
- `char` (char(1) NULL)
- `description` (varchar(50) NULL)
- `position` (int, NULL)


[![Макет таблицы AJAX](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)

Макет таблицы AJAX ([Просмотр полноразмерное изображение](drag-and-drop-via-reorderlist-cs/_static/image3.png))


После этого заполните таблицу с помощью нескольких значений. Обратите внимание, что `position` столбец содержит порядок сортировки элементов.


[![Начальные данные в таблице AJAX](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)

Начальные данные в таблице AJAX ([Просмотр полноразмерное изображение](drag-and-drop-via-reorderlist-cs/_static/image6.png))


Следующий шаг необходим для создания `SqlDataSource` управления для связи с новой базы данных и соответствующей таблицей. Источник данных должен поддерживать `SELECT` и `UPDATE` команды SQL. При последующем изменении порядок элементов списка `ReorderList` управления автоматически отправляет двух значений в источник данных `Update` команды: новое положение и идентификатор элемента. Таким образом, источника данных требованиям `<UpdateParameters>` раздел для этих двух значений:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

`ReorderList` Элемент управления должен задать следующие атрибуты:

- `AllowReorder`: Ли можно упорядочить элементы списка
- `DataSourceID`: Идентификатор источника данных
- `DataKeyField`: Имя столбца первичного ключа в источнике данных
- `SortOrderField`: Столбца источника данных, представляющий порядок сортировки для элементов списка

В `<DragHandleTemplate>` и `<ItemTemplate>` разделах макета списка может быть настроена. Кроме того, возможна привязка данных с помощью `Eval()` метода, как показано ниже:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

Сведения о стиле следующий код CSS (на который ссылается `<DragHandleTemplate>` раздел `ReorderList` управления) гарантирует, что указатель мыши изменяется соответствующим образом при наведении курсора на маркер перетаскивания:

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

Наконец `ScriptManager` элемент управления инициализирует ASP.NET AJAX для страницы:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

Этот пример можно выполнить в браузере и немного изменить порядок элементов списка. Затем перезагрузите страницу и/или посмотрите на базе данных. Измененная позиций вестись и также отражаются по значениям в `position` столбца в базе данных, которые все без кода, только с помощью разметки.


[![Данные в базы данных изменяется в соответствии с новой порядок элементов списка](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)

Данные в базы данных изменяется в соответствии с новый список элементов заказа ([Просмотр полноразмерное изображение](drag-and-drop-via-reorderlist-cs/_static/image9.png))

> [!div class="step-by-step"]
> [Назад](using-postbacks-with-reorderlist-cs.md)
> [Вперед](using-postbacks-with-reorderlist-vb.md)
