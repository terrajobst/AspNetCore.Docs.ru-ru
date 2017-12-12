---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: "Отображение таблицы базы данных (Visual Basic) | Документы Microsoft"
author: microsoft
description: "В этом учебнике я демонстрируются два способа отображения набора записей базы данных. Показать два метода форматирования набора записей базы данных в HTML-ta..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 6dc0aa91cfb68d308ed098f429d3251d424ab778
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="displaying-a-table-of-database-data-vb"></a><span data-ttu-id="d8bbc-104">Отображение таблицы базы данных (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="d8bbc-104">Displaying a Table of Database Data (VB)</span></span>
====================
<span data-ttu-id="d8bbc-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d8bbc-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="d8bbc-106">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="d8bbc-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> <span data-ttu-id="d8bbc-107">В этом учебнике я демонстрируются два способа отображения набора записей базы данных.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="d8bbc-108">Показать два метода форматирования набора записей базы данных в таблицу HTML.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="d8bbc-109">Во-первых я расскажу, как можно форматировать записи базы данных непосредственно в представлении.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="d8bbc-110">Далее я продемонстрирую, как можно воспользоваться преимуществами частичными репликами при форматировании записи базы данных.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="d8bbc-111">Целью данного учебника является объясняется, как отображать HTML-таблицы базы данных в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="d8bbc-112">Во-первых вы узнаете, как использовать средства формирования шаблонов, включенные в Visual Studio для создания представления, автоматически отображается набор записей.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="d8bbc-113">Далее показано, как использовать разделяемый в качестве шаблона при форматировании записи базы данных.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="d8bbc-114">Создать классы модели</span><span class="sxs-lookup"><span data-stu-id="d8bbc-114">Create the Model Classes</span></span>

<span data-ttu-id="d8bbc-115">Мы собираемся отображение набора записей из таблицы базы данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="d8bbc-116">Фильмы таблицы базы данных содержит следующие столбцы:</span><span class="sxs-lookup"><span data-stu-id="d8bbc-116">The Movies database table contains the following columns:</span></span>

<a id="0.4_table01"></a>


| <span data-ttu-id="d8bbc-117">**Имя столбца**</span><span class="sxs-lookup"><span data-stu-id="d8bbc-117">**Column Name**</span></span> | <span data-ttu-id="d8bbc-118">**Тип данных**</span><span class="sxs-lookup"><span data-stu-id="d8bbc-118">**Data Type**</span></span> | <span data-ttu-id="d8bbc-119">**Разрешить значения NULL**</span><span class="sxs-lookup"><span data-stu-id="d8bbc-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d8bbc-120">Идентификатор</span><span class="sxs-lookup"><span data-stu-id="d8bbc-120">Id</span></span> | <span data-ttu-id="d8bbc-121">Int</span><span class="sxs-lookup"><span data-stu-id="d8bbc-121">Int</span></span> | <span data-ttu-id="d8bbc-122">False</span><span class="sxs-lookup"><span data-stu-id="d8bbc-122">False</span></span> |
| <span data-ttu-id="d8bbc-123">Заголовок</span><span class="sxs-lookup"><span data-stu-id="d8bbc-123">Title</span></span> | <span data-ttu-id="d8bbc-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="d8bbc-124">Nvarchar(200)</span></span> | <span data-ttu-id="d8bbc-125">False</span><span class="sxs-lookup"><span data-stu-id="d8bbc-125">False</span></span> |
| <span data-ttu-id="d8bbc-126">Директор</span><span class="sxs-lookup"><span data-stu-id="d8bbc-126">Director</span></span> | <span data-ttu-id="d8bbc-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="d8bbc-127">NVarchar(50)</span></span> | <span data-ttu-id="d8bbc-128">False</span><span class="sxs-lookup"><span data-stu-id="d8bbc-128">False</span></span> |
| <span data-ttu-id="d8bbc-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="d8bbc-129">DateReleased</span></span> | <span data-ttu-id="d8bbc-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="d8bbc-130">DateTime</span></span> | <span data-ttu-id="d8bbc-131">False</span><span class="sxs-lookup"><span data-stu-id="d8bbc-131">False</span></span> |


<span data-ttu-id="d8bbc-132">Для представления таблицы фильмы в приложении ASP.NET MVC, необходимо создать класс модели.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="d8bbc-133">В этом учебнике мы используем для создания классов нашей модели Entity Framework корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d8bbc-134">В этом учебнике мы используем Entity Framework корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="d8bbc-135">Тем не менее важно понимать, что можно использовать множество различных технологий для взаимодействия с базой данных из приложения ASP.NET MVC, включая LINQ to SQL, NHibernate и ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="d8bbc-136">Выполните следующие действия, чтобы запустить мастер моделей EDM.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="d8bbc-137">Щелкните правой кнопкой мыши папку модели в окне обозревателя решений и выберите пункт меню **добавить, новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="d8bbc-138">Выберите **данные** категорию и выберите **ADO.NET Entity Data Model** шаблона.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="d8bbc-139">Присвойте имя модели данных *MoviesDBModel.edmx* и нажмите кнопку **добавить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="d8bbc-140">После нажатия кнопки Добавить появится мастер моделей EDM (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="d8bbc-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="d8bbc-141">Выполните следующие действия, чтобы завершить работу мастера.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="d8bbc-142">В **Выбор содержимого модели** шаг, выберите **создать из базы данных** параметр.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="d8bbc-143">В **Выбор подключения к данным** шаг, используйте *MoviesDB.mdf* подключения данных и имени *MoviesDBEntities* параметров подключения.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="d8bbc-144">Нажмите кнопку **Далее** кнопки.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="d8bbc-145">В **Выбор объектов базы данных** шаг, разверните узел таблицы, выберите таблицу, фильмов.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="d8bbc-146">Введите пространство имен *моделей* и нажмите кнопку **Готово** кнопки.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="d8bbc-147">[![Создание LINQ для классов SQL](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d8bbc-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span></span>

<span data-ttu-id="d8bbc-148">**На рисунке 01**: создание классов LINQ to SQL ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="d8bbc-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image2.png))</span></span>


<span data-ttu-id="d8bbc-149">После завершения мастера моделей EDM, откроется конструктор модели EDM.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="d8bbc-150">Конструктор должен отображать фильмы сущностей (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="d8bbc-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="d8bbc-151">[![Конструктор моделей EDM](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d8bbc-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span></span>

<span data-ttu-id="d8bbc-152">**На рисунке 02**: конструктор моделей EDM ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="d8bbc-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image4.png))</span></span>


<span data-ttu-id="d8bbc-153">Необходимо внести одно изменение, перед продолжением.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-153">We need to make one change before we continue.</span></span> <span data-ttu-id="d8bbc-154">Мастер EDM создает класс с именем модели *фильмов* , представляющий таблицу базы данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="d8bbc-155">Так как мы будем использовать фильмы класс для представления определенного фильма, необходимо изменить имя класса, чтобы быть *фильма* вместо *фильмов* (единственного числа вместо множественного числа).</span><span class="sxs-lookup"><span data-stu-id="d8bbc-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="d8bbc-156">Дважды щелкните имя класса в рабочей области конструктора и измените имя класса из фильмов на фильма.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="d8bbc-157">После внесения этого изменения, нажмите кнопку **Сохранить** кнопку (значок дискеты) для создания класса фильма.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="d8bbc-158">Создание контроллера фильмов</span><span class="sxs-lookup"><span data-stu-id="d8bbc-158">Create the Movies Controller</span></span>

<span data-ttu-id="d8bbc-159">Теперь, когда у нас есть способ представления нашим записям базы данных, можно создать контроллер, который возвращает коллекции фильмов.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="d8bbc-160">В окне обозревателя решений Visual Studio щелкните правой кнопкой мыши папку Controllers и выбрать пункт меню **Add, контроллер** (см. рис. 3).</span><span class="sxs-lookup"><span data-stu-id="d8bbc-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="d8bbc-161">[![Добавить контроллер меню](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d8bbc-161">[![The Add Controller Menu](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span></span>

<span data-ttu-id="d8bbc-162">**На рисунке 03**: добавить контроллер меню ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d8bbc-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image6.png))</span></span>


<span data-ttu-id="d8bbc-163">Когда **добавить контроллер** откроется диалоговое окно, введите имя контроллера MovieController (см. рис. 4).</span><span class="sxs-lookup"><span data-stu-id="d8bbc-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="d8bbc-164">Нажмите кнопку **добавить** кнопку, чтобы добавить новый контроллер.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="d8bbc-165">[![Диалоговое окно добавления контроллера](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="d8bbc-165">[![The Add Controller dialog](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span></span>

<span data-ttu-id="d8bbc-166">**На рисунке 04**: диалоговое окно добавления контроллера ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="d8bbc-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image8.png))</span></span>


<span data-ttu-id="d8bbc-167">Нам нужно изменить действие Index(), предоставляемые контроллером фильм, чтобы он возвращал набора записей базы данных.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="d8bbc-168">Контроллер, чтобы он выглядел контроллера в листинге 1.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="d8bbc-169">**Перечисление Controllers\MovieController.vb. 1.**</span><span class="sxs-lookup"><span data-stu-id="d8bbc-169">**Listing 1 – Controllers\MovieController.vb**</span></span>

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

<span data-ttu-id="d8bbc-170">В список 1 MoviesDBEntities класс используется для представления MoviesDB базы данных.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="d8bbc-171">Выражение *сущностей. MovieSet.ToList()* возвращает набор всех фильмов фильмы таблицы базы данных.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-171">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="d8bbc-172">Создание представления</span><span class="sxs-lookup"><span data-stu-id="d8bbc-172">Create the View</span></span>

<span data-ttu-id="d8bbc-173">Чтобы воспользоваться преимуществами формирование шаблонов, предоставляемых Visual Studio — самый простой способ отображения набора записей базы данных в таблицу HTML.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-173">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="d8bbc-174">Построить приложение, выбрав параметр меню **построить, построить решение**.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-174">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="d8bbc-175">Необходимо построить приложение, прежде чем открывать **добавить представление** диалогового окна или пользовательские классы данных не будет отображаться в диалоговом окне.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-175">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="d8bbc-176">Щелкните правой кнопкой мыши действие Index() и выбрать пункт меню **добавить представление** (см. рис. 5).</span><span class="sxs-lookup"><span data-stu-id="d8bbc-176">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="d8bbc-177">[![Добавление представления](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="d8bbc-177">[![Adding a view](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span></span>

<span data-ttu-id="d8bbc-178">**На рисунке 05**: Добавление представления ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="d8bbc-178">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image10.png))</span></span>


<span data-ttu-id="d8bbc-179">В **добавить представление** диалогового окна, установите флажок "с меткой" **создать строго типизированное представление**.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-179">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="d8bbc-180">Выберите класс фильма как **просматривать данные класс**.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-180">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="d8bbc-181">Выберите *списка* как **просматривать содержимое** (см. рис. 6).</span><span class="sxs-lookup"><span data-stu-id="d8bbc-181">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="d8bbc-182">Выбор этих параметров создает строго типизированное представление, отображающее список фильмов.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-182">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="d8bbc-183">[![Диалоговое окно добавления представления](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="d8bbc-183">[![The Add View dialog](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span></span>

<span data-ttu-id="d8bbc-184">**На рисунке 06**: диалоговое окно добавления представления ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="d8bbc-184">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image12.png))</span></span>


<span data-ttu-id="d8bbc-185">После нажатия кнопки **добавить** автоматически создается кнопка, представление в списке 2.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-185">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="d8bbc-186">Это представление содержит код, необходимый для прохода по коллекции фильмов и отображения всех свойств фильма.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-186">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="d8bbc-187">**Перечисление Views\Movie\Index.aspx. 2.**</span><span class="sxs-lookup"><span data-stu-id="d8bbc-187">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

<span data-ttu-id="d8bbc-188">Приложение можно запустить, выбрав параметр меню **отладка, начать отладку** (или при нажатии клавиши F5).</span><span class="sxs-lookup"><span data-stu-id="d8bbc-188">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="d8bbc-189">Запуск приложения запускает Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-189">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="d8bbc-190">Если перейти по URL-адресу /Movie вы увидите страницу на рис. 7.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-190">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="d8bbc-191">[![Таблица фильмов](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="d8bbc-191">[![A table of movies](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span></span>

<span data-ttu-id="d8bbc-192">**На рисунке 07**: таблица фильмов ([Просмотр полноразмерное изображение](displaying-a-table-of-database-data-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="d8bbc-192">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image14.png))</span></span>


<span data-ttu-id="d8bbc-193">Если вам не нравится что-либо о внешний вид сетки, записей базы данных на рис. 7 можно просто изменить представление Index.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-193">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="d8bbc-194">Например, можно изменить *DateReleased* заголовок *Дата выпуска* , изменив индекс представления.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-194">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="d8bbc-195">Создание шаблона с частичной</span><span class="sxs-lookup"><span data-stu-id="d8bbc-195">Create a Template with a Partial</span></span>

<span data-ttu-id="d8bbc-196">При представлении станет слишком сложным, рекомендуется запустить остановом представление частичными репликами.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-196">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="d8bbc-197">С помощью частичными репликами упрощает представлений для понимания и поддержки.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-197">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="d8bbc-198">Мы создадим разделяемый, можно использовать в качестве шаблона для форматирования каждой записи фильма базы данных.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-198">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="d8bbc-199">Выполните следующие действия для создания изменяющего столбец разделяемого.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-199">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="d8bbc-200">Щелкните правой кнопкой мыши папку Views\Movie и выбрать пункт меню **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-200">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="d8bbc-201">Установите флажок "с меткой" *Создать разделяемое представление (ASCX)*.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-201">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="d8bbc-202">Имя частичного *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-202">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="d8bbc-203">Установите флажок "с меткой" **создать строго типизированное представление**.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-203">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="d8bbc-204">Выберите фильм в качестве *просматривать данные класс*.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-204">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="d8bbc-205">Выберите пустой как *просматривать содержимое*.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-205">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="d8bbc-206">Нажмите кнопку **добавить** кнопку, чтобы добавить разделяемом в проект.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-206">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="d8bbc-207">После выполнения этих действий изменения частичное MovieTemplate должен выглядеть как листинге 3.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-207">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="d8bbc-208">**Листинг 3 – Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="d8bbc-208">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

<span data-ttu-id="d8bbc-209">Partial в списке 3 содержит шаблон для одной строки записей.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-209">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="d8bbc-210">Измененный представление Index в листинге 4 использует MovieTemplate частичной.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-210">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="d8bbc-211">**Листинг 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="d8bbc-211">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

<span data-ttu-id="d8bbc-212">Представление со списком 4 содержит For каждый цикл, который проходит через все фильмов.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-212">The view in Listing 4 contains a For Each loop that iterates through all of the movies.</span></span> <span data-ttu-id="d8bbc-213">Для каждого фильма частичного MovieTemplate используется для форматирования фильма.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-213">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="d8bbc-214">Путем вызова вспомогательного метода RenderPartial() MovieTemplate подготавливается к просмотру.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-214">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="d8bbc-215">Измененный представление индекса представляет то же самое таблицу HTML записей базы данных.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-215">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="d8bbc-216">Однако представление значительно упрощен.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-216">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="d8bbc-217">Метод RenderPartial() отличается от большинства других вспомогательных методов, так как он не возвращает строки.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-217">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="d8bbc-218">Таким образом, необходимо вызвать метод RenderPartial() с помощью &lt;% Html.RenderPartial() %&gt; вместо &lt;% = Html.RenderPartial() %&gt;.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-218">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial() %&gt; instead of &lt;%= Html.RenderPartial() %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="d8bbc-219">Сводка</span><span class="sxs-lookup"><span data-stu-id="d8bbc-219">Summary</span></span>

<span data-ttu-id="d8bbc-220">Целью данного учебника было объясняется, как отображать набора записей базы данных в таблицу HTML.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-220">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="d8bbc-221">Во-первых вы узнали, как получить набор записей базы данных из действия контроллера, используя преимущества платформы Entity Framework корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-221">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="d8bbc-222">Далее вы узнали, как использовать формирование шаблонов Visual Studio для создания представления, автоматически отображается коллекция элементов.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-222">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="d8bbc-223">Наконец вы узнали, как для упрощения просмотра, используя преимущества частичной.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-223">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="d8bbc-224">Вы узнали, как использовать разделяемый в качестве шаблона, что можно форматировать каждой записи базы данных.</span><span class="sxs-lookup"><span data-stu-id="d8bbc-224">You learned how to use a partial as a template so that you can format each database record.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d8bbc-225">[Назад](creating-model-classes-with-linq-to-sql-vb.md)
[Вперед](performing-simple-validation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d8bbc-225">[Previous](creating-model-classes-with-linq-to-sql-vb.md)
[Next](performing-simple-validation-vb.md)</span></span>
