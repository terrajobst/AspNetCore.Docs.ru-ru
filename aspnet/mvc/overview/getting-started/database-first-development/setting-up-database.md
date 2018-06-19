---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Приступая к работе с Entity Framework 6 Database First MVC 5 с помощью | Документы Microsoft
author: tfitzmac
description: С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложения, который предоставляет интерфейс для существующей базы данных. Этот учебник seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: ae60b5c808d2522c66dc17ccf7d16fefdc65d552
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879339"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a><span data-ttu-id="382d0-104">Приступая к работе с Entity Framework 6 Database First MVC 5 с помощью</span><span class="sxs-lookup"><span data-stu-id="382d0-104">Getting Started with Entity Framework 6 Database First using MVC 5</span></span>
====================
<span data-ttu-id="382d0-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="382d0-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="382d0-106">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложения, который предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="382d0-107">Этот ряд учебника показано, как автоматически создают код, который дает пользователям возможность отображения, изменения, создания и удаления данных, которые хранятся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="382d0-108">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-108">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="382d0-109">В последней части этого ряда развертывается сайта и базы данных в Azure.</span><span class="sxs-lookup"><span data-stu-id="382d0-109">In the last part of the series, you will deploy the site and database to Azure.</span></span>
> 
> <span data-ttu-id="382d0-110">Эта часть ряда фокусируется на создание базы данных и заполнении его данными.</span><span class="sxs-lookup"><span data-stu-id="382d0-110">This part of the series focuses on creating a database and populating it with data.</span></span>
> 
> <span data-ttu-id="382d0-111">Эта серия был записан с публикаций из Tom Dykstra и Рик Андерсон.</span><span class="sxs-lookup"><span data-stu-id="382d0-111">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="382d0-112">Она была улучшена на основе отзывов пользователей в разделе "Примечания".</span><span class="sxs-lookup"><span data-stu-id="382d0-112">It was improved based on feedback from users in the comments section.</span></span>


## <a name="introduction"></a><span data-ttu-id="382d0-113">Вступление</span><span class="sxs-lookup"><span data-stu-id="382d0-113">Introduction</span></span>

<span data-ttu-id="382d0-114">В этом разделе показано, как начинать с существующей базы данных и быстро создавать веб-приложения, в которой пользователи могут взаимодействовать с данными.</span><span class="sxs-lookup"><span data-stu-id="382d0-114">This topic shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="382d0-115">В нем Entity Framework 6 и MVC 5 для создания веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="382d0-115">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="382d0-116">Формирование шаблонов ASP.NET позволяет автоматически создавать код для отображения, обновления, создания и удаления данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-116">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="382d0-117">С помощью средства публикации в среде Visual Studio, можно легко развернуть сайта и базы данных в Azure.</span><span class="sxs-lookup"><span data-stu-id="382d0-117">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="382d0-118">В этом разделе рассматривается ситуация, где имеется база данных и требуется создать код для веб-приложения, основанные на полях этой базы данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-118">This topic addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="382d0-119">Такой подход называется первой базы данных разработки.</span><span class="sxs-lookup"><span data-stu-id="382d0-119">This approach is called Database First development.</span></span> <span data-ttu-id="382d0-120">Если вы еще нет существующей базы данных, вместо этого можно использовать подход, именуемый разработки Code First, которая включает в себя определения классов данных и создания базы данных из свойств класса.</span><span class="sxs-lookup"><span data-stu-id="382d0-120">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

<span data-ttu-id="382d0-121">Простейший пример шаблона разработки Code First, в разделе [Приступая к работе с ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="382d0-121">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="382d0-122">Более сложный пример, в разделе [Создание модели данных Entity Framework для приложения ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="382d0-122">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="382d0-123">Рекомендации по выбору подхода Entity Framework см. в разделе [принципы разработки с использованием Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span><span class="sxs-lookup"><span data-stu-id="382d0-123">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="382d0-124">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="382d0-124">Prerequisites</span></span>

<span data-ttu-id="382d0-125">Visual Studio 2013 или Visual Studio Express 2013 для Web</span><span class="sxs-lookup"><span data-stu-id="382d0-125">Visual Studio 2013 or Visual Studio Express 2013 for Web</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="382d0-126">Настройка базы данных</span><span class="sxs-lookup"><span data-stu-id="382d0-126">Set up the database</span></span>

<span data-ttu-id="382d0-127">Для имитации среды наличия существующей базы данных, сначала создать базу данных с предварительно заполняется данными и затем создать веб-приложения, который подключается к базе данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-127">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="382d0-128">Этот учебник был разработан с использованием LocalDB с Visual Studio 2013 или Visual Studio Express 2013 для Web.</span><span class="sxs-lookup"><span data-stu-id="382d0-128">This tutorial was developed using LocalDB with either Visual Studio 2013 or Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="382d0-129">Можно использовать существующий сервер базы данных вместо LocalDB, но в зависимости от установленной версии Visual Studio и тип базы данных, все данные средства в Visual Studio может не поддерживаться.</span><span class="sxs-lookup"><span data-stu-id="382d0-129">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="382d0-130">Если средства недоступны для базы данных, может потребоваться выполнить некоторые шаги настройки базы данных в пакет управления для базы данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-130">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="382d0-131">При наличии проблемы с инструменты для баз данных в вашей версии Visual Studio, убедитесь, что вы установили последнюю версию средства базы данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-131">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="382d0-132">Сведения об обновлении или установке инструменты для баз данных см. в разделе [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span><span class="sxs-lookup"><span data-stu-id="382d0-132">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span></span>

<span data-ttu-id="382d0-133">Запустите Visual Studio и создать **проект базы данных SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="382d0-133">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="382d0-134">Назовите проект **ContosoUniversityData**.</span><span class="sxs-lookup"><span data-stu-id="382d0-134">Name the project **ContosoUniversityData**.</span></span>

![Создание проекта базы данных](setting-up-database/_static/image1.png)

<span data-ttu-id="382d0-136">Теперь у вас есть проект пустой базы данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-136">You now have an empty database project.</span></span> <span data-ttu-id="382d0-137">Вам предстоит развертывание этой базы данных в Azure далее в этом учебнике, необходимо задать в качестве целевой платформы для проекта базы данных SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="382d0-137">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="382d0-138">Задание целевой платформы не развертывает фактически базе данных. Это лишь означает, что проект базы данных будет проверки правильности совместим с целевой платформой проекта базы данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-138">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="382d0-139">Чтобы задать целевую платформу, откройте **свойства** для проекта и выберите **базы данных SQL Microsoft Azure** для целевой платформы.</span><span class="sxs-lookup"><span data-stu-id="382d0-139">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

![Задайте для целевой платформы](setting-up-database/_static/image2.png)

<span data-ttu-id="382d0-141">Можно создавать таблицы, необходимые для этого учебника, добавив сценарии SQL, определяющие таблиц.</span><span class="sxs-lookup"><span data-stu-id="382d0-141">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="382d0-142">Щелкните правой кнопкой мыши проект и добавить новый элемент.</span><span class="sxs-lookup"><span data-stu-id="382d0-142">Right-click your project and add a new item.</span></span>

![Добавление нового элемента](setting-up-database/_static/image3.png)

<span data-ttu-id="382d0-144">Добавьте новую таблицу с именем студента.</span><span class="sxs-lookup"><span data-stu-id="382d0-144">Add a new table named Student.</span></span>

![Добавление таблицы для учащихся](setting-up-database/_static/image4.png)

<span data-ttu-id="382d0-146">В файле таблице замените следующий код, чтобы создать таблицу команды T-SQL.</span><span class="sxs-lookup"><span data-stu-id="382d0-146">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="382d0-147">Обратите внимание, что окно конструктора автоматически синхронизируется с кодом.</span><span class="sxs-lookup"><span data-stu-id="382d0-147">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="382d0-148">Можно работать с помощью кода или конструктора.</span><span class="sxs-lookup"><span data-stu-id="382d0-148">You can work with either the code or designer.</span></span>

![Показать код и Дизайн](setting-up-database/_static/image5.png)

<span data-ttu-id="382d0-150">Добавьте другую таблицу.</span><span class="sxs-lookup"><span data-stu-id="382d0-150">Add another table.</span></span> <span data-ttu-id="382d0-151">Время, назовите его курса и используйте следующие команды T-SQL.</span><span class="sxs-lookup"><span data-stu-id="382d0-151">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="382d0-152">Кроме того, повторите это действие несколько раз можно создать таблицу с именем регистрации.</span><span class="sxs-lookup"><span data-stu-id="382d0-152">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="382d0-153">Можно заполнить базу данных данными через сценарий, выполняемый после развертывания базы данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-153">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="382d0-154">Добавьте в проект скрипта после развертывания.</span><span class="sxs-lookup"><span data-stu-id="382d0-154">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="382d0-155">Можно использовать имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="382d0-155">You can use the default name.</span></span>

![Добавить скрипт, выполняемый после развертывания](setting-up-database/_static/image6.png)

<span data-ttu-id="382d0-157">Добавьте следующий код T-SQL в скрипт после развертывания.</span><span class="sxs-lookup"><span data-stu-id="382d0-157">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="382d0-158">Этот скрипт просто добавляет данные в базу данных при отсутствии сопоставления записей.</span><span class="sxs-lookup"><span data-stu-id="382d0-158">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="382d0-159">Она не перезаписать или удалить любые данные, введенные в базу данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-159">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="382d0-160">Важно отметить, что после развертывания скрипт выполняется каждый раз при развертывании проекта базы данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-160">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="382d0-161">Таким образом необходимо тщательно проанализировать требования к при записи этого скрипта.</span><span class="sxs-lookup"><span data-stu-id="382d0-161">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="382d0-162">В некоторых случаях вы можете начать с известным набором данных каждый раз при развертывании проекта.</span><span class="sxs-lookup"><span data-stu-id="382d0-162">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="382d0-163">В других случаях может потребоваться не изменять существующие данные любым способом.</span><span class="sxs-lookup"><span data-stu-id="382d0-163">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="382d0-164">На основе требований, можно указать, следует ли выполняемый после развертывания скрипт, или необходимо включить в скрипт.</span><span class="sxs-lookup"><span data-stu-id="382d0-164">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="382d0-165">Дополнительные сведения о заполнении базы данных с помощью сценария после развертывания см. в разделе [включая данных в проекте базы данных SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span><span class="sxs-lookup"><span data-stu-id="382d0-165">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="382d0-166">Теперь у вас есть 4 файлы скрипта SQL, но не реально существующих таблиц.</span><span class="sxs-lookup"><span data-stu-id="382d0-166">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="382d0-167">Все готово для развертывания проекта базы данных в localdb.</span><span class="sxs-lookup"><span data-stu-id="382d0-167">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="382d0-168">В Visual Studio нажмите кнопку "Пуск" (или F5) для построения и развертывания проекта базы данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-168">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="382d0-169">Проверьте, вкладка «Вывод», чтобы проверить успешность построения и развертывания.</span><span class="sxs-lookup"><span data-stu-id="382d0-169">Check the Output tab to verify that the build and deployment succeeded.</span></span>

![Показывать выходные данные](setting-up-database/_static/image7.png)

<span data-ttu-id="382d0-171">Для просмотра, создания новой базы данных, откройте **обозреватель объектов SQL Server** и найдите имя проекта в нужную базу данных локального сервера (в данном случае **\ProjectsV12 (localdb)**).</span><span class="sxs-lookup"><span data-stu-id="382d0-171">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV12**).</span></span>

![Показать новой базы данных](setting-up-database/_static/image8.png)

<span data-ttu-id="382d0-173">Чтобы увидеть, что таблицы заполняются данными, щелкните правой кнопкой мыши таблицу и выберите **данные представления**.</span><span class="sxs-lookup"><span data-stu-id="382d0-173">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![Показать таблицу данных](setting-up-database/_static/image9.png)

<span data-ttu-id="382d0-175">Открывается для редактирования представление табличных данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-175">An editable view of the table data is displayed.</span></span>

![Показать результаты для таблицы данных](setting-up-database/_static/image10.png)

<span data-ttu-id="382d0-177">Теперь базы данных настраивается и заполняется данными.</span><span class="sxs-lookup"><span data-stu-id="382d0-177">Your database is now set up and populated with data.</span></span> <span data-ttu-id="382d0-178">В следующем уроке будет создать веб-приложение для базы данных.</span><span class="sxs-lookup"><span data-stu-id="382d0-178">In the next tutorial, you will create a web application for the database.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="382d0-179">Вперед</span><span class="sxs-lookup"><span data-stu-id="382d0-179">Next</span></span>](creating-the-web-application.md)
