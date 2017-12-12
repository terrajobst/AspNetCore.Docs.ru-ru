---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: "Привязка данных к Accordion (VB) | Документы Microsoft"
author: wenz
description: "Гармошка управления в наборе элементов управления AJAX предоставляет несколько областей и пользователь может содержать один из них одновременно. Панелей обычно объявляются w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: aeff732e4daed6ed22fd5f3b6adcdeb6082aae53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="databinding-to-an-accordion-vb"></a>Привязка данных к Accordion (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)

> Гармошка управления в наборе элементов управления AJAX предоставляет несколько областей и пользователь может содержать один из них одновременно. Обычно панелей объявлены внутри самой страницы, но привязка к источнику данных обеспечивает большую гибкость.


## <a name="overview"></a>Обзор

Гармошка управления в наборе элементов управления AJAX предоставляет несколько областей и пользователь может содержать один из них одновременно. Обычно панелей объявлены внутри самой страницы, но привязка к источнику данных обеспечивает большую гибкость.

## <a name="steps"></a>Шаги

Во-первых источник данных является обязательным. В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition. Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна как отдельный загружаемый в разделе [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Базы данных AdventureWorks входит в состав образцов баз данных и образцы SQL Server 2005 (загрузить в [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Самый простой способ настроить базы данных будет использовать Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.

Для данного образца предполагается, что экземпляр SQL Server 2005 Express Edition вызывается `SQLEXPRESS` и находится на том же компьютере, что веб-сервер; Кроме того, это настройки по умолчанию. Если настройки отличаются, необходимо адаптировать сведения о соединении для базы данных.

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

Затем добавьте источник данных на страницу. Чтобы использовать ограниченный объем данных, мы выбрать только первых пяти записей в таблице поставщиков базы данных AdventureWorks. При использовании помощника по Visual Studio для создания источника данных, помните, что ошибки в текущей версии не предшествует имени таблицы (`Vendor`) с `Purchasing`. В следующем примере показана правильный синтаксис:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

Запомните имя источника данных (ID). Эта Идентификация очень затем должно использоваться в `DataSourceID` свойства элемента управления Гармошка:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

В элементе управления Гармошка можно создать шаблоны для различных частей элемента управления, включая заголовок (`<HeaderTemplate>`) и содержимым (`<ContentTemplate>`). В этих элементах просто вывести данные из данных источника, используя `DataBinder.Eval()` метод:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

При загрузке страницы, источник данных должен быть привязан к Гармошка следующим кодом на стороне сервера:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

Чтобы завершить этот пример, необходимо определить два класса CSS, на которые имеются ссылки в элементе управления Гармошка (свойствами `HeaderCssClass` и `ContentCssClass`). Поместите следующую разметку в `<head>` раздел страницы:

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


[![Данные для Гармошка извлекаются непосредственно из источника данных](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)

Данные для Гармошка извлекаются непосредственно из источника данных ([Просмотр полноразмерное изображение](databinding-to-an-accordion-vb/_static/image3.png))

>[!div class="step-by-step"]
[Назад](dynamically-adding-an-accordion-pane-cs.md)
[Вперед](dynamically-adding-an-accordion-pane-vb.md)
