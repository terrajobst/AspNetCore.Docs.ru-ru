---
uid: web-forms/videos/how-do-i/how-do-i-implement-error-handling-when-sending-email-with-aspnet
title: '[Инструкции] Реализация обработки ошибок при отправке сообщения электронной почты с помощью ASP.NET | Документация Майкрософт'
author: rick-anderson
description: Крис Пелз показано, как реализовать обработку ошибок при отправке сообщения электронной почты с помощью ASP.NET. Он создает веб-страницу ASP.NET для отправки электронной почты, показано, как настроить & lt....
ms.author: aspnetcontent
ms.date: 11/06/2008
ms.assetid: c02ffd50-aa19-4cdc-b1bf-760989979a61
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-implement-error-handling-when-sending-email-with-aspnet
msc.type: video
ms.openlocfilehash: ebe6f451cad2ba4a9311b5882337b637de7f381e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814731"
---
<a name="how-do-i-implement-error-handling-when-sending-email-with-aspnet"></a><span data-ttu-id="3725c-104">[Инструкции] Реализация обработки ошибок при отправке сообщения электронной почты с помощью ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3725c-104">[How Do I:] Implement Error Handling when Sending Email with ASP.NET</span></span>
====================
<span data-ttu-id="3725c-105">по [Крис Пелз](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="3725c-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="3725c-106">Крис Пелз показано, как реализовать обработку ошибок при отправке сообщения электронной почты с помощью ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3725c-106">Chris Pels shows how to implement error handling when sending an email with ASP.NET.</span></span> <span data-ttu-id="3725c-107">Он создает веб-страницу ASP.NET для отправки электронной почты, показано, как настроить &lt;mailSettings&gt; в файле web.config, описывает класс System.Net.Mail и как он используется для создания и отправки сообщений электронной почты.</span><span class="sxs-lookup"><span data-stu-id="3725c-107">He creates an ASP.NET web page to send email, shows how to configure &lt;mailSettings&gt; in the web.config file, describes the System.Net.Mail class and how it's used to create and send email messages.</span></span> <span data-ttu-id="3725c-108">Затем он добавляет обработки ошибок, используя классы System.Net.Mail исключений, которые предоставляют сведения об ошибках, которые могут возникнуть при отправке сообщения электронной почты и рассматривает SmtpStatusCode перечисления, который содержит список возможных значений при отправке сообщения электронной почты с SmtpClient.</span><span class="sxs-lookup"><span data-stu-id="3725c-108">He then adds error handling using System.Net.Mail exception classes, which provide information about errors that can occur when sending email, and reviews the SmtpStatusCode enumeration, which provides a list of possible outcomes when sending an email with the SmtpClient.</span></span> <span data-ttu-id="3725c-109">Наконец он отправляет тестовое электронное сообщение, которое приводит к появлению исключения и просматривает сведения в отладчике Visual Studio обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="3725c-109">Finally, he sends a test email that raises an exception and reviews the error handling information in the Visual Studio debugger.</span></span>

[<span data-ttu-id="3725c-110">&#9654;Просмотрите видео (24 мин.)</span><span class="sxs-lookup"><span data-stu-id="3725c-110">&#9654; Watch video (24 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-error-handling-when-sending-email-with-aspnet)
