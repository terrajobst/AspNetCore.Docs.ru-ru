---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Изменение анимаций с помощью клиентского кода (C#) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Можно также анимации...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 253377ef6019a672680c6e819349357627ef111b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828444"
---
<a name="changing-an-animation-using-client-side-code-c"></a>Изменение анимаций с помощью клиентского кода (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Также можно изменить анимации с помощью пользовательского кода JavaScript на стороне клиента.


## <a name="overview"></a>Обзор

Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Также можно изменить анимации с помощью пользовательского кода JavaScript на стороне клиента.

## <a name="steps"></a>Шаги

Во-первых, включите `ScriptManager` страницы; затем ASP.NET AJAX library загружена, что позволяет использовать набор средств управления:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

Анимация будет применяться к панели текста, который выглядит следующим образом:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

В связанный класс CSS для панели определить цвет фона, удобная и также установить фиксированную ширину для панели:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

Фактический анимация запускается с HTML-кнопок:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Затем добавьте `AnimationExtender` на страницу, предоставляя `ID`, `TargetControlID` атрибут и обязательным `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Следует отметить, что не `<Animations>` узел в `AnimationExtender` элемента управления. Пользовательский код JavaScript используется для предоставления анимации для использования с элементом управления.

Как и в API сервера из `AnimationExtender`, нет простого способа еще назначить анимации расширителя. Тем не менее расширителя предоставляют несколько методов для чтения и записи анимации зарегистрировано различные события (`OnClick`, `OnLoad`, и так далее). Далее приводятся некоторые примеры.

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Формат возвращаемого значения `get_*()` функции и формат аргумента для `set_*()` функции представляет собой строку JSON, предоставляя объектное представление XML-разметку, которые бы. В настоящее время невозможно передать объект в, но можно считать объект из данной анимации (`get_OnXXXBehavior()` методов).

Здесь представляет собой строку JSON (без кавычек разделителя и форматировать хорошо) представляющий анимацию, инициируемой этой кнопкой, но анимации на панели, изменяя его размер и исчезновение в то же время:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

Следующий код JavaScript назначает этот descripting JSON для `OnClick` анимации текущего расширителя и запускает его:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![Анимация запускается немедленно, без щелчка мыши (и с очень небольшой разметкой)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

Анимация запускается немедленно, без щелчка мышью (и с очень небольшой разметкой) ([Просмотр полноразмерного изображения](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](executing-animations-using-client-side-code-cs.md)
> [Вперед](animating-an-updatepanel-control-cs.md)
