---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: "Добавление моделей и контроллеры | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: b75eae11fd99b60864256f79d4770a3007487964
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="add-models-and-controllers"></a><span data-ttu-id="e4975-102">Добавление моделей и контроллеры</span><span class="sxs-lookup"><span data-stu-id="e4975-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="e4975-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e4975-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e4975-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="e4975-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="e4975-105">В этом разделе вы добавите модели классы, определяющие сущности базы данных.</span><span class="sxs-lookup"><span data-stu-id="e4975-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="e4975-106">Затем нужно добавить контроллеры веб-API, которые выполняют операции CRUD над этими сущностями.</span><span class="sxs-lookup"><span data-stu-id="e4975-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="e4975-107">Добавлять классы модели</span><span class="sxs-lookup"><span data-stu-id="e4975-107">Add Model Classes</span></span>

<span data-ttu-id="e4975-108">В этом учебнике мы создадим базы данных на основе подхода «Первый кода» для Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="e4975-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="e4975-109">В режиме Code First записи классов C#, которые соответствуют таблицам базы данных и EF создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="e4975-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="e4975-110">(Дополнительные сведения см. в разделе [принципы разработки с использованием Entity Framework](https://msdn.microsoft.com/en-us/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="e4975-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/en-us/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="e4975-111">Начинается с определения наши объекты домена как POCO (plain old CLR объекты).</span><span class="sxs-lookup"><span data-stu-id="e4975-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="e4975-112">Мы создадим POCO следующие:</span><span class="sxs-lookup"><span data-stu-id="e4975-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="e4975-113">Дизайнер</span><span class="sxs-lookup"><span data-stu-id="e4975-113">Author</span></span>
- <span data-ttu-id="e4975-114">Книги</span><span class="sxs-lookup"><span data-stu-id="e4975-114">Book</span></span>

<span data-ttu-id="e4975-115">В обозревателе решений щелкните правой кнопкой мыши папку модели.</span><span class="sxs-lookup"><span data-stu-id="e4975-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="e4975-116">Выберите **добавить**, а затем выберите **класса**.</span><span class="sxs-lookup"><span data-stu-id="e4975-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="e4975-117">Присвойте классу имя `Author`.</span><span class="sxs-lookup"><span data-stu-id="e4975-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="e4975-118">Замените весь код шаблона в Author.cs следующий код.</span><span class="sxs-lookup"><span data-stu-id="e4975-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="e4975-119">Добавьте еще один класс с именем `Book`, с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="e4975-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="e4975-120">Платформа Entity Framework будет использовать эти модели для создания таблиц базы данных.</span><span class="sxs-lookup"><span data-stu-id="e4975-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="e4975-121">Для каждой модели `Id` свойство станет столбец первичного ключа таблицы базы данных.</span><span class="sxs-lookup"><span data-stu-id="e4975-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="e4975-122">В классе книги `AuthorId` определяет внешний ключ в `Author` таблицы.</span><span class="sxs-lookup"><span data-stu-id="e4975-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="e4975-123">(Для простоты я полагаю, каждая книга имеет одного автора.) Книги класс также содержит свойства навигации для связанного `Author`.</span><span class="sxs-lookup"><span data-stu-id="e4975-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="e4975-124">Свойство навигации можно использовать для доступа к связанной `Author` в коде.</span><span class="sxs-lookup"><span data-stu-id="e4975-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="e4975-125">Подробнее о свойствах навигации в часть 4, [обработки отношениями сущностей](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="e4975-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="e4975-126">Добавление контроллеров веб-API</span><span class="sxs-lookup"><span data-stu-id="e4975-126">Add Web API Controllers</span></span>

<span data-ttu-id="e4975-127">В этом разделе мы добавим контроллеры веб-API, которые поддерживают операции CRUD (Создание, чтение, обновление и удаление).</span><span class="sxs-lookup"><span data-stu-id="e4975-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="e4975-128">Контроллеры будет использовать Entity Framework для взаимодействия с уровня базы данных.</span><span class="sxs-lookup"><span data-stu-id="e4975-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="e4975-129">Во-первых можно удалить файл Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="e4975-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="e4975-130">Этот файл содержит пример контроллер веб-API, но это не требуется для этого учебника.</span><span class="sxs-lookup"><span data-stu-id="e4975-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="e4975-131">Затем выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="e4975-131">Next, build the project.</span></span> <span data-ttu-id="e4975-132">Формирование шаблонов веб-API использует отражение для поиска классы модели, поэтому он должен скомпилированную сборку.</span><span class="sxs-lookup"><span data-stu-id="e4975-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="e4975-133">В обозревателе решений щелкните правой кнопкой мыши папку Controllers.</span><span class="sxs-lookup"><span data-stu-id="e4975-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="e4975-134">Выберите **добавить**, а затем выберите **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="e4975-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="e4975-135">В **Добавление формирования шаблонов** диалогового окна выберите «веб-API 2 контроллер с действиями, использующий Entity Framework».</span><span class="sxs-lookup"><span data-stu-id="e4975-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="e4975-136">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="e4975-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="e4975-137">В **добавить контроллер** диалоговое окно, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="e4975-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="e4975-138">В **класс модели** раскрывающийся список, выберите `Author` класса.</span><span class="sxs-lookup"><span data-stu-id="e4975-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="e4975-139">(Если вы не видите она появляется в раскрывающемся списке, убедитесь, что построение проекта.)</span><span class="sxs-lookup"><span data-stu-id="e4975-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="e4975-140">Проверьте «Используйте асинхронные действия контроллера».</span><span class="sxs-lookup"><span data-stu-id="e4975-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="e4975-141">Имя контроллера как оставить &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="e4975-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="e4975-142">Щелкните плюс (+) рядом с кнопкой **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="e4975-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="e4975-143">В **новый контекст данных** диалогового окна, оставьте имя по умолчанию и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="e4975-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="e4975-144">Нажмите кнопку **добавить** для завершения **добавить контроллер** диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="e4975-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="e4975-145">Диалоговое окно добавляет два класса в проект:</span><span class="sxs-lookup"><span data-stu-id="e4975-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="e4975-146">`AuthorsController`Определяет контроллер веб-API.</span><span class="sxs-lookup"><span data-stu-id="e4975-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="e4975-147">Контроллер реализует API REST, который клиенты используют для выполнения операций CRUD в списке авторов.</span><span class="sxs-lookup"><span data-stu-id="e4975-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="e4975-148">`BookServiceContext`управляет объектами сущностей во время выполнения, включая заполнение данными из базы данных, отслеживание изменений и сохранения данных в базу данных.</span><span class="sxs-lookup"><span data-stu-id="e4975-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="e4975-149">Он наследуется от `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="e4975-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="e4975-150">На этом этапе построения проекта.</span><span class="sxs-lookup"><span data-stu-id="e4975-150">At this point, build the project again.</span></span> <span data-ttu-id="e4975-151">Теперь выполните те же шаги, чтобы добавить контроллер API для `Book` сущностей.</span><span class="sxs-lookup"><span data-stu-id="e4975-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="e4975-152">На этот раз выберите `Book` класс модели и выберите существующий `BookServiceContext` класс для класса контекста данных.</span><span class="sxs-lookup"><span data-stu-id="e4975-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="e4975-153">(Не создавать новый контекст данных). Нажмите кнопку **добавить** Добавление контроллера.</span><span class="sxs-lookup"><span data-stu-id="e4975-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

>[!div class="step-by-step"]
<span data-ttu-id="e4975-154">[Назад](part-1.md)
[Вперед](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="e4975-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
