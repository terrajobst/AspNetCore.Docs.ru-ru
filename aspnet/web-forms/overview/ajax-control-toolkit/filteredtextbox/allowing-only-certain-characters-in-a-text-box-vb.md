---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Разрешение только некоторых символов в текстовом поле (Visual Basic) | Документация Майкрософт
author: wenz
description: Проверяющие элементы управления ASP.NET гарантирует, что вводимые пользователем данные разрешены только некоторых символов. Тем не менее это по-прежнему не позволяет пользователям вводить недопустимые...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: aec5a3af98cf40e460f4164fb8950e8029002937
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829113"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Разрешение только некоторых символов в текстовом поле (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> Проверяющие элементы управления ASP.NET гарантирует, что вводимые пользователем данные разрешены только некоторых символов. Тем не менее это по-прежнему не запрещает пользователям вводить недопустимые символы и при попытке отправить форму.


## <a name="overview"></a>Обзор

Проверяющие элементы управления ASP.NET гарантирует, что вводимые пользователем данные разрешены только некоторых символов. Тем не менее это по-прежнему не запрещает пользователям вводить недопустимые символы и при попытке отправить форму.

## <a name="steps"></a>Шаги

ASP.NET AJAX Control Toolkit содержит `FilteredTextBox` элемента управления, который расширяет текстового поля. После активации, можно вводить только определенный набор символов в поле.

Чтобы это работало, необходимо сначала обычным образом ASP.NET AJAX `ScriptManager` которого загружала библиотеки JavaScript, которые также используется ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

Затем нам нужно текстового поля:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Наконец `FilteredTextBoxExtender` берет на себя ограничение символов, пользователь может ввести элемент управления. Сначала задайте `TargetControlID` атрибут `ID` из `TextBox` элемента управления. Выберите один из доступных `FilterType` значений:

- `Custom` по умолчанию; Вы должны предоставить список допустимых знаков
- `LowercaseLetters` только строчные буквы
- `Numbers` только цифры
- `UppercaseLetters` только прописные буквы

Если `Custom FilterType` используется, `ValidChars` свойство должно быть задано и укажите список символов, которые может ввести. Между прочим: при попытке вставить текст в текстовое поле, будут удалены все недопустимые символы.

Далее приведена разметка для `FilteredTextBoxExtender` элемент управления, который допускает только цифры (то, что также было бы возможным с `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Запустите страницу и попробуйте ввести буквы в том случае, если включить JavaScript, он не будет работать; Тем не менее, на странице отображаются цифры. Тем не менее Обратите внимание, что защита `FilteredTextBox` предоставляет не пуленепробиваема: если JavaScript включен, все данные могут вводиться в текстовом поле, поэтому необходимо использовать средства дополнительной проверки, т. е. ASP. NET-элементов управления проверки.


[![Можно вводить только цифры](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Можно вводить только цифры ([Просмотр полноразмерного изображения](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](allowing-only-certain-characters-in-a-text-box-cs.md)
