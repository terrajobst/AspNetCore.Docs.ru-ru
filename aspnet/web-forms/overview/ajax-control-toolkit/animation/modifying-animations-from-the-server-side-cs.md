---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Изменение анимации со стороны сервера (C#) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Кроме того, можете анимации...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 6946875552c885ffb1f2a2eb7e728b85d7dd3973
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869014"
---
<a name="modifying-animations-from-the-server-side-c"></a>Изменение анимации со стороны сервера (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. На стороне сервера также может измениться анимации


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. На стороне сервера также может измениться анимации

## <a name="steps"></a>Шаги

Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

Анимация применяется панель текста, который выглядит следующим образом:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

Остальная часть кода выполняется на стороне сервера, а не разметку. Вместо этого он использует код для создания `AnimationExtender` управления:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

Однако набор средств управления в настоящее время не предоставляет доступ к API для создания отдельных анимации. Однако можно задать `AnimationExtender`анимации свойства в строку, содержащего разметку XML, которая используется при назначении анимации декларативно. Чтобы создать XML, которые не должны содержать `<Animations>` элемента, можно использовать XML .NET Framework поддерживает или, как показано в следующем коде просто указать строку:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Наконец, добавьте `AnimationExtender` управления для текущей страницы в `<form runat="server">` элемент, убедившись, что анимация включено и выполняется:

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![Анимация создается с помощью C# и Visual Basic код со стороны сервера](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

Анимация создается с помощью C# и Visual Basic код со стороны сервера ([Просмотр полноразмерное изображение](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](triggering-an-animation-in-another-control-cs.md)
> [Вперед](executing-animations-using-client-side-code-cs.md)
