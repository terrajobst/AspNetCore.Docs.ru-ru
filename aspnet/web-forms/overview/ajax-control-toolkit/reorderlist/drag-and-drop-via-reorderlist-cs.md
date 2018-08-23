---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: Перетаскивание с помощью элемента управления ReorderList (C#) | Документация Майкрософт
author: wenz
description: Элемент управления ReorderList в AJAX Control Toolkit предоставляет список, можно ли изменять расположение пользователем с помощью операции перетаскивания. Текущий порядок элементов в списке должны...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 15ae6ae60381f3f656f667a97dac72dbb283c80e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835729"
---
<a name="drag-and-drop-via-reorderlist-c"></a>Перетаскивание с помощью элемента управления ReorderList (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)

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


[![Макет таблицы AJAX](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)

Макет таблицы AJAX ([Просмотр полноразмерного изображения](drag-and-drop-via-reorderlist-cs/_static/image3.png))


Затем введите таблицы с несколькими значениями. Обратите внимание, что `position` столбец содержит порядок сортировки элементов.


[![Начальные данные в таблице AJAX](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)

Начальные данные в таблице AJAX ([Просмотр полноразмерного изображения](drag-and-drop-via-reorderlist-cs/_static/image6.png))


Для следующего шага требуется для создания `SqlDataSource` управления для взаимодействия с новой базы данных и соответствующей таблицей. Источник данных должен поддерживать `SELECT` и `UPDATE` команды SQL. При последующем изменении порядок элементов списка `ReorderList` элемент управления автоматически отправляет два значения в источник данных `Update` команда: новое положение и идентификатор элемента. Таким образом, источника данных требованиям `<UpdateParameters>` раздел для этих двух значений:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

`ReorderList` Управления необходимо задать следующие атрибуты:

- `AllowReorder`: Ли можно упорядочить элементы списка
- `DataSourceID`: Идентификатор источника данных
- `DataKeyField`: Имя столбца первичного ключа в источнике данных
- `SortOrderField`: Исходный столбец данных, представляющий порядок сортировки для элементов списка

В `<DragHandleTemplate>` и `<ItemTemplate>` разделы, макет списка может быть настроена. Кроме того, возможна привязка данных с помощью `Eval()` метод, как показано ниже:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

Сведения о стиле следующий код CSS (на который указывает ссылка в `<DragHandleTemplate>` раздел `ReorderList` управления) гарантирует, что указатель мыши изменяется соответствующим образом при его наведении маркер перетаскивания:

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

Наконец `ScriptManager` управления инициализирует ASP.NET AJAX для страницы:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

Выполнить этот пример в браузере и немного изменить порядок элементов списка. Затем перезагрузите страницу и/или посмотрите на базе данных. Измененная позиций вестись и также отражаются по значениям в `position` столбца в базе данных, которые все без кода, вы можете просто с помощью разметки.


[![Изменения данных в базе данных в соответствии с новой порядок элементов списка](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)

Элемент данных при изменениях базы данных в соответствии со списком нового заказа ([Просмотр полноразмерного изображения](drag-and-drop-via-reorderlist-cs/_static/image9.png))

> [!div class="step-by-step"]
> [Назад](using-postbacks-with-reorderlist-cs.md)
> [Вперед](using-postbacks-with-reorderlist-vb.md)
