---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Создание элемента управления оценки (C#) | Документы Microsoft
author: wenz
description: Многие веб-сайты из электронной коммерции на сайты сообщества предлагают пользователям скорость статей или элементы. Это обычно требует некоторых кодирование, но у нас есть...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a48cf0ed9402e2875e87ba7bdb76afc5f501a670
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879599"
---
<a name="creating-a-rating-control-c"></a>Создание элемента управления оценки (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)

> Многие веб-сайты из электронной коммерции на сайты сообщества предлагают пользователям скорость статей или элементы. Это обычно требует некоторых кодирование, но у нас есть набор элементов управления в своем распоряжении.


## <a name="overview"></a>Обзор

Многие веб-сайты из электронной коммерции на сайты сообщества предлагают пользователям скорость статей или элементы. Это обычно требует некоторых кодирование, но у нас есть набор элементов управления в своем распоряжении.

## <a name="steps"></a>Шаги

Во-первых, необходимо (минимум) два вида образов: один для заполненного Оценка элемента и один для оценки пустой элемент. Элемент оценка обычно является звезды или смайлик. Для этого сценария найти трех файлов, smiley.png и empty.png и done.png смайлик как часть загрузки исходного кода для этого учебника.

Затем создайте новый файл ASP.NET и начните с добавления `ScriptManager` управления к нему:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

Затем добавьте `Rating` управления из набора элементов управления ASP.NET AJAX. Следующие атрибуты должны быть заданы в этом примере:

- `CurrentRating` для использования первоначальной оценка
- `MaxRating` Максимальное ограничение
- `EmptyStarCssClass` класс CSS для использования при пустом элементе оценку (звездочка)
- `FilledStarCssClass` класс CSS для использования при заполнении элемента оценку (звездочка)
- `StarCssClass` класс CSS для видимой stat
- `WaitingStarCssClass` класс CSS для использования в процессе оценку отправляется обратно на сервер

А вот разметка, которая создает элемент управления оценок с пятью элементами (smileys), которых нет заполняется изначально:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

Три упоминаемого классов CSS теперь требуется для отображения файлов соответствующий образ, это легко сделать с помощью CSS:

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

Убедитесь, что укажите ширину и высоту три изображения, в противном случае может выглядеть немного сильно система отображения.

Наконец результат оценки должно быть отображаемое пользователю (или, по крайней мере, сохраненные в базе данных). Поэтому добавьте метки для вывода текста сообщения, а кнопка отправки для отправки обратно оценка формы на сервер:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

В серверный код, обращения к элементу управления оценка через его `ID` и последующий доступ к его `CurrentRating` свойство, являющееся числом элементов выбранным рейтингом в нашем примере значение в диапазоне от 0 до 5.

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

Сохраните страницу и загрузить его в адресную строку браузера. Происходит при наведении указателя мыши на элементы (изначально пустой) оценка влияния JavaScript: изменения рейтинга. При нажатии кнопки на наборе звезд, сохраняется текущая оценка. Наконец при отправке формы, серверный код генерирует выбранным рейтингом.


[![Создание оценок системы с минимальным объемом кода](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)

Создание систему оценок с помощью минимального программного кода ([Просмотр полноразмерное изображение](creating-a-rating-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](creating-a-rating-control-vb.md)
