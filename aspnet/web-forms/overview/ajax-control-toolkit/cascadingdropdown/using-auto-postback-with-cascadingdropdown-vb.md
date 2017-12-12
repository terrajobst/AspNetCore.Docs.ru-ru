---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: "Использование автоматического выполнения обратной с CascadingDropDown (VB) | Документы Microsoft"
author: wenz
description: "CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в anoth..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: f8059f44b4efbf59ebe7b3d2fd5400e886642a90
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a>Использование автоматического выполнения обратной с CascadingDropDown (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)

> CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList. Однако при использовании элемента управления CascadingDropDown, ASP. Компонент управления DropDownList в NET AutoPostBack не работает, поскольку асинхронно загрузку данных в списке создает (неиспользуемые) обратной передачи, сам. Этот эффект можно избежать с помощью кода JavaScript.


## <a name="overview"></a>Обзор

CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList. (Для экземпляра один список содержит список штатов США, и далее список заполняется основных городов, в этом состоянии.) Однако при использовании элемента управления CascadingDropDown, ASP. Компонент управления DropDownList в NET AutoPostBack не работает, поскольку асинхронно загрузку данных в списке создает (неиспользуемые) обратной передачи, сам. Этот эффект можно избежать с помощью кода JavaScript.

## <a name="steps"></a>Шаги

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в &lt; `form` &gt; элемент):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

Затем элемент управления DropDownList требуется:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

Для этого списка добавляется расширителя CascadingDropDown, предоставляя URL-адрес и метод информации веб-службы:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

Расширитель CascadingDropDown асинхронно вызывает веб-службы со следующей сигнатурой метода:

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

Метод возвращает массив объектов типа CascadingDropDown значение. Тип конструктора сначала требуется заголовок элемента списка, а затем значение (HTML `value` атрибут).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

Загрузка страницы в браузере будут заполнены раскрывающийся список с тремя поставщиков предварительно выбран второй. Кроме того, определяет ASP.NET `__doPostBack()` метода JavaScript. После загрузки страницы, этот вызов JavaScript добавляется к раскрывающемуся списку, но только при наличии элементов в нем. Если в списке нет элементов, набор элементов управления загружается, поэтому код JavaScript использует время ожидания и предпринимает попытку через полсекунды.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

Таким образом, обратная передача выполняется только при наличии фактически элементов в списке, а пользователь выбирает запись.


[![При выборе элемента списка вызывает обратную передачу](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)

При выборе элемента списка вызывает обратную передачу ([Просмотр полноразмерное изображение](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))

>[!div class="step-by-step"]
[Назад](presetting-list-entries-with-cascadingdropdown-vb.md)
