---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Выполнение анимации с помощью кода на стороне клиента (Visual Basic) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Выполнение анимации...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: e2b8c499edaccc70d439a576feb80e91cb28c1c7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="executing-animations-using-client-side-code-vb"></a>Выполнение анимации с помощью кода на стороне клиента (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Выполнения анимации может также запускаться с помощью настраиваемый код JavaScript на стороне клиента.


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Выполнения анимации может также запускаться с помощью настраиваемый код JavaScript на стороне клиента.

## <a name="steps"></a>Шаги

Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

Анимация применяется панель текста, который выглядит следующим образом:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

В пределах `<Animations>` узла, используйте `<OnClick>` однократного запуска анимации пользователь нажимает кнопку на панели. Добавьте две анимации для выполнения parallelly:

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

Для примера анимации (и другие анимации, созданные с помощью набора средств управления) выполняется с помощью кода JavaScript, когда страница выполняется. Во-первых, мы необходим доступ к `AnimationExtender` элемента управления. Библиотека ASP.NET AJAX предоставляет `$find()` функции для выполнения этой задачи:

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

`AnimationExtender` Элемент управления предоставляет широкие возможности API, включая методы с именами, идентичными обработчикам событий, используемых в XML-разметку: `OnClick()`, `OnLoad()`, и т. д. Например, вызов `OnClick()` метод выполняется анимация в течение `<OnClick>` элемент `AnimationExtender` управления:

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

Ниже приведен полный код JavaScript клиентского имитацией нажмите на панели после полной загрузки страницы Обратите внимание, что `pageLoad()` вызываемая ASP.NET AJAX один раз страницы используется имя функции и все включены были библиотек JavaScript загрузить.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


[![Анимация выполняется немедленно, без щелчка кнопкой мыши](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

Анимация выполняется немедленно, без щелчок мыши ([Просмотр полноразмерное изображение](executing-animations-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](modifying-animations-from-the-server-side-vb.md)
> [Вперед](changing-an-animation-using-client-side-code-vb.md)
