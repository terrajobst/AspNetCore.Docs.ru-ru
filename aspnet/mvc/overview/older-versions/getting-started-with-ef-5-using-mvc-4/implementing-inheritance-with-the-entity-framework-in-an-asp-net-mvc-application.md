---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Реализация наследования с использованием Entity Framework в приложении ASP.NET MVC (8 из 10) | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d3c3d19a62f601d007b0fac031ade1eef9f35771
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378940"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a><span data-ttu-id="128ac-103">Реализация наследования с использованием Entity Framework в приложении ASP.NET MVC (8 из 10)</span><span class="sxs-lookup"><span data-stu-id="128ac-103">Implementing Inheritance with the Entity Framework in an ASP.NET MVC Application (8 of 10)</span></span>
====================
<span data-ttu-id="128ac-104">по [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="128ac-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="128ac-105">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="128ac-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="128ac-106">Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, используя Entity Framework 5 Code First и Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="128ac-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="128ac-107">Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="128ac-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="128ac-108">Серии руководств можно начать с самого начала или [Загрузите начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.</span><span class="sxs-lookup"><span data-stu-id="128ac-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="128ac-109">Если вы столкнулись с проблемами, не удается устранить, [скачать завершенного глава](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему.</span><span class="sxs-lookup"><span data-stu-id="128ac-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="128ac-110">Обычно можно найти решение проблемы, сравнивая код, чтобы полный код.</span><span class="sxs-lookup"><span data-stu-id="128ac-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="128ac-111">Некоторые распространенные ошибки и способы их устранения, см. в разделе [ошибки и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="128ac-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="128ac-112">В предыдущем учебнике показана обработка исключений параллелизма.</span><span class="sxs-lookup"><span data-stu-id="128ac-112">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="128ac-113">В этом учебнике демонстрируется, как реализовать наследование в модели данных.</span><span class="sxs-lookup"><span data-stu-id="128ac-113">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="128ac-114">В объектно ориентированное программирование, чтобы исключить избыточный код можно использовать наследование.</span><span class="sxs-lookup"><span data-stu-id="128ac-114">In object-oriented programming, you can use inheritance to eliminate redundant code.</span></span> <span data-ttu-id="128ac-115">В рамках этого учебника вы измените классы `Instructor` и `Student` таким образом, чтобы они были производными от базового класса `Person`, который содержит общие свойства для преподавателей и учащихся, такие как `LastName`.</span><span class="sxs-lookup"><span data-stu-id="128ac-115">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="128ac-116">Изменения вносятся в коде, а не на веб-страницах, и автоматически отражаются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="128ac-116">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a><span data-ttu-id="128ac-117">Таблица на иерархию и таблица на тип наследования</span><span class="sxs-lookup"><span data-stu-id="128ac-117">Table-per-Hierarchy versus Table-per-Type Inheritance</span></span>

<span data-ttu-id="128ac-118">В объектно ориентированного программирования, можно использовать наследование для упрощения работы с связанных классов.</span><span class="sxs-lookup"><span data-stu-id="128ac-118">In object-oriented programming, you can use inheritance to make it easier to work with related classes.</span></span> <span data-ttu-id="128ac-119">Например `Instructor` и `Student` классы в `School` модели данных совместно используют несколько свойств, что приводит к избыточный код:</span><span class="sxs-lookup"><span data-stu-id="128ac-119">For example, the `Instructor` and `Student` classes in the `School` data model share several properties, which results in redundant code:</span></span>

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

<span data-ttu-id="128ac-121">Предположим, что вам требуется исключить повторяющийся код для свойств, которые являются общими для сущностей `Instructor` и `Student`.</span><span class="sxs-lookup"><span data-stu-id="128ac-121">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="128ac-122">Можно создать `Person` базовый класс, который содержит только те общие свойства, а затем сделать `Instructor` и `Student` сущности являются производными от этого базового класса, как показано на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="128ac-122">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

<span data-ttu-id="128ac-124">Структура наследования может быть представлена в базе данных несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="128ac-124">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="128ac-125">Имеется `Person` таблицу, содержащую сведения об учащихся и преподавателей в одной таблице.</span><span class="sxs-lookup"><span data-stu-id="128ac-125">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="128ac-126">Некоторые столбцы могут относиться только к преподавателям (`HireDate`), некоторые только к учащимся (`EnrollmentDate`), некоторые для обоих (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="128ac-126">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="128ac-127">Как правило, пришлось бы *дискриминатора* представляет столбец, который указывает, какой тип каждой строки.</span><span class="sxs-lookup"><span data-stu-id="128ac-127">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="128ac-128">Например, в столбце дискриминатора может указываться значение "Instructor" для преподавателей и "Student" для учащихся.</span><span class="sxs-lookup"><span data-stu-id="128ac-128">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Таблицы на hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

<span data-ttu-id="128ac-130">Такая модель создания структуры наследования сущностей из одной таблицы базы данных называется *таблица на иерархию* наследования (TPH).</span><span class="sxs-lookup"><span data-stu-id="128ac-130">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="128ac-131">В качестве альтернативы можно создать базу данных, которая будет иметь приближенный к структуре наследования вид.</span><span class="sxs-lookup"><span data-stu-id="128ac-131">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="128ac-132">Например, может иметь только поля с именами `Person` таблицы и иметь отдельные `Instructor` и `Student` таблицы с полями даты.</span><span class="sxs-lookup"><span data-stu-id="128ac-132">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Таблицы на type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

<span data-ttu-id="128ac-134">Такая модель создания таблицы базы данных для каждого класса сущности называется *одна таблица на тип* наследование (TPT).</span><span class="sxs-lookup"><span data-stu-id="128ac-134">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="128ac-135">TPH наследования как правило, обеспечивают более высокую производительность на платформе Entity Framework, чем шаблоны наследования TPT, так как шаблоны TPT может привести к сложные запросы на соединение.</span><span class="sxs-lookup"><span data-stu-id="128ac-135">TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span> <span data-ttu-id="128ac-136">В этом учебнике демонстрируется реализация модели наследования "одна таблица на иерархию".</span><span class="sxs-lookup"><span data-stu-id="128ac-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="128ac-137">Это можно сделать, выполнив следующие действия:</span><span class="sxs-lookup"><span data-stu-id="128ac-137">You'll do that by performing the following steps:</span></span>

- <span data-ttu-id="128ac-138">Создание `Person` класса и измените `Instructor` и `Student` классам наследовать от `Person`.</span><span class="sxs-lookup"><span data-stu-id="128ac-138">Create a `Person` class and change the `Instructor` and `Student` classes to derive from `Person`.</span></span>
- <span data-ttu-id="128ac-139">Добавьте код сопоставления модели для базы данных в классе контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="128ac-139">Add model-to-database mapping code to the database context class.</span></span>
- <span data-ttu-id="128ac-140">Изменение `InstructorID` и `StudentID` ссылки в проекте для `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="128ac-140">Change `InstructorID` and `StudentID` references throughout the project to `PersonID`.</span></span>

## <a name="creating-the-person-class"></a><span data-ttu-id="128ac-141">Создание класса Person</span><span class="sxs-lookup"><span data-stu-id="128ac-141">Creating the Person Class</span></span>

 <span data-ttu-id="128ac-142">Примечание: Вы не сможете компиляции проекта после создания классов, указанных ниже, пока вы не обновите контроллеры, которые использует эти классы.</span><span class="sxs-lookup"><span data-stu-id="128ac-142">Note: You won't be able to compile the project after creating the classes below until you update the controllers that uses these classes.</span></span> 

<span data-ttu-id="128ac-143">В *моделей* папке создайте *Person.cs* и замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="128ac-143">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="128ac-144">В *Instructor.cs*, являются производными `Instructor` класса из `Person` класса и удалите поля ключа и имени.</span><span class="sxs-lookup"><span data-stu-id="128ac-144">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="128ac-145">Код будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="128ac-145">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="128ac-146">Внести аналогичные изменения к *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="128ac-146">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="128ac-147">`Student` Класса будет выглядеть как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="128ac-147">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a><span data-ttu-id="128ac-148">Добавление типа сущности Person в модель</span><span class="sxs-lookup"><span data-stu-id="128ac-148">Adding the Person Entity Type to the Model</span></span>

<span data-ttu-id="128ac-149">В *SchoolContext.cs*, добавьте `DbSet` свойство для `Person` типа сущности:</span><span class="sxs-lookup"><span data-stu-id="128ac-149">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="128ac-150">Это все, что требуется платформе Entity Framework для настройки наследования типа "одна таблица на иерархию".</span><span class="sxs-lookup"><span data-stu-id="128ac-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="128ac-151">Как можно будет увидеть, когда базы данных не будет создана повторно, он будет иметь `Person` таблицы вместо `Student` и `Instructor` таблицы.</span><span class="sxs-lookup"><span data-stu-id="128ac-151">As you'll see, when the database is re-created, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="changing-instructorid-and-studentid-to-personid"></a><span data-ttu-id="128ac-152">Изменение InstructorID и StudentID на PersonID</span><span class="sxs-lookup"><span data-stu-id="128ac-152">Changing InstructorID and StudentID to PersonID</span></span>

<span data-ttu-id="128ac-153">В *SchoolContext.cs*, в операторе сопоставления преподавателя курсов измените `MapRightKey("InstructorID")` для `MapRightKey("PersonID")`:</span><span class="sxs-lookup"><span data-stu-id="128ac-153">In *SchoolContext.cs*, in the Instructor-Course mapping statement, change `MapRightKey("InstructorID")` to `MapRightKey("PersonID")`:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

<span data-ttu-id="128ac-154">Это изменение не является необходимым. изменяется только имя столбца InstructorID в таблицы соединения многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="128ac-154">This change isn't required; it just changes the name of the InstructorID column in the many-to-many join table.</span></span> <span data-ttu-id="128ac-155">Если оставить имя, InstructorID приложение по-прежнему будет работать правильно.</span><span class="sxs-lookup"><span data-stu-id="128ac-155">If you left the name as InstructorID, the application would still work correctly.</span></span> <span data-ttu-id="128ac-156">Ниже приведен полный *SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="128ac-156">Here is the completed *SchoolContext.cs*:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

<span data-ttu-id="128ac-157">Далее необходимо изменить `InstructorID` для `PersonID` и `StudentID` для `PersonID` в проекте ***за исключением*** в файлах с меткой времени миграции *миграций* папка.</span><span class="sxs-lookup"><span data-stu-id="128ac-157">Next you need to change `InstructorID` to `PersonID` and `StudentID` to `PersonID` throughout the project ***except*** in the time-stamped migrations files in the *Migrations* folder.</span></span> <span data-ttu-id="128ac-158">Для этого вы сможете найти открывать только файлы, которые должны быть изменены, а затем выполнения глобальных изменений на открытые файлы.</span><span class="sxs-lookup"><span data-stu-id="128ac-158">To do that you'll find and open only the files that need to be changed, then perform a global change on the opened files.</span></span> <span data-ttu-id="128ac-159">Единственным файлом в *миграций* папка, следует изменить *Migrations\Configuration.cs.*</span><span class="sxs-lookup"><span data-stu-id="128ac-159">The only file in the *Migrations* folder you should change is *Migrations\Configuration.cs.*</span></span>

1. > [!IMPORTANT]
   > <span data-ttu-id="128ac-160">Перед началом закрытие всех открытых файлов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="128ac-160">Begin by closing all the open files in Visual Studio.</span></span>
2. <span data-ttu-id="128ac-161">Нажмите кнопку **найти и заменить — найти все файлы** в **изменить** меню и выполните поиск всех файлов в проекте, содержащих `InstructorID`.</span><span class="sxs-lookup"><span data-stu-id="128ac-161">Click **Find and Replace -- Find all Files** in the **Edit** menu, and then search for all files in the project that contain `InstructorID`.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. <span data-ttu-id="128ac-162">Откройте каждый файл в **результаты поиска** окно ***за исключением*** &lt;отметку времени&gt;*\_.cs* переноса файлов в *Миграций* папки, дважды щелкнув одну строку для каждого файла.</span><span class="sxs-lookup"><span data-stu-id="128ac-162">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. <span data-ttu-id="128ac-163">Откройте **заменить в файлах** диалоговое окно и изменение **папка** для **все открытые документы**.</span><span class="sxs-lookup"><span data-stu-id="128ac-163">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
5. <span data-ttu-id="128ac-164">Используйте **заменить в файлах** диалогового окна, чтобы изменить все `InstructorID` для `PersonID.`</span><span class="sxs-lookup"><span data-stu-id="128ac-164">Use the **Replace in Files** dialog to change all `InstructorID` to `PersonID.`</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. <span data-ttu-id="128ac-165">Найти все файлы в проекте, который содержит `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="128ac-165">Find all the files in the project that contain `StudentID`.</span></span>
7. <span data-ttu-id="128ac-166">Откройте каждый файл в **результаты поиска** окно ***за исключением*** &lt;отметку времени&gt;*\_\*.cs* файлы данных миграции в *миграций* папки, дважды щелкнув одну строку для каждого файла.</span><span class="sxs-lookup"><span data-stu-id="128ac-166">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_\*.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. <span data-ttu-id="128ac-167">Откройте **заменить в файлах** диалоговое окно и изменение **папка** для **все открытые документы**.</span><span class="sxs-lookup"><span data-stu-id="128ac-167">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
9. <span data-ttu-id="128ac-168">Используйте **заменить в файлах** диалогового окна, чтобы изменить все `StudentID` для `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="128ac-168">Use the **Replace in Files** dialog to change all `StudentID` to `PersonID`.</span></span>   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. <span data-ttu-id="128ac-169">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="128ac-169">Build the project.</span></span>

<span data-ttu-id="128ac-170">(Обратите внимание, что этот пример демонстрирует *недостаток* из `classnameID` шаблон именования первичные ключи.</span><span class="sxs-lookup"><span data-stu-id="128ac-170">(Note that this demonstrates a *disadvantage* of the `classnameID` pattern for naming primary keys.</span></span> <span data-ttu-id="128ac-171">Если идентификатор первичные ключи присвоил без префикса имени класса, *не* переименование было бы необходимо сейчас.)</span><span class="sxs-lookup"><span data-stu-id="128ac-171">If you had named primary keys ID without prefixing the class name, *no* renaming would be necessary now.)</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="128ac-172">Создание и изменение файла миграции</span><span class="sxs-lookup"><span data-stu-id="128ac-172">Create and Update a Migrations File</span></span>

<span data-ttu-id="128ac-173">В консоли диспетчера пакетов (PMC), введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="128ac-173">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="128ac-174">Запустите `Update-Database` команду в PMC.</span><span class="sxs-lookup"><span data-stu-id="128ac-174">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="128ac-175">Команда завершится ошибкой на этом этапе, так как у нас есть миграции не знает как обрабатывать существующие данные.</span><span class="sxs-lookup"><span data-stu-id="128ac-175">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="128ac-176">Вы получите следующую ошибку:</span><span class="sxs-lookup"><span data-stu-id="128ac-176">You get the following error:</span></span>

<span data-ttu-id="128ac-177">*Конфликт инструкции ALTER TABLE с ограничением FOREIGN KEY «FK\_dbo. Отдел\_dbo. Person\_PersonID». Конфликт произошел в базе данных «ContosoUniversity», таблица «dbo. Пользователь», столбец «PersonID».*</span><span class="sxs-lookup"><span data-stu-id="128ac-177">*The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK\_dbo.Department\_dbo.Person\_PersonID". The conflict occurred in database "ContosoUniversity", table "dbo.Person", column 'PersonID'.*</span></span>

<span data-ttu-id="128ac-178">Откройте *миграций\&lt; метка времени&gt;\_Inheritance.cs* и замените `Up` метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="128ac-178">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

<span data-ttu-id="128ac-179">Запустите `update-database` еще раз выполните команду.</span><span class="sxs-lookup"><span data-stu-id="128ac-179">Run the `update-database` command again.</span></span>

> [!NOTE]
> <span data-ttu-id="128ac-180">Это можно получить другие ошибки при переносе данных и внесения изменений схемы.</span><span class="sxs-lookup"><span data-stu-id="128ac-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="128ac-181">Если вы получаете ошибки миграции не удается устранить, можно будет продолжить руководства, изменив строку подключения в *Web.config* файла или удаление базы данных.</span><span class="sxs-lookup"><span data-stu-id="128ac-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or deleting the database.</span></span> <span data-ttu-id="128ac-182">Самым простым подходом является переименовать базу данных, в *Web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="128ac-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="128ac-183">Например, изменить имя базы данных на CU\_тестирования, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="128ac-183">For example, change the database name to CU\_test as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> <span data-ttu-id="128ac-184">В новой базе данных нет данных для переноса и `update-database` команда является гораздо большей долей вероятности завершится без ошибок.</span><span class="sxs-lookup"><span data-stu-id="128ac-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="128ac-185">Инструкции о том, как удалить базу данных, см. в разделе [как удалить базу данных из Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="128ac-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="128ac-186">При этом подходе для продолжения работы с учебником, пропустите шаг развертывания в конце этого руководства, так как развернутого веб-сайта будет та же ошибка, при выполнении миграции автоматически.</span><span class="sxs-lookup"><span data-stu-id="128ac-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial, since the deployed site would get the same error when it runs migrations automatically.</span></span> <span data-ttu-id="128ac-187">Если вы хотите устранении ошибки миграции, самый лучший ресурс является одним из форумы Entity Framework или StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="128ac-187">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="128ac-188">Тестирование</span><span class="sxs-lookup"><span data-stu-id="128ac-188">Testing</span></span>

<span data-ttu-id="128ac-189">Запустите сайт и попробуйте открыть различные страницы.</span><span class="sxs-lookup"><span data-stu-id="128ac-189">Run the site and try various pages.</span></span> <span data-ttu-id="128ac-190">Все работает так же, как и раньше.</span><span class="sxs-lookup"><span data-stu-id="128ac-190">Everything works the same as it did before.</span></span>

<span data-ttu-id="128ac-191">В **обозреватель серверов,** разверните **SchoolContext** и затем **таблиц**, и вы увидите, что **учащихся** и **преподавателя**  таблиц были заменены **Person** таблицы.</span><span class="sxs-lookup"><span data-stu-id="128ac-191">In **Server Explorer,** expand **SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="128ac-192">Разверните **Person** таблицы чтобы увидеть, все столбцы, которые ранее были в наличии **учащихся** и **преподавателя** таблиц.</span><span class="sxs-lookup"><span data-stu-id="128ac-192">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="128ac-194">Щелкните таблицу Person правой кнопкой мыши и выберите команду **Показать данные таблицы**, чтобы просмотреть столбец дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="128ac-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="128ac-195">На следующей схеме показана структура новой базы данных School:</span><span class="sxs-lookup"><span data-stu-id="128ac-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a><span data-ttu-id="128ac-197">Сводка</span><span class="sxs-lookup"><span data-stu-id="128ac-197">Summary</span></span>

<span data-ttu-id="128ac-198">Таблица на иерархию наследования теперь реализовано для `Person`, `Student`, и `Instructor` классы.</span><span class="sxs-lookup"><span data-stu-id="128ac-198">Table-per-hierarchy inheritance has now been implemented for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="128ac-199">Дополнительные сведения об этом и других структур наследования см. в разделе [стратегии сопоставление наследования](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) в блоге Morteza Manavi.</span><span class="sxs-lookup"><span data-stu-id="128ac-199">For more information about this and other inheritance structures, see [Inheritance Mapping Strategies](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) on Morteza Manavi's blog.</span></span> <span data-ttu-id="128ac-200">В следующем учебном курсе вы увидите некоторые способы реализации репозиторий и блок рабочих шаблонов.</span><span class="sxs-lookup"><span data-stu-id="128ac-200">In the next tutorial you'll see some ways to implement the repository and unit of work patterns.</span></span>

<span data-ttu-id="128ac-201">Ссылки на другие ресурсы Entity Framework можно найти в [схема содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="128ac-201">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="128ac-202">[Назад](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="128ac-202">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span></span>
