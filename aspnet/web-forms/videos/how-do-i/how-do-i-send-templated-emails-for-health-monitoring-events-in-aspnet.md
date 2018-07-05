---
uid: web-forms/videos/how-do-i/how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet
title: '[Инструкции] Отправка шаблонных сообщений электронной почты для мониторинга событий в ASP.NET работоспособности | Документация Майкрософт'
author: rick-anderson
description: В этом видео Крис Пелз показано, как использовать TemplatedEmailWebEventProvider для отправки сообщений электронной почты при возникновении события мониторинга работоспособности, использовать шаблон для t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2008
ms.topic: article
ms.assetid: 5c107c6e-9fb7-4206-bd3f-221cb0767f8a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet
msc.type: video
ms.openlocfilehash: 407a9bd1e38e474286e58e94f4d98d4302a02223
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378557"
---
<a name="how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet"></a><span data-ttu-id="9296b-103">[Инструкции] Отправка шаблонных сообщений электронной почты для мониторинга событий в ASP.NET работоспособности</span><span class="sxs-lookup"><span data-stu-id="9296b-103">[How Do I:] Send Templated Emails for Health Monitoring Events in ASP.NET</span></span>
====================
<span data-ttu-id="9296b-104">по [Крис Пелз](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="9296b-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="9296b-105">В этом видео Крис Пелз показано, как использовать TemplatedEmailWebEventProvider для отправки сообщений электронной почты при возникновении события мониторинга работоспособности, которые используют шаблон для содержимое сообщения электронной почты.</span><span class="sxs-lookup"><span data-stu-id="9296b-105">In this video Chris Pels shows how to use the TemplatedEmailWebEventProvider to send emails when health monitoring events occur that utilize a template for the email content.</span></span> <span data-ttu-id="9296b-106">Во-первых, см. в разделе Настройка &lt;поставщика&gt; и &lt;правила&gt; элементы в файле web.config, чтобы реализовать использование шаблона сообщения электронной почты и связать события с помощью поставщика электронной почты шаблонного мониторинга состояния.</span><span class="sxs-lookup"><span data-stu-id="9296b-106">First, see how to configure the &lt;provider&gt; and &lt;rules&gt; elements in the web.config file to implement the use of templated email and associate a health monitoring event with the templated email provider.</span></span> <span data-ttu-id="9296b-107">После настройки шаблона поставщика см. в разделе Создание шаблона сообщения электронной почты, с помощью как стандартная страница .aspx.</span><span class="sxs-lookup"><span data-stu-id="9296b-107">Once the templated provider is configured see how to create the email template using as standard .aspx page.</span></span> <span data-ttu-id="9296b-108">Узнайте, какие сведения можно найти в классе MailEventNotificaitonInfo, который передается по TemplatedEmailWebEventProvider на ASPX-страницу шаблона.</span><span class="sxs-lookup"><span data-stu-id="9296b-108">Learn what information is available in the MailEventNotificaitonInfo class that is passed by the TemplatedEmailWebEventProvider to the template .aspx page.</span></span> <span data-ttu-id="9296b-109">См. в разделе, как его можно включать любые данные, соответствующие в содержимое сообщения электронной почты.</span><span class="sxs-lookup"><span data-stu-id="9296b-109">See how it can be used to include whatever information is appropriate in the email content.</span></span> <span data-ttu-id="9296b-110">Наконец просмотрите веб-сайту теста, который отправляет сообщения электронной почты в ответ на события мониторинга работоспособности.</span><span class="sxs-lookup"><span data-stu-id="9296b-110">Finally, view the test web site which sends emails in response to health monitoring events.</span></span> <span data-ttu-id="9296b-111">Затем просмотрите фактические электронных сообщениях, получаемых, содержащих событие контроля рабочего состояния на основе шаблона.</span><span class="sxs-lookup"><span data-stu-id="9296b-111">Then view the actual emails received that contain the health monitoring event information based upon the template.</span></span>

[<span data-ttu-id="9296b-112">&#9654;Просмотрите видео (25 минут)</span><span class="sxs-lookup"><span data-stu-id="9296b-112">&#9654; Watch video (25 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet)
