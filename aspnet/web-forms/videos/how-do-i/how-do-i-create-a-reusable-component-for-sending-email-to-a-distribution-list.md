---
uid: web-forms/videos/how-do-i/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list
title: '[Инструкции] Создание повторно используемого компонента для отправки сообщений электронной почты списка рассылки | Документация Майкрософт'
author: rick-anderson
description: В этом видео Крис Пелз показано, как создать компонент, который можно использовать на нескольких веб-страниц и веб-сайтов, отправляет сообщения электронной почты в список получателей. Этого достаточно...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/04/2008
ms.topic: article
ms.assetid: 13dd3a26-c210-432e-91fe-355c979060b3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list
msc.type: video
ms.openlocfilehash: b67ab5a7ced1eb731eb71b98956e53ae2e067e1b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384255"
---
<a name="how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list"></a><span data-ttu-id="aafeb-104">[Инструкции] Создание повторно используемого компонента для отправки сообщений электронной почты списка рассылки</span><span class="sxs-lookup"><span data-stu-id="aafeb-104">[How Do I:] Create a Reusable Component for Sending Email to a Distribution List</span></span>
====================
<span data-ttu-id="aafeb-105">по [Крис Пелз](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="aafeb-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="aafeb-106">В этом видео Крис Пелз показано, как создать компонент, который можно использовать на нескольких веб-страниц и веб-сайтов, отправляет сообщения электронной почты в список получателей.</span><span class="sxs-lookup"><span data-stu-id="aafeb-106">In this video Chris Pels shows how to create a component that can be used on multiple web pages and web sites that sends emails to a list of recipients.</span></span> <span data-ttu-id="aafeb-107">Во-первых, веб-сайта ASP.NET будет настроен для отправки электронной почты с помощью &lt;mailSettings&gt; в файле web.config.</span><span class="sxs-lookup"><span data-stu-id="aafeb-107">First, the ASP.NET web site will be configured to send email using the &lt;mailSettings&gt; in the web.config file.</span></span> <span data-ttu-id="aafeb-108">Затем создаются класса и несколько методов для чтения список получателей из источника данных (DB, XML и др.) и отправить сообщение электронной почты всех получателей, используя классы System.Net.Mail.</span><span class="sxs-lookup"><span data-stu-id="aafeb-108">Then a class and several methods are created to read a list of recipients from a data source (DB, XML, etc.) and send an email message to each of the recipients using the System.Net.Mail classes.</span></span> <span data-ttu-id="aafeb-109">Обработка включается как часть этого процесса исключения.</span><span class="sxs-lookup"><span data-stu-id="aafeb-109">As part of this process exception handling is included.</span></span> <span data-ttu-id="aafeb-110">Кроме того, создается пользовательский интерфейс позволяет пользователю указать элементы такие как адрес отправителя, тема, добавить вложения и т. д.</span><span class="sxs-lookup"><span data-stu-id="aafeb-110">In addition, a user interface is created to allow the user to specify items such as the From address, Subject, add an attachment, etc.</span></span>

[<span data-ttu-id="aafeb-111">&#9654;Просмотрите видео (35 мин.)</span><span class="sxs-lookup"><span data-stu-id="aafeb-111">&#9654; Watch video (35 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list)
