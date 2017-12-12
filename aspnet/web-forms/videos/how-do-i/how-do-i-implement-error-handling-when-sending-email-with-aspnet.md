---
uid: web-forms/videos/how-do-i/how-do-i-implement-error-handling-when-sending-email-with-aspnet
title: "[Инструкции:] Реализация обработки ошибок, при отправке сообщения электронной почты с ASP.NET | Документы Microsoft"
author: rick-anderson
description: "Крис пиксел показано, как реализовать обработку ошибок при отправке писем с ASP.NET. Он создает веб-страницу ASP.NET для отправки электронной почты, показано, как настроить & lt...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/06/2008
ms.topic: article
ms.assetid: c02ffd50-aa19-4cdc-b1bf-760989979a61
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-implement-error-handling-when-sending-email-with-aspnet
msc.type: video
ms.openlocfilehash: a860eb958956bcac1682e8be7d4e70a9d44b2c4d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-implement-error-handling-when-sending-email-with-aspnet"></a><span data-ttu-id="6d166-104">[Инструкции:] Реализация обработки ошибок, при отправке сообщения электронной почты с ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6d166-104">[How Do I:] Implement Error Handling when Sending Email with ASP.NET</span></span>
====================
<span data-ttu-id="6d166-105">по [Крис пиксел](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="6d166-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="6d166-106">Крис пиксел показано, как реализовать обработку ошибок при отправке писем с ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6d166-106">Chris Pels shows how to implement error handling when sending an email with ASP.NET.</span></span> <span data-ttu-id="6d166-107">Он создает веб-страницу ASP.NET для отправки электронной почты, показано, как настроить &lt;mailSettings&gt; в файле web.config описывает данный класс System.Net.Mail и как она используется для создания и отправки сообщений электронной почты.</span><span class="sxs-lookup"><span data-stu-id="6d166-107">He creates an ASP.NET web page to send email, shows how to configure &lt;mailSettings&gt; in the web.config file, describes the System.Net.Mail class and how it's used to create and send email messages.</span></span> <span data-ttu-id="6d166-108">Затем он добавляет обработки ошибок с помощью System.Net.Mail исключения классов, которые предоставляют сведения об ошибках, которые могут возникнуть при отправке сообщения электронной почты и рассматривает SmtpStatusCode перечисления, который содержит список возможных значений при отправке писем с SmtpClient.</span><span class="sxs-lookup"><span data-stu-id="6d166-108">He then adds error handling using System.Net.Mail exception classes, which provide information about errors that can occur when sending email, and reviews the SmtpStatusCode enumeration, which provides a list of possible outcomes when sending an email with the SmtpClient.</span></span> <span data-ttu-id="6d166-109">Наконец он отправляет тестовое сообщение электронной почты, который вызывает исключение и просматривает данные в отладчике Visual Studio обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="6d166-109">Finally, he sends a test email that raises an exception and reviews the error handling information in the Visual Studio debugger.</span></span>

[<span data-ttu-id="6d166-110">&#9654; Посмотрите видео (24 в минутах)</span><span class="sxs-lookup"><span data-stu-id="6d166-110">&#9654; Watch video (24 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-error-handling-when-sending-email-with-aspnet)
