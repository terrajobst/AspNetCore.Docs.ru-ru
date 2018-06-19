---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Динамическое добавление Гармошка области (VB) | Документы Microsoft
author: wenz
description: Гармошка управления в наборе элементов управления AJAX предоставляет несколько областей и пользователь может содержать один из них одновременно. Панелей обычно объявляются w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 68c60ba6d4be5eb6709f7558d6be4165f8232a4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868728"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Динамическое добавление Гармошка области (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> Гармошка управления в наборе элементов управления AJAX предоставляет несколько областей и пользователь может содержать один из них одновременно. Обычно панелей объявлены внутри самой страницы, но код на стороне сервера может использоваться для достижения такого же результата.


## <a name="overview"></a>Обзор

Гармошка управления в наборе элементов управления AJAX предоставляет несколько областей и пользователь может содержать один из них одновременно. Обычно панелей объявлены внутри самой страницы, но код на стороне сервера может использоваться для достижения такого же результата.

## <a name="steps"></a>Шаги

Элемент управления Гармошка предоставляет все важные свойства серверного кода. Помимо прочего `Panes` свойство предоставляет доступ к коллекции областей, которые составляют Accordion. Каждой области нет типа `AccordionPane`. Таким образом, это просто создание такой области:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

`HeaderContainer` Свойство `AccordionPane` предоставляет доступ к элементам управления ASP.NET в раздел заголовка области; `ContentContainer` свойство `AccordionPane` делает то же самое для раздела содержимого панели. Это позволяет коду ASP.NET для добавления содержимого в области:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Наконец, необходимо добавить pane(s) `Panes` коллекцию Гармошка:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Ниже приведен полный серверный код, добавляющий к элементу управления Гармошка две панели:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

Отсутствует единственный элемент является Accordion, который зависит от наличия ASP.NET `ScriptManager` управления:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Чтобы завершить процесс в примере, двух классов CSS, на который ссылается элемент управления Гармошка предоставляют сведения о стиле для браузера:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![Данные в Гармошка динамически добавленные серверный код](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Данные в Гармошка динамически добавленные серверный код ([Просмотр полноразмерное изображение](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](databinding-to-an-accordion-vb.md)
