---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Перенос и формирование шаблонов ASP.NET MVC 4 Entity Framework | Документы Microsoft
author: rick-anderson
description: Знакомы с методами контроллера ASP.NET MVC 4, и завершена &quot;вспомогательные методы, форм и проверки&quot; практическая работа следует иметь в виду...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 42a12ee39223a06054382dbe9b4784196a706216
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306849"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="5c83c-103">Перенос и формирование шаблонов ASP.NET MVC 4 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5c83c-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="5c83c-104">По [Web лагеря команды](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="5c83c-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="5c83c-105">Загрузите комплект учебных материалов лагеря Web</span><span class="sxs-lookup"><span data-stu-id="5c83c-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="5c83c-106">Знакомы с методами контроллера ASP.NET MVC 4, и завершена &quot;вспомогательные методы, форм и проверки&quot; практическая работа следует помнить, что многие логику для создания, обновления, перечисления и удаления любой сущности данных, он будет повторяться среди приложение.</span><span class="sxs-lookup"><span data-stu-id="5c83c-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="5c83c-107">Считается, что если модель содержит несколько классов для управления, можно скорее всего, тратят много времени, записи, POST и GET методы действий для каждой сущности операции, а также каждое из представлений.</span><span class="sxs-lookup"><span data-stu-id="5c83c-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="5c83c-108">В этой лаборатории вы узнаете, как использовать формирование шаблонов ASP.NET MVC 4 для автоматического создания базовых показателей приложения CRUD (Создание, чтение, обновление и удаление).</span><span class="sxs-lookup"><span data-stu-id="5c83c-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="5c83c-109">Начиная с простой класс модели и без единой строки кода, вы создадите контроллера, который будет содержать все операции CRUD, а также все необходимые представления.</span><span class="sxs-lookup"><span data-stu-id="5c83c-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="5c83c-110">После построения и запуска простого решения, необходимо будет создан вместе с логику MVC и представления для работы с данными базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="5c83c-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="5c83c-111">Кроме того вы узнаете, как просто можно использовать Entity Framework миграции для обновления модели на протяжении всего приложения.</span><span class="sxs-lookup"><span data-stu-id="5c83c-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="5c83c-112">Entity Framework миграции позволяют изменить базу данных, после изменения модели с простых шагов.</span><span class="sxs-lookup"><span data-stu-id="5c83c-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="5c83c-113">С помощью всех этих помните можно для создания и обслуживания веб-приложений более эффективно преимуществами с новейшими функциями ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="5c83c-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="5c83c-114">Все образцы кода и фрагменты кода включаются в Web лагеря комплект учебных материалов, доступные в [выпуски Microsoft Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="5c83c-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="5c83c-115">Проект для этой лаборатории доступен на [переноса и ASP.NET MVC 4 Entity Framework формирование шаблонов](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="5c83c-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="5c83c-116">Цели</span><span class="sxs-lookup"><span data-stu-id="5c83c-116">Objectives</span></span>

<span data-ttu-id="5c83c-117">В этой лаборатории практических вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="5c83c-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="5c83c-118">Используйте формирование шаблонов ASP.NET для операций CRUD в контроллерах.</span><span class="sxs-lookup"><span data-stu-id="5c83c-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="5c83c-119">Измените модель базы данных, с помощью миграций Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5c83c-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="5c83c-120">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5c83c-120">Prerequisites</span></span>

<span data-ttu-id="5c83c-121">Необходимо иметь следующие элементы для этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="5c83c-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="5c83c-122">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или выше (чтение [приложение A](#AppendixA) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="5c83c-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="5c83c-123">Установка</span><span class="sxs-lookup"><span data-stu-id="5c83c-123">Setup</span></span>

<span data-ttu-id="5c83c-124">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="5c83c-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="5c83c-125">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступна как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5c83c-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="5c83c-126">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="5c83c-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="5c83c-127">Если вы не знакомы с фрагментов кода Visual Studio и хотите узнать о способах их использования, можно ссылаться в приложение из этого документа &quot; [с помощью фрагментов кода в приложении б](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="5c83c-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="5c83c-128">Упражнения</span><span class="sxs-lookup"><span data-stu-id="5c83c-128">Exercises</span></span>

<span data-ttu-id="5c83c-129">Следующее упражнение составляют это Практическое лабораторное занятие:</span><span class="sxs-lookup"><span data-stu-id="5c83c-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="5c83c-130">С помощью формирования шаблонов ASP.NET MVC 4 с Entity Framework миграции</span><span class="sxs-lookup"><span data-stu-id="5c83c-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="5c83c-131">В этом упражнении сопровождается **окончания** папку, содержащую полученное в результате решение, должен быть получен после завершения выполнения упражнения.</span><span class="sxs-lookup"><span data-stu-id="5c83c-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="5c83c-132">Это решение можно использовать как руководство, если вам нужна дополнительная помощь, работа в упражнении.</span><span class="sxs-lookup"><span data-stu-id="5c83c-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="5c83c-133">Предполагаемое время для выполнения этого занятия: **30 минут**</span><span class="sxs-lookup"><span data-stu-id="5c83c-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="5c83c-134">Упражнение 1: Использование формирование шаблонов ASP.NET MVC 4 с Entity Framework миграции</span><span class="sxs-lookup"><span data-stu-id="5c83c-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="5c83c-135">Формирование шаблонов ASP.NET MVC предоставляет быстрый способ создания операций CRUD стандартизованного, создание необходимую логику, которая позволяет приложению взаимодействовать с уровня базы данных.</span><span class="sxs-lookup"><span data-stu-id="5c83c-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="5c83c-136">В этом упражнении вы узнаете, как использовать формирование шаблонов ASP.NET MVC 4 с кодом, сначала создайте методы CRUD.</span><span class="sxs-lookup"><span data-stu-id="5c83c-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="5c83c-137">Затем вы узнаете, как обновлять модель внесения изменений в базе данных с помощью миграций Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5c83c-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="5c83c-138">Проект 1 — Создание нового ASP.NET MVC 4 задачи с помощью формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="5c83c-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="5c83c-139">Если это еще не открыто, запустите **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="5c83c-140">Выберите **файл | Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-140">Select **File | New Project**.</span></span> <span data-ttu-id="5c83c-141">В командлет New Project диалоговое окно, в разделе **Visual C# | Web** выберите **веб-приложение ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="5c83c-142">Имя проекта для **MVC4andEFMigrations** и укажите расположение **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** папку части этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="5c83c-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="5c83c-143">Задать **имя решения** для **начать** и обеспечить **создать каталог для решения** проверяется.</span><span class="sxs-lookup"><span data-stu-id="5c83c-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="5c83c-144">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-144">Click **OK**.</span></span>

    <span data-ttu-id="5c83c-145">![Диалоговое окно "новый проект ASP.NET MVC 4"](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "новое диалоговое окно проекта ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="5c83c-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="5c83c-146">*Диалоговое окно "новый проект ASP.NET MVC 4"*</span><span class="sxs-lookup"><span data-stu-id="5c83c-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="5c83c-147">В **нового проекта ASP.NET MVC 4** диалогового окна выберите **веб-приложение** шаблона и убедитесь, что **Razor** нажата **обработчик представлений**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="5c83c-148">Нажмите кнопку **ОК**, чтобы создать проект.</span><span class="sxs-lookup"><span data-stu-id="5c83c-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="5c83c-149">![Новый Интернет-приложения ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "новых Интернет-приложения ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="5c83c-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="5c83c-150">*Новый Интернет-приложения ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="5c83c-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="5c83c-151">В обозревателе решений щелкните правой кнопкой мыши **моделей** и выберите **добавить | Класс** создание простого класса человека (POCO).</span><span class="sxs-lookup"><span data-stu-id="5c83c-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="5c83c-152">Назовите его **лицо** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="5c83c-153">Откройте класс Person и вставьте следующие свойства.</span><span class="sxs-lookup"><span data-stu-id="5c83c-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="5c83c-154">(Фрагмент - кода *ASP.NET MVC 4 и Entity Framework миграции - свойства пользователя сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="5c83c-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="5c83c-155">Нажмите кнопку **сборки | Построение решения** для сохранения изменений и постройте проект.</span><span class="sxs-lookup"><span data-stu-id="5c83c-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="5c83c-156">![Сборка приложения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "построение приложения")</span><span class="sxs-lookup"><span data-stu-id="5c83c-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="5c83c-157">*Сборка приложения*</span><span class="sxs-lookup"><span data-stu-id="5c83c-157">*Building the Application*</span></span>
7. <span data-ttu-id="5c83c-158">В обозревателе решений щелкните правой кнопкой мыши папку controllers и выбрать **добавить | Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="5c83c-159">Назовите контроллер *PersonController* и завершить **параметры формирования шаблонов** со следующими значениями.</span><span class="sxs-lookup"><span data-stu-id="5c83c-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="5c83c-160">В **шаблона** раскрывающемся списке выберите **контроллер MVC с действиями чтения и записи и представлениями, использующий Entity Framework** параметр.</span><span class="sxs-lookup"><span data-stu-id="5c83c-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="5c83c-161">В **класс модели** раскрывающемся списке выберите **лицо** класса.</span><span class="sxs-lookup"><span data-stu-id="5c83c-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="5c83c-162">В **класс контекста данных** выберите  **&lt;новый контекст данных... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="5c83c-163">Выберите любое имя и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="5c83c-164">В **представления** раскрывающемся списке, убедитесь, что **Razor** выбран.</span><span class="sxs-lookup"><span data-stu-id="5c83c-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="5c83c-165">![Добавление пользователя контроллера с помощью формирования шаблонов](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Добавление пользователя контроллера с помощью формирования шаблонов")</span><span class="sxs-lookup"><span data-stu-id="5c83c-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="5c83c-166">*Добавление пользователя контроллера с помощью формирования шаблонов*</span><span class="sxs-lookup"><span data-stu-id="5c83c-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="5c83c-167">Нажмите кнопку **добавить** Создание нового контроллера для пользователя с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="5c83c-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="5c83c-168">Вы создали действия контроллера, а также представления.</span><span class="sxs-lookup"><span data-stu-id="5c83c-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="5c83c-169">![После создания пользователя контроллера с помощью формирования шаблонов](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "после создания пользователя контроллера с помощью формирования шаблонов")</span><span class="sxs-lookup"><span data-stu-id="5c83c-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="5c83c-170">*После создания пользователя контроллера с помощью формирования шаблонов*</span><span class="sxs-lookup"><span data-stu-id="5c83c-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="5c83c-171">Откройте **PersonController** класса.</span><span class="sxs-lookup"><span data-stu-id="5c83c-171">Open **PersonController** class.</span></span> <span data-ttu-id="5c83c-172">Обратите внимание, что полный CRUD методы действия были созданы автоматически.</span><span class="sxs-lookup"><span data-stu-id="5c83c-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="5c83c-173">![Внутри контроллера лица](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "внутри пользователя контроллера")</span><span class="sxs-lookup"><span data-stu-id="5c83c-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="5c83c-174">*Внутри контроллера Person*</span><span class="sxs-lookup"><span data-stu-id="5c83c-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="5c83c-175">Задача 2 выполняемого приложения</span><span class="sxs-lookup"><span data-stu-id="5c83c-175">Task 2- Running the application</span></span>

<span data-ttu-id="5c83c-176">На этом этапе базе данных еще не создан.</span><span class="sxs-lookup"><span data-stu-id="5c83c-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="5c83c-177">В этой задаче будет запустите приложение в первый раз и протестировать операции CRUD.</span><span class="sxs-lookup"><span data-stu-id="5c83c-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="5c83c-178">Базы данных создается с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="5c83c-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="5c83c-179">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="5c83c-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="5c83c-180">В браузере, добавьте **/Person** URL-адрес, чтобы открыть страницу пользователя.</span><span class="sxs-lookup"><span data-stu-id="5c83c-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="5c83c-181">![Первый запуск приложения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "первый запуск приложения")</span><span class="sxs-lookup"><span data-stu-id="5c83c-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="5c83c-182">*Приложение: сначала запустите*</span><span class="sxs-lookup"><span data-stu-id="5c83c-182">*Application: first run*</span></span>
3. <span data-ttu-id="5c83c-183">Вы лица страниц и протестировать операции CRUD.</span><span class="sxs-lookup"><span data-stu-id="5c83c-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="5c83c-184">Нажмите кнопку **создать новый** для добавления нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="5c83c-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="5c83c-185">Введите имя и фамилию и нажмите кнопку **создать**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="5c83c-186">![Добавление нового лица](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Добавление нового пользователя")</span><span class="sxs-lookup"><span data-stu-id="5c83c-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="5c83c-187">*Добавление нового пользователя*</span><span class="sxs-lookup"><span data-stu-id="5c83c-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="5c83c-188">В списке его можно удалить, изменить или добавить элементы.</span><span class="sxs-lookup"><span data-stu-id="5c83c-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="5c83c-189">![список людей](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "список людей")</span><span class="sxs-lookup"><span data-stu-id="5c83c-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="5c83c-190">*Список людей*</span><span class="sxs-lookup"><span data-stu-id="5c83c-190">*Person list*</span></span>
    3. <span data-ttu-id="5c83c-191">Нажмите кнопку **сведения** открыть сведения пользователя.</span><span class="sxs-lookup"><span data-stu-id="5c83c-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="5c83c-192">![Сведения о его](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "сведений пользователя")</span><span class="sxs-lookup"><span data-stu-id="5c83c-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="5c83c-193">*Подробные сведения для пользователя*</span><span class="sxs-lookup"><span data-stu-id="5c83c-193">*Person's details*</span></span>
4. <span data-ttu-id="5c83c-194">Закройте окно браузера и вернитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5c83c-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="5c83c-195">Обратите внимание, что создан всей CRUD для сущности person во всем приложении — из модели для представления - без необходимости написания единой строки кода!</span><span class="sxs-lookup"><span data-stu-id="5c83c-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="5c83c-196">Задача 3-обновления базы данных, с помощью Entity Framework миграции</span><span class="sxs-lookup"><span data-stu-id="5c83c-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="5c83c-197">В этой задаче будет обновлять базу данных, с помощью миграций Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5c83c-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="5c83c-198">Вы поймете, как просто можно изменить модель и отражение изменений в базах данных, используя функцию миграции Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5c83c-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="5c83c-199">Откройте консоль диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="5c83c-199">Open the Package Manager Console.</span></span> <span data-ttu-id="5c83c-200">Выберите **инструменты | Диспетчер пакетов библиотеки | Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="5c83c-201">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5c83c-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="5c83c-202">PMC</span><span class="sxs-lookup"><span data-stu-id="5c83c-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="5c83c-203">![Включение миграции](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "включение миграции")</span><span class="sxs-lookup"><span data-stu-id="5c83c-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="5c83c-204">*Включение миграции*</span><span class="sxs-lookup"><span data-stu-id="5c83c-204">*Enabling migrations*</span></span>

    <span data-ttu-id="5c83c-205">Включить миграцию команда создает **миграций** папке, которая содержит скрипт для инициализации базы данных.</span><span class="sxs-lookup"><span data-stu-id="5c83c-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="5c83c-206">![Миграция папку](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "папку миграции")</span><span class="sxs-lookup"><span data-stu-id="5c83c-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="5c83c-207">*Миграция папки*</span><span class="sxs-lookup"><span data-stu-id="5c83c-207">*Migrations folder*</span></span>
3. <span data-ttu-id="5c83c-208">Откройте **Configuration.cs** файл в папке миграции.</span><span class="sxs-lookup"><span data-stu-id="5c83c-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="5c83c-209">Найдите конструктор класса и измените **AutomaticMigrationsEnabled** значение *true*.</span><span class="sxs-lookup"><span data-stu-id="5c83c-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="5c83c-210">Откройте класс Person и добавьте атрибут для отчества данного лица.</span><span class="sxs-lookup"><span data-stu-id="5c83c-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="5c83c-211">Модель изменяется с помощью этого нового атрибута.</span><span class="sxs-lookup"><span data-stu-id="5c83c-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="5c83c-212">Выберите **сборки | Построение решения** меню для построения приложения.</span><span class="sxs-lookup"><span data-stu-id="5c83c-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="5c83c-213">![Сборка приложения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "построение приложения")</span><span class="sxs-lookup"><span data-stu-id="5c83c-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="5c83c-214">*Сборка приложения*</span><span class="sxs-lookup"><span data-stu-id="5c83c-214">*Building the application*</span></span>
6. <span data-ttu-id="5c83c-215">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5c83c-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="5c83c-216">PMC</span><span class="sxs-lookup"><span data-stu-id="5c83c-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="5c83c-217">Эта команда будет выглядеть изменения в объекты данных, и затем добавьте необходимые команды соответствующим образом изменить базу данных.</span><span class="sxs-lookup"><span data-stu-id="5c83c-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="5c83c-218">![Добавление отчество](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Добавление отчество")</span><span class="sxs-lookup"><span data-stu-id="5c83c-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="5c83c-219">*Добавление отчество*</span><span class="sxs-lookup"><span data-stu-id="5c83c-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="5c83c-220">(Необязательно) Можно выполнить следующую команду для создания скрипта SQL разностное обновление.</span><span class="sxs-lookup"><span data-stu-id="5c83c-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="5c83c-221">Это позволит обновить базу данных вручную (в этом случае необязательно), или применить изменения в других базах данных:</span><span class="sxs-lookup"><span data-stu-id="5c83c-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="5c83c-222">PMC</span><span class="sxs-lookup"><span data-stu-id="5c83c-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="5c83c-223">![Формирование скрипта SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "формирование скрипта SQL")</span><span class="sxs-lookup"><span data-stu-id="5c83c-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="5c83c-224">*Формирование скрипта SQL*</span><span class="sxs-lookup"><span data-stu-id="5c83c-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="5c83c-225">![Скрипт SQL update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "обновление скрипта SQL")</span><span class="sxs-lookup"><span data-stu-id="5c83c-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="5c83c-226">*Скрипт SQL update*</span><span class="sxs-lookup"><span data-stu-id="5c83c-226">*SQL Script update*</span></span>
8. <span data-ttu-id="5c83c-227">В консоли диспетчера пакетов введите следующую команду, чтобы обновить базу данных:</span><span class="sxs-lookup"><span data-stu-id="5c83c-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="5c83c-228">PMC</span><span class="sxs-lookup"><span data-stu-id="5c83c-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="5c83c-229">![Обновление базы данных](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "обновление базы данных")</span><span class="sxs-lookup"><span data-stu-id="5c83c-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="5c83c-230">*Обновление базы данных*</span><span class="sxs-lookup"><span data-stu-id="5c83c-230">*Updating the Database*</span></span>

    <span data-ttu-id="5c83c-231">При этом будет добавлено **MiddleName** столбца в **людей** таблицы для сопоставления используется текущее определение **лицо** класса.</span><span class="sxs-lookup"><span data-stu-id="5c83c-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="5c83c-232">После обновления базы данных, щелкните правой кнопкой мыши папку контроллера и выберите **добавить | Контроллер** Добавление пользователя контроллера снова (полная с теми же значениями).</span><span class="sxs-lookup"><span data-stu-id="5c83c-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="5c83c-233">Это обновляет существующие методы и представления, Добавление нового атрибута.</span><span class="sxs-lookup"><span data-stu-id="5c83c-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="5c83c-234">![Добавление, обновление контроллера](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Добавление обновление контроллера")</span><span class="sxs-lookup"><span data-stu-id="5c83c-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="5c83c-235">*Обновление контроллера*</span><span class="sxs-lookup"><span data-stu-id="5c83c-235">*Updating the controller*</span></span>
10. <span data-ttu-id="5c83c-236">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-236">Click **Add**.</span></span> <span data-ttu-id="5c83c-237">Выберите значения **перезаписать PersonController.cs** и **перезаписать связанные представления** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Добавление перезаписи контроллера](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="5c83c-239">*Обновление контроллера*</span><span class="sxs-lookup"><span data-stu-id="5c83c-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="5c83c-240">Task4 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="5c83c-240">Task4- Running the application</span></span>

1. <span data-ttu-id="5c83c-241">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="5c83c-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="5c83c-242">Откройте **/Person**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-242">Open **/Person**.</span></span> <span data-ttu-id="5c83c-243">Обратите внимание, что данные была сохранена во время отчество столбец был добавлен.</span><span class="sxs-lookup"><span data-stu-id="5c83c-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="5c83c-244">![Отчество добавлены](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "отчество добавлен")</span><span class="sxs-lookup"><span data-stu-id="5c83c-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="5c83c-245">*Отчество добавлен*</span><span class="sxs-lookup"><span data-stu-id="5c83c-245">*Middle Name added*</span></span>
3. <span data-ttu-id="5c83c-246">Если щелкнуть **изменить**, можно будет добавить второе имя текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="5c83c-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="5c83c-247">![Отчество edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "отчество выпуска")</span><span class="sxs-lookup"><span data-stu-id="5c83c-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="5c83c-248">Сводка</span><span class="sxs-lookup"><span data-stu-id="5c83c-248">Summary</span></span>

<span data-ttu-id="5c83c-249">В этой практической было изучено простых шагов для создания операций CRUD с формирование шаблонов ASP.NET MVC 4 с помощью любой класс модели.</span><span class="sxs-lookup"><span data-stu-id="5c83c-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="5c83c-250">Затем вы узнали, как для выполнения обновлений сквозного в приложении - из базы данных для просмотра - с помощью миграций Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5c83c-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="5c83c-251">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="5c83c-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="5c83c-252">Можно установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию, используя **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="5c83c-253">Следующие инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="5c83c-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="5c83c-254">Последовательно выберите пункты [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="5c83c-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="5c83c-255">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="5c83c-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="5c83c-256">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-256">Click on **Install Now**.</span></span> <span data-ttu-id="5c83c-257">Если у вас **Web Platform Installer** вы будете перенаправлены, чтобы загрузить и установить ее сначала.</span><span class="sxs-lookup"><span data-stu-id="5c83c-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="5c83c-258">Один раз **Web Platform Installer** открыто, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="5c83c-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="5c83c-259">![Установка Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="5c83c-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="5c83c-260">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="5c83c-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="5c83c-261">Чтение всех продуктов лицензий и условия и нажмите кнопку **принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="5c83c-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензионного соглашения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="5c83c-263">*Принятие условий лицензионного соглашения*</span><span class="sxs-lookup"><span data-stu-id="5c83c-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="5c83c-264">Дождитесь завершения процесса загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="5c83c-264">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="5c83c-266">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="5c83c-266">*Installation progress*</span></span>
6. <span data-ttu-id="5c83c-267">По завершении установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-267">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="5c83c-269">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="5c83c-269">*Installation completed*</span></span>
7. <span data-ttu-id="5c83c-270">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="5c83c-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="5c83c-271">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и начинается запись &quot; **VS Express**&quot;, выберите команду **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="5c83c-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="5c83c-273">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="5c83c-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="5c83c-274">Приложение б. фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="5c83c-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="5c83c-275">С помощью фрагментов кода у вас есть весь код, который требуется под рукой.</span><span class="sxs-lookup"><span data-stu-id="5c83c-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="5c83c-276">Документ лаборатории сообщает только при их использовании, как показано на следующем рисунке.</span><span class="sxs-lookup"><span data-stu-id="5c83c-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="5c83c-277">![Фрагменты кода Visual Studio для вставки кода в проекте](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "фрагменты кода с помощью Visual Studio вставьте код в проект")</span><span class="sxs-lookup"><span data-stu-id="5c83c-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="5c83c-278">*Фрагменты кода Visual Studio для вставки кода в проекте*</span><span class="sxs-lookup"><span data-stu-id="5c83c-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="5c83c-279">***Добавление фрагмента кода, с помощью клавиатуры (только C#)***</span><span class="sxs-lookup"><span data-stu-id="5c83c-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="5c83c-280">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="5c83c-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="5c83c-281">Начните вводить имя фрагмента (без пробелов и дефисы).</span><span class="sxs-lookup"><span data-stu-id="5c83c-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="5c83c-282">Смотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="5c83c-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="5c83c-283">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="5c83c-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="5c83c-284">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="5c83c-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="5c83c-285">![Начните вводить имя фрагмента](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="5c83c-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="5c83c-286">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="5c83c-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="5c83c-287">![Нажмите клавишу Tab, чтобы выделить выделенный фрагмент](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="5c83c-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="5c83c-288">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="5c83c-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="5c83c-289">![Снова нажмите клавишу Tab и фрагмент будет расширен](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "будет расширен, снова нажмите клавишу Tab и фрагмента кода")</span><span class="sxs-lookup"><span data-stu-id="5c83c-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="5c83c-290">*Снова нажмите клавишу Tab и фрагмент будет расширен*</span><span class="sxs-lookup"><span data-stu-id="5c83c-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="5c83c-291">***Добавление фрагмента кода, с помощью мыши (C#, Visual Basic и XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="5c83c-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="5c83c-292">Щелкните правой кнопкой мыши место вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="5c83c-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="5c83c-293">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="5c83c-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="5c83c-294">Выберите соответствующий фрагмент из списка, щелкнув по ней.</span><span class="sxs-lookup"><span data-stu-id="5c83c-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="5c83c-295">![Щелкните правой кнопкой мыши место для вставки фрагмента кода и выбора вставить фрагмент](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "место для вставки фрагмента кода и выбора вставить фрагмент кода щелкните правой кнопкой мыши")</span><span class="sxs-lookup"><span data-stu-id="5c83c-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="5c83c-296">*Щелкните правой кнопкой мыши в место вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="5c83c-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="5c83c-297">![Выберите из списка, соответствующего фрагмента, щелкнув его](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="5c83c-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="5c83c-298">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="5c83c-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
