---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: "Отобразить сведения об элементе | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 0b6ae9384843712cae824ea662b984a40f021e57
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="display-item-details"></a><span data-ttu-id="e434e-102">Отобразить сведения о элемента</span><span class="sxs-lookup"><span data-stu-id="e434e-102">Display Item Details</span></span>
====================
<span data-ttu-id="e434e-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e434e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e434e-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="e434e-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="e434e-105">В этом разделе будет добавлена возможность просмотра сведений для каждой книги.</span><span class="sxs-lookup"><span data-stu-id="e434e-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="e434e-106">В файле app.js добавьте следующий код для модели представления:</span><span class="sxs-lookup"><span data-stu-id="e434e-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="e434e-107">В Views/Home/Index.cshtml добавьте элемент привязки данных ссылку сведения:</span><span class="sxs-lookup"><span data-stu-id="e434e-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="e434e-108">Данный код привязывает обработчик щелчка для &lt;&gt; элемент `getBookDetail` функции в модели представления.</span><span class="sxs-lookup"><span data-stu-id="e434e-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="e434e-109">В том же файле Замените следующие комментариев:</span><span class="sxs-lookup"><span data-stu-id="e434e-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="e434e-110">следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e434e-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="e434e-111">Эта разметка создает таблицу, которая является привязкой к данным свойствам `detail` наблюдаемый в модели представления.</span><span class="sxs-lookup"><span data-stu-id="e434e-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="e434e-112">«&lt;!--Ko--&gt; &quot; синтаксис позволяет включить привязку маскирования вне элемента DOM.</span><span class="sxs-lookup"><span data-stu-id="e434e-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="e434e-113">В этом случае `if` привязка передает в этом разделе разметки для отображения только если `details` отлично от null.</span><span class="sxs-lookup"><span data-stu-id="e434e-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="e434e-114">Теперь, если запустить приложение и выберите один из &quot;сведений&quot; ссылки приложения будут выведены подробности книги.</span><span class="sxs-lookup"><span data-stu-id="e434e-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

>[!div class="step-by-step"]
<span data-ttu-id="e434e-115">[Назад](part-7.md)
[Вперед](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="e434e-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
