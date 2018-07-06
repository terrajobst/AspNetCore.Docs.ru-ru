---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[Инструкции]  Страницы ASP.NET на основе сведений в заголовке HTTP кэша | Документация Майкрософт'
author: rick-anderson
description: В этом видео Крис Пелз показано, как сохранить страницы в кэше вывода ASP.NET на основе сведений в заголовке HTTP. Во-первых, потенциальные верхни HTTP...
ms.author: aspnetcontent
ms.date: 02/26/2009
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: 64c5c1d82376b1a3ef7c4423c3b3a372ce5ab238
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821461"
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="22219-104">[Инструкции]  Кэш страницы ASP.NET на основе сведений в заголовке HTTP</span><span class="sxs-lookup"><span data-stu-id="22219-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>
====================
<span data-ttu-id="22219-105">по [Крис Пелз](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="22219-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="22219-106">В этом видео Крис Пелз показано, как сохранить страницы в кэше вывода ASP.NET на основе сведений в заголовке HTTP.</span><span class="sxs-lookup"><span data-stu-id="22219-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="22219-107">Во-первых рассмотрены потенциальные значения заголовка HTTP.</span><span class="sxs-lookup"><span data-stu-id="22219-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="22219-108">Затем пример страницы создается и используется директива OutputCache VaryByHeader атрибут, который содержит значение «принять язык», заголовок HTTP для управления кэшированием в зависимости от языка браузера пользователя.</span><span class="sxs-lookup"><span data-stu-id="22219-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="22219-109">На странице примеров просмотре в Internet Explorer, настроенная на английском языке, а затем в FireFox, настроенная на использование французского языка.</span><span class="sxs-lookup"><span data-stu-id="22219-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="22219-110">Наконец рассматривается возможность переместите определение кэша CacheProfile в файле web.config.</span><span class="sxs-lookup"><span data-stu-id="22219-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="22219-111">&#9654;Просмотрите видео (12 минут)</span><span class="sxs-lookup"><span data-stu-id="22219-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
