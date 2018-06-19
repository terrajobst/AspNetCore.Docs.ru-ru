---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-use-httpverbs-attributes-in-an-mvc-application
title: 'Инструкции: использование HttpVerbs атрибуты в MVC-приложении? | Документы Майкрософт'
author: rick-anderson
description: В этот видеоролик пиксел Крис показывает, как использовать атрибуты HttpVerbs для управления доступом к действия MVC. Во-первых пример приложения создается с использованием соадминистратора по умолчанию...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2009
ms.topic: article
ms.assetid: d2488a1d-0f3f-4994-8fbe-4f59b8c9503e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-use-httpverbs-attributes-in-an-mvc-application
msc.type: video
ms.openlocfilehash: 42817e712db0489f277ac65250b9c0f102088c0a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869534"
---
<a name="how-do-i-use-httpverbs-attributes-in-an-mvc-application"></a><span data-ttu-id="814b5-105">Инструкции: использование HttpVerbs атрибуты в MVC-приложении?</span><span class="sxs-lookup"><span data-stu-id="814b5-105">How Do I: Use HttpVerbs Attributes in an MVC Application?</span></span>
====================
<span data-ttu-id="814b5-106">по [Крис пиксел](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="814b5-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="814b5-107">В этот видеоролик пиксел Крис показывает, как использовать атрибуты HttpVerbs для управления доступом к действия MVC.</span><span class="sxs-lookup"><span data-stu-id="814b5-107">In this video Chris Pels shows how to use the HttpVerbs attributes to control access to MVC actions.</span></span> <span data-ttu-id="814b5-108">Во-первых пример приложения создается с контроллера по умолчанию и представление для редактирования сведений.</span><span class="sxs-lookup"><span data-stu-id="814b5-108">First, a sample application is created with a default controller and view for editing the information.</span></span> <span data-ttu-id="814b5-109">Далее второе действие индекс будет добавлен контроллер, который имеет атрибут HttpPost, который ограничивает его вызывается только в том случае, если используется HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="814b5-109">Next, a second Index action is added to the controller which has an HttpPost attribute which restricts it to being called only when an HTTP POST is used.</span></span> <span data-ttu-id="814b5-110">В дальнейших действий атрибут AcceptVerbs() реализуется как альтернативный синтаксис для Visual Studio 2008.</span><span class="sxs-lookup"><span data-stu-id="814b5-110">As a follow-up, the AcceptVerbs() attribute is implemented as an alternative syntax for Visual Studio 2008.</span></span> <span data-ttu-id="814b5-111">Затем рассматривается использование HttpVerbs для предотвращения угрозу безопасности, связанные с использованием HTTP GET для выполнения инструкции delete по ссылке.</span><span class="sxs-lookup"><span data-stu-id="814b5-111">A use of the HttpVerbs for preventing the security risk associated with using an HTTP GET to perform a delete from a link is then discussed.</span></span>

[<span data-ttu-id="814b5-112">&#9654;Посмотрите видео (16 минут)</span><span class="sxs-lookup"><span data-stu-id="814b5-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-httpverbs-attributes-in-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="814b5-113">[Назад](how-do-i-work-with-model-binders-in-an-mvc-application.md)
> [Вперед](mvc2-html-encoding.md)</span><span class="sxs-lookup"><span data-stu-id="814b5-113">[Previous](how-do-i-work-with-model-binders-in-an-mvc-application.md)
[Next](mvc2-html-encoding.md)</span></span>
