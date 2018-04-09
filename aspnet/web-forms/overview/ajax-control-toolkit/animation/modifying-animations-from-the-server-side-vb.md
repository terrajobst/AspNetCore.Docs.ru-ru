---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Изменение анимации со стороны сервера (VB) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Кроме того, можете анимации...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b9ce85fc5040b2318233b3c553c2cf53dd03555
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="6b7a3-104">Изменение анимации со стороны сервера (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="6b7a3-104">Modifying Animations From The Server Side (VB)</span></span>
====================
<span data-ttu-id="6b7a3-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6b7a3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6b7a3-106">[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6b7a3-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="6b7a3-107">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="6b7a3-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6b7a3-108">На стороне сервера также может измениться анимации</span><span class="sxs-lookup"><span data-stu-id="6b7a3-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="6b7a3-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="6b7a3-109">Overview</span></span>

<span data-ttu-id="6b7a3-110">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="6b7a3-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6b7a3-111">На стороне сервера также может измениться анимации</span><span class="sxs-lookup"><span data-stu-id="6b7a3-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="6b7a3-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="6b7a3-112">Steps</span></span>

<span data-ttu-id="6b7a3-113">Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="6b7a3-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="6b7a3-114">Анимация применяется панель текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6b7a3-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="6b7a3-115">В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:</span><span class="sxs-lookup"><span data-stu-id="6b7a3-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="6b7a3-116">Остальная часть кода выполняется на стороне сервера, а не разметку. Вместо этого он использует код для создания `AnimationExtender` управления:</span><span class="sxs-lookup"><span data-stu-id="6b7a3-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="6b7a3-117">Однако набор средств управления в настоящее время не предоставляет доступ к API для создания отдельных анимации.</span><span class="sxs-lookup"><span data-stu-id="6b7a3-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="6b7a3-118">Однако можно задать `AnimationExtender`анимации свойства в строку, содержащего разметку XML, которая используется при назначении анимации декларативно.</span><span class="sxs-lookup"><span data-stu-id="6b7a3-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="6b7a3-119">Чтобы создать XML, которые не должны содержать `<Animations>` элемента, можно использовать XML .NET Framework поддерживает или, как показано в следующем коде просто указать строку:</span><span class="sxs-lookup"><span data-stu-id="6b7a3-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="6b7a3-120">Наконец, добавьте `AnimationExtender` управления для текущей страницы в `<form runat="server">` элемент, убедившись, что анимация включено и выполняется:</span><span class="sxs-lookup"><span data-stu-id="6b7a3-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


<span data-ttu-id="6b7a3-121">[![Анимация создается с помощью C# и Visual Basic код со стороны сервера](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6b7a3-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="6b7a3-122">Анимация создается с помощью C# и Visual Basic код со стороны сервера ([Просмотр полноразмерное изображение](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6b7a3-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6b7a3-123">[Назад](triggering-an-animation-in-another-control-vb.md)
> [Вперед](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6b7a3-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
