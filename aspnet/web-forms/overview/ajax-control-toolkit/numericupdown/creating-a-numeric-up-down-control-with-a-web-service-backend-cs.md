---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Создание числового вверх или вниз элемента управления с помощью серверной части веб-службы (C#) | Документы Microsoft
author: wenz
description: Вместо позволить пользователю ввести значение в поле Проверка, числовой вверх и вниз (то есть в Windows и других операционных системах) может оказаться как дополнительные c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 942902bdba93fe4fef8a9122403c6d5c62e6123c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868819"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a>Создание числового элемента управления вверх/вниз с серверной части веб-службы (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)

> Вместо позволить пользователю ввести значение в поле проверьте, может оказаться как дополнительные числовой для увеличения или уменьшения управления (который существует в Windows и других операционных системах) все возможности. По умолчанию элемент управления NumericUpDown всегда увеличивает или уменьшает значение на 1, но веб-службы подтверждает большую гибкость.


## <a name="overview"></a>Обзор

Вместо позволить пользователю ввести значение в поле проверьте, может оказаться как дополнительные числовой для увеличения или уменьшения управления (который существует в Windows и других операционных системах) все возможности. По умолчанию `NumericUpDown` управления всегда увеличивает или уменьшает значение на 1, но веб-службы подтверждает большую гибкость.

## <a name="steps"></a>Шаги

Содержит набор элементов управления ASP.NET AJAX `NumericUpDown` расширителя, который автоматически добавляет две кнопки в текстовое поле: одно для увеличения количества его значения, один для его уменьшения. Однако элемент управления также поддерживает вызов веб-службы (или вызов метода страницы). При каждом щелчке вверх или вниз, JavaScript, код подключается к веб-сервер и выполняет метод существует. Сигнатура метода является приведенному ниже:

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

`current` Аргумент — это текущее значение в текстовом поле; `tag` атрибут имеет дополнительный контекст данных, можно задать в качестве свойства `NumericUpDown` расширителя (но не обязательно).

Для этого образца числовые вверх или вниз элемента управления должны разрешать только значения, которые представляют собой степени двух: 1, 2, 4, 8, 16, 32, 64 и т. д. Таким образом метод выполняется, когда пользователь хочет увеличить значение необходимо double старое значение; другой метод должны деления значения на два. Вот полный веб-службы:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

Наконец создайте новую страницу ASP.NET. Как обычно требуется `ScriptManager` управления `TextBox` управления и `NumericUpDownExtender` элемента управления. Для второго вы должны предоставить информации веб-службы:

- `ServiceDownMethod` Имя списка веб-метод или метод страницы
- `ServiceDownPath` путь к веб-службы с вниз метода службы; пропустить, если вы используете метод страницы
- `ServiceUpMethod` Имя вверх веб-метод или метод страницы
- `ServiceUpPath` путь к веб-службы с вверх метода службы; пропустить, если вы используете метод страницы

Ниже приведен полный разметку страницы.

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

При запуске страницы, обратите внимание на то, как значение в текстовом поле всегда удваивается при щелчке на верхнюю кнопку и уменьшается вдвое при нажатии на кнопку ниже.


[![Отображаются только те числа, которые являются степенью числа 2](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

Отображаются только те числа, которые являются степенью числа 2 ([Просмотр полноразмерное изображение](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
