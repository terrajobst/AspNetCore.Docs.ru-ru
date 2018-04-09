---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: Практические советы. Создание вспомогательного класса пользовательского HTML для приложения MVC? | Документы Майкрософт
author: rick-anderson
description: В этом видеоролике пиксел Крис показано создание пользовательского HtmlHelper, недоступную в стандартный набор в MVC-приложениях. Первый пример MVC прило...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/11/2009
ms.topic: article
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 92faa04e1eefec0852604d51987ddaa9ee58838a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="daab1-105">Практические советы. Создание вспомогательного класса пользовательского HTML для приложения MVC?</span><span class="sxs-lookup"><span data-stu-id="daab1-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>
====================
<span data-ttu-id="daab1-106">по [Крис пиксел](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="daab1-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="daab1-107">В этом видеоролике пиксел Крис показано создание пользовательского HtmlHelper, недоступную в стандартный набор в MVC-приложениях.</span><span class="sxs-lookup"><span data-stu-id="daab1-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="daab1-108">Во-первых пример MVC-приложения создается с демонстрационной контроллер и представление для проверки пользовательских HtmlHelper.</span><span class="sxs-lookup"><span data-stu-id="daab1-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="daab1-109">Затем модуль создается с открытой функции, который является методом расширения, который представляет реализацию пользовательского HtmlHelper.</span><span class="sxs-lookup"><span data-stu-id="daab1-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="daab1-110">Пользовательская вспомогательная — для создания `<img>` теги на странице и получает несколько входных параметров, включая идентификатор, URL-адрес и замещающий текст для тега image.</span><span class="sxs-lookup"><span data-stu-id="daab1-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="daab1-111">Логика затем добавляется в функцию для возвращения выполненной `<img>` тег с указанными сведениями.</span><span class="sxs-lookup"><span data-stu-id="daab1-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="daab1-112">Затем пользовательские HtmlHelper используется для демонстрационной страницы для отображения изображения.</span><span class="sxs-lookup"><span data-stu-id="daab1-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="daab1-113">Наконец, пользовательские HtmlHelper расширяется для включения нескольких конструктор переопределений, которые обеспечивают гибкость, более легко Создание различных `<img>` тегов.</span><span class="sxs-lookup"><span data-stu-id="daab1-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="daab1-114">&#9654;Посмотрите видео (18 минут)</span><span class="sxs-lookup"><span data-stu-id="daab1-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="daab1-115">[Назад](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [Вперед](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="daab1-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
