---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Реализация наследования с использованием платформы Entity Framework в приложении ASP.NET MVC (8, 10) | Документы Microsoft"
author: tdykstra
description: "Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 54e46c6f996b6fe86a227c851562e61678b02780
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a><span data-ttu-id="8a388-103">Реализация наследования с использованием платформы Entity Framework в приложении ASP.NET MVC (8, 10)</span><span class="sxs-lookup"><span data-stu-id="8a388-103">Implementing Inheritance with the Entity Framework in an ASP.NET MVC Application (8 of 10)</span></span>
====================
<span data-ttu-id="8a388-104">По [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8a388-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="8a388-105">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="8a388-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="8a388-106">Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8a388-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="8a388-107">Сведения о учебника серии см [в первом учебнике ряда](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="8a388-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="8a388-108">Учебник рядов можно запустить с самого начала или [загрузить начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.</span><span class="sxs-lookup"><span data-stu-id="8a388-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="8a388-109">Если возникли проблемы, не удается устранить, [загрузить завершенного главе](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему.</span><span class="sxs-lookup"><span data-stu-id="8a388-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="8a388-110">Обычно можно найти решение проблемы путем сравнения код для завершения кода.</span><span class="sxs-lookup"><span data-stu-id="8a388-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="8a388-111">Некоторые распространенные ошибки и способы их устранения см. в разделе [ошибок и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="8a388-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="8a388-112">В предыдущем учебнике обработки исключений параллелизма.</span><span class="sxs-lookup"><span data-stu-id="8a388-112">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="8a388-113">Этот учебник будет показано, как реализовать наследование в модели данных.</span><span class="sxs-lookup"><span data-stu-id="8a388-113">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="8a388-114">В объектно ориентированное программирование, чтобы исключить избыточный код можно использовать наследование.</span><span class="sxs-lookup"><span data-stu-id="8a388-114">In object-oriented programming, you can use inheritance to eliminate redundant code.</span></span> <span data-ttu-id="8a388-115">В этом учебнике мы изменим `Instructor` и `Student` классов, чтобы они являются производными от `Person` базового класса, который содержит свойства, такие как `LastName` , которые являются общими для преподавателей и учащихся.</span><span class="sxs-lookup"><span data-stu-id="8a388-115">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="8a388-116">Не добавить или изменить веб-страницы, но мы изменим некоторую часть кода, и эти изменения будут автоматически отражаться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8a388-116">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a><span data-ttu-id="8a388-117">Таблица иерархию и таблица на тип наследования</span><span class="sxs-lookup"><span data-stu-id="8a388-117">Table-per-Hierarchy versus Table-per-Type Inheritance</span></span>

<span data-ttu-id="8a388-118">В объектно ориентированного программирования, можно использовать наследование для упрощения работы с связанных классов.</span><span class="sxs-lookup"><span data-stu-id="8a388-118">In object-oriented programming, you can use inheritance to make it easier to work with related classes.</span></span> <span data-ttu-id="8a388-119">Например `Instructor` и `Student` классы в `School` модели данных совместно использовать несколько свойств, что приводит к избыточный код:</span><span class="sxs-lookup"><span data-stu-id="8a388-119">For example, the `Instructor` and `Student` classes in the `School` data model share several properties, which results in redundant code:</span></span>

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

<span data-ttu-id="8a388-121">Предположим, что нужно исключить избыточный код для свойств, которые являются общими для `Instructor` и `Student` сущности.</span><span class="sxs-lookup"><span data-stu-id="8a388-121">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="8a388-122">Можно создать `Person` базового класса, который содержит только те общие свойства, а затем сделать `Instructor` и `Student` сущности являются производными от базового класса, как показано на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="8a388-122">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

<span data-ttu-id="8a388-124">Существует несколько способов эта структура наследования можно представить в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8a388-124">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="8a388-125">У вас может быть `Person` таблицу, содержащую сведения о учащихся и инструкторов в одной таблице.</span><span class="sxs-lookup"><span data-stu-id="8a388-125">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="8a388-126">Некоторые столбцы могут применяются только к инструкторов (`HireDate`), некоторые только для учащихся (`EnrollmentDate`), некоторые для обоих (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="8a388-126">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="8a388-127">Как правило, будет иметь *дискриминатора* представляет столбец, указывающий, какой тип каждой строки.</span><span class="sxs-lookup"><span data-stu-id="8a388-127">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="8a388-128">Например столбец дискриминатора установлена «Инструктора» для инструкторов и «Студентов» для учащихся.</span><span class="sxs-lookup"><span data-stu-id="8a388-128">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Таблицы на hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

<span data-ttu-id="8a388-130">Эта модель создания структура наследования сущностей из одной таблицы базы данных называется *таблица на иерархию* наследование (TPH).</span><span class="sxs-lookup"><span data-stu-id="8a388-130">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="8a388-131">Альтернативой является база данных больше похожи на структуру наследования.</span><span class="sxs-lookup"><span data-stu-id="8a388-131">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="8a388-132">Например, может содержать только имя поля `Person` таблицы и иметь отдельный `Instructor` и `Student` таблицы с полями даты.</span><span class="sxs-lookup"><span data-stu-id="8a388-132">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Таблицы на type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

<span data-ttu-id="8a388-134">Этот шаблон создания таблицы базы данных для каждого класса сущности вызывается *одна таблица на тип* наследования (TPT).</span><span class="sxs-lookup"><span data-stu-id="8a388-134">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="8a388-135">TPH схемы наследования обычно работы с большей производительностью в платформе Entity Framework, чем TPT схемы наследования, поскольку шаблоны TPT может привести к запросы сложного соединения.</span><span class="sxs-lookup"><span data-stu-id="8a388-135">TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span> <span data-ttu-id="8a388-136">Этот учебник демонстрирует реализацию TPH наследования.</span><span class="sxs-lookup"><span data-stu-id="8a388-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="8a388-137">Вам предстоит выполнить, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="8a388-137">You'll do that by performing the following steps:</span></span>

- <span data-ttu-id="8a388-138">Создание `Person` и измените их `Instructor` и `Student` классы являются производными от `Person`.</span><span class="sxs-lookup"><span data-stu-id="8a388-138">Create a `Person` class and change the `Instructor` and `Student` classes to derive from `Person`.</span></span>
- <span data-ttu-id="8a388-139">Добавьте код сопоставления модели в базу данных для класса контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="8a388-139">Add model-to-database mapping code to the database context class.</span></span>
- <span data-ttu-id="8a388-140">Изменение `InstructorID` и `StudentID` ссылки в проекте для `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="8a388-140">Change `InstructorID` and `StudentID` references throughout the project to `PersonID`.</span></span>

## <a name="creating-the-person-class"></a><span data-ttu-id="8a388-141">Создание класса Person</span><span class="sxs-lookup"><span data-stu-id="8a388-141">Creating the Person Class</span></span>

 <span data-ttu-id="8a388-142">Примечание: Невозможно скомпилировать проект после создания классов ниже, пока вы не обновите контроллеры, которые использует эти классы.</span><span class="sxs-lookup"><span data-stu-id="8a388-142">Note: You won't be able to compile the project after creating the classes below until you update the controllers that uses these classes.</span></span> 

<span data-ttu-id="8a388-143">В *моделей* папке создайте *Person.cs* и замените код шаблона с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="8a388-143">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="8a388-144">В *Instructor.cs*, являются производными `Instructor` класса из `Person` класса и удалять поля ключа и имени.</span><span class="sxs-lookup"><span data-stu-id="8a388-144">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="8a388-145">Код будет выглядеть как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8a388-145">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="8a388-146">Внести аналогичные изменения в *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="8a388-146">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="8a388-147">`Student` Класс будет выглядеть как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8a388-147">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a><span data-ttu-id="8a388-148">Добавление типа Person сущности в модель</span><span class="sxs-lookup"><span data-stu-id="8a388-148">Adding the Person Entity Type to the Model</span></span>

<span data-ttu-id="8a388-149">В *SchoolContext.cs*, добавьте `DbSet` свойство `Person` типа сущности:</span><span class="sxs-lookup"><span data-stu-id="8a388-149">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="8a388-150">Это все, что платформа Entity Framework требуется для настройки таблица на иерархию наследования.</span><span class="sxs-lookup"><span data-stu-id="8a388-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="8a388-151">Как можно будет увидеть, когда база данных создается заново, он будет иметь `Person` таблицы вместо `Student` и `Instructor` таблицы.</span><span class="sxs-lookup"><span data-stu-id="8a388-151">As you'll see, when the database is re-created, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="changing-instructorid-and-studentid-to-personid"></a><span data-ttu-id="8a388-152">Изменение InstructorID и идентификатором StudentID на PersonID</span><span class="sxs-lookup"><span data-stu-id="8a388-152">Changing InstructorID and StudentID to PersonID</span></span>

<span data-ttu-id="8a388-153">В *SchoolContext.cs*, в операторе сопоставления курса инструктора изменить `MapRightKey("InstructorID")` для `MapRightKey("PersonID")`:</span><span class="sxs-lookup"><span data-stu-id="8a388-153">In *SchoolContext.cs*, in the Instructor-Course mapping statement, change `MapRightKey("InstructorID")` to `MapRightKey("PersonID")`:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

<span data-ttu-id="8a388-154">Это изменение не требуется; изменяется только имя столбца InstructorID в многие ко многим соединяемой таблице.</span><span class="sxs-lookup"><span data-stu-id="8a388-154">This change isn't required; it just changes the name of the InstructorID column in the many-to-many join table.</span></span> <span data-ttu-id="8a388-155">Если оставить имя InstructorID, приложение по-прежнему будет работать неправильно.</span><span class="sxs-lookup"><span data-stu-id="8a388-155">If you left the name as InstructorID, the application would still work correctly.</span></span> <span data-ttu-id="8a388-156">Ниже приведен полный *SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="8a388-156">Here is the completed *SchoolContext.cs*:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

<span data-ttu-id="8a388-157">Далее необходимо изменить `InstructorID` для `PersonID` и `StudentID` для `PersonID` в проекте ***за исключением*** в файлах с меткой времени миграции *миграций* папка.</span><span class="sxs-lookup"><span data-stu-id="8a388-157">Next you need to change `InstructorID` to `PersonID` and `StudentID` to `PersonID` throughout the project ***except*** in the time-stamped migrations files in the *Migrations* folder.</span></span> <span data-ttu-id="8a388-158">Для этого вы будете найти открывать только файлы, которые должны быть изменены, а затем провести Глобальное изменение открытых файлов.</span><span class="sxs-lookup"><span data-stu-id="8a388-158">To do that you'll find and open only the files that need to be changed, then perform a global change on the opened files.</span></span> <span data-ttu-id="8a388-159">Единственным файлом в *миграций* папка, следует изменить *Migrations\Configuration.cs.*</span><span class="sxs-lookup"><span data-stu-id="8a388-159">The only file in the *Migrations* folder you should change is *Migrations\Configuration.cs.*</span></span>

1. > [!IMPORTANT]
 > <span data-ttu-id="8a388-160">Перед началом закрытие всех открытых файлов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8a388-160">Begin by closing all the open files in Visual Studio.</span></span>
2. <span data-ttu-id="8a388-161">Нажмите кнопку **найти и заменить — найти все файлы** в **изменить** меню и выполните поиск всех файлов в проекте, содержащих `InstructorID`.</span><span class="sxs-lookup"><span data-stu-id="8a388-161">Click **Find and Replace -- Find all Files** in the **Edit** menu, and then search for all files in the project that contain `InstructorID`.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. <span data-ttu-id="8a388-162">Откройте каждый файл в **результаты поиска** окна ***за исключением*** &lt;отметку времени&gt;*\_.cs* переноса файлов в *Миграций* папки, дважды щелкнув одну строку для каждого файла.</span><span class="sxs-lookup"><span data-stu-id="8a388-162">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. <span data-ttu-id="8a388-163">Откройте **заменить в файлах** диалоговое окно и изменение **папка** для **все открытые документы**.</span><span class="sxs-lookup"><span data-stu-id="8a388-163">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
5. <span data-ttu-id="8a388-164">Используйте **заменить в файлах** диалоговое окно для изменения всех `InstructorID` для`PersonID.`</span><span class="sxs-lookup"><span data-stu-id="8a388-164">Use the **Replace in Files** dialog to change all `InstructorID` to `PersonID.`</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. <span data-ttu-id="8a388-165">Найти все файлы в проекте, который содержит `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="8a388-165">Find all the files in the project that contain `StudentID`.</span></span>
7. <span data-ttu-id="8a388-166">Откройте каждый файл в **результаты поиска** окна ***за исключением*** &lt;отметку времени&gt;*\_\*.cs* миграции файлов в *миграций* папки, дважды щелкнув одну строку для каждого файла.</span><span class="sxs-lookup"><span data-stu-id="8a388-166">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_\*.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. <span data-ttu-id="8a388-167">Откройте **заменить в файлах** диалоговое окно и изменение **папка** для **все открытые документы**.</span><span class="sxs-lookup"><span data-stu-id="8a388-167">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
9. <span data-ttu-id="8a388-168">Используйте **заменить в файлах** диалоговое окно для изменения всех `StudentID` для `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="8a388-168">Use the **Replace in Files** dialog to change all `StudentID` to `PersonID`.</span></span>   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. <span data-ttu-id="8a388-169">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="8a388-169">Build the project.</span></span>

<span data-ttu-id="8a388-170">(Обратите внимание, что этот пример демонстрирует *недостаток* из `classnameID` шаблон именования первичные ключи.</span><span class="sxs-lookup"><span data-stu-id="8a388-170">(Note that this demonstrates a *disadvantage* of the `classnameID` pattern for naming primary keys.</span></span> <span data-ttu-id="8a388-171">Если бы код первичных ключей без префикса имени класса *не* переименование потребовалась бы теперь.)</span><span class="sxs-lookup"><span data-stu-id="8a388-171">If you had named primary keys ID without prefixing the class name, *no* renaming would be necessary now.)</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="8a388-172">Создание и редактирование файла миграции</span><span class="sxs-lookup"><span data-stu-id="8a388-172">Create and Update a Migrations File</span></span>

<span data-ttu-id="8a388-173">В пакет Диспетчер консоли (PMC), введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="8a388-173">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="8a388-174">Запустите `Update-Database` в PMC команду.</span><span class="sxs-lookup"><span data-stu-id="8a388-174">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="8a388-175">Команда не будет выполнена на этом этапе у существующих данных, миграция не знает, как обрабатывать.</span><span class="sxs-lookup"><span data-stu-id="8a388-175">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="8a388-176">Появиться следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="8a388-176">You get the following error:</span></span>

<span data-ttu-id="8a388-177">*Конфликт инструкции ALTER TABLE с ограничением FOREIGN KEY «FK\_dbo. Отдел\_dbo. Лицо\_PersonID». Конфликт произошел в таблицу в базе данных «ContosoUniversity», «dbo. Пользователь», столбец «PersonID».*</span><span class="sxs-lookup"><span data-stu-id="8a388-177">*The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK\_dbo.Department\_dbo.Person\_PersonID". The conflict occurred in database "ContosoUniversity", table "dbo.Person", column 'PersonID'.*</span></span>

<span data-ttu-id="8a388-178">Откройте *миграций\&lt; отметка времени&gt;\_Inheritance.cs* и замените `Up` метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="8a388-178">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

<span data-ttu-id="8a388-179">Запустите `update-database` еще раз.</span><span class="sxs-lookup"><span data-stu-id="8a388-179">Run the `update-database` command again.</span></span>

> [!NOTE]
> <span data-ttu-id="8a388-180">Можно получить другие ошибки при переносе данных и внесения изменений схемы.</span><span class="sxs-lookup"><span data-stu-id="8a388-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="8a388-181">Если возникают ошибки при миграции не удается устранить, можно будет продолжить учебника, изменив строку подключения в *Web.config* файла или удаление базы данных.</span><span class="sxs-lookup"><span data-stu-id="8a388-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or deleting the database.</span></span> <span data-ttu-id="8a388-182">Самым простым подходом является переименовать базу данных, в *Web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="8a388-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="8a388-183">Например, измените имя базы данных на CU\_тестирования, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8a388-183">For example, change the database name to CU\_test as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> <span data-ttu-id="8a388-184">С помощью новой базы данных нет данных для переноса и `update-database` команда гораздо больше шансов завершиться без ошибок.</span><span class="sxs-lookup"><span data-stu-id="8a388-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="8a388-185">Инструкции по удалению базы данных см. в разделе [как удалить базу данных из Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="8a388-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="8a388-186">При использовании этого подхода, чтобы продолжить работу с учебником, пропустите шаг развертывания в конце этого учебника, поскольку развернутого сайта будет получена та же ошибка возникла при выполнении миграции автоматически.</span><span class="sxs-lookup"><span data-stu-id="8a388-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial, since the deployed site would get the same error when it runs migrations automatically.</span></span> <span data-ttu-id="8a388-187">Если вы хотите устранении ошибки миграции, лучший ресурс является одним из форумов Entity Framework или StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="8a388-187">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="8a388-188">Тестирование</span><span class="sxs-lookup"><span data-stu-id="8a388-188">Testing</span></span>

<span data-ttu-id="8a388-189">Запустите сайт и попробуйте различные страницы.</span><span class="sxs-lookup"><span data-stu-id="8a388-189">Run the site and try various pages.</span></span> <span data-ttu-id="8a388-190">Все, что работает так же, как раньше.</span><span class="sxs-lookup"><span data-stu-id="8a388-190">Everything works the same as it did before.</span></span>

<span data-ttu-id="8a388-191">В **обозревателя серверов** разверните **SchoolContext** и затем **таблиц**, и вы увидите, что **студента** и **инструктора**  таблицы были заменены **лицо** таблицы.</span><span class="sxs-lookup"><span data-stu-id="8a388-191">In **Server Explorer,** expand **SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="8a388-192">Разверните **лицо** таблица отобразится все столбцы, которые используются в наличии **студента** и **инструктора** таблиц.</span><span class="sxs-lookup"><span data-stu-id="8a388-192">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="8a388-194">Щелкните правой кнопкой мыши таблицу Person и нажмите кнопку **Показать таблицу данных** в столбце дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="8a388-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="8a388-195">На следующей схеме показана структура новой базы данных School:</span><span class="sxs-lookup"><span data-stu-id="8a388-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a><span data-ttu-id="8a388-197">Сводка</span><span class="sxs-lookup"><span data-stu-id="8a388-197">Summary</span></span>

<span data-ttu-id="8a388-198">Таблица на иерархию наследования теперь реализована для `Person`, `Student`, и `Instructor` классы.</span><span class="sxs-lookup"><span data-stu-id="8a388-198">Table-per-hierarchy inheritance has now been implemented for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="8a388-199">Дополнительные сведения об этом и других структур наследования см. в разделе [стратегии сопоставление наследования](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) Morteza Manavi блога.</span><span class="sxs-lookup"><span data-stu-id="8a388-199">For more information about this and other inheritance structures, see [Inheritance Mapping Strategies](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) on Morteza Manavi's blog.</span></span> <span data-ttu-id="8a388-200">В следующем уроке вы увидите некоторые способы реализации хранилища и единицы шаблонов рабочих элементов.</span><span class="sxs-lookup"><span data-stu-id="8a388-200">In the next tutorial you'll see some ways to implement the repository and unit of work patterns.</span></span>

<span data-ttu-id="8a388-201">Ссылки на другие ресурсы Entity Framework можно найти в [Карта содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="8a388-201">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8a388-202">[Назад](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Вперед](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="8a388-202">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span></span>
