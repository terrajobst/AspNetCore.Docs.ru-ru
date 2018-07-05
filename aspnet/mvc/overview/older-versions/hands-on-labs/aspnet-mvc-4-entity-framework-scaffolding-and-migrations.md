---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Перенос и ASP.NET MVC 4 Entity Framework, формирование шаблонов | Документация Майкрософт
author: rick-anderson
description: Если вы знакомы с методов контроллера ASP.NET MVC 4 или завершили &quot;вспомогательные методы, формы и проверка&quot; Практическое лабораторное занятие, следует иметь в виду...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: a385ffd3f0067d4ac56d592b0f2bf151fc0f8dd4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394213"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="92f36-103">Перенос и ASP.NET MVC 4 Entity Framework, формирование шаблонов</span><span class="sxs-lookup"><span data-stu-id="92f36-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="92f36-104">по [Web Слышатся Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="92f36-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="92f36-105">Скачайте комплект учебных материалов по лагеря Web</span><span class="sxs-lookup"><span data-stu-id="92f36-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="92f36-106">Если вы знакомы с методов контроллера ASP.NET MVC 4 или завершили &quot;вспомогательные методы, формы и проверка&quot; Практическое лабораторное занятие, следует иметь в виду, многие из логики, чтобы создать, обновить, отобразить и удалить любой сущности данных, он повторяется Среди приложений.</span><span class="sxs-lookup"><span data-stu-id="92f36-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="92f36-107">Помимо того, что, если модель содержит несколько классов для управления, можно скорее всего, тратят значительное время написания методы действий POST и GET для каждой сущности операции, а также каждое из этих представлений.</span><span class="sxs-lookup"><span data-stu-id="92f36-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="92f36-108">В этом лабораторном занятии вы узнаете, как использовать формирование шаблонов ASP.NET MVC 4 для автоматического создания базового плана приложения CRUD (Create, Read, Update и Delete).</span><span class="sxs-lookup"><span data-stu-id="92f36-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="92f36-109">Начиная от простой класс модели и, не написав ни строчки кода, вы создадите контроллер, который будет содержать все операции CRUD, а также все необходимые представления.</span><span class="sxs-lookup"><span data-stu-id="92f36-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="92f36-110">После построения и запуска простого решения, вы получите базу данных приложения, созданный вместе с MVC логику и представления для манипуляций с данными.</span><span class="sxs-lookup"><span data-stu-id="92f36-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="92f36-111">Кроме того вы узнаете, насколько это просто использовать миграции Entity Framework для выполнения обновлений модели на протяжении всего приложения.</span><span class="sxs-lookup"><span data-stu-id="92f36-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="92f36-112">Миграции Entity Framework позволяют изменить базу данных, после изменения модели с помощью простых шагов.</span><span class="sxs-lookup"><span data-stu-id="92f36-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="92f36-113">С все это в виду можно для создания и поддержки веб-приложения более эффективно, используя преимущества новые возможности ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="92f36-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="92f36-114">Все примеры кода и фрагменты кода включены в Web лагеря комплект обучающих материалов, доступных из в [выпуски Microsoft Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="92f36-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="92f36-115">Проект, относящиеся к этой лаборатории доступен в [механизма формирования шаблонов ASP.NET MVC 4 Entity Framework и миграции](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="92f36-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="92f36-116">Цели</span><span class="sxs-lookup"><span data-stu-id="92f36-116">Objectives</span></span>

<span data-ttu-id="92f36-117">В этом Практическое лабораторное занятие, вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="92f36-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="92f36-118">Используйте формирование шаблонов ASP.NET для операций CRUD в контроллерах.</span><span class="sxs-lookup"><span data-stu-id="92f36-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="92f36-119">Измените модель базы данных, с помощью миграций Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="92f36-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="92f36-120">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="92f36-120">Prerequisites</span></span>

<span data-ttu-id="92f36-121">Необходимо иметь следующие элементы во укомплектовать данную лабораторию:</span><span class="sxs-lookup"><span data-stu-id="92f36-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="92f36-122">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или руководителя (чтение [приложение А](#AppendixA) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="92f36-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="92f36-123">Установка</span><span class="sxs-lookup"><span data-stu-id="92f36-123">Setup</span></span>

<span data-ttu-id="92f36-124">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="92f36-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="92f36-125">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступен как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="92f36-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="92f36-126">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="92f36-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="92f36-127">Если вы не знакомы с фрагментов кода Visual Studio и хотите узнать о способах их использования, можно ссылаться в приложение из этого документа &quot; [с помощью фрагментов кода в приложении б](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="92f36-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="92f36-128">Упражнения</span><span class="sxs-lookup"><span data-stu-id="92f36-128">Exercises</span></span>

<span data-ttu-id="92f36-129">Следующее упражнение составляющие этот практический семинар:</span><span class="sxs-lookup"><span data-stu-id="92f36-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="92f36-130">С помощью формирования шаблонов ASP.NET MVC 4 с помощью миграций Entity Framework</span><span class="sxs-lookup"><span data-stu-id="92f36-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="92f36-131">В этом упражнении сопровождается **окончания** папку, содержащую полученное решение, должен быть получен после завершения упражнения.</span><span class="sxs-lookup"><span data-stu-id="92f36-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="92f36-132">Если вам нужна дополнительная помощь, работа это упражнение, можно использовать это решение по.</span><span class="sxs-lookup"><span data-stu-id="92f36-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="92f36-133">Предполагаемое время для выполнения этого лабораторного: **30 минут**</span><span class="sxs-lookup"><span data-stu-id="92f36-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="92f36-134">Упражнение 1: С помощью формирования шаблонов ASP.NET MVC 4 с помощью миграций Entity Framework</span><span class="sxs-lookup"><span data-stu-id="92f36-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="92f36-135">Формирование шаблонов ASP.NET MVC предоставляет быстрый способ для создания операции CRUD в стандартную процедуру, создание необходимую логику, которая позволяет приложению взаимодействовать с уровня базы данных.</span><span class="sxs-lookup"><span data-stu-id="92f36-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="92f36-136">В этом упражнении вы узнаете, как использовать формирование шаблонов ASP.NET MVC 4 с кодом, сначала создайте CRUD-методы.</span><span class="sxs-lookup"><span data-stu-id="92f36-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="92f36-137">Затем вы узнаете, как обновить модель применение изменений в базе данных с помощью миграций Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="92f36-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="92f36-138">Проект 1 — Создание нового ASP.NET MVC 4 задачи с помощью формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="92f36-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="92f36-139">Если это еще не открыто, запустите **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="92f36-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="92f36-140">Выберите **файл | Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="92f36-140">Select **File | New Project**.</span></span> <span data-ttu-id="92f36-141">В New Project диалоговое окно, в разделе **Visual C# | Web** выберите **веб-приложение ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="92f36-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="92f36-142">Назовите проект для **MVC4andEFMigrations** и задайте расположение **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** папку данную лабораторию.</span><span class="sxs-lookup"><span data-stu-id="92f36-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="92f36-143">Задайте **имя решения** для **начать** , причем **создать каталог для решения** проверяется.</span><span class="sxs-lookup"><span data-stu-id="92f36-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="92f36-144">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="92f36-144">Click **OK**.</span></span>

    <span data-ttu-id="92f36-145">![Диалоговое окно "новый проект ASP.NET MVC 4"](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "диалоговое окно \"новый проект ASP.NET MVC 4\"")</span><span class="sxs-lookup"><span data-stu-id="92f36-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="92f36-146">*Диалоговое окно "новый проект ASP.NET MVC 4"*</span><span class="sxs-lookup"><span data-stu-id="92f36-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="92f36-147">В **создания проекта ASP.NET MVC 4** диалогового окна выберите **веб-приложение** шаблона и убедитесь, что **Razor** является выбранным **обработчик представлений**.</span><span class="sxs-lookup"><span data-stu-id="92f36-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="92f36-148">Нажмите кнопку **ОК**, чтобы создать проект.</span><span class="sxs-lookup"><span data-stu-id="92f36-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="92f36-149">![Новый Интернет-приложения ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "новый Интернет-приложения ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="92f36-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="92f36-150">*Новый Интернет-приложения ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="92f36-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="92f36-151">В обозревателе решений щелкните правой кнопкой мыши **моделей** и выберите **Add | Класс** для создания простой класс человека (POCO).</span><span class="sxs-lookup"><span data-stu-id="92f36-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="92f36-152">Назовите его **Person** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="92f36-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="92f36-153">Откройте класс Person и вставьте следующие свойства.</span><span class="sxs-lookup"><span data-stu-id="92f36-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="92f36-154">(Code Snippet - *ASP.NET MVC 4 и миграции Entity Framework — свойства сервера Ex1 Person*)</span><span class="sxs-lookup"><span data-stu-id="92f36-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="92f36-155">Нажмите кнопку **сборки | Создание решения** для сохранения изменений и сборки проекта.</span><span class="sxs-lookup"><span data-stu-id="92f36-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="92f36-156">![Сборка приложения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "сборка приложения")</span><span class="sxs-lookup"><span data-stu-id="92f36-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="92f36-157">*Сборка приложения*</span><span class="sxs-lookup"><span data-stu-id="92f36-157">*Building the Application*</span></span>
7. <span data-ttu-id="92f36-158">В обозревателе решений щелкните правой кнопкой мыши папку controllers и выберите **Add | Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="92f36-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="92f36-159">Назовите контроллер *PersonController* и завершите **параметры формирования шаблонов** со следующими значениями.</span><span class="sxs-lookup"><span data-stu-id="92f36-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="92f36-160">В **шаблона** стрелку раскрывающегося списка выберите **контроллер MVC с действиями чтения и записи и представлениями, использующий Entity Framework** параметр.</span><span class="sxs-lookup"><span data-stu-id="92f36-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="92f36-161">В **класс модели** стрелку раскрывающегося списка выберите **Person** класса.</span><span class="sxs-lookup"><span data-stu-id="92f36-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="92f36-162">В **класс контекста данных** выберите  **&lt;новый контекст данных... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="92f36-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="92f36-163">Выберите любое имя и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="92f36-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="92f36-164">В **представления** выберите в раскрывающемся списке, убедитесь, что **Razor** выбран.</span><span class="sxs-lookup"><span data-stu-id="92f36-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="92f36-165">![Добавление контроллера Person с помощью формирования шаблонов](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Добавление контроллера Person с помощью формирования шаблонов")</span><span class="sxs-lookup"><span data-stu-id="92f36-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="92f36-166">*Добавление контроллера Person с помощью формирования шаблонов*</span><span class="sxs-lookup"><span data-stu-id="92f36-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="92f36-167">Нажмите кнопку **добавить** для создания нового контроллера для пользователя с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="92f36-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="92f36-168">Теперь вы создали действий контроллера, а также представления.</span><span class="sxs-lookup"><span data-stu-id="92f36-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="92f36-169">![После создания с помощью формирования шаблонов контроллера Person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "после создания с помощью формирования шаблонов контроллера Person")</span><span class="sxs-lookup"><span data-stu-id="92f36-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="92f36-170">*После создания с помощью формирования шаблонов контроллера Person*</span><span class="sxs-lookup"><span data-stu-id="92f36-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="92f36-171">Откройте **PersonController** класса.</span><span class="sxs-lookup"><span data-stu-id="92f36-171">Open **PersonController** class.</span></span> <span data-ttu-id="92f36-172">Обратите внимание на то, что все CRUD-методы действия были созданы автоматически.</span><span class="sxs-lookup"><span data-stu-id="92f36-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="92f36-173">![Внутри контроллера Person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "контроллера внутри Person")</span><span class="sxs-lookup"><span data-stu-id="92f36-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="92f36-174">*Внутри контроллера Person*</span><span class="sxs-lookup"><span data-stu-id="92f36-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="92f36-175">Задача 2 выполняющиеся приложения</span><span class="sxs-lookup"><span data-stu-id="92f36-175">Task 2- Running the application</span></span>

<span data-ttu-id="92f36-176">На этом этапе база данных не еще создается.</span><span class="sxs-lookup"><span data-stu-id="92f36-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="92f36-177">В этой задаче будет запустить приложение в первый раз и проверки операций CRUD.</span><span class="sxs-lookup"><span data-stu-id="92f36-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="92f36-178">Базы данных будет создаваться в режиме реального времени с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="92f36-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="92f36-179">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="92f36-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="92f36-180">В браузере, добавьте **/Person** на URL-адрес, чтобы открыть страницу «Person».</span><span class="sxs-lookup"><span data-stu-id="92f36-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="92f36-181">![Первый запуск приложения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "первый запуск приложения")</span><span class="sxs-lookup"><span data-stu-id="92f36-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="92f36-182">*Приложение: сначала запустите*</span><span class="sxs-lookup"><span data-stu-id="92f36-182">*Application: first run*</span></span>
3. <span data-ttu-id="92f36-183">Вы человек страниц и проверки операций CRUD.</span><span class="sxs-lookup"><span data-stu-id="92f36-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="92f36-184">Нажмите кнопку **Create New** для добавления нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="92f36-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="92f36-185">Введите имя и фамилию и нажмите кнопку **создать**.</span><span class="sxs-lookup"><span data-stu-id="92f36-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="92f36-186">![Добавление нового лица](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Добавление нового пользователя")</span><span class="sxs-lookup"><span data-stu-id="92f36-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="92f36-187">*Добавление нового пользователя*</span><span class="sxs-lookup"><span data-stu-id="92f36-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="92f36-188">В списке человека можно удалить, изменить или добавить элементы.</span><span class="sxs-lookup"><span data-stu-id="92f36-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="92f36-189">![список людей](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "список людей")</span><span class="sxs-lookup"><span data-stu-id="92f36-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="92f36-190">*Список людей*</span><span class="sxs-lookup"><span data-stu-id="92f36-190">*Person list*</span></span>
    3. <span data-ttu-id="92f36-191">Нажмите кнопку **сведения** чтобы открыть сведения человека.</span><span class="sxs-lookup"><span data-stu-id="92f36-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="92f36-192">![Сведения о его](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "сведения человека")</span><span class="sxs-lookup"><span data-stu-id="92f36-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="92f36-193">*Сведения о человека*</span><span class="sxs-lookup"><span data-stu-id="92f36-193">*Person's details*</span></span>
4. <span data-ttu-id="92f36-194">Закройте браузер и вернуться в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="92f36-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="92f36-195">Обратите внимание на то, что вы создали всю CRUD для сущности person в приложении - из модели к представлениям - без необходимости писать ни одной строки кода!</span><span class="sxs-lookup"><span data-stu-id="92f36-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="92f36-196">Задача 3-обновление базы данных, с помощью миграций Entity Framework</span><span class="sxs-lookup"><span data-stu-id="92f36-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="92f36-197">В этой задаче будет обновить базу данных с помощью миграций Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="92f36-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="92f36-198">Вы поймете, насколько это просто для изменения модели и отражение изменений в базах данных с помощью функции миграции Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="92f36-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="92f36-199">Откройте консоль диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="92f36-199">Open the Package Manager Console.</span></span> <span data-ttu-id="92f36-200">Выберите **инструменты | Диспетчер пакетов библиотеки | Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="92f36-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="92f36-201">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="92f36-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="92f36-202">PMC</span><span class="sxs-lookup"><span data-stu-id="92f36-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="92f36-203">![Включение Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "включение миграции")</span><span class="sxs-lookup"><span data-stu-id="92f36-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="92f36-204">*Включение миграции*</span><span class="sxs-lookup"><span data-stu-id="92f36-204">*Enabling migrations*</span></span>

    <span data-ttu-id="92f36-205">Команда включения миграции создает **миграций** папку, которая содержит скрипт для инициализации базы данных.</span><span class="sxs-lookup"><span data-stu-id="92f36-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="92f36-206">![Папку migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "папку Migrations")</span><span class="sxs-lookup"><span data-stu-id="92f36-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="92f36-207">*Папку migrations*</span><span class="sxs-lookup"><span data-stu-id="92f36-207">*Migrations folder*</span></span>
3. <span data-ttu-id="92f36-208">Откройте **Configuration.cs** файл в папку Migrations.</span><span class="sxs-lookup"><span data-stu-id="92f36-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="92f36-209">Найдите конструктор класса и измените **AutomaticMigrationsEnabled** значение *true*.</span><span class="sxs-lookup"><span data-stu-id="92f36-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="92f36-210">Откройте класс Person и добавьте атрибут для отчества человека.</span><span class="sxs-lookup"><span data-stu-id="92f36-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="92f36-211">С помощью этого нового атрибута вы изменяете модель.</span><span class="sxs-lookup"><span data-stu-id="92f36-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="92f36-212">Выберите **сборки | Создание решения** меню для построения приложения.</span><span class="sxs-lookup"><span data-stu-id="92f36-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="92f36-213">![Сборка приложения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "сборка приложения")</span><span class="sxs-lookup"><span data-stu-id="92f36-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="92f36-214">*Сборка приложения*</span><span class="sxs-lookup"><span data-stu-id="92f36-214">*Building the application*</span></span>
6. <span data-ttu-id="92f36-215">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="92f36-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="92f36-216">PMC</span><span class="sxs-lookup"><span data-stu-id="92f36-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="92f36-217">Эта команда выполнит поиск изменений объектов данных, и затем добавьте необходимые команды соответствующим образом изменить базу данных.</span><span class="sxs-lookup"><span data-stu-id="92f36-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="92f36-218">![Добавление отчество](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Добавление отчество")</span><span class="sxs-lookup"><span data-stu-id="92f36-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="92f36-219">*Добавление отчество*</span><span class="sxs-lookup"><span data-stu-id="92f36-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="92f36-220">(Необязательно) Можно выполнить следующую команду, чтобы создать сценарий SQL разностное обновление.</span><span class="sxs-lookup"><span data-stu-id="92f36-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="92f36-221">Это позволит вам обновить базу данных вручную (в этом случае он не требуется), или применить изменения в других базах данных:</span><span class="sxs-lookup"><span data-stu-id="92f36-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="92f36-222">PMC</span><span class="sxs-lookup"><span data-stu-id="92f36-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="92f36-223">![Формирование скрипта SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "формирование скрипта SQL")</span><span class="sxs-lookup"><span data-stu-id="92f36-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="92f36-224">*Формирование скрипта SQL*</span><span class="sxs-lookup"><span data-stu-id="92f36-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="92f36-225">![Обновление скрипта SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "обновления скрипта SQL")</span><span class="sxs-lookup"><span data-stu-id="92f36-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="92f36-226">*Обновление скрипта SQL*</span><span class="sxs-lookup"><span data-stu-id="92f36-226">*SQL Script update*</span></span>
8. <span data-ttu-id="92f36-227">В консоли диспетчера пакетов введите следующую команду, чтобы обновить базу данных:</span><span class="sxs-lookup"><span data-stu-id="92f36-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="92f36-228">PMC</span><span class="sxs-lookup"><span data-stu-id="92f36-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="92f36-229">![Обновление базы данных](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "обновление базы данных")</span><span class="sxs-lookup"><span data-stu-id="92f36-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="92f36-230">*Обновление базы данных*</span><span class="sxs-lookup"><span data-stu-id="92f36-230">*Updating the Database*</span></span>

    <span data-ttu-id="92f36-231">При этом будет добавлено **MiddleName** столбца в **людей** сопоставляемой используется текущее определение таблицы **Person** класса.</span><span class="sxs-lookup"><span data-stu-id="92f36-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="92f36-232">После обновления базы данных, щелкните правой кнопкой мыши папку контроллера и выберите **Add | Контроллер** Добавление пользователя контроллера снова (полная с теми же значениями).</span><span class="sxs-lookup"><span data-stu-id="92f36-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="92f36-233">Будут обновлены существующие методы и представления, добавления нового атрибута.</span><span class="sxs-lookup"><span data-stu-id="92f36-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="92f36-234">![Добавление, обновление контроллера](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Добавление обновление контроллера")</span><span class="sxs-lookup"><span data-stu-id="92f36-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="92f36-235">*Обновление контроллера*</span><span class="sxs-lookup"><span data-stu-id="92f36-235">*Updating the controller*</span></span>
10. <span data-ttu-id="92f36-236">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="92f36-236">Click **Add**.</span></span> <span data-ttu-id="92f36-237">Выберите значения **перезаписать PersonController.cs** и **перезаписи связанного представления** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="92f36-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Добавление перезаписать контроллера](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="92f36-239">*Обновление контроллера*</span><span class="sxs-lookup"><span data-stu-id="92f36-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="92f36-240">Task4 — выполнение приложения</span><span class="sxs-lookup"><span data-stu-id="92f36-240">Task4- Running the application</span></span>

1. <span data-ttu-id="92f36-241">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="92f36-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="92f36-242">Откройте **/Person**.</span><span class="sxs-lookup"><span data-stu-id="92f36-242">Open **/Person**.</span></span> <span data-ttu-id="92f36-243">Обратите внимание на то, что данные была сохранена хотя отчество столбец был добавлен.</span><span class="sxs-lookup"><span data-stu-id="92f36-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="92f36-244">![Отчество добавлены](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "отчество добавлены")</span><span class="sxs-lookup"><span data-stu-id="92f36-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="92f36-245">*Отчество добавлены*</span><span class="sxs-lookup"><span data-stu-id="92f36-245">*Middle Name added*</span></span>
3. <span data-ttu-id="92f36-246">Если щелкнуть **изменить**, можно будет добавить второе имя текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="92f36-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="92f36-247">![Отчество edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edition отчество")</span><span class="sxs-lookup"><span data-stu-id="92f36-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="92f36-248">Сводка</span><span class="sxs-lookup"><span data-stu-id="92f36-248">Summary</span></span>

<span data-ttu-id="92f36-249">В этой практической работу вы узнали, как простые шаги для создания операции CRUD с формирование шаблонов ASP.NET MVC 4 с помощью любой класс модели.</span><span class="sxs-lookup"><span data-stu-id="92f36-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="92f36-250">Затем вы узнали, как для выполнения сквозного обновления в приложении - из базы данных к представлениям - с помощью миграций Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="92f36-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="92f36-251">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="92f36-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="92f36-252">Вы можете установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию с помощью **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="92f36-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="92f36-253">Приведенные ниже инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="92f36-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="92f36-254">Перейдите к [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="92f36-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="92f36-255">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="92f36-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="92f36-256">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="92f36-256">Click on **Install Now**.</span></span> <span data-ttu-id="92f36-257">Если у вас нет **установщика веб-платформы** вы будете перенаправлены к сначала загрузить и установить его.</span><span class="sxs-lookup"><span data-stu-id="92f36-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="92f36-258">Один раз **установщика веб-платформы** открыт, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="92f36-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="92f36-259">![Установка Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="92f36-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="92f36-260">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="92f36-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="92f36-261">Прочтите лицензии и условия все продукты и нажмите кнопку **я принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="92f36-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензии](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="92f36-263">*Принятие условий лицензии*</span><span class="sxs-lookup"><span data-stu-id="92f36-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="92f36-264">Подождите, пока не завершится процесс загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="92f36-264">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="92f36-266">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="92f36-266">*Installation progress*</span></span>
6. <span data-ttu-id="92f36-267">После завершения установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="92f36-267">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="92f36-269">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="92f36-269">*Installation completed*</span></span>
7. <span data-ttu-id="92f36-270">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="92f36-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="92f36-271">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и Займитесь написанием &quot; **VS Express**&quot;, нажмите кнопку на **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="92f36-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="92f36-273">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="92f36-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="92f36-274">Приложение б. Использование фрагментов кода</span><span class="sxs-lookup"><span data-stu-id="92f36-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="92f36-275">С помощью фрагментов кода у вас есть весь код, который требуется в вашем распоряжении.</span><span class="sxs-lookup"><span data-stu-id="92f36-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="92f36-276">Лаборатории документ поможет определить точно при их использовании, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="92f36-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="92f36-277">![Фрагменты кода Visual Studio, чтобы вставить код в проект](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "фрагменты кода с помощью Visual Studio, чтобы вставить код в проект")</span><span class="sxs-lookup"><span data-stu-id="92f36-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="92f36-278">*Фрагменты кода Visual Studio, чтобы вставить код в проект*</span><span class="sxs-lookup"><span data-stu-id="92f36-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="92f36-279">***Чтобы добавить фрагмент кода, с помощью клавиатуры (только C#)***</span><span class="sxs-lookup"><span data-stu-id="92f36-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="92f36-280">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="92f36-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="92f36-281">Начните вводить имя фрагмента (без пробелов и дефисов).</span><span class="sxs-lookup"><span data-stu-id="92f36-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="92f36-282">Посмотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="92f36-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="92f36-283">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="92f36-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="92f36-284">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="92f36-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="92f36-285">![Начните вводить имя фрагмента](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="92f36-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="92f36-286">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="92f36-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="92f36-287">![Нажмите клавишу Tab, чтобы выделить фрагмент в выделенный](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="92f36-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="92f36-288">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="92f36-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="92f36-289">![Снова нажмите клавишу Tab и фрагмент будет расширяться](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "еще раз нажмите клавишу Tab и фрагмент будет расширяться")</span><span class="sxs-lookup"><span data-stu-id="92f36-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="92f36-290">*Снова нажмите клавишу Tab и фрагмент будет расширяться*</span><span class="sxs-lookup"><span data-stu-id="92f36-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="92f36-291">***Чтобы добавить фрагмент кода, с помощью мыши (C#, Visual Basic и XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="92f36-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="92f36-292">Щелкните правой кнопкой мыши место для вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="92f36-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="92f36-293">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="92f36-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="92f36-294">Выберите соответствующий фрагмент из списка, щелкнув его.</span><span class="sxs-lookup"><span data-stu-id="92f36-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="92f36-295">![Щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент")</span><span class="sxs-lookup"><span data-stu-id="92f36-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="92f36-296">*Щелкните правой кнопкой мыши место для вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="92f36-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="92f36-297">![Выберите соответствующий фрагмент из списка, щелкнув по ней](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="92f36-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="92f36-298">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="92f36-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
