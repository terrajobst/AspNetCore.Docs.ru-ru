---
uid: web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
title: '[Инструкции] Реализация шаблона постоянного обмена данными с помощью веб-служб? | Документы Майкрософт'
author: JoeStagner
description: В традиционных веб-сайта браузером и сервером не поддерживают текущих подключений, но обмениваться данными только в ответ на пользователя, выполняющего акт...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/22/2007
ms.topic: article
ms.assetid: 424c06cd-6d61-43cd-a1f2-d1a6b62e47b1
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
msc.type: video
ms.openlocfilehash: 8d7aac37b3b5b47f0533454f2d1d6f1f8677af99
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367478"
---
<a name="how-do-i-implement-the-persistent-communications-pattern-using-web-services"></a><span data-ttu-id="790a5-104">[Инструкции] Реализация шаблона постоянного обмена данными с помощью веб-служб?</span><span class="sxs-lookup"><span data-stu-id="790a5-104">[How Do I:] Implement the Persistent Communications Pattern using Web Services?</span></span>
====================
<span data-ttu-id="790a5-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="790a5-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="790a5-106">В традиционных веб-сайта браузером и сервером не поддерживают текущих подключений, но обмениваться данными только в ответ на пользователя, выполняющего действие.</span><span class="sxs-lookup"><span data-stu-id="790a5-106">In a traditional Web site the browser and the server do not maintain an ongoing communication, but communicate only in response to the user performing an action.</span></span> <span data-ttu-id="790a5-107">Для современных веб-сайта, где страницы становится контейнер приложения может быть полезным для браузером и сервером для поддержания текущего обмена данными, чтобы обновления страниц могут происходить даже без пользователя, выполняющего действие.</span><span class="sxs-lookup"><span data-stu-id="790a5-107">In a modern Web site where the page becomes an application container, it can be advantageous for the browser and the server to maintain an ongoing communication so that page updates can occur without the user performing an action.</span></span> <span data-ttu-id="790a5-108">Это называется шаблона постоянного обмена данными для AJAX.</span><span class="sxs-lookup"><span data-stu-id="790a5-108">This is known as the Persistent Communications Pattern for AJAX.</span></span> <span data-ttu-id="790a5-109">AJAX для ASP.NET предоставляет два основных способа для веб-разработчиков для реализации шаблона постоянного обмена данными.</span><span class="sxs-lookup"><span data-stu-id="790a5-109">ASP.NET AJAX provides two main ways for Web developers to implement the Persistent Communications Pattern.</span></span> <span data-ttu-id="790a5-110">В предыдущих видео мы увидели, как использовать UpdatePanel в AJAX ASP.NET в качестве основы для реализации.</span><span class="sxs-lookup"><span data-stu-id="790a5-110">In an earlier video we saw how to use the ASP.NET AJAX UpdatePanel as the basis of the implementation.</span></span> <span data-ttu-id="790a5-111">В этом видео мы узнаем, как реализовать тот же шаблон, используя JavaScrpt вызов веб-службы, что устраняет потребность в ASP.NET AJAX UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="790a5-111">In this video we learn how to implement the same pattern using a JavaScrpt call to a Web service, which removes the need for an ASP.NET AJAX UpdatePanel.</span></span>

[<span data-ttu-id="790a5-112">&#9654;Просмотрите видео (16 минут)</span><span class="sxs-lookup"><span data-stu-id="790a5-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-the-persistent-communications-pattern-using-web-services)

> [!div class="step-by-step"]
> <span data-ttu-id="790a5-113">[Назад](how-do-i-localize-an-aspnet-ajax-application.md)
> [Вперед](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span><span class="sxs-lookup"><span data-stu-id="790a5-113">[Previous](how-do-i-localize-an-aspnet-ajax-application.md)
[Next](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span></span>
