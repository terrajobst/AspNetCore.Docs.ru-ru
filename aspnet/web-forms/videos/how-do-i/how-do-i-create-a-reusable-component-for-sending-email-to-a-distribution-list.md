---
uid: web-forms/videos/how-do-i/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list
title: '[Инструкции] Создание повторно используемого компонента для отправки сообщений электронной почты списка рассылки | Документация Майкрософт'
author: rick-anderson
description: В этом видео Крис Пелз показано, как создать компонент, который можно использовать на нескольких веб-страниц и веб-сайтов, отправляет сообщения электронной почты в список получателей. Этого достаточно...
ms.author: aspnetcontent
ms.date: 12/04/2008
ms.assetid: 13dd3a26-c210-432e-91fe-355c979060b3
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list
msc.type: video
ms.openlocfilehash: 71bed3d232703bbab0fd12414985a56f69af05f5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812718"
---
<a name="how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list"></a><span data-ttu-id="a6d95-104">[Инструкции] Создание повторно используемого компонента для отправки сообщений электронной почты списка рассылки</span><span class="sxs-lookup"><span data-stu-id="a6d95-104">[How Do I:] Create a Reusable Component for Sending Email to a Distribution List</span></span>
====================
<span data-ttu-id="a6d95-105">по [Крис Пелз](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="a6d95-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="a6d95-106">В этом видео Крис Пелз показано, как создать компонент, который можно использовать на нескольких веб-страниц и веб-сайтов, отправляет сообщения электронной почты в список получателей.</span><span class="sxs-lookup"><span data-stu-id="a6d95-106">In this video Chris Pels shows how to create a component that can be used on multiple web pages and web sites that sends emails to a list of recipients.</span></span> <span data-ttu-id="a6d95-107">Во-первых, веб-сайта ASP.NET будет настроен для отправки электронной почты с помощью &lt;mailSettings&gt; в файле web.config.</span><span class="sxs-lookup"><span data-stu-id="a6d95-107">First, the ASP.NET web site will be configured to send email using the &lt;mailSettings&gt; in the web.config file.</span></span> <span data-ttu-id="a6d95-108">Затем создаются класса и несколько методов для чтения список получателей из источника данных (DB, XML и др.) и отправить сообщение электронной почты всех получателей, используя классы System.Net.Mail.</span><span class="sxs-lookup"><span data-stu-id="a6d95-108">Then a class and several methods are created to read a list of recipients from a data source (DB, XML, etc.) and send an email message to each of the recipients using the System.Net.Mail classes.</span></span> <span data-ttu-id="a6d95-109">Обработка включается как часть этого процесса исключения.</span><span class="sxs-lookup"><span data-stu-id="a6d95-109">As part of this process exception handling is included.</span></span> <span data-ttu-id="a6d95-110">Кроме того, создается пользовательский интерфейс позволяет пользователю указать элементы такие как адрес отправителя, тема, добавить вложения и т. д.</span><span class="sxs-lookup"><span data-stu-id="a6d95-110">In addition, a user interface is created to allow the user to specify items such as the From address, Subject, add an attachment, etc.</span></span>

[<span data-ttu-id="a6d95-111">&#9654;Просмотрите видео (35 мин.)</span><span class="sxs-lookup"><span data-stu-id="a6d95-111">&#9654; Watch video (35 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list)
