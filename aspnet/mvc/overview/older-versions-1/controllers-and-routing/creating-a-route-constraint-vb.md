---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: "Создание ограничения маршрута (VB) | Документы Microsoft"
author: StephenWalther
description: "В этом учебнике Стивен Вальтер показано, как можно контролировать, как браузер запрашивает маршруты соответствия путем создания ограничения маршрута с помощью регулярных выражений."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 67ff2666f4558abd4f8d9bddffd7aef8bb68d7bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-route-constraint-vb"></a>Создание ограничения маршрута (Visual Basic)
====================
по [Стивен Вальтер](https://github.com/StephenWalther)

> В этом учебнике Стивен Вальтер показано, как можно контролировать, как браузер запрашивает маршруты соответствия путем создания ограничения маршрута с помощью регулярных выражений.


Ограничения маршрута используется для ограничения запросы браузера, которые соответствуют конкретной маршрута. Регулярное выражение можно использовать для указания ограничения маршрута.

Например предположим, что в файле Global.asax вы определили маршрута в листинге 1.

**Листинг 1 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

Листинг 1 содержит маршрут с именем продукта. Маршрут продукта можно использовать для сопоставления запросы браузера ProductController, содержащихся в списке 2.

**Листинг 2 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Обратите внимание, что Details() действию, предоставляемым контроллером продукта принимает один параметр с именем productId. Этот параметр является параметром целое число со знаком.

Маршрут, определенный в список 1 будет соответствовать любой из следующих URL-адресов:

- / Продукта/23
- / Продукта/7

К сожалению маршрут будет также сопоставлять следующие URL-адреса:

- / Продукта или сведения
- / Продукта или apple

Поскольку действие Details() ожидает целочисленный параметр, выполняющего запрос, содержащий нечто, отличное от целочисленное значение приведет к ошибке. К примеру при вводе /Product/apple URL-адрес в адресную строку браузера будет возвращена страница ошибки на рис. 1.


[![Диалоговое окно нового проекта](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**На рисунке 01**: страница разрезать ([щелкните, чтобы просмотреть полноразмерное изображение](creating-a-route-constraint-vb/_static/image2.png))


Что Вы действительно хотите сделать — только соответствует URL-адреса, содержащие productId правильную целое число со знаком. Ограничение можно использовать при определении маршрута для ограничения URL-адреса, которые соответствуют маршруту. Измененный маршрут продукта в списке 3 содержит ограничение регулярное выражение, которое соответствует только целые числа.

**Листинг 3 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

\D+ регулярного выражения соответствует одной или более целых чисел. Это ограничение в результате сопоставления следующие URL-адреса маршрута продукта:

- / Продукта/3
- / Product-8999

Но не следующие URL-адреса:

- / Продукта или apple
- / Продукта

Эти запросы браузера будет обрабатываться по другому маршруту или, если отсутствуют соответствующие маршруты *не удалось найти ресурс* будет возвращена ошибка.

>[!div class="step-by-step"]
[Назад](creating-custom-routes-vb.md)
[Вперед](creating-a-custom-route-constraint-vb.md)
