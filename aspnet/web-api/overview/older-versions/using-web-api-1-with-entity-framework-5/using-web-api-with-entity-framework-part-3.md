---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Часть 3: Создание контроллера администрирования | Документация Майкрософт'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: b9d21edec7b5006beea83395cdfc5ae181992e7d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365049"
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="0ff00-102">Часть 3: Создание контроллера администрирования</span><span class="sxs-lookup"><span data-stu-id="0ff00-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="0ff00-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0ff00-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0ff00-104">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="0ff00-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="0ff00-105">Добавление контроллера администрирования</span><span class="sxs-lookup"><span data-stu-id="0ff00-105">Add an Admin Controller</span></span>

<span data-ttu-id="0ff00-106">В этом разделе мы добавим контроллер веб-API, который поддерживает CRUD (Создание, чтение, обновление и удаление) операций по продуктам.</span><span class="sxs-lookup"><span data-stu-id="0ff00-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="0ff00-107">Контроллер будет использовать Entity Framework для взаимодействия с уровня базы данных.</span><span class="sxs-lookup"><span data-stu-id="0ff00-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="0ff00-108">Только администраторы будут иметь возможность использовать этот контроллер.</span><span class="sxs-lookup"><span data-stu-id="0ff00-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="0ff00-109">Клиенты получат доступ к продуктов через другой контроллер.</span><span class="sxs-lookup"><span data-stu-id="0ff00-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="0ff00-110">В обозревателе решений щелкните правой кнопкой мыши папку Controllers.</span><span class="sxs-lookup"><span data-stu-id="0ff00-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="0ff00-111">Выберите **добавить** и затем **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="0ff00-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="0ff00-112">В **Добавление контроллера** диалоговое окно, назовите контроллер `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="0ff00-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="0ff00-113">В разделе **шаблона**выберите &quot;контроллер API с действиями чтения и записи, с помощью Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="0ff00-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="0ff00-114">В разделе **класс модели**, выберите «Product (ProductStore.Models)».</span><span class="sxs-lookup"><span data-stu-id="0ff00-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="0ff00-115">В разделе **контекст данных**, выберите "&lt;новый контекст данных&gt;«.</span><span class="sxs-lookup"><span data-stu-id="0ff00-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="0ff00-116">Если **класс модели** раскрывающегося списка не отображаются все классы модели компилировать проект.</span><span class="sxs-lookup"><span data-stu-id="0ff00-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="0ff00-117">Платформа Entity Framework использует отражение, поэтому оно должно скомпилированной сборки.</span><span class="sxs-lookup"><span data-stu-id="0ff00-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="0ff00-118">Выбрав "&lt;новый контекст данных&gt;" откроется **новый контекст данных** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="0ff00-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="0ff00-119">Имя контекста данных `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="0ff00-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="0ff00-120">Нажмите кнопку **ОК** Отклонить **новый контекст данных** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="0ff00-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="0ff00-121">В **Добавление контроллера** диалоговое окно, нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="0ff00-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="0ff00-122">Вот, что был добавлен в проект:</span><span class="sxs-lookup"><span data-stu-id="0ff00-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="0ff00-123">Класс с именем `OrdersContext` , наследуемый от класса **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="0ff00-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="0ff00-124">Этот класс обеспечивает связь между моделями POCO и базе данных.</span><span class="sxs-lookup"><span data-stu-id="0ff00-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="0ff00-125">Контроллер Web API с именем `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="0ff00-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="0ff00-126">Этот контроллер поддерживает операции CRUD в `Product` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="0ff00-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="0ff00-127">Она использует `OrdersContext` класс для взаимодействия с Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0ff00-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="0ff00-128">Новая строка подключения базы данных в файле Web.config.</span><span class="sxs-lookup"><span data-stu-id="0ff00-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="0ff00-129">Откройте файл OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="0ff00-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="0ff00-130">Обратите внимание на то, что конструктор указывает имя строки подключения базы данных.</span><span class="sxs-lookup"><span data-stu-id="0ff00-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="0ff00-131">Это имя относится к строке подключения, который был добавлен в файл Web.config.</span><span class="sxs-lookup"><span data-stu-id="0ff00-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="0ff00-132">Добавьте в класс `OrdersContext` следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="0ff00-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="0ff00-133">Объект **DbSet** представляет набор сущностей, которые могут запрашиваться.</span><span class="sxs-lookup"><span data-stu-id="0ff00-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="0ff00-134">Ниже приведен полный список для `OrdersContext` класса:</span><span class="sxs-lookup"><span data-stu-id="0ff00-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="0ff00-135">`AdminController` Класс определяет пять методов, которые реализуют основные функции CRUD.</span><span class="sxs-lookup"><span data-stu-id="0ff00-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="0ff00-136">Каждый метод соответствует URI, который может вызвать клиент:</span><span class="sxs-lookup"><span data-stu-id="0ff00-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="0ff00-137">Метод контроллера</span><span class="sxs-lookup"><span data-stu-id="0ff00-137">Controller Method</span></span> | <span data-ttu-id="0ff00-138">Описание:</span><span class="sxs-lookup"><span data-stu-id="0ff00-138">Description</span></span> | <span data-ttu-id="0ff00-139">URI</span><span class="sxs-lookup"><span data-stu-id="0ff00-139">URI</span></span> | <span data-ttu-id="0ff00-140">Метод HTTP</span><span class="sxs-lookup"><span data-stu-id="0ff00-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="0ff00-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="0ff00-141">GetProducts</span></span> | <span data-ttu-id="0ff00-142">Получает все продукты.</span><span class="sxs-lookup"><span data-stu-id="0ff00-142">Gets all products.</span></span> | <span data-ttu-id="0ff00-143">API/products</span><span class="sxs-lookup"><span data-stu-id="0ff00-143">api/products</span></span> | <span data-ttu-id="0ff00-144">GET</span><span class="sxs-lookup"><span data-stu-id="0ff00-144">GET</span></span> |
| <span data-ttu-id="0ff00-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="0ff00-145">GetProduct</span></span> | <span data-ttu-id="0ff00-146">Находит продукта по идентификатору.</span><span class="sxs-lookup"><span data-stu-id="0ff00-146">Finds a product by ID.</span></span> | <span data-ttu-id="0ff00-147">API/products/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="0ff00-147">api/products/*id*</span></span> | <span data-ttu-id="0ff00-148">GET</span><span class="sxs-lookup"><span data-stu-id="0ff00-148">GET</span></span> |
| <span data-ttu-id="0ff00-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="0ff00-149">PutProduct</span></span> | <span data-ttu-id="0ff00-150">Обновление продукта.</span><span class="sxs-lookup"><span data-stu-id="0ff00-150">Updates a product.</span></span> | <span data-ttu-id="0ff00-151">API/products/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="0ff00-151">api/products/*id*</span></span> | <span data-ttu-id="0ff00-152">PUT</span><span class="sxs-lookup"><span data-stu-id="0ff00-152">PUT</span></span> |
| <span data-ttu-id="0ff00-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="0ff00-153">PostProduct</span></span> | <span data-ttu-id="0ff00-154">Создает новый продукт.</span><span class="sxs-lookup"><span data-stu-id="0ff00-154">Creates a new product.</span></span> | <span data-ttu-id="0ff00-155">API/products</span><span class="sxs-lookup"><span data-stu-id="0ff00-155">api/products</span></span> | <span data-ttu-id="0ff00-156">ПОМЕСТИТЬ</span><span class="sxs-lookup"><span data-stu-id="0ff00-156">POST</span></span> |
| <span data-ttu-id="0ff00-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="0ff00-157">DeleteProduct</span></span> | <span data-ttu-id="0ff00-158">Удаление продукта.</span><span class="sxs-lookup"><span data-stu-id="0ff00-158">Deletes a product.</span></span> | <span data-ttu-id="0ff00-159">API/products/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="0ff00-159">api/products/*id*</span></span> | <span data-ttu-id="0ff00-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="0ff00-160">DELETE</span></span> |

<span data-ttu-id="0ff00-161">Каждый метод вызывает `OrdersContext` для запроса к базе данных.</span><span class="sxs-lookup"><span data-stu-id="0ff00-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="0ff00-162">Вызовите методы, которые изменить коллекцию (PUT, POST и DELETE) `db.SaveChanges` для сохранения изменений в базу данных.</span><span class="sxs-lookup"><span data-stu-id="0ff00-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="0ff00-163">Контроллеры создаются на HTTP-запрос и затем удален, поэтому необходимо сохранить изменения перед возвратом метода.</span><span class="sxs-lookup"><span data-stu-id="0ff00-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="0ff00-164">Добавить инициализатор базы данных</span><span class="sxs-lookup"><span data-stu-id="0ff00-164">Add a Database Initializer</span></span>

<span data-ttu-id="0ff00-165">Платформа Entity Framework есть замечательная функция, которая позволяет вам заполнить базу данных при запуске и автоматически воссоздать базу данных при каждом изменении модели.</span><span class="sxs-lookup"><span data-stu-id="0ff00-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="0ff00-166">Эта функция полезна во время разработки, так как у вас всегда есть некоторые тестовые данные, даже при изменении модели.</span><span class="sxs-lookup"><span data-stu-id="0ff00-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="0ff00-167">В обозревателе решений щелкните правой кнопкой мыши папку Models и создайте новый класс с именем `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="0ff00-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="0ff00-168">Вставьте следующую реализацию:</span><span class="sxs-lookup"><span data-stu-id="0ff00-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="0ff00-169">Путем наследования от **DropCreateDatabaseIfModelChanges** класс, мы указываем Entity Framework, чтобы удалить базу данных, каждый раз, когда мы изменим классы моделей.</span><span class="sxs-lookup"><span data-stu-id="0ff00-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="0ff00-170">Когда Entity Framework создает (или повторно создает) базы данных, он вызывает **начальное значение** метод для заполнения таблиц.</span><span class="sxs-lookup"><span data-stu-id="0ff00-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="0ff00-171">Мы используем **начальное значение** метод, чтобы добавить некоторые продукты пример, а также пример заказа.</span><span class="sxs-lookup"><span data-stu-id="0ff00-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="0ff00-172">Эта функция очень удобно для тестирования, но не используйте **DropCreateDatabaseIfModelChanges** класса в рабочей среде, так как вы можете потерять данные при изменении модели.</span><span class="sxs-lookup"><span data-stu-id="0ff00-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="0ff00-173">Затем откройте Global.asax и добавьте следующий код, чтобы **приложения\_запустить** метод:</span><span class="sxs-lookup"><span data-stu-id="0ff00-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="0ff00-174">Отправьте запрос к контроллеру</span><span class="sxs-lookup"><span data-stu-id="0ff00-174">Send a Request to the Controller</span></span>

<span data-ttu-id="0ff00-175">На этом этапе мы еще не написали код клиента, но вы можете вызвать web API с помощью веб-браузер или отладки HTTP, такие как средство [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="0ff00-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="0ff00-176">В Visual Studio нажмите клавишу F5, чтобы начать отладку.</span><span class="sxs-lookup"><span data-stu-id="0ff00-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="0ff00-177">Веб-браузере будет открыта `http://localhost:*portnum*/`, где *portnum* является некоторое число портов.</span><span class="sxs-lookup"><span data-stu-id="0ff00-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="0ff00-178">Отправить запрос HTTP для "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="0ff00-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="0ff00-179">Первый запрос может быть медленное, поскольку Entify Framework необходимо создать и заполнить базу данных.</span><span class="sxs-lookup"><span data-stu-id="0ff00-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="0ff00-180">Ответ должен нечто похожее на следующее:</span><span class="sxs-lookup"><span data-stu-id="0ff00-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="0ff00-181">[Назад](using-web-api-with-entity-framework-part-2.md)
> [Вперед](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="0ff00-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
