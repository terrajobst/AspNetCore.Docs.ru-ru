---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Жизненный цикл приложения ASP.NET MVC 5 | Документация Майкрософт
author: cephalin
description: Скачайте документ PDF, что диаграммы жизненного цикла приложения ASP.NET MVC 5. В этом документе жизненного цикла показано высокоуровневое представление жизненного цикла MVC...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 98735f2e04bdd0f2fec19524e59f6272dbc4ca57
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393919"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="fbe92-104">Жизненный цикл приложения ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="fbe92-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="fbe92-105">по [Cephas Лин](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="fbe92-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="fbe92-106">Скачать PDF-документ</span><span class="sxs-lookup"><span data-stu-id="fbe92-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="fbe92-107">Здесь вы можете скачать PDF-документ, диаграммы, жизненного цикла каждого приложения ASP.NET MVC 5, получение HTTP запроса на отправку HTTP-ответ обратно клиенту.</span><span class="sxs-lookup"><span data-stu-id="fbe92-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="fbe92-108">Она предназначена, так как средство обучения для тех, кто еще не работали с ASP.NET MVC, а также как ссылку для тех, кому требуется вникать в определенных аспектов приложения.</span><span class="sxs-lookup"><span data-stu-id="fbe92-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="fbe92-109">PDF-документ имеет следующие особенности:</span><span class="sxs-lookup"><span data-stu-id="fbe92-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="fbe92-110">Соответствующие [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) этапы, которые помогут вам понять, где MVC интегрируется в [жизненный цикл приложения ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).</span><span class="sxs-lookup"><span data-stu-id="fbe92-110">Relevant [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/library/bb470252.aspx).</span></span>
- <span data-ttu-id="fbe92-111">Высокоуровневый обзор жизненного цикла приложения MVC, где вы понимаете основные этапы, из которых каждое приложение MVC передает в конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="fbe92-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="fbe92-112">Подробное представление, показывающий Детализирует вниз подробную информацию о конвейере обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="fbe92-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="fbe92-113">Вы можете сравнить высокоуровневое представление и подробное представление, чтобы увидеть, как собираются сведения о жизненных циклах на несколько этапов.</span><span class="sxs-lookup"><span data-stu-id="fbe92-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="fbe92-114">[Скачивание PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) для увеличения см. в разделе.</span><span class="sxs-lookup"><span data-stu-id="fbe92-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="fbe92-115">Размещение и цель всеми переопределяемыми методами на [контроллера](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) объекта в конвейере обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="fbe92-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="fbe92-116">Может или не иметь необходимости переопределять любые один метод, но важно понимать их роль в жизненном цикле приложения таким образом, вы можете написать код на этапе соответствующие жизненного цикла для эффекта, вы нашли.</span><span class="sxs-lookup"><span data-stu-id="fbe92-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="fbe92-117">Схемы из дутого вверх, показывающая способ вызова каждого из типов фильтра (проверки подлинности, авторизации, действие и результат).</span><span class="sxs-lookup"><span data-stu-id="fbe92-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="fbe92-118">Ссылка на полезные статьи или блога из каждого интересующую точку в представлении «Подробности».</span><span class="sxs-lookup"><span data-stu-id="fbe92-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="fbe92-119">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="fbe92-119">Next Steps</span></span>

<span data-ttu-id="fbe92-120">Этот документ соответствует требованиям?</span><span class="sxs-lookup"><span data-stu-id="fbe92-120">Does this document meet your need?</span></span> <span data-ttu-id="fbe92-121">Мы ценим ваши отзывы.</span><span class="sxs-lookup"><span data-stu-id="fbe92-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="fbe92-122">Если вам необходимо узнать о жизненном цикле ASP.NET MVC в приложении, [Stackoverflow](http://stackoverflow.com/help) и [форумах ASP.NET MVC](https://forums.asp.net/1146.aspx) — отличное место для запроса.</span><span class="sxs-lookup"><span data-stu-id="fbe92-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="fbe92-123">Выполните [мне](https://twitter.com/Cephas_MSFT) в twitter, поэтому вы можете получать обновления на моем новейшие учебники.</span><span class="sxs-lookup"><span data-stu-id="fbe92-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
