---
uid: web-forms/videos/how-do-i/how-do-i-configure-email-notification-for-health-monitoring-on-an-aspnet-web-site
title: '[Инструкции:] Настройка уведомлений по электронной почте для наблюдения за работоспособностью веб-узла ASP.NET | Документы Microsoft'
author: rick-anderson
description: В этой видео пиксел Крис показано, как настроить уведомления по электронной почте для мониторинга в веб-сайт ASP.NET состояния. Во-первых в разделе Настройка отправки e...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/11/2008
ms.topic: article
ms.assetid: 1fa884c0-582e-4dc6-abb6-a5ec70d43ffb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-configure-email-notification-for-health-monitoring-on-an-aspnet-web-site
msc.type: video
ms.openlocfilehash: 91b931bb81dd27ade536e6edefe2cec2c5f8e3a9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26521763"
---
<a name="how-do-i-configure-email-notification-for-health-monitoring-on-an-aspnet-web-site"></a><span data-ttu-id="a0555-104">[Инструкции:] Настройка уведомлений по электронной почте для мониторинга на веб-сайте ASP.NET состояния</span><span class="sxs-lookup"><span data-stu-id="a0555-104">[How Do I:] Configure Email Notification for Health Monitoring on an ASP.NET Web Site</span></span>
====================
<span data-ttu-id="a0555-105">по [Крис пиксел](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="a0555-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="a0555-106">В этой видео пиксел Крис показано, как настроить уведомления по электронной почте для мониторинга в веб-сайт ASP.NET состояния.</span><span class="sxs-lookup"><span data-stu-id="a0555-106">In this video Chris Pels shows how to configure email notification for health monitoring in an ASP.NET web site.</span></span> <span data-ttu-id="a0555-107">Во-первых, узнать, как настроить отправку электронной почты с помощью веб-узла ASP.NET &lt;emailSettings&gt; в файле web.config.</span><span class="sxs-lookup"><span data-stu-id="a0555-107">First, see how to configure the sending of email in an ASP.NET web site through the use of the &lt;emailSettings&gt; element in the web.config file.</span></span> <span data-ttu-id="a0555-108">Далее показано добавление SimpleMailWebEventProvider, которая используется для отправки сообщений электронной почты для мониторинга событий как поставщик состояния.</span><span class="sxs-lookup"><span data-stu-id="a0555-108">Next, learn how to add the SimpleMailWebEventProvider which sends emails for health monitoring events as a provider.</span></span> <span data-ttu-id="a0555-109">Затем в разделе стандартные работоспособности, наблюдение за событиями, которые могут использоваться с уведомление по электронной почте с помощью проверки конфигурации мониторинга работоспособности уровня компьютера.</span><span class="sxs-lookup"><span data-stu-id="a0555-109">Then see the standard health monitoring events that can be used with email notification by examining the machine level health monitoring configuration.</span></span> <span data-ttu-id="a0555-110">После просмотра доступных событий см. реализации правила, которое сопоставляет «Все события» для поставщика услуг электронной почты.</span><span class="sxs-lookup"><span data-stu-id="a0555-110">After reviewing the available events see a rule implemented that maps the "All Events" to the email provider.</span></span> <span data-ttu-id="a0555-111">При запуске веб-сайта несколько сообщений электронной почты отправляются затем и проверяются, когда они поступают в Outlook.</span><span class="sxs-lookup"><span data-stu-id="a0555-111">Upon starting the web site several emails are then sent and examined when they are received in Outlook.</span></span> <span data-ttu-id="a0555-112">Наконец рассматриваются некоторые основные принципы, в которых могут сопоставляться событий работоспособности, мониторинга поставщика услуг электронной почты.</span><span class="sxs-lookup"><span data-stu-id="a0555-112">Finally, some basic principles of which events might be mapped to the health monitoring email provider are discussed.</span></span>

[<span data-ttu-id="a0555-113">&#9654; Посмотрите видео (25 минут)</span><span class="sxs-lookup"><span data-stu-id="a0555-113">&#9654; Watch video (25 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-configure-email-notification-for-health-monitoring-on-an-aspnet-web-site)
