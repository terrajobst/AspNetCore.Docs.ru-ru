---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: "Изменение анимации со стороны сервера (C#) | Документы Microsoft"
author: wenz
description: "Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Кроме того, можете анимации..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: e1f9bda03b3e3edf3bbdc591cde9858944d39321
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="modifying-animations-from-the-server-side-c"></a><span data-ttu-id="40cf8-104">Изменение анимации со стороны сервера (C#)</span><span class="sxs-lookup"><span data-stu-id="40cf8-104">Modifying Animations From The Server Side (C#)</span></span>
====================
<span data-ttu-id="40cf8-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="40cf8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="40cf8-106">[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="40cf8-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span></span>

> <span data-ttu-id="40cf8-107">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="40cf8-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="40cf8-108">На стороне сервера также может измениться анимации</span><span class="sxs-lookup"><span data-stu-id="40cf8-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="40cf8-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="40cf8-109">Overview</span></span>

<span data-ttu-id="40cf8-110">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="40cf8-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="40cf8-111">На стороне сервера также может измениться анимации</span><span class="sxs-lookup"><span data-stu-id="40cf8-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="40cf8-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="40cf8-112">Steps</span></span>

<span data-ttu-id="40cf8-113">Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="40cf8-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

<span data-ttu-id="40cf8-114">Анимация применяется панель текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="40cf8-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

<span data-ttu-id="40cf8-115">В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:</span><span class="sxs-lookup"><span data-stu-id="40cf8-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

<span data-ttu-id="40cf8-116">Остальная часть кода выполняется на стороне сервера, а не разметку. Вместо этого он использует код для создания `AnimationExtender` управления:</span><span class="sxs-lookup"><span data-stu-id="40cf8-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

<span data-ttu-id="40cf8-117">Однако набор средств управления в настоящее время не предоставляет доступ к API для создания отдельных анимации.</span><span class="sxs-lookup"><span data-stu-id="40cf8-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="40cf8-118">Однако можно задать `AnimationExtender`анимации свойства в строку, содержащего разметку XML, которая используется при назначении анимации декларативно.</span><span class="sxs-lookup"><span data-stu-id="40cf8-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="40cf8-119">Чтобы создать XML, которые не должны содержать `<Animations>` элемента, можно использовать XML .NET Framework поддерживает или, как показано в следующем коде просто указать строку:</span><span class="sxs-lookup"><span data-stu-id="40cf8-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

<span data-ttu-id="40cf8-120">Наконец, добавьте `AnimationExtender` управления для текущей страницы в `<form runat="server">` элемент, убедившись, что анимация включено и выполняется:</span><span class="sxs-lookup"><span data-stu-id="40cf8-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


<span data-ttu-id="40cf8-121">[![Анимация создается с помощью C# и Visual Basic код со стороны сервера](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="40cf8-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span></span>

<span data-ttu-id="40cf8-122">Анимация создается с помощью C# и Visual Basic код со стороны сервера ([Просмотр полноразмерное изображение](modifying-animations-from-the-server-side-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="40cf8-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="40cf8-123">[Назад](triggering-an-animation-in-another-control-cs.md)
[Вперед](executing-animations-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="40cf8-123">[Previous](triggering-an-animation-in-another-control-cs.md)
[Next](executing-animations-using-client-side-code-cs.md)</span></span>
