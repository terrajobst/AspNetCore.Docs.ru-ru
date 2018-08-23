---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Привязка данных к Accordion (C#) | Документация Майкрософт
author: wenz
description: Элемент управления Accordion в AJAX Control Toolkit предоставляет несколько областей и пользователь может одновременно отобразить один из них. Панели обычно объявляются w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 930392bd33cfeb7dec52a6084a5d401a6134a7ef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829021"
---
<a name="databinding-to-an-accordion-c"></a>Привязка данных к Accordion (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> Элемент управления Accordion в AJAX Control Toolkit предоставляет несколько областей и пользователь может одновременно отобразить один из них. Обычно панелей объявлены внутри самой страницы, но привязка к источнику данных обеспечивает большую гибкость.


## <a name="overview"></a>Обзор

Элемент управления Accordion в AJAX Control Toolkit предоставляет несколько областей и пользователь может одновременно отобразить один из них. Обычно панелей объявлены внутри самой страницы, но привязка к источнику данных обеспечивает большую гибкость.

## <a name="steps"></a>Шаги

Во-первых источником данных является обязательным. В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition. Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна для загрузки в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Базы данных AdventureWorks является частью образцы SQL Server 2005 и образцов баз данных (загрузить в [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Самый простой способ настроить базы данных является использование Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.

В этом примере мы предполагаем, что экземпляр SQL Server 2005 Express Edition называется `SQLEXPRESS` и находится на одном компьютере с веб-сервера; это также настройки по умолчанию. Если настройки отличаются, вам нужно адаптировать сведения о соединении для базы данных.

Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Затем добавьте источник данных на страницу. Чтобы использовать ограниченный объем данных, мы выбрать только первые пять записей в таблице поставщиков базы данных AdventureWorks. Если вы используете этот помощник используется Visual Studio для создания источника данных, виду, что ошибки в текущей версии не префикса имени таблицы (`Vendor`) с `Purchasing`. В следующей разметке показан правильный синтаксис:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Запомните имя источника данных (идентификатор). Затем следует использовать этот код очень `DataSourceID` свойство элемента управления Accordion:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

В элементе управления Accordion, можно создать шаблоны для различных частей элемента управления, включая заголовок (`<HeaderTemplate>`) и содержимое (`<ContentTemplate>`). В этих элементах, просто выводить данные на основе данных источник данных, с помощью `DataBinder.Eval()` метод:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

При загрузке страницы, источник данных должен быть привязан к accordion этот код на стороне сервера:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Чтобы завершить этот пример, необходимо определить класс CSS, к которым обращается в элемент управления Accordion (в свойствах `HeaderCssClass` и `ContentCssClass`). Поместите следующую разметку в `<head>` раздел страницы:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![Данные в Гармошка поступают непосредственно из источника данных](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Данные в Гармошка поступают непосредственно из источника данных ([Просмотр полноразмерного изображения](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](dynamically-adding-an-accordion-pane-cs.md)
