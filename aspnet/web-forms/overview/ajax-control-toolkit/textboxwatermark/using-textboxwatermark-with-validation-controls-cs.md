---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Использование элемента управления TextBoxWatermark с проверяющими элементами управления (C#) | Документация Майкрософт
author: wenz
description: Элемент управления TextBoxWatermark в AJAX Control Toolkit расширяет текстовое поле для отображения текста в поле. Когда пользователь щелкает в поле, его я...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 35dd340ffce7b83303a44a0d6532ce73be5350f0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836719"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>Использование элемента управления TextBoxWatermark с проверяющими элементами управления (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> Элемент управления TextBoxWatermark в AJAX Control Toolkit расширяет текстовое поле для отображения текста в поле. Когда пользователь щелкает в поле, оно будет очищено. Если пользователь оставляет поле без ввода текста, заданными текст отображается повторно. Это может привести с проверяющими элементами управления ASP.NET, на той же странице, но может преодолеть эти проблемы.


## <a name="overview"></a>Обзор

`TextBoxWatermark` В AJAX Control Toolkit, расширяет текстовое поле для отображения текста в поле. Когда пользователь щелкает в поле, оно будет очищено. Если пользователь оставляет поле без ввода текста, заданными текст отображается повторно. Это может привести с проверяющими элементами управления ASP.NET, на той же странице, но может преодолеть эти проблемы.

## <a name="steps"></a>Шаги

Базовая настройка примера является следующее: `TextBox` водяными знаками элемента управления с помощью `TextBoxWatermarkExtender` элемента управления. Кнопка запускает обратную передачу и позже будет использоваться для активации элементов управления проверки на странице. Кроме того `ScriptManager` управления необходим для инициализации ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Теперь добавьте `RequiredFieldValidator` элемент управления, который проверяет, существует ли текстовое поле при отправке формы. `InitialValue` Свойства проверяющего элемента управления должно быть присвоено значение, которое используется в `TextBoxWatermarkExtender` управления: при отправке формы неизменными текстовое значение значение предела в ней:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Тем не менее есть одна проблема, при таком подходе: Если клиент отключает JavaScript, поле не автоматически заполняемом с текстом водяного знака, поэтому `RequiredFieldValidator` не вызывает сообщение об ошибке. Таким образом, второй `RequiredFieldValidator` управления необходим, который проверяет наличие пустом текстовом поле (пропуск `InitialValue` атрибут).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Так как оба проверяющие элементы управления используют `Display` = `"Dynamic"`, конечный пользователь не может отличить от внешнего вида, какой из двух проверяющие элементы управления произошло; вместо этого, похоже, возникла только один из них.

Наконец добавьте код на стороне сервера для вывода текст в поле, если не проверяющий элемент управления выдаваться сообщение об ошибке:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![Проверяющий элемент управления сообщает, что отсутствует текст в поле](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

Проверяющий элемент управления сообщает, что в поле отсутствует текст ([Просмотр полноразмерного изображения](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](using-textboxwatermark-in-a-formview-cs.md)
> [Вперед](using-textboxwatermark-in-a-formview-vb.md)
