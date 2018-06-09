---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Включение операций CRUD в ASP.NET Web API 1 | Документы Microsoft
author: MikeWasson
description: Этот учебник показывает, как для поддержки операций CRUD в службы HTTP с помощью веб-API ASP.NET. Версии программного обеспечения, используемые в учебник Visual Studio 2012 Web точки доступа...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 69b7d5453b6ff36d6e28a69428b016cb8cfd06e9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "29153012"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="1d410-104">Включение операций CRUD в ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="1d410-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="1d410-105">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1d410-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="1d410-106">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="1d410-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="1d410-107">Этот учебник показывает, как для поддержки операций CRUD в службы HTTP с помощью веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1d410-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1d410-108">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="1d410-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1d410-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="1d410-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="1d410-110">Веб-API 1 (также работает с веб-API 2)</span><span class="sxs-lookup"><span data-stu-id="1d410-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="1d410-111">Обозначает CRUD &quot;создания, чтения, обновления и удаления,&quot; эти четыре основные операции.</span><span class="sxs-lookup"><span data-stu-id="1d410-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="1d410-112">Многие службы HTTP также модели операций CRUD через REST или API REST like.</span><span class="sxs-lookup"><span data-stu-id="1d410-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="1d410-113">В этом учебнике будет создание очень простого веб-API для управления списком продуктов.</span><span class="sxs-lookup"><span data-stu-id="1d410-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="1d410-114">Каждого продукта будет содержать имя, цену и категории (такие как &quot;toys&quot; или &quot;оборудования&quot;), а также идентификатора продукта.</span><span class="sxs-lookup"><span data-stu-id="1d410-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="1d410-115">Продукты API предоставляет следующие методы.</span><span class="sxs-lookup"><span data-stu-id="1d410-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="1d410-116">Действие</span><span class="sxs-lookup"><span data-stu-id="1d410-116">Action</span></span> | <span data-ttu-id="1d410-117">Метод HTTP</span><span class="sxs-lookup"><span data-stu-id="1d410-117">HTTP method</span></span> | <span data-ttu-id="1d410-118">Относительный URI</span><span class="sxs-lookup"><span data-stu-id="1d410-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d410-119">Получить список всех продуктов</span><span class="sxs-lookup"><span data-stu-id="1d410-119">Get a list of all products</span></span> | <span data-ttu-id="1d410-120">GET</span><span class="sxs-lookup"><span data-stu-id="1d410-120">GET</span></span> | <span data-ttu-id="1d410-121">/ api/продуктов</span><span class="sxs-lookup"><span data-stu-id="1d410-121">/api/products</span></span> |
| <span data-ttu-id="1d410-122">Получение продукта по Идентификатору</span><span class="sxs-lookup"><span data-stu-id="1d410-122">Get a product by ID</span></span> | <span data-ttu-id="1d410-123">GET</span><span class="sxs-lookup"><span data-stu-id="1d410-123">GET</span></span> | <span data-ttu-id="1d410-124">/API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="1d410-124">/api/products/*id*</span></span> |
| <span data-ttu-id="1d410-125">Получение продукта по категориям</span><span class="sxs-lookup"><span data-stu-id="1d410-125">Get a product by category</span></span> | <span data-ttu-id="1d410-126">GET</span><span class="sxs-lookup"><span data-stu-id="1d410-126">GET</span></span> | <span data-ttu-id="1d410-127">/ api/продуктов? категории =*категории*</span><span class="sxs-lookup"><span data-stu-id="1d410-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="1d410-128">Создать продукт</span><span class="sxs-lookup"><span data-stu-id="1d410-128">Create a new product</span></span> | <span data-ttu-id="1d410-129">ПОМЕСТИТЬ</span><span class="sxs-lookup"><span data-stu-id="1d410-129">POST</span></span> | <span data-ttu-id="1d410-130">/ api/продуктов</span><span class="sxs-lookup"><span data-stu-id="1d410-130">/api/products</span></span> |
| <span data-ttu-id="1d410-131">Обновления продукта</span><span class="sxs-lookup"><span data-stu-id="1d410-131">Update a product</span></span> | <span data-ttu-id="1d410-132">PUT</span><span class="sxs-lookup"><span data-stu-id="1d410-132">PUT</span></span> | <span data-ttu-id="1d410-133">/API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="1d410-133">/api/products/*id*</span></span> |
| <span data-ttu-id="1d410-134">Удаление продукта</span><span class="sxs-lookup"><span data-stu-id="1d410-134">Delete a product</span></span> | <span data-ttu-id="1d410-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="1d410-135">DELETE</span></span> | <span data-ttu-id="1d410-136">/API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="1d410-136">/api/products/*id*</span></span> |

<span data-ttu-id="1d410-137">Обратите внимание, что часть URL-адреса, включают идентификатор продукта в пути.</span><span class="sxs-lookup"><span data-stu-id="1d410-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="1d410-138">Например, чтобы получить продукта, идентификатор которого — 28, клиент отправляет запрос GET `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="1d410-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="1d410-139">Ресурсы</span><span class="sxs-lookup"><span data-stu-id="1d410-139">Resources</span></span>

<span data-ttu-id="1d410-140">Продукты API определяет идентификаторы URI для двух типов ресурсов:</span><span class="sxs-lookup"><span data-stu-id="1d410-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="1d410-141">Ресурс</span><span class="sxs-lookup"><span data-stu-id="1d410-141">Resource</span></span> | <span data-ttu-id="1d410-142">URI</span><span class="sxs-lookup"><span data-stu-id="1d410-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="1d410-143">Список всех продуктов.</span><span class="sxs-lookup"><span data-stu-id="1d410-143">The list of all the products.</span></span> | <span data-ttu-id="1d410-144">/ api/продуктов</span><span class="sxs-lookup"><span data-stu-id="1d410-144">/api/products</span></span> |
| <span data-ttu-id="1d410-145">Отдельного продукта.</span><span class="sxs-lookup"><span data-stu-id="1d410-145">An individual product.</span></span> | <span data-ttu-id="1d410-146">/API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="1d410-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="1d410-147">Методы</span><span class="sxs-lookup"><span data-stu-id="1d410-147">Methods</span></span>

<span data-ttu-id="1d410-148">Четыре основных метода HTTP (GET, PUT, POST и DELETE) может быть сопоставлен операций CRUD следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1d410-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="1d410-149">GET возвращает представление ресурса в указанный универсальный код Ресурса.</span><span class="sxs-lookup"><span data-stu-id="1d410-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="1d410-150">GET должен не имеют побочных эффектов на сервере.</span><span class="sxs-lookup"><span data-stu-id="1d410-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="1d410-151">PUT обновляет ресурс с указанным URI.</span><span class="sxs-lookup"><span data-stu-id="1d410-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="1d410-152">PUT может также использоваться для создания нового ресурса с указанным URI, если сервер разрешает клиентам задавать новые URI.</span><span class="sxs-lookup"><span data-stu-id="1d410-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="1d410-153">В этом учебнике API не поддерживают создание с помощью PUT.</span><span class="sxs-lookup"><span data-stu-id="1d410-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="1d410-154">POST создает новый ресурс.</span><span class="sxs-lookup"><span data-stu-id="1d410-154">POST creates a new resource.</span></span> <span data-ttu-id="1d410-155">Сервер назначает URI нового объекта и возвращает этот URI как часть ответного сообщения.</span><span class="sxs-lookup"><span data-stu-id="1d410-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="1d410-156">DELETE удаляет ресурс с указанным URI.</span><span class="sxs-lookup"><span data-stu-id="1d410-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="1d410-157">Примечание: Метод PUT заменяет сущность всего продукта.</span><span class="sxs-lookup"><span data-stu-id="1d410-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="1d410-158">То есть клиент ожидается отправки полное представление обновленного продукта.</span><span class="sxs-lookup"><span data-stu-id="1d410-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="1d410-159">Если вы хотите поддерживать частичных обновлений, метод PATCH является предпочтительным.</span><span class="sxs-lookup"><span data-stu-id="1d410-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="1d410-160">Этот учебник не реализует ИСПРАВЛЕНИЯ.</span><span class="sxs-lookup"><span data-stu-id="1d410-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="1d410-161">Создайте новый проект Web API</span><span class="sxs-lookup"><span data-stu-id="1d410-161">Create a New Web API Project</span></span>

<span data-ttu-id="1d410-162">Запустить Visual Studio и выберите **новый проект** из **запустить** страницы.</span><span class="sxs-lookup"><span data-stu-id="1d410-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="1d410-163">Или из **файл** последовательно выберите пункты **New** и затем **проекта**.</span><span class="sxs-lookup"><span data-stu-id="1d410-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="1d410-164">В **шаблоны** выберите **установленные шаблоны** и разверните **Visual C#** узла.</span><span class="sxs-lookup"><span data-stu-id="1d410-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="1d410-165">В разделе **Visual C#** выберите **Web**.</span><span class="sxs-lookup"><span data-stu-id="1d410-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="1d410-166">В списке шаблонов проектов выберите **веб-приложение ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="1d410-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="1d410-167">Назовите проект &quot;ProductStore&quot; и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1d410-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="1d410-168">В **нового проекта ASP.NET MVC 4** диалогового окна выберите **веб-API** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1d410-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="1d410-169">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="1d410-169">Adding a Model</span></span>

<span data-ttu-id="1d410-170">*Модель* — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="1d410-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="1d410-171">В веб-API ASP.NET строго типизированными объектами CLR можно использовать в качестве модели, и они будут автоматически сериализованы в XML или JSON для клиента.</span><span class="sxs-lookup"><span data-stu-id="1d410-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="1d410-172">Для ProductStore API состоит наши данные продуктов, поэтому мы создадим новый класс с именем `Product`.</span><span class="sxs-lookup"><span data-stu-id="1d410-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="1d410-173">Если обозреватель решений не отображается, нажмите кнопку **представление** и выбрать пункт **обозревателе решений**.</span><span class="sxs-lookup"><span data-stu-id="1d410-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="1d410-174">В обозревателе решений щелкните правой кнопкой мыши **моделей** папки.</span><span class="sxs-lookup"><span data-stu-id="1d410-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="1d410-175">Meny контекста, выберите **добавить**, а затем выберите **класса**.</span><span class="sxs-lookup"><span data-stu-id="1d410-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="1d410-176">Имя класса &quot;продукта&quot;.</span><span class="sxs-lookup"><span data-stu-id="1d410-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="1d410-177">Добавьте следующие свойства для `Product` класса.</span><span class="sxs-lookup"><span data-stu-id="1d410-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="1d410-178">Добавление репозитория</span><span class="sxs-lookup"><span data-stu-id="1d410-178">Adding a Repository</span></span>

<span data-ttu-id="1d410-179">Необходимо хранить в коллекции продуктов.</span><span class="sxs-lookup"><span data-stu-id="1d410-179">We need to store a collection of products.</span></span> <span data-ttu-id="1d410-180">Это хороший способ разделения коллекции из реализации служб.</span><span class="sxs-lookup"><span data-stu-id="1d410-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="1d410-181">Таким образом, мы можем изменить резервного хранилища без перезаписи класс службы.</span><span class="sxs-lookup"><span data-stu-id="1d410-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="1d410-182">Этот тип конструктора называется *репозитория* шаблон.</span><span class="sxs-lookup"><span data-stu-id="1d410-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="1d410-183">Начинается с определения универсальный интерфейс для репозитория.</span><span class="sxs-lookup"><span data-stu-id="1d410-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="1d410-184">В обозревателе решений щелкните правой кнопкой мыши **моделей** папки.</span><span class="sxs-lookup"><span data-stu-id="1d410-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="1d410-185">Выберите **добавить**, а затем выберите **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="1d410-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="1d410-186">В **шаблоны** выберите **установленные шаблоны** и разверните узел C#.</span><span class="sxs-lookup"><span data-stu-id="1d410-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="1d410-187">В C#, выберите **кода**.</span><span class="sxs-lookup"><span data-stu-id="1d410-187">Under C#, select **Code**.</span></span> <span data-ttu-id="1d410-188">В списке шаблонов кода, выберите **интерфейс**.</span><span class="sxs-lookup"><span data-stu-id="1d410-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="1d410-189">Назовите этот интерфейс &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="1d410-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="1d410-190">Добавьте реализацию следующего:</span><span class="sxs-lookup"><span data-stu-id="1d410-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="1d410-191">Теперь добавьте еще один класс в папке «модели», с именем &quot;ProductRepository.&quot; Этот класс реализует интерфейс `IProductRespository`.</span><span class="sxs-lookup"><span data-stu-id="1d410-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="1d410-192">Добавьте реализацию следующего:</span><span class="sxs-lookup"><span data-stu-id="1d410-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="1d410-193">Репозитории сохраняет список в локальной памяти.</span><span class="sxs-lookup"><span data-stu-id="1d410-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="1d410-194">Это ОК учебник, но в реальном приложении будут храниться данные извне, либо базы данных или в Облачное хранилище.</span><span class="sxs-lookup"><span data-stu-id="1d410-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="1d410-195">Шаблон репозитория поможет упростить его изменить реализацию более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="1d410-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="1d410-196">Добавление контроллера Web API</span><span class="sxs-lookup"><span data-stu-id="1d410-196">Adding a Web API Controller</span></span>

<span data-ttu-id="1d410-197">Если вы работали с ASP.NET MVC, затем вы уже знакомы с контроллерами.</span><span class="sxs-lookup"><span data-stu-id="1d410-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="1d410-198">В ASP.NET Web API *контроллера* — это класс, который обрабатывает HTTP-запросы от клиента.</span><span class="sxs-lookup"><span data-stu-id="1d410-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="1d410-199">При создании проекта мастером создания проекта автоматически создается два контроллера.</span><span class="sxs-lookup"><span data-stu-id="1d410-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="1d410-200">Чтобы увидеть их, разверните папку Controllers в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="1d410-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="1d410-201">HomeController — это традиционные контроллер ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1d410-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="1d410-202">Он отвечает за обслуживающий HTML-страниц для сайта и не связаны непосредственно с нашими веб-API.</span><span class="sxs-lookup"><span data-stu-id="1d410-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="1d410-203">ValuesController является контроллера WebAPI примере.</span><span class="sxs-lookup"><span data-stu-id="1d410-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="1d410-204">Продолжить и удалить ValuesController, дважды щелкнув файл в обозревателе решений и выбрав **удалить.**</span><span class="sxs-lookup"><span data-stu-id="1d410-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="1d410-205">Добавьте новый контроллер, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1d410-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="1d410-206">В **обозревателе решений**, щелкните правой кнопкой мыши папку Controllers.</span><span class="sxs-lookup"><span data-stu-id="1d410-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="1d410-207">Выберите **добавить** , а затем выберите **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="1d410-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="1d410-208">В **добавления контроллера** мастера, имя контроллера &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="1d410-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="1d410-209">В **шаблона** раскрывающемся списке выберите **пустой контроллер API**.</span><span class="sxs-lookup"><span data-stu-id="1d410-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="1d410-210">Затем нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="1d410-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="1d410-211">Поместить в папку с именем контроллеров вашей contollers необязательно.</span><span class="sxs-lookup"><span data-stu-id="1d410-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="1d410-212">Имя папки не имеет значения; Это просто удобный способ организации исходные файлы.</span><span class="sxs-lookup"><span data-stu-id="1d410-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="1d410-213">**Добавить контроллер** мастер создаст файл с именем ProductsController.cs в папке Controllers находится.</span><span class="sxs-lookup"><span data-stu-id="1d410-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="1d410-214">Если этот файл уже не открыт, дважды щелкните файл, чтобы открыть его.</span><span class="sxs-lookup"><span data-stu-id="1d410-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="1d410-215">Добавьте следующие **с помощью** инструкции:</span><span class="sxs-lookup"><span data-stu-id="1d410-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="1d410-216">Добавьте поле, которое хранит **IProductRepository** экземпляра.</span><span class="sxs-lookup"><span data-stu-id="1d410-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="1d410-217">Вызов `new ProductRepository()` в контроллере не лучший проектированию, привязывает контроллера для данного экземпляра `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="1d410-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="1d410-218">Более удачным подходом в разделе [с помощью сопоставителя зависимостей Web API](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="1d410-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="1d410-219">Получение сведений о ресурсе</span><span class="sxs-lookup"><span data-stu-id="1d410-219">Getting a Resource</span></span>

<span data-ttu-id="1d410-220">Предоставляет несколько ProductStore API &quot;чтения&quot; действия, как методы HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="1d410-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="1d410-221">Каждое действие будет соответствовать методу в `ProductsController` класса.</span><span class="sxs-lookup"><span data-stu-id="1d410-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="1d410-222">Действие</span><span class="sxs-lookup"><span data-stu-id="1d410-222">Action</span></span> | <span data-ttu-id="1d410-223">Метод HTTP</span><span class="sxs-lookup"><span data-stu-id="1d410-223">HTTP method</span></span> | <span data-ttu-id="1d410-224">Относительный URI</span><span class="sxs-lookup"><span data-stu-id="1d410-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d410-225">Получить список всех продуктов</span><span class="sxs-lookup"><span data-stu-id="1d410-225">Get a list of all products</span></span> | <span data-ttu-id="1d410-226">GET</span><span class="sxs-lookup"><span data-stu-id="1d410-226">GET</span></span> | <span data-ttu-id="1d410-227">/ api/продуктов</span><span class="sxs-lookup"><span data-stu-id="1d410-227">/api/products</span></span> |
| <span data-ttu-id="1d410-228">Получение продукта по Идентификатору</span><span class="sxs-lookup"><span data-stu-id="1d410-228">Get a product by ID</span></span> | <span data-ttu-id="1d410-229">GET</span><span class="sxs-lookup"><span data-stu-id="1d410-229">GET</span></span> | <span data-ttu-id="1d410-230">/API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="1d410-230">/api/products/*id*</span></span> |
| <span data-ttu-id="1d410-231">Получение продукта по категориям</span><span class="sxs-lookup"><span data-stu-id="1d410-231">Get a product by category</span></span> | <span data-ttu-id="1d410-232">GET</span><span class="sxs-lookup"><span data-stu-id="1d410-232">GET</span></span> | <span data-ttu-id="1d410-233">/ api/продуктов? категории =*категории*</span><span class="sxs-lookup"><span data-stu-id="1d410-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="1d410-234">Чтобы получить список всех продуктов, добавьте этот метод `ProductsController` класса:</span><span class="sxs-lookup"><span data-stu-id="1d410-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="1d410-235">Имя метода начинается с &quot;получить&quot;, поэтому по соглашению оно сопоставлено запросов GET.</span><span class="sxs-lookup"><span data-stu-id="1d410-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="1d410-236">Кроме того, поскольку метод не имеет параметров, он был сопоставлен URI, который не содержит *&quot;идентификатор&quot;* сегмента пути.</span><span class="sxs-lookup"><span data-stu-id="1d410-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="1d410-237">Чтобы получить продукта по Идентификатору, добавьте этот метод, чтобы `ProductsController` класса:</span><span class="sxs-lookup"><span data-stu-id="1d410-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="1d410-238">Этот метод также с названиями &quot;получить&quot;, но метод имеет параметр с именем *идентификатор*. Этот параметр сопоставляется &quot;идентификатор&quot; сегмент пути URI.</span><span class="sxs-lookup"><span data-stu-id="1d410-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="1d410-239">Платформа веб-API ASP.NET автоматически преобразует идентификатор в правильный тип данных (**int**) для параметра.</span><span class="sxs-lookup"><span data-stu-id="1d410-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="1d410-240">Метод GetProduct вызывает исключение типа **HttpResponseException** Если *идентификатор* является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="1d410-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="1d410-241">Это исключение платформой будут преобразовываться в ошибку 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="1d410-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="1d410-242">Наконец добавьте метод для поиска продуктов по категориям:</span><span class="sxs-lookup"><span data-stu-id="1d410-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="1d410-243">Если URI запроса содержит строку запроса, веб-API пытается сопоставить параметры запроса для параметров метода контроллера.</span><span class="sxs-lookup"><span data-stu-id="1d410-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="1d410-244">Таким образом, URI вида «api и продукты? категории =*категории*"будет сопоставлен этот метод.</span><span class="sxs-lookup"><span data-stu-id="1d410-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="1d410-245">Создание ресурса</span><span class="sxs-lookup"><span data-stu-id="1d410-245">Creating a Resource</span></span>

<span data-ttu-id="1d410-246">Далее мы добавим метод `ProductsController` класса, чтобы создать новый продукт.</span><span class="sxs-lookup"><span data-stu-id="1d410-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="1d410-247">Ниже приведен простой реализации метода.</span><span class="sxs-lookup"><span data-stu-id="1d410-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="1d410-248">Обратите внимание два аспекта этот метод.</span><span class="sxs-lookup"><span data-stu-id="1d410-248">Note two things about this method:</span></span>

- <span data-ttu-id="1d410-249">Имя метода начинается с &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="1d410-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="1d410-250">Чтобы создать новый продукт, клиент отправляет запрос HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="1d410-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="1d410-251">Метод принимает параметр типа Product.</span><span class="sxs-lookup"><span data-stu-id="1d410-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="1d410-252">В веб-API десериализации параметров со сложными типами, в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="1d410-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="1d410-253">Таким образом ожидается, что клиенту отправлять сериализованное представление объекта продукта в формате XML или JSON.</span><span class="sxs-lookup"><span data-stu-id="1d410-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="1d410-254">Эта реализация будет работать, но не является неполной.</span><span class="sxs-lookup"><span data-stu-id="1d410-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="1d410-255">В идеальном случае мы хотели бы HTTP-ответа, относятся следующие:</span><span class="sxs-lookup"><span data-stu-id="1d410-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="1d410-256">**Код ответа:** по умолчанию платформа веб-API задает код состояния ответа 200 (ОК).</span><span class="sxs-lookup"><span data-stu-id="1d410-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="1d410-257">Но по протоколу HTTP/1.1, когда запрос POST приводит к созданию ресурса, сервера следует ответить с состоянием 201 (создано).</span><span class="sxs-lookup"><span data-stu-id="1d410-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="1d410-258">**Расположение:** при сервер создает ресурс, он должен содержать URI нового ресурса в заголовке Location ответа.</span><span class="sxs-lookup"><span data-stu-id="1d410-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="1d410-259">Веб-API ASP.NET позволяет легко управлять сообщения HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="1d410-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="1d410-260">Вот улучшенную реализацию.</span><span class="sxs-lookup"><span data-stu-id="1d410-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="1d410-261">Обратите внимание, что тип возвращаемого значения метода **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="1d410-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="1d410-262">Путем возвращения **HttpResponseMessage** вместо продукта, можно управлять сведения сообщения HTTP-ответа, включая код состояния и заголовок расположения.</span><span class="sxs-lookup"><span data-stu-id="1d410-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="1d410-263">**CreateResponse** метод создает **HttpResponseMessage** и автоматически записывает сериализованное представление объекта продукта в тексте fo ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="1d410-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="1d410-264">В этом примере не проверяет `Product`.</span><span class="sxs-lookup"><span data-stu-id="1d410-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="1d410-265">Сведения о проверке модели см. в разделе [проверки модели в ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="1d410-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="1d410-266">Обновление ресурса</span><span class="sxs-lookup"><span data-stu-id="1d410-266">Updating a Resource</span></span>

<span data-ttu-id="1d410-267">Обновление продукта с помощью метода PUT проста:</span><span class="sxs-lookup"><span data-stu-id="1d410-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="1d410-268">Имя метода начинается с &quot;поместить... &quot;, поэтому веб-API сопоставляет его запросы PUT.</span><span class="sxs-lookup"><span data-stu-id="1d410-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="1d410-269">Этот метод принимает два параметра код продукта и обновленного продукта.</span><span class="sxs-lookup"><span data-stu-id="1d410-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="1d410-270">*Идентификатор* параметра берется из пути URI и *продукта* параметр десериализуется в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="1d410-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="1d410-271">По умолчанию платформа ASP.NET Web API принимает простые типы параметров маршрута и сложных типов в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="1d410-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="1d410-272">Удаление ресурса</span><span class="sxs-lookup"><span data-stu-id="1d410-272">Deleting a Resource</span></span>

<span data-ttu-id="1d410-273">Чтобы удалить resourse, определите метод «Удалить...».</span><span class="sxs-lookup"><span data-stu-id="1d410-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="1d410-274">Если запрос DELETE завершается успешно, он может возвращать состояния 200 (ОК) с тело сущности, описывающая состояние; состояние 202 (принято), если удаление по-прежнему ожидающие; или состояния 204 (нет содержимого) без тела сущности.</span><span class="sxs-lookup"><span data-stu-id="1d410-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="1d410-275">В этом случае `DeleteProduct` имеет метод `void` тип возвращаемого значения, поэтому веб-API ASP.NET автоматически преобразует это в состоянии код 204 (нет содержимого).</span><span class="sxs-lookup"><span data-stu-id="1d410-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
