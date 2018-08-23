---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Динамическое добавление области Accordion (C#) | Документация Майкрософт
author: wenz
description: Элемент управления Accordion в AJAX Control Toolkit предоставляет несколько областей и пользователь может одновременно отобразить один из них. Панели обычно объявляются w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 16cefadb7086a658b20558526757f9229a43fcc9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836303"
---
<a name="dynamically-adding-an-accordion-pane-c"></a>Динамическое добавление области Accordion (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> Элемент управления Accordion в AJAX Control Toolkit предоставляет несколько областей и пользователь может одновременно отобразить один из них. Обычно панелей объявлены внутри самой страницы, но серверный код может использоваться для достижения того же результата.


## <a name="overview"></a>Обзор

Элемент управления Accordion в AJAX Control Toolkit предоставляет несколько областей и пользователь может одновременно отобразить один из них. Обычно панелей объявлены внутри самой страницы, но серверный код может использоваться для достижения того же результата.

## <a name="steps"></a>Шаги

Элемент управления Accordion предоставляет все важные свойства серверного кода. Помимо прочего `Panes` свойство предоставляет доступ к коллекции областей, которые составляют Accordion. Каждой области есть типа `AccordionPane`. Таким образом, это просто создание такой области:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

`HeaderContainer` Свойство `AccordionPane` предоставляет доступ к элементам управления ASP.NET, в разделе заголовка области; `ContentContainer` свойство `AccordionPane` делает то же самое для содержимого части области. Благодаря этому код ASP.NET для добавления содержимого в области:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Наконец, необходимо добавить pane(s) `Panes` коллекцию Гармошка:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

Ниже приведен полный код на стороне сервера, который добавляет две панели в элемент управления Accordion.

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

Единственный отсутствующий элемент является Accordion, который зависит от наличия ASP.NET `ScriptManager` управления:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Чтобы завершить работу в примере, эти два класса CSS, на который ссылается элемент управления Accordion предоставляют сведения о стиле для браузера:

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![Данные в Гармошка динамически добавленные серверный код](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

Данные в Гармошка динамически добавленные серверного кода ([Просмотр полноразмерного изображения](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](databinding-to-an-accordion-cs.md)
> [Вперед](databinding-to-an-accordion-vb.md)
