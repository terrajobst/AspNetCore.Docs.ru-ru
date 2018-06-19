---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Основные сведения о процессе выполнения ASP.NET MVC | Документы Microsoft
author: microsoft
description: Узнайте, как платформа ASP.NET MVC обрабатывает пошаговые запрос браузера.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26500733"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="65d97-103">Основные сведения о процессе выполнения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="65d97-103">Understanding the ASP.NET MVC Execution Process</span></span>
====================
<span data-ttu-id="65d97-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="65d97-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="65d97-105">Узнайте, как платформа ASP.NET MVC обрабатывает пошаговые запрос браузера.</span><span class="sxs-lookup"><span data-stu-id="65d97-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>


<span data-ttu-id="65d97-106">Запросы на основе ASP.NET MVC веб-приложения сначала пройти через **UrlRoutingModule** объект, используемый модулем HTTP.</span><span class="sxs-lookup"><span data-stu-id="65d97-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="65d97-107">Этот модуль анализирует запрос и выбирает маршрут.</span><span class="sxs-lookup"><span data-stu-id="65d97-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="65d97-108">**UrlRoutingModule** выбирает первый объект маршрута, соответствующий текущему запросу.</span><span class="sxs-lookup"><span data-stu-id="65d97-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="65d97-109">(Объект маршрута — это класс, реализующий **RouteBase**, и обычно является экземпляром класса **маршрута** класса.) Если подходящих маршрутов нет, **UrlRoutingModule** объекта ничего не делает и передает запрос обратно на обычный запрос ASP.NET или IIS обработку.</span><span class="sxs-lookup"><span data-stu-id="65d97-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="65d97-110">Из выбранного **маршрута** объекта, **UrlRoutingModule** объектом **IRouteHandler** объекта, с которым связан **маршрута**объекта.</span><span class="sxs-lookup"><span data-stu-id="65d97-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="65d97-111">Как правило, в MVC-приложении, это будет экземпляр **MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="65d97-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="65d97-112">**IRouteHandler** создает экземпляр **IHttpHandler** и передает его **IHttpContext** объекта.</span><span class="sxs-lookup"><span data-stu-id="65d97-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="65d97-113">По умолчанию **IHttpHandler** экземпляра для MVC — **MvcHandler** объекта.</span><span class="sxs-lookup"><span data-stu-id="65d97-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="65d97-114">**MvcHandler** объект затем выбирает контроллер, который будет обрабатывать запрос.</span><span class="sxs-lookup"><span data-stu-id="65d97-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="65d97-115">При выполнении MVC веб-приложения ASP.NET в IIS 7.0 без расширения имени файла является обязательным для проектов MVC.</span><span class="sxs-lookup"><span data-stu-id="65d97-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="65d97-116">В службах IIS 6.0 обработчик требуется сопоставить расширение имени файла MVC было связано, к библиотеке DLL ISAPI ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="65d97-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>


<span data-ttu-id="65d97-117">Модуль и обработчик являются точками входа с платформой ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="65d97-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="65d97-118">Они выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="65d97-118">They perform the following actions:</span></span>

- <span data-ttu-id="65d97-119">Выбор нужного контроллера в веб-приложение MVC.</span><span class="sxs-lookup"><span data-stu-id="65d97-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="65d97-120">Получение отдельного экземпляра контроллера.</span><span class="sxs-lookup"><span data-stu-id="65d97-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="65d97-121">Вызов контроллера **Execute** метод.</span><span class="sxs-lookup"><span data-stu-id="65d97-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="65d97-122">Ниже перечислены этапы выполнения веб-проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="65d97-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="65d97-123">Получение первого запроса для приложения</span><span class="sxs-lookup"><span data-stu-id="65d97-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="65d97-124">В файле Global.asax **маршрута** объекты добавляются в **RouteTable** объекта.</span><span class="sxs-lookup"><span data-stu-id="65d97-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="65d97-125">Выполнение маршрутизации</span><span class="sxs-lookup"><span data-stu-id="65d97-125">Perform routing</span></span> 

    - <span data-ttu-id="65d97-126">**UrlRoutingModule** модуль использует первый подходящий **маршрута** объекта в **RouteTable** коллекции для создания **RouteData** Объект, который затем используется для создания **RequestContext** (**IHttpContext**) объекта.</span><span class="sxs-lookup"><span data-stu-id="65d97-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="65d97-127">Создание обработчика запроса MVC</span><span class="sxs-lookup"><span data-stu-id="65d97-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="65d97-128">**MvcRouteHandler** объект создает экземпляр **MvcHandler** класса и передает его **RequestContext** экземпляра.</span><span class="sxs-lookup"><span data-stu-id="65d97-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="65d97-129">Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="65d97-129">Create controller</span></span> 

    - <span data-ttu-id="65d97-130">**MvcHandler** объектов используют **RequestContext** экземпляра для идентификации **IControllerFactory** объекта (обычно это экземпляр  **DefaultControllerFactory** класса) для создания экземпляра контроллера с.</span><span class="sxs-lookup"><span data-stu-id="65d97-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="65d97-131">Контроллер EXECUTE — **MvcHandler** экземпляр вызывает контроллер s **Execute** метод.</span><span class="sxs-lookup"><span data-stu-id="65d97-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="65d97-132">Вызов действия</span><span class="sxs-lookup"><span data-stu-id="65d97-132">Invoke action</span></span> 

    - <span data-ttu-id="65d97-133">Наследуют большинство контроллеров **контроллера** базового класса.</span><span class="sxs-lookup"><span data-stu-id="65d97-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="65d97-134">Для контроллеров, сделать это **ControllerActionInvoker** объекта, связанного с контроллером определяет, какой метод действия контроллера класса для вызова, а затем вызывает этот метод.</span><span class="sxs-lookup"><span data-stu-id="65d97-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="65d97-135">Выполнение результата</span><span class="sxs-lookup"><span data-stu-id="65d97-135">Execute result</span></span> 

    - <span data-ttu-id="65d97-136">Метод типичные действия может реагировать на действия пользователя, подготовьте соответствующие данные ответа и выполните результат, возвращая тип результата.</span><span class="sxs-lookup"><span data-stu-id="65d97-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="65d97-137">Следующие типы встроенных результатов, которые могут быть выполнены: **ViewResult** (который выполняет визуализацию представления и является типом результата чаще всего), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, и **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="65d97-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
