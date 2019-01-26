---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Шаблон: Реализация наследования с использованием EF в приложениях ASP.NET MVC 5'
description: В этом учебнике демонстрируется, как реализовать наследование в модели данных.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 79513edce7ac3044f6f547149400cba7d307edfa
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889903"
---
# <a name="template-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a><span data-ttu-id="ca983-103">Шаблон: Реализация наследования с использованием EF в приложении ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="ca983-103">Template: Implement Inheritance with EF in an ASP.NET MVC 5 app</span></span>

<span data-ttu-id="ca983-104">В предыдущем учебнике показана обработка исключений параллелизма.</span><span class="sxs-lookup"><span data-stu-id="ca983-104">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="ca983-105">В этом учебнике демонстрируется, как реализовать наследование в модели данных.</span><span class="sxs-lookup"><span data-stu-id="ca983-105">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="ca983-106">В объектно ориентированного программирования, можно использовать [наследования](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) для упрощения [повторное использование кода](http://en.wikipedia.org/wiki/Code_reuse).</span><span class="sxs-lookup"><span data-stu-id="ca983-106">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="ca983-107">В рамках этого учебника вы измените классы `Instructor` и `Student` таким образом, чтобы они были производными от базового класса `Person`, который содержит общие свойства для преподавателей и учащихся, такие как `LastName`.</span><span class="sxs-lookup"><span data-stu-id="ca983-107">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="ca983-108">Изменения вносятся в коде, а не на веб-страницах, и автоматически отражаются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ca983-108">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

<span data-ttu-id="ca983-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="ca983-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ca983-110">Узнайте, как сопоставить наследования для базы данных</span><span class="sxs-lookup"><span data-stu-id="ca983-110">Learn to map inheritance to database</span></span>
> * <span data-ttu-id="ca983-111">Создание класса Person</span><span class="sxs-lookup"><span data-stu-id="ca983-111">Create the Person class</span></span>
> * <span data-ttu-id="ca983-112">Обновление Instructor и Student</span><span class="sxs-lookup"><span data-stu-id="ca983-112">Update Instructor and Student</span></span>
> * <span data-ttu-id="ca983-113">Добавить пользователя в модели</span><span class="sxs-lookup"><span data-stu-id="ca983-113">Add Person to the Model</span></span>
> * <span data-ttu-id="ca983-114">Создание и обновление миграции</span><span class="sxs-lookup"><span data-stu-id="ca983-114">Create and Update Migrations</span></span>
> * <span data-ttu-id="ca983-115">Протестировать реализацию</span><span class="sxs-lookup"><span data-stu-id="ca983-115">Test the implementation</span></span>
> * <span data-ttu-id="ca983-116">Развертывание в Azure</span><span class="sxs-lookup"><span data-stu-id="ca983-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ca983-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ca983-117">Prerequisites</span></span>

* [<span data-ttu-id="ca983-118">Реализация наследования</span><span class="sxs-lookup"><span data-stu-id="ca983-118">Implementing Inheritance</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a><span data-ttu-id="ca983-119">Сопоставление наследования для базы данных</span><span class="sxs-lookup"><span data-stu-id="ca983-119">Map inheritance to database</span></span>

<span data-ttu-id="ca983-120">`Instructor` И `Student` классы в `School` модели данных имеют несколько свойств, которые идентичны:</span><span class="sxs-lookup"><span data-stu-id="ca983-120">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="ca983-122">Предположим, что вам требуется исключить повторяющийся код для свойств, которые являются общими для сущностей `Instructor` и `Student`.</span><span class="sxs-lookup"><span data-stu-id="ca983-122">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="ca983-123">Кроме того, вам может потребоваться написать службу, которая может форматировать имена как преподавателей, так и учащихся.</span><span class="sxs-lookup"><span data-stu-id="ca983-123">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="ca983-124">Можно создать `Person` базовый класс, который содержит только те общие свойства, а затем сделать `Instructor` и `Student` сущности являются производными от этого базового класса, как показано на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="ca983-124">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="ca983-126">Структура наследования может быть представлена в базе данных несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="ca983-126">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="ca983-127">Имеется `Person` таблицу, содержащую сведения об учащихся и преподавателей в одной таблице.</span><span class="sxs-lookup"><span data-stu-id="ca983-127">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="ca983-128">Некоторые столбцы могут относиться только к преподавателям (`HireDate`), некоторые только к учащимся (`EnrollmentDate`), некоторые для обоих (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="ca983-128">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="ca983-129">Как правило, пришлось бы *дискриминатора* представляет столбец, который указывает, какой тип каждой строки.</span><span class="sxs-lookup"><span data-stu-id="ca983-129">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="ca983-130">Например, в столбце дискриминатора может указываться значение "Instructor" для преподавателей и "Student" для учащихся.</span><span class="sxs-lookup"><span data-stu-id="ca983-130">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Таблицы на hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="ca983-132">Такая модель создания структуры наследования сущностей из одной таблицы базы данных называется *таблица на иерархию* наследования (TPH).</span><span class="sxs-lookup"><span data-stu-id="ca983-132">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="ca983-133">В качестве альтернативы можно создать базу данных, которая будет иметь приближенный к структуре наследования вид.</span><span class="sxs-lookup"><span data-stu-id="ca983-133">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="ca983-134">Например, может иметь только поля с именами `Person` таблицы и иметь отдельные `Instructor` и `Student` таблицы с полями даты.</span><span class="sxs-lookup"><span data-stu-id="ca983-134">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Таблицы на type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="ca983-136">Такая модель создания таблицы базы данных для каждого класса сущности называется *одна таблица на тип* наследование (TPT).</span><span class="sxs-lookup"><span data-stu-id="ca983-136">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="ca983-137">Кроме того, можно сопоставить все не являющиеся абстрактными типы с отдельными таблицами.</span><span class="sxs-lookup"><span data-stu-id="ca983-137">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="ca983-138">Все свойства класса, включая унаследованные, сопоставляются со столбцами в соответствующей таблице.</span><span class="sxs-lookup"><span data-stu-id="ca983-138">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="ca983-139">Такая модель называется наследованием типа "одна таблица на конкретный класс".</span><span class="sxs-lookup"><span data-stu-id="ca983-139">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="ca983-140">Если вы реализовали наследование TPC для `Person`, `Student`, и `Instructor` классы, как показано выше, `Student` и `Instructor` таблицы будет выглядеть после реализации наследования, чем раньше не отличается.</span><span class="sxs-lookup"><span data-stu-id="ca983-140">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="ca983-141">TPC и шаблоны наследование TPH как правило, обеспечивают более высокую производительность на платформе Entity Framework, чем шаблоны наследования TPT, так как шаблоны TPT может привести к сложные запросы на соединение.</span><span class="sxs-lookup"><span data-stu-id="ca983-141">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="ca983-142">В этом учебнике демонстрируется реализация модели наследования "одна таблица на иерархию".</span><span class="sxs-lookup"><span data-stu-id="ca983-142">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="ca983-143">TPH является шаблон наследования по умолчанию в Entity Framework, поэтому все, что необходимо сделать это создание `Person` измените `Instructor` и `Student` классам наследовать от `Person`, добавьте новый класс `DbContext`и создание Миграция.</span><span class="sxs-lookup"><span data-stu-id="ca983-143">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="ca983-144">(Сведения о том, как реализовать другие схемы наследования, см. в разделе [сопоставление наследования таблица на тип (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) и [таблица на конкретный класс (TPC) наследование сопоставления](https://msdn.microsoft.com/data/jj591617#2.6) в MSDN Документация по Entity Framework.)</span><span class="sxs-lookup"><span data-stu-id="ca983-144">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="ca983-145">Создание класса Person</span><span class="sxs-lookup"><span data-stu-id="ca983-145">Create the Person class</span></span>

<span data-ttu-id="ca983-146">В *моделей* папке создайте *Person.cs* и замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ca983-146">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a><span data-ttu-id="ca983-147">Обновление Instructor и Student</span><span class="sxs-lookup"><span data-stu-id="ca983-147">Update Instructor and Student</span></span>

<span data-ttu-id="ca983-148">Теперь обновите *Instructor.cs* и *Sudent.cs* наследование значений из *Person.sc*.</span><span class="sxs-lookup"><span data-stu-id="ca983-148">Now update the *Instructor.cs* and *Sudent.cs* to inherit values from the *Person.sc*.</span></span>

<span data-ttu-id="ca983-149">В *Instructor.cs*, являются производными `Instructor` класса из `Person` класса и удалите поля ключа и имени.</span><span class="sxs-lookup"><span data-stu-id="ca983-149">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="ca983-150">Код будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ca983-150">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="ca983-151">Внести аналогичные изменения к *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="ca983-151">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="ca983-152">`Student` Класса будет выглядеть как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="ca983-152">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a><span data-ttu-id="ca983-153">Добавить пользователя в модели</span><span class="sxs-lookup"><span data-stu-id="ca983-153">Add Person to the Model</span></span>

<span data-ttu-id="ca983-154">В *SchoolContext.cs*, добавьте `DbSet` свойство для `Person` типа сущности:</span><span class="sxs-lookup"><span data-stu-id="ca983-154">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="ca983-155">Это все, что требуется платформе Entity Framework для настройки наследования типа "одна таблица на иерархию".</span><span class="sxs-lookup"><span data-stu-id="ca983-155">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="ca983-156">Как вы увидите, при обновлении базы данных, он будет иметь `Person` таблицы вместо `Student` и `Instructor` таблицы.</span><span class="sxs-lookup"><span data-stu-id="ca983-156">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-migrations"></a><span data-ttu-id="ca983-157">Создание и обновление миграции</span><span class="sxs-lookup"><span data-stu-id="ca983-157">Create and Update Migrations</span></span>

<span data-ttu-id="ca983-158">В консоли диспетчера пакетов (PMC), введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ca983-158">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="ca983-159">Запустите `Update-Database` команду в PMC.</span><span class="sxs-lookup"><span data-stu-id="ca983-159">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="ca983-160">Команда завершится ошибкой на этом этапе, так как у нас есть миграции не знает как обрабатывать существующие данные.</span><span class="sxs-lookup"><span data-stu-id="ca983-160">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="ca983-161">Вы получаете сообщение об ошибке, аналогичный приведенному ниже:</span><span class="sxs-lookup"><span data-stu-id="ca983-161">You get an error message like the following one:</span></span>

> <span data-ttu-id="ca983-162">*Невозможно удалить объект "dbo. Преподаватель "так как он ссылается ограничение FOREIGN KEY.*</span><span class="sxs-lookup"><span data-stu-id="ca983-162">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>


<span data-ttu-id="ca983-163">Откройте *миграций\&lt; метка времени&gt;\_Inheritance.cs* и замените `Up` метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ca983-163">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="ca983-164">Этот код выполняет следующие задачи по обновлению базы данных:</span><span class="sxs-lookup"><span data-stu-id="ca983-164">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="ca983-165">Удаляет ограничения внешнего ключа и индексы, которые указывают на таблицу Student.</span><span class="sxs-lookup"><span data-stu-id="ca983-165">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="ca983-166">Переименовывает таблицу Instructor в Person и вносит изменения, необходимые для сохранения в ней данных из таблицы Student:</span><span class="sxs-lookup"><span data-stu-id="ca983-166">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="ca983-167">Добавляет допускающий значения NULL тип EnrollmentDate для учащихся.</span><span class="sxs-lookup"><span data-stu-id="ca983-167">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="ca983-168">Добавляет столбец дискриминатора, который указывает, представляет ли строка учащегося или преподавателя.</span><span class="sxs-lookup"><span data-stu-id="ca983-168">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="ca983-169">Изменяет тип HireDate на допускающий значения NULL, поскольку в строках для учащихся не будет указываться дата приема на работу.</span><span class="sxs-lookup"><span data-stu-id="ca983-169">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="ca983-170">Добавляет временное поле, которое будет использоваться для обновления внешних ключей, указывающих на учащихся.</span><span class="sxs-lookup"><span data-stu-id="ca983-170">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="ca983-171">При копировании записей учащихся в таблицу Person они получат новые значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="ca983-171">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="ca983-172">Копирует данные из таблицы Student в таблицу Person.</span><span class="sxs-lookup"><span data-stu-id="ca983-172">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="ca983-173">При этом записям учащихся назначаются новые значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="ca983-173">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="ca983-174">Исправляет значения внешнего ключа, которые указывают на учащихся.</span><span class="sxs-lookup"><span data-stu-id="ca983-174">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="ca983-175">Повторно создает ограничения внешнего ключа и индексы, которые после этого указывают на таблицу Person.</span><span class="sxs-lookup"><span data-stu-id="ca983-175">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="ca983-176">(Если вместо целочисленного типа первичного ключа используется GUID, значения первичного ключа для записей учащихся изменять не потребуется и некоторые из этих действий можно пропустить.)</span><span class="sxs-lookup"><span data-stu-id="ca983-176">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="ca983-177">Запустите `update-database` еще раз выполните команду.</span><span class="sxs-lookup"><span data-stu-id="ca983-177">Run the `update-database` command again.</span></span>

<span data-ttu-id="ca983-178">(В рабочей системе вы вносите соответствующие изменения для метода вниз в случае, если вам когда-либо приходилось использовать, чтобы вернуться к предыдущей версии базы данных.</span><span class="sxs-lookup"><span data-stu-id="ca983-178">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="ca983-179">В этом руководстве вы не используете метод вниз.)</span><span class="sxs-lookup"><span data-stu-id="ca983-179">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="ca983-180">Это можно получить другие ошибки при переносе данных и внесения изменений схемы.</span><span class="sxs-lookup"><span data-stu-id="ca983-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="ca983-181">Если вы получаете ошибки миграции не удается устранить, можно будет продолжить руководства, изменив строку подключения в *Web.config* файла или путем удаления базы данных.</span><span class="sxs-lookup"><span data-stu-id="ca983-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="ca983-182">Самым простым подходом является переименовать базу данных, в *Web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="ca983-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="ca983-183">Например измените имя базы данных на ContosoUniversity2, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="ca983-183">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> <span data-ttu-id="ca983-184">В новой базе данных нет данных для переноса и `update-database` команда является гораздо большей долей вероятности завершится без ошибок.</span><span class="sxs-lookup"><span data-stu-id="ca983-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="ca983-185">Инструкции о том, как удалить базу данных, см. в разделе [как удалить базу данных из Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="ca983-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="ca983-186">При использовании этого подхода для продолжения работы с учебником пропустить шаг развертывания в конце этого руководства, или развернуть для нового сайта и базы данных.</span><span class="sxs-lookup"><span data-stu-id="ca983-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="ca983-187">Если вы развернете обновление на тот же сайт, на которой развертывание уже, EF получите та же ошибка, при выполнении миграции автоматически.</span><span class="sxs-lookup"><span data-stu-id="ca983-187">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="ca983-188">Если вы хотите устранении ошибки миграции, самый лучший ресурс является одним из форумы Entity Framework или StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="ca983-188">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>

## <a name="test-the-implementation"></a><span data-ttu-id="ca983-189">Протестировать реализацию</span><span class="sxs-lookup"><span data-stu-id="ca983-189">Test the implementation</span></span>

<span data-ttu-id="ca983-190">Запустите сайт и попробуйте открыть различные страницы.</span><span class="sxs-lookup"><span data-stu-id="ca983-190">Run the site and try various pages.</span></span> <span data-ttu-id="ca983-191">Все работает так же, как и раньше.</span><span class="sxs-lookup"><span data-stu-id="ca983-191">Everything works the same as it did before.</span></span>

<span data-ttu-id="ca983-192">В **обозреватель серверов,** разверните **Connections\SchoolContext данных** и затем **таблиц**, и вы увидите, что **учащихся** и **Преподавателя** таблиц были заменены **Person** таблицы.</span><span class="sxs-lookup"><span data-stu-id="ca983-192">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="ca983-193">Разверните **Person** таблицы чтобы увидеть, все столбцы, которые ранее были в наличии **учащихся** и **преподавателя** таблиц.</span><span class="sxs-lookup"><span data-stu-id="ca983-193">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

<span data-ttu-id="ca983-194">Щелкните таблицу Person правой кнопкой мыши и выберите команду **Показать данные таблицы**, чтобы просмотреть столбец дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="ca983-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

<span data-ttu-id="ca983-195">На следующей схеме показана структура новой базы данных School:</span><span class="sxs-lookup"><span data-stu-id="ca983-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="ca983-197">Развертывание в Azure</span><span class="sxs-lookup"><span data-stu-id="ca983-197">Deploy to Azure</span></span>

<span data-ttu-id="ca983-198">В этом разделе, необходимо завершить необязательный **развертыванию приложения в Azure** статьи [части 3, сортировку, фильтрацию и разбиение по страницам](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="ca983-198">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="ca983-199">Если у вас есть ошибки миграции, которые можно разрешить путем удаления базы данных в локальном проекте, пропустите этот шаг; или создайте новый сайт и базы данных, а развертывание в новую среду.</span><span class="sxs-lookup"><span data-stu-id="ca983-199">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="ca983-200">В Visual Studio щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **публикации** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="ca983-200">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>

2. <span data-ttu-id="ca983-201">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="ca983-201">Click **Publish**.</span></span>

    <span data-ttu-id="ca983-202">Веб-приложение откроется в браузере по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ca983-202">The Web app opens in your default browser.</span></span>

3. <span data-ttu-id="ca983-203">Протестируйте приложение, чтобы проверить его работы.</span><span class="sxs-lookup"><span data-stu-id="ca983-203">Test the application to verify it's working.</span></span>

    <span data-ttu-id="ca983-204">При первом запуске страницы, который обращается к базе данных, платформа Entity Framework выполняет все миграции `Up` методы, необходимые для приведения базы данных в актуальном состоянии с текущей моделью данных.</span><span class="sxs-lookup"><span data-stu-id="ca983-204">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="ca983-205">Получение кода</span><span class="sxs-lookup"><span data-stu-id="ca983-205">Get the code</span></span>

[<span data-ttu-id="ca983-206">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="ca983-206">Download Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="ca983-207">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ca983-207">Additional resources</span></span>

<span data-ttu-id="ca983-208">Ссылки на другие ресурсы Entity Framework можно найти в [доступ к данным ASP.NET — рекомендуемые ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="ca983-208">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="ca983-209">Дополнительные сведения об этом и других структур наследования см. в разделе [модель наследования TPT](https://msdn.microsoft.com/data/jj618293) и [шаблон наследование TPH](https://msdn.microsoft.com/data/jj618292) на сайте MSDN.</span><span class="sxs-lookup"><span data-stu-id="ca983-209">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="ca983-210">В рамках следующего учебника вы узнаете, как работать в нескольких сценариях Entity Framework с расширенными возможностями.</span><span class="sxs-lookup"><span data-stu-id="ca983-210">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ca983-211">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="ca983-211">Next steps</span></span>

<span data-ttu-id="ca983-212">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="ca983-212">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ca983-213">Узнали для сопоставления наследования для базы данных</span><span class="sxs-lookup"><span data-stu-id="ca983-213">Learned to map inheritance to database</span></span>
> * <span data-ttu-id="ca983-214">Создан класс Person</span><span class="sxs-lookup"><span data-stu-id="ca983-214">Created the Person class</span></span>
> * <span data-ttu-id="ca983-215">Обновленные Instructor и Student</span><span class="sxs-lookup"><span data-stu-id="ca983-215">Updated Instructor and Student</span></span>
> * <span data-ttu-id="ca983-216">Добавлена Person в модель</span><span class="sxs-lookup"><span data-stu-id="ca983-216">Added Person to the Model</span></span>
> * <span data-ttu-id="ca983-217">Создать и обновить миграции</span><span class="sxs-lookup"><span data-stu-id="ca983-217">Created and Update Migrations</span></span>
> * <span data-ttu-id="ca983-218">Тестирование реализации</span><span class="sxs-lookup"><span data-stu-id="ca983-218">Tested the implementation</span></span>
> * <span data-ttu-id="ca983-219">Развертывания в Azure</span><span class="sxs-lookup"><span data-stu-id="ca983-219">Deployed to Azure</span></span>

<span data-ttu-id="ca983-220">Перейдите к следующей статьи вы узнаете о разделах, которые необходимо учитывать при переходе от основ разработки веб-приложений ASP.NET, использующих Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="ca983-220">Advance to the next article to learn about topics that are useful to be aware of when you go beyond the basics of developing ASP.NET web applications that use Entity Framework Code First.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="ca983-221">Дополнительные сценарии платформы Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ca983-221">Advanced Entity Framework Scenarios</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)