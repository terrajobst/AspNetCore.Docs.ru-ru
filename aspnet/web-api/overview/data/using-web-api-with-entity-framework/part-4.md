---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Обработка отношениями сущностей | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 3c82724739b8ccb7c6b13788a5420af1e61c990b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872693"
---
<a name="handling-entity-relations"></a><span data-ttu-id="42f52-102">Обработка отношениями сущностей</span><span class="sxs-lookup"><span data-stu-id="42f52-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="42f52-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="42f52-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="42f52-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="42f52-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="42f52-105">В этом разделе приводятся некоторые сведения, как EF загружает связанные сущности и как обрабатывать циклических переходов свойства в классах модели.</span><span class="sxs-lookup"><span data-stu-id="42f52-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="42f52-106">(Этот раздел предоставляет фундаментальных знаний и не требуется для завершения работы с учебником.</span><span class="sxs-lookup"><span data-stu-id="42f52-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="42f52-107">Если вы предпочитаете, перейдите к [. часть 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="42f52-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="42f52-108">Eager загрузка сравнению отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="42f52-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="42f52-109">При использовании EF с реляционной базой данных, важно понимать, каким образом EF загружает связанные данные.</span><span class="sxs-lookup"><span data-stu-id="42f52-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="42f52-110">Также полезно для просмотра запросов SQL, EF приводит к возникновению ошибки.</span><span class="sxs-lookup"><span data-stu-id="42f52-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="42f52-111">Для трассировки SQL, добавьте следующую строку кода, `BookServiceContext` конструктор:</span><span class="sxs-lookup"><span data-stu-id="42f52-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="42f52-112">При отправке запроса GET для /api/books, он возвращает JSON следующим образом:</span><span class="sxs-lookup"><span data-stu-id="42f52-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="42f52-113">Вы увидите в свойстве Author пуст, несмотря на то, что в книге содержатся допустимые AuthorId.</span><span class="sxs-lookup"><span data-stu-id="42f52-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="42f52-114">Это потому, что EF не загружает связанные сущности автора.</span><span class="sxs-lookup"><span data-stu-id="42f52-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="42f52-115">Это подтверждается журнал трассировки SQL-запроса:</span><span class="sxs-lookup"><span data-stu-id="42f52-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="42f52-116">Инструкция SELECT принимает из электронной таблицы и не ссылаются на таблицу автора.</span><span class="sxs-lookup"><span data-stu-id="42f52-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="42f52-117">Для справки ниже приведен метод в `BooksController` класс, который возвращает список книг.</span><span class="sxs-lookup"><span data-stu-id="42f52-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="42f52-118">Давайте посмотрим, как может возвращать автора как часть данных JSON.</span><span class="sxs-lookup"><span data-stu-id="42f52-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="42f52-119">Существует три способа загрузки связанных данных в Entity Framework: упреждающую отложенную загрузку и явная загрузка.</span><span class="sxs-lookup"><span data-stu-id="42f52-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="42f52-120">Существуют преимущества и недостатки, с помощью каждого метода, поэтому очень важно понять, как они работают.</span><span class="sxs-lookup"><span data-stu-id="42f52-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="42f52-121">Активная загрузка</span><span class="sxs-lookup"><span data-stu-id="42f52-121">Eager Loading</span></span>

<span data-ttu-id="42f52-122">С *упреждающую*, EF загружает связанные сущности как часть запроса начальную базу данных.</span><span class="sxs-lookup"><span data-stu-id="42f52-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="42f52-123">Чтобы выполнить упреждающую, используйте **System.Data.Entity.Include** метода расширения.</span><span class="sxs-lookup"><span data-stu-id="42f52-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="42f52-124">Это заставляет EF, чтобы включить данные автора в запросе.</span><span class="sxs-lookup"><span data-stu-id="42f52-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="42f52-125">Если этого не сделать и запустить приложение, теперь данные JSON выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="42f52-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="42f52-126">Журнал трассировки показывает, что EF выполняется соединение таблиц книги и автор.</span><span class="sxs-lookup"><span data-stu-id="42f52-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="42f52-127">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="42f52-127">Lazy Loading</span></span>

<span data-ttu-id="42f52-128">С отложенной загрузкой EF автоматически загружает связанные сущности при разыменовании свойство навигации для этой сущности.</span><span class="sxs-lookup"><span data-stu-id="42f52-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="42f52-129">Чтобы включить отложенную загрузку, создать виртуальный свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="42f52-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="42f52-130">Например в класс книги:</span><span class="sxs-lookup"><span data-stu-id="42f52-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="42f52-131">Теперь рассмотрим следующий код:</span><span class="sxs-lookup"><span data-stu-id="42f52-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="42f52-132">Если отложенная загрузка включена, доступ к `Author` свойство `books[0]` вызывает EF запросов к базе данных для автора.</span><span class="sxs-lookup"><span data-stu-id="42f52-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="42f52-133">Отложенная загрузка требует несколько обмена базы данных, так как EF отправляет запрос каждый раз, он извлекает связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="42f52-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="42f52-134">Как правило требуется отложенная загрузка отключена для объектов, которые можно сериализовать.</span><span class="sxs-lookup"><span data-stu-id="42f52-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="42f52-135">Сериализатор должен читать все свойства в модели, которая запускает загрузка связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="42f52-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="42f52-136">Например ниже приведено SQL-запросы, когда EF сериализует список книг с отложенной загрузкой включена.</span><span class="sxs-lookup"><span data-stu-id="42f52-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="42f52-137">Вы увидите, что EF делает три разных запросов для авторов три.</span><span class="sxs-lookup"><span data-stu-id="42f52-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="42f52-138">Существует еще раз, когда может потребоваться использовать отложенную загрузку.</span><span class="sxs-lookup"><span data-stu-id="42f52-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="42f52-139">Безотложная загрузка может привести к EF для создания очень сложные соединения.</span><span class="sxs-lookup"><span data-stu-id="42f52-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="42f52-140">Связанные сущности, которые могут потребоваться для небольшого подмножества данных и отложенная загрузка будет более эффективным.</span><span class="sxs-lookup"><span data-stu-id="42f52-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="42f52-141">Сериализовать объекты передачи данных (DTO) вместо сущности является одним из способов избежать проблем при сериализации.</span><span class="sxs-lookup"><span data-stu-id="42f52-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="42f52-142">Далее в этой статье мы рассмотрим этот подход.</span><span class="sxs-lookup"><span data-stu-id="42f52-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="42f52-143">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="42f52-143">Explicit Loading</span></span>

<span data-ttu-id="42f52-144">Явная загрузка похожа на отложенную загрузку, за исключением того, что явным образом получать взаимосвязанные данные в коде; Это не происходит автоматически при доступе к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="42f52-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="42f52-145">Явная загрузка обеспечивает больший контроль над тем, когда для загрузки связанных данных, но требует дополнительного кода.</span><span class="sxs-lookup"><span data-stu-id="42f52-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="42f52-146">Дополнительные сведения о явная загрузка см. в разделе [загрузка связанных сущностей](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="42f52-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="42f52-147">Свойства навигации и циклические ссылки</span><span class="sxs-lookup"><span data-stu-id="42f52-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="42f52-148">Когда определены в моделях книги и автор, определены свойством навигации на `Book` класс для связи автор книги, но в обратном направлении не я определял свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="42f52-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="42f52-149">Что произойдет, если добавить соответствующее свойство навигации для `Author` класса?</span><span class="sxs-lookup"><span data-stu-id="42f52-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="42f52-150">К сожалению это создает проблему при сериализации модели.</span><span class="sxs-lookup"><span data-stu-id="42f52-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="42f52-151">При загрузке связанных данных, он создает граф объекта.</span><span class="sxs-lookup"><span data-stu-id="42f52-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="42f52-152">Когда модуль форматирования в формате JSON или XML пытается сериализации графа, он вызовет исключение.</span><span class="sxs-lookup"><span data-stu-id="42f52-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="42f52-153">Двумя модулями форматирования вызывают исключение другого сообщения.</span><span class="sxs-lookup"><span data-stu-id="42f52-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="42f52-154">Ниже приведен пример форматирования JSON:</span><span class="sxs-lookup"><span data-stu-id="42f52-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="42f52-155">Вот модуль форматирования XML.</span><span class="sxs-lookup"><span data-stu-id="42f52-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="42f52-156">Одним из решений является использование DTO, описывающие в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="42f52-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="42f52-157">Кроме того можно настроить форматирования данных JSON и XML для обработки графа циклов.</span><span class="sxs-lookup"><span data-stu-id="42f52-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="42f52-158">Дополнительные сведения см. в разделе [обработки циклических ссылок объекта](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="42f52-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="42f52-159">В этом учебнике не требуется `Author.Book` свойство навигации, поэтому его можно опустить.</span><span class="sxs-lookup"><span data-stu-id="42f52-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="42f52-160">[Назад](part-3.md)
> [Вперед](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="42f52-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
