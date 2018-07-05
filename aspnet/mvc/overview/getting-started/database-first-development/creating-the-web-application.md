---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'EF Database First с ASP.NET MVC: Создание веб-приложения и модели данных | Документация Майкрософт'
author: tfitzmac
description: С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных. Этот учебник seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: a1c4e3365d320ee54d378de33a77666558a854d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398249"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a><span data-ttu-id="13676-104">EF Database First с ASP.NET MVC: Создание веб-приложения и модели данных</span><span class="sxs-lookup"><span data-stu-id="13676-104">EF Database First with ASP.NET MVC: Creating the Web Application and Data Models</span></span>
====================
<span data-ttu-id="13676-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="13676-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="13676-106">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="13676-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="13676-107">В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="13676-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="13676-108">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="13676-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="13676-109">Части этой серии рассматривается создание веб-приложения и создание моделей данных, основанных на таблицах базы данных.</span><span class="sxs-lookup"><span data-stu-id="13676-109">This part of the series focuses on creating the web application, and generating the data models based on your database tables.</span></span>


## <a name="create-a-new-aspnet-web-application"></a><span data-ttu-id="13676-110">Создание нового веб-приложения ASP.NET</span><span class="sxs-lookup"><span data-stu-id="13676-110">Create a new ASP.NET Web Application</span></span>

<span data-ttu-id="13676-111">В новое решение или в том же решении, что проект базы данных, создайте новый проект в Visual Studio и выберите **веб-приложение ASP.NET** шаблона.</span><span class="sxs-lookup"><span data-stu-id="13676-111">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="13676-112">Назовите проект **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="13676-112">Name the project **ContosoSite**.</span></span>

![Создание проекта](creating-the-web-application/_static/image1.png)

<span data-ttu-id="13676-114">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="13676-114">Click **OK**.</span></span>

<span data-ttu-id="13676-115">В окне "новый проект ASP.NET", выберите **MVC** шаблона.</span><span class="sxs-lookup"><span data-stu-id="13676-115">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="13676-116">Можно снять **разместить в облаке** параметр сейчас, так как вы развернете приложение в облако позднее.</span><span class="sxs-lookup"><span data-stu-id="13676-116">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="13676-117">Нажмите кнопку **ОК** для создания приложения.</span><span class="sxs-lookup"><span data-stu-id="13676-117">Click **OK** to create the application.</span></span>

![Выберите шаблон mvc](creating-the-web-application/_static/image2.png)

<span data-ttu-id="13676-119">Создается проект с по умолчанию файлы и папки.</span><span class="sxs-lookup"><span data-stu-id="13676-119">The project is created with the default files and folders.</span></span>

<span data-ttu-id="13676-120">В этом руководстве используется Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="13676-120">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="13676-121">Можно еще раз проверьте версии Entity Framework в проекте в окне «Управление пакетами NuGet».</span><span class="sxs-lookup"><span data-stu-id="13676-121">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="13676-122">При необходимости обновите версию Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="13676-122">If necessary, update your version of Entity Framework.</span></span>

![Показать версию](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="13676-124">Создание моделей</span><span class="sxs-lookup"><span data-stu-id="13676-124">Generate the models</span></span>

<span data-ttu-id="13676-125">Теперь вы создадите модели Entity Framework из таблиц базы данных.</span><span class="sxs-lookup"><span data-stu-id="13676-125">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="13676-126">Эти модели представляют собой классы, которые будут использоваться для работы с данными.</span><span class="sxs-lookup"><span data-stu-id="13676-126">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="13676-127">Каждая модель отражает таблицы в базе данных и содержит свойства, которые соответствуют столбцам в таблице.</span><span class="sxs-lookup"><span data-stu-id="13676-127">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="13676-128">Щелкните правой кнопкой мыши **моделей** и выберите **добавить** и **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="13676-128">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

![Добавление нового элемента](creating-the-web-application/_static/image4.png)

<span data-ttu-id="13676-130">В окне «Добавление нового элемента» выберите **данных** в левой области и **ADO.NET Entity Data Model** из параметров в центральной области.</span><span class="sxs-lookup"><span data-stu-id="13676-130">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="13676-131">Назовите новый файл модели **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="13676-131">Name the new model file **ContosoModel**.</span></span>

![Создание модели](creating-the-web-application/_static/image5.png)

<span data-ttu-id="13676-133">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="13676-133">Click **Add**.</span></span>

<span data-ttu-id="13676-134">В мастере модели EDM выберите **конструктор EF из базы данных**.</span><span class="sxs-lookup"><span data-stu-id="13676-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

![Создать из базы данных](creating-the-web-application/_static/image6.png)

<span data-ttu-id="13676-136">Нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="13676-136">Click **Next**.</span></span>

<span data-ttu-id="13676-137">При наличии подключения базы данных, определенные в вашей среде разработки, может появиться одно из этих подключений предварительно выбраны.</span><span class="sxs-lookup"><span data-stu-id="13676-137">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="13676-138">Тем не менее вы хотите создать новое подключение к базе данных, созданной в первой части этого руководства.</span><span class="sxs-lookup"><span data-stu-id="13676-138">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="13676-139">Нажмите кнопку **новое подключение** кнопки.</span><span class="sxs-lookup"><span data-stu-id="13676-139">Click the **New Connection** button.</span></span>

![подключение к базе данных](creating-the-web-application/_static/image7.png)

<span data-ttu-id="13676-141">В окне «Свойства подключения» введите имя локального сервера, где она была создана (в данном случае **\ProjectsV12 (localdb)**).</span><span class="sxs-lookup"><span data-stu-id="13676-141">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV12**).</span></span> <span data-ttu-id="13676-142">После ввода имени сервера, выберите ContosoUniversityData из доступных баз данных.</span><span class="sxs-lookup"><span data-stu-id="13676-142">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![Задание свойств соединения](creating-the-web-application/_static/image8.png)

<span data-ttu-id="13676-144">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="13676-144">Click **OK**.</span></span>

<span data-ttu-id="13676-145">Теперь будут показаны свойства подключения.</span><span class="sxs-lookup"><span data-stu-id="13676-145">The correct connection properties are now displayed.</span></span> <span data-ttu-id="13676-146">Можно использовать имя по умолчанию для подключения в файле Web.Config</span><span class="sxs-lookup"><span data-stu-id="13676-146">You can use the default name for connection in the Web.Config file</span></span>

![параметры подключения](creating-the-web-application/_static/image9.png)

<span data-ttu-id="13676-148">Нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="13676-148">Click **Next**.</span></span>

<span data-ttu-id="13676-149">Выберите **таблиц** для создания моделей для всех трех таблиц.</span><span class="sxs-lookup"><span data-stu-id="13676-149">Select **Tables** to generate models for all three tables.</span></span>

![Выбор таблиц](creating-the-web-application/_static/image10.png)

<span data-ttu-id="13676-151">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="13676-151">Click **Finish**.</span></span>

<span data-ttu-id="13676-152">Если появляется предупреждение системы безопасности, выберите **ОК** для продолжения выполнения шаблона.</span><span class="sxs-lookup"><span data-stu-id="13676-152">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="13676-153">Модели создаются на основе таблиц базы данных и отображения диаграммы, показывающий свойства и связи между таблицами.</span><span class="sxs-lookup"><span data-stu-id="13676-153">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![Схема модели](creating-the-web-application/_static/image11.png)

<span data-ttu-id="13676-155">Папку Models теперь включает много новых файлов, связанные с моделями, которые были созданы из базы данных.</span><span class="sxs-lookup"><span data-stu-id="13676-155">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

![Показать новые файлы модели](creating-the-web-application/_static/image12.png)

<span data-ttu-id="13676-157">**ContosoModel.Context.cs** файл содержит класс, производный от **DbContext** класса, а также предоставляет свойство для каждого класса модели, который соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="13676-157">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="13676-158">**Course.cs**, **Enrollment.cs**, и **Student.cs** файлы содержат классы моделей, которые представляют таблицы базы данных.</span><span class="sxs-lookup"><span data-stu-id="13676-158">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="13676-159">Класс контекста и классы моделей будет использовать при работе с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="13676-159">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="13676-160">Прежде чем продолжить с этим руководством, выполните сборку проекта.</span><span class="sxs-lookup"><span data-stu-id="13676-160">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="13676-161">В следующем разделе вы создадите код на основе моделей данных, но этот раздел не будет работать, если проект не построен.</span><span class="sxs-lookup"><span data-stu-id="13676-161">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="13676-162">[Назад](setting-up-database.md)
> [Вперед](generating-views.md)</span><span class="sxs-lookup"><span data-stu-id="13676-162">[Previous](setting-up-database.md)
[Next](generating-views.md)</span></span>
