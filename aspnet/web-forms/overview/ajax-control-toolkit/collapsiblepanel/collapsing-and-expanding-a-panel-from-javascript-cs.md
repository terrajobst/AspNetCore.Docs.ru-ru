---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Свертывание и развертывание панели из кода JavaScript (C#) | Документация Майкрософт
author: wenz
description: Элемент управления CollapsiblePanel в ASP.NET AJAX Control Toolkit расширяет панель и предоставляет ему возможность свернуть его содержимое и разверните его...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 85a9c6e6cc56cc563eefea6bc863950ba71149ba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402760"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Свертывание и развертывание панели из кода JavaScript (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> Элемент управления CollapsiblePanel в ASP.NET AJAX Control Toolkit расширяет панель и предоставляет ему возможность свернуть его содержимое и развернуть ее снова. Эти два действия также может запускаться из пользовательского кода JavaScript.


## <a name="overview"></a>Обзор

Элемент управления CollapsiblePanel в ASP.NET AJAX Control Toolkit расширяет панель и предоставляет ему возможность свернуть его содержимое и развернуть ее снова. Эти два действия также может запускаться из пользовательского кода JavaScript.

## <a name="steps"></a>Шаги

Во-первых, создайте новую страницу ASP.NET и включите `ScriptManager` внутри `<form>` элемент. Это загружает библиотеки ASP.NET AJAX, который необходим набор элементов управления:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

Затем создайте панели с определенным текстом, можно увидеть эффект развернут или свернут.

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Как вы видите, панели ссылается на класс CSS, который приведен ниже (и по сути определяет фонового цвета и ширины панели):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

`CollapsiblePanelExtender` Элемент управления требует `TargetControlID` таким образом, набор средств знает, какая панель, чтобы свернуть или развернуть по запросу:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

К сожалению расширитель в настоящее время не предоставляет интерфейс API для сворачивать и разворачивать панели, но некоторые методы недокументированные происходит. Во-первых добавьте три кнопки HTML для страницы, который затем инициирует клиентским сценарием JavaScript, чтобы свернуть или развернуть содержимое панели:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

В коде JavaScript на стороне клиента (к работе с `<script type="text/javascript">`), `$find()` метод должен использоваться для доступа к `CollapsiblePanelExtender`. `$find("cpe")` Возвращает ссылку на него. Оттуда на конкретные методы решит поставленной задачи.

Метод для открытия (расширение) называется панели `_doOpen()`; в следующем коде реализуется `doOpen()` функция, вызываемая при нажатии первой кнопки:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Для закрытия или свертывания панели, `_doClose()` метод должен быть выполнен. Поэтому когда пользователь щелкает второй кнопки, называется следующий код JavaScript:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

Третья кнопка переключает состояние панели: из свернуты развернут и наоборот. `CollapsiblePanelExtender` Предоставляет `toggle()` метод, который делает именно это: изменяет состояние панели. Однако имеется также другой подход (который внутренне используется `toggle()` метод): `get_Collapsed()` метод `CollapsiblePanelExtender()` сообщает о том, свернута ли панели или нет. В зависимости от возвращаемого значения функции, она затем либо развернут (`_doOpen()` метод) или свернут (`_doClose()`) метод:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![Третья кнопка изменяет состояние панели: из свернуть, чтобы развернутое и обратно](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

Третья кнопка изменяет состояние панели: из свернуть, чтобы развернутые и назад ([Просмотр полноразмерного изображения](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](collapsing-and-expanding-a-panel-from-javascript-vb.md)
