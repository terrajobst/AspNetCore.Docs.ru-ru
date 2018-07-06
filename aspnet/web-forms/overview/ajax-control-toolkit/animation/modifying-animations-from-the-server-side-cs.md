---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Изменение анимации со стороны сервера (C#) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Анимации могут также...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 8954f025873ea553fde26c6e2330ce6e5be2b539
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804058"
---
<a name="modifying-animations-from-the-server-side-c"></a>Изменение анимации со стороны сервера (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Также может измениться анимации на стороне сервера


## <a name="overview"></a>Обзор

Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Также может измениться анимации на стороне сервера

## <a name="steps"></a>Шаги

Во-первых, включите `ScriptManager` страницы; затем ASP.NET AJAX library загружена, что позволяет использовать набор средств управления:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

Анимация будет применяться к панели текста, который выглядит следующим образом:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

В связанный класс CSS для панели определить цвет фона, удобная и также установить фиксированную ширину для панели:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

Остальная часть кода выполняется на стороне сервера и не использует разметки; Вместо этого он использует код для создания `AnimationExtender` управления:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

Тем не менее набор элементов управления в настоящее время не предоставляет доступ к API для создания отдельных анимаций. Тем не менее можно задать `AnimationExtender`свойство Animations в строку содержащего XML-разметку, при назначении анимации декларативно. Чтобы создать XML, который не может содержать `<Animations>` элемент, можно использовать XML платформы .NET Framework поддерживает или, как в следующем коде, просто укажите строку:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Наконец, добавьте `AnimationExtender` управления в текущую страницу в `<form runat="server">` элемент, убедившись, что анимация включена и выполняется:

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![Анимация создается с использованием серверного C# и Visual Basic кода](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

Анимация создается с использованием серверного C# и Visual Basic кода ([Просмотр полноразмерного изображения](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](triggering-an-animation-in-another-control-cs.md)
> [Вперед](executing-animations-using-client-side-code-cs.md)
