---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Заполнение списка, используя CascadingDropDown (C#) | Документы Microsoft
author: wenz
description: CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: c9e47f6484e49013004bf15084f98440ee67558e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870977"
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>Заполнение списка, используя CascadingDropDown (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList. (Для экземпляра один список содержит список штатов США, и далее список заполняется основных городов, в этом состоянии.) Для решения первой проверки — фактически заполнить раскрывающийся список с помощью этого элемента управления.


## <a name="overview"></a>Обзор

CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList. (Для экземпляра один список содержит список штатов США, и далее список заполняется основных городов, в этом состоянии.) Для решения первой проверки — фактически заполнить раскрывающийся список с помощью этого элемента управления.

## <a name="steps"></a>Шаги

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Затем элемент управления DropDownList требуется:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Расширитель CascadingDropDown добавляется для этого списка. Он передает выполнение асинхронного запроса для веб-службы, которая будет возвращать список записей, отображаемых в списке. Чтобы это работало необходимо задать следующие атрибуты CascadingDropDown:

- `ServicePath`: URL-адрес веб-службы доставки элементов списка
- `ServiceMethod`: Веб-метода доставки элементов списка
- `TargetControlID`: Идентификатор из раскрывающегося списка
- `Category`: Категории сведений, переданных при вызове веб-метода
- `PromptText`: Текст, отображаемый при асинхронной загрузке данных списка с сервера

Разметка для `CascadingDropDown` элемента. Единственное различие между C# и VB — имя связанного веб-службы:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Код JavaScript, поступающих от `CascadingDropDown` расширителя вызывает метод веб-службы со следующей сигнатурой:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Поэтому является важным аспектом методу требуется вернуть массив объектов типа `CascadingDropDownNameValue` (определяемые элементов управления ASP.NET AJAX). В `CascadingDropDownNameValue` конструктор первый текстовый элемент списка, а затем его значение необходимо указать, как `<option value="VALUE">NAME</option>` в HTML. Вот некоторые образцы данных:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Загрузка страницы в браузере, запускающих списка должен быть заполнен трех поставщиков.


[![Список заполняется автоматически](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

Список заполняется автоматически ([Просмотр полноразмерное изображение](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](using-cascadingdropdown-with-a-database-cs.md)
