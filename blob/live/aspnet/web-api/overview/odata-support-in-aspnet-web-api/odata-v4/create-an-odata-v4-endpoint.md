---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: "Создайте конечную точку OData v4 с помощью ASP.NET Web API 2.2 | Документы Microsoft"
author: MikeWasson
description: "Протокол Open Data Protocol (OData) — это протокол доступа к данным для веб. OData предоставляет единообразный способ запроса и управления наборами данных через операции CRUD..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="59a4e-104">Создайте конечную точку OData v4 с помощью ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="59a4e-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="59a4e-105">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="59a4e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="59a4e-106">Протокол Open Data Protocol (OData) — это протокол доступа к данным для веб.</span><span class="sxs-lookup"><span data-stu-id="59a4e-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="59a4e-107">OData предоставляет единообразный способ запроса и управления наборами данных через операций CRUD (Создание, чтение, обновление и удаление).</span><span class="sxs-lookup"><span data-stu-id="59a4e-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
> 
> <span data-ttu-id="59a4e-108">Веб-API ASP.NET поддерживает v3 и v4 протокола.</span><span class="sxs-lookup"><span data-stu-id="59a4e-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="59a4e-109">Может содержать конечную точку v4, запускаемую side-by-side с конечной точкой v3.</span><span class="sxs-lookup"><span data-stu-id="59a4e-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
> 
> <span data-ttu-id="59a4e-110">Этого учебника показано, как создать конечную точку OData v4, которая поддерживает операции CRUD.</span><span class="sxs-lookup"><span data-stu-id="59a4e-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="59a4e-111">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="59a4e-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="59a4e-112">Веб-API 2.2</span><span class="sxs-lookup"><span data-stu-id="59a4e-112">Web API 2.2</span></span>
> - <span data-ttu-id="59a4e-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="59a4e-113">OData v4</span></span>
> - [<span data-ttu-id="59a4e-114">Visual Studio 2013 с обновлением 2</span><span class="sxs-lookup"><span data-stu-id="59a4e-114">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="59a4e-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="59a4e-115">Entity Framework 6</span></span>
> - <span data-ttu-id="59a4e-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="59a4e-116">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="59a4e-117">Версии учебника</span><span class="sxs-lookup"><span data-stu-id="59a4e-117">Tutorial versions</span></span>
> 
> <span data-ttu-id="59a4e-118">OData версии 3, в разделе [Создание конечной точки OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="59a4e-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="59a4e-119">Создание проекта Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59a4e-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="59a4e-120">В Visual Studio из **файл** последовательно выберите пункты **New** &gt; **проекта**.</span><span class="sxs-lookup"><span data-stu-id="59a4e-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="59a4e-121">Разверните **установленные** &gt; **шаблоны** &gt; **Visual C#** &gt; **Web**и выберите  **Веб-приложение ASP.NET** шаблона.</span><span class="sxs-lookup"><span data-stu-id="59a4e-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="59a4e-122">Назовите проект &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="59a4e-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="59a4e-123">В **новый проект** диалогового окна выберите **пустой** шаблона.</span><span class="sxs-lookup"><span data-stu-id="59a4e-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="59a4e-124">В разделе &quot;добавить папки и основные ссылки... &quot;, нажмите кнопку **веб-API**.</span><span class="sxs-lookup"><span data-stu-id="59a4e-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="59a4e-125">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="59a4e-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="59a4e-126">Установить пакеты OData</span><span class="sxs-lookup"><span data-stu-id="59a4e-126">Install the OData Packages</span></span>

<span data-ttu-id="59a4e-127">Из **средства** последовательно выберите пункты **диспетчера пакетов NuGet** &gt; **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="59a4e-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="59a4e-128">В окне консоли диспетчера пакетов введите следующее:</span><span class="sxs-lookup"><span data-stu-id="59a4e-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="59a4e-129">Эта команда устанавливает последние пакеты OData NuGet.</span><span class="sxs-lookup"><span data-stu-id="59a4e-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="59a4e-130">Добавьте класс модели</span><span class="sxs-lookup"><span data-stu-id="59a4e-130">Add a Model Class</span></span>

<span data-ttu-id="59a4e-131">Объект *модель* — это объект, представляющий сущность данных в приложении.</span><span class="sxs-lookup"><span data-stu-id="59a4e-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="59a4e-132">В обозревателе решений щелкните правой кнопкой мыши папку модели.</span><span class="sxs-lookup"><span data-stu-id="59a4e-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="59a4e-133">В контекстном меню выберите **добавить** &gt; **класса**.</span><span class="sxs-lookup"><span data-stu-id="59a4e-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="59a4e-134">По соглашению классы модели, помещаются в папку Models, но нет необходимости соответствуют этому соглашению в собственных проектах.</span><span class="sxs-lookup"><span data-stu-id="59a4e-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="59a4e-135">Присвойте классу имя `Product`.</span><span class="sxs-lookup"><span data-stu-id="59a4e-135">Name the class `Product`.</span></span> <span data-ttu-id="59a4e-136">В файле Product.cs замените стандартный код следующим:</span><span class="sxs-lookup"><span data-stu-id="59a4e-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="59a4e-137">`Id` Является ключом сущности.</span><span class="sxs-lookup"><span data-stu-id="59a4e-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="59a4e-138">Клиент может запрашивать сущности по ключу.</span><span class="sxs-lookup"><span data-stu-id="59a4e-138">Clients can query entities by key.</span></span> <span data-ttu-id="59a4e-139">Например, чтобы получить продукта с Идентификатором 5, URI имеет следующий вид `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="59a4e-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="59a4e-140">`Id` Свойство будет первичного ключа в серверную базу данных.</span><span class="sxs-lookup"><span data-stu-id="59a4e-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="59a4e-141">Включение платформы Entity Framework</span><span class="sxs-lookup"><span data-stu-id="59a4e-141">Enable Entity Framework</span></span>

<span data-ttu-id="59a4e-142">В этом учебнике мы будем использовать Entity Framework (EF) Code First для создания базы данных с таблицами.</span><span class="sxs-lookup"><span data-stu-id="59a4e-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="59a4e-143">EF не требует OData веб-API.</span><span class="sxs-lookup"><span data-stu-id="59a4e-143">Web API OData does not require EF.</span></span> <span data-ttu-id="59a4e-144">Используйте любой слой доступа к данным, может преобразовывать сущностей базы данных в модели.</span><span class="sxs-lookup"><span data-stu-id="59a4e-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="59a4e-145">Во-первых необходимо установите пакет NuGet для EF.</span><span class="sxs-lookup"><span data-stu-id="59a4e-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="59a4e-146">Из **средства** последовательно выберите пункты **диспетчера пакетов NuGet** &gt; **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="59a4e-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="59a4e-147">В окне консоли диспетчера пакетов введите следующее:</span><span class="sxs-lookup"><span data-stu-id="59a4e-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="59a4e-148">Откройте файл Web.config и добавьте следующий раздел в **конфигурации** элемент после того, как **configSections** элемента.</span><span class="sxs-lookup"><span data-stu-id="59a4e-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="59a4e-149">Этот параметр добавляет строку подключения для базы данных LocalDB.</span><span class="sxs-lookup"><span data-stu-id="59a4e-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="59a4e-150">Эта база данных будет использоваться при локальном запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="59a4e-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="59a4e-151">Добавьте класс с именем `ProductsContext` папке «модели»:</span><span class="sxs-lookup"><span data-stu-id="59a4e-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="59a4e-152">В конструкторе `"name=ProductsContext"` предоставляет имя строки подключения.</span><span class="sxs-lookup"><span data-stu-id="59a4e-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="59a4e-153">Настройка конечной точки OData</span><span class="sxs-lookup"><span data-stu-id="59a4e-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="59a4e-154">Откройте файл приложения\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="59a4e-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="59a4e-155">Добавьте следующие **с помощью** инструкции:</span><span class="sxs-lookup"><span data-stu-id="59a4e-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="59a4e-156">Затем добавьте следующий код в **зарегистрировать** метод:</span><span class="sxs-lookup"><span data-stu-id="59a4e-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="59a4e-157">Этот код выполняет следующие операции:</span><span class="sxs-lookup"><span data-stu-id="59a4e-157">This code does two things:</span></span>

- <span data-ttu-id="59a4e-158">Создает модель EDM (модель EDM).</span><span class="sxs-lookup"><span data-stu-id="59a4e-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="59a4e-159">Добавляет маршрут.</span><span class="sxs-lookup"><span data-stu-id="59a4e-159">Adds a route.</span></span>

<span data-ttu-id="59a4e-160">EDM-это абстрактный модель данных.</span><span class="sxs-lookup"><span data-stu-id="59a4e-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="59a4e-161">Модель EDM используется для создания документ метаданных службы.</span><span class="sxs-lookup"><span data-stu-id="59a4e-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="59a4e-162">**ODataConventionModelBuilder** класс создает EDM, с использованием контекста именования по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="59a4e-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="59a4e-163">Этот подход требует наименьшего объема кода.</span><span class="sxs-lookup"><span data-stu-id="59a4e-163">This approach requires the least code.</span></span> <span data-ttu-id="59a4e-164">Если требуется больший контроль над модели EDM, можно использовать **ODataModelBuilder** класса для создания модели EDM, добавив явным образом свойства, ключи и свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="59a4e-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="59a4e-165">Объект *маршрута* узнает перенаправления HTTP-запросов к конечной точке веб-API.</span><span class="sxs-lookup"><span data-stu-id="59a4e-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="59a4e-166">Чтобы создать маршрут OData v4, вызовите **MapODataServiceRoute** метода расширения.</span><span class="sxs-lookup"><span data-stu-id="59a4e-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="59a4e-167">Если приложение имеет несколько конечных точек OData, создайте отдельный маршрут для каждого.</span><span class="sxs-lookup"><span data-stu-id="59a4e-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="59a4e-168">Укажите каждый маршрут маршрута, уникальное имя и префикс.</span><span class="sxs-lookup"><span data-stu-id="59a4e-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="59a4e-169">Добавить контроллер OData</span><span class="sxs-lookup"><span data-stu-id="59a4e-169">Add the OData Controller</span></span>

<span data-ttu-id="59a4e-170">Объект *контроллера* — это класс, который обрабатывает HTTP-запросы.</span><span class="sxs-lookup"><span data-stu-id="59a4e-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="59a4e-171">Создается отдельный контроллер для каждого набора сущностей в службе OData.</span><span class="sxs-lookup"><span data-stu-id="59a4e-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="59a4e-172">В этом учебнике вы создадите один контроллер для `Product` сущности.</span><span class="sxs-lookup"><span data-stu-id="59a4e-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="59a4e-173">В обозревателе решений щелкните правой кнопкой мыши папку Controllers и выбрать **добавить** &gt; **класса**.</span><span class="sxs-lookup"><span data-stu-id="59a4e-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="59a4e-174">Присвойте классу имя `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="59a4e-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="59a4e-175">Версия этого учебника для OData v3 использует **добавить контроллер** формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="59a4e-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="59a4e-176">В настоящее время нет без формирования шаблонов для OData v4.</span><span class="sxs-lookup"><span data-stu-id="59a4e-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="59a4e-177">Замените код для шаблона в ProductsController.cs ниже.</span><span class="sxs-lookup"><span data-stu-id="59a4e-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="59a4e-178">Использует контроллер `ProductsContext` класса для доступа к базе данных с помощью EF.</span><span class="sxs-lookup"><span data-stu-id="59a4e-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="59a4e-179">Обратите внимание, что контроллер переопределяет **Dispose** метод **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="59a4e-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="59a4e-180">Это является начальной точкой для контроллера.</span><span class="sxs-lookup"><span data-stu-id="59a4e-180">This is the starting point for the controller.</span></span> <span data-ttu-id="59a4e-181">Далее мы добавим методы для всех операций CRUD.</span><span class="sxs-lookup"><span data-stu-id="59a4e-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="59a4e-182">Запрос к набору сущностей</span><span class="sxs-lookup"><span data-stu-id="59a4e-182">Querying the Entity Set</span></span>

<span data-ttu-id="59a4e-183">Добавьте следующие методы для `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="59a4e-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="59a4e-184">Без параметров версию `Get` метод возвращает всю коллекцию продуктов.</span><span class="sxs-lookup"><span data-stu-id="59a4e-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="59a4e-185">`Get` Метод с *ключ* параметр ищет продукта по его ключу (в этом случае `Id` свойство).</span><span class="sxs-lookup"><span data-stu-id="59a4e-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="59a4e-186">**[EnableQuery]** атрибутов позволяет изменить этот запрос, используя параметры запроса $filter, $sort и $page клиентам.</span><span class="sxs-lookup"><span data-stu-id="59a4e-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="59a4e-187">Дополнительные сведения см. в разделе [поддержки параметров запроса OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="59a4e-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="59a4e-188">Добавление сущности в наборе сущностей</span><span class="sxs-lookup"><span data-stu-id="59a4e-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="59a4e-189">Чтобы клиенты могли добавить новый продукт в базе данных, добавьте следующий метод `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="59a4e-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="59a4e-190">Обновление сущности</span><span class="sxs-lookup"><span data-stu-id="59a4e-190">Updating an Entity</span></span>

<span data-ttu-id="59a4e-191">Обновление сущности, PUT и PATCH OData поддерживает два различную семантику.</span><span class="sxs-lookup"><span data-stu-id="59a4e-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="59a4e-192">ИСПРАВЛЕНИЕ выполняет частичное обновление.</span><span class="sxs-lookup"><span data-stu-id="59a4e-192">PATCH performs a partial update.</span></span> <span data-ttu-id="59a4e-193">Клиент указывает только те свойства, для обновления.</span><span class="sxs-lookup"><span data-stu-id="59a4e-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="59a4e-194">PUT заменяет всю сущность.</span><span class="sxs-lookup"><span data-stu-id="59a4e-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="59a4e-195">Недостатком PUT является то, что клиент должен отправить значения для всех свойств в сущности, включая значения, которые не изменяются.</span><span class="sxs-lookup"><span data-stu-id="59a4e-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="59a4e-196">[Спецификации OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) утверждается, что исправление является предпочтительным.</span><span class="sxs-lookup"><span data-stu-id="59a4e-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="59a4e-197">Ниже приведен код для методов PUT и PATCH в любом случае:</span><span class="sxs-lookup"><span data-stu-id="59a4e-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="59a4e-198">В случае обновления, контроллер использует **дельта&lt;T&gt;**  тип для отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="59a4e-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="59a4e-199">Удаление сущности</span><span class="sxs-lookup"><span data-stu-id="59a4e-199">Deleting an Entity</span></span>

<span data-ttu-id="59a4e-200">Чтобы клиенты могли удалить продукт из базы данных, добавьте следующий метод `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="59a4e-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
