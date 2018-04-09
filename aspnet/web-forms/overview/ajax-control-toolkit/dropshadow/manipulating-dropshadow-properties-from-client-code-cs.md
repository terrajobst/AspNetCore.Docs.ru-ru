---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Изменения свойств DropShadow из клиентского кода (C#) | Документы Microsoft
author: wenz
description: Настройка интерфейса редактирования DataList
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 37a7784e1d42477e31938e1d15495993ac86fc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="c0100-103">Изменения свойств DropShadow из клиентского кода (C#)</span><span class="sxs-lookup"><span data-stu-id="c0100-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="c0100-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c0100-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c0100-105">[Загрузить код](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c0100-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="c0100-106">Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью.</span><span class="sxs-lookup"><span data-stu-id="c0100-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="c0100-107">Также можно изменить свойства этого расширителя с помощью кода JavaScript клиента.</span><span class="sxs-lookup"><span data-stu-id="c0100-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="c0100-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="c0100-108">Overview</span></span>

<span data-ttu-id="c0100-109">Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью.</span><span class="sxs-lookup"><span data-stu-id="c0100-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="c0100-110">Также можно изменить свойства этого расширителя с помощью кода JavaScript клиента.</span><span class="sxs-lookup"><span data-stu-id="c0100-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="c0100-111">Шаги</span><span class="sxs-lookup"><span data-stu-id="c0100-111">Steps</span></span>

<span data-ttu-id="c0100-112">Код начинается со панель, содержащая несколько строк текста.</span><span class="sxs-lookup"><span data-stu-id="c0100-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="c0100-113">Связанный класс CSS предоставляет панели Цвет фона для работы с низким приоритетом:</span><span class="sxs-lookup"><span data-stu-id="c0100-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="c0100-114">`DropShadowExtender` Добавляется расширение панель с тенью, равным 50% прозрачности:</span><span class="sxs-lookup"><span data-stu-id="c0100-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="c0100-115">Затем ASP.NET AJAX `ScriptManager` управления включает набор средств управления для работы:</span><span class="sxs-lookup"><span data-stu-id="c0100-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="c0100-116">Другую панель содержит две связи JavaScript задайте прозрачность тени: минус ссылку регулирует прозрачность тени, плюс ссылку повышает его.</span><span class="sxs-lookup"><span data-stu-id="c0100-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="c0100-117">Функция JavaScript, которая `changeOpacity()` необходимо сначала найдите `DropShadowExtender` управления на странице.</span><span class="sxs-lookup"><span data-stu-id="c0100-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="c0100-118">Определяет ASP.NET AJAX `$find()` метод для точно этой задачи.</span><span class="sxs-lookup"><span data-stu-id="c0100-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="c0100-119">Затем `get_Opacity()` метод извлекает текущий непрозрачность `set_Opacity()` метод устанавливает его.</span><span class="sxs-lookup"><span data-stu-id="c0100-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="c0100-120">Код JavaScript помещает текущее значение непрозрачности в `<label>` элемента:</span><span class="sxs-lookup"><span data-stu-id="c0100-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="c0100-121">[![Непрозрачность меняется на стороне клиента](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c0100-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="c0100-122">Непрозрачность меняется на стороне клиента ([Просмотр полноразмерное изображение](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c0100-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c0100-123">[Назад](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Вперед](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c0100-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
