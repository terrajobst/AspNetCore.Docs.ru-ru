---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Общие сведения о процессе выполнения ASP.NET MVC | Документация Майкрософт
author: microsoft
description: Узнайте, как платформа ASP.NET MVC обрабатывает запрос браузера пошаговые.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 3a3bcf4b78e3b19fb4d293f67b68c3a790d05221
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838464"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="a8f76-103">Общие сведения о процессе выполнения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a8f76-103">Understanding the ASP.NET MVC Execution Process</span></span>
====================
<span data-ttu-id="a8f76-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a8f76-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a8f76-105">Узнайте, как платформа ASP.NET MVC обрабатывает запрос браузера пошаговые.</span><span class="sxs-lookup"><span data-stu-id="a8f76-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>


<span data-ttu-id="a8f76-106">Запросы на основе ASP.NET MVC веб-приложения сначала пройти через **UrlRoutingModule** объект, являющийся модулем HTTP.</span><span class="sxs-lookup"><span data-stu-id="a8f76-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="a8f76-107">Этот модуль анализирует запрос и выбирает маршрут.</span><span class="sxs-lookup"><span data-stu-id="a8f76-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="a8f76-108">**UrlRoutingModule** объекта выбирает первый объект маршрута, соответствующий текущему запросу.</span><span class="sxs-lookup"><span data-stu-id="a8f76-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="a8f76-109">(Объект маршрута — это класс, реализующий **RouteBase**, и обычно является экземпляром **маршрута** класса.) Если подходящих маршрутов нет, **UrlRoutingModule** объекта ничего не делает и передает запрос обратно на обычный запрос ASP.NET или IIS обработку.</span><span class="sxs-lookup"><span data-stu-id="a8f76-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="a8f76-110">Из выбранного **маршрута** объекта, **UrlRoutingModule** объектом **IRouteHandler** объекта, связанного с **маршрута**объекта.</span><span class="sxs-lookup"><span data-stu-id="a8f76-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="a8f76-111">Как правило, в приложении MVC, это будет экземпляр **значение MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="a8f76-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="a8f76-112">**IRouteHandler** создает экземпляр **IHttpHandler** и передает его **IHttpContext** объекта.</span><span class="sxs-lookup"><span data-stu-id="a8f76-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="a8f76-113">По умолчанию **IHttpHandler** экземпляра для MVC — **MvcHandler** объекта.</span><span class="sxs-lookup"><span data-stu-id="a8f76-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="a8f76-114">**MvcHandler** объект затем выбирает контроллер, который будет обрабатывать запрос.</span><span class="sxs-lookup"><span data-stu-id="a8f76-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="a8f76-115">Когда MVC веб-приложение ASP.NET работает в IIS 7.0, без расширения имени файла является обязательным для проектов MVC.</span><span class="sxs-lookup"><span data-stu-id="a8f76-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="a8f76-116">Тем не менее в IIS 6.0, обработчик необходимо, чтобы расширение имени файла MVC было связано с библиотекой DLL ISAPI ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a8f76-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>


<span data-ttu-id="a8f76-117">Модуль и обработчик, являются точками входа с платформой ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a8f76-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="a8f76-118">Они выполняют следующие действия:</span><span class="sxs-lookup"><span data-stu-id="a8f76-118">They perform the following actions:</span></span>

- <span data-ttu-id="a8f76-119">Выбор нужного контроллера в веб-приложение MVC.</span><span class="sxs-lookup"><span data-stu-id="a8f76-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="a8f76-120">Получение отдельного экземпляра контроллера.</span><span class="sxs-lookup"><span data-stu-id="a8f76-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="a8f76-121">Вызов контроллера **Execute** метод.</span><span class="sxs-lookup"><span data-stu-id="a8f76-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="a8f76-122">В следующем списке перечислены этапы выполнения для MVC веб-проекта:</span><span class="sxs-lookup"><span data-stu-id="a8f76-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="a8f76-123">Получение первого запроса приложения</span><span class="sxs-lookup"><span data-stu-id="a8f76-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="a8f76-124">В файле Global.asax **маршрута** объекты добавляются в **RouteTable** объекта.</span><span class="sxs-lookup"><span data-stu-id="a8f76-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="a8f76-125">Выполнение маршрутизации</span><span class="sxs-lookup"><span data-stu-id="a8f76-125">Perform routing</span></span> 

    - <span data-ttu-id="a8f76-126">**UrlRoutingModule** модуль использует первый подходящий **маршрута** объекта в **RouteTable** коллекции для создания **RouteData** Объект, который затем используется для создания **RequestContext** (**IHttpContext**) объекта.</span><span class="sxs-lookup"><span data-stu-id="a8f76-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="a8f76-127">Создание обработчика запроса MVC</span><span class="sxs-lookup"><span data-stu-id="a8f76-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="a8f76-128">**Значение MvcRouteHandler** объекта создает экземпляр класса **MvcHandler** класса и передает его **RequestContext** экземпляра.</span><span class="sxs-lookup"><span data-stu-id="a8f76-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="a8f76-129">Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="a8f76-129">Create controller</span></span> 

    - <span data-ttu-id="a8f76-130">**MvcHandler** объектов используют **RequestContext** экземпляра для идентификации **IControllerFactory** объекта (обычно это экземпляр  **DefaultControllerFactory** класс) для создания экземпляра контроллера с.</span><span class="sxs-lookup"><span data-stu-id="a8f76-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="a8f76-131">Контроллер EXECUTE — **MvcHandler** экземпляра вызывает у контроллера s **Execute** метод.</span><span class="sxs-lookup"><span data-stu-id="a8f76-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="a8f76-132">Вызов действия</span><span class="sxs-lookup"><span data-stu-id="a8f76-132">Invoke action</span></span> 

    - <span data-ttu-id="a8f76-133">Большинство контроллеров наследуются от класса **контроллера** базового класса.</span><span class="sxs-lookup"><span data-stu-id="a8f76-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="a8f76-134">Для контроллеров, для выполнения этой задачи **ControllerActionInvoker** объект, связанный с контроллером определяет, какой метод действия контроллера класса для вызова и затем вызывает этот метод.</span><span class="sxs-lookup"><span data-stu-id="a8f76-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="a8f76-135">Выполнение результата</span><span class="sxs-lookup"><span data-stu-id="a8f76-135">Execute result</span></span> 

    - <span data-ttu-id="a8f76-136">Метод типичные действия может реагировать на действия пользователя, Подготовка соответствующие данные ответа и затем выполните результат, возвращая тип результата.</span><span class="sxs-lookup"><span data-stu-id="a8f76-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="a8f76-137">К исполняемым типам результата, которые могут быть выполнены следующие: **ViewResult** (который выполняет визуализацию представления и является типом результата чаще всего), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, и **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="a8f76-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
