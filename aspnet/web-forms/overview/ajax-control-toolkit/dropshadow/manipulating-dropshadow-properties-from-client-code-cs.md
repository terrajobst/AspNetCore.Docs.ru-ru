---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Изменения свойств DropShadow из клиентского кода (C#) | Документы Microsoft
author: wenz
description: Настройка интерфейса редактирования DataList
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 37a7784e1d42477e31938e1d15495993ac86fc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870340"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a>Изменения свойств DropShadow из клиентского кода (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью. Также можно изменить свойства этого расширителя с помощью кода JavaScript клиента.


## <a name="overview"></a>Обзор

Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью. Также можно изменить свойства этого расширителя с помощью кода JavaScript клиента.

## <a name="steps"></a>Шаги

Код начинается со панель, содержащая несколько строк текста.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

Связанный класс CSS предоставляет панели Цвет фона для работы с низким приоритетом:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

`DropShadowExtender` Добавляется расширение панель с тенью, равным 50% прозрачности:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Затем ASP.NET AJAX `ScriptManager` управления включает набор средств управления для работы:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Другую панель содержит две связи JavaScript задайте прозрачность тени: минус ссылку регулирует прозрачность тени, плюс ссылку повышает его.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

Функция JavaScript, которая `changeOpacity()` необходимо сначала найдите `DropShadowExtender` управления на странице. Определяет ASP.NET AJAX `$find()` метод для точно этой задачи. Затем `get_Opacity()` метод извлекает текущий непрозрачность `set_Opacity()` метод устанавливает его. Код JavaScript помещает текущее значение непрозрачности в `<label>` элемента:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![Непрозрачность меняется на стороне клиента](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

Непрозрачность меняется на стороне клиента ([Просмотр полноразмерное изображение](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Вперед](adjusting-the-z-index-of-a-dropshadow-vb.md)
