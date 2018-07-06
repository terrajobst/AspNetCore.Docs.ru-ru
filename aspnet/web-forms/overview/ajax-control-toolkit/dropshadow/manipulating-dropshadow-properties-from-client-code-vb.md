---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Обработка свойств DropShadow из клиентского кода (Visual Basic) | Документация Майкрософт
author: wenz
description: Элемент управления DropShadow в AJAX Control Toolkit расширяет панель с эффектом отбрасывания тени. Также можно изменить свойства этого расширителя с помощью клиента JavaScrip...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b9220eecca224c2b1e0f19c972c6c2a4dc9c7d35
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841413"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a>Обработка свойств DropShadow из клиентского кода (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> Элемент управления DropShadow в AJAX Control Toolkit расширяет панель с эффектом отбрасывания тени. Также можно изменить свойства этого расширителя с помощью клиентского кода JavaScript.


## <a name="overview"></a>Обзор

Элемент управления DropShadow в AJAX Control Toolkit расширяет панель с эффектом отбрасывания тени. Также можно изменить свойства этого расширителя с помощью клиентского кода JavaScript.

## <a name="steps"></a>Шаги

Код начинается с панель, содержащую несколько строк текста:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

Связанный класс CSS предоставляет панели неплохо фоновый цвет:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

`DropShadowExtender` Добавляется расширение панели с помощью эффекта тени, непрозрачность, равным 50%:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

Затем, ASP.NET AJAX `ScriptManager` управления включает набор средств управления для работы:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

Другую панель содержит две ссылки на JavaScript для непрозрачности тени: ссылку "минус" уменьшается прозрачности тени, плюс ссылку повышает его.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

Функция JavaScript, которая `changeOpacity()` необходимо затем найти `DropShadowExtender` управления на этой странице. AJAX для ASP.NET определяет `$find()` метод для именно этой задачи. Затем `get_Opacity()` метод извлекает текущий непрозрачность `set_Opacity()` метод устанавливает его. Код JavaScript помещает текущее значение непрозрачности в `<label>` элемент:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


[![Прозрачность изменяется на стороне клиента](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

Прозрачность изменяется на стороне клиента ([Просмотр полноразмерного изображения](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](adjusting-the-z-index-of-a-dropshadow-vb.md)
