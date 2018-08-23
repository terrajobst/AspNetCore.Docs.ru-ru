---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Свертывание и развертывание панели из кода JavaScript (VB) | Документация Майкрософт
author: wenz
description: Элемент управления CollapsiblePanel в ASP.NET AJAX Control Toolkit расширяет панель и предоставляет ему возможность свернуть его содержимое и разверните его...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 1f80a6979966a887db0557b4f1b98570e10b1ab7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837933"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a>Свертывание и развертывание панели из кода JavaScript (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)

> Элемент управления CollapsiblePanel в ASP.NET AJAX Control Toolkit расширяет панель и предоставляет ему возможность свернуть его содержимое и развернуть ее снова. Эти два действия также может запускаться из пользовательского кода JavaScript.


## <a name="overview"></a>Обзор

Элемент управления CollapsiblePanel в ASP.NET AJAX Control Toolkit расширяет панель и предоставляет ему возможность свернуть его содержимое и развернуть ее снова. Эти два действия также может запускаться из пользовательского кода JavaScript.

## <a name="steps"></a>Шаги

Во-первых, создайте новую страницу ASP.NET и включите `ScriptManager` внутри `<form>` элемент. Это загружает библиотеки ASP.NET AJAX, который необходим набор элементов управления:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

Затем создайте панели с определенным текстом, можно увидеть эффект развернут или свернут.

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

Как вы видите, панели ссылается на класс CSS, который приведен ниже (и по сути определяет фонового цвета и ширины панели):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

`CollapsiblePanelExtender` Элемент управления требует `TargetControlID` таким образом, набор средств знает, какая панель, чтобы свернуть или развернуть по запросу:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

К сожалению расширитель в настоящее время не предоставляет интерфейс API для сворачивать и разворачивать панели, но некоторые методы недокументированные происходит. Во-первых добавьте три кнопки HTML для страницы, который затем инициирует клиентским сценарием JavaScript, чтобы свернуть или развернуть содержимое панели:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

В коде JavaScript на стороне клиента (к работе с `<script type="text/javascript">`), `$find()` метод должен использоваться для доступа к `CollapsiblePanelExtender`. `$find("cpe")` Возвращает ссылку на него. Оттуда на конкретные методы решит поставленной задачи.

Метод для открытия (расширение) называется панели `_doOpen()`; в следующем коде реализуется `doOpen()` функция, вызываемая при нажатии первой кнопки:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

Для закрытия или свертывания панели, `_doClose()` метод должен быть выполнен. Поэтому когда пользователь щелкает второй кнопки, называется следующий код JavaScript:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

Третья кнопка переключает состояние панели: из свернуты развернут и наоборот. `CollapsiblePanelExtender` Предоставляет `toggle()` метод, который делает именно это: изменяет состояние панели. Однако имеется также другой подход (который внутренне используется `toggle()` метод): `get_Collapsed()` метод `CollapsiblePanelExtender()` сообщает о том, свернута ли панели или нет. В зависимости от возвращаемого значения функции, она затем либо развернут (`_doOpen()` метод) или свернут (`_doClose()`) метод:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


[![Третья кнопка изменяет состояние панели: из свернуть, чтобы развернутое и обратно](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)

Третья кнопка изменяет состояние панели: из свернуть, чтобы развернутые и назад ([Просмотр полноразмерного изображения](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](collapsing-and-expanding-a-panel-from-javascript-cs.md)
