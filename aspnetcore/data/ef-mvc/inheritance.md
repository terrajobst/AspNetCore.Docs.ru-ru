---
title: "Основные ASP.NET MVC с основными EF - наследования - 9, 10"
author: tdykstra
description: "Этот учебник будет показано, как реализовать наследование в модели данных с использованием Entity Framework Core в приложении ASP.NET Core."
keywords: "ASP.NET Core, Entity Framework Core, наследования"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 41dc0db7-6f17-453e-aba6-633430609c74
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 10bde121dac3bdbbf0e55f2d146d91dea0f0210f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a><span data-ttu-id="3d395-104">Наследование - Core EF учебнику ASP.NET Core MVC (9, 10)</span><span class="sxs-lookup"><span data-stu-id="3d395-104">Inheritance - EF Core with ASP.NET Core MVC tutorial (9 of 10)</span></span>

<span data-ttu-id="3d395-105">По [Tom Dykstra](https://github.com/tdykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3d395-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3d395-106">Contoso университета примера веб-приложения показано, как создавать веб-приложения ASP.NET MVC ядра с помощью основного Entity Framework и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3d395-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="3d395-107">Сведения о учебника серии см [в первом учебнике ряда](intro.md).</span><span class="sxs-lookup"><span data-stu-id="3d395-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="3d395-108">В предыдущем учебнике обработки исключений параллелизма.</span><span class="sxs-lookup"><span data-stu-id="3d395-108">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="3d395-109">Этот учебник будет показано, как реализовать наследование в модели данных.</span><span class="sxs-lookup"><span data-stu-id="3d395-109">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="3d395-110">В объектно ориентированного программирования, можно использовать наследование для упрощения повторного использования кода.</span><span class="sxs-lookup"><span data-stu-id="3d395-110">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="3d395-111">В этом учебнике мы изменим `Instructor` и `Student` классов, чтобы они являются производными от `Person` базового класса, который содержит свойства, такие как `LastName` , которые являются общими для преподавателей и учащихся.</span><span class="sxs-lookup"><span data-stu-id="3d395-111">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="3d395-112">Не добавить или изменить веб-страницы, но мы изменим некоторую часть кода, и эти изменения будут автоматически отражаться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3d395-112">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="3d395-113">Параметры для сопоставления наследования с таблицами базы данных</span><span class="sxs-lookup"><span data-stu-id="3d395-113">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="3d395-114">`Instructor` И `Student` классы в модели данных школе имеют несколько свойств, которые идентичны:</span><span class="sxs-lookup"><span data-stu-id="3d395-114">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Классы учащихся и инструкторов](inheritance/_static/no-inheritance.png)

<span data-ttu-id="3d395-116">Предположим, что нужно исключить избыточный код для свойств, которые являются общими для `Instructor` и `Student` сущности.</span><span class="sxs-lookup"><span data-stu-id="3d395-116">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="3d395-117">Или вы хотите создать службу, имена форматов без интересуясь ли имя, откуда инструктор или студент.</span><span class="sxs-lookup"><span data-stu-id="3d395-117">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="3d395-118">Можно создать `Person` базового класса, который содержит только те свойства общего, а затем сделать `Instructor` и `Student` классы наследуются от базового класса, как показано на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="3d395-118">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Учащихся и инструкторов классы, производные от класса Person](inheritance/_static/inheritance.png)

<span data-ttu-id="3d395-120">Существует несколько способов эта структура наследования можно представить в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3d395-120">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="3d395-121">Можно создать таблицу Person, включая сведения о студентов и инструкторов в одной таблице.</span><span class="sxs-lookup"><span data-stu-id="3d395-121">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="3d395-122">Некоторые столбцы могут применяются только к инструкторов (HireDate) к учащихся (EnrollmentDate), некоторые как параметр (Фамилия, имя).</span><span class="sxs-lookup"><span data-stu-id="3d395-122">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="3d395-123">Как правило имеется столбец дискриминатора, чтобы указать, какой тип, каждая строка представляет.</span><span class="sxs-lookup"><span data-stu-id="3d395-123">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="3d395-124">Например столбец дискриминатора установлена «Инструктора» для инструкторов и «Студентов» для учащихся.</span><span class="sxs-lookup"><span data-stu-id="3d395-124">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Пример таблица на иерархию](inheritance/_static/tph.png)

<span data-ttu-id="3d395-126">Этот шаблон создания структура наследования сущностей из одной таблицы базы данных называется таблица на иерархию наследования (TPH).</span><span class="sxs-lookup"><span data-stu-id="3d395-126">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="3d395-127">Альтернативой является база данных больше похожи на структуру наследования.</span><span class="sxs-lookup"><span data-stu-id="3d395-127">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="3d395-128">Например можно только имя поля из таблицы Person и иметь отдельные таблицы с полями даты инструктора и студентов.</span><span class="sxs-lookup"><span data-stu-id="3d395-128">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Наследование типа «одна таблица на тип»](inheritance/_static/tpt.png)

<span data-ttu-id="3d395-130">Этот шаблон создания таблицы базы данных для каждого класса сущности, называется одна таблица на тип (TPT) наследования.</span><span class="sxs-lookup"><span data-stu-id="3d395-130">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="3d395-131">Еще другой вариант — Сопоставьте все типы неабстрактного для отдельных таблиц.</span><span class="sxs-lookup"><span data-stu-id="3d395-131">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="3d395-132">Все свойства класса, включая унаследованные свойства сопоставляются со столбцами и соответствующая таблица.</span><span class="sxs-lookup"><span data-stu-id="3d395-132">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="3d395-133">Этот шаблон вызывается наследования таблицы каждой конкретной реализацией класса (TPC).</span><span class="sxs-lookup"><span data-stu-id="3d395-133">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="3d395-134">Если реализован TPC наследования для человека, учащихся и инструкторов классов, как показано выше, таблицами Student и инструктора выглядит не отличается после реализации наследования, чем раньше.</span><span class="sxs-lookup"><span data-stu-id="3d395-134">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="3d395-135">TPC и TPH схемы наследования обычно обеспечивают лучшую производительность, чем TPT схемы наследования, поскольку TPT шаблоны запросов сложного соединения может привести.</span><span class="sxs-lookup"><span data-stu-id="3d395-135">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="3d395-136">Этот учебник демонстрирует реализацию TPH наследования.</span><span class="sxs-lookup"><span data-stu-id="3d395-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="3d395-137">TPH является только наследование, поддерживающий Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3d395-137">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="3d395-138">Вы выполните — создать `Person` класса, измените `Instructor` и `Student` классы являются производными от `Person`, добавьте новый класс `DbContext`и создание миграции.</span><span class="sxs-lookup"><span data-stu-id="3d395-138">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP] 
> <span data-ttu-id="3d395-139">Рекомендуется сохранить копию проекта перед вносит следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="3d395-139">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="3d395-140">Затем Если возникли проблемы и нужно начать заново его будет проще запустить из сохраненного проекта вместо обращения действия выполняется для этого учебника, или будет в начало весь ряд.</span><span class="sxs-lookup"><span data-stu-id="3d395-140">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="3d395-141">Создание класса Person</span><span class="sxs-lookup"><span data-stu-id="3d395-141">Create the Person class</span></span>

<span data-ttu-id="3d395-142">В папке Models находится создайте Person.cs и замените код шаблона с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="3d395-142">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="3d395-143">Учащихся и инструкторов классы наследуют от пользователя сделать</span><span class="sxs-lookup"><span data-stu-id="3d395-143">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="3d395-144">В *Instructor.cs*, класс инструктора производный от класса Person и удалить ключа и имя поля.</span><span class="sxs-lookup"><span data-stu-id="3d395-144">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="3d395-145">Код будет выглядеть как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="3d395-145">The code will look like the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="3d395-146">Внесения изменений в *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="3d395-146">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="3d395-147">Добавить в модель данных сущности типа Person</span><span class="sxs-lookup"><span data-stu-id="3d395-147">Add the Person entity type to the data model</span></span>

<span data-ttu-id="3d395-148">Добавление типа сущности пользователя для *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="3d395-148">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="3d395-149">Новые строки будут выделены.</span><span class="sxs-lookup"><span data-stu-id="3d395-149">The new lines are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="3d395-150">Это все, что платформа Entity Framework требуется для настройки таблица на иерархию наследования.</span><span class="sxs-lookup"><span data-stu-id="3d395-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="3d395-151">Как вы увидите, при обновлении базы данных, он будет иметь таблицы Person вместо таблицами Student и инструктора.</span><span class="sxs-lookup"><span data-stu-id="3d395-151">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="3d395-152">Создание и настройка миграции кода</span><span class="sxs-lookup"><span data-stu-id="3d395-152">Create and customize migration code</span></span>

<span data-ttu-id="3d395-153">Сохраните изменения и выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="3d395-153">Save your changes and build the project.</span></span> <span data-ttu-id="3d395-154">Затем откройте командную строку в папке проекта и введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="3d395-154">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="3d395-155">Не запускайте `database update` еще команды.</span><span class="sxs-lookup"><span data-stu-id="3d395-155">Don't run the `database update` command yet.</span></span> <span data-ttu-id="3d395-156">Этой команды приведет к потере данных, так как он будет удалить таблицу инструктора и переименовать таблицу студента лицу.</span><span class="sxs-lookup"><span data-stu-id="3d395-156">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="3d395-157">Необходимо предоставить пользовательский код для сохранения данных.</span><span class="sxs-lookup"><span data-stu-id="3d395-157">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="3d395-158">Откройте *миграций /\<timestamp > _Inheritance.cs* и замените `Up` метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="3d395-158">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="3d395-159">Этот код отвечает за следующие задачи обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="3d395-159">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="3d395-160">Удаление ограничения внешнего ключа и индексы, которые указывают на таблицу учащихся.</span><span class="sxs-lookup"><span data-stu-id="3d395-160">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="3d395-161">Переименовывает таблицу в качестве лица инструктора и вносит изменения, необходимые для хранения данных студента:</span><span class="sxs-lookup"><span data-stu-id="3d395-161">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="3d395-162">Добавляет EnrollmentDate допускает значения NULL для учащихся.</span><span class="sxs-lookup"><span data-stu-id="3d395-162">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="3d395-163">Добавляет столбец дискриминатора для указания, является ли строка для учащихся или инструктор.</span><span class="sxs-lookup"><span data-stu-id="3d395-163">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="3d395-164">Делает HireDate допускает значение NULL, поскольку студента строк не будет иметь дат приема на работу.</span><span class="sxs-lookup"><span data-stu-id="3d395-164">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="3d395-165">Добавление временного поля, которое будет использоваться для обновления внешние ключи, указывающие на учащихся.</span><span class="sxs-lookup"><span data-stu-id="3d395-165">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="3d395-166">При копировании студентов в таблице Person, где они могут получить новые значения первичных ключей.</span><span class="sxs-lookup"><span data-stu-id="3d395-166">When you copy students into the Person table they'll get new primary key values.</span></span>

* <span data-ttu-id="3d395-167">Копирует данные из таблицы студента в таблицу Person.</span><span class="sxs-lookup"><span data-stu-id="3d395-167">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="3d395-168">В этом случае учащихся получить назначения нового значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="3d395-168">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="3d395-169">Устраняет значения внешнего ключа, указывающие на учащихся.</span><span class="sxs-lookup"><span data-stu-id="3d395-169">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="3d395-170">Повторно создает ограничения внешнего ключа и индексы, теперь указывает таблицы Person.</span><span class="sxs-lookup"><span data-stu-id="3d395-170">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="3d395-171">(Если вы использовали GUID вместо целое число в качестве первичного ключа типа, студента значения первичного ключа не пришлось изменять и удалось некоторые из них была пропущена.)</span><span class="sxs-lookup"><span data-stu-id="3d395-171">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="3d395-172">Запустите `database update` команды:</span><span class="sxs-lookup"><span data-stu-id="3d395-172">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="3d395-173">(В рабочей системе будет внести соответствующие изменения в `Down` метод в случае, когда-либо было необходимо использовать, чтобы вернуться к предыдущей версии базы данных.</span><span class="sxs-lookup"><span data-stu-id="3d395-173">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="3d395-174">В этом учебнике вы не используете `Down` метод.)</span><span class="sxs-lookup"><span data-stu-id="3d395-174">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE] 
> <span data-ttu-id="3d395-175">Можно получить другие ошибки при внесении изменений схемы в базу данных с существующие данные.</span><span class="sxs-lookup"><span data-stu-id="3d395-175">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="3d395-176">Если возникли ошибки миграции, которые не удается устранить, можно изменить имя базы данных в строке соединения или удалить базу данных.</span><span class="sxs-lookup"><span data-stu-id="3d395-176">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="3d395-177">Нет данных для переноса с новой базой данных, и команду update-database чаще завершиться без ошибок.</span><span class="sxs-lookup"><span data-stu-id="3d395-177">With a new database, there is no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="3d395-178">Чтобы удалить базу данных, используйте SSOX или запустите `database drop` команду CLI.</span><span class="sxs-lookup"><span data-stu-id="3d395-178">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="3d395-179">Тестирование с помощью наследования реализации</span><span class="sxs-lookup"><span data-stu-id="3d395-179">Test with inheritance implemented</span></span>

<span data-ttu-id="3d395-180">Запустите приложение и попробуйте различные страницы.</span><span class="sxs-lookup"><span data-stu-id="3d395-180">Run the app and try various pages.</span></span> <span data-ttu-id="3d395-181">Все, что работает так же, как раньше.</span><span class="sxs-lookup"><span data-stu-id="3d395-181">Everything works the same as it did before.</span></span>

<span data-ttu-id="3d395-182">В **обозреватель объектов SQL Server**, разверните **данные соединения и SchoolContext** и затем **таблиц**, и вы увидите, что таблицами Student и инструктора были заменены Таблицы Person.</span><span class="sxs-lookup"><span data-stu-id="3d395-182">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="3d395-183">Откройте в конструкторе таблиц Person и вы увидите, что все столбцы, которые используются в таблицах учащихся и инструкторов наличии.</span><span class="sxs-lookup"><span data-stu-id="3d395-183">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Таблица Person в SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="3d395-185">Щелкните правой кнопкой мыши таблицу Person и нажмите кнопку **Показать таблицу данных** в столбце дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="3d395-185">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Таблица Person в SSOX - таблицы данных](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="3d395-187">Сводка</span><span class="sxs-lookup"><span data-stu-id="3d395-187">Summary</span></span>

<span data-ttu-id="3d395-188">Таблица на иерархию наследования для реализации `Person`, `Student`, и `Instructor` классы.</span><span class="sxs-lookup"><span data-stu-id="3d395-188">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="3d395-189">Дополнительные сведения о наследовании в Entity Framework Core см. в разделе [наследования](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="3d395-189">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="3d395-190">В следующем уроке вы увидите, как обрабатывать разнообразные относительно сложных сценариев Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3d395-190">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3d395-191">[Назад](concurrency.md)
[Вперед](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="3d395-191">[Previous](concurrency.md)
[Next](advanced.md)</span></span>  
