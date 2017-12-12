---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: "Часть 6: Создание продукта и порядок контроллеров | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: ef7674476e0db334642daa29e352f615135b07ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="a34fd-102">Часть 6: Создание продукта и порядок контроллеров</span><span class="sxs-lookup"><span data-stu-id="a34fd-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="a34fd-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a34fd-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a34fd-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="a34fd-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="a34fd-105">Добавить контроллер продуктов</span><span class="sxs-lookup"><span data-stu-id="a34fd-105">Add a Products Controller</span></span>

<span data-ttu-id="a34fd-106">Контроллер администратора — для пользователей с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="a34fd-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="a34fd-107">Клиенты, с другой стороны, можно просмотреть продукты, но невозможно создать, обновить или удалить их.</span><span class="sxs-lookup"><span data-stu-id="a34fd-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="a34fd-108">Мы легко можно ограничить доступ к методам Post, Put и Delete, не закрывая методы Get.</span><span class="sxs-lookup"><span data-stu-id="a34fd-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="a34fd-109">Но просматривать данные, возвращаемые для продукта:</span><span class="sxs-lookup"><span data-stu-id="a34fd-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="a34fd-110">`ActualCost` Свойства не должны быть видимыми для клиентов.</span><span class="sxs-lookup"><span data-stu-id="a34fd-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="a34fd-111">Решение состоит в определении *объект передачи данных* (DTO), которая включает подмножество свойств, которые должны быть видимыми для клиентов.</span><span class="sxs-lookup"><span data-stu-id="a34fd-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="a34fd-112">Мы будем использовать LINQ для проекта `Product` экземпляры `ProductDTO` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="a34fd-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="a34fd-113">Добавьте класс с именем `ProductDTO` в папке «Models».</span><span class="sxs-lookup"><span data-stu-id="a34fd-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="a34fd-114">Теперь добавьте контроллера.</span><span class="sxs-lookup"><span data-stu-id="a34fd-114">Now add the controller.</span></span> <span data-ttu-id="a34fd-115">В обозревателе решений щелкните правой кнопкой мыши папку Controllers.</span><span class="sxs-lookup"><span data-stu-id="a34fd-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="a34fd-116">Выберите **добавить**, а затем выберите **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="a34fd-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="a34fd-117">В **добавления контроллера** диалога, имя контроллера &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="a34fd-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="a34fd-118">В разделе **шаблона**выберите **контроллер пустой API**.</span><span class="sxs-lookup"><span data-stu-id="a34fd-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="a34fd-119">Замените весь код в файле исходного кода с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="a34fd-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="a34fd-120">По-прежнему использует контроллер `OrdersContext` запросов к базе данных.</span><span class="sxs-lookup"><span data-stu-id="a34fd-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="a34fd-121">Однако вместо возвращения `Product` экземпляры напрямую, мы называем `MapProducts` в проект их на `ProductDTO` экземпляров:</span><span class="sxs-lookup"><span data-stu-id="a34fd-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="a34fd-122">`MapProducts` Возвращает **IQueryable**, поэтому мы можно составить результат с другими параметрами.</span><span class="sxs-lookup"><span data-stu-id="a34fd-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="a34fd-123">Это можно увидеть в `GetProduct` метод, который добавляет **где** предложения запроса:</span><span class="sxs-lookup"><span data-stu-id="a34fd-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="a34fd-124">Добавить контроллер заказов</span><span class="sxs-lookup"><span data-stu-id="a34fd-124">Add an Orders Controller</span></span>

<span data-ttu-id="a34fd-125">Затем добавьте контроллера, который позволяет пользователям создавать и просматривать заказы.</span><span class="sxs-lookup"><span data-stu-id="a34fd-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="a34fd-126">Мы начнем с другой DTO.</span><span class="sxs-lookup"><span data-stu-id="a34fd-126">We'll start with another DTO.</span></span> <span data-ttu-id="a34fd-127">В обозревателе решений щелкните правой кнопкой мыши папку модели и добавьте класс с именем `OrderDTO` используется следующая реализация:</span><span class="sxs-lookup"><span data-stu-id="a34fd-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="a34fd-128">Теперь добавьте контроллера.</span><span class="sxs-lookup"><span data-stu-id="a34fd-128">Now add the controller.</span></span> <span data-ttu-id="a34fd-129">В обозревателе решений щелкните правой кнопкой мыши папку Controllers.</span><span class="sxs-lookup"><span data-stu-id="a34fd-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="a34fd-130">Выберите **добавить**, а затем выберите **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="a34fd-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="a34fd-131">В **добавить контроллер** диалогового окна, задайте следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="a34fd-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="a34fd-132">В разделе **имя контроллера**, введите «OrdersController».</span><span class="sxs-lookup"><span data-stu-id="a34fd-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="a34fd-133">В разделе **шаблона**, выберите «Контроллер API с действиями чтения и записи, использующий Entity Framework».</span><span class="sxs-lookup"><span data-stu-id="a34fd-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="a34fd-134">В разделе **класс модели**выберите &quot;порядке (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="a34fd-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="a34fd-135">В разделе **класс контекста данных**выберите &quot;OrdersContext (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="a34fd-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="a34fd-136">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a34fd-136">Click **Add**.</span></span> <span data-ttu-id="a34fd-137">При этом добавляется файл с именем OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="a34fd-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="a34fd-138">Далее нам нужно изменить реализацию контроллера по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a34fd-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="a34fd-139">Сначала удалите `PutOrder` и `DeleteOrder` методы.</span><span class="sxs-lookup"><span data-stu-id="a34fd-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="a34fd-140">В этом образце клиентов нельзя изменить или удалить существующие заказы.</span><span class="sxs-lookup"><span data-stu-id="a34fd-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="a34fd-141">В реальном приложении необходимо значительный объем внутренней логики для обработки таких случаев.</span><span class="sxs-lookup"><span data-stu-id="a34fd-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="a34fd-142">(Например, был уже заказ?)</span><span class="sxs-lookup"><span data-stu-id="a34fd-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="a34fd-143">Изменение `GetOrders` метод для возврата только заказы, принадлежащие пользователю:</span><span class="sxs-lookup"><span data-stu-id="a34fd-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="a34fd-144">Изменение `GetOrder` метод следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a34fd-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="a34fd-145">Ниже приведены изменения, которые мы внесли в метод.</span><span class="sxs-lookup"><span data-stu-id="a34fd-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="a34fd-146">Возвращает значение `OrderDTO` экземпляра, а не `Order`.</span><span class="sxs-lookup"><span data-stu-id="a34fd-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="a34fd-147">Когда мы запросов к базе данных для заказа, мы используем [DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395) метод, чтобы получить связанный `OrderDetail` и `Product` сущности.</span><span class="sxs-lookup"><span data-stu-id="a34fd-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="a34fd-148">Мы одноуровневые результат с помощью проекции.</span><span class="sxs-lookup"><span data-stu-id="a34fd-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="a34fd-149">HTTP-ответ будет содержать массив продуктов с количествами:</span><span class="sxs-lookup"><span data-stu-id="a34fd-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="a34fd-150">Этот формат является облегчает клиентам использование чем исходный объект графа, который содержит вложенные сущности (заказа, подробности и продуктах).</span><span class="sxs-lookup"><span data-stu-id="a34fd-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="a34fd-151">Последний метод учитывать его `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="a34fd-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="a34fd-152">В данный момент, этот метод принимает `Order` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="a34fd-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="a34fd-153">Но что произойдет, если клиент отправляет текст запроса следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a34fd-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="a34fd-154">Это хорошо структурированный заказ и Entity Framework счастью будет вставлять его в базу данных.</span><span class="sxs-lookup"><span data-stu-id="a34fd-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="a34fd-155">Однако она содержит сущности Product, которая ранее не существовал.</span><span class="sxs-lookup"><span data-stu-id="a34fd-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="a34fd-156">Клиент только что созданный новый продукт в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a34fd-156">The client just created a new product in our database!</span></span> <span data-ttu-id="a34fd-157">Это будет неожиданные отделу fullfilment заказа при обнаружении заказ на koala медведей.</span><span class="sxs-lookup"><span data-stu-id="a34fd-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="a34fd-158">Мораль, должны быть очень осторожными данные, которые принимают в запросе POST или PUT.</span><span class="sxs-lookup"><span data-stu-id="a34fd-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="a34fd-159">Чтобы избежать этой проблемы, измените `PostOrder` метод, чтобы использовать `OrderDTO` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="a34fd-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="a34fd-160">Используйте `OrderDTO` для создания `Order`.</span><span class="sxs-lookup"><span data-stu-id="a34fd-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="a34fd-161">Обратите внимание, что мы используем `ProductID` и `Quantity` свойства и мы игнорировать любые значения, отправляемых клиентом для имени продукта или цены.</span><span class="sxs-lookup"><span data-stu-id="a34fd-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="a34fd-162">Если код продукта не является допустимым, он будет нарушать ограничение внешнего ключа в базе данных, и операция вставки завершится ошибкой, как должно.</span><span class="sxs-lookup"><span data-stu-id="a34fd-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="a34fd-163">Ниже приведен полный `PostOrder` метод:</span><span class="sxs-lookup"><span data-stu-id="a34fd-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="a34fd-164">Наконец, добавьте **авторизовать** атрибут контроллера:</span><span class="sxs-lookup"><span data-stu-id="a34fd-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="a34fd-165">Теперь только зарегистрированные пользователи можно создать или просмотреть заказы.</span><span class="sxs-lookup"><span data-stu-id="a34fd-165">Now only registered users can create or view orders.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a34fd-166">[Назад](using-web-api-with-entity-framework-part-5.md)
[Вперед](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="a34fd-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
