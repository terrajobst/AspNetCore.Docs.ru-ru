---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: "[Инструкции:]  Страницы ASP.NET на основе данных в заголовке HTTP кэша | Документы Microsoft"
author: rick-anderson
description: "В этой видео пиксел Крис показано, как сохранить страницы в кэше вывода ASP.NET на основе данных в заголовке HTTP. Первый потенциальных стили HTTP..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2009
ms.topic: article
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: ce5ea10396d0fe31d72425e2431102a0cb0c3bd0
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="88429-104">[Инструкции:]  Страницы ASP.NET на основе данных в заголовке HTTP кэша</span><span class="sxs-lookup"><span data-stu-id="88429-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>
====================
<span data-ttu-id="88429-105">по [Крис пиксел](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="88429-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="88429-106">В этой видео пиксел Крис показано, как сохранить страницы в кэше вывода ASP.NET на основе данных в заголовке HTTP.</span><span class="sxs-lookup"><span data-stu-id="88429-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="88429-107">Во-первых рассматриваются возможные значения заголовка HTTP.</span><span class="sxs-lookup"><span data-stu-id="88429-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="88429-108">Затем пример страницы создается и используется директива OutputCache атрибутом VaryByHeader, который содержит значение «принять язык», заголовок HTTP для управления кэшированием в зависимости от языка браузера пользователя.</span><span class="sxs-lookup"><span data-stu-id="88429-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="88429-109">На странице примеров просматривается в IE, для которого установлен английский (США), а затем в FireFox, настроенная на использование французского языка.</span><span class="sxs-lookup"><span data-stu-id="88429-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="88429-110">Наконец рассматривается возможность переместите определение кэша CacheProfile в файле web.config.</span><span class="sxs-lookup"><span data-stu-id="88429-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="88429-111">&#9654; Посмотрите видео (12 минут)</span><span class="sxs-lookup"><span data-stu-id="88429-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
