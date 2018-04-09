---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Использование TextBoxWatermark с проверяющие элементы управления (C#) | Документы Microsoft
author: wenz
description: Элемент управления TextBoxWatermark в наборе элементов управления AJAX расширяет текстовое поле для отображения текста в поле. Когда пользователь щелкает в поле, оно я...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: b5cc7974f3444b54770cba54b991aab7b103f753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>Использование TextBoxWatermark с проверяющие элементы управления (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> Элемент управления TextBoxWatermark в наборе элементов управления AJAX расширяет текстовое поле для отображения текста в поле. Когда пользователь щелкает в поле, оно будет очищено. Если поле остается без ввода текста, заданными текст снова появится. Это может конфликтовать с элементов управления проверкой ASP.NET на одной странице, но может преодолеть эти проблемы.


## <a name="overview"></a>Обзор

`TextBoxWatermark` В наборе элементов управления AJAX, расширяет текстовое поле для отображения текста в поле. Когда пользователь щелкает в поле, оно будет очищено. Если поле остается без ввода текста, заданными текст снова появится. Это может конфликтовать с элементов управления проверкой ASP.NET на одной странице, но может преодолеть эти проблемы.

## <a name="steps"></a>Шаги

Базовая настройка образца является следующее: `TextBox` водяные знаки управления с помощью `TextBoxWatermarkExtender` элемента управления. Кнопка запускает обратной передачи и позже будет использоваться для запуска проверяющие элементы управления на странице. Кроме того `ScriptManager` управления необходим для инициализации ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Теперь добавьте `RequiredFieldValidator` элемент управления, который проверяет, существует ли текст в поле при отправке формы. `InitialValue` Свойства проверяющего элемента управления должно быть присвоено то же значение, которое используется в `TextBoxWatermarkExtender` управления: при отправке формы без изменений текстовое значение является значением водяного знака в ней:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Однако есть одна проблема этого подхода: Если клиент отключает JavaScript, текстовое поле не автоматически заполняемом с текста водяного знака, поэтому `RequiredFieldValidator` не вызывает сообщение об ошибке. Таким образом, второй `RequiredFieldValidator` управления требуется, который проверяет наличие пустые текстовые поля (пропуск `InitialValue` атрибута).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Поскольку оба проверяющие элементы управления используют `Display` = `"Dynamic"`, конечный пользователь не может отличить от внешнего вида, какой из двух проверяющие элементы управления было запущено; вместо этого выглядит так, будто был только один из них.

Наконец добавьте некоторые серверный код для вывода текста в поле, если проверяющий элемент управления выдаваться сообщение об ошибке:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![Проверяющий элемент управления сообщает, что в поле отсутствует текст](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

Проверяющий элемент управления сообщает, что в поле отсутствует текст ([Просмотр полноразмерное изображение](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](using-textboxwatermark-in-a-formview-cs.md)
> [Вперед](using-textboxwatermark-in-a-formview-vb.md)
