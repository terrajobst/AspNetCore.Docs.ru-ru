---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: Использование ConfirmButton в элементе управления Repeater (C#) | Документация Майкрософт
author: wenz
description: Расширитель ConfirmButton в AJAX Control Toolkit создает Да/Нет всплывающего меню, когда пользователь нажимает кнопку (включая управления LinkButton). Да — только если...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: fd0cdde5db44d1437000008f1fb5e7b13f0ca77a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805240"
---
<a name="using-a-confirmbutton-in-a-repeater-c"></a>Использование ConfirmButton в элементе управления Repeater (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)

> Расширитель ConfirmButton в AJAX Control Toolkit создает Да/Нет всплывающего меню, когда пользователь нажимает кнопку (включая управления LinkButton). Только если выбран параметр Да, кнопки выполняется действие, в противном случае отменена. Это также возможно в элементе управления repeater.


## <a name="overview"></a>Обзор

Расширитель ConfirmButton в AJAX Control Toolkit создает Да/Нет всплывающего меню, когда пользователь нажимает кнопку (включая управления LinkButton). Только если выбран параметр Да, кнопки выполняется действие, в противном случае отменена. Это также возможно в элементе управления repeater.

## <a name="steps"></a>Шаги

Во-первых источником данных является обязательным. В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition. Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна для загрузки в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Базы данных AdventureWorks является частью образцы SQL Server 2005 и образцов баз данных (загрузить в [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Самый простой способ настроить базы данных является использование Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.

В этом примере мы предполагаем, что экземпляр SQL Server 2005 Express Edition называется `SQLEXPRESS` и находится на одном компьютере с веб-сервера; это также настройки по умолчанию. Если настройки отличаются, вам нужно адаптировать сведения о соединении для базы данных.

Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

Выберите источник данных является обязательным. Для простоты извлекаются только первые пять записей в таблице поставщиков AdventureWorks. Обратите внимание, что при использовании мастера Visual Studio для создания источника данных, имя таблицы (`Vendors`) в настоящее время нет префикса правильно `Purchasing`. Следующая разметка является правильная:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

Затем этот источник данных можно использовать в элементе управления repeater. Как обычно `DataBinder.Eval()` метод получает данные из источника данных. `ConfirmButtonExtender` Управления затем должен быть помещен в `<ItemTemplate>` части элемента управления repeater, чтобы он располагался для каждой записи в источнике данных.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]


[![Появляется кнопка «подтверждение» рядом с каждой записи из источника данных](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)

Рядом с каждой записи из источника данных отображается «подтверждение» ([Просмотр полноразмерного изображения](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](using-a-confirmbutton-in-a-repeater-vb.md)
