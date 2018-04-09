---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Создание конечной точки OData v3 с веб-API 2 | Документы Microsoft
author: MikeWasson
description: Протокол Open Data Protocol (OData) — это протокол доступа к данным для веб. OData предоставляет единообразный способ структуру данных, запроса данных и работы с данными...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 227faacd3f42731e08a4cd2b71075776309961b6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="46a6c-104">Создание конечной точки OData v3 с веб-API 2</span><span class="sxs-lookup"><span data-stu-id="46a6c-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="46a6c-105">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="46a6c-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="46a6c-106">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="46a6c-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="46a6c-107">[Open Data Protocol](http://www.odata.org/) (OData) — это протокол доступа к данным для веб.</span><span class="sxs-lookup"><span data-stu-id="46a6c-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="46a6c-108">OData предоставляет единообразный способ структуру данных, запрашивать данные и управлять набором данных посредством операций CRUD (Создание, чтение, обновление и удаление).</span><span class="sxs-lookup"><span data-stu-id="46a6c-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="46a6c-109">OData поддерживает AtomPub (XML) и форматах JSON.</span><span class="sxs-lookup"><span data-stu-id="46a6c-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="46a6c-110">OData также определяет способ предоставления метаданных о данных.</span><span class="sxs-lookup"><span data-stu-id="46a6c-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="46a6c-111">Клиенты могут использовать метаданные для обнаружения информации о типе и связи в наборе данных.</span><span class="sxs-lookup"><span data-stu-id="46a6c-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
> 
> <span data-ttu-id="46a6c-112">Веб-API ASP.NET позволяет легко создать конечную точку OData для набора данных.</span><span class="sxs-lookup"><span data-stu-id="46a6c-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="46a6c-113">Можно управлять, конечная точка поддерживает только операции, которые OData.</span><span class="sxs-lookup"><span data-stu-id="46a6c-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="46a6c-114">Можно разместить несколько конечных точек OData, наряду с конечными точками не OData.</span><span class="sxs-lookup"><span data-stu-id="46a6c-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="46a6c-115">У вас есть полный контроль над вашей модели данных, серверную бизнес-логики и уровень данных.</span><span class="sxs-lookup"><span data-stu-id="46a6c-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="46a6c-116">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="46a6c-116">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="46a6c-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="46a6c-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="46a6c-118">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="46a6c-118">Web API 2</span></span>
> - <span data-ttu-id="46a6c-119">OData версии 3</span><span class="sxs-lookup"><span data-stu-id="46a6c-119">OData Version 3</span></span>
> - <span data-ttu-id="46a6c-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="46a6c-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="46a6c-121">Fiddler Web отладки прокси-сервера (необязательно)</span><span class="sxs-lookup"><span data-stu-id="46a6c-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
> 
> <span data-ttu-id="46a6c-122">Поддержка API OData Web была добавлена в [ASP.NET и веб-средства 2012.2 обновление](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="46a6c-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="46a6c-123">Однако в этом учебнике используется формирование шаблонов, который был добавлен в Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="46a6c-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="46a6c-124">В этом учебнике вы создадите простой конечной точки OData, клиенты могут выполнять запросы.</span><span class="sxs-lookup"><span data-stu-id="46a6c-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="46a6c-125">Вы также создадите клиент C# для конечной точки.</span><span class="sxs-lookup"><span data-stu-id="46a6c-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="46a6c-126">После завершения этого учебника, следующий набор учебников показывают, как добавить дополнительную функциональность, включая отношения сущностей, действия, и выберите развернуть $ $.</span><span class="sxs-lookup"><span data-stu-id="46a6c-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="46a6c-127">Создание проекта Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46a6c-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="46a6c-128">Добавление модели сущностей</span><span class="sxs-lookup"><span data-stu-id="46a6c-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="46a6c-129">Добавить контроллер OData</span><span class="sxs-lookup"><span data-stu-id="46a6c-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="46a6c-130">Добавление модели EDM и маршрута</span><span class="sxs-lookup"><span data-stu-id="46a6c-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="46a6c-131">Начальное значение базы данных (необязательно)</span><span class="sxs-lookup"><span data-stu-id="46a6c-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="46a6c-132">Изучение конечной точки OData</span><span class="sxs-lookup"><span data-stu-id="46a6c-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="46a6c-133">Форматы сериализации OData</span><span class="sxs-lookup"><span data-stu-id="46a6c-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="46a6c-134">Создание проекта Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46a6c-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="46a6c-135">В этом учебнике создается конечная точка OData, которая поддерживает основные операции CRUD.</span><span class="sxs-lookup"><span data-stu-id="46a6c-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="46a6c-136">Конечная точка будет предоставлять один ресурс, список продуктов.</span><span class="sxs-lookup"><span data-stu-id="46a6c-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="46a6c-137">Учебники по более поздней версии будут добавлены дополнительные функции.</span><span class="sxs-lookup"><span data-stu-id="46a6c-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="46a6c-138">Запустите Visual Studio и выберите **новый проект** с начальной страницы.</span><span class="sxs-lookup"><span data-stu-id="46a6c-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="46a6c-139">Или из **файл** последовательно выберите пункты **New** и затем **проекта**.</span><span class="sxs-lookup"><span data-stu-id="46a6c-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="46a6c-140">В **шаблоны** выберите **установленные шаблоны** и разверните узел Visual C#.</span><span class="sxs-lookup"><span data-stu-id="46a6c-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="46a6c-141">В разделе **Visual C#**выберите **Web**.</span><span class="sxs-lookup"><span data-stu-id="46a6c-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="46a6c-142">Выберите **веб-приложение ASP.NET** шаблона.</span><span class="sxs-lookup"><span data-stu-id="46a6c-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="46a6c-143">В **новый проект ASP.NET** диалогового окна выберите **пустой** шаблона.</span><span class="sxs-lookup"><span data-stu-id="46a6c-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="46a6c-144">В разделе &quot;добавить папки и основные ссылки для... &quot;, проверьте **веб-API**.</span><span class="sxs-lookup"><span data-stu-id="46a6c-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="46a6c-145">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="46a6c-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="46a6c-146">Добавление модели сущностей</span><span class="sxs-lookup"><span data-stu-id="46a6c-146">Add an Entity Model</span></span>

<span data-ttu-id="46a6c-147">*Модель* — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="46a6c-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="46a6c-148">В этом учебнике мы должны модель, которая представляет продукт.</span><span class="sxs-lookup"><span data-stu-id="46a6c-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="46a6c-149">Модель соответствует нашей тип сущности OData.</span><span class="sxs-lookup"><span data-stu-id="46a6c-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="46a6c-150">В обозревателе решений щелкните правой кнопкой мыши папку модели.</span><span class="sxs-lookup"><span data-stu-id="46a6c-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="46a6c-151">В контекстном меню выберите **добавить** выберите **класса**.</span><span class="sxs-lookup"><span data-stu-id="46a6c-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="46a6c-152">В **добавить новое** товара диалогового окна, задайте имя для класса &quot;продукта&quot;.</span><span class="sxs-lookup"><span data-stu-id="46a6c-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="46a6c-153">По соглашению классы модели, помещаются в папку Models.</span><span class="sxs-lookup"><span data-stu-id="46a6c-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="46a6c-154">Нет необходимости соответствуют этому соглашению в собственных проектах, но оно будет использоваться для этого учебника.</span><span class="sxs-lookup"><span data-stu-id="46a6c-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="46a6c-155">В файле Product.cs добавьте следующее определение класса:</span><span class="sxs-lookup"><span data-stu-id="46a6c-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="46a6c-156">Свойство идентификатора будет ключ сущности.</span><span class="sxs-lookup"><span data-stu-id="46a6c-156">The ID property will be the entity key.</span></span> <span data-ttu-id="46a6c-157">Клиенты могут запрашивать продуктов по идентификатору.</span><span class="sxs-lookup"><span data-stu-id="46a6c-157">Clients can query products by ID.</span></span> <span data-ttu-id="46a6c-158">Это поле также будет первичного ключа в серверную базу данных.</span><span class="sxs-lookup"><span data-stu-id="46a6c-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="46a6c-159">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="46a6c-159">Build the project now.</span></span> <span data-ttu-id="46a6c-160">На следующем шаге мы будем использовать некоторые формирование шаблонов Visual Studio, который использует отражение, чтобы определить тип продукта.</span><span class="sxs-lookup"><span data-stu-id="46a6c-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="46a6c-161">Добавить контроллер OData</span><span class="sxs-lookup"><span data-stu-id="46a6c-161">Add an OData Controller</span></span>

<span data-ttu-id="46a6c-162">Объект *контроллера* — это класс, который обрабатывает HTTP-запросы.</span><span class="sxs-lookup"><span data-stu-id="46a6c-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="46a6c-163">Можно определить отдельные контроллера для каждого набора сущностей в службы OData.</span><span class="sxs-lookup"><span data-stu-id="46a6c-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="46a6c-164">В этом учебнике мы создадим одного контроллера.</span><span class="sxs-lookup"><span data-stu-id="46a6c-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="46a6c-165">В обозревателе решений щелкните правой кнопкой мыши папку Controllers.</span><span class="sxs-lookup"><span data-stu-id="46a6c-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="46a6c-166">Выберите **добавить** , а затем выберите **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="46a6c-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="46a6c-167">В **Добавление формирования шаблонов** диалогового окна выберите &quot;Web API 2 OData контроллер с действиями, использующий Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="46a6c-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="46a6c-168">В **добавить контроллер** диалога, имя контроллера «ProductsController».</span><span class="sxs-lookup"><span data-stu-id="46a6c-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="46a6c-169">Выберите &quot;использовать асинхронные действия контроллера&quot; флажок.</span><span class="sxs-lookup"><span data-stu-id="46a6c-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="46a6c-170">В **модель** раскрывающегося списка выберите класс продукта.</span><span class="sxs-lookup"><span data-stu-id="46a6c-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="46a6c-171">Нажмите кнопку **новый контекст данных...**  кнопки.</span><span class="sxs-lookup"><span data-stu-id="46a6c-171">Click the **New data context...** button.</span></span> <span data-ttu-id="46a6c-172">Оставьте имя по умолчанию для типа контекста данных и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="46a6c-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="46a6c-173">В диалоговом окне Добавить контроллер Добавление контроллера нажмите кнопку "Добавить".</span><span class="sxs-lookup"><span data-stu-id="46a6c-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="46a6c-174">Примечание: Если вы получаете сообщение об ошибке с сообщением, &quot;произошла ошибка при получении типа... &quot;, убедитесь, что построение проекта Visual Studio после добавления класса Product.</span><span class="sxs-lookup"><span data-stu-id="46a6c-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="46a6c-175">Формирование шаблонов использует отражение для поиска класса.</span><span class="sxs-lookup"><span data-stu-id="46a6c-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="46a6c-176">Формирование шаблонов добавляет в проект двух файлах кода:</span><span class="sxs-lookup"><span data-stu-id="46a6c-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="46a6c-177">Products.cs определяет контроллера веб-API, который реализует конечной точкой OData.</span><span class="sxs-lookup"><span data-stu-id="46a6c-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="46a6c-178">ProductServiceContext.cs предоставляет методы запросов к основной базе данных с помощью платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="46a6c-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="46a6c-179">Добавление модели EDM и маршрута</span><span class="sxs-lookup"><span data-stu-id="46a6c-179">Add the EDM and Route</span></span>

<span data-ttu-id="46a6c-180">В обозревателе решений разверните приложение\_запустите папку и откройте файл с именем WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="46a6c-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="46a6c-181">Этот класс содержит код конфигурации для веб-API.</span><span class="sxs-lookup"><span data-stu-id="46a6c-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="46a6c-182">Замените этот код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="46a6c-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="46a6c-183">Этот код выполняет следующие операции:</span><span class="sxs-lookup"><span data-stu-id="46a6c-183">This code does two things:</span></span>

- <span data-ttu-id="46a6c-184">Создает модель данных сущности (EDM) для конечной точки OData.</span><span class="sxs-lookup"><span data-stu-id="46a6c-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="46a6c-185">Добавляет маршрут для конечной точки.</span><span class="sxs-lookup"><span data-stu-id="46a6c-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="46a6c-186">EDM-это абстрактный модель данных.</span><span class="sxs-lookup"><span data-stu-id="46a6c-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="46a6c-187">Модель EDM используется для создания документов метаданных и определения URL-адреса для службы.</span><span class="sxs-lookup"><span data-stu-id="46a6c-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="46a6c-188">**ODataConventionModelBuilder** создает EDM с помощью набора соглашения об именовании по умолчанию модели EDM.</span><span class="sxs-lookup"><span data-stu-id="46a6c-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="46a6c-189">Этот подход требует наименьшего объема кода.</span><span class="sxs-lookup"><span data-stu-id="46a6c-189">This approach requires the least code.</span></span> <span data-ttu-id="46a6c-190">Если требуется больший контроль над модели EDM, можно использовать **ODataModelBuilder** класса для создания модели EDM, добавив явным образом свойства, ключи и свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="46a6c-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="46a6c-191">**EntitySet** метод добавляет к модели EDM набор сущностей:</span><span class="sxs-lookup"><span data-stu-id="46a6c-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="46a6c-192">Строка «Продукты» определяет имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="46a6c-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="46a6c-193">Имя контроллера, должно соответствовать имени набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="46a6c-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="46a6c-194">В этом учебнике набор сущностей, который называется «Продукты», именем контроллера `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="46a6c-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="46a6c-195">Если вы присвоили «ProductSet» набор сущностей, будет назван контроллера `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="46a6c-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="46a6c-196">Обратите внимание, что конечная точка может иметь несколько наборов сущностей.</span><span class="sxs-lookup"><span data-stu-id="46a6c-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="46a6c-197">Вызовите **EntitySet&lt;T&gt;**  для каждой сущности значение, а затем определите соответствующий контроллер.</span><span class="sxs-lookup"><span data-stu-id="46a6c-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="46a6c-198">**MapODataRoute** метод добавляет маршрут для конечной точки OData.</span><span class="sxs-lookup"><span data-stu-id="46a6c-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="46a6c-199">Первый параметр — понятное имя для маршрута.</span><span class="sxs-lookup"><span data-stu-id="46a6c-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="46a6c-200">Клиенты службы, это имя не отображается.</span><span class="sxs-lookup"><span data-stu-id="46a6c-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="46a6c-201">Второй параметр — префикс URI для конечной точки.</span><span class="sxs-lookup"><span data-stu-id="46a6c-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="46a6c-202">Этот код, URI для набора сущностей Products задан http://<em>hostname</em>  /odata и продуктов.</span><span class="sxs-lookup"><span data-stu-id="46a6c-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="46a6c-203">Приложение может иметь более одной конечной точки OData.</span><span class="sxs-lookup"><span data-stu-id="46a6c-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="46a6c-204">Для каждой конечной точки, вызовите <strong>MapODataRoute</strong> и укажите имя уникального маршрута и уникальный префикс URI.</span><span class="sxs-lookup"><span data-stu-id="46a6c-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="46a6c-205">Начальное значение базы данных (необязательно)</span><span class="sxs-lookup"><span data-stu-id="46a6c-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="46a6c-206">На этом шаге используется Entity Framework в качестве начального базы данных с некоторые тестовые данные.</span><span class="sxs-lookup"><span data-stu-id="46a6c-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="46a6c-207">Этот шаг необязателен, но он позволяет немедленно тестирование конечной точки OData.</span><span class="sxs-lookup"><span data-stu-id="46a6c-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="46a6c-208">Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="46a6c-208">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="46a6c-209">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="46a6c-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="46a6c-210">Это добавляет папку с именем миграция и файл кода с именем Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="46a6c-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="46a6c-211">Откройте этот файл и добавьте следующий код в `Configuration.Seed` метод.</span><span class="sxs-lookup"><span data-stu-id="46a6c-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="46a6c-212">В окне консоли диспетчера пакетов введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="46a6c-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="46a6c-213">Эти команды создают код, создает базу данных, а затем выполняет этот код.</span><span class="sxs-lookup"><span data-stu-id="46a6c-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="46a6c-214">Изучение конечной точки OData</span><span class="sxs-lookup"><span data-stu-id="46a6c-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="46a6c-215">В этом разделе мы будем использовать [отладки веб-прокси Fiddler](http://www.fiddler2.com) для отправки запросов к конечной точке и изучения ответные сообщения.</span><span class="sxs-lookup"><span data-stu-id="46a6c-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="46a6c-216">Это поможет вам понять возможности конечной точки OData.</span><span class="sxs-lookup"><span data-stu-id="46a6c-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="46a6c-217">В Visual Studio нажмите клавишу F5 для запуска отладки.</span><span class="sxs-lookup"><span data-stu-id="46a6c-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="46a6c-218">По умолчанию Visual Studio открывает в браузере `http://localhost:*port*`, где *порт* — номер порта, настроенного в параметрах проекта.</span><span class="sxs-lookup"><span data-stu-id="46a6c-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="46a6c-219">Можно изменить номер порта в параметрах проекта.</span><span class="sxs-lookup"><span data-stu-id="46a6c-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="46a6c-220">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="46a6c-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="46a6c-221">В окне «Свойства» выберите **Web**.</span><span class="sxs-lookup"><span data-stu-id="46a6c-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="46a6c-222">Введите номер порта в **URL-адрес проекта**.</span><span class="sxs-lookup"><span data-stu-id="46a6c-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="46a6c-223">Сервисный документ</span><span class="sxs-lookup"><span data-stu-id="46a6c-223">Service Document</span></span>

<span data-ttu-id="46a6c-224">*Сервисный документ* содержит список наборов сущностей для конечной точки OData.</span><span class="sxs-lookup"><span data-stu-id="46a6c-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="46a6c-225">Чтобы получить Сервисный документ, отправка запроса GET для корневой URI службы.</span><span class="sxs-lookup"><span data-stu-id="46a6c-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="46a6c-226">С помощью Fiddler, введите следующий URI в **Composer** вкладка: `http://localhost:port/odata/`, где *порт* — номер порта.</span><span class="sxs-lookup"><span data-stu-id="46a6c-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="46a6c-227">Нажмите кнопку **Execute** кнопки.</span><span class="sxs-lookup"><span data-stu-id="46a6c-227">Click the **Execute** button.</span></span> <span data-ttu-id="46a6c-228">Fiddler приложение отправляет запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="46a6c-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="46a6c-229">Должны появиться в списке веб-сеансами ответа.</span><span class="sxs-lookup"><span data-stu-id="46a6c-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="46a6c-230">Если все работает, код состояния будет 200.</span><span class="sxs-lookup"><span data-stu-id="46a6c-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="46a6c-231">Дважды щелкните в списке веб-сеансами, чтобы просмотреть сведения о ответное сообщение на вкладке инспекторы ответа.</span><span class="sxs-lookup"><span data-stu-id="46a6c-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="46a6c-232">Необработанные сообщения HTTP-ответа должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="46a6c-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="46a6c-233">По умолчанию веб-API возвращает Сервисный документ в формате AtomPub.</span><span class="sxs-lookup"><span data-stu-id="46a6c-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="46a6c-234">Чтобы запросить JSON, добавьте следующий заголовок HTTP-запроса:</span><span class="sxs-lookup"><span data-stu-id="46a6c-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="46a6c-235">Теперь в HTTP-ответе содержит полезные данные JSON:</span><span class="sxs-lookup"><span data-stu-id="46a6c-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="46a6c-236">Документ метаданных службы</span><span class="sxs-lookup"><span data-stu-id="46a6c-236">Service Metadata Document</span></span>

<span data-ttu-id="46a6c-237">*Документ метаданных службы* описывается модель данных службы на языке XML вызывается язык определения концептуальной схемы (CSDL).</span><span class="sxs-lookup"><span data-stu-id="46a6c-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="46a6c-238">Документ метаданных показана структура данных в службе и может использоваться для создания кода клиента.</span><span class="sxs-lookup"><span data-stu-id="46a6c-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="46a6c-239">Чтобы получить документ метаданных, отправка запроса GET к `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="46a6c-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="46a6c-240">Вот метаданных для конечной точки, показанными в данном руководстве.</span><span class="sxs-lookup"><span data-stu-id="46a6c-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="46a6c-241">Набор сущностей</span><span class="sxs-lookup"><span data-stu-id="46a6c-241">Entity Set</span></span>

<span data-ttu-id="46a6c-242">Для получения набора сущностей Products, отправьте запрос GET к `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="46a6c-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="46a6c-243">Объект</span><span class="sxs-lookup"><span data-stu-id="46a6c-243">Entity</span></span>

<span data-ttu-id="46a6c-244">Для получения конкретного продукта, отправьте запрос GET к `http://localhost:port/odata/Products(1)`, где «1» — идентификатор продукта.</span><span class="sxs-lookup"><span data-stu-id="46a6c-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="46a6c-245">Форматы сериализации OData</span><span class="sxs-lookup"><span data-stu-id="46a6c-245">OData Serialization Formats</span></span>

<span data-ttu-id="46a6c-246">OData поддерживает несколько форматов сериализации:</span><span class="sxs-lookup"><span data-stu-id="46a6c-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="46a6c-247">Публикации Atom (XML)</span><span class="sxs-lookup"><span data-stu-id="46a6c-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="46a6c-248">JSON «light» (появился в OData v3)</span><span class="sxs-lookup"><span data-stu-id="46a6c-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="46a6c-249">JSON «verbose» (OData v2)</span><span class="sxs-lookup"><span data-stu-id="46a6c-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="46a6c-250">По умолчанию веб-API в формате AtomPubJSON «light».</span><span class="sxs-lookup"><span data-stu-id="46a6c-250">By default, Web API uses AtomPubJSON "light" format.</span></span> 

<span data-ttu-id="46a6c-251">Чтобы получить формат AtomPub, задайте заголовок Accept значение «application/atom + xml».</span><span class="sxs-lookup"><span data-stu-id="46a6c-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="46a6c-252">Ниже приведен пример текста ответа.</span><span class="sxs-lookup"><span data-stu-id="46a6c-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="46a6c-253">Вы увидите одним из очевидных недостатков формат Atom: это гораздо более подробным по сравнению JSON light.</span><span class="sxs-lookup"><span data-stu-id="46a6c-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="46a6c-254">Тем не менее если у вас есть клиент, который понимает AtomPub, клиент может предпочтение этого формата JSON.</span><span class="sxs-lookup"><span data-stu-id="46a6c-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="46a6c-255">Вот JSON облегченную версию той же сущности.</span><span class="sxs-lookup"><span data-stu-id="46a6c-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="46a6c-256">Формат JSON light появилась в версии 3 протокола OData.</span><span class="sxs-lookup"><span data-stu-id="46a6c-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="46a6c-257">Для обеспечения обратной совместимости клиент может запрашивать старый формат JSON «verbose».</span><span class="sxs-lookup"><span data-stu-id="46a6c-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="46a6c-258">Запрос verbose JSON, задайте для заголовка Accept `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="46a6c-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="46a6c-259">Ниже приведен подробный версии.</span><span class="sxs-lookup"><span data-stu-id="46a6c-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="46a6c-260">Этот формат передает дополнительные метаданные в текст ответа, который можно добавить значительную нагрузку на весь сеанс.</span><span class="sxs-lookup"><span data-stu-id="46a6c-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="46a6c-261">Кроме того он добавляет уровень косвенности, заключив объект в свойство с именем «d».</span><span class="sxs-lookup"><span data-stu-id="46a6c-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="46a6c-262">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="46a6c-262">Next Steps</span></span>

- [<span data-ttu-id="46a6c-263">Добавление связей сущностей</span><span class="sxs-lookup"><span data-stu-id="46a6c-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="46a6c-264">Добавление действия OData</span><span class="sxs-lookup"><span data-stu-id="46a6c-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="46a6c-265">Вызов службы OData из клиента .NET</span><span class="sxs-lookup"><span data-stu-id="46a6c-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
