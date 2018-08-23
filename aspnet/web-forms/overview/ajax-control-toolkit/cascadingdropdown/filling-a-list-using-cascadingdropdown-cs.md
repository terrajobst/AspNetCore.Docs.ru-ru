---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Заполнение списка с помощью CascadingDropDown (C#) | Документация Майкрософт
author: wenz
description: Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 795b2a2ee80d3d2bed6f752887d5f9d974151a56
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829062"
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>Заполнение списка с помощью CascadingDropDown (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в другой DropDownList. (К примеру, один список содержит список штатов США, и следующий список заполняется крупнейших городов в этом состоянии.) Первая трудность заключается для решения — фактически заполнение раскрывающегося списка, с помощью этого элемента управления.


## <a name="overview"></a>Обзор

Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в другой DropDownList. (К примеру, один список содержит список штатов США, и следующий список заполняется крупнейших городов в этом состоянии.) Первая трудность заключается для решения — фактически заполнение раскрывающегося списка, с помощью этого элемента управления.

## <a name="steps"></a>Шаги

Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Затем необходим элемент управления DropDownList:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Для этого списка расширитель CascadingDropDown добавляется. Он отправит асинхронный запрос к веб-службу, которая будет возвращать список записей для отображения в списке. Чтобы это работало должны быть заданы следующие атрибуты CascadingDropDown:

- `ServicePath`: URL-адрес веб-службы, предоставляя элементов списка
- `ServiceMethod`: Метод веб-доставка элементов списка
- `TargetControlID`: Идентификатор из раскрывающегося списка
- `Category`: Категории сведений, переданных при вызове веб-метода
- `PromptText`: Текст, отображаемый при асинхронной загрузкой данных с сервера

Далее приведена разметка для `CascadingDropDown` элемент. Единственное различие между C# и VB — имя связанного веб-службы:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Код JavaScript, поступающие от `CascadingDropDown` расширения вызывает метод веб-службы со следующей сигнатурой:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Это важный аспект, метод должен возвращать массив объектов типа `CascadingDropDownNameValue` (определяется по ASP.NET AJAX Control Toolkit). В `CascadingDropDownNameValue` конструктор, первый текстовый элемент списка, а затем его значение должно быть указано, так же, как `<option value="VALUE">NAME</option>` в HTML. Вот некоторые примеры данных:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Загрузку страницы в браузере будет активировать списка, чтобы заполнить трех поставщиков.


[![Список заполняется автоматически](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

Список заполняется автоматически ([Просмотр полноразмерного изображения](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](using-cascadingdropdown-with-a-database-cs.md)
