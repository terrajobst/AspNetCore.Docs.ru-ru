---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: "Изменения свойств DropShadow из клиентского кода (VB) | Документы Microsoft"
author: wenz
description: "Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью. Также можно изменить свойства этого расширителя с помощью клиента JavaScrip..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 706ccb5a95873aad4c1b9e0942ab06cf4162710a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a>Изменения свойств DropShadow из клиентского кода (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью. Также можно изменить свойства этого расширителя с помощью кода JavaScript клиента.


## <a name="overview"></a>Обзор

Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью. Также можно изменить свойства этого расширителя с помощью кода JavaScript клиента.

## <a name="steps"></a>Шаги

Код начинается со панель, содержащая несколько строк текста.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

Связанный класс CSS предоставляет панели Цвет фона для работы с низким приоритетом:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

`DropShadowExtender` Добавляется расширение панель с тенью, равным 50% прозрачности:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

Затем ASP.NET AJAX `ScriptManager` управления включает набор средств управления для работы:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

Другую панель содержит две связи JavaScript задайте прозрачность тени: минус ссылку регулирует прозрачность тени, плюс ссылку повышает его.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

Функция JavaScript, которая `changeOpacity()` необходимо сначала найдите `DropShadowExtender` управления на странице. Определяет ASP.NET AJAX `$find()` метод для точно этой задачи. Затем `get_Opacity()` метод извлекает текущий непрозрачность `set_Opacity()` метод устанавливает его. Код JavaScript помещает текущее значение непрозрачности в `<label>` элемента:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


[![Непрозрачность меняется на стороне клиента](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

Непрозрачность меняется на стороне клиента ([Просмотр полноразмерное изображение](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[Назад](adjusting-the-z-index-of-a-dropshadow-vb.md)
