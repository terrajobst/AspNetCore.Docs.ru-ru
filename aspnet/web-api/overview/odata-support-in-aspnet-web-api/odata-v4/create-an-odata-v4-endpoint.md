---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Создайте конечную точку OData версии 4 с помощью ASP.NET Web API 2.2 | Документация Майкрософт
author: MikeWasson
description: Open Data Protocol (OData) — это протокол доступа к данным веб-приложений. OData предоставляет универсальный способ запросов и управление ими наборов данных с помощью операции CRUD...
ms.author: aspnetcontent
ms.date: 06/24/2014
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 04fad9b569972f11256c6b7288db34d4996ca8bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804216"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="96626-104">Создайте конечную точку OData версии 4 с помощью ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="96626-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="96626-105">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="96626-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="96626-106">Open Data Protocol (OData) — это протокол доступа к данным веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="96626-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="96626-107">OData предоставляет универсальный способ запросов и управление ими наборов данных с помощью операции CRUD (Создание, чтение, обновление и удаление).</span><span class="sxs-lookup"><span data-stu-id="96626-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
> 
> <span data-ttu-id="96626-108">Веб-API ASP.NET поддерживает v3 и v4 протокола.</span><span class="sxs-lookup"><span data-stu-id="96626-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="96626-109">Может иметь конечную точку версии 4, которая запускает side-by-side с конечной точкой v3.</span><span class="sxs-lookup"><span data-stu-id="96626-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
> 
> <span data-ttu-id="96626-110">Этом руководстве показано, как создать конечную точку OData v4, поддерживающий операции CRUD.</span><span class="sxs-lookup"><span data-stu-id="96626-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="96626-111">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="96626-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="96626-112">Веб-API 2.2</span><span class="sxs-lookup"><span data-stu-id="96626-112">Web API 2.2</span></span>
> - <span data-ttu-id="96626-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="96626-113">OData v4</span></span>
> - [<span data-ttu-id="96626-114">Visual Studio 2013 с обновлением 2</span><span class="sxs-lookup"><span data-stu-id="96626-114">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="96626-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="96626-115">Entity Framework 6</span></span>
> - <span data-ttu-id="96626-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="96626-116">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="96626-117">Учебника по версии</span><span class="sxs-lookup"><span data-stu-id="96626-117">Tutorial versions</span></span>
> 
> <span data-ttu-id="96626-118">OData версии 3, см. в разделе [Создание конечной точки OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="96626-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="96626-119">Создание проекта Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96626-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="96626-120">В Visual Studio из **файл** меню, выберите **New** &gt; **проекта**.</span><span class="sxs-lookup"><span data-stu-id="96626-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="96626-121">Разверните **установленные** &gt; **шаблоны** &gt; **Visual C#** &gt; **Web**и выберите  **Веб-приложение ASP.NET** шаблона.</span><span class="sxs-lookup"><span data-stu-id="96626-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="96626-122">Назовите проект &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="96626-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="96626-123">В **новый проект** диалоговом окне выберите **пустой** шаблона.</span><span class="sxs-lookup"><span data-stu-id="96626-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="96626-124">В разделе &quot;добавить папки и основные ссылки... &quot;, нажмите кнопку **веб-API**.</span><span class="sxs-lookup"><span data-stu-id="96626-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="96626-125">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="96626-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="96626-126">Установите пакеты OData</span><span class="sxs-lookup"><span data-stu-id="96626-126">Install the OData Packages</span></span>

<span data-ttu-id="96626-127">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** &gt; **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="96626-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="96626-128">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="96626-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="96626-129">Эта команда устанавливает последнюю версию пакетов OData NuGet.</span><span class="sxs-lookup"><span data-stu-id="96626-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="96626-130">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="96626-130">Add a Model Class</span></span>

<span data-ttu-id="96626-131">Объект *модели* — это объект, представляющий сущность данных в приложении.</span><span class="sxs-lookup"><span data-stu-id="96626-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="96626-132">В обозревателе решений щелкните правой кнопкой мыши папку Models.</span><span class="sxs-lookup"><span data-stu-id="96626-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="96626-133">В контекстном меню выберите **добавить** &gt; **класс**.</span><span class="sxs-lookup"><span data-stu-id="96626-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="96626-134">По соглашению классы моделей, помещаются в папку Models, но у вас нет следуют соглашению в своих собственных проектах.</span><span class="sxs-lookup"><span data-stu-id="96626-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="96626-135">Присвойте классу имя `Product`.</span><span class="sxs-lookup"><span data-stu-id="96626-135">Name the class `Product`.</span></span> <span data-ttu-id="96626-136">В файле замените код шаблона следующим:</span><span class="sxs-lookup"><span data-stu-id="96626-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="96626-137">`Id` Свойство является ключом сущности.</span><span class="sxs-lookup"><span data-stu-id="96626-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="96626-138">Клиенты могут запрашивать сущности по ключу.</span><span class="sxs-lookup"><span data-stu-id="96626-138">Clients can query entities by key.</span></span> <span data-ttu-id="96626-139">Например, чтобы получить продукт с Идентификатором 5, URI имеет `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="96626-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="96626-140">`Id` Свойство также будет иметь первичный ключ в базе данных серверной части.</span><span class="sxs-lookup"><span data-stu-id="96626-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="96626-141">Включить Entity Framework</span><span class="sxs-lookup"><span data-stu-id="96626-141">Enable Entity Framework</span></span>

<span data-ttu-id="96626-142">В этом учебнике мы будем использовать Entity Framework (EF) Code First для создания серверной базы данных.</span><span class="sxs-lookup"><span data-stu-id="96626-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="96626-143">Веб-API OData EF не требуется.</span><span class="sxs-lookup"><span data-stu-id="96626-143">Web API OData does not require EF.</span></span> <span data-ttu-id="96626-144">Используйте любой уровень доступа к данным, который можно преобразовать сущностей базы данных в модели.</span><span class="sxs-lookup"><span data-stu-id="96626-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="96626-145">Во-первых установите пакет NuGet для Entity FRAMEWORK.</span><span class="sxs-lookup"><span data-stu-id="96626-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="96626-146">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** &gt; **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="96626-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="96626-147">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="96626-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="96626-148">Откройте файл Web.config и добавьте следующий раздел внутри **конфигурации** элемент после **configSections** элемент.</span><span class="sxs-lookup"><span data-stu-id="96626-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="96626-149">Этот параметр добавляет строку подключения для базы данных LocalDB.</span><span class="sxs-lookup"><span data-stu-id="96626-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="96626-150">Эта база данных будет использоваться при локальном запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="96626-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="96626-151">Добавьте класс с именем `ProductsContext` папке «модели»:</span><span class="sxs-lookup"><span data-stu-id="96626-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="96626-152">В конструкторе `"name=ProductsContext"` предоставляет имя строки подключения.</span><span class="sxs-lookup"><span data-stu-id="96626-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="96626-153">Настройка конечной точки OData</span><span class="sxs-lookup"><span data-stu-id="96626-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="96626-154">Откройте файл приложения\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="96626-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="96626-155">Добавьте следующий **с помощью** инструкции:</span><span class="sxs-lookup"><span data-stu-id="96626-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="96626-156">Затем добавьте следующий код, чтобы **зарегистрировать** метод:</span><span class="sxs-lookup"><span data-stu-id="96626-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="96626-157">Этот код делает две вещи:</span><span class="sxs-lookup"><span data-stu-id="96626-157">This code does two things:</span></span>

- <span data-ttu-id="96626-158">Создает Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="96626-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="96626-159">Добавление маршрута.</span><span class="sxs-lookup"><span data-stu-id="96626-159">Adds a route.</span></span>

<span data-ttu-id="96626-160">EDM является абстрактной моделью данных.</span><span class="sxs-lookup"><span data-stu-id="96626-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="96626-161">Модель EDM используется для создания документ метаданных службы.</span><span class="sxs-lookup"><span data-stu-id="96626-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="96626-162">**ODataConventionModelBuilder** класс создает EDM с помощью соглашения об именовании по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="96626-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="96626-163">Этот подход требует наименьшего объема кода.</span><span class="sxs-lookup"><span data-stu-id="96626-163">This approach requires the least code.</span></span> <span data-ttu-id="96626-164">Если требуется больший контроль над модели EDM, можно использовать **ODataModelBuilder** класс, чтобы создать модель EDM путем добавления свойств, разделов и свойств навигации явным образом.</span><span class="sxs-lookup"><span data-stu-id="96626-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="96626-165">Объект *маршрута* сообщает способ маршрутизации HTTP-запросы на конечную точку веб-API.</span><span class="sxs-lookup"><span data-stu-id="96626-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="96626-166">Чтобы создать маршрут OData v4, вызовите **MapODataServiceRoute** метода расширения.</span><span class="sxs-lookup"><span data-stu-id="96626-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="96626-167">Если приложение имеет несколько конечных точек OData, создайте отдельный маршрут для каждого.</span><span class="sxs-lookup"><span data-stu-id="96626-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="96626-168">Присвойте каждого маршрута, уникальное имя маршрута и префикса.</span><span class="sxs-lookup"><span data-stu-id="96626-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="96626-169">Добавление контроллера OData</span><span class="sxs-lookup"><span data-stu-id="96626-169">Add the OData Controller</span></span>

<span data-ttu-id="96626-170">Объект *контроллера* является классом, который обрабатывает HTTP-запросы.</span><span class="sxs-lookup"><span data-stu-id="96626-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="96626-171">Можно создать отдельный контроллер для каждого набора сущностей в службе OData.</span><span class="sxs-lookup"><span data-stu-id="96626-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="96626-172">В этом руководстве вы создадите один контроллер для `Product` сущности.</span><span class="sxs-lookup"><span data-stu-id="96626-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="96626-173">В обозревателе решений щелкните правой кнопкой мыши папку Controllers и выберите **добавить** &gt; **класс**.</span><span class="sxs-lookup"><span data-stu-id="96626-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="96626-174">Присвойте классу имя `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="96626-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="96626-175">Версия этого руководства для OData v3 использует **Добавление контроллера** формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="96626-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="96626-176">В настоящее время нет без формирования шаблонов для OData v4.</span><span class="sxs-lookup"><span data-stu-id="96626-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="96626-177">Замените код в ProductsController.cs следующим.</span><span class="sxs-lookup"><span data-stu-id="96626-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="96626-178">Использует контроллер `ProductsContext` класс для доступа к базе данных с помощью EF.</span><span class="sxs-lookup"><span data-stu-id="96626-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="96626-179">Обратите внимание, что контроллер переопределяет **Dispose** метод **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="96626-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="96626-180">Это отправной точкой для контроллера.</span><span class="sxs-lookup"><span data-stu-id="96626-180">This is the starting point for the controller.</span></span> <span data-ttu-id="96626-181">Далее мы добавим методы для всех операций CRUD.</span><span class="sxs-lookup"><span data-stu-id="96626-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="96626-182">Запрос набора сущностей</span><span class="sxs-lookup"><span data-stu-id="96626-182">Querying the Entity Set</span></span>

<span data-ttu-id="96626-183">Добавьте следующие методы для `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="96626-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="96626-184">Без параметров версию `Get` метод возвращает всей коллекции продуктов.</span><span class="sxs-lookup"><span data-stu-id="96626-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="96626-185">`Get` Метод с *ключ* параметр ищет продукта по его ключу (в этом случае `Id` свойство).</span><span class="sxs-lookup"><span data-stu-id="96626-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="96626-186">**[EnableQuery]** атрибут позволяет клиентам изменить этот запрос, используя параметры запроса $filter, $sort и $page.</span><span class="sxs-lookup"><span data-stu-id="96626-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="96626-187">Дополнительные сведения см. в разделе [поддержки параметров запроса OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="96626-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="96626-188">Добавление сущности в наборе сущностей</span><span class="sxs-lookup"><span data-stu-id="96626-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="96626-189">Чтобы клиенты могли добавить новый продукт в базу данных, добавьте следующий метод в `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="96626-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="96626-190">Обновление сущности</span><span class="sxs-lookup"><span data-stu-id="96626-190">Updating an Entity</span></span>

<span data-ttu-id="96626-191">OData поддерживает два разных семантику для обновления сущностей, ИСПРАВЛЕНИЯМИ и PUT.</span><span class="sxs-lookup"><span data-stu-id="96626-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="96626-192">PATCH выполняет частичное обновление.</span><span class="sxs-lookup"><span data-stu-id="96626-192">PATCH performs a partial update.</span></span> <span data-ttu-id="96626-193">Клиент указывает только те свойства, для обновления.</span><span class="sxs-lookup"><span data-stu-id="96626-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="96626-194">PUT заменяет всю сущность.</span><span class="sxs-lookup"><span data-stu-id="96626-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="96626-195">Недостатком PUT является то, что клиент должен отправлять значения для всех свойств в сущности, включая значения, которые не изменяются.</span><span class="sxs-lookup"><span data-stu-id="96626-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="96626-196">[Спецификации OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) утверждается, что исправление является предпочтительным.</span><span class="sxs-lookup"><span data-stu-id="96626-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="96626-197">Ниже приведен код для методов PUT и PATCH в любом случае:</span><span class="sxs-lookup"><span data-stu-id="96626-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="96626-198">В случае с ИСПРАВЛЕНИЯМИ, контроллер использует **разностных&lt;T&gt;**  тип для отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="96626-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="96626-199">Удаление сущности</span><span class="sxs-lookup"><span data-stu-id="96626-199">Deleting an Entity</span></span>

<span data-ttu-id="96626-200">Чтобы клиенты могли удалить продукт из базы данных, добавьте следующий метод в `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="96626-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
