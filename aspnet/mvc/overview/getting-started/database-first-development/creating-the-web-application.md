---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: Учебник. Создание веб-приложения и моделей данных для EF Database First с ASP.NET MVC
description: Это руководство посвящено созданию веб-приложения и создание моделей данных, основанных на таблицах базы данных.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: dced55386c3f810e406c5c2b3f0071b45e3b2dbd
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236371"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="90bfe-103">Учебник. Создание веб-приложения и моделей данных для EF Database First с ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="90bfe-103">Tutorial: Create the the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="90bfe-104">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="90bfe-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="90bfe-105">В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="90bfe-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="90bfe-106">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="90bfe-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="90bfe-107">Это руководство посвящено созданию веб-приложения и создание моделей данных, основанных на таблицах базы данных.</span><span class="sxs-lookup"><span data-stu-id="90bfe-107">This tutorial focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="90bfe-108">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="90bfe-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="90bfe-109">Создание веб-приложения ASP.NET</span><span class="sxs-lookup"><span data-stu-id="90bfe-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="90bfe-110">Создание моделей</span><span class="sxs-lookup"><span data-stu-id="90bfe-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90bfe-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="90bfe-111">Prerequisites</span></span>

* [<span data-ttu-id="90bfe-112">Начало работы с Entity Framework 6 Database First с помощью MVC 5</span><span class="sxs-lookup"><span data-stu-id="90bfe-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="90bfe-113">Создание веб-приложения ASP.NET</span><span class="sxs-lookup"><span data-stu-id="90bfe-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="90bfe-114">В новое решение или в том же решении, что проект базы данных, создайте новый проект в Visual Studio и выберите **веб-приложение ASP.NET** шаблона.</span><span class="sxs-lookup"><span data-stu-id="90bfe-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="90bfe-115">Назовите проект **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="90bfe-115">Name the project **ContosoSite**.</span></span>

![Создание проекта](creating-the-web-application/_static/image1.png)

<span data-ttu-id="90bfe-117">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="90bfe-117">Click **OK**.</span></span>

<span data-ttu-id="90bfe-118">В окне "новый проект ASP.NET", выберите **MVC** шаблона.</span><span class="sxs-lookup"><span data-stu-id="90bfe-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="90bfe-119">Можно снять **разместить в облаке** параметр сейчас, так как вы развернете приложение в облако позднее.</span><span class="sxs-lookup"><span data-stu-id="90bfe-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="90bfe-120">Нажмите кнопку **ОК** для создания приложения.</span><span class="sxs-lookup"><span data-stu-id="90bfe-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="90bfe-121">Создается проект с по умолчанию файлы и папки.</span><span class="sxs-lookup"><span data-stu-id="90bfe-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="90bfe-122">В этом руководстве используется Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="90bfe-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="90bfe-123">Можно еще раз проверьте версии Entity Framework в проекте в окне «Управление пакетами NuGet».</span><span class="sxs-lookup"><span data-stu-id="90bfe-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="90bfe-124">При необходимости обновите версию Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="90bfe-124">If necessary, update your version of Entity Framework.</span></span>

![Показать версию](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="90bfe-126">Создание моделей</span><span class="sxs-lookup"><span data-stu-id="90bfe-126">Generate the models</span></span>

<span data-ttu-id="90bfe-127">Теперь вы создадите модели Entity Framework из таблиц базы данных.</span><span class="sxs-lookup"><span data-stu-id="90bfe-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="90bfe-128">Эти модели представляют собой классы, которые будут использоваться для работы с данными.</span><span class="sxs-lookup"><span data-stu-id="90bfe-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="90bfe-129">Каждая модель отражает таблицы в базе данных и содержит свойства, которые соответствуют столбцам в таблице.</span><span class="sxs-lookup"><span data-stu-id="90bfe-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="90bfe-130">Щелкните правой кнопкой мыши **моделей** и выберите **добавить** и **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="90bfe-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="90bfe-131">В окне «Добавление нового элемента» выберите **данных** в левой области и **ADO.NET Entity Data Model** из параметров в центральной области.</span><span class="sxs-lookup"><span data-stu-id="90bfe-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="90bfe-132">Назовите новый файл модели **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="90bfe-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="90bfe-133">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="90bfe-133">Click **Add**.</span></span>

<span data-ttu-id="90bfe-134">В мастере модели EDM выберите **конструктор EF из базы данных**.</span><span class="sxs-lookup"><span data-stu-id="90bfe-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="90bfe-135">Нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="90bfe-135">Click **Next**.</span></span>

<span data-ttu-id="90bfe-136">При наличии подключения базы данных, определенные в вашей среде разработки, может появиться одно из этих подключений предварительно выбраны.</span><span class="sxs-lookup"><span data-stu-id="90bfe-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="90bfe-137">Тем не менее вы хотите создать новое подключение к базе данных, созданной в первой части этого руководства.</span><span class="sxs-lookup"><span data-stu-id="90bfe-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="90bfe-138">Нажмите кнопку **новое подключение** кнопки.</span><span class="sxs-lookup"><span data-stu-id="90bfe-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="90bfe-139">В окне «Свойства подключения» введите имя локального сервера, где она была создана (в данном случае **\Projects13 (localdb)**).</span><span class="sxs-lookup"><span data-stu-id="90bfe-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\Projects13**).</span></span> <span data-ttu-id="90bfe-140">После ввода имени сервера, выберите ContosoUniversityData из доступных баз данных.</span><span class="sxs-lookup"><span data-stu-id="90bfe-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![Задание свойств соединения](creating-the-web-application/_static/image8.png)

<span data-ttu-id="90bfe-142">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="90bfe-142">Click **OK**.</span></span>

<span data-ttu-id="90bfe-143">Теперь будут показаны свойства подключения.</span><span class="sxs-lookup"><span data-stu-id="90bfe-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="90bfe-144">Можно использовать имя по умолчанию для подключения в файле Web.Config.</span><span class="sxs-lookup"><span data-stu-id="90bfe-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="90bfe-145">Нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="90bfe-145">Click **Next**.</span></span>

<span data-ttu-id="90bfe-146">Выберите последнюю версию Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="90bfe-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="90bfe-147">Нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="90bfe-147">Click **Next**.</span></span>

<span data-ttu-id="90bfe-148">Выберите **таблиц** для создания моделей для всех трех таблиц.</span><span class="sxs-lookup"><span data-stu-id="90bfe-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="90bfe-149">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="90bfe-149">Click **Finish**.</span></span>

<span data-ttu-id="90bfe-150">Если появляется предупреждение системы безопасности, выберите **ОК** для продолжения выполнения шаблона.</span><span class="sxs-lookup"><span data-stu-id="90bfe-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="90bfe-151">Модели создаются на основе таблиц базы данных и отображения диаграммы, показывающий свойства и связи между таблицами.</span><span class="sxs-lookup"><span data-stu-id="90bfe-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![Схема модели](creating-the-web-application/_static/image11.png)

<span data-ttu-id="90bfe-153">Папку Models теперь включает много новых файлов, связанные с моделями, которые были созданы из базы данных.</span><span class="sxs-lookup"><span data-stu-id="90bfe-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="90bfe-154">**ContosoModel.Context.cs** файл содержит класс, производный от **DbContext** класса, а также предоставляет свойство для каждого класса модели, который соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="90bfe-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="90bfe-155">**Course.cs**, **Enrollment.cs**, и **Student.cs** файлы содержат классы моделей, которые представляют таблицы базы данных.</span><span class="sxs-lookup"><span data-stu-id="90bfe-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="90bfe-156">Класс контекста и классы моделей будет использовать при работе с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="90bfe-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="90bfe-157">Прежде чем продолжить с этим руководством, выполните сборку проекта.</span><span class="sxs-lookup"><span data-stu-id="90bfe-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="90bfe-158">В следующем разделе вы создадите код на основе моделей данных, но этот раздел не будет работать, если проект не построен.</span><span class="sxs-lookup"><span data-stu-id="90bfe-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90bfe-159">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="90bfe-159">Next steps</span></span>

<span data-ttu-id="90bfe-160">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="90bfe-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="90bfe-161">Создали веб-приложение ASP.NET</span><span class="sxs-lookup"><span data-stu-id="90bfe-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="90bfe-162">Созданные модели</span><span class="sxs-lookup"><span data-stu-id="90bfe-162">Generated the models</span></span>

<span data-ttu-id="90bfe-163">Перейдите к следующему руководству, чтобы научиться создавать создания кода, на основе моделей данных.</span><span class="sxs-lookup"><span data-stu-id="90bfe-163">Advance to the next tutorial to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="90bfe-164">Создание представлений</span><span class="sxs-lookup"><span data-stu-id="90bfe-164">Generating views</span></span>](generating-views.md)