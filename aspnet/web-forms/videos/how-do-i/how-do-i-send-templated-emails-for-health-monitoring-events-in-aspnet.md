---
uid: web-forms/videos/how-do-i/how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet
title: "[Инструкции:] Отправка событий в ASP.NET мониторинга работоспособности шаблон сообщения электронной почты | Документы Microsoft"
author: rick-anderson
description: "В этом видеоролике пиксел Крис демонстрируется использование TemplatedEmailWebEventProvider для отправки сообщений электронной почты при возникновении событий мониторинга работоспособности, используйте шаблон для t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2008
ms.topic: article
ms.assetid: 5c107c6e-9fb7-4206-bd3f-221cb0767f8a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet
msc.type: video
ms.openlocfilehash: 80852c52c453bf353173dbdff79de185a9b7420b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet"></a><span data-ttu-id="053c6-103">[Инструкции:] Отправка событий в ASP.NET мониторинга работоспособности шаблон сообщения электронной почты</span><span class="sxs-lookup"><span data-stu-id="053c6-103">[How Do I:] Send Templated Emails for Health Monitoring Events in ASP.NET</span></span>
====================
<span data-ttu-id="053c6-104">по [Крис пиксел](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="053c6-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="053c6-105">В этом видеоролике пиксел Крис показано, как использовать TemplatedEmailWebEventProvider для отправки сообщений электронной почты при возникновении событий мониторинга работоспособности, использующие шаблон содержимого электронной почты.</span><span class="sxs-lookup"><span data-stu-id="053c6-105">In this video Chris Pels shows how to use the TemplatedEmailWebEventProvider to send emails when health monitoring events occur that utilize a template for the email content.</span></span> <span data-ttu-id="053c6-106">Во-первых, в разделе Настройка &lt;поставщика&gt; и &lt;правила&gt; элементы в файле web.config для реализации используйте шаблон электронной почты и связать событие с поставщиком шаблон электронной почты мониторинга работоспособности.</span><span class="sxs-lookup"><span data-stu-id="053c6-106">First, see how to configure the &lt;provider&gt; and &lt;rules&gt; elements in the web.config file to implement the use of templated email and associate a health monitoring event with the templated email provider.</span></span> <span data-ttu-id="053c6-107">После настройки шаблона поставщика разделе создайте шаблон электронной почты, используя в стандартную страницу ASPX.</span><span class="sxs-lookup"><span data-stu-id="053c6-107">Once the templated provider is configured see how to create the email template using as standard .aspx page.</span></span> <span data-ttu-id="053c6-108">Узнайте, какие сведения можно найти в классе MailEventNotificaitonInfo, который передается по TemplatedEmailWebEventProvider ASPX-страницу шаблона.</span><span class="sxs-lookup"><span data-stu-id="053c6-108">Learn what information is available in the MailEventNotificaitonInfo class that is passed by the TemplatedEmailWebEventProvider to the template .aspx page.</span></span> <span data-ttu-id="053c6-109">В разделе как он может использоваться для включения сведения, соответствующие содержимое электронной почты.</span><span class="sxs-lookup"><span data-stu-id="053c6-109">See how it can be used to include whatever information is appropriate in the email content.</span></span> <span data-ttu-id="053c6-110">Наконец просмотрите тестирования веб-сайта, которая используется для отправки сообщений электронной почты в ответ на события мониторинга работоспособности.</span><span class="sxs-lookup"><span data-stu-id="053c6-110">Finally, view the test web site which sends emails in response to health monitoring events.</span></span> <span data-ttu-id="053c6-111">Затем просмотрите фактические сообщения электронной почты получил, содержащие сведения о событии, основанный на шаблоне наблюдения за работоспособностью.</span><span class="sxs-lookup"><span data-stu-id="053c6-111">Then view the actual emails received that contain the health monitoring event information based upon the template.</span></span>

[<span data-ttu-id="053c6-112">&#9654; Посмотрите видео (25 минут)</span><span class="sxs-lookup"><span data-stu-id="053c6-112">&#9654; Watch video (25 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet)
