---
uid: web-forms/videos/how-do-i/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list
title: "[Инструкции:] Создать повторно используемый компонент для отправки электронной почты в список рассылки | Документы Microsoft"
author: rick-anderson
description: "В этой видео пиксел Крис показано, как создать компонент, который может использоваться на нескольких веб-страниц и веб-сайтов, отправляет сообщения электронной почты в список получателей. Первы..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/04/2008
ms.topic: article
ms.assetid: 13dd3a26-c210-432e-91fe-355c979060b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list
msc.type: video
ms.openlocfilehash: 06a2dd7bbcd3087d8d2566f1015eccb8bc7ec8aa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list"></a><span data-ttu-id="99022-104">[Инструкции:] Создать повторно используемый компонент для отправки электронной почты в список рассылки</span><span class="sxs-lookup"><span data-stu-id="99022-104">[How Do I:] Create a Reusable Component for Sending Email to a Distribution List</span></span>
====================
<span data-ttu-id="99022-105">по [Крис пиксел](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="99022-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="99022-106">В этой видео пиксел Крис показано, как создать компонент, который может использоваться на нескольких веб-страниц и веб-сайтов, отправляет сообщения электронной почты в список получателей.</span><span class="sxs-lookup"><span data-stu-id="99022-106">In this video Chris Pels shows how to create a component that can be used on multiple web pages and web sites that sends emails to a list of recipients.</span></span> <span data-ttu-id="99022-107">Во-первых, веб-сайта ASP.NET будет настроен для отправки электронной почты с помощью &lt;mailSettings&gt; в файле web.config.</span><span class="sxs-lookup"><span data-stu-id="99022-107">First, the ASP.NET web site will be configured to send email using the &lt;mailSettings&gt; in the web.config file.</span></span> <span data-ttu-id="99022-108">Затем класс и ряда методов создаются для чтения список получателей из источника данных (DB, XML, т. д.) и отправьте сообщение электронной почты для каждого из адресов, используйте классы System.Net.Mail.</span><span class="sxs-lookup"><span data-stu-id="99022-108">Then a class and several methods are created to read a list of recipients from a data source (DB, XML, etc.) and send an email message to each of the recipients using the System.Net.Mail classes.</span></span> <span data-ttu-id="99022-109">Обработка включается как часть этого процесса исключения.</span><span class="sxs-lookup"><span data-stu-id="99022-109">As part of this process exception handling is included.</span></span> <span data-ttu-id="99022-110">Кроме того, создается пользовательский интерфейс позволяет пользователю указать элементы, такие как адрес отправителя, тема, добавить вложение и т. д.</span><span class="sxs-lookup"><span data-stu-id="99022-110">In addition, a user interface is created to allow the user to specify items such as the From address, Subject, add an attachment, etc.</span></span>

[<span data-ttu-id="99022-111">&#9654; Посмотрите видео (35 минут)</span><span class="sxs-lookup"><span data-stu-id="99022-111">&#9654; Watch video (35 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list)
