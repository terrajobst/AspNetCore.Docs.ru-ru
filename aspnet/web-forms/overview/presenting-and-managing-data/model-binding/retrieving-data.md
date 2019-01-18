---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Извлечение и отображение данных с помощью модели привязки и веб-форм | Документация Майкрософт
author: Rick-Anderson
description: В этой серии руководств показано основными аспектами с помощью привязки модели с проектом веб-форм ASP.NET. Привязка модели позволяет взаимодействие с данными более прямой-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396289"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="9b30d-104">Извлечение и отображение данных с помощью привязки модели и веб-форм</span><span class="sxs-lookup"><span data-stu-id="9b30d-104">Retrieving and displaying data with model binding and web forms</span></span>
====================

> <span data-ttu-id="9b30d-105">В этой серии руководств показано основными аспектами с помощью привязки модели с проектом веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9b30d-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="9b30d-106">Привязка модели упрощает взаимодействие с данными более эффективную чем работа с данными объектов источника (например, элемент управления ObjectDataSource и SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="9b30d-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="9b30d-107">Эта серия начинается с вводный материал и перемещает до более продвинутых в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="9b30d-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
>  <span data-ttu-id="9b30d-108">Шаблон привязки модели работает с любой технологии доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="9b30d-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="9b30d-109">В этом руководстве используется платформа Entity Framework, но можно использовать технологии доступа к данным, который наиболее вам знакомы.</span><span class="sxs-lookup"><span data-stu-id="9b30d-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="9b30d-110">Из элемента управления с привязкой к данным сервера, например в элементе управления GridView, ListView, DetailsView и FormView укажите имена методов для выбора, обновления, удаления и создания данных.</span><span class="sxs-lookup"><span data-stu-id="9b30d-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="9b30d-111">В этом руководстве вы укажете значение для SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="9b30d-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="9b30d-112">В этом методе обеспечивают логику для получения данных.</span><span class="sxs-lookup"><span data-stu-id="9b30d-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="9b30d-113">В следующем учебном курсе UpdateMethod и InsertMethod и DeleteMethod установит значения.</span><span class="sxs-lookup"><span data-stu-id="9b30d-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="9b30d-114">Вы можете [загрузить](https://go.microsoft.com/fwlink/?LinkId=286116) весь проект в C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="9b30d-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="9b30d-115">Загружаемый код работает с Visual Studio 2012 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="9b30d-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="9b30d-116">Он использует шаблон Visual Studio 2012, который немного отличается от шаблона Visual Studio 2017, в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="9b30d-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="9b30d-117">В этом руководстве вы запустите приложение в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b30d-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="9b30d-118">Можно также развернуть приложение поставщика услуг размещения и сделать его доступным через Интернет.</span><span class="sxs-lookup"><span data-stu-id="9b30d-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="9b30d-119">Корпорация Майкрософт предлагает бесплатные услуг хостинга до 10 веб-сайтов в</span><span class="sxs-lookup"><span data-stu-id="9b30d-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="9b30d-120">[бесплатную пробную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="9b30d-120">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="9b30d-121">Сведения о развертывании веб-проекта Visual Studio веб-приложениях службы приложений Azure, см. в разделе [веб-развертывание ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) рядов.</span><span class="sxs-lookup"><span data-stu-id="9b30d-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="9b30d-122">Этот учебник также показано, как использовать Entity Framework Code First Migrations для развертывания базы данных SQL Server в базу данных SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="9b30d-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9b30d-123">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="9b30d-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="9b30d-124">Microsoft Visual Studio 2017 или Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="9b30d-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="9b30d-125">Этот учебник также работает с Visual Studio 2012 и Visual Studio 2013, но существуют некоторые различия в пользовательский интерфейс и проекта шаблон.</span><span class="sxs-lookup"><span data-stu-id="9b30d-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="9b30d-126">Вы создадите</span><span class="sxs-lookup"><span data-stu-id="9b30d-126">What you'll build</span></span>

<span data-ttu-id="9b30d-127">В этом руководстве вам потребуется:</span><span class="sxs-lookup"><span data-stu-id="9b30d-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="9b30d-128">Создание объектов данных, которые отражают университет с студентов, зачисленных на курсы</span><span class="sxs-lookup"><span data-stu-id="9b30d-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="9b30d-129">Построение таблиц базы данных из объектов</span><span class="sxs-lookup"><span data-stu-id="9b30d-129">Build database tables from the objects</span></span>
* <span data-ttu-id="9b30d-130">Заполнение базы данных тестовыми данными</span><span class="sxs-lookup"><span data-stu-id="9b30d-130">Populate the database with test data</span></span>
* <span data-ttu-id="9b30d-131">Отображение данных в веб-формы</span><span class="sxs-lookup"><span data-stu-id="9b30d-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="9b30d-132">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="9b30d-132">Create the project</span></span>

1. <span data-ttu-id="9b30d-133">В Visual Studio 2017 создайте **веб-приложение ASP.NET (.NET Framework)** проект с именем **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![Создание проекта](retrieving-data/_static/image19.png)

2. <span data-ttu-id="9b30d-135">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-135">Select **OK**.</span></span> <span data-ttu-id="9b30d-136">Появляется диалоговое окно, чтобы выбрать шаблон.</span><span class="sxs-lookup"><span data-stu-id="9b30d-136">The dialog box to select a template appears.</span></span>

   ![Выберите веб-форм](retrieving-data/_static/image3.png)

3. <span data-ttu-id="9b30d-138">Выберите **веб-форм** шаблона.</span><span class="sxs-lookup"><span data-stu-id="9b30d-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="9b30d-139">При необходимости задайте для аутентификации **учетные записи отдельных пользователей**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="9b30d-140">Нажмите кнопку **ОК**, чтобы создать проект.</span><span class="sxs-lookup"><span data-stu-id="9b30d-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="9b30d-141">Изменение внешнего вида узла</span><span class="sxs-lookup"><span data-stu-id="9b30d-141">Modify site appearance</span></span>

   <span data-ttu-id="9b30d-142">Внести некоторые изменения для настройки внешнего вида узла.</span><span class="sxs-lookup"><span data-stu-id="9b30d-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="9b30d-143">Откройте файл Site.Master.</span><span class="sxs-lookup"><span data-stu-id="9b30d-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="9b30d-144">Изменить заголовок, отображаемый **университета Contoso** и не **Мое приложение ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="9b30d-145">Измените текст заголовка из **имя_приложения** для **университета Contoso**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="9b30d-146">Изменение заголовка ссылок навигации для сайта соответствующие.</span><span class="sxs-lookup"><span data-stu-id="9b30d-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="9b30d-147">Удаление ссылок для **о** и **контакт** и вместо этого ссылка на **учащихся** страницы, который будет создан.</span><span class="sxs-lookup"><span data-stu-id="9b30d-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="9b30d-148">Сохраните Site.Master.</span><span class="sxs-lookup"><span data-stu-id="9b30d-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="9b30d-149">Добавьте веб-форму для отображения данных об учащихся</span><span class="sxs-lookup"><span data-stu-id="9b30d-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="9b30d-150">В **обозревателе решений**, щелкните правой кнопкой мыши проект, выберите **добавить** и затем **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="9b30d-151">В **Добавление нового элемента** выберите **веб-форма с главной страницы** шаблона и назовите его **Students.aspx**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![Создание страницы](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="9b30d-153">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="9b30d-154">Главная страница веб-формы, выберите **Site.Master**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="9b30d-155">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-155">Select **OK**.</span></span>
   

## <a name="add-the-data-model"></a><span data-ttu-id="9b30d-156">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="9b30d-156">Add the data model</span></span>

<span data-ttu-id="9b30d-157">В **моделей** папки, добавьте класс с именем **UniversityModels.cs**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="9b30d-158">Щелкните правой кнопкой мыши **моделей**выберите **добавить**, а затем **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="9b30d-159">Откроется диалоговое окно **Добавление нового элемента**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="9b30d-160">В меню навигации слева выберите **кода**, затем **класс**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![Создание класса модели](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="9b30d-162">Назовите класс **UniversityModels.cs** и выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="9b30d-163">В этом файле определения `SchoolContext`, `Student`, `Enrollment`, и `Course` классов следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9b30d-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="9b30d-164">`SchoolContext` Класс является производным от `DbContext`, который управляет подключения к базе данных и изменения в данных.</span><span class="sxs-lookup"><span data-stu-id="9b30d-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="9b30d-165">В `Student` класса, обратите внимание, что атрибуты, примененные к `FirstName`, `LastName`, и `Year` свойства.</span><span class="sxs-lookup"><span data-stu-id="9b30d-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="9b30d-166">В этом учебнике используется эти атрибуты для проверки данных.</span><span class="sxs-lookup"><span data-stu-id="9b30d-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="9b30d-167">Чтобы упростить код, только эти свойства помечаются атрибутами проверки данных.</span><span class="sxs-lookup"><span data-stu-id="9b30d-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="9b30d-168">В реальном проекте можно применить ко всем свойствам, требуя проверки атрибутов проверки.</span><span class="sxs-lookup"><span data-stu-id="9b30d-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="9b30d-169">Сохраните UniversityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="9b30d-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="9b30d-170">Настройка базы данных, основанные на классах</span><span class="sxs-lookup"><span data-stu-id="9b30d-170">Set up the database based on classes</span></span>

<span data-ttu-id="9b30d-171">В этом руководстве используется [Code First Migrations](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) для создания объектов и таблиц баз данных.</span><span class="sxs-lookup"><span data-stu-id="9b30d-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="9b30d-172">Эти таблицы хранят сведения об учащихся и их курсы.</span><span class="sxs-lookup"><span data-stu-id="9b30d-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="9b30d-173">Выберите **средства** > **диспетчер пакетов NuGet** > **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="9b30d-174">В **консоль диспетчера пакетов**, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9b30d-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="9b30d-175">Если команда выполнена успешно, появится сообщение о том, что были включены миграций.</span><span class="sxs-lookup"><span data-stu-id="9b30d-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![выполняет миграцию](retrieving-data/_static/image8.png)

      <span data-ttu-id="9b30d-177">Обратите внимание, что файл с именем *Configuration.cs* будет создана.</span><span class="sxs-lookup"><span data-stu-id="9b30d-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="9b30d-178">`Configuration` Класс имеет `Seed` метод, который можно предварительно заполнить таблицы базы данных тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="9b30d-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="9b30d-179">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="9b30d-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="9b30d-180">Откройте Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="9b30d-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="9b30d-181">Добавьте следующий код в метод `Seed` .</span><span class="sxs-lookup"><span data-stu-id="9b30d-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="9b30d-182">Кроме того, добавьте `using` инструкции для `ContosoUniversityModelBinding. Models` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="9b30d-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="9b30d-183">Сохраните Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="9b30d-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="9b30d-184">В консоли диспетчера пакетов выполните команду **добавьте миграции исходного**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="9b30d-185">Выполните команду **обновления базы данных**.</span><span class="sxs-lookup"><span data-stu-id="9b30d-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="9b30d-186">Если получено исключение при выполнении этой команды `StudentID` и `CourseID` значения могут отличаться от `Seed` значения метода.</span><span class="sxs-lookup"><span data-stu-id="9b30d-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="9b30d-187">Откройте эти таблицы базы данных и найти существующие значения для `StudentID` и `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="9b30d-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="9b30d-188">Добавьте эти значения в код для заполнения `Enrollments` таблицы.</span><span class="sxs-lookup"><span data-stu-id="9b30d-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="9b30d-189">Добавление элемента управления GridView</span><span class="sxs-lookup"><span data-stu-id="9b30d-189">Add a GridView control</span></span>

<span data-ttu-id="9b30d-190">С заполненной базы данных вы теперь все готово для получения этих данных и отображения его.</span><span class="sxs-lookup"><span data-stu-id="9b30d-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="9b30d-191">Open Students.aspx.</span><span class="sxs-lookup"><span data-stu-id="9b30d-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="9b30d-192">Найдите `MainContent` заполнителя.</span><span class="sxs-lookup"><span data-stu-id="9b30d-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="9b30d-193">В рамку, добавьте **GridView** элемент управления, который включает в себя этот код.</span><span class="sxs-lookup"><span data-stu-id="9b30d-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="9b30d-194">Сведения:</span><span class="sxs-lookup"><span data-stu-id="9b30d-194">Things to note:</span></span>
   * <span data-ttu-id="9b30d-195">Обратите внимание, что значение, заданное для `SelectMethod` свойства в элементе GridView.</span><span class="sxs-lookup"><span data-stu-id="9b30d-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="9b30d-196">Это значение указывает метод, используемый для извлечения данных GridView, который создается в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="9b30d-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="9b30d-197">`ItemType` Свойству `Student` класса, созданного ранее.</span><span class="sxs-lookup"><span data-stu-id="9b30d-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="9b30d-198">Этот параметр позволяет ссылаться на свойства класса в разметке.</span><span class="sxs-lookup"><span data-stu-id="9b30d-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="9b30d-199">Например `Student` класс имеет коллекцию с именем `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="9b30d-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="9b30d-200">Можно использовать `Item.Enrollments` для извлечения этой коллекции и затем использовать [синтаксиса LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) для получения каждого учащегося зарегистрированы кредиты sum.</span><span class="sxs-lookup"><span data-stu-id="9b30d-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="9b30d-201">Сохраните Students.aspx.</span><span class="sxs-lookup"><span data-stu-id="9b30d-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="9b30d-202">Добавьте код для получения данных</span><span class="sxs-lookup"><span data-stu-id="9b30d-202">Add code to retrieve data</span></span>

   <span data-ttu-id="9b30d-203">Добавьте в файл кода Students.aspx, метод, указанный для `SelectMethod` значение.</span><span class="sxs-lookup"><span data-stu-id="9b30d-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="9b30d-204">Откройте Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="9b30d-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="9b30d-205">Добавить `using` инструкций для `ContosoUniversityModelBinding. Models` и `System.Data.Entity` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="9b30d-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="9b30d-206">Добавьте метод, указанный для `SelectMethod`:</span><span class="sxs-lookup"><span data-stu-id="9b30d-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="9b30d-207">`Include` Предложение повышает производительность запросов, но не требуется.</span><span class="sxs-lookup"><span data-stu-id="9b30d-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="9b30d-208">Без `Include` предложения, данные извлекаются с помощью [ *отложенная загрузка*](https://en.wikipedia.org/wiki/Lazy_loading), который используется для отправки отдельный запрос к базе данных каждый раз связанные данные извлекаются.</span><span class="sxs-lookup"><span data-stu-id="9b30d-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="9b30d-209">С помощью `Include` предложения, данные извлекаются с помощью *Безотложная загрузка*, означающее запроса к одной базе данных получает все связанные данные.</span><span class="sxs-lookup"><span data-stu-id="9b30d-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="9b30d-210">Если связанные данные не используются, Безотложная загрузка является менее эффективным, так как получить дополнительные данные.</span><span class="sxs-lookup"><span data-stu-id="9b30d-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="9b30d-211">Тем не менее в этом случае Безотложная загрузка дает максимальную производительность поскольку взаимосвязанные данные отображаются для каждой записи.</span><span class="sxs-lookup"><span data-stu-id="9b30d-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="9b30d-212">Дополнительные сведения о вопросах производительности при загрузке связанных данных, см. в разделе **Lazy Eager и явная загрузка связанных данных** статьи [чтение связанных данных с помощью Entity Framework в ASP.NET Приложение MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) статьи.</span><span class="sxs-lookup"><span data-stu-id="9b30d-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="9b30d-213">По умолчанию данные сортируются по значениям свойства, помеченного как ключ.</span><span class="sxs-lookup"><span data-stu-id="9b30d-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="9b30d-214">Вы можете добавить `OrderBy` предложение, чтобы указать значение сортировки.</span><span class="sxs-lookup"><span data-stu-id="9b30d-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="9b30d-215">В этом примере значение по умолчанию `StudentID` свойство используется для сортировки.</span><span class="sxs-lookup"><span data-stu-id="9b30d-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="9b30d-216">В [сортировка, разбиение по страницам и фильтрация данных](sorting-paging-and-filtering-data.md) статьи, пользователь включено для выбора столбца для сортировки.</span><span class="sxs-lookup"><span data-stu-id="9b30d-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="9b30d-217">Сохраните Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="9b30d-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="9b30d-218">Запустите приложение</span><span class="sxs-lookup"><span data-stu-id="9b30d-218">Run your application</span></span> 

<span data-ttu-id="9b30d-219">Запустите веб-приложения (**F5**) и перейдите к **учащихся** страницы, где отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="9b30d-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![Отображение данных](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="9b30d-221">Автоматическое создание методы привязки модели</span><span class="sxs-lookup"><span data-stu-id="9b30d-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="9b30d-222">При работе с этой серии руководств, можно просто скопировать код из учебника в проект.</span><span class="sxs-lookup"><span data-stu-id="9b30d-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="9b30d-223">Однако Недостаток такого подхода является, вам может не по нескольким признакам функции, предоставляемые Visual Studio для автоматического создания кода для методы привязки модели.</span><span class="sxs-lookup"><span data-stu-id="9b30d-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="9b30d-224">При работе на ваших собственных проектов, автоматическое создание кода может сэкономить время и получить представление о том, как реализовать операцию.</span><span class="sxs-lookup"><span data-stu-id="9b30d-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="9b30d-225">В этом разделе описывается функция создания кода.</span><span class="sxs-lookup"><span data-stu-id="9b30d-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="9b30d-226">В этом разделе носит чисто информационный характер и не содержит любой код, который необходимо реализовать в своем проекте.</span><span class="sxs-lookup"><span data-stu-id="9b30d-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="9b30d-227">При установке значения для `SelectMethod`, `UpdateMethod`, `InsertMethod`, или `DeleteMethod` свойства в коде разметки, можно выбрать **создайте новый метод** параметр.</span><span class="sxs-lookup"><span data-stu-id="9b30d-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![Создание метода](retrieving-data/_static/image18.png)

<span data-ttu-id="9b30d-229">Visual Studio не только создает метод в коде программной части с правильной сигнатурой, но также создает код реализации для выполнения операции.</span><span class="sxs-lookup"><span data-stu-id="9b30d-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="9b30d-230">Если сначала установить `ItemType` возможности, свойство перед использованием автоматического создания кода, в созданном коде используется, введите для операций.</span><span class="sxs-lookup"><span data-stu-id="9b30d-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="9b30d-231">Например, при задании `UpdateMethod` автоматически создается свойство, следующий код:</span><span class="sxs-lookup"><span data-stu-id="9b30d-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="9b30d-232">Опять же этот код не должны быть добавлены в проект.</span><span class="sxs-lookup"><span data-stu-id="9b30d-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="9b30d-233">В следующем руководстве вы реализуете методы для обновления, удаления и добавления новых данных.</span><span class="sxs-lookup"><span data-stu-id="9b30d-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="9b30d-234">Сводка</span><span class="sxs-lookup"><span data-stu-id="9b30d-234">Summary</span></span>

<span data-ttu-id="9b30d-235">В этом руководстве вы создана классы модели данных и создать базу данных из этих классов.</span><span class="sxs-lookup"><span data-stu-id="9b30d-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="9b30d-236">Заполнение таблиц базы данных тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="9b30d-236">You filled the database tables with test data.</span></span> <span data-ttu-id="9b30d-237">Используемые привязки модели для получения данных из базы данных и затем отображаются данные в элементе управления GridView.</span><span class="sxs-lookup"><span data-stu-id="9b30d-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="9b30d-238">В следующем [руководстве](updating-deleting-and-creating-data.md) в этой серии, если включить обновление, удаление и создание данных.</span><span class="sxs-lookup"><span data-stu-id="9b30d-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9b30d-239">Вперед</span><span class="sxs-lookup"><span data-stu-id="9b30d-239">Next</span></span>](updating-deleting-and-creating-data.md)
