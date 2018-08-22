---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Обработка свойств DropShadow из клиентского кода (Visual Basic) | Документация Майкрософт
author: wenz
description: Элемент управления DropShadow в AJAX Control Toolkit расширяет панель с эффектом отбрасывания тени. Также можно изменить свойства этого расширителя с помощью клиента JavaScrip...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: d8b8330174f6f49e96c42a0e15eeaf934bf24f87
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824063"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="49120-104">Обработка свойств DropShadow из клиентского кода (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="49120-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="49120-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="49120-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="49120-106">[Скачать код](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="49120-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="49120-107">Элемент управления DropShadow в AJAX Control Toolkit расширяет панель с эффектом отбрасывания тени.</span><span class="sxs-lookup"><span data-stu-id="49120-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="49120-108">Также можно изменить свойства этого расширителя с помощью клиентского кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="49120-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="49120-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="49120-109">Overview</span></span>

<span data-ttu-id="49120-110">Элемент управления DropShadow в AJAX Control Toolkit расширяет панель с эффектом отбрасывания тени.</span><span class="sxs-lookup"><span data-stu-id="49120-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="49120-111">Также можно изменить свойства этого расширителя с помощью клиентского кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="49120-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="49120-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="49120-112">Steps</span></span>

<span data-ttu-id="49120-113">Код начинается с панель, содержащую несколько строк текста:</span><span class="sxs-lookup"><span data-stu-id="49120-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="49120-114">Связанный класс CSS предоставляет панели неплохо фоновый цвет:</span><span class="sxs-lookup"><span data-stu-id="49120-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="49120-115">`DropShadowExtender` Добавляется расширение панели с помощью эффекта тени, непрозрачность, равным 50%:</span><span class="sxs-lookup"><span data-stu-id="49120-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="49120-116">Затем, ASP.NET AJAX `ScriptManager` управления включает набор средств управления для работы:</span><span class="sxs-lookup"><span data-stu-id="49120-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="49120-117">Другую панель содержит две ссылки на JavaScript для непрозрачности тени: ссылку "минус" уменьшается прозрачности тени, плюс ссылку повышает его.</span><span class="sxs-lookup"><span data-stu-id="49120-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="49120-118">Функция JavaScript, которая `changeOpacity()` необходимо затем найти `DropShadowExtender` управления на этой странице.</span><span class="sxs-lookup"><span data-stu-id="49120-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="49120-119">AJAX для ASP.NET определяет `$find()` метод для именно этой задачи.</span><span class="sxs-lookup"><span data-stu-id="49120-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="49120-120">Затем `get_Opacity()` метод извлекает текущий непрозрачность `set_Opacity()` метод устанавливает его.</span><span class="sxs-lookup"><span data-stu-id="49120-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="49120-121">Код JavaScript помещает текущее значение непрозрачности в `<label>` элемент:</span><span class="sxs-lookup"><span data-stu-id="49120-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="49120-122">[![Прозрачность изменяется на стороне клиента](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="49120-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="49120-123">Прозрачность изменяется на стороне клиента ([Просмотр полноразмерного изображения](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="49120-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="49120-124">Назад</span><span class="sxs-lookup"><span data-stu-id="49120-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
