---
uid: web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
title: '[Инструкции:] С помощью условного UpdateMode UpdatePanel? | Документы Майкрософт'
author: JoeStagner
description: ASP.NET AJAX UpdatePanel включает UpdateMode свойство, которое может быть задано значение «Всегда» или «Условный». Значение по умолчанию — всегда, в этом случае UpdatePan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/01/2007
ms.topic: article
ms.assetid: 10b5bad3-4c18-464f-9454-0b3e60b7b8be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
msc.type: video
ms.openlocfilehash: 45416f101e6e85060f68c594192cef35503bcfed
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30884786"
---
<a name="how-do-i-use-the-conditional-updatemode-of-the-updatepanel"></a><span data-ttu-id="03e34-105">[Инструкции:] С помощью условного UpdateMode UpdatePanel?</span><span class="sxs-lookup"><span data-stu-id="03e34-105">[How Do I:] Use the Conditional UpdateMode of the UpdatePanel?</span></span>
====================
<span data-ttu-id="03e34-106">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="03e34-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="03e34-107">ASP.NET AJAX UpdatePanel включает UpdateMode свойство, которое может быть задано значение «Всегда» или «Условный».</span><span class="sxs-lookup"><span data-stu-id="03e34-107">The ASP.NET AJAX UpdatePanel includes an UpdateMode property that may be set to 'Always' or 'Conditional'.</span></span> <span data-ttu-id="03e34-108">Значение по умолчанию — всегда, в этом случае UpdatePanel всегда будет обновлять его содержимое во время асинхронной обратной передачи.</span><span class="sxs-lookup"><span data-stu-id="03e34-108">The default is Always, in which case the UpdatePanel will always update its content during an asychronous postback.</span></span> <span data-ttu-id="03e34-109">В этом видео мы узнаем, как можно задать для UpdateMode значение Conditional, в котором регистр UpdatePanel только обновит его содержимое при нашей серверный код вызывает метод обновления.</span><span class="sxs-lookup"><span data-stu-id="03e34-109">In this video we learn how we can set the UpdateMode to Conditional, in which case the UpdatePanel will only update its content when our server-side code calls its Update method.</span></span> <span data-ttu-id="03e34-110">Это позволяет использовать условную логику в ваш код C# или Visual Basic, чтобы определить, будет ли UpdatePanel обновления его содержимого во время текущей асинхронной обратной передачи.</span><span class="sxs-lookup"><span data-stu-id="03e34-110">This allows you to use conditional logic in your C# or Visual Basic code to determine whether the UpdatePanel will update its content during the current asynchronous postback.</span></span>

[<span data-ttu-id="03e34-111">&#9654;Посмотрите видео (13 мин.)</span><span class="sxs-lookup"><span data-stu-id="03e34-111">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-conditional-updatemode-of-the-updatepanel)

> [!div class="step-by-step"]
> <span data-ttu-id="03e34-112">[Назад](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
> [Вперед](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span><span class="sxs-lookup"><span data-stu-id="03e34-112">[Previous](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
[Next](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span></span>
