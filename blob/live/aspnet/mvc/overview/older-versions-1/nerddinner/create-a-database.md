---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: "Создание базы данных | Документы Microsoft"
author: microsoft
description: "Шаг 2 показаны действия для создания базы данных, содержащий все компании dinner и RSVP данных для нашего приложения обновление NerdDinner."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 7635722fc357356edd06fb4cff301a8c4dfebbef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="create-a-database"></a><span data-ttu-id="97229-103">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="97229-103">Create a Database</span></span>
====================
<span data-ttu-id="97229-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="97229-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="97229-105">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="97229-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="97229-106">Это шаг 2 бесплатных [учебника «Обновление NerdDinner» приложения](introducing-the-nerddinner-tutorial.md) , обходов сквозной как построить небольшой, но завершается веб-приложения с помощью ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="97229-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="97229-107">Шаг 2 показаны действия для создания базы данных, содержащий все компании dinner и RSVP данных для нашего приложения обновление NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="97229-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="97229-108">При использовании ASP.NET MVC 3, которому рекомендуется следовать [Приступая к работе с MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) или [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) учебники.</span><span class="sxs-lookup"><span data-stu-id="97229-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="97229-109">Обновление NerdDinner шаг 2: Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="97229-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="97229-110">Мы будем работать с базы данных для хранения всех данных компании Dinner и RSVP для нашего приложения обновление NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="97229-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="97229-111">Ниже описывается создание базы данных, используя бесплатный выпуск SQL Server Express (которые легко можно установить с помощью V2 из [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="97229-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="97229-112">Весь код, мы записываем работает с SQL Server Express и полного сервера SQL.</span><span class="sxs-lookup"><span data-stu-id="97229-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="97229-113">Создание новой базы данных SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="97229-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="97229-114">Мы будем начать, щелкните правой кнопкой мыши на наш веб-проекта, а затем выберите **Add -&gt;новый элемент** команды меню:</span><span class="sxs-lookup"><span data-stu-id="97229-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="97229-115">Откроется диалоговое окно «Добавление нового элемента» Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="97229-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="97229-116">Мы фильтрация по категории «Данные» и выберите шаблон «Базы данных SQL Server»:</span><span class="sxs-lookup"><span data-stu-id="97229-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="97229-117">Мы назовем базу данных SQL Server Express, необходимо создать «NerdDinner.mdf» и нажмите ОК.</span><span class="sxs-lookup"><span data-stu-id="97229-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="97229-118">Visual Studio будет затем спросите нас, если мы хотим добавить этот файл в нашем \App\_каталог данных (которого является каталогом установки с чтения и записи безопасности списки управления доступом):</span><span class="sxs-lookup"><span data-stu-id="97229-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="97229-119">Мы будем нажмите «Да» и нашей новой базы данных будет создана и добавлена в наш обозреватель решений:</span><span class="sxs-lookup"><span data-stu-id="97229-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="97229-120">Создание таблиц в базе данных</span><span class="sxs-lookup"><span data-stu-id="97229-120">Creating Tables within our Database</span></span>

<span data-ttu-id="97229-121">Теперь у нас есть новую пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="97229-121">We now have a new empty database.</span></span> <span data-ttu-id="97229-122">Давайте добавим некоторые таблицы к ней.</span><span class="sxs-lookup"><span data-stu-id="97229-122">Let's add some tables to it.</span></span>

<span data-ttu-id="97229-123">Для этого мы перейдите в окно вкладка «Обозреватель серверов» в Visual Studio, которая позволяет управлять базы данных и серверы.</span><span class="sxs-lookup"><span data-stu-id="97229-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="97229-124">SQL Server, экспресс-выпуск базы данных, хранимые в \App\_папку данных приложения автоматически будут отображаться в обозревателе серверов.</span><span class="sxs-lookup"><span data-stu-id="97229-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="97229-125">Значок «Соединение с базой данных» в верхней части окна «Обозреватель серверов» при необходимости можно использовать для добавления в список, а также дополнительные базы данных SQL Server (локальные и удаленные):</span><span class="sxs-lookup"><span data-stu-id="97229-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="97229-126">Мы добавим две таблицы обновление NerdDinner базе данных — один для хранения нашей ужинов, а другая — для отслеживания RSVP принятия к ним.</span><span class="sxs-lookup"><span data-stu-id="97229-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="97229-127">Мы можем создать новые таблицы, щелкнув папку «Таблицы» в базе данных и выбрать команду меню «Добавить новую таблицу»:</span><span class="sxs-lookup"><span data-stu-id="97229-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="97229-128">То будет открыт конструктор таблицы, который позволяет настроить схему нашей таблице.</span><span class="sxs-lookup"><span data-stu-id="97229-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="97229-129">Для нашей таблице «Ужинов» мы добавим 10 столбцов данных:</span><span class="sxs-lookup"><span data-stu-id="97229-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="97229-130">Нам нужен столбец «DinnerID» в уникальный первичного ключа для таблицы.</span><span class="sxs-lookup"><span data-stu-id="97229-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="97229-131">Мы можно настроить, щелкнув правой кнопкой мыши по столбцу «DinnerID» и выбрав пункт меню «Задать первичный ключ».</span><span class="sxs-lookup"><span data-stu-id="97229-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="97229-132">Помимо внесения DinnerID первичный ключ, также необходимо настроить его как столбец «удостоверение», значение которого автоматически увеличивается на единицу при добавлении новой строки данных таблицы (первой строки вставляются Dinner будет иметь DinnerID 1 это означает, что второй вставляемые строки будет иметь DinnerID 2, и т. д).</span><span class="sxs-lookup"><span data-stu-id="97229-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="97229-133">Мы может это сделать, выбрав столбец «DinnerID» и затем с помощью редактора «Свойства столбца» для задания свойства "(Is Identity)» в столбце «Да».</span><span class="sxs-lookup"><span data-stu-id="97229-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="97229-134">Мы будем использовать значения по умолчанию стандартной (начинается с 1 и увеличивается на 1 для каждой новой строки компании Dinner):</span><span class="sxs-lookup"><span data-stu-id="97229-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="97229-135">Мы будем сохраните нашей таблице путем ввода Ctrl-S или с помощью **файл -&gt;Сохранить** команду меню.</span><span class="sxs-lookup"><span data-stu-id="97229-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="97229-136">Система предложит нам назовите таблицу.</span><span class="sxs-lookup"><span data-stu-id="97229-136">This will prompt us to name the table.</span></span> <span data-ttu-id="97229-137">Мы назовем ее «Ужинов»:</span><span class="sxs-lookup"><span data-stu-id="97229-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="97229-138">Затем нашей новой таблицы ужинов будут отображаться в базе данных в обозревателе серверов.</span><span class="sxs-lookup"><span data-stu-id="97229-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="97229-139">Мы будем повторите описанные выше шаги и создать таблицу «RSVP».</span><span class="sxs-lookup"><span data-stu-id="97229-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="97229-140">Таблица с содержит три столбца.</span><span class="sxs-lookup"><span data-stu-id="97229-140">This table with have 3 columns.</span></span> <span data-ttu-id="97229-141">Мы будет установки RsvpID столбец как первичный ключ, а также сделать столбец идентификаторов:</span><span class="sxs-lookup"><span data-stu-id="97229-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="97229-142">Мы сохраните его и присвойте ему имя «RSVP».</span><span class="sxs-lookup"><span data-stu-id="97229-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="97229-143">Настройка внешнего ключа связи между таблицами</span><span class="sxs-lookup"><span data-stu-id="97229-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="97229-144">Теперь у нас есть две таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="97229-144">We now have two tables within our database.</span></span> <span data-ttu-id="97229-145">Наш последний шаг конструктора схемы будет настроить связь «один ко многим» между этими двумя таблицами — таким образом, мы можно связать каждую строку Dinner ноль или более строк RSVP, применяемых к этому.</span><span class="sxs-lookup"><span data-stu-id="97229-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="97229-146">Мы сделаем, настроив столбца «DinnerID» таблицы RSVP иметь связь внешнего ключа к столбцу «DinnerID» в таблице «Ужинов».</span><span class="sxs-lookup"><span data-stu-id="97229-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="97229-147">Чтобы сделать это, будет открыт в конструкторе таблиц таблицу RSVP, дважды щелкнув его в обозревателе серверов.</span><span class="sxs-lookup"><span data-stu-id="97229-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="97229-148">Затем мы выберем столбец «DinnerID», щелкните правой кнопкой мыши и выберите «Relationshps...»</span><span class="sxs-lookup"><span data-stu-id="97229-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…"</span></span> <span data-ttu-id="97229-149">команды контекстного меню:</span><span class="sxs-lookup"><span data-stu-id="97229-149">context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="97229-150">Появится диалоговое окно, которое можно использовать для установки связи между таблицами:</span><span class="sxs-lookup"><span data-stu-id="97229-150">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="97229-151">Мы будем нажмите кнопку «Добавить», чтобы добавить новое отношение к диалоговому окну.</span><span class="sxs-lookup"><span data-stu-id="97229-151">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="97229-152">После добавления связи мы будет разверните узел «Спецификация таблиц и столбцов» представления дерева в сетке свойств в правой части диалогового окна, а затем нажмите кнопку «...» справа от его:</span><span class="sxs-lookup"><span data-stu-id="97229-152">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="97229-153">Нажав кнопку «...» появится другое диалоговое окно, позволяющий указать, какие таблицы и столбцы, участвующие в связи, а также позволяют имя связи.</span><span class="sxs-lookup"><span data-stu-id="97229-153">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="97229-154">Мы изменить первичный ключ таблицы «Ужинов» и выберите столбец «DinnerID» в таблице ужинов как первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="97229-154">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="97229-155">Нашей таблице RSVP будет таблицы внешнего ключа и RSVP. Столбец DinnerID будет связан как внешний ключ:</span><span class="sxs-lookup"><span data-stu-id="97229-155">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="97229-156">Теперь каждая строка в таблице RSVP будет связано со строкой в таблице обед.</span><span class="sxs-lookup"><span data-stu-id="97229-156">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="97229-157">SQL Server будет поддерживать целостность для нас и предотвратить нам добавления новой строки RSVP, если он не указывает допустимую строку компании Dinner.</span><span class="sxs-lookup"><span data-stu-id="97229-157">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="97229-158">Он будет также предотвратить нам при наличии по-прежнему RSVP строки, ссылающиеся на него при удалении строки Dinner.</span><span class="sxs-lookup"><span data-stu-id="97229-158">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="97229-159">Добавление данных в нашем таблицы</span><span class="sxs-lookup"><span data-stu-id="97229-159">Adding Data to our Tables</span></span>

<span data-ttu-id="97229-160">Добавим путем добавления к нашей таблице ужинов образцами данных.</span><span class="sxs-lookup"><span data-stu-id="97229-160">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="97229-161">Можно добавить данные в таблицу, щелкнув его в обозревателе серверов и выбрав команду «Показать таблицу данных»:</span><span class="sxs-lookup"><span data-stu-id="97229-161">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="97229-162">Мы добавим несколько строк данных компании Dinner, можно использовать позже, как мы начнем реализовывать приложения:</span><span class="sxs-lookup"><span data-stu-id="97229-162">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="97229-163">Следующий шаг</span><span class="sxs-lookup"><span data-stu-id="97229-163">Next Step</span></span>

<span data-ttu-id="97229-164">Мы создано базе данных.</span><span class="sxs-lookup"><span data-stu-id="97229-164">We've finished creating our database.</span></span> <span data-ttu-id="97229-165">Теперь создадим классов модели, которые можно использовать для запроса и его обновления.</span><span class="sxs-lookup"><span data-stu-id="97229-165">Let's now create model classes that we can use to query and update it.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="97229-166">[Назад](create-a-new-aspnet-mvc-project.md)
[Вперед](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="97229-166">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
