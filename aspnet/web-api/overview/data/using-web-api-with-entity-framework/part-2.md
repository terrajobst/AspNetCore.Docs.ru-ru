---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Добавление моделей и контроллеры | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 015bb9698d81387d03ea8f9629316fb53232e708
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="add-models-and-controllers"></a><span data-ttu-id="fe144-102">Добавление моделей и контроллеры</span><span class="sxs-lookup"><span data-stu-id="fe144-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="fe144-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fe144-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="fe144-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="fe144-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="fe144-105">В этом разделе вы добавите модели классы, определяющие сущности базы данных.</span><span class="sxs-lookup"><span data-stu-id="fe144-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="fe144-106">Затем нужно добавить контроллеры веб-API, которые выполняют операции CRUD над этими сущностями.</span><span class="sxs-lookup"><span data-stu-id="fe144-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="fe144-107">Добавлять классы модели</span><span class="sxs-lookup"><span data-stu-id="fe144-107">Add Model Classes</span></span>

<span data-ttu-id="fe144-108">В этом учебнике мы создадим базы данных на основе подхода «Первый кода» для Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="fe144-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="fe144-109">В режиме Code First записи классов C#, которые соответствуют таблицам базы данных и EF создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="fe144-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="fe144-110">(Дополнительные сведения см. в разделе [принципы разработки с использованием Entity Framework](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="fe144-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="fe144-111">Начинается с определения наши объекты домена как POCO (plain old CLR объекты).</span><span class="sxs-lookup"><span data-stu-id="fe144-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="fe144-112">Мы создадим POCO следующие:</span><span class="sxs-lookup"><span data-stu-id="fe144-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="fe144-113">Дизайнер</span><span class="sxs-lookup"><span data-stu-id="fe144-113">Author</span></span>
- <span data-ttu-id="fe144-114">Книги</span><span class="sxs-lookup"><span data-stu-id="fe144-114">Book</span></span>

<span data-ttu-id="fe144-115">В обозревателе решений щелкните правой кнопкой мыши папку модели.</span><span class="sxs-lookup"><span data-stu-id="fe144-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="fe144-116">Выберите **добавить**, а затем выберите **класса**.</span><span class="sxs-lookup"><span data-stu-id="fe144-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="fe144-117">Присвойте классу имя `Author`.</span><span class="sxs-lookup"><span data-stu-id="fe144-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="fe144-118">Замените весь код шаблона в Author.cs следующий код.</span><span class="sxs-lookup"><span data-stu-id="fe144-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="fe144-119">Добавьте еще один класс с именем `Book`, с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="fe144-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="fe144-120">Платформа Entity Framework будет использовать эти модели для создания таблиц базы данных.</span><span class="sxs-lookup"><span data-stu-id="fe144-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="fe144-121">Для каждой модели `Id` свойство станет столбец первичного ключа таблицы базы данных.</span><span class="sxs-lookup"><span data-stu-id="fe144-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="fe144-122">В классе книги `AuthorId` определяет внешний ключ в `Author` таблицы.</span><span class="sxs-lookup"><span data-stu-id="fe144-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="fe144-123">(Для простоты я полагаю, каждая книга имеет одного автора.) Книги класс также содержит свойства навигации для связанного `Author`.</span><span class="sxs-lookup"><span data-stu-id="fe144-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="fe144-124">Свойство навигации можно использовать для доступа к связанной `Author` в коде.</span><span class="sxs-lookup"><span data-stu-id="fe144-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="fe144-125">Подробнее о свойствах навигации в часть 4, [обработки отношениями сущностей](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="fe144-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="fe144-126">Добавление контроллеров веб-API</span><span class="sxs-lookup"><span data-stu-id="fe144-126">Add Web API Controllers</span></span>

<span data-ttu-id="fe144-127">В этом разделе мы добавим контроллеры веб-API, которые поддерживают операции CRUD (Создание, чтение, обновление и удаление).</span><span class="sxs-lookup"><span data-stu-id="fe144-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="fe144-128">Контроллеры будет использовать Entity Framework для взаимодействия с уровня базы данных.</span><span class="sxs-lookup"><span data-stu-id="fe144-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="fe144-129">Во-первых можно удалить файл Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="fe144-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="fe144-130">Этот файл содержит пример контроллер веб-API, но это не требуется для этого учебника.</span><span class="sxs-lookup"><span data-stu-id="fe144-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="fe144-131">Затем выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="fe144-131">Next, build the project.</span></span> <span data-ttu-id="fe144-132">Формирование шаблонов веб-API использует отражение для поиска классы модели, поэтому он должен скомпилированную сборку.</span><span class="sxs-lookup"><span data-stu-id="fe144-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="fe144-133">В обозревателе решений щелкните правой кнопкой мыши папку Controllers.</span><span class="sxs-lookup"><span data-stu-id="fe144-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="fe144-134">Выберите **добавить**, а затем выберите **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="fe144-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="fe144-135">В **Добавление формирования шаблонов** диалогового окна выберите «веб-API 2 контроллер с действиями, использующий Entity Framework».</span><span class="sxs-lookup"><span data-stu-id="fe144-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="fe144-136">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="fe144-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="fe144-137">В **добавить контроллер** диалоговое окно, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="fe144-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="fe144-138">В **класс модели** раскрывающийся список, выберите `Author` класса.</span><span class="sxs-lookup"><span data-stu-id="fe144-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="fe144-139">(Если вы не видите она появляется в раскрывающемся списке, убедитесь, что построение проекта.)</span><span class="sxs-lookup"><span data-stu-id="fe144-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="fe144-140">Проверьте «Используйте асинхронные действия контроллера».</span><span class="sxs-lookup"><span data-stu-id="fe144-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="fe144-141">Имя контроллера как оставить &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="fe144-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="fe144-142">Щелкните плюс (+) рядом с кнопкой **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="fe144-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="fe144-143">В **новый контекст данных** диалогового окна, оставьте имя по умолчанию и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="fe144-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="fe144-144">Нажмите кнопку **добавить** для завершения **добавить контроллер** диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="fe144-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="fe144-145">Диалоговое окно добавляет два класса в проект:</span><span class="sxs-lookup"><span data-stu-id="fe144-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="fe144-146">`AuthorsController` Определяет контроллер веб-API.</span><span class="sxs-lookup"><span data-stu-id="fe144-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="fe144-147">Контроллер реализует API REST, который клиенты используют для выполнения операций CRUD в списке авторов.</span><span class="sxs-lookup"><span data-stu-id="fe144-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="fe144-148">`BookServiceContext` управляет объектами сущностей во время выполнения, включая заполнение данными из базы данных, отслеживание изменений и сохранения данных в базу данных.</span><span class="sxs-lookup"><span data-stu-id="fe144-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="fe144-149">Он наследуется от `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="fe144-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="fe144-150">На этом этапе построения проекта.</span><span class="sxs-lookup"><span data-stu-id="fe144-150">At this point, build the project again.</span></span> <span data-ttu-id="fe144-151">Теперь выполните те же шаги, чтобы добавить контроллер API для `Book` сущностей.</span><span class="sxs-lookup"><span data-stu-id="fe144-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="fe144-152">На этот раз выберите `Book` класс модели и выберите существующий `BookServiceContext` класс для класса контекста данных.</span><span class="sxs-lookup"><span data-stu-id="fe144-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="fe144-153">(Не создавать новый контекст данных). Нажмите кнопку **добавить** Добавление контроллера.</span><span class="sxs-lookup"><span data-stu-id="fe144-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="fe144-154">[Назад](part-1.md)
> [Вперед](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="fe144-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
