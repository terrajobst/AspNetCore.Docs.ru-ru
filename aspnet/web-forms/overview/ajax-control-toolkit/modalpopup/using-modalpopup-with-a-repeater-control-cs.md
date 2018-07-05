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
<a name="using-modalpopup-with-a-repeater-control-c"></a>Использование ModalPopup с элементом управления Repeater (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)

> Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Можно также использовать этот элемент управления в элементе управления repeater.


## <a name="overview"></a>Обзор

Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Можно также использовать этот элемент управления в элементе управления repeater.

## <a name="steps"></a>Шаги

Во-первых источником данных является обязательным. В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition. Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна для загрузки в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Базы данных AdventureWorks является частью образцы SQL Server 2005 и образцов баз данных (загрузить в [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Самый простой способ настроить базы данных является использование Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных. В этом примере мы предполагаем, что экземпляр SQL Server 2005 Express Edition называется `SQLEXPRESS` и находится на одном компьютере с веб-сервера; это также настройки по умолчанию. Если настройки отличаются, вам нужно адаптировать сведения о соединении для базы данных. Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

Затем добавьте источник данных на страницу. Чтобы использовать ограниченный объем данных, мы выбрать только первые пять записей в таблице поставщиков базы данных AdventureWorks. Если вы используете этот помощник используется Visual Studio для создания источника данных, виду, что ошибки в текущей версии не префикса имени таблицы (`Vendor`) с `Purchasing`. В следующей разметке показан правильный синтаксис:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

Добавьте панель, который служит в качестве модального всплывающего окна. Он содержит `Button` элемента управления, чтобы закрыть всплывающее окно еще раз:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

Чтобы работать в элементе repeater всплывающее окно `ModalPopupExtender` управления должны быть размещены в `<ItemTemplate>` разделе повторителя. Поэтому панели находится за пределами элемента управления repeater, но расширителя находится внутри. Далее приведена разметка для элемента управления repeater:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

Затем с рядом с ним кнопка, инициирующая модальное всплывающее окно отображается каждый элемент в источнике данных.


[![Модального всплывающего окна можно активировать для каждой записи источника данных](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)

Модального всплывающего окна можно активировать для каждой записи источника данных ([Просмотр полноразмерного изображения](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](launching-a-modal-popup-window-from-server-code-cs.md)
> [Вперед](handling-postbacks-from-a-modalpopup-cs.md)
