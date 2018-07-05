---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'EF Database First с ASP.NET MVC: изменение базы данных | Документация Майкрософт'
author: tfitzmac
description: С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных. Этот учебник seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 7c9bca87c51bee35be2c5b533916255be80056b0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385438"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="b1f80-104">EF Database First с ASP.NET MVC: изменение базы данных</span><span class="sxs-lookup"><span data-stu-id="b1f80-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="b1f80-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b1f80-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b1f80-106">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="b1f80-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="b1f80-107">В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="b1f80-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="b1f80-108">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="b1f80-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="b1f80-109">Части этой серии посвящена выполнении обновления структуры базы данных и распространение этого изменения на протяжении всего веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="b1f80-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="b1f80-110">Добавление столбца</span><span class="sxs-lookup"><span data-stu-id="b1f80-110">Add a column</span></span>

<span data-ttu-id="b1f80-111">Если обновить структуру таблицы в базе данных, необходимо убедиться, что изменение распространяется на модели данных, представления и контроллера.</span><span class="sxs-lookup"><span data-stu-id="b1f80-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="b1f80-112">В этом учебнике вы добавите нового столбца на таблицу "Student" для записи отчество учащегося.</span><span class="sxs-lookup"><span data-stu-id="b1f80-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="b1f80-113">Чтобы добавить этот столбец, откройте проект базы данных и откройте файл Student.sql.</span><span class="sxs-lookup"><span data-stu-id="b1f80-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="b1f80-114">Через конструктор или кода T-SQL, добавьте столбец с именем **MiddleName** , NVARCHAR(50) и допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="b1f80-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![Добавление отчество](changing-the-database/_static/image1.png)

<span data-ttu-id="b1f80-116">Разверните это изменение в локальную базу данных, запустив проект базы данных (или F5).</span><span class="sxs-lookup"><span data-stu-id="b1f80-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="b1f80-117">Новое поле добавляется в таблицу.</span><span class="sxs-lookup"><span data-stu-id="b1f80-117">The new field is added to the table.</span></span> <span data-ttu-id="b1f80-118">Если вы не видите его в обозревателе объектов SQL Server, нажмите кнопку "Обновить" в области.</span><span class="sxs-lookup"><span data-stu-id="b1f80-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Показать новый столбец](changing-the-database/_static/image2.png)

<span data-ttu-id="b1f80-120">Новый столбец существует в таблице базы данных, но он не существует в классе модели данных.</span><span class="sxs-lookup"><span data-stu-id="b1f80-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="b1f80-121">Необходимо обновить модель, чтобы включить новый столбец.</span><span class="sxs-lookup"><span data-stu-id="b1f80-121">You must update the model to include your new column.</span></span> <span data-ttu-id="b1f80-122">В **моделей** откройте **ContosoModel.edmx** файл для отображения на схеме модели.</span><span class="sxs-lookup"><span data-stu-id="b1f80-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="b1f80-123">Обратите внимание на то, что модель Student не содержит свойство MiddleName.</span><span class="sxs-lookup"><span data-stu-id="b1f80-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="b1f80-124">Щелкните правой кнопкой мыши в области конструктора и выберите **обновить модель из базы данных**.</span><span class="sxs-lookup"><span data-stu-id="b1f80-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![обновление модели](changing-the-database/_static/image3.png)

<span data-ttu-id="b1f80-126">В мастере обновлений, выберите **обновить** вкладку и **учащихся** таблицы.</span><span class="sxs-lookup"><span data-stu-id="b1f80-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![Мастер обновления](changing-the-database/_static/image4.png)

<span data-ttu-id="b1f80-128">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="b1f80-128">Click **Finish**.</span></span>

<span data-ttu-id="b1f80-129">После завершения процесса обновления схемы базы данных включает в себя новый **MiddleName** свойство.</span><span class="sxs-lookup"><span data-stu-id="b1f80-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="b1f80-130">Сохранить **ContosoModel.edmx** файла.</span><span class="sxs-lookup"><span data-stu-id="b1f80-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="b1f80-131">Сохраните этот файл для нового свойства передаются на **Student.cs** класса.</span><span class="sxs-lookup"><span data-stu-id="b1f80-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="b1f80-132">Теперь обновления базы данных и модели.</span><span class="sxs-lookup"><span data-stu-id="b1f80-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="b1f80-133">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="b1f80-133">Build the solution.</span></span>

<span data-ttu-id="b1f80-134">К сожалению, представления по-прежнему не содержат новое свойство.</span><span class="sxs-lookup"><span data-stu-id="b1f80-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="b1f80-135">Для обновления представления можно двумя способами - можно повторно создать представления, снова добавив формирования шаблонов для класса Student, или можно вручную добавить новое свойство в существующие представления.</span><span class="sxs-lookup"><span data-stu-id="b1f80-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="b1f80-136">В этом руководстве вы добавите формирование шаблонов еще раз, так как не были внесены настраиваемые изменения в представления, автоматически создается.</span><span class="sxs-lookup"><span data-stu-id="b1f80-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="b1f80-137">Можно вручную добавить свойство, когда были внесены изменения в представления и не хотите потерять внесенные изменения.</span><span class="sxs-lookup"><span data-stu-id="b1f80-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="b1f80-138">Чтобы обеспечить представления создаются повторно, удалите **учащихся** папке **представления**и удалить **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="b1f80-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="b1f80-139">Щелкните, правой кнопкой мыши **контроллеров** папки и добавление формирования шаблонов для **учащихся** модели.</span><span class="sxs-lookup"><span data-stu-id="b1f80-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="b1f80-140">Опять же, назовите контроллер **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="b1f80-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="b1f80-141">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b1f80-141">Select **OK**.</span></span>

<span data-ttu-id="b1f80-142">Представлений теперь содержит указанное свойство MiddleName.</span><span class="sxs-lookup"><span data-stu-id="b1f80-142">The views now contain the MiddleName property.</span></span>

![Показать отчество](changing-the-database/_static/image5.png)

<span data-ttu-id="b1f80-144">В следующем разделе будет добавлен код для настройки представления для отображения сведений о записи учащегося.</span><span class="sxs-lookup"><span data-stu-id="b1f80-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b1f80-145">[Назад](generating-views.md)
> [Вперед](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="b1f80-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
