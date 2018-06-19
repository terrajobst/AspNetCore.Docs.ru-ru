---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Инструкции:] Чтобы заменить HTML на странице ASP.NET используйте свойство Reponse.Filter | Документы Microsoft'
author: rick-anderson
description: В этот видеоролик пиксел Крис показывает, как использовать свойство Reponse.Filter перехватывать и изменять, отправляемых на страницу HTML. Во-первых пример страницы создается w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26525533"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="472bb-104">[Инструкции:] Свойство Reponse.Filter позволяет заменить HTML на странице ASP.NET</span><span class="sxs-lookup"><span data-stu-id="472bb-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="472bb-105">по [Крис пиксел](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="472bb-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="472bb-106">В этот видеоролик пиксел Крис показывает, как использовать свойство Reponse.Filter перехватывать и изменять, отправляемых на страницу HTML.</span><span class="sxs-lookup"><span data-stu-id="472bb-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="472bb-107">Во-первых пример страницы создается простой текст.</span><span class="sxs-lookup"><span data-stu-id="472bb-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="472bb-108">Затем создается пользовательский класс потока, служащий поток замены для текущего потока, отправляемых в браузер пользователя.</span><span class="sxs-lookup"><span data-stu-id="472bb-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="472bb-109">В этом классе пользовательского потока содержимого страницы извлекаются из потока, изменить, а затем записывается в поток ответа.</span><span class="sxs-lookup"><span data-stu-id="472bb-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="472bb-110">В этот настраиваемый класс потока метода записи настроено таким образом, чтобы заменить HTML в базовый поток ответа, тем самым изменение отправки в браузер пользователя.</span><span class="sxs-lookup"><span data-stu-id="472bb-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="472bb-111">Наконец, новый класс stream назначается свойство Response.Filter на странице\_событию загрузки, тем самым предоставляя механизм для изменения содержимого страницы.</span><span class="sxs-lookup"><span data-stu-id="472bb-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="472bb-112">&#9654; Посмотрите видео (13 мин.)</span><span class="sxs-lookup"><span data-stu-id="472bb-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
