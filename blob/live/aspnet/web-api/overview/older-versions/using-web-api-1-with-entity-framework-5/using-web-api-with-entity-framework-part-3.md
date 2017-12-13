---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: "Часть 3: Создание контроллера Admin | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 6fadfb6e96ae287fc5f81516b7535e03853c7e6a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="a7d93-102">Часть 3: Создание контроллера администратора</span><span class="sxs-lookup"><span data-stu-id="a7d93-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="a7d93-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a7d93-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a7d93-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="a7d93-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="a7d93-105">Добавить контроллер администратора</span><span class="sxs-lookup"><span data-stu-id="a7d93-105">Add an Admin Controller</span></span>

<span data-ttu-id="a7d93-106">В этом разделе будет добавлен контроллер веб-API, который поддерживает CRUD (Создание, чтение, обновление и удаление) операций по продуктам.</span><span class="sxs-lookup"><span data-stu-id="a7d93-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="a7d93-107">Контроллер будет использовать Entity Framework для взаимодействия с уровня базы данных.</span><span class="sxs-lookup"><span data-stu-id="a7d93-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="a7d93-108">Только администраторы будут иметь возможность использовать данный контроллер.</span><span class="sxs-lookup"><span data-stu-id="a7d93-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="a7d93-109">Клиенты будут получать доступ продукты по другому контроллеру.</span><span class="sxs-lookup"><span data-stu-id="a7d93-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="a7d93-110">В обозревателе решений щелкните правой кнопкой мыши папку Controllers.</span><span class="sxs-lookup"><span data-stu-id="a7d93-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="a7d93-111">Выберите **добавить** и затем **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="a7d93-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="a7d93-112">В **добавления контроллера** диалога, имя контроллера `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="a7d93-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="a7d93-113">В разделе **шаблона**выберите &quot;контроллер API с действиями чтения и записи, использующий Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="a7d93-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="a7d93-114">В разделе **класс модели**, выберите «Product (ProductStore.Models)».</span><span class="sxs-lookup"><span data-stu-id="a7d93-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="a7d93-115">В разделе **контекст данных**, выберите «&lt;новый контекст данных&gt;».</span><span class="sxs-lookup"><span data-stu-id="a7d93-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="a7d93-116">Если **класс модели** раскрывающийся список не содержит всех классов модели, убедитесь, что при компиляции проекта.</span><span class="sxs-lookup"><span data-stu-id="a7d93-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="a7d93-117">Entity Framework использует отражение, поэтому он должен скомпилированную сборку.</span><span class="sxs-lookup"><span data-stu-id="a7d93-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="a7d93-118">При выборе «&lt;новый контекст данных&gt;» будет открыт **новый контекст данных** диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="a7d93-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="a7d93-119">Имя контекста данных `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="a7d93-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="a7d93-120">Нажмите кнопку **ОК** закрыть **новый контекст данных** диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="a7d93-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="a7d93-121">В **добавления контроллера** диалоговое окно, нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="a7d93-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="a7d93-122">Вот, что был добавлен в проект.</span><span class="sxs-lookup"><span data-stu-id="a7d93-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="a7d93-123">Класс с именем `OrdersContext` , производный от **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="a7d93-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="a7d93-124">Этот класс обеспечивает связь между моделями POCO и базы данных.</span><span class="sxs-lookup"><span data-stu-id="a7d93-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="a7d93-125">Контроллер веб-API с именем `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="a7d93-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="a7d93-126">Этот контроллер поддерживает операций CRUD в `Product` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="a7d93-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="a7d93-127">Она использует `OrdersContext` класс для взаимодействия с Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a7d93-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="a7d93-128">Новая строка подключения базы данных в файле Web.config.</span><span class="sxs-lookup"><span data-stu-id="a7d93-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="a7d93-129">Откройте файл OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="a7d93-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="a7d93-130">Обратите внимание, что конструктор указывает имя строки подключения базы данных.</span><span class="sxs-lookup"><span data-stu-id="a7d93-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="a7d93-131">Это имя относится к строке соединения, который был добавлен в файл Web.config.</span><span class="sxs-lookup"><span data-stu-id="a7d93-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="a7d93-132">Добавьте в класс `OrdersContext` следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="a7d93-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="a7d93-133">Объект **DbSet** представляет набор сущностей, которые могут запрашиваться.</span><span class="sxs-lookup"><span data-stu-id="a7d93-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="a7d93-134">Ниже приведен полный листинг для `OrdersContext` класса:</span><span class="sxs-lookup"><span data-stu-id="a7d93-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="a7d93-135">`AdminController` Класс определяет пять методов, которые реализуют базовые функциональные возможности CRUD.</span><span class="sxs-lookup"><span data-stu-id="a7d93-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="a7d93-136">Каждый метод соответствует URI, который может вызывать клиент:</span><span class="sxs-lookup"><span data-stu-id="a7d93-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="a7d93-137">Метод контроллера</span><span class="sxs-lookup"><span data-stu-id="a7d93-137">Controller Method</span></span> | <span data-ttu-id="a7d93-138">Описание</span><span class="sxs-lookup"><span data-stu-id="a7d93-138">Description</span></span> | <span data-ttu-id="a7d93-139">URI</span><span class="sxs-lookup"><span data-stu-id="a7d93-139">URI</span></span> | <span data-ttu-id="a7d93-140">Метод HTTP</span><span class="sxs-lookup"><span data-stu-id="a7d93-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a7d93-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="a7d93-141">GetProducts</span></span> | <span data-ttu-id="a7d93-142">Получает все продукты.</span><span class="sxs-lookup"><span data-stu-id="a7d93-142">Gets all products.</span></span> | <span data-ttu-id="a7d93-143">API и продуктов</span><span class="sxs-lookup"><span data-stu-id="a7d93-143">api/products</span></span> | <span data-ttu-id="a7d93-144">GET</span><span class="sxs-lookup"><span data-stu-id="a7d93-144">GET</span></span> |
| <span data-ttu-id="a7d93-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="a7d93-145">GetProduct</span></span> | <span data-ttu-id="a7d93-146">Выполняет поиск продуктов по идентификатору.</span><span class="sxs-lookup"><span data-stu-id="a7d93-146">Finds a product by ID.</span></span> | <span data-ttu-id="a7d93-147">API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="a7d93-147">api/products/*id*</span></span> | <span data-ttu-id="a7d93-148">GET</span><span class="sxs-lookup"><span data-stu-id="a7d93-148">GET</span></span> |
| <span data-ttu-id="a7d93-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="a7d93-149">PutProduct</span></span> | <span data-ttu-id="a7d93-150">Обновления продукта.</span><span class="sxs-lookup"><span data-stu-id="a7d93-150">Updates a product.</span></span> | <span data-ttu-id="a7d93-151">API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="a7d93-151">api/products/*id*</span></span> | <span data-ttu-id="a7d93-152">PUT</span><span class="sxs-lookup"><span data-stu-id="a7d93-152">PUT</span></span> |
| <span data-ttu-id="a7d93-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="a7d93-153">PostProduct</span></span> | <span data-ttu-id="a7d93-154">Создает новый продукт.</span><span class="sxs-lookup"><span data-stu-id="a7d93-154">Creates a new product.</span></span> | <span data-ttu-id="a7d93-155">API и продуктов</span><span class="sxs-lookup"><span data-stu-id="a7d93-155">api/products</span></span> | <span data-ttu-id="a7d93-156">ПОМЕСТИТЬ</span><span class="sxs-lookup"><span data-stu-id="a7d93-156">POST</span></span> |
| <span data-ttu-id="a7d93-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="a7d93-157">DeleteProduct</span></span> | <span data-ttu-id="a7d93-158">Удаление продукта.</span><span class="sxs-lookup"><span data-stu-id="a7d93-158">Deletes a product.</span></span> | <span data-ttu-id="a7d93-159">API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="a7d93-159">api/products/*id*</span></span> | <span data-ttu-id="a7d93-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="a7d93-160">DELETE</span></span> |

<span data-ttu-id="a7d93-161">Вызывает каждый метод `OrdersContext` запросов к базе данных.</span><span class="sxs-lookup"><span data-stu-id="a7d93-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="a7d93-162">Вызовите методы, позволяющие изменять коллекцию (PUT, POST и DELETE) `db.SaveChanges` для сохранения изменений в базу данных.</span><span class="sxs-lookup"><span data-stu-id="a7d93-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="a7d93-163">Контроллеры создаются на HTTP-запрос и затем ликвидирован, поэтому это необходимо для сохранения изменений перед возвратом метода.</span><span class="sxs-lookup"><span data-stu-id="a7d93-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="a7d93-164">Добавление инициализатора базы данных</span><span class="sxs-lookup"><span data-stu-id="a7d93-164">Add a Database Initializer</span></span>

<span data-ttu-id="a7d93-165">Платформа Entity Framework должна удобная функция, которая позволяет заполнить базу данных во время запуска и автоматически повторно создать базу данных при каждом изменении модели.</span><span class="sxs-lookup"><span data-stu-id="a7d93-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="a7d93-166">Эта возможность полезна во время разработки, если у вас всегда некоторые тестовые данные, даже при изменении модели.</span><span class="sxs-lookup"><span data-stu-id="a7d93-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="a7d93-167">В обозревателе решений щелкните правой кнопкой мыши папку модели и создать новый класс с именем `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="a7d93-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="a7d93-168">Вставьте следующие реализации:</span><span class="sxs-lookup"><span data-stu-id="a7d93-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="a7d93-169">Путем наследования от **DropCreateDatabaseIfModelChanges** класса, мы указываем удалить базу данных, каждый раз, когда мы изменить классы модели Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a7d93-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="a7d93-170">Если Entity Framework создает (или повторно создает) базы данных, он вызывает **начальное значение** метод для заполнения таблиц.</span><span class="sxs-lookup"><span data-stu-id="a7d93-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="a7d93-171">Мы используем **начальное значение** метод, чтобы добавить некоторые из примера, а также пример заказа.</span><span class="sxs-lookup"><span data-stu-id="a7d93-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="a7d93-172">Эта функция отлично подходит для тестирования, но не по **DropCreateDatabaseIfModelChanges** класса в рабочей среде, так как вы можете потерять данные при изменении модели.</span><span class="sxs-lookup"><span data-stu-id="a7d93-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="a7d93-173">Затем откройте Global.asax и добавьте следующий код в **приложения\_запустить** метод:</span><span class="sxs-lookup"><span data-stu-id="a7d93-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="a7d93-174">Отправка запроса на контроллер</span><span class="sxs-lookup"><span data-stu-id="a7d93-174">Send a Request to the Controller</span></span>

<span data-ttu-id="a7d93-175">На этом этапе мы еще не записаны кода клиента, но можно вызвать веб-сайте, такие как средства API с помощью веб-браузера или отладка HTTP [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="a7d93-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="a7d93-176">В Visual Studio нажмите клавишу F5 для запуска отладки.</span><span class="sxs-lookup"><span data-stu-id="a7d93-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="a7d93-177">Веб-браузере будет открыта `http://localhost:*portnum*/`, где *portnum* — некоторые номер порта.</span><span class="sxs-lookup"><span data-stu-id="a7d93-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="a7d93-178">Отправить запрос HTTP»`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="a7d93-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="a7d93-179">Первый запрос может быть медленное, потому что Entify Framework необходимо создать и инициализировать базу данных.</span><span class="sxs-lookup"><span data-stu-id="a7d93-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="a7d93-180">Ответ должен нечто похожее на следующее:</span><span class="sxs-lookup"><span data-stu-id="a7d93-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

>[!div class="step-by-step"]
<span data-ttu-id="a7d93-181">[Назад](using-web-api-with-entity-framework-part-2.md)
[Вперед](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="a7d93-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
