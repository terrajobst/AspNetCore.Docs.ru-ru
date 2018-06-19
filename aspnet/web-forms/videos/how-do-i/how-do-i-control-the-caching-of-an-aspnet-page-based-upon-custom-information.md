---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Инструкции:] Кэширование страницы ASP.NET на основе данных пользовательского элемента управления | Документы Microsoft'
author: rick-anderson
description: В этой видео пиксел Крис показывает способ управления критерии для кэширования страницы ASP.NET на основе пользовательских данных. Пример страницы создается и затем O...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: b785de4e1161ae82ee458148aee4b30820801147
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528133"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="0b52e-104">[Инструкции:] Кэширование страницы ASP.NET на основе данных пользовательского элемента управления</span><span class="sxs-lookup"><span data-stu-id="0b52e-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="0b52e-105">по [Крис пиксел](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="0b52e-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="0b52e-106">В этой видео пиксел Крис показывает способ управления критерии для кэширования страницы ASP.NET на основе пользовательских данных.</span><span class="sxs-lookup"><span data-stu-id="0b52e-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="0b52e-107">Пример страницы создается и используется директива OutputCache атрибутом VaryByCustom, который содержит пользовательское значение.</span><span class="sxs-lookup"><span data-stu-id="0b52e-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="0b52e-108">Затем метод GetVaryCustomByString() переопределяется в модуль global.asax, который обеспечивает обработку настраиваемого атрибута.</span><span class="sxs-lookup"><span data-stu-id="0b52e-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="0b52e-109">В этом методе возвращается строка, однозначно идентифицирующий кэшированная версия страницы.</span><span class="sxs-lookup"><span data-stu-id="0b52e-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="0b52e-110">Наконец есть обсуждение как кэширование, используя пользовательское значение может использоваться несколькими способами для веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="0b52e-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="0b52e-111">&#9654; Посмотрите видео (12 минут)</span><span class="sxs-lookup"><span data-stu-id="0b52e-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
