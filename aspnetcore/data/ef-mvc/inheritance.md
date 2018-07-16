---
title: ASP.NET Core MVC с EF Core — наследование — 9 из 10
author: rick-anderson
description: В этом учебнике показано, как реализовать наследование в модели данных с использованием платформы Entity Framework Core в приложении ASP.NET Core.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/inheritance
ms.openlocfilehash: a71954297f44f936893a7f1e9d3b0685f81378b9
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126708"
---
# <a name="aspnet-core-mvc-with-ef-core---inheritance---9-of-10"></a><span data-ttu-id="6ab0c-103">ASP.NET Core MVC с EF Core — наследование — 9 из 10</span><span class="sxs-lookup"><span data-stu-id="6ab0c-103">ASP.NET Core MVC with EF Core - Inheritance - 9 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6ab0c-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="6ab0c-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6ab0c-105">На примере учебного веб-приложения "Университет Contoso" демонстрируется процесс создания веб-приложений ASP.NET Core MVC с помощью Entity Framework Core и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="6ab0c-106">Сведения о серии руководств см. в [первом руководстве серии](intro.md).</span><span class="sxs-lookup"><span data-stu-id="6ab0c-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="6ab0c-107">В предыдущем учебнике была показана обработка исключений параллелизма.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-107">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="6ab0c-108">В этом учебнике демонстрируется, как реализовать наследование в модели данных.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-108">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="6ab0c-109">В объектно-ориентированном программировании наследование применяется для оптимизации повторного использования кода.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-109">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="6ab0c-110">В рамках этого учебника вы измените классы `Instructor` и `Student` таким образом, чтобы они были производными от базового класса `Person`, который содержит общие свойства для преподавателей и учащихся, такие как `LastName`.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-110">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="6ab0c-111">Изменения вносятся в коде, а не на веб-страницах, и автоматически отражаются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-111">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="6ab0c-112">Варианты сопоставления наследования для таблиц базы данных</span><span class="sxs-lookup"><span data-stu-id="6ab0c-112">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="6ab0c-113">Классы `Instructor` и `Student` в модели данных School имеют несколько идентичных свойств:</span><span class="sxs-lookup"><span data-stu-id="6ab0c-113">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Классы Student и Instructor](inheritance/_static/no-inheritance.png)

<span data-ttu-id="6ab0c-115">Предположим, что вам требуется исключить повторяющийся код для свойств, которые являются общими для сущностей `Instructor` и `Student`.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-115">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="6ab0c-116">Кроме того, вам может потребоваться написать службу, которая может форматировать имена как преподавателей, так и учащихся.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-116">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="6ab0c-117">В таких сценариях можно создать базовый класс `Person`, который содержит только общие свойства, а затем унаследовать от него классы `Instructor` и `Student`, как показано на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="6ab0c-117">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Классы Student и Instructor, производные от класса Person](inheritance/_static/inheritance.png)

<span data-ttu-id="6ab0c-119">Структура наследования может быть представлена в базе данных несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-119">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="6ab0c-120">Вы можете создать таблицу Person, которая будет содержать одновременно информацию о преподавателях и учащихся.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-120">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="6ab0c-121">Некоторые столбцы могут относиться только к преподавателям (HireDate), некоторые только к учащимся (EnrollmentDate), а некоторые одновременно к обеим сущностям (LastName, FirstName).</span><span class="sxs-lookup"><span data-stu-id="6ab0c-121">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="6ab0c-122">Как правило, используется столбец дискриминатора, который указывает на тип, представленный соответствующей строкой.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-122">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="6ab0c-123">Например, в столбце дискриминатора может указываться значение "Instructor" для преподавателей и "Student" для учащихся.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-123">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Пример наследования типа "одна таблица на иерархию"](inheritance/_static/tph.png)

<span data-ttu-id="6ab0c-125">Такая модель, описывающая формирование структуры наследования сущностей на основе одной таблицы базы данных, называется наследованием типа "одна таблица на иерархию".</span><span class="sxs-lookup"><span data-stu-id="6ab0c-125">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="6ab0c-126">В качестве альтернативы можно создать базу данных, которая будет иметь приближенный к структуре наследования вид.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-126">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="6ab0c-127">Например, можно хранить в таблице Person только поля с именами и создать отдельные таблицы Instructor и Student с полями дат.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-127">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Наследование типа «одна таблица на тип»](inheritance/_static/tpt.png)

<span data-ttu-id="6ab0c-129">Такая модель создания таблицы базы данных для каждого класса сущности называется наследованием типа "одна таблица на тип".</span><span class="sxs-lookup"><span data-stu-id="6ab0c-129">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="6ab0c-130">Кроме того, можно сопоставить все не являющиеся абстрактными типы с отдельными таблицами.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-130">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="6ab0c-131">Все свойства класса, включая унаследованные, сопоставляются со столбцами в соответствующей таблице.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-131">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="6ab0c-132">Такая модель называется наследованием типа "одна таблица на конкретный класс".</span><span class="sxs-lookup"><span data-stu-id="6ab0c-132">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="6ab0c-133">Если реализовать наследование типа "одна таблица на конкретный класс" для показанных выше классов Person, Student и Instructor, таблицы Student и Instructor после реализации наследования будут выглядеть так же, как и до этого.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-133">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="6ab0c-134">Модели наследования "одна таблица на иерархию" и "одна таблица на конкретный класс", как правило, обеспечивают более высокую производительность по сравнению с моделью "одна таблица на тип", поскольку при использовании последней могут выполняться сложные запросы на соединение.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-134">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="6ab0c-135">В этом учебнике демонстрируется реализация модели наследования "одна таблица на иерархию".</span><span class="sxs-lookup"><span data-stu-id="6ab0c-135">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="6ab0c-136">Платформа Entity Framework поддерживает только модель наследования "одна таблица на иерархию".</span><span class="sxs-lookup"><span data-stu-id="6ab0c-136">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="6ab0c-137">Вам необходимо создать класс `Person`, изменить классы `Instructor` и `Student` так, чтобы они были производными от класса `Person`, добавить новый класс в `DbContext`, после чего создать миграцию.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-137">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP]
> <span data-ttu-id="6ab0c-138">Перед внесением изменений рекомендуется сохранить копию проекта.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-138">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="6ab0c-139">В этом случае, если возникнут проблемы, вы сможете вернуться к сохраненному проекту вместо того, чтобы отменять выполненные в рамках этого учебника действия или начинать всю серию сначала.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-139">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="6ab0c-140">Создание класса Person</span><span class="sxs-lookup"><span data-stu-id="6ab0c-140">Create the Person class</span></span>

<span data-ttu-id="6ab0c-141">В папке Models создайте файл Person.cs и замените код шаблона следующим:</span><span class="sxs-lookup"><span data-stu-id="6ab0c-141">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="6ab0c-142">Создание классов Student и Instructor, унаследованных от класса Person</span><span class="sxs-lookup"><span data-stu-id="6ab0c-142">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="6ab0c-143">В файле *Instructor.cs* измените класс Instructor так, чтобы он был производным от класса Person, и удалите поля ключа и имени.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-143">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="6ab0c-144">Код будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6ab0c-144">The code will look like the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="6ab0c-145">Выполните те же изменения в файле *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-145">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="6ab0c-146">Добавление типа сущности Person в модель данных</span><span class="sxs-lookup"><span data-stu-id="6ab0c-146">Add the Person entity type to the data model</span></span>

<span data-ttu-id="6ab0c-147">Добавьте тип сущности Person в файл *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-147">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="6ab0c-148">Новые строки выделены.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-148">The new lines are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="6ab0c-149">Это все, что требуется платформе Entity Framework для настройки наследования типа "одна таблица на иерархию".</span><span class="sxs-lookup"><span data-stu-id="6ab0c-149">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="6ab0c-150">Как видно, после обновления базы данных в ней будет присутствовать таблица Person вместо таблиц Student и Instructor.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-150">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="6ab0c-151">Создание и настройка кода миграции</span><span class="sxs-lookup"><span data-stu-id="6ab0c-151">Create and customize migration code</span></span>

<span data-ttu-id="6ab0c-152">Сохраните изменения и выполните сборку проекта.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-152">Save your changes and build the project.</span></span> <span data-ttu-id="6ab0c-153">Затем откройте командное окно в папке проекта и введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6ab0c-153">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="6ab0c-154">На этом этапе не выполняйте команду `database update`.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-154">Don't run the `database update` command yet.</span></span> <span data-ttu-id="6ab0c-155">Ее выполнение приведет к потере данных, поскольку будет удалена таблица Instructor, а таблица Student будет переименована в Person.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-155">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="6ab0c-156">Для сохранения существующих данных потребуется настраиваемый код.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-156">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="6ab0c-157">Откройте файл *Migrations/\<метка_времени>_Inheritance.cs* и замените метод `Up` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6ab0c-157">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="6ab0c-158">Этот код выполняет следующие задачи по обновлению базы данных:</span><span class="sxs-lookup"><span data-stu-id="6ab0c-158">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="6ab0c-159">Удаляет ограничения внешнего ключа и индексы, которые указывают на таблицу Student.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-159">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="6ab0c-160">Переименовывает таблицу Instructor в Person и вносит изменения, необходимые для сохранения в ней данных из таблицы Student:</span><span class="sxs-lookup"><span data-stu-id="6ab0c-160">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="6ab0c-161">Добавляет допускающий значения NULL тип EnrollmentDate для учащихся.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-161">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="6ab0c-162">Добавляет столбец дискриминатора, который указывает, представляет ли строка учащегося или преподавателя.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-162">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="6ab0c-163">Изменяет тип HireDate на допускающий значения NULL, поскольку в строках для учащихся не будет указываться дата приема на работу.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-163">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="6ab0c-164">Добавляет временное поле, которое будет использоваться для обновления внешних ключей, указывающих на учащихся.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-164">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="6ab0c-165">При копировании записей учащихся в таблицу Person им назначаются новые значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-165">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="6ab0c-166">Копирует данные из таблицы Student в таблицу Person.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-166">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="6ab0c-167">При этом записям учащихся назначаются новые значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-167">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="6ab0c-168">Исправляет значения внешнего ключа, которые указывают на учащихся.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-168">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="6ab0c-169">Повторно создает ограничения внешнего ключа и индексы, которые после этого указывают на таблицу Person.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-169">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="6ab0c-170">(Если вместо целочисленного типа первичного ключа используется GUID, значения первичного ключа для записей учащихся изменять не потребуется и некоторые из этих действий можно пропустить.)</span><span class="sxs-lookup"><span data-stu-id="6ab0c-170">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="6ab0c-171">Выполните команду `database update`:</span><span class="sxs-lookup"><span data-stu-id="6ab0c-171">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="6ab0c-172">(В рабочей системе необходимо внести соответствующие изменения в метод `Down` на случай, если вам потребуется вернуться к предыдущей версии базы данных.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-172">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="6ab0c-173">В этом учебнике метод `Down` не используется.)</span><span class="sxs-lookup"><span data-stu-id="6ab0c-173">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE]
> <span data-ttu-id="6ab0c-174">При изменении схемы в базе, содержащей существующие данные, возможны другие ошибки.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-174">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="6ab0c-175">Если вы получаете ошибки миграции, которые не удается устранить, измените имя базы данных в строке подключения или удалите базу данных.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-175">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="6ab0c-176">В новой базе не будет данных, которые требуется перенести, в результате чего команда обновления базы данных с большей долей вероятности завершится без ошибок.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-176">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="6ab0c-177">Чтобы удалить базу данных, используйте средство SSOX или выполните команду `database drop` в интерфейсе командной строки.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-177">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="6ab0c-178">Тестирование после реализации наследования</span><span class="sxs-lookup"><span data-stu-id="6ab0c-178">Test with inheritance implemented</span></span>

<span data-ttu-id="6ab0c-179">Запустите приложение и попробуйте открыть различные страницы.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-179">Run the app and try various pages.</span></span> <span data-ttu-id="6ab0c-180">Все работает так же, как и раньше.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-180">Everything works the same as it did before.</span></span>

<span data-ttu-id="6ab0c-181">В **обозревателе объектов SQL Server** разверните узел **Data Connections/SchoolContext** и затем **Таблицы**. Вы увидите, что вместо таблиц Student и Instructor появилась таблица Person.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-181">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="6ab0c-182">Откройте таблицу Person в конструкторе и убедитесь, что в ней представлены все столбцы, содержавшиеся в таблицах Student и Instructor.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-182">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Таблица Person в окне SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="6ab0c-184">Щелкните таблицу Person правой кнопкой мыши и выберите команду **Показать данные таблицы**, чтобы просмотреть столбец дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-184">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Таблица Person в окне SSOX — данные таблицы](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="6ab0c-186">Сводка</span><span class="sxs-lookup"><span data-stu-id="6ab0c-186">Summary</span></span>

<span data-ttu-id="6ab0c-187">Вы реализовали наследование типа "одна таблица на иерархию" для классов `Person`, `Student` и `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-187">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="6ab0c-188">Дополнительные сведения о наследовании на платформе Entity Framework Core см. в разделе [Наследование](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="6ab0c-188">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="6ab0c-189">В рамках следующего учебника вы узнаете, как работать в нескольких сценариях Entity Framework с расширенными возможностями.</span><span class="sxs-lookup"><span data-stu-id="6ab0c-189">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="6ab0c-190">[Назад](concurrency.md)
> [Вперед](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="6ab0c-190">[Previous](concurrency.md)
[Next](advanced.md)</span></span>
