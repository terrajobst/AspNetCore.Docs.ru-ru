---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Обработка свойств DropShadow из клиентского кода (C#) | Документация Майкрософт
author: wenz
description: Настройка интерфейса правки элемента управления DataList
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 7255e8b2b4c3c5f1c5ebf5a05851c13c07e49c5e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820315"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="d97e8-103">Обработка свойств DropShadow из клиентского кода (C#)</span><span class="sxs-lookup"><span data-stu-id="d97e8-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="d97e8-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d97e8-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d97e8-105">[Скачать код](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d97e8-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="d97e8-106">Элемент управления DropShadow в AJAX Control Toolkit расширяет панель с эффектом отбрасывания тени.</span><span class="sxs-lookup"><span data-stu-id="d97e8-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="d97e8-107">Также можно изменить свойства этого расширителя с помощью клиентского кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d97e8-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="d97e8-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="d97e8-108">Overview</span></span>

<span data-ttu-id="d97e8-109">Элемент управления DropShadow в AJAX Control Toolkit расширяет панель с эффектом отбрасывания тени.</span><span class="sxs-lookup"><span data-stu-id="d97e8-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="d97e8-110">Также можно изменить свойства этого расширителя с помощью клиентского кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d97e8-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="d97e8-111">Шаги</span><span class="sxs-lookup"><span data-stu-id="d97e8-111">Steps</span></span>

<span data-ttu-id="d97e8-112">Код начинается с панель, содержащую несколько строк текста:</span><span class="sxs-lookup"><span data-stu-id="d97e8-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="d97e8-113">Связанный класс CSS предоставляет панели неплохо фоновый цвет:</span><span class="sxs-lookup"><span data-stu-id="d97e8-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="d97e8-114">`DropShadowExtender` Добавляется расширение панели с помощью эффекта тени, непрозрачность, равным 50%:</span><span class="sxs-lookup"><span data-stu-id="d97e8-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="d97e8-115">Затем, ASP.NET AJAX `ScriptManager` управления включает набор средств управления для работы:</span><span class="sxs-lookup"><span data-stu-id="d97e8-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="d97e8-116">Другую панель содержит две ссылки на JavaScript для непрозрачности тени: ссылку "минус" уменьшается прозрачности тени, плюс ссылку повышает его.</span><span class="sxs-lookup"><span data-stu-id="d97e8-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="d97e8-117">Функция JavaScript, которая `changeOpacity()` необходимо затем найти `DropShadowExtender` управления на этой странице.</span><span class="sxs-lookup"><span data-stu-id="d97e8-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="d97e8-118">AJAX для ASP.NET определяет `$find()` метод для именно этой задачи.</span><span class="sxs-lookup"><span data-stu-id="d97e8-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="d97e8-119">Затем `get_Opacity()` метод извлекает текущий непрозрачность `set_Opacity()` метод устанавливает его.</span><span class="sxs-lookup"><span data-stu-id="d97e8-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="d97e8-120">Код JavaScript помещает текущее значение непрозрачности в `<label>` элемент:</span><span class="sxs-lookup"><span data-stu-id="d97e8-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="d97e8-121">[![Прозрачность изменяется на стороне клиента](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d97e8-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="d97e8-122">Прозрачность изменяется на стороне клиента ([Просмотр полноразмерного изображения](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d97e8-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d97e8-123">[Назад](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Вперед](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d97e8-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
