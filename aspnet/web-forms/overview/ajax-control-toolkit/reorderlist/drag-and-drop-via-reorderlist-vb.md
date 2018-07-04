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
<a name="drag-and-drop-via-reorderlist-vb"></a>Перетаскивание с помощью элемента управления ReorderList (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> Элемент управления ReorderList в AJAX Control Toolkit предоставляет список, можно ли изменять расположение пользователем с помощью операции перетаскивания. Текущий порядок элементов в списке должны быть сохранены на сервере.


## <a name="overview"></a>Обзор

`ReorderList` Элемент управления в AJAX Control Toolkit предоставляет список, можно ли изменять расположение пользователем с помощью операции перетаскивания. Текущий порядок элементов в списке должны быть сохранены на сервере.

## <a name="steps"></a>Шаги

`ReorderList` Элемент управления поддерживает привязку данных из базы данных в список. Самое главное она также поддерживает запись изменений порядку элемент списка в хранилище данных.

Этот образец использует Microsoft SQL Server 2005 Express Edition в качестве хранилища данных. Базы данных является необязательным (и бесплатной) часть установки Visual Studio, включая экспресс-выпуск. Эта схема также доступна для загрузки в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). В этом примере мы предполагаем, что экземпляр SQL Server 2005 Express Edition называется `SQLEXPRESS` и находится на одном компьютере с веб-сервера; это также настройки по умолчанию. Если настройки отличаются, вам нужно адаптировать сведения о соединении для базы данных.

Самый простой способ установить базу данных, — использовать Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Подключитесь к серверу, дважды щелкните `Databases` и создать новую базу данных (щелкните правой кнопкой мыши и выберите `New Database`) вызывается `Tutorials`.

В этой базе данных, создать новую таблицу с именем `AJAX` с следующие четыре столбца:

- `id` (первичный ключ, integer, удостоверение, not NULL)
- `char` (char(1), NULL)
- `description` (varchar(50), NULL)
- `position` (int, NULL)


[![Макет таблицы AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

Макет таблицы AJAX ([Просмотр полноразмерного изображения](drag-and-drop-via-reorderlist-vb/_static/image3.png))


Затем введите таблицы с несколькими значениями. Обратите внимание, что `position` столбец содержит порядок сортировки элементов.


[![Начальные данные в таблице AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

Начальные данные в таблице AJAX ([Просмотр полноразмерного изображения](drag-and-drop-via-reorderlist-vb/_static/image6.png))


Для следующего шага требуется для создания `SqlDataSource` управления для взаимодействия с новой базы данных и соответствующей таблицей. Источник данных должен поддерживать `SELECT` и `UPDATE` команды SQL. При последующем изменении порядок элементов списка `ReorderList` элемент управления автоматически отправляет два значения в источник данных `Update` команда: новое положение и идентификатор элемента. Таким образом, источника данных требованиям `<UpdateParameters>` раздел для этих двух значений:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

`ReorderList` Управления необходимо задать следующие атрибуты:

- `AllowReorder`: Ли можно упорядочить элементы списка
- `DataSourceID`: Идентификатор источника данных
- `DataKeyField`: Имя столбца первичного ключа в источнике данных
- `SortOrderField`: Исходный столбец данных, представляющий порядок сортировки для элементов списка

В `<DragHandleTemplate>` и `<ItemTemplate>` разделы, макет списка может быть настроена. Кроме того, возможна привязка данных с помощью `Eval()` метод, как показано ниже:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

Сведения о стиле следующий код CSS (на который указывает ссылка в `<DragHandleTemplate>` раздел `ReorderList` управления) гарантирует, что указатель мыши изменяется соответствующим образом при его наведении маркер перетаскивания:

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

Наконец `ScriptManager` управления инициализирует ASP.NET AJAX для страницы:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

Выполнить этот пример в браузере и немного изменить порядок элементов списка. Затем перезагрузите страницу и/или посмотрите на базе данных. Измененная позиций вестись и также отражаются по значениям в `position` столбца в базе данных, которые все без кода, вы можете просто с помощью разметки.


[![Изменения данных в базе данных в соответствии с новой порядок элементов списка](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

Элемент данных при изменениях базы данных в соответствии со списком нового заказа ([Просмотр полноразмерного изображения](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [Назад](using-postbacks-with-reorderlist-vb.md)
