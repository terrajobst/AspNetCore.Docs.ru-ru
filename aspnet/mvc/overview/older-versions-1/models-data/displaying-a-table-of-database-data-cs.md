---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Отображение таблицы базы данных (C#) | Документы Microsoft
author: microsoft
description: В этом учебнике я демонстрируются два способа отображения набора записей базы данных. Показать два метода форматирования набора записей базы данных в HTML-ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 1d5dc9dd4a82e4577c6c1a3b124d45fef0b0f67c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="displaying-a-table-of-database-data-c"></a><span data-ttu-id="53034-104">Отображение таблицы базы данных (C#)</span><span class="sxs-lookup"><span data-stu-id="53034-104">Displaying a Table of Database Data (C#)</span></span>
====================
<span data-ttu-id="53034-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="53034-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="53034-106">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="53034-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> <span data-ttu-id="53034-107">В этом учебнике я демонстрируются два способа отображения набора записей базы данных.</span><span class="sxs-lookup"><span data-stu-id="53034-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="53034-108">Показать два метода форматирования набора записей базы данных в таблицу HTML.</span><span class="sxs-lookup"><span data-stu-id="53034-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="53034-109">Во-первых я расскажу, как можно форматировать записи базы данных непосредственно в представлении.</span><span class="sxs-lookup"><span data-stu-id="53034-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="53034-110">Далее я продемонстрирую, как можно воспользоваться преимуществами частичными репликами при форматировании записи базы данных.</span><span class="sxs-lookup"><span data-stu-id="53034-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="53034-111">Целью данного учебника является объясняется, как отображать HTML-таблицы базы данных в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="53034-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="53034-112">Во-первых вы узнаете, как использовать средства формирования шаблонов, включенные в Visual Studio для создания представления, автоматически отображается набор записей.</span><span class="sxs-lookup"><span data-stu-id="53034-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="53034-113">Далее показано, как использовать разделяемый в качестве шаблона при форматировании записи базы данных.</span><span class="sxs-lookup"><span data-stu-id="53034-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="53034-114">Создать классы модели</span><span class="sxs-lookup"><span data-stu-id="53034-114">Create the Model Classes</span></span>

<span data-ttu-id="53034-115">Мы собираемся отображение набора записей из таблицы базы данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="53034-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="53034-116">Фильмы таблицы базы данных содержит следующие столбцы:</span><span class="sxs-lookup"><span data-stu-id="53034-116">The Movies database table contains the following columns:</span></span>

<a id="0.3_table01"></a>


| <span data-ttu-id="53034-117">**Имя столбца**</span><span class="sxs-lookup"><span data-stu-id="53034-117">**Column Name**</span></span> | <span data-ttu-id="53034-118">**Тип данных**</span><span class="sxs-lookup"><span data-stu-id="53034-118">**Data Type**</span></span> | <span data-ttu-id="53034-119">**Разрешить значения NULL**</span><span class="sxs-lookup"><span data-stu-id="53034-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="53034-120">Идентификатор</span><span class="sxs-lookup"><span data-stu-id="53034-120">Id</span></span> | <span data-ttu-id="53034-121">Int</span><span class="sxs-lookup"><span data-stu-id="53034-121">Int</span></span> | <span data-ttu-id="53034-122">False</span><span class="sxs-lookup"><span data-stu-id="53034-122">False</span></span> |
| <span data-ttu-id="53034-123">Заголовок</span><span class="sxs-lookup"><span data-stu-id="53034-123">Title</span></span> | <span data-ttu-id="53034-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="53034-124">Nvarchar(200)</span></span> | <span data-ttu-id="53034-125">False</span><span class="sxs-lookup"><span data-stu-id="53034-125">False</span></span> |
| <span data-ttu-id="53034-126">Директор</span><span class="sxs-lookup"><span data-stu-id="53034-126">Director</span></span> | <span data-ttu-id="53034-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="53034-127">NVarchar(50)</span></span> | <span data-ttu-id="53034-128">False</span><span class="sxs-lookup"><span data-stu-id="53034-128">False</span></span> |
| <span data-ttu-id="53034-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="53034-129">DateReleased</span></span> | <span data-ttu-id="53034-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="53034-130">DateTime</span></span> | <span data-ttu-id="53034-131">False</span><span class="sxs-lookup"><span data-stu-id="53034-131">False</span></span> |


<span data-ttu-id="53034-132">Для представления таблицы фильмы в приложении ASP.NET MVC, необходимо создать класс модели.</span><span class="sxs-lookup"><span data-stu-id="53034-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="53034-133">В этом учебнике мы используем для создания классов нашей модели Entity Framework корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="53034-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="53034-134">В этом учебнике мы используем Entity Framework корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="53034-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="53034-135">Тем не менее важно понимать, что можно использовать множество различных технологий для взаимодействия с базой данных из приложения ASP.NET MVC, включая LINQ to SQL, NHibernate и ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="53034-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="53034-136">Выполните следующие действия, чтобы запустить мастер моделей EDM.</span><span class="sxs-lookup"><span data-stu-id="53034-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="53034-137">Щелкните правой кнопкой мыши папку модели в окне обозревателя решений и выберите пункт меню **добавить, новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="53034-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="53034-138">Выберите **данные** категорию и выберите **ADO.NET Entity Data Model** шаблона.</span><span class="sxs-lookup"><span data-stu-id="53034-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="53034-139">Присвойте имя модели данных *MoviesDBModel.edmx* и нажмите кнопку **добавить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="53034-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="53034-140">После нажатия кнопки Добавить появится мастер моделей EDM (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="53034-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="53034-141">Выполните следующие действия, чтобы завершить работу мастера.</span><span class="sxs-lookup"><span data-stu-id="53034-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="53034-142">В **Выбор содержимого модели** шаг, выберите **создать из базы данных** параметр.</span><span class="sxs-lookup"><span data-stu-id="53034-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="53034-143">В **Выбор подключения к данным** шаг, используйте *MoviesDB.mdf* подключения данных и имени *MoviesDBEntities* параметров подключения.</span><span class="sxs-lookup"><span data-stu-id="53034-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="53034-144">Нажмите кнопку **Далее** кнопки.</span><span class="sxs-lookup"><span data-stu-id="53034-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="53034-145">В **Выбор объектов базы данных** шаг, разверните узел таблицы, выберите таблицу, фильмов.</span><span class="sxs-lookup"><span data-stu-id="53034-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="53034-146">Введите пространство имен *моделей* и нажмите кнопку **Готово** кнопки.</span><span class="sxs-lookup"><span data-stu-id="53034-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="53034-147">[![Создание LINQ для классов SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="53034-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="53034-148">**На рисунке 01**: создание классов LINQ to SQL ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="53034-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image2.png))</span></span>


<span data-ttu-id="53034-149">После завершения мастера моделей EDM, откроется конструктор модели EDM.</span><span class="sxs-lookup"><span data-stu-id="53034-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="53034-150">Конструктор должен отображать фильмы сущностей (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="53034-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="53034-151">[![Конструктор моделей EDM](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="53034-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span></span>

<span data-ttu-id="53034-152">**На рисунке 02**: конструктор моделей EDM ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="53034-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image4.png))</span></span>


<span data-ttu-id="53034-153">Необходимо внести одно изменение, перед продолжением.</span><span class="sxs-lookup"><span data-stu-id="53034-153">We need to make one change before we continue.</span></span> <span data-ttu-id="53034-154">Мастер EDM создает класс с именем модели *фильмов* , представляющий таблицу базы данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="53034-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="53034-155">Так как мы будем использовать фильмы класс для представления определенного фильма, необходимо изменить имя класса, чтобы быть *фильма* вместо *фильмов* (единственного числа вместо множественного числа).</span><span class="sxs-lookup"><span data-stu-id="53034-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="53034-156">Дважды щелкните имя класса в рабочей области конструктора и измените имя класса из фильмов на фильма.</span><span class="sxs-lookup"><span data-stu-id="53034-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="53034-157">После внесения этого изменения, нажмите кнопку **Сохранить** кнопку (значок дискеты) для создания класса фильма.</span><span class="sxs-lookup"><span data-stu-id="53034-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="53034-158">Создание контроллера фильмов</span><span class="sxs-lookup"><span data-stu-id="53034-158">Create the Movies Controller</span></span>

<span data-ttu-id="53034-159">Теперь, когда у нас есть способ представления нашим записям базы данных, можно создать контроллер, который возвращает коллекции фильмов.</span><span class="sxs-lookup"><span data-stu-id="53034-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="53034-160">В окне обозревателя решений Visual Studio щелкните правой кнопкой мыши папку Controllers и выбрать пункт меню **Add, контроллер** (см. рис. 3).</span><span class="sxs-lookup"><span data-stu-id="53034-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="53034-161">[![Добавить контроллер меню](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="53034-161">[![The Add Controller Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span></span>

<span data-ttu-id="53034-162">**На рисунке 03**: добавить контроллер меню ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="53034-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image6.png))</span></span>


<span data-ttu-id="53034-163">Когда **добавить контроллер** откроется диалоговое окно, введите имя контроллера MovieController (см. рис. 4).</span><span class="sxs-lookup"><span data-stu-id="53034-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="53034-164">Нажмите кнопку **добавить** кнопку, чтобы добавить новый контроллер.</span><span class="sxs-lookup"><span data-stu-id="53034-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="53034-165">[![Диалоговое окно добавления контроллера](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="53034-165">[![The Add Controller dialog](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span></span>

<span data-ttu-id="53034-166">**На рисунке 04**: диалоговое окно добавления контроллера ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="53034-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image8.png))</span></span>


<span data-ttu-id="53034-167">Нам нужно изменить действие Index(), предоставляемые контроллером фильм, чтобы он возвращал набора записей базы данных.</span><span class="sxs-lookup"><span data-stu-id="53034-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="53034-168">Контроллер, чтобы он выглядел контроллера в листинге 1.</span><span class="sxs-lookup"><span data-stu-id="53034-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="53034-169">**Перечисление Controllers\MovieController.cs. 1.**</span><span class="sxs-lookup"><span data-stu-id="53034-169">**Listing 1 – Controllers\MovieController.cs**</span></span>

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

<span data-ttu-id="53034-170">В список 1 MoviesDBEntities класс используется для представления MoviesDB базы данных.</span><span class="sxs-lookup"><span data-stu-id="53034-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="53034-171">Чтобы использовать этот класс, необходимо импортировать пространство имен MvcApplication1.Models следующим образом:</span><span class="sxs-lookup"><span data-stu-id="53034-171">To use this class, you need to import the MvcApplication1.Models namespace like this:</span></span>

<span data-ttu-id="53034-172">с помощью MvcApplication1.Models;</span><span class="sxs-lookup"><span data-stu-id="53034-172">using MvcApplication1.Models;</span></span>

<span data-ttu-id="53034-173">Выражение *сущностей. MovieSet.ToList()* возвращает набор всех фильмов фильмы таблицы базы данных.</span><span class="sxs-lookup"><span data-stu-id="53034-173">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="53034-174">Создание представления</span><span class="sxs-lookup"><span data-stu-id="53034-174">Create the View</span></span>

<span data-ttu-id="53034-175">Чтобы воспользоваться преимуществами формирование шаблонов, предоставляемых Visual Studio — самый простой способ отображения набора записей базы данных в таблицу HTML.</span><span class="sxs-lookup"><span data-stu-id="53034-175">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="53034-176">Построить приложение, выбрав параметр меню **построить, построить решение**.</span><span class="sxs-lookup"><span data-stu-id="53034-176">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="53034-177">Необходимо построить приложение, прежде чем открывать **добавить представление** диалогового окна или пользовательские классы данных не будет отображаться в диалоговом окне.</span><span class="sxs-lookup"><span data-stu-id="53034-177">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="53034-178">Щелкните правой кнопкой мыши действие Index() и выбрать пункт меню **добавить представление** (см. рис. 5).</span><span class="sxs-lookup"><span data-stu-id="53034-178">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="53034-179">[![Добавление представления](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="53034-179">[![Adding a view](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="53034-180">**На рисунке 05**: Добавление представления ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="53034-180">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="53034-181">В **добавить представление** диалогового окна, установите флажок "с меткой" **создать строго типизированное представление**.</span><span class="sxs-lookup"><span data-stu-id="53034-181">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="53034-182">Выберите класс фильма как **просматривать данные класс**.</span><span class="sxs-lookup"><span data-stu-id="53034-182">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="53034-183">Выберите *списка* как **просматривать содержимое** (см. рис. 6).</span><span class="sxs-lookup"><span data-stu-id="53034-183">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="53034-184">Выбор этих параметров создает строго типизированное представление, отображающее список фильмов.</span><span class="sxs-lookup"><span data-stu-id="53034-184">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="53034-185">[![Диалоговое окно добавления представления](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="53034-185">[![The Add View dialog](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="53034-186">**На рисунке 06**: диалоговое окно добавления представления ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="53034-186">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image12.png))</span></span>


<span data-ttu-id="53034-187">После нажатия кнопки **добавить** автоматически создается кнопка, представление в списке 2.</span><span class="sxs-lookup"><span data-stu-id="53034-187">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="53034-188">Это представление содержит код, необходимый для прохода по коллекции фильмов и отображения всех свойств фильма.</span><span class="sxs-lookup"><span data-stu-id="53034-188">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="53034-189">**Перечисление Views\Movie\Index.aspx. 2.**</span><span class="sxs-lookup"><span data-stu-id="53034-189">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

<span data-ttu-id="53034-190">Приложение можно запустить, выбрав параметр меню **отладка, начать отладку** (или при нажатии клавиши F5).</span><span class="sxs-lookup"><span data-stu-id="53034-190">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="53034-191">Запуск приложения запускает Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="53034-191">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="53034-192">Если перейти по URL-адресу /Movie вы увидите страницу на рис. 7.</span><span class="sxs-lookup"><span data-stu-id="53034-192">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="53034-193">[![Таблица фильмов](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="53034-193">[![A table of movies](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span></span>

<span data-ttu-id="53034-194">**На рисунке 07**: таблица фильмов ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="53034-194">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="53034-195">Если вам не нравится что-либо о внешний вид сетки, записей базы данных на рис. 7 можно просто изменить представление Index.</span><span class="sxs-lookup"><span data-stu-id="53034-195">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="53034-196">Например, можно изменить *DateReleased* заголовок *Дата выпуска* , изменив индекс представления.</span><span class="sxs-lookup"><span data-stu-id="53034-196">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="53034-197">Создание шаблона с частичной</span><span class="sxs-lookup"><span data-stu-id="53034-197">Create a Template with a Partial</span></span>

<span data-ttu-id="53034-198">При представлении станет слишком сложным, рекомендуется запустить остановом представление частичными репликами.</span><span class="sxs-lookup"><span data-stu-id="53034-198">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="53034-199">С помощью частичными репликами упрощает представлений для понимания и поддержки.</span><span class="sxs-lookup"><span data-stu-id="53034-199">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="53034-200">Мы создадим разделяемый, можно использовать в качестве шаблона для форматирования каждой записи фильма базы данных.</span><span class="sxs-lookup"><span data-stu-id="53034-200">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="53034-201">Выполните следующие действия для создания изменяющего столбец разделяемого.</span><span class="sxs-lookup"><span data-stu-id="53034-201">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="53034-202">Щелкните правой кнопкой мыши папку Views\Movie и выбрать пункт меню **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="53034-202">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="53034-203">Установите флажок "с меткой" *Создать разделяемое представление (ASCX)*.</span><span class="sxs-lookup"><span data-stu-id="53034-203">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="53034-204">Имя частичного *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="53034-204">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="53034-205">Установите флажок "с меткой" **создать строго типизированное представление**.</span><span class="sxs-lookup"><span data-stu-id="53034-205">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="53034-206">Выберите фильм в качестве *просматривать данные класс*.</span><span class="sxs-lookup"><span data-stu-id="53034-206">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="53034-207">Выберите пустой как *просматривать содержимое*.</span><span class="sxs-lookup"><span data-stu-id="53034-207">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="53034-208">Нажмите кнопку **добавить** кнопку, чтобы добавить разделяемом в проект.</span><span class="sxs-lookup"><span data-stu-id="53034-208">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="53034-209">После выполнения этих действий изменения частичное MovieTemplate должен выглядеть как листинге 3.</span><span class="sxs-lookup"><span data-stu-id="53034-209">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="53034-210">**Листинг 3 – Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="53034-210">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

<span data-ttu-id="53034-211">Partial в списке 3 содержит шаблон для одной строки записей.</span><span class="sxs-lookup"><span data-stu-id="53034-211">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="53034-212">Измененный представление Index в листинге 4 использует MovieTemplate частичной.</span><span class="sxs-lookup"><span data-stu-id="53034-212">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="53034-213">**Листинг 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="53034-213">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

<span data-ttu-id="53034-214">Представление со списком 4 содержит цикл, который выполняет итерацию всех фильмы.</span><span class="sxs-lookup"><span data-stu-id="53034-214">The view in Listing 4 contains a foreach loop that iterates through all of the movies.</span></span> <span data-ttu-id="53034-215">Для каждого фильма частичного MovieTemplate используется для форматирования фильма.</span><span class="sxs-lookup"><span data-stu-id="53034-215">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="53034-216">Путем вызова вспомогательного метода RenderPartial() MovieTemplate подготавливается к просмотру.</span><span class="sxs-lookup"><span data-stu-id="53034-216">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="53034-217">Измененный представление индекса представляет то же самое таблицу HTML записей базы данных.</span><span class="sxs-lookup"><span data-stu-id="53034-217">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="53034-218">Однако представление значительно упрощен.</span><span class="sxs-lookup"><span data-stu-id="53034-218">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="53034-219">Метод RenderPartial() отличается от большинства других вспомогательных методов, так как он не возвращает строки.</span><span class="sxs-lookup"><span data-stu-id="53034-219">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="53034-220">Таким образом, необходимо вызвать метод RenderPartial() с помощью &lt;% Html.RenderPartial(); %&gt; вместо &lt;% = Html.RenderPartial(); %&gt;.</span><span class="sxs-lookup"><span data-stu-id="53034-220">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial(); %&gt; instead of &lt;%= Html.RenderPartial(); %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="53034-221">Сводка</span><span class="sxs-lookup"><span data-stu-id="53034-221">Summary</span></span>

<span data-ttu-id="53034-222">Целью данного учебника было объясняется, как отображать набора записей базы данных в таблицу HTML.</span><span class="sxs-lookup"><span data-stu-id="53034-222">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="53034-223">Во-первых вы узнали, как получить набор записей базы данных из действия контроллера, используя преимущества платформы Entity Framework корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="53034-223">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="53034-224">Далее вы узнали, как использовать формирование шаблонов Visual Studio для создания представления, автоматически отображается коллекция элементов.</span><span class="sxs-lookup"><span data-stu-id="53034-224">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="53034-225">Наконец вы узнали, как для упрощения просмотра, используя преимущества частичной.</span><span class="sxs-lookup"><span data-stu-id="53034-225">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="53034-226">Вы узнали, как использовать разделяемый в качестве шаблона, что можно форматировать каждой записи базы данных.</span><span class="sxs-lookup"><span data-stu-id="53034-226">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="53034-227">[Назад](creating-model-classes-with-linq-to-sql-cs.md)
> [Вперед](performing-simple-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="53034-227">[Previous](creating-model-classes-with-linq-to-sql-cs.md)
[Next](performing-simple-validation-cs.md)</span></span>
