---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Свертывание и развертывание панели из кода JavaScript (VB) | Документы Microsoft
author: wenz
description: Предоставляет ему возможность свернуть его содержимое и разверните его и расширяет их возможности панели управления CollapsiblePanel в наборе элементов управления ASP.NET AJAX...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 5cf61cd0d8204a5405ba62cd3884d66ccb21968b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873122"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a>Свертывание и развертывание панели из кода JavaScript (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)

> CollapsiblePanel управления в наборе элементов управления ASP.NET AJAX расширяет панели и предоставляет ему возможность свернуть его содержимое и развернуть его снова. Эти два действия могут также инициироваться из пользовательского кода JavaScript.


## <a name="overview"></a>Обзор

CollapsiblePanel управления в наборе элементов управления ASP.NET AJAX расширяет панели и предоставляет ему возможность свернуть его содержимое и развернуть его снова. Эти два действия могут также инициироваться из пользовательского кода JavaScript.

## <a name="steps"></a>Шаги

Во-первых, создайте новую страницу ASP.NET и включите `ScriptManager` в пределах одного `<form>` элемента. Это загружает библиотеки ASP.NET AJAX, необходимый для набора элементов управления:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

Затем создайте панель с какой-либо текст, можно увидеть эффект развернут или свернут:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

Как видите, панель ссылается на класс CSS, который приведен ниже (и по существу, определяет фоновый цвет и ширину панели):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

`CollapsiblePanelExtender` Управления требует `TargetControlID` таким образом, набор средств знает, какая панель, чтобы свернуть или развернуть по запросу:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

К сожалению расширитель в настоящее время не предоставляет определенный интерфейс API для свертывания и развертывания панели, но некоторые методы недокументированные происходит. Во-первых добавьте три кнопки HTML для страницы, которое затем запускает клиентский сценарий JavaScript, чтобы свернуть или развернуть содержимого панели:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

В код JavaScript на стороне клиента (работы с `<script type="text/javascript">`), `$find()` метод должен использоваться для доступа к `CollapsiblePanelExtender`. `$find("cpe")` Возвращает ссылку на него. Оттуда на определенных методов будет решения поставленной задачи.

Метод для открытия (расширение) называется панели `_doOpen()`; в следующем коде реализуется `doOpen()` функция, вызываемая при нажатии кнопки первой:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

Для закрытия или свертывания панели, `_doClose()` метод должен быть выполнен. Поэтому при щелчке вторая кнопка называется следующий код JavaScript:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

Третья кнопка переключает состояние панели: из свернуть, чтобы развернуть и наоборот. `CollapsiblePanelExtender` Предоставляет `toggle()` метод, который делает именно это: изменяет состояние панели. Однако есть еще один подход (который внутренне используется для `toggle()` метод): `get_Collapsed()` метод `CollapsiblePanelExtender()` сообщает, свернута ли панель или нет. В зависимости от возвращаемого значения функции, является панели, то либо разворачивать (`_doOpen()` метод) и в свернутом виде (`_doClose()`) метод:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


[![Третья кнопка изменяет состояние панели: из свернуть, чтобы развернутые и назад](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)

Третья кнопка изменяет состояние панели: из свернуть, чтобы развернутые и задней ([Просмотр полноразмерное изображение](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](collapsing-and-expanding-a-panel-from-javascript-cs.md)
