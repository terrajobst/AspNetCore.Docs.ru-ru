---
uid: web-forms/videos/how-do-i/how-do-i-embed-an-image-in-an-email-with-aspnet
title: '[Инструкции] Внедрение изображения в сообщение электронной почты с помощью ASP.NET | Документация Майкрософт'
author: rick-anderson
description: Крис Пелз показано, как внедрить изображение в сообщение электронной почты с помощью ASP.NET. Он создает веб-форму (с полями для, из тему и текст), использует AlternateView...
ms.author: riande
ms.date: 11/06/2008
ms.assetid: 424788ac-0a43-4063-99e7-db5aa4c66a9d
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-embed-an-image-in-an-email-with-aspnet
msc.type: video
ms.openlocfilehash: d046c988c060580b856fb65c90521c3a2815e760
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827725"
---
<a name="how-do-i-embed-an-image-in-an-email-with-aspnet"></a><span data-ttu-id="64e37-104">[Инструкции] Внедрение изображения в сообщение электронной почты с помощью ASP.NET</span><span class="sxs-lookup"><span data-stu-id="64e37-104">[How Do I:] Embed an Image in an Email with ASP.NET</span></span>
====================
<span data-ttu-id="64e37-105">по [Крис Пелз](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="64e37-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="64e37-106">Крис Пелз показано, как внедрить изображение в сообщение электронной почты с помощью ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="64e37-106">Chris Pels shows how to embed an image in an email with ASP.NET.</span></span> <span data-ttu-id="64e37-107">Он создает веб-форму (с полями для, из тему и текст), для создания текста и HTML версиях сообщение электронной почты использует класса AlternateView, изображение хранится в экземпляре класса LinkedResource, он внедряется в HTML AlternateView.</span><span class="sxs-lookup"><span data-stu-id="64e37-107">He creates a web form (with fields for To, From, Subject, and Body), uses the AlternateView class to create text and HTML versions of an email, stores an image in a LinkedResource class instance, embeds it in the HTML AlternateView.</span></span> <span data-ttu-id="64e37-108">Затем он добавляет в объект MailMessage обе версии и дважды, отправляет сообщение электронной почты, сначала с HTML-получения возможностей, а затем как доступное только для текста.</span><span class="sxs-lookup"><span data-stu-id="64e37-108">He then adds both versions to the MailMessage object and sends the email twice, first with HTML-receiving capabilities enabled and then as text-only.</span></span> <span data-ttu-id="64e37-109">В Outlook отображается как сообщения электронной почты.</span><span class="sxs-lookup"><span data-stu-id="64e37-109">Both email messages appear in Outlook.</span></span> <span data-ttu-id="64e37-110">HTML-версия показывает внедренные изображения. Текстовая версия — нет.</span><span class="sxs-lookup"><span data-stu-id="64e37-110">The HTML version shows the embedded image; the text version does not.</span></span>

[<span data-ttu-id="64e37-111">&#9654;Просмотрите видео (19 минут)</span><span class="sxs-lookup"><span data-stu-id="64e37-111">&#9654; Watch video (19 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-embed-an-image-in-an-email-with-aspnet)
