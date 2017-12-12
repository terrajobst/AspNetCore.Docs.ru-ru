---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: "С помощью ConfirmButton в повторителя (C#) | Документы Microsoft"
author: wenz
description: "Расширения ConfirmButton в наборе элементов управления AJAX создает Да/Нет всплывающего меню, когда пользователь щелкает на кнопке (включая управления LinkButton). Да — только тогда, когда..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 12fd3e4e58e6a73720ade38372caff496f7f2c97
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-a-confirmbutton-in-a-repeater-c"></a>С помощью ConfirmButton в повторителя (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)

> Расширения ConfirmButton в наборе элементов управления AJAX создает Да/Нет всплывающего меню, когда пользователь щелкает на кнопке (включая управления LinkButton). Только если выбран параметр Да, кнопки действие выполняется, в противном случае отменена. Это также можно сделать в повторитель.


## <a name="overview"></a>Обзор

Расширения ConfirmButton в наборе элементов управления AJAX создает Да/Нет всплывающего меню, когда пользователь щелкает на кнопке (включая управления LinkButton). Только если выбран параметр Да, кнопки действие выполняется, в противном случае отменена. Это также можно сделать в повторитель.

## <a name="steps"></a>Шаги

Во-первых источник данных является обязательным. В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition. Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна как отдельный загружаемый в разделе [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Базы данных AdventureWorks входит в состав образцов баз данных и образцы SQL Server 2005 (загрузить в [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Самый простой способ настроить базы данных будет использовать Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.

Для данного образца предполагается, что экземпляр SQL Server 2005 Express Edition вызывается `SQLEXPRESS` и находится на том же компьютере, что веб-сервер; Кроме того, это настройки по умолчанию. Если настройки отличаются, необходимо адаптировать сведения о соединении для базы данных.

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

После этого источника данных является обязательным. Для простоты извлекаются только первых пяти записей в таблице поставщиков AdventureWorks. Обратите внимание, что при использовании мастера Visual Studio для создания источника данных, имя таблицы (`Vendors`) в настоящее время нет префикса правильно `Purchasing`. Является правильным следующую разметку:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

Затем этот источник данных можно использовать в пределах повторитель. Как обычно `DataBinder.Eval()` метод получает данные из источника данных. `ConfirmButtonExtender` Управления затем должен находиться в пределах `<ItemTemplate>` раздел повторителя, чтобы он располагался для каждой из записей в источнике данных.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]


[![«Подтверждение» рядом с каждой записи из источника данных](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)

«Подтверждение» рядом с каждой записи из источника данных ([Просмотр полноразмерное изображение](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))

>[!div class="step-by-step"]
[Вперед](using-a-confirmbutton-in-a-repeater-vb.md)
