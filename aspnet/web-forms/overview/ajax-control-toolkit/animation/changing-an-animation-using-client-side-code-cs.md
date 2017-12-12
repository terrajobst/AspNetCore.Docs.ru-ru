---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: "Изменения анимации с помощью кода на стороне клиента (C#) | Документы Microsoft"
author: wenz
description: "Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Можно также анимации..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 6dcaeac073f54b0804fe3acf7ec22491b1cbbba5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="changing-an-animation-using-client-side-code-c"></a>Изменения анимации с помощью кода на стороне клиента (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Также можно изменить анимации с помощью настраиваемый код JavaScript на стороне клиента.


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Также можно изменить анимации с помощью настраиваемый код JavaScript на стороне клиента.

## <a name="steps"></a>Шаги

Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

Анимация применяется панель текста, который выглядит следующим образом:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

Фактический анимации, запускаемого по HTML-кнопок:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Следует отметить, что не `<Animations>` узла в `AnimationExtender` элемента управления. Настраиваемый код JavaScript используется для предоставления анимации для использования с элементом управления.

Как и в случае с сервером API `AnimationExtender`, невозможно просто присвоить расширителя анимации еще. Однако расширителя предоставлять несколько методов для чтения и записи анимации регистрации различных событий (`OnClick`, `OnLoad`и так далее). Далее приводятся некоторые примеры.

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Формат возвращаемого значения `get_*()` функции и формат аргумента для `set_*()` функции представляет собой строку JSON, предоставляя объектное представление XML-разметку, которые бы. В настоящее время невозможно передать объект в, но это можно считать объект из данной анимации (`get_OnXXXBehavior()` методов).

Вот строка JSON (без разделителя кавычек и форматировать хорошо) представляющий анимации запуска по щелчку кнопки, но анимации панели, его размера и исчезновение одновременно:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

Следующий код JavaScript назначает это descripting JSON для `OnClick` анимации текущего расширителя и запускает ее:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![Анимация выполняется немедленно, без щелчка мыши (и с минимальными разметкой)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

Анимация выполняется немедленно, без щелчок мыши (и с минимальными разметкой) ([Просмотр полноразмерное изображение](changing-an-animation-using-client-side-code-cs/_static/image3.png))

>[!div class="step-by-step"]
[Назад](executing-animations-using-client-side-code-cs.md)
[Вперед](animating-an-updatepanel-control-cs.md)
