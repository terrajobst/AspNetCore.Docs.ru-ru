---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Создание числового вверх/вниз элемента управления с помощью серверной веб-службы (Visual Basic) | Документация Майкрософт
author: wenz
description: Вместо позволить пользователю ввести значение в типа "флажок", числовой вверх/вниз элемента управления (который существует в Windows и других операционных системах) может оказаться как дополнительные c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: bf69b3e6932df528e8ccd2348ffa6f13132f99f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828542"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Создание числового элемента управления вверх/вниз с помощью серверной веб-службы (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> Вместо позволить пользователю ввести значение в типа "флажок", числовой вверх/вниз элемента управления (который существует в Windows и других операционных системах) может оказаться как более уверенно. По умолчанию элемент управления NumericUpDown всегда увеличивает или уменьшает значение на 1, но веб-службы подтверждает большую гибкость.


## <a name="overview"></a>Обзор

Вместо позволить пользователю ввести значение в типа "флажок", числовой вверх/вниз элемента управления (который существует в Windows и других операционных системах) может оказаться как более уверенно. По умолчанию `NumericUpDown` управления всегда увеличивает или уменьшает значение на 1, но веб-службы подтверждает большую гибкость.

## <a name="steps"></a>Шаги

ASP.NET AJAX Control Toolkit содержит `NumericUpDown` расширителя, который автоматически добавляет две кнопки в текстовое поле: одно для увеличения количества его значения, один для его уменьшения. Однако элемент управления также поддерживает вызов веб-службы (или вызов метода страницы). При каждом щелчке вверх или вниз, JavaScript, код подключается к веб-сервер и выполняет метод существует. Подпись метода — приведенному ниже:

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

`current` Аргумент является текущее значение в текстовом поле; `tag` атрибут является дополнительный контекст данных, которое можно задать в качестве свойства `NumericUpDown` расширитель (но не является обязательным).

В данном примере числовые вверх/вниз элемента управления должны разрешать только значения, являющиеся степенями двух: 1, 2, 4, 8, 16, 32, 64 и т. д. Следовательно метод, выполняемый, когда пользователь хочет увеличить значение должен double старое значение; другой метод необходимо разделить значение два. Итак, вот полный веб-службы:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

Наконец создайте новую страницу ASP.NET. Как правило, требуется `ScriptManager` элемента управления, `TextBox` управления и `NumericUpDownExtender` элемента управления. Для последнего необходимо предоставить данные веб-службы:

- `ServiceDownMethod` Имя от списка веб-метода или странице метод
- `ServiceDownPath` путь к веб-службы с помощью метода вниз службы; пропустить, если вы используете метод страницы
- `ServiceUpMethod` Имя вверх веб-метода или странице метод
- `ServiceUpPath` путь к веб-службы с помощью метода вверх службы; пропустить, если вы используете метод страницы

Вот полная разметка для страницы:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

При запуске страницы, обратите внимание на то, как значение в текстовое поле всегда удваивается при нажмите кнопку "верхний" и уменьшается вдвое при нажатии на кнопку ниже.


[![Отображаются только те числа, которые являются степенью числа 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

Отображаются только те числа, которые являются степенью двойки ([Просмотр полноразмерного изображения](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
