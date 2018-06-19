---
uid: web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
title: '[Инструкции:] Реализации постоянного шаблона обмена данными с помощью веб-службы? | Документы Майкрософт'
author: JoeStagner
description: В традиционной веб-сайта браузером и сервером не поддерживать непрерывную связь, но обмениваться данными только в ответ на пользователя, выполняющего акт...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/22/2007
ms.topic: article
ms.assetid: 424c06cd-6d61-43cd-a1f2-d1a6b62e47b1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
msc.type: video
ms.openlocfilehash: 7aa696443beb1f084a5b3f5a91e761bef1fdc034
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30883187"
---
<a name="how-do-i-implement-the-persistent-communications-pattern-using-web-services"></a><span data-ttu-id="017c9-104">[Инструкции:] Реализации постоянного шаблона обмена данными с помощью веб-службы?</span><span class="sxs-lookup"><span data-stu-id="017c9-104">[How Do I:] Implement the Persistent Communications Pattern using Web Services?</span></span>
====================
<span data-ttu-id="017c9-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="017c9-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="017c9-106">В традиционной веб-сайта браузером и сервером не поддерживать непрерывную связь, но обмениваться данными только в ответ на пользователя, выполняющего действие.</span><span class="sxs-lookup"><span data-stu-id="017c9-106">In a traditional Web site the browser and the server do not maintain an ongoing communication, but communicate only in response to the user performing an action.</span></span> <span data-ttu-id="017c9-107">В современных веб-сайта, когда страница становится контейнер приложения может быть полезным для браузером и сервером для поддержания текущих подключений, чтобы страницы могут обновляться без пользователя, выполняющего действие.</span><span class="sxs-lookup"><span data-stu-id="017c9-107">In a modern Web site where the page becomes an application container, it can be advantageous for the browser and the server to maintain an ongoing communication so that page updates can occur without the user performing an action.</span></span> <span data-ttu-id="017c9-108">Это известно как шаблон постоянного взаимодействия для AJAX.</span><span class="sxs-lookup"><span data-stu-id="017c9-108">This is known as the Persistent Communications Pattern for AJAX.</span></span> <span data-ttu-id="017c9-109">ASP.NET AJAX предоставляет два основных способа для веб-разработчиков для реализации постоянного шаблона обмена данными.</span><span class="sxs-lookup"><span data-stu-id="017c9-109">ASP.NET AJAX provides two main ways for Web developers to implement the Persistent Communications Pattern.</span></span> <span data-ttu-id="017c9-110">В более ранних видео мы узнали, как для использования в качестве основы для реализации ASP.NET AJAX UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="017c9-110">In an earlier video we saw how to use the ASP.NET AJAX UpdatePanel as the basis of the implementation.</span></span> <span data-ttu-id="017c9-111">В этом видеоролике рассказано, как реализовать тот же шаблон, с помощью вызова JavaScrpt веб-службы, который избавляет от необходимости ASP.NET AJAX UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="017c9-111">In this video we learn how to implement the same pattern using a JavaScrpt call to a Web service, which removes the need for an ASP.NET AJAX UpdatePanel.</span></span>

[<span data-ttu-id="017c9-112">&#9654;Посмотрите видео (16 минут)</span><span class="sxs-lookup"><span data-stu-id="017c9-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-the-persistent-communications-pattern-using-web-services)

> [!div class="step-by-step"]
> <span data-ttu-id="017c9-113">[Назад](how-do-i-localize-an-aspnet-ajax-application.md)
> [Вперед](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span><span class="sxs-lookup"><span data-stu-id="017c9-113">[Previous](how-do-i-localize-an-aspnet-ajax-application.md)
[Next](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span></span>
