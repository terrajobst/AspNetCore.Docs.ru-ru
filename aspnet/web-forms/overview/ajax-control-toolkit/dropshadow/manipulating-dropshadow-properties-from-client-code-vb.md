---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Изменения свойств DropShadow из клиентского кода (VB) | Документы Microsoft
author: wenz
description: Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью. Также можно изменить свойства этого расширителя с помощью клиента JavaScrip...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b5b024811ea511e67ce180169de9f0b7e3ef51d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870912"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="7dc54-104">Изменения свойств DropShadow из клиентского кода (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="7dc54-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="7dc54-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7dc54-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7dc54-106">[Загрузить код](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7dc54-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="7dc54-107">Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью.</span><span class="sxs-lookup"><span data-stu-id="7dc54-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="7dc54-108">Также можно изменить свойства этого расширителя с помощью кода JavaScript клиента.</span><span class="sxs-lookup"><span data-stu-id="7dc54-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="7dc54-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="7dc54-109">Overview</span></span>

<span data-ttu-id="7dc54-110">Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью.</span><span class="sxs-lookup"><span data-stu-id="7dc54-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="7dc54-111">Также можно изменить свойства этого расширителя с помощью кода JavaScript клиента.</span><span class="sxs-lookup"><span data-stu-id="7dc54-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="7dc54-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="7dc54-112">Steps</span></span>

<span data-ttu-id="7dc54-113">Код начинается со панель, содержащая несколько строк текста.</span><span class="sxs-lookup"><span data-stu-id="7dc54-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="7dc54-114">Связанный класс CSS предоставляет панели Цвет фона для работы с низким приоритетом:</span><span class="sxs-lookup"><span data-stu-id="7dc54-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="7dc54-115">`DropShadowExtender` Добавляется расширение панель с тенью, равным 50% прозрачности:</span><span class="sxs-lookup"><span data-stu-id="7dc54-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="7dc54-116">Затем ASP.NET AJAX `ScriptManager` управления включает набор средств управления для работы:</span><span class="sxs-lookup"><span data-stu-id="7dc54-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="7dc54-117">Другую панель содержит две связи JavaScript задайте прозрачность тени: минус ссылку регулирует прозрачность тени, плюс ссылку повышает его.</span><span class="sxs-lookup"><span data-stu-id="7dc54-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="7dc54-118">Функция JavaScript, которая `changeOpacity()` необходимо сначала найдите `DropShadowExtender` управления на странице.</span><span class="sxs-lookup"><span data-stu-id="7dc54-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="7dc54-119">Определяет ASP.NET AJAX `$find()` метод для точно этой задачи.</span><span class="sxs-lookup"><span data-stu-id="7dc54-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="7dc54-120">Затем `get_Opacity()` метод извлекает текущий непрозрачность `set_Opacity()` метод устанавливает его.</span><span class="sxs-lookup"><span data-stu-id="7dc54-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="7dc54-121">Код JavaScript помещает текущее значение непрозрачности в `<label>` элемента:</span><span class="sxs-lookup"><span data-stu-id="7dc54-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="7dc54-122">[![Непрозрачность меняется на стороне клиента](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7dc54-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="7dc54-123">Непрозрачность меняется на стороне клиента ([Просмотр полноразмерное изображение](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7dc54-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7dc54-124">Назад</span><span class="sxs-lookup"><span data-stu-id="7dc54-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
