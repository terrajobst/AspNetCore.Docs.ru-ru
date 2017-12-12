---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: "Разрешение только для некоторых символов в текстовом поле (Visual Basic) | Документы Microsoft"
author: wenz
description: "Проверяющие элементы управления ASP.NET гарантирует, что вводимые пользователем данные разрешены только определенные символы. Однако это по-прежнему не препятствует пользователям вводить недопустимые..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: b41ec1dfda5d85c625026e1f1e1ecd7e190ee3ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Разрешение только для некоторых символов в текстовом поле (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> Проверяющие элементы управления ASP.NET гарантирует, что вводимые пользователем данные разрешены только определенные символы. Однако это по-прежнему не запрещает пользователям вводить недопустимые символы и при попытке отправить форму.


## <a name="overview"></a>Обзор

Проверяющие элементы управления ASP.NET гарантирует, что вводимые пользователем данные разрешены только определенные символы. Однако это по-прежнему не запрещает пользователям вводить недопустимые символы и при попытке отправить форму.

## <a name="steps"></a>Шаги

Содержит набор элементов управления ASP.NET AJAX `FilteredTextBox` элемента управления, который расширяет текстовое поле. После активации только определенный набор символов можно ввести в поле.

Чтобы это работало, необходимо сначала обычным образом ASP.NET AJAX `ScriptManager` открывается библиотеки JavaScript, которые также используются в наборе элементов управления ASP.NET AJAX:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

Затем мы должны текстовое поле.

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Наконец `FilteredTextBoxExtender` управления отвечает за ограничение символы разрешены для ввода пользователя. Сначала следует задать `TargetControlID` атрибут `ID` из `TextBox` элемента управления. Выберите один из доступных `FilterType` значений:

- `Custom`по умолчанию; Вы должны предоставить список допустимых символов
- `LowercaseLetters`только строчные буквы
- `Numbers`только цифры
- `UppercaseLetters`только прописные буквы

Если `Custom FilterType` используется, `ValidChars` свойства должен быть задан и указать список символов, которые могут быть типизированы. Кстати: при попытке вставить текст в текстовом поле, будут удалены все недопустимые символы.

Разметка для `FilteredTextBoxExtender` управления, который разрешает только цифры (то, что также было бы возможно при `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Запустить страницу и повторите вводить буквы, если включить JavaScript, работать не будет; Тем не менее знаков, отображаемых на странице. Однако обратите внимание, что защита `FilteredTextBox` предоставляет не является подтверждением маркера: если JavaScript включен, все данные могут вводиться в текстовом поле, поэтому следует использовать средства дополнительную проверку, т. е. ASP. NET проверяющие элементы управления.


[![Можно вводить только цифры](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Можно вводить только цифры ([Просмотр полноразмерное изображение](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

>[!div class="step-by-step"]
[Назад](allowing-only-certain-characters-in-a-text-box-cs.md)
