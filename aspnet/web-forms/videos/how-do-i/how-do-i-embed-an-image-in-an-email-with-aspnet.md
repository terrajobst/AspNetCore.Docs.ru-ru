---
uid: web-forms/videos/how-do-i/how-do-i-embed-an-image-in-an-email-with-aspnet
title: "[Инструкции:] Внедрить изображение в сообщение электронной почты с ASP.NET | Документы Microsoft"
author: rick-anderson
description: "Крис пиксел показано, как внедрить изображение в сообщение электронной почты с ASP.NET. Он создает веб-форму (с полями для для, из темы и текст), использует AlternateView..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/06/2008
ms.topic: article
ms.assetid: 424788ac-0a43-4063-99e7-db5aa4c66a9d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-embed-an-image-in-an-email-with-aspnet
msc.type: video
ms.openlocfilehash: 04c6bc9f49bd57dc14a3ab09e3c56028d8166158
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-embed-an-image-in-an-email-with-aspnet"></a><span data-ttu-id="fc95e-104">[Инструкции:] Внедрить изображение в сообщение электронной почты с ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fc95e-104">[How Do I:] Embed an Image in an Email with ASP.NET</span></span>
====================
<span data-ttu-id="fc95e-105">по [Крис пиксел](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="fc95e-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="fc95e-106">Крис пиксел показано, как внедрить изображение в сообщение электронной почты с ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fc95e-106">Chris Pels shows how to embed an image in an email with ASP.NET.</span></span> <span data-ttu-id="fc95e-107">Он создает веб-форму (с полями для для, из темы и текст), используется класс AlternateView для создания текста и HTML-версий электронной почты, изображение хранится в экземпляре класса LinkedResource, включает его в HTML AlternateView.</span><span class="sxs-lookup"><span data-stu-id="fc95e-107">He creates a web form (with fields for To, From, Subject, and Body), uses the AlternateView class to create text and HTML versions of an email, stores an image in a LinkedResource class instance, embeds it in the HTML AlternateView.</span></span> <span data-ttu-id="fc95e-108">Он добавляет в объект MailMessage обе версии и отправляет сообщение электронной почты дважды: сначала включены возможности получения HTML и затем как текст только для чтения.</span><span class="sxs-lookup"><span data-stu-id="fc95e-108">He then adds both versions to the MailMessage object and sends the email twice, first with HTML-receiving capabilities enabled and then as text-only.</span></span> <span data-ttu-id="fc95e-109">Отображаются как сообщения электронной почты в Outlook.</span><span class="sxs-lookup"><span data-stu-id="fc95e-109">Both email messages appear in Outlook.</span></span> <span data-ttu-id="fc95e-110">В формате HTML показано внедренного изображения. Текстовая версия не отвечает.</span><span class="sxs-lookup"><span data-stu-id="fc95e-110">The HTML version shows the embedded image; the text version does not.</span></span>

[<span data-ttu-id="fc95e-111">&#9654; Посмотрите видео (19 минут)</span><span class="sxs-lookup"><span data-stu-id="fc95e-111">&#9654; Watch video (19 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-embed-an-image-in-an-email-with-aspnet)
