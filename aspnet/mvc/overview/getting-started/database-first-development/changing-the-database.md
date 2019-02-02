---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: Учебник. Изменить базу данных для EF Database First с помощью приложения ASP.NET MVC
description: Это руководство посвящено выполнении обновления структуры базы данных и распространение этого изменения на протяжении всего веб-приложения.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667639"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="1f3a3-103">Учебник. Изменить базу данных для EF Database First с помощью приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="1f3a3-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="1f3a3-104">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="1f3a3-105">В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="1f3a3-106">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="1f3a3-107">Это руководство посвящено выполнении обновления структуры базы данных и распространение этого изменения на протяжении всего веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="1f3a3-108">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1f3a3-109">Добавление столбца</span><span class="sxs-lookup"><span data-stu-id="1f3a3-109">Add a column</span></span>
> * <span data-ttu-id="1f3a3-110">Добавьте свойство к представлениям</span><span class="sxs-lookup"><span data-stu-id="1f3a3-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f3a3-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="1f3a3-111">Prerequisites</span></span>

* [<span data-ttu-id="1f3a3-112">Создание представлений</span><span class="sxs-lookup"><span data-stu-id="1f3a3-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="1f3a3-113">Добавление столбца</span><span class="sxs-lookup"><span data-stu-id="1f3a3-113">Add a column</span></span>

<span data-ttu-id="1f3a3-114">Если обновить структуру таблицы в базе данных, необходимо убедиться, что изменение распространяется на модели данных, представления и контроллера.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="1f3a3-115">В этом учебнике вы добавите нового столбца на таблицу "Student" для записи отчество учащегося.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="1f3a3-116">Чтобы добавить этот столбец, откройте проект базы данных и откройте файл Student.sql.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="1f3a3-117">Через конструктор или кода T-SQL, добавьте столбец с именем **MiddleName** , NVARCHAR(50) и допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="1f3a3-118">Разверните это изменение в локальную базу данных, запустив проект базы данных (или F5).</span><span class="sxs-lookup"><span data-stu-id="1f3a3-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="1f3a3-119">Новое поле добавляется в таблицу.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-119">The new field is added to the table.</span></span> <span data-ttu-id="1f3a3-120">Если вы не видите его в обозревателе объектов SQL Server, нажмите кнопку "Обновить" в области.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Показать новый столбец](changing-the-database/_static/image2.png)

<span data-ttu-id="1f3a3-122">Новый столбец существует в таблице базы данных, но он не существует в классе модели данных.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="1f3a3-123">Необходимо обновить модель, чтобы включить новый столбец.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-123">You must update the model to include your new column.</span></span> <span data-ttu-id="1f3a3-124">В **моделей** откройте **ContosoModel.edmx** файл для отображения на схеме модели.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="1f3a3-125">Обратите внимание на то, что модель Student не содержит свойство MiddleName.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="1f3a3-126">Щелкните правой кнопкой мыши в области конструктора и выберите **обновить модель из базы данных**.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="1f3a3-127">В мастере обновлений, выберите **обновить** , а затем — **таблиц** > **dbo** > **учащихся**.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="1f3a3-128">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-128">Click **Finish**.</span></span>

<span data-ttu-id="1f3a3-129">После завершения процесса обновления схемы базы данных включает в себя новый **MiddleName** свойство.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="1f3a3-130">Сохранить **ContosoModel.edmx** файла.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="1f3a3-131">Сохраните этот файл для нового свойства передаются на **Student.cs** класса.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="1f3a3-132">Теперь обновления базы данных и модели.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="1f3a3-133">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="1f3a3-134">Добавьте свойство к представлениям</span><span class="sxs-lookup"><span data-stu-id="1f3a3-134">Add the property to the views</span></span>

<span data-ttu-id="1f3a3-135">К сожалению, представления по-прежнему не содержат новое свойство.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="1f3a3-136">Для обновления представления можно двумя способами - можно повторно создать представления, снова добавив формирования шаблонов для класса Student, или можно вручную добавить новое свойство в существующие представления.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="1f3a3-137">В этом руководстве вы добавите формирование шаблонов еще раз, так как не были внесены настраиваемые изменения в представления, автоматически создается.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="1f3a3-138">Можно вручную добавить свойство, когда были внесены изменения в представления и не хотите потерять внесенные изменения.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="1f3a3-139">Чтобы обеспечить представления создаются повторно, удалите **учащихся** папке **представления**и удалить **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="1f3a3-140">Щелкните, правой кнопкой мыши **контроллеров** папки и добавление формирования шаблонов для **учащихся** модели.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="1f3a3-141">Опять же, назовите контроллер **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="1f3a3-142">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-142">Select **Add**.</span></span>

<span data-ttu-id="1f3a3-143">Сборку решения еще раз.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-143">Build the solution again.</span></span> <span data-ttu-id="1f3a3-144">Представлений теперь содержит указанное свойство MiddleName.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-144">The views now contain the MiddleName property.</span></span>

![Показать отчество](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="1f3a3-146">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="1f3a3-146">Next steps</span></span>

<span data-ttu-id="1f3a3-147">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1f3a3-148">Добавлен столбец</span><span class="sxs-lookup"><span data-stu-id="1f3a3-148">Added a column</span></span>
> * <span data-ttu-id="1f3a3-149">Добавлено свойство к представлениям</span><span class="sxs-lookup"><span data-stu-id="1f3a3-149">Added the property to the views</span></span>

<span data-ttu-id="1f3a3-150">Перейдите к следующему руководству, чтобы узнать, как настраивать представление для отображения сведений о записи учащегося.</span><span class="sxs-lookup"><span data-stu-id="1f3a3-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="1f3a3-151">Настройка представления</span><span class="sxs-lookup"><span data-stu-id="1f3a3-151">Customize a view</span></span>](customizing-a-view.md)