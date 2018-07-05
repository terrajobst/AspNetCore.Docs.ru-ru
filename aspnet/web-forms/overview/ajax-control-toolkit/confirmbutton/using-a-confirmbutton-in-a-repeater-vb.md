---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Использование ConfirmButton в элементе управления Repeater (VB) | Документация Майкрософт
author: wenz
description: Расширитель ConfirmButton в AJAX Control Toolkit создает Да/Нет всплывающего меню, когда пользователь нажимает кнопку (включая управления LinkButton). Да — только если...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: ce403e84766f586eca36ef6bc513d9fbf7bd1d40
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396423"
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a>Использование ConfirmButton в элементе управления Repeater (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> Расширитель ConfirmButton в AJAX Control Toolkit создает Да/Нет всплывающего меню, когда пользователь нажимает кнопку (включая управления LinkButton). Только если выбран параметр Да, кнопки выполняется действие, в противном случае отменена. Это также возможно в элементе управления repeater.


## <a name="overview"></a>Обзор

Расширитель ConfirmButton в AJAX Control Toolkit создает Да/Нет всплывающего меню, когда пользователь нажимает кнопку (включая управления LinkButton). Только если выбран параметр Да, кнопки выполняется действие, в противном случае отменена. Это также возможно в элементе управления repeater.

## <a name="steps"></a>Шаги

Во-первых источником данных является обязательным. В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition. Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна для загрузки в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Базы данных AdventureWorks является частью образцы SQL Server 2005 и образцов баз данных (загрузить в [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Самый простой способ настроить базы данных является использование Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.

В этом примере мы предполагаем, что экземпляр SQL Server 2005 Express Edition называется `SQLEXPRESS` и находится на одном компьютере с веб-сервера; это также настройки по умолчанию. Если настройки отличаются, вам нужно адаптировать сведения о соединении для базы данных.

Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

Выберите источник данных является обязательным. Для простоты извлекаются только первые пять записей в таблице поставщиков AdventureWorks. Обратите внимание, что при использовании мастера Visual Studio для создания источника данных, имя таблицы (`Vendors`) в настоящее время нет префикса правильно `Purchasing`. Следующая разметка является правильная:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

Затем этот источник данных можно использовать в элементе управления repeater. Как обычно `DataBinder.Eval()` метод получает данные из источника данных. `ConfirmButtonExtender` Управления затем должен быть помещен в `<ItemTemplate>` части элемента управления repeater, чтобы он располагался для каждой записи в источнике данных.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


[![Появляется кнопка «подтверждение» рядом с каждой записи из источника данных](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

Рядом с каждой записи из источника данных отображается «подтверждение» ([Просмотр полноразмерного изображения](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](using-a-confirmbutton-in-a-repeater-cs.md)
