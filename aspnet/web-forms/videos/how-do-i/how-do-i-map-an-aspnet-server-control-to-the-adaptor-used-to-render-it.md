---
uid: web-forms/videos/how-do-i/how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it
title: '[Инструкции:] Сопоставить адаптер, используемый для его отображения элемента управления сервера ASP.NET | Документы Microsoft'
author: rick-anderson
description: В этой видео пиксел Крис будет показано, как использовать адаптер элемента управления для предоставления различных готовых для просмотра серверного элемента управления ASP.NET без изменения c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/19/2008
ms.topic: article
ms.assetid: d4b498ef-8e1c-4fa2-9c35-1f32f20bb9b7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it
msc.type: video
ms.openlocfilehash: 675874da54fac688fa4df70ea1efaba477d30139
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26526063"
---
<a name="how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it"></a><span data-ttu-id="d6dd2-103">[Инструкции:] Сопоставить адаптер, используемый для его отображения элемента управления сервера ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d6dd2-103">[How Do I:] Map an ASP.NET Server Control to the Adaptor Used to Render It</span></span>
====================
<span data-ttu-id="d6dd2-104">по [Крис пиксел](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="d6dd2-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="d6dd2-105">В этой видео пиксел Крис будет показано, как использовать адаптер элемента управления для предоставления различных готовых для просмотра серверного элемента управления ASP.NET без изменения самого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="d6dd2-105">In this video Chris Pels will show how to use a control adaptor to provide different renderings for an ASP.NET server control without actually changing the control itself.</span></span> <span data-ttu-id="d6dd2-106">В этом видео будет адаптирован для отображения каждого элемента списка по горизонтали с помощью элементов DIV вместо традиционных элементов UL элемента управления ASP.NET BulletList.</span><span class="sxs-lookup"><span data-stu-id="d6dd2-106">In this video, an ASP.NET BulletList control will be adapted to display each list item horizontally using DIV elements instead of the traditional UL elements.</span></span> <span data-ttu-id="d6dd2-107">Сначала посмотрите, как создать класс, наследующий WebControlAdaptor и затем реализует код для визуализации в новый формат списка.</span><span class="sxs-lookup"><span data-stu-id="d6dd2-107">First, see how to create a class that inherits WebControlAdaptor and then implements the code to render the new list format.</span></span> <span data-ttu-id="d6dd2-108">Далее ознакомьтесь со способом новый адаптер элемента управления для элемента управления сервера ASP.NET в файле определения типа BROWSER.</span><span class="sxs-lookup"><span data-stu-id="d6dd2-108">Next, learn how to map the new control adaptor to the ASP.NET server control in the .browser definition file.</span></span> <span data-ttu-id="d6dd2-109">Затем показано, как для использования нового адаптера управления на страницах веб-узла.</span><span class="sxs-lookup"><span data-stu-id="d6dd2-109">Then see how to use the new control adaptor on pages in a web site.</span></span> <span data-ttu-id="d6dd2-110">Наконец Узнайте, как адаптер управления могут быть связаны с все браузеры или определенные типы браузеров.</span><span class="sxs-lookup"><span data-stu-id="d6dd2-110">Finally, learn how a control adaptor can be associated with either all browsers or specific types of browsers.</span></span>

[<span data-ttu-id="d6dd2-111">&#9654; Посмотрите видео (в минутах 23)</span><span class="sxs-lookup"><span data-stu-id="d6dd2-111">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it)
