---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Создать объекты передачи данных (DTO) | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="4dc8c-102">Создать объекты передачи данных (DTO)</span><span class="sxs-lookup"><span data-stu-id="4dc8c-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="4dc8c-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4dc8c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4dc8c-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="4dc8c-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="4dc8c-105">Справа теперь наших веб-API предоставляет сущностей базы данных на клиент.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="4dc8c-106">Клиент получает данные, которые сопоставляются непосредственно в таблицах базы данных.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="4dc8c-107">Однако это не всегда рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-107">However, that's not always a good idea.</span></span> <span data-ttu-id="4dc8c-108">Иногда требуется изменить форму данных, отправляемых клиенту.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="4dc8c-109">Например, можно сделать следующее:</span><span class="sxs-lookup"><span data-stu-id="4dc8c-109">For example, you might want to:</span></span>

- <span data-ttu-id="4dc8c-110">Удалите циклические ссылки (см. выше).</span><span class="sxs-lookup"><span data-stu-id="4dc8c-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="4dc8c-111">Скрыть определенным свойствам, которые клиенты не могут просмотреть.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="4dc8c-112">Пропустите некоторые свойства, чтобы уменьшить размер полезных данных.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="4dc8c-113">Сведение графов объектов, содержащих вложенные объекты, чтобы сделать их более удобным для клиентов.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="4dc8c-114">Избегайте «чрезмерно учета» уязвимостей.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="4dc8c-115">(См. [проверка модели](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) обсуждение чрезмерное учета.)</span><span class="sxs-lookup"><span data-stu-id="4dc8c-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="4dc8c-116">Отделяете на уровне службы из вашего уровня базы данных.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="4dc8c-117">Чтобы сделать это, можно определить *объект передачи данных* (DTO).</span><span class="sxs-lookup"><span data-stu-id="4dc8c-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="4dc8c-118">Объект DTO — это объект, который определяет, каким образом данные будут отправлены по сети.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="4dc8c-119">Давайте посмотрим, как это работает с сущностью книги.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="4dc8c-120">В папке Models находится добавьте два класса DTO:</span><span class="sxs-lookup"><span data-stu-id="4dc8c-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="4dc8c-121">`BookDetailDTO` Класс включает все свойства из книги модели, за исключением того, что `AuthorName` является строкой, которая будет содержать имя автора.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="4dc8c-122">`BookDTO` Класс содержит подмножество свойств из `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="4dc8c-123">Затем замените два метода GET в `BooksController` класса с версиями, которые возвращают DTO.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="4dc8c-124">Мы будем использовать LINQ **выберите** инструкции для преобразования из сущностей книг в DTO.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="4dc8c-125">Ниже приведен код SQL, созданный новый `GetBooks` метод.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="4dc8c-126">Вы увидите, что EF преобразует LINQ **выберите** в инструкцию SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="4dc8c-127">Наконец, измените `PostBook` метод для возврата DTO.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="4dc8c-128">В этом учебнике мы преобразование DTO вручную в коде.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="4dc8c-129">Другой вариант — использовать библиотеку как [AutoMapper](http://automapper.org/) , обрабатывает преобразование автоматически.</span><span class="sxs-lookup"><span data-stu-id="4dc8c-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="4dc8c-130">[Назад](part-4.md)
> [Вперед](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="4dc8c-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
