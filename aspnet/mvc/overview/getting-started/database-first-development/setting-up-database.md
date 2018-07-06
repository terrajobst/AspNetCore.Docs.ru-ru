---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Начало работы с Entity Framework 6 Database First с помощью MVC 5 | Документация Майкрософт
author: tfitzmac
description: С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных. Этот учебник seri...
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 20e590ad1b3a59f93d1ba48a2564ddc1a5cbb315
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836124"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a><span data-ttu-id="68a9b-104">Начало работы с Entity Framework 6 Database First с помощью MVC 5</span><span class="sxs-lookup"><span data-stu-id="68a9b-104">Getting Started with Entity Framework 6 Database First using MVC 5</span></span>
====================
<span data-ttu-id="68a9b-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="68a9b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="68a9b-106">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="68a9b-107">В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="68a9b-108">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-108">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="68a9b-109">В последней части серии вы развернете сайта и базы данных в Azure.</span><span class="sxs-lookup"><span data-stu-id="68a9b-109">In the last part of the series, you will deploy the site and database to Azure.</span></span>
> 
> <span data-ttu-id="68a9b-110">Части этой серии рассматривается создание базы данных и заполнение его данными.</span><span class="sxs-lookup"><span data-stu-id="68a9b-110">This part of the series focuses on creating a database and populating it with data.</span></span>
> 
> <span data-ttu-id="68a9b-111">В этой серии было написано с материалами из том Дайкстра и Рик Андерсон.</span><span class="sxs-lookup"><span data-stu-id="68a9b-111">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="68a9b-112">Она была улучшена зависимости на отзывы от пользователей в разделе "Примечания".</span><span class="sxs-lookup"><span data-stu-id="68a9b-112">It was improved based on feedback from users in the comments section.</span></span>


## <a name="introduction"></a><span data-ttu-id="68a9b-113">Вступление</span><span class="sxs-lookup"><span data-stu-id="68a9b-113">Introduction</span></span>

<span data-ttu-id="68a9b-114">В этом разделе показано, как начать с существующей базы данных и быстро создавать веб-приложения, который позволяет пользователям взаимодействовать с данными.</span><span class="sxs-lookup"><span data-stu-id="68a9b-114">This topic shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="68a9b-115">Создание веб-приложения использует Entity Framework 6 и MVC 5.</span><span class="sxs-lookup"><span data-stu-id="68a9b-115">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="68a9b-116">Компонент формирования шаблонов ASP.NET позволяет автоматически создавать код для отображения, обновления, создания и удаления данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-116">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="68a9b-117">С помощью средства публикации в Visual Studio, вы можете легко развернуть сайта и базы данных в Azure.</span><span class="sxs-lookup"><span data-stu-id="68a9b-117">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="68a9b-118">В этом разделе рассматриваются ситуации, в котором базы данных и хотите создать код для веб-приложения на основе полей этой базы данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-118">This topic addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="68a9b-119">Такой подход называется Database First разработки.</span><span class="sxs-lookup"><span data-stu-id="68a9b-119">This approach is called Database First development.</span></span> <span data-ttu-id="68a9b-120">Если у вас еще нет существующей базы данных, вместо этого можно использовать такой подход называют разработки Code First, которая включает в себя определение классов данных и создании базы данных из свойства класса.</span><span class="sxs-lookup"><span data-stu-id="68a9b-120">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

<span data-ttu-id="68a9b-121">Вводный пример шаблона разработки Code First, см. в разделе [Приступая к работе с ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="68a9b-121">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="68a9b-122">Более сложный пример, см. в разделе [Создание модели данных Entity Framework для приложения ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="68a9b-122">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="68a9b-123">Рекомендации по выбору подхода Entity Framework, см. в разделе [принципы разработки с использованием Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span><span class="sxs-lookup"><span data-stu-id="68a9b-123">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="68a9b-124">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="68a9b-124">Prerequisites</span></span>

<span data-ttu-id="68a9b-125">Visual Studio 2013 или Visual Studio Express 2013 для Web</span><span class="sxs-lookup"><span data-stu-id="68a9b-125">Visual Studio 2013 or Visual Studio Express 2013 for Web</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="68a9b-126">Настройка базы данных</span><span class="sxs-lookup"><span data-stu-id="68a9b-126">Set up the database</span></span>

<span data-ttu-id="68a9b-127">Чтобы имитировать среду размещения существующей базы данных, сначала создать базу данных с предварительно заполняется данными и затем создать веб-приложения, которое подключается к базе данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-127">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="68a9b-128">Этот учебник был разработан использование LocalDB с Visual Studio 2013 или Visual Studio Express 2013 для Web.</span><span class="sxs-lookup"><span data-stu-id="68a9b-128">This tutorial was developed using LocalDB with either Visual Studio 2013 or Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="68a9b-129">Вместо LocalDB можно использовать существующий сервер базы данных, но в зависимости от установленной версии Visual Studio и тип базы данных, все средства анализа данных в Visual Studio может не поддерживаться.</span><span class="sxs-lookup"><span data-stu-id="68a9b-129">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="68a9b-130">Если средства недоступны для базы данных, может потребоваться выполнить некоторые шаги конкретной базы данных в пакет управления для базы данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-130">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="68a9b-131">Если у вас есть проблема с инструменты для баз данных в вашей версии Visual Studio, убедитесь, что вы установили последнюю версию инструменты для баз данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-131">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="68a9b-132">Сведения об обновлении или установке инструменты для баз данных, см. в разделе [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span><span class="sxs-lookup"><span data-stu-id="68a9b-132">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span></span>

<span data-ttu-id="68a9b-133">Запустите Visual Studio и создайте **проект базы данных SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="68a9b-133">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="68a9b-134">Назовите проект **ContosoUniversityData**.</span><span class="sxs-lookup"><span data-stu-id="68a9b-134">Name the project **ContosoUniversityData**.</span></span>

![Создать проект базы данных](setting-up-database/_static/image1.png)

<span data-ttu-id="68a9b-136">Теперь у вас есть пустая база данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-136">You now have an empty database project.</span></span> <span data-ttu-id="68a9b-137">Вы развернете этой базы данных в Azure далее в этом руководстве, необходимо задать базу данных SQL Azure в качестве целевой платформы для проекта.</span><span class="sxs-lookup"><span data-stu-id="68a9b-137">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="68a9b-138">Задание целевой платформы не развертывает фактически базы данных; Это только означает, что проект базы данных будет убедитесь, что структуры базы данных совместим с целевой платформой.</span><span class="sxs-lookup"><span data-stu-id="68a9b-138">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="68a9b-139">Чтобы задать целевую платформу, откройте **свойства** для проекта и выберите **базы данных SQL Microsoft Azure** для целевой платформы.</span><span class="sxs-lookup"><span data-stu-id="68a9b-139">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

![набор целевой платформы](setting-up-database/_static/image2.png)

<span data-ttu-id="68a9b-141">Можно создавать таблицы, необходимых для этого руководства путем добавления скриптов SQL, которые определяют таблицы.</span><span class="sxs-lookup"><span data-stu-id="68a9b-141">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="68a9b-142">Щелкните правой кнопкой мыши проект и добавить новый элемент.</span><span class="sxs-lookup"><span data-stu-id="68a9b-142">Right-click your project and add a new item.</span></span>

![Добавление нового элемента](setting-up-database/_static/image3.png)

<span data-ttu-id="68a9b-144">Добавьте новую таблицу с именем Student.</span><span class="sxs-lookup"><span data-stu-id="68a9b-144">Add a new table named Student.</span></span>

![Добавьте таблицу student](setting-up-database/_static/image4.png)

<span data-ttu-id="68a9b-146">В файле таблицы замените следующий код, чтобы создать таблицу команду T-SQL.</span><span class="sxs-lookup"><span data-stu-id="68a9b-146">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="68a9b-147">Обратите внимание, что окне конструктора автоматически синхронизирует с кодом.</span><span class="sxs-lookup"><span data-stu-id="68a9b-147">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="68a9b-148">Вы можете работать с код или конструктор.</span><span class="sxs-lookup"><span data-stu-id="68a9b-148">You can work with either the code or designer.</span></span>

![Показать код и Дизайн](setting-up-database/_static/image5.png)

<span data-ttu-id="68a9b-150">Добавьте другой таблицы.</span><span class="sxs-lookup"><span data-stu-id="68a9b-150">Add another table.</span></span> <span data-ttu-id="68a9b-151">Время, назовите его курс и используйте следующую команду T-SQL.</span><span class="sxs-lookup"><span data-stu-id="68a9b-151">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="68a9b-152">И повторить еще раз, чтобы создать таблицу с именем регистрации.</span><span class="sxs-lookup"><span data-stu-id="68a9b-152">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="68a9b-153">Можно заполнить базу данных данными через сценарий, выполняемый после развертывания базы данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-153">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="68a9b-154">Добавьте сценарий после развертывания проекта.</span><span class="sxs-lookup"><span data-stu-id="68a9b-154">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="68a9b-155">Можно использовать имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="68a9b-155">You can use the default name.</span></span>

![Добавить скрипт после развертывания](setting-up-database/_static/image6.png)

<span data-ttu-id="68a9b-157">Добавьте следующий код T-SQL в скрипт после развертывания.</span><span class="sxs-lookup"><span data-stu-id="68a9b-157">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="68a9b-158">Этот скрипт просто добавляет данные в базу данных при обнаружении нет соответствующей записи.</span><span class="sxs-lookup"><span data-stu-id="68a9b-158">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="68a9b-159">Он не перезаписать или удалить все данные, введенные в базу данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-159">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="68a9b-160">Важно отметить, что сценарий после развертывания выполняется каждый раз при развертывании проекта базы данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-160">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="68a9b-161">Таким образом необходимо тщательно проанализировать требования к при записи этого скрипта.</span><span class="sxs-lookup"><span data-stu-id="68a9b-161">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="68a9b-162">В некоторых случаях вы можете начинать с набором известных данных, каждый раз при развертывании проекта.</span><span class="sxs-lookup"><span data-stu-id="68a9b-162">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="68a9b-163">В других случаях можно не изменять существующие данные любым способом.</span><span class="sxs-lookup"><span data-stu-id="68a9b-163">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="68a9b-164">На основе требований, можно решить, необходим ли сценарий после развертывания или что необходимо для включения в скрипт.</span><span class="sxs-lookup"><span data-stu-id="68a9b-164">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="68a9b-165">Дополнительные сведения о заполнении базы данных с помощью скрипта после развертывания см. в разделе [данных, включая, в проект базы данных SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span><span class="sxs-lookup"><span data-stu-id="68a9b-165">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="68a9b-166">Теперь у вас есть 4 файлов скриптов SQL, но не фактический таблицы.</span><span class="sxs-lookup"><span data-stu-id="68a9b-166">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="68a9b-167">Все готово для развертывания проекта базы данных localdb.</span><span class="sxs-lookup"><span data-stu-id="68a9b-167">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="68a9b-168">В Visual Studio щелкните кнопку "Пуск" (или F5) для создания и развертывания проекта базы данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-168">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="68a9b-169">Проверьте, вкладка «Вывод», чтобы убедиться в успешном построения и развертывания.</span><span class="sxs-lookup"><span data-stu-id="68a9b-169">Check the Output tab to verify that the build and deployment succeeded.</span></span>

![Показать выходные данные](setting-up-database/_static/image7.png)

<span data-ttu-id="68a9b-171">Для просмотра, создания новой базы данных, откройте **обозреватель объектов SQL Server** и найдите имя проекта в нужной базе данных локального сервера (в данном случае **\ProjectsV12 (localdb)**).</span><span class="sxs-lookup"><span data-stu-id="68a9b-171">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV12**).</span></span>

![Показать новой базы данных](setting-up-database/_static/image8.png)

<span data-ttu-id="68a9b-173">Чтобы увидеть, что таблицы заполняются данными, щелкните правой кнопкой мыши таблицу и выберите **данные представления**.</span><span class="sxs-lookup"><span data-stu-id="68a9b-173">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![Показать таблицу данных](setting-up-database/_static/image9.png)

<span data-ttu-id="68a9b-175">Редактируемое представление таблицы отображается.</span><span class="sxs-lookup"><span data-stu-id="68a9b-175">An editable view of the table data is displayed.</span></span>

![Показать результаты для таблицы данных](setting-up-database/_static/image10.png)

<span data-ttu-id="68a9b-177">Теперь базы данных настраивается и заполняется данными.</span><span class="sxs-lookup"><span data-stu-id="68a9b-177">Your database is now set up and populated with data.</span></span> <span data-ttu-id="68a9b-178">В следующем руководстве вы создадите веб-приложения для базы данных.</span><span class="sxs-lookup"><span data-stu-id="68a9b-178">In the next tutorial, you will create a web application for the database.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="68a9b-179">Вперед</span><span class="sxs-lookup"><span data-stu-id="68a9b-179">Next</span></span>](creating-the-web-application.md)
