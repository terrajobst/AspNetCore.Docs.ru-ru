---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Модели ASP.NET MVC 4 и доступа к данным | Документы Microsoft
author: rick-anderson
description: 'Примечание: Это Практическое лабораторное занятие предполагается наличие базовых знаний о ASP.NET MVC. Если вы не использовали ASP.NET MVC, прежде чем, мы рекомендуем перейти по ASP.NET MVC 4...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 081a71ef67a6eee6c84058c30f9e15301afbed23
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="7157c-104">Модели ASP.NET MVC 4 и доступа к данным</span><span class="sxs-lookup"><span data-stu-id="7157c-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="7157c-105">По [Web лагеря команды](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="7157c-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="7157c-106">Загрузите комплект учебных материалов лагеря Web</span><span class="sxs-lookup"><span data-stu-id="7157c-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="7157c-107">Это Практическое лабораторное занятие предполагает иметь базовые знания об **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="7157c-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="7157c-108">Если вы не использовали **ASP.NET MVC** раньше, мы рекомендуем перейти **ASP.NET MVC 4 основы** Практическое лабораторное занятие.</span><span class="sxs-lookup"><span data-stu-id="7157c-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="7157c-109">Эта лаборатория рассматриваются улучшения и новые функции, описанные ранее путем применения незначительные изменения примера веб-приложения в папке исходных файлов.</span><span class="sxs-lookup"><span data-stu-id="7157c-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="7157c-110">Все образцы кода и фрагменты кода включаются в Web лагеря комплект учебных материалов, доступных в [выпуски Microsoft Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="7157c-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="7157c-111">Проект для этой лаборатории доступен на [ASP.NET MVC 4 моделей и доступа к данным](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="7157c-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="7157c-112">В **ASP.NET MVC основы** Практическое лабораторное занятие, можно передачи жестко данных на контроллерах шаблонов представлений.</span><span class="sxs-lookup"><span data-stu-id="7157c-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="7157c-113">Однако для создания реальных веб-приложения, может потребоваться использовать реальной базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="7157c-114">Это Практическое лабораторное занятие будет показано, как для использования компонента database engine для хранения и извлечения данных, необходимые для работы приложения Music Store.</span><span class="sxs-lookup"><span data-stu-id="7157c-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="7157c-115">Чтобы реализовать это решение, будет начинаться с существующей базы данных и создание модели EDM из него.</span><span class="sxs-lookup"><span data-stu-id="7157c-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="7157c-116">В этой лаборатории будет соответствовать **Database First** подход а также **Code First** подход.</span><span class="sxs-lookup"><span data-stu-id="7157c-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="7157c-117">Однако можно также использовать **Model First** подход, той же модели, используя средства создания и затем создать базу данных из него.</span><span class="sxs-lookup"><span data-stu-id="7157c-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="7157c-118">![Vs первой базы данных. Модели первый](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Сначала модели")</span><span class="sxs-lookup"><span data-stu-id="7157c-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="7157c-119">*Vs первой базы данных. Сначала модели*</span><span class="sxs-lookup"><span data-stu-id="7157c-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="7157c-120">После создания модели, будет выполнять правильную корректировки в StoreController для обеспечения хранения представлений с данными, извлеченными из базы данных, вместо использования жестко запрограммированных данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="7157c-121">Не потребуется внести изменения в Просмотр шаблонов, так как StoreController вернется же ViewModels шаблонов представлений, несмотря на то, что это время будут извлекаться данные из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="7157c-122">**Первый подход на основе кода**</span><span class="sxs-lookup"><span data-stu-id="7157c-122">**The Code First Approach**</span></span>

<span data-ttu-id="7157c-123">Code First подход позволяет определить модель из кода без создания классов, которые обычно связаны с платформой.</span><span class="sxs-lookup"><span data-stu-id="7157c-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="7157c-124">В коде во-первых, объекты модели определяются с POCO, &quot;старые объекты CLR&quot;.</span><span class="sxs-lookup"><span data-stu-id="7157c-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="7157c-125">POCO, простых простых классов, которые не поддерживают наследование и не реализуют интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="7157c-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="7157c-126">Мы будет автоматически создавать базы данных из них, или можно использовать существующую базу данных и создать сопоставление класса из кода.</span><span class="sxs-lookup"><span data-stu-id="7157c-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="7157c-127">Преимущества использования этого подхода является сохранение независимо от платформы сохраняемости (в данном случае Entity Framework), модели как классы POCO не связаны с платформой сопоставления.</span><span class="sxs-lookup"><span data-stu-id="7157c-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="7157c-128">Эта лаборатория основана на ASP.NET MVC 4 и версии образца приложения Music Store настроенное и свернут помещается только функции, показанные в этом Практическое лабораторное занятие.</span><span class="sxs-lookup"><span data-stu-id="7157c-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="7157c-129">Если вы хотите изучить весь **Music Store** учебного приложения, его можно найти в [MVC Music Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="7157c-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="7157c-130">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="7157c-130">Prerequisites</span></span>

<span data-ttu-id="7157c-131">Необходимо иметь следующие элементы для этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="7157c-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="7157c-132">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или выше (чтение [приложение A](#AppendixA) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="7157c-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="7157c-133">Установка</span><span class="sxs-lookup"><span data-stu-id="7157c-133">Setup</span></span>

<span data-ttu-id="7157c-134">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="7157c-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="7157c-135">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступна как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7157c-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="7157c-136">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="7157c-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="7157c-137">Если вы не знакомы с фрагментов кода Visual Studio и хотите узнать о способах их использования, можно ссылаться в приложение из этого документа &quot; [фрагменты кода с помощью приложение C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="7157c-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="7157c-138">Упражнения</span><span class="sxs-lookup"><span data-stu-id="7157c-138">Exercises</span></span>

<span data-ttu-id="7157c-139">Это Практическое лабораторное занятие состоит в выполнении следующих упражнений.</span><span class="sxs-lookup"><span data-stu-id="7157c-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="7157c-140">Упражнение 1: Добавление базы данных</span><span class="sxs-lookup"><span data-stu-id="7157c-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="7157c-141">Упражнение 2: Создание базы данных с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="7157c-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="7157c-142">Упражнение 3: Запросы к базе данных с параметрами</span><span class="sxs-lookup"><span data-stu-id="7157c-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="7157c-143">Каждого упражнения сопровождается **окончания** папку, содержащую полученное в результате решение, должен быть получен после завершения выполнения упражнений.</span><span class="sxs-lookup"><span data-stu-id="7157c-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="7157c-144">Это решение можно использовать как руководство, если вам нужна дополнительная помощь при выполнении упражнений.</span><span class="sxs-lookup"><span data-stu-id="7157c-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="7157c-145">Предполагаемое время для выполнения этого занятия: **35 минут**.</span><span class="sxs-lookup"><span data-stu-id="7157c-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="7157c-146">Упражнение 1: Добавление базы данных</span><span class="sxs-lookup"><span data-stu-id="7157c-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="7157c-147">В этом упражнении вы узнаете, как добавить базу данных с таблицами MusicStore приложение в решение, чтобы использовать его данные.</span><span class="sxs-lookup"><span data-stu-id="7157c-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="7157c-148">После базы данных с помощью модели создается и добавляется в решение, будет изменить класс StoreController, чтобы предоставить шаблон представления с данными, извлеченными из базы данных, вместо жестко запрограммированных значений.</span><span class="sxs-lookup"><span data-stu-id="7157c-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="7157c-149">Задача 1. Добавление базы данных</span><span class="sxs-lookup"><span data-stu-id="7157c-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="7157c-150">В этой задаче вы добавите уже созданную базу данных с основной таблицы MusicStore приложения в решение.</span><span class="sxs-lookup"><span data-stu-id="7157c-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="7157c-151">Откройте **начать** решений, расположенный **источника или сервера Ex1-AddingADatabaseDBFirst/Begin/** папки.</span><span class="sxs-lookup"><span data-stu-id="7157c-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="7157c-152">Необходимо загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="7157c-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7157c-153">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7157c-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7157c-154">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="7157c-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7157c-155">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="7157c-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7157c-156">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="7157c-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7157c-157">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="7157c-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7157c-158">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="7157c-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="7157c-159">Добавить **MvcMusicStore** файла базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="7157c-160">В этой лаборатории практических будет использовать уже созданные базу данных с именем **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="7157c-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="7157c-161">Чтобы сделать это, щелкните правой кнопкой мыши **приложения\_данные** папку, выберите пункт **добавить** и нажмите кнопку **существующий элемент**.</span><span class="sxs-lookup"><span data-stu-id="7157c-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="7157c-162">Перейдите к **\Source\Assets** и выберите **MvcMusicStore.mdf** файла.</span><span class="sxs-lookup"><span data-stu-id="7157c-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="7157c-163">![Добавление существующего элемента](aspnet-mvc-4-models-and-data-access/_static/image2.png "Добавление существующего элемента")</span><span class="sxs-lookup"><span data-stu-id="7157c-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="7157c-164">*Добавление существующего элемента*</span><span class="sxs-lookup"><span data-stu-id="7157c-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="7157c-165">![Файл базы данных MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf файл базы данных")</span><span class="sxs-lookup"><span data-stu-id="7157c-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="7157c-166">*Файл базы данных MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="7157c-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="7157c-167">Базы данных будет добавлен в проект.</span><span class="sxs-lookup"><span data-stu-id="7157c-167">The database has been added to the project.</span></span> <span data-ttu-id="7157c-168">Даже в том случае, если база данных находится в решении, можно запрашивать и обновить его, как она была размещена в другой сервер базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="7157c-169">![MvcMusicStore базы данных в обозревателе решений](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore базы данных в обозревателе решений")</span><span class="sxs-lookup"><span data-stu-id="7157c-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="7157c-170">*MvcMusicStore базы данных в обозревателе решений*</span><span class="sxs-lookup"><span data-stu-id="7157c-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="7157c-171">Проверьте подключение к базе данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-171">Verify the connection to the database.</span></span> <span data-ttu-id="7157c-172">Для этого дважды щелкните **MvcMusicStore.mdf** для установления соединения.</span><span class="sxs-lookup"><span data-stu-id="7157c-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="7157c-173">![Подключение к MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "подключение к MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="7157c-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="7157c-174">*Подключение к MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="7157c-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="7157c-175">Задача 2 - Создание модели данных</span><span class="sxs-lookup"><span data-stu-id="7157c-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="7157c-176">В этой задаче вы создадите модель данных для взаимодействия с базой данных, добавленных в предыдущей задаче.</span><span class="sxs-lookup"><span data-stu-id="7157c-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="7157c-177">Создайте модель данных, представляющий базу данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="7157c-178">Для этого в обозревателе решений щелкните правой кнопкой мыши **моделей** папку, выберите пункт **добавить** и нажмите кнопку **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="7157c-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="7157c-179">В **Добавление нового элемента** диалогового окна выберите **данные** шаблона и затем **ADO.NET Entity Data Model** элемента.</span><span class="sxs-lookup"><span data-stu-id="7157c-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="7157c-180">Изменение имени модели данных для **StoreDB.edmx** и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="7157c-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="7157c-181">![Добавление модели EDM ADO.NET StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "Добавление StoreDB модели EDM ADO.NET")</span><span class="sxs-lookup"><span data-stu-id="7157c-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="7157c-182">*Добавление StoreDB модели EDM ADO.NET*</span><span class="sxs-lookup"><span data-stu-id="7157c-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="7157c-183">**Мастер моделей EDM** будут отображаться.</span><span class="sxs-lookup"><span data-stu-id="7157c-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="7157c-184">Этот мастер поможет выполнить создание слоя модели.</span><span class="sxs-lookup"><span data-stu-id="7157c-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="7157c-185">Поскольку модель должна быть создана в зависимости от существующей базы данных recentyl добавлен; выберите **создать из базы данных** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="7157c-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="7157c-186">![Выбор содержимого модели](aspnet-mvc-4-models-and-data-access/_static/image7.png "Выбор содержимого модели")</span><span class="sxs-lookup"><span data-stu-id="7157c-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="7157c-187">*Выбор содержимого модели*</span><span class="sxs-lookup"><span data-stu-id="7157c-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="7157c-188">Поскольку при создании модели из базы данных, необходимо указать соединение для использования.</span><span class="sxs-lookup"><span data-stu-id="7157c-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="7157c-189">Нажмите кнопку **новое соединение**.</span><span class="sxs-lookup"><span data-stu-id="7157c-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="7157c-190">Выберите **файл базы данных Microsoft SQL Server** и нажмите кнопку **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="7157c-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="7157c-191">![Выбрать источник данных](aspnet-mvc-4-models-and-data-access/_static/image8.png "выбрать источник данных")</span><span class="sxs-lookup"><span data-stu-id="7157c-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="7157c-192">*Выберите источник данных*</span><span class="sxs-lookup"><span data-stu-id="7157c-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="7157c-193">Нажмите кнопку **Обзор** и выберите базу данных **MvcMusicStore.mdf** находится в **приложения\_данные** папку и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="7157c-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="7157c-194">![Свойства подключения](aspnet-mvc-4-models-and-data-access/_static/image9.png "свойства соединения")</span><span class="sxs-lookup"><span data-stu-id="7157c-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="7157c-195">*Свойства подключения*</span><span class="sxs-lookup"><span data-stu-id="7157c-195">*Connection properties*</span></span>
6. <span data-ttu-id="7157c-196">Создаваемый класс должен иметь имя, совпадающее с именем строки подключения сущности, поэтому ее имя на **MusicStoreEntities** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="7157c-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="7157c-197">![Выбор подключения к данным](aspnet-mvc-4-models-and-data-access/_static/image10.png "Выбор подключения к данным")</span><span class="sxs-lookup"><span data-stu-id="7157c-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="7157c-198">*Выбор подключения к данным*</span><span class="sxs-lookup"><span data-stu-id="7157c-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="7157c-199">Выберите объекты базы данных для использования.</span><span class="sxs-lookup"><span data-stu-id="7157c-199">Choose the database objects to use.</span></span> <span data-ttu-id="7157c-200">Модель сущность будет использовать только в таблицах базы данных, выберите **таблиц** параметр и убедитесь, что **включить столбцы внешних ключей в модель** и **единственном или множественном числе создан имена объектов** выбраны параметры.</span><span class="sxs-lookup"><span data-stu-id="7157c-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="7157c-201">Изменить пространство имен модели для **MvcMusicStore.Model** и нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="7157c-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="7157c-202">![Выбор объектов базы данных](aspnet-mvc-4-models-and-data-access/_static/image11.png "Выбор объектов базы данных")</span><span class="sxs-lookup"><span data-stu-id="7157c-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="7157c-203">*Выбор объектов базы данных*</span><span class="sxs-lookup"><span data-stu-id="7157c-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7157c-204">Если отображается диалоговое окно с предупреждением системы безопасности, нажмите кнопку **ОК** для выполнения шаблона и создания классов сущностей модели.</span><span class="sxs-lookup"><span data-stu-id="7157c-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="7157c-205">В схеме сущности для базы данных будет отображаться, пока создается отдельный класс, который сопоставляется с каждой таблицы базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="7157c-206">Например **альбомы** таблицы представляется **альбом** класса, где каждый столбец в таблице сопоставит свойству класса.</span><span class="sxs-lookup"><span data-stu-id="7157c-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="7157c-207">Это позволит вам для запроса и работать с объектами, которые представляют строки в базе данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="7157c-208">![Схема Entity](aspnet-mvc-4-models-and-data-access/_static/image12.png "схема Entity")</span><span class="sxs-lookup"><span data-stu-id="7157c-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="7157c-209">*Схема Entity*</span><span class="sxs-lookup"><span data-stu-id="7157c-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7157c-210">Шаблоны T4 (.tt), запустите код для создания классов сущностей и приведет к перезаписи существующих классов с тем же именем.</span><span class="sxs-lookup"><span data-stu-id="7157c-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="7157c-211">В этом примере классы &quot;альбом&quot;, &quot;жанр&quot; и &quot;исполнителя&quot; были переопределены в создаваемом коде.</span><span class="sxs-lookup"><span data-stu-id="7157c-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="7157c-212">Задача 3. построение приложения</span><span class="sxs-lookup"><span data-stu-id="7157c-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="7157c-213">В этой задаче производится проверка, несмотря на то, что создание модели удалили **альбом**, **жанр** и **исполнителя** классы модели успешного построения проекта с помощью новые классы модели данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="7157c-214">Постройте проект, выбрав **построения** пункта меню и затем **построения MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="7157c-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="7157c-215">![Построение проекта](aspnet-mvc-4-models-and-data-access/_static/image13.png "построение проекта")</span><span class="sxs-lookup"><span data-stu-id="7157c-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="7157c-216">*Построение проекта*</span><span class="sxs-lookup"><span data-stu-id="7157c-216">*Building the project*</span></span>
2. <span data-ttu-id="7157c-217">Сборка проекта выполняется успешно.</span><span class="sxs-lookup"><span data-stu-id="7157c-217">The project builds successfully.</span></span> <span data-ttu-id="7157c-218">Почему он по-прежнему работает?</span><span class="sxs-lookup"><span data-stu-id="7157c-218">Why does it still work?</span></span> <span data-ttu-id="7157c-219">Он работает так, как таблицы базы данных содержат поля, которые включают свойства, которые вы использовали в классах, удален **альбом** и **жанр**.</span><span class="sxs-lookup"><span data-stu-id="7157c-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="7157c-220">![Сборки успешно](aspnet-mvc-4-models-and-data-access/_static/image14.png "сборки выполнено успешно")</span><span class="sxs-lookup"><span data-stu-id="7157c-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="7157c-221">*Выполняет построение успешно выполнено*</span><span class="sxs-lookup"><span data-stu-id="7157c-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="7157c-222">Хотя формат диаграммы, конструктор отображает сущности, они являются подлинными классов C#.</span><span class="sxs-lookup"><span data-stu-id="7157c-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="7157c-223">Разверните **StoreDB.edmx** узел в обозревателе решений и затем **StoreDB.tt**, вы увидите новый создаваемых классах.</span><span class="sxs-lookup"><span data-stu-id="7157c-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="7157c-224">![Созданные файлы](aspnet-mvc-4-models-and-data-access/_static/image15.png "созданных файлов")</span><span class="sxs-lookup"><span data-stu-id="7157c-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="7157c-225">*Создаваемые файлы*</span><span class="sxs-lookup"><span data-stu-id="7157c-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="7157c-226">Задача 4 - запросы к базе данных</span><span class="sxs-lookup"><span data-stu-id="7157c-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="7157c-227">В этой задаче будет обновлена класс StoreController таким образом, чтобы вместо жестко задано в данных, отправляет запрос базы данных для получения сведений.</span><span class="sxs-lookup"><span data-stu-id="7157c-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="7157c-228">Откройте **Controllers\StoreController.cs** и добавьте следующее поле класса, содержащего экземпляр **MusicStoreEntities** класс с именем **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="7157c-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="7157c-229">(Фрагмент - кода *моделей и доступа к данным — storeDB сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="7157c-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
~~~
2. <span data-ttu-id="7157c-230">**MusicStoreEntities** класс предоставляет свойства коллекции для каждой таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="7157c-231">Обновление **Обзор** метода действия для получения жанр со всеми **альбомы**.</span><span class="sxs-lookup"><span data-stu-id="7157c-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="7157c-232">(Фрагмент - кода *моделей и доступа к данным — Обзор хранилища сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="7157c-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
~~~
3. <span data-ttu-id="7157c-233">Обновление **индекс** метода действия для получения всех жанров.</span><span class="sxs-lookup"><span data-stu-id="7157c-233">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="7157c-234">(Фрагмент - кода *моделей и доступа к данным — индекс хранилища сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="7157c-234">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
~~~
4. <span data-ttu-id="7157c-235">Обновление **индекс** метода действия для получения всех жанров и преобразовать коллекцию в список.</span><span class="sxs-lookup"><span data-stu-id="7157c-235">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="7157c-236">(Фрагмент - кода *моделей и доступа к данным — GenreMenu хранилище сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="7157c-236">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]
~~~

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="7157c-237">Задача 5 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="7157c-237">Task 5 - Running the Application</span></span>

<span data-ttu-id="7157c-238">В этой задаче будет проверять, что страницы индекса хранилища теперь отображаются жанров, хранимые в базе данных вместо тех, которые жестко закодированное значение.</span><span class="sxs-lookup"><span data-stu-id="7157c-238">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="7157c-239">Нет необходимости изменить шаблон представления, так как **StoreController** возвращает те же сущности как и раньше, несмотря на то, что это время будут извлекаться данные из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-239">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="7157c-240">Пересоберите решение и нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="7157c-240">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7157c-241">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="7157c-241">The project starts in the Home page.</span></span> <span data-ttu-id="7157c-242">Убедитесь, что в меню **жанров** больше не жестко задано в список, и данные извлекаются непосредственно из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-242">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="7157c-244">*Просмотр жанров из базы данных*</span><span class="sxs-lookup"><span data-stu-id="7157c-244">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="7157c-245">Теперь перейдите к любой жанр и убедитесь, что альбомов заполняются из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-245">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="7157c-246">![Просмотр альбомов из базы данных](aspnet-mvc-4-models-and-data-access/_static/image17.png "просмотра альбомы из базы данных")</span><span class="sxs-lookup"><span data-stu-id="7157c-246">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="7157c-247">*Просмотр альбомов из базы данных*</span><span class="sxs-lookup"><span data-stu-id="7157c-247">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="7157c-248">Упражнение 2: Создание базы данных сначала с помощью кода</span><span class="sxs-lookup"><span data-stu-id="7157c-248">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="7157c-249">В этом упражнении будет рассматривается использование Code First подход для создания базы данных с таблицами, MusicStore приложения и доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="7157c-249">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="7157c-250">После создания модели, будут изменены StoreController для предоставления Просмотр шаблона с данными, извлеченными из базы данных, вместо жестко заданных значений.</span><span class="sxs-lookup"><span data-stu-id="7157c-250">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="7157c-251">Если выполнить упражнения 1 и уже работали с базы данных первый подход, вы узнаете как получить те же результаты с другим процессом.</span><span class="sxs-lookup"><span data-stu-id="7157c-251">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="7157c-252">Чтобы облегчить ознакомление помеченные задачи, которые вместе с Упражнение 1.</span><span class="sxs-lookup"><span data-stu-id="7157c-252">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="7157c-253">Если вы не завершены упражнения 1, но для получения кода первый подход, можно запустить из этого упражнения и получить полный покрытие раздела.</span><span class="sxs-lookup"><span data-stu-id="7157c-253">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="7157c-254">Задача 1 - заполнению образца данных</span><span class="sxs-lookup"><span data-stu-id="7157c-254">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="7157c-255">В этой задаче вы заполните базу данных с образцами данных при intially создается с помощью сначала код.</span><span class="sxs-lookup"><span data-stu-id="7157c-255">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="7157c-256">Откройте **начать** решений, расположенный **источника/Ex2-CreatingADatabaseCodeFirst/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="7157c-256">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="7157c-257">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="7157c-257">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="7157c-258">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="7157c-258">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7157c-259">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7157c-259">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7157c-260">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="7157c-260">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7157c-261">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="7157c-261">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7157c-262">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="7157c-262">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7157c-263">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="7157c-263">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7157c-264">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="7157c-264">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="7157c-265">Добавить **SampleData.cs** файл **моделей** папки.</span><span class="sxs-lookup"><span data-stu-id="7157c-265">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="7157c-266">Чтобы сделать это, щелкните правой кнопкой мыши **моделей** папку, выберите пункт **добавить** и нажмите кнопку **существующий элемент**.</span><span class="sxs-lookup"><span data-stu-id="7157c-266">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="7157c-267">Перейдите к **\Source\Assets** и выберите **SampleData.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="7157c-267">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="7157c-268">![Образец данных заполнения кода](aspnet-mvc-4-models-and-data-access/_static/image18.png "образец данных заполнения кода")</span><span class="sxs-lookup"><span data-stu-id="7157c-268">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="7157c-269">*Образец данных заполнения кода*</span><span class="sxs-lookup"><span data-stu-id="7157c-269">*Sample data populate code*</span></span>
3. <span data-ttu-id="7157c-270">Откройте **Global.asax.cs** файл и добавьте следующее *с помощью* инструкции.</span><span class="sxs-lookup"><span data-stu-id="7157c-270">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="7157c-271">(Фрагмент - кода *моделей и доступа к данным — глобальный Asax директивы Ex2*)</span><span class="sxs-lookup"><span data-stu-id="7157c-271">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
~~~
4. <span data-ttu-id="7157c-272">В **приложения\_Start()** метод добавьте следующую строку для задания инициализатора базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-272">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="7157c-273">(Фрагмент - кода *моделей и доступа к данным — глобальный Asax SetInitializer Ex2*)</span><span class="sxs-lookup"><span data-stu-id="7157c-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]
~~~

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="7157c-274">Задача 2 - Настройка подключения к базе данных</span><span class="sxs-lookup"><span data-stu-id="7157c-274">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="7157c-275">Теперь, когда вы уже добавлена в свой проект базы данных, будут записаны **Web.config** строку подключения файлов.</span><span class="sxs-lookup"><span data-stu-id="7157c-275">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="7157c-276">Добавить строку подключения в **Web.config**. Чтобы сделать это, откройте **Web.config** в корневой каталог проекта и замените строку подключения с именем DefaultConnection со следующей строки в **&lt;connectionStrings&gt;** раздела:</span><span class="sxs-lookup"><span data-stu-id="7157c-276">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="7157c-277">![Расположение файла Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "расположение файла Web.config")</span><span class="sxs-lookup"><span data-stu-id="7157c-277">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="7157c-278">*расположение файла Web.config*</span><span class="sxs-lookup"><span data-stu-id="7157c-278">*Web.config file location*</span></span>


~~~
[!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="7157c-279">Задача 3. Работа с моделью</span><span class="sxs-lookup"><span data-stu-id="7157c-279">Task 3 - Working with the Model</span></span>

<span data-ttu-id="7157c-280">Теперь, когда вы уже настроили соединение с базой данных, будет связан с таблицами базы данных модели.</span><span class="sxs-lookup"><span data-stu-id="7157c-280">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="7157c-281">В этой задаче будет создан класс, который будет связан в базу данных в режиме Code First.</span><span class="sxs-lookup"><span data-stu-id="7157c-281">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="7157c-282">Помните, отсутствует класс модели POCO, который должен быть изменен.</span><span class="sxs-lookup"><span data-stu-id="7157c-282">Remember that there is an existent POCO model class that should be modified.</span></span>

   > [!NOTE]
> <span data-ttu-id="7157c-283">Если вы выполнили Упражнение 1, можно заметить, то, что этот шаг выполнялась в мастере.</span><span class="sxs-lookup"><span data-stu-id="7157c-283">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="7157c-284">Выполняя Code First, будут вручную создать классы, которые будут связаны с сущностями данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-284">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>


1. <span data-ttu-id="7157c-285">Откройте класс модели POCO **жанр** из **моделей** папку проекта и включают идентификатор.</span><span class="sxs-lookup"><span data-stu-id="7157c-285">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="7157c-286">Используйте целочисленное свойство с именем **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="7157c-286">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="7157c-287">(Фрагмент - кода *моделей и доступа к данным — первый жанр Ex2 код*)</span><span class="sxs-lookup"><span data-stu-id="7157c-287">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

> [!NOTE]
> To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.
> 
> You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
~~~
2. <span data-ttu-id="7157c-288">Теперь откройте класс модели POCO **альбом** из **моделей** папку проекта и включить внешние ключи, создавать свойства с именами **GenreId** и  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="7157c-288">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="7157c-289">Этот класс уже есть **GenreId** для первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="7157c-289">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="7157c-290">(Фрагмент - кода *моделей и доступа к данным — первый альбом Ex2 код*)</span><span class="sxs-lookup"><span data-stu-id="7157c-290">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
~~~
3. <span data-ttu-id="7157c-291">Откройте класс модели POCO **исполнителя** и включать **ArtistId** свойство.</span><span class="sxs-lookup"><span data-stu-id="7157c-291">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="7157c-292">(Фрагмент - кода *моделей и доступа к данным — первый исполнителя Ex2 код*)</span><span class="sxs-lookup"><span data-stu-id="7157c-292">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
~~~
4. <span data-ttu-id="7157c-293">Щелкните правой кнопкой мыши **моделей** папку проекта и выберите **добавить | Класс**.</span><span class="sxs-lookup"><span data-stu-id="7157c-293">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="7157c-294">Назовите файл **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="7157c-294">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="7157c-295">Нажмите кнопку **Add.**</span><span class="sxs-lookup"><span data-stu-id="7157c-295">Then, click **Add.**</span></span>

    <span data-ttu-id="7157c-296">![Добавление класса](aspnet-mvc-4-models-and-data-access/_static/image20.png "Добавление класса")</span><span class="sxs-lookup"><span data-stu-id="7157c-296">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="7157c-297">*Добавление нового элемента*</span><span class="sxs-lookup"><span data-stu-id="7157c-297">*Adding a new item*</span></span>

    <span data-ttu-id="7157c-298">![Добавление класса class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Добавление class2")</span><span class="sxs-lookup"><span data-stu-id="7157c-298">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="7157c-299">*Добавление класса*</span><span class="sxs-lookup"><span data-stu-id="7157c-299">*Adding a class*</span></span>
5. <span data-ttu-id="7157c-300">Откройте класс, который вы только что создали, **MusicStoreEntities.cs**и включите пространства имен **System.Data.Entity** и **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="7157c-300">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
~~~
6. <span data-ttu-id="7157c-301">Замените объявление класса для расширения **DbContext** класса: Объявите общую **DBSet** и Переопределите **OnModelCreating** метод.</span><span class="sxs-lookup"><span data-stu-id="7157c-301">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="7157c-302">После выполнения этого шага вы получите класс домена, которое будет связано с платформой Entity Framework модели.</span><span class="sxs-lookup"><span data-stu-id="7157c-302">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="7157c-303">Для этого замените код класса с помощью следующего:</span><span class="sxs-lookup"><span data-stu-id="7157c-303">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="7157c-304">(Фрагмент - кода *моделей и доступа к данным — первый MusicStoreEntities Ex2 код*)</span><span class="sxs-lookup"><span data-stu-id="7157c-304">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre. By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table. You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)
~~~

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="7157c-305">Задача 4 - запросы к базе данных</span><span class="sxs-lookup"><span data-stu-id="7157c-305">Task 4 - Querying the Database</span></span>

<span data-ttu-id="7157c-306">В этой задаче будет обновлена StoreController класса, чтобы вместо жестко задано в данных, он будет извлекать его из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-306">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="7157c-307">Эта задача является вместе с Упражнение 1.</span><span class="sxs-lookup"><span data-stu-id="7157c-307">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="7157c-308">Если вы выполнили Упражнение 1 можно заметить, эти шаги одинаковы в обоих подходов (базы данных сначала или сначала код).</span><span class="sxs-lookup"><span data-stu-id="7157c-308">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="7157c-309">Они отличаются в том, как данные, связанные с моделью, но доступ к сущностям данных прозрачно еще из контроллера.</span><span class="sxs-lookup"><span data-stu-id="7157c-309">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="7157c-310">Откройте **Controllers\StoreController.cs** и добавьте следующее поле класса, содержащего экземпляр **MusicStoreEntities** класс с именем **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="7157c-310">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="7157c-311">(Фрагмент - кода *моделей и доступа к данным — storeDB сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="7157c-311">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
~~~
2. <span data-ttu-id="7157c-312">**MusicStoreEntities** класс предоставляет свойства коллекции для каждой таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-312">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="7157c-313">Обновление **Обзор** метода действия для получения жанр со всеми **альбомы**.</span><span class="sxs-lookup"><span data-stu-id="7157c-313">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="7157c-314">(Фрагмент - кода *моделей и доступа к данным — Обзор хранилища Ex2*)</span><span class="sxs-lookup"><span data-stu-id="7157c-314">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
~~~
3. <span data-ttu-id="7157c-315">Обновление **индекс** метода действия для получения всех жанров.</span><span class="sxs-lookup"><span data-stu-id="7157c-315">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="7157c-316">(Фрагмент - кода *моделей и доступа к данным — индекс хранилища Ex2*)</span><span class="sxs-lookup"><span data-stu-id="7157c-316">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
~~~
4. <span data-ttu-id="7157c-317">Обновление **индекс** метода действия для получения всех жанров и преобразовать коллекцию в список.</span><span class="sxs-lookup"><span data-stu-id="7157c-317">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="7157c-318">(Фрагмент - кода *моделей и доступа к данным — Ex2 хранилища GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="7157c-318">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]
~~~

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="7157c-319">Задача 5 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="7157c-319">Task 5 - Running the Application</span></span>

<span data-ttu-id="7157c-320">В этой задаче будет проверять, что страницы индекса хранилища теперь отображаются жанров, хранимые в базе данных вместо тех, которые жестко закодированное значение.</span><span class="sxs-lookup"><span data-stu-id="7157c-320">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="7157c-321">Нет необходимости изменить шаблон представления, так как **StoreController** возвращает же **StoreIndexViewModel** как и раньше, но на этот раз будут извлекаться данные из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-321">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="7157c-322">Пересоберите решение и нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="7157c-322">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7157c-323">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="7157c-323">The project starts in the Home page.</span></span> <span data-ttu-id="7157c-324">Убедитесь, что в меню **жанров** больше не жестко задано в список, и данные извлекаются непосредственно из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-324">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="7157c-326">*Просмотр жанров из базы данных*</span><span class="sxs-lookup"><span data-stu-id="7157c-326">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="7157c-327">Теперь перейдите к любой жанр и убедитесь, что альбомов заполняются из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-327">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="7157c-328">![Просмотр альбомов из базы данных](aspnet-mvc-4-models-and-data-access/_static/image23.png "просмотра альбомы из базы данных")</span><span class="sxs-lookup"><span data-stu-id="7157c-328">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="7157c-329">*Просмотр альбомов из базы данных*</span><span class="sxs-lookup"><span data-stu-id="7157c-329">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="7157c-330">Упражнение 3: Запросы к базе данных с параметрами</span><span class="sxs-lookup"><span data-stu-id="7157c-330">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="7157c-331">В этом упражнении вы узнаете, как в базе данных, используя параметры и способы формирования результата запроса, функцию, которая уменьшает число баз данных обращается к извлечения данных более эффективным способом.</span><span class="sxs-lookup"><span data-stu-id="7157c-331">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="7157c-332">Дополнительные сведения о формировании результатов запроса, посетите следующий [статьи msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="7157c-332">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="7157c-333">Задача 1 - изменение StoreController для получения альбомы из базы данных</span><span class="sxs-lookup"><span data-stu-id="7157c-333">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="7157c-334">В этой задаче будет изменено **StoreController** класса для доступа к базе данных для получения альбомы из определенного жанра.</span><span class="sxs-lookup"><span data-stu-id="7157c-334">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="7157c-335">Откройте **начать** решений, расположенный **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** папку, если вы хотите использовать подход сначала код или **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** папку, если вы хотите использовать подход сначала база данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-335">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="7157c-336">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="7157c-336">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="7157c-337">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="7157c-337">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7157c-338">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7157c-338">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7157c-339">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="7157c-339">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7157c-340">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="7157c-340">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7157c-341">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="7157c-341">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7157c-342">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="7157c-342">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7157c-343">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="7157c-343">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="7157c-344">Откройте **StoreController** классе, чтобы изменить **Обзор** метода действия.</span><span class="sxs-lookup"><span data-stu-id="7157c-344">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="7157c-345">Для этого в **обозревателе решений**, разверните **контроллеров** папку и дважды щелкните **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="7157c-345">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="7157c-346">Изменение **Обзор** метода действия для получения альбомов для определенного жанра.</span><span class="sxs-lookup"><span data-stu-id="7157c-346">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="7157c-347">Для этого замените следующий код:</span><span class="sxs-lookup"><span data-stu-id="7157c-347">To do this, replace the following code:</span></span>

    <span data-ttu-id="7157c-348">(Фрагмент - кода *моделей и доступа к данным — Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="7157c-348">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too. You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album. The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.
> 
> You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved. This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information. In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.
> 
> The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well. This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="7157c-349">Задача 2 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="7157c-349">Task 2 - Running the Application</span></span>

<span data-ttu-id="7157c-350">В этой задаче будет запустите приложение и извлечь фотоальбомы определенного жанра из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-350">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="7157c-351">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="7157c-351">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7157c-352">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="7157c-352">The project starts in the Home page.</span></span> <span data-ttu-id="7157c-353">Изменить URL-адрес **/Store/обзора? жанр = Pop** чтобы убедиться что результаты извлекаются из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-353">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="7157c-354">![Просмотр по жанру](aspnet-mvc-4-models-and-data-access/_static/image24.png "просмотра по жанру")</span><span class="sxs-lookup"><span data-stu-id="7157c-354">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="7157c-355">*Просмотр/Store/обзора? жанр = Pop*</span><span class="sxs-lookup"><span data-stu-id="7157c-355">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="7157c-356">Задача 3. доступ к альбомам, по идентификатору</span><span class="sxs-lookup"><span data-stu-id="7157c-356">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="7157c-357">В этой задаче будет повторяться предыдущей процедуры, чтобы получить альбомов по их идентификаторам.</span><span class="sxs-lookup"><span data-stu-id="7157c-357">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="7157c-358">Закройте браузер, если необходимо, чтобы вернуться в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7157c-358">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="7157c-359">Откройте **StoreController** классе, чтобы изменить **сведения** метода действия.</span><span class="sxs-lookup"><span data-stu-id="7157c-359">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="7157c-360">Для этого в **обозревателе решений**, разверните **контроллеров** папку и дважды щелкните **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="7157c-360">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="7157c-361">Изменение **сведения** на основе метода действия, чтобы получить сведения о дисках их **идентификатор**. Для этого замените следующий код:</span><span class="sxs-lookup"><span data-stu-id="7157c-361">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="7157c-362">(Фрагмент - кода *моделей и доступа к данным — Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="7157c-362">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="7157c-363">Задача 4. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="7157c-363">Task 4 - Running the Application</span></span>

<span data-ttu-id="7157c-364">В этой задаче будет запустите приложение в веб-браузер и получить сведения об альбоме по их идентификаторам.</span><span class="sxs-lookup"><span data-stu-id="7157c-364">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="7157c-365">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="7157c-365">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7157c-366">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="7157c-366">The project starts in the Home page.</span></span> <span data-ttu-id="7157c-367">Изменить URL-адрес **/Store/Details/51** или жанров перейдите и выберите альбом, чтобы убедиться что результаты извлекаются из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-367">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="7157c-368">![Просмотр сведений](aspnet-mvc-4-models-and-data-access/_static/image25.png "Просмотр сведений")</span><span class="sxs-lookup"><span data-stu-id="7157c-368">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="7157c-369">*Просмотр /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="7157c-369">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="7157c-370">Кроме того, можно развернуть это приложение на веб-сайтов Windows Azure ниже [приложение б. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="7157c-370">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="7157c-371">Сводка</span><span class="sxs-lookup"><span data-stu-id="7157c-371">Summary</span></span>

<span data-ttu-id="7157c-372">Путем выполнения этого практические занятия вы изучили основы модели ASP.NET MVC и доступа к данным, с помощью **Database First** подход а также **Code First** подход:</span><span class="sxs-lookup"><span data-stu-id="7157c-372">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="7157c-373">Добавление базы данных в решение для обработки данных</span><span class="sxs-lookup"><span data-stu-id="7157c-373">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="7157c-374">Обновление контроллеров для предоставления шаблонов представлений с данными, извлеченными из базы данных вместо жестко запрограммированный</span><span class="sxs-lookup"><span data-stu-id="7157c-374">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="7157c-375">Как в базе данных с помощью параметров</span><span class="sxs-lookup"><span data-stu-id="7157c-375">How to query the database using parameters</span></span>
- <span data-ttu-id="7157c-376">Как использовать запрос результат формирование, возможность сокращает число обращений к базе данных, извлечение данных более эффективным способом</span><span class="sxs-lookup"><span data-stu-id="7157c-376">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="7157c-377">При использовании подхода Database First и Code First в Microsoft Entity Framework для связывания базы данных с моделью</span><span class="sxs-lookup"><span data-stu-id="7157c-377">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="7157c-378">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="7157c-378">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="7157c-379">Можно установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию, используя **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="7157c-379">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="7157c-380">Следующие инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="7157c-380">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="7157c-381">Последовательно выберите пункты [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="7157c-381">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="7157c-382">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="7157c-382">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="7157c-383">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="7157c-383">Click on **Install Now**.</span></span> <span data-ttu-id="7157c-384">Если у вас **Web Platform Installer** вы будете перенаправлены, чтобы загрузить и установить ее сначала.</span><span class="sxs-lookup"><span data-stu-id="7157c-384">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="7157c-385">Один раз **Web Platform Installer** открыто, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="7157c-385">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="7157c-386">![Установка Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="7157c-386">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="7157c-387">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="7157c-387">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="7157c-388">Чтение всех продуктов лицензий и условия и нажмите кнопку **принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="7157c-388">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензионного соглашения](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="7157c-390">*Принятие условий лицензионного соглашения*</span><span class="sxs-lookup"><span data-stu-id="7157c-390">*Accepting the license terms*</span></span>
5. <span data-ttu-id="7157c-391">Дождитесь завершения процесса загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="7157c-391">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="7157c-393">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="7157c-393">*Installation progress*</span></span>
6. <span data-ttu-id="7157c-394">По завершении установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="7157c-394">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="7157c-396">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="7157c-396">*Installation completed*</span></span>
7. <span data-ttu-id="7157c-397">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="7157c-397">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="7157c-398">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и начинается запись &quot; **VS Express**&quot;, выберите команду **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="7157c-398">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="7157c-400">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="7157c-400">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="7157c-401">Приложение б. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="7157c-401">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="7157c-402">В этом приложении будет показано, как создать новый веб-сайт на портале управления Windows Azure и публикация приложения, получить, выполнив в лаборатории преимуществами средство публикации веб-развертывания, предоставленного Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="7157c-402">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="7157c-403">Задача 1. Создание нового веб-узла с Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7157c-403">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="7157c-404">Последовательно выберите пункты [портала управления Windows Azure](https://manage.windowsazure.com/) и войдите с использованием учетных данных Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="7157c-404">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7157c-405">С помощью Windows Azure можно бесплатно размещения 10 веб-сайтов ASP.NET и затем масштабирование по мере увеличения трафика.</span><span class="sxs-lookup"><span data-stu-id="7157c-405">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="7157c-406">Вы можете зарегистрироваться [здесь](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="7157c-406">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="7157c-407">![Войдите на портал Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "входа на портал Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="7157c-407">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="7157c-408">*Выполните вход на портале управления Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="7157c-408">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="7157c-409">Нажмите кнопку **New** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="7157c-409">Click **New** on the command bar.</span></span>

    <span data-ttu-id="7157c-410">![Создание нового веб-сайта](aspnet-mvc-4-models-and-data-access/_static/image32.png "создания нового веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="7157c-410">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="7157c-411">*Создание нового веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="7157c-411">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="7157c-412">Нажмите кнопку **вычислений** | **веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="7157c-412">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="7157c-413">Выберите **быстрое создание** параметр.</span><span class="sxs-lookup"><span data-stu-id="7157c-413">Then select **Quick Create** option.</span></span> <span data-ttu-id="7157c-414">Укажите URL-адрес доступен для нового веб-сайта и нажмите кнопку **Создание веб-сайта**.</span><span class="sxs-lookup"><span data-stu-id="7157c-414">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7157c-415">Веб-приложение, работающее в облаке, вы можете управлять размещается веб-сайта Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="7157c-415">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="7157c-416">Параметр быстрого создания позволяет развернуть завершенное веб-приложение для Windows Azure веб-сайта из вне портала.</span><span class="sxs-lookup"><span data-stu-id="7157c-416">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="7157c-417">Он не включает действия по настройке базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-417">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="7157c-418">![Создание нового веб-сайта с помощью быстрого создания](aspnet-mvc-4-models-and-data-access/_static/image33.png "создания нового веб-сайта с помощью быстрого создания")</span><span class="sxs-lookup"><span data-stu-id="7157c-418">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="7157c-419">*Создание нового веб-сайта с помощью быстрого создания*</span><span class="sxs-lookup"><span data-stu-id="7157c-419">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="7157c-420">Подождите, пока новый **веб-сайт** создается.</span><span class="sxs-lookup"><span data-stu-id="7157c-420">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="7157c-421">После создания веб-сайта по ссылке **URL-адрес** столбца.</span><span class="sxs-lookup"><span data-stu-id="7157c-421">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="7157c-422">Проверьте, что новый веб-сайт работает.</span><span class="sxs-lookup"><span data-stu-id="7157c-422">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="7157c-423">![Просмотр новых веб-сайт](aspnet-mvc-4-models-and-data-access/_static/image34.png "просмотра на новый веб-сайт")</span><span class="sxs-lookup"><span data-stu-id="7157c-423">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="7157c-424">*Просмотр новых веб-сайт*</span><span class="sxs-lookup"><span data-stu-id="7157c-424">*Browsing to the new web site*</span></span>

    <span data-ttu-id="7157c-425">![Веб-сайта под управлением](aspnet-mvc-4-models-and-data-access/_static/image35.png "веб-сайта под управлением")</span><span class="sxs-lookup"><span data-stu-id="7157c-425">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="7157c-426">*Запуск веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="7157c-426">*Web site running*</span></span>
6. <span data-ttu-id="7157c-427">Вернитесь на портал и щелкните имя веб-сайта в разделе **имя** столбец для отображения страницы управления.</span><span class="sxs-lookup"><span data-stu-id="7157c-427">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="7157c-428">![Открытие страницы управления веб-сайт](aspnet-mvc-4-models-and-data-access/_static/image36.png "Открытие страниц управления веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="7157c-428">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="7157c-429">*Открытие страницы управления веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="7157c-429">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="7157c-430">В **панели мониторинга** в разделе **беглого** щелкните **загрузить профиль публикации** ссылку.</span><span class="sxs-lookup"><span data-stu-id="7157c-430">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7157c-431">*Профиль публикации* содержит все сведения, необходимые для публикации веб-приложения на веб-сайте Windows Azure для каждого разрешенного метода публикации.</span><span class="sxs-lookup"><span data-stu-id="7157c-431">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="7157c-432">Профиль публикации содержит URL-адреса, учетные данные пользователя и строк базы данных, необходимых для подключения и проверки подлинности с каждой из конечных точек, для которых включен метода публикации.</span><span class="sxs-lookup"><span data-stu-id="7157c-432">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="7157c-433">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express для Web** и **Microsoft Visual Studio 2012** поддерживают чтение профилей публикации для автоматизации настройки этих программ для Публикация веб-приложений на веб-сайтов Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="7157c-433">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="7157c-434">![Профиль публикации веб-сайт загрузки](aspnet-mvc-4-models-and-data-access/_static/image37.png "профиль публикации веб-сайта загрузки")</span><span class="sxs-lookup"><span data-stu-id="7157c-434">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="7157c-435">*Профиль публикации веб-сайта загрузки*</span><span class="sxs-lookup"><span data-stu-id="7157c-435">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="7157c-436">Загрузите файл профиля публикации в известном расположении.</span><span class="sxs-lookup"><span data-stu-id="7157c-436">Download the publish profile file to a known location.</span></span> <span data-ttu-id="7157c-437">Далее в этом упражнении вы увидите как использовать этот файл для публикации веб-приложения для веб-сайтов, Windows Azure из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7157c-437">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="7157c-438">![Сохранить файл профиля публикации](aspnet-mvc-4-models-and-data-access/_static/image38.png "Сохранение профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="7157c-438">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="7157c-439">*Сохранить файл профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="7157c-439">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="7157c-440">Задача 2 - Настройка сервера базы данных</span><span class="sxs-lookup"><span data-stu-id="7157c-440">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="7157c-441">Если приложение использует SQL Server базы данных, необходимо будет создать сервер базы данных SQL.</span><span class="sxs-lookup"><span data-stu-id="7157c-441">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="7157c-442">Если вы хотите развернуть простое приложение, которое не использует SQL Server может пропустить эту задачу.</span><span class="sxs-lookup"><span data-stu-id="7157c-442">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="7157c-443">Сервер базы данных SQL необходимо для хранения базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="7157c-443">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="7157c-444">Можно просматривать серверы базы данных SQL из подписки на портале управления Windows Azure в **баз данных Sql** | **серверы** | **сервера Панель мониторинга**.</span><span class="sxs-lookup"><span data-stu-id="7157c-444">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="7157c-445">Если у вас на сервер, созданный, можно создать ее, используя **добавить** кнопки на панели команд.</span><span class="sxs-lookup"><span data-stu-id="7157c-445">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="7157c-446">Обратите внимание **имя сервера и URL-адрес, имя входа администратора и пароль**, как будет использовать их в следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="7157c-446">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="7157c-447">Не следует создавать базу данных, как он будет создан в более поздней стадии.</span><span class="sxs-lookup"><span data-stu-id="7157c-447">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="7157c-448">![Панель мониторинга базы данных SQL Server](aspnet-mvc-4-models-and-data-access/_static/image39.png "панель мониторинга базы данных SQL Server")</span><span class="sxs-lookup"><span data-stu-id="7157c-448">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="7157c-449">*Панель мониторинга базы данных SQL Server*</span><span class="sxs-lookup"><span data-stu-id="7157c-449">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="7157c-450">В следующей задаче требуется проверить подключение к базе данных из Visual Studio, по этой причине необходимо включить локальный IP-адреса в список серверов **разрешенные IP-адреса**.</span><span class="sxs-lookup"><span data-stu-id="7157c-450">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="7157c-451">Чтобы сделать это, нажмите кнопку **Настройка**, выберите IP-адрес из **текущий IP-адрес клиента** и вставьте его на **начальный IP-адрес** и **конечный IP-адрес** текстовые поля и нажмите кнопку ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) кнопки.</span><span class="sxs-lookup"><span data-stu-id="7157c-451">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Добавление IP-адрес клиента](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="7157c-453">*Добавление IP-адрес клиента*</span><span class="sxs-lookup"><span data-stu-id="7157c-453">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="7157c-454">Один раз **IP-адрес клиента** добавляется разрешенных IP-адресов выберите на **Сохранить** для подтверждения изменения.</span><span class="sxs-lookup"><span data-stu-id="7157c-454">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Подтверждение изменений](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="7157c-456">*Подтверждение изменений*</span><span class="sxs-lookup"><span data-stu-id="7157c-456">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="7157c-457">Задача 3. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="7157c-457">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="7157c-458">Вернитесь в решении ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7157c-458">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="7157c-459">В **обозревателе решений**, щелкните правой кнопкой мыши проект веб-сайта и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="7157c-459">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="7157c-460">![Публикация приложения](aspnet-mvc-4-models-and-data-access/_static/image43.png "публикация приложения")</span><span class="sxs-lookup"><span data-stu-id="7157c-460">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="7157c-461">*Публикация веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="7157c-461">*Publishing the web site*</span></span>
2. <span data-ttu-id="7157c-462">Импортируйте профиль публикации, сохраненный в первой задаче.</span><span class="sxs-lookup"><span data-stu-id="7157c-462">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="7157c-463">![Импорт профиля публикации](aspnet-mvc-4-models-and-data-access/_static/image44.png "Импорт профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="7157c-463">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="7157c-464">*Импорт профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="7157c-464">*Importing publish profile*</span></span>
3. <span data-ttu-id="7157c-465">Нажмите кнопку **Проверка подключения**.</span><span class="sxs-lookup"><span data-stu-id="7157c-465">Click **Validate Connection**.</span></span> <span data-ttu-id="7157c-466">После завершения проверки нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="7157c-466">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7157c-467">Проверка завершена, как только вы увидите зеленый флажок рядом с "Проверить подключение".</span><span class="sxs-lookup"><span data-stu-id="7157c-467">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="7157c-468">![Проверка подключения](aspnet-mvc-4-models-and-data-access/_static/image45.png "Проверка подключения")</span><span class="sxs-lookup"><span data-stu-id="7157c-468">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="7157c-469">*Проверка подключения*</span><span class="sxs-lookup"><span data-stu-id="7157c-469">*Validating connection*</span></span>
4. <span data-ttu-id="7157c-470">В **параметры** в разделе **баз данных** статьи, нажмите кнопку рядом с текстового поля для подключения к базе данных (т. е. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="7157c-470">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="7157c-471">![Конфигурация веб-развертывания](aspnet-mvc-4-models-and-data-access/_static/image46.png "конфигурация веб-развертывания")</span><span class="sxs-lookup"><span data-stu-id="7157c-471">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="7157c-472">*Конфигурация веб-развертывания*</span><span class="sxs-lookup"><span data-stu-id="7157c-472">*Web deploy configuration*</span></span>
5. <span data-ttu-id="7157c-473">Настройте подключение к базе данных следующим образом.</span><span class="sxs-lookup"><span data-stu-id="7157c-473">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="7157c-474">В **имя сервера** введите ваш URL-адрес сервера базы данных SQL с помощью *tcp:* префикс.</span><span class="sxs-lookup"><span data-stu-id="7157c-474">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="7157c-475">В **имя пользователя** введите имя входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="7157c-475">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="7157c-476">В **пароль** введите пароль входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="7157c-476">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="7157c-477">Введите имя новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="7157c-477">Type a new database name.</span></span>

     <span data-ttu-id="7157c-478">![Настройка строки подключения назначения](aspnet-mvc-4-models-and-data-access/_static/image47.png "Настройка целевой строки подключения")</span><span class="sxs-lookup"><span data-stu-id="7157c-478">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="7157c-479">*Настройка целевой строки подключения*</span><span class="sxs-lookup"><span data-stu-id="7157c-479">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="7157c-480">Затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="7157c-480">Then click **OK**.</span></span> <span data-ttu-id="7157c-481">Когда появится запрос на создание базы данных щелкните **Да**.</span><span class="sxs-lookup"><span data-stu-id="7157c-481">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="7157c-482">![Создание базы данных](aspnet-mvc-4-models-and-data-access/_static/image48.png "Создание строки базы данных")</span><span class="sxs-lookup"><span data-stu-id="7157c-482">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="7157c-483">*Создание базы данных*</span><span class="sxs-lookup"><span data-stu-id="7157c-483">*Creating the database*</span></span>
7. <span data-ttu-id="7157c-484">Строка подключения, который будет использоваться для подключения к базе данных SQL в Windows Azure отображается внутри текстового поля по умолчанию соединения.</span><span class="sxs-lookup"><span data-stu-id="7157c-484">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="7157c-485">Затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="7157c-485">Then click **Next**.</span></span>

    <span data-ttu-id="7157c-486">![Строка подключения, указывающий базу данных SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "строку соединения, указывающий базу данных SQL")</span><span class="sxs-lookup"><span data-stu-id="7157c-486">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="7157c-487">*Строка подключения, указывающий базу данных SQL*</span><span class="sxs-lookup"><span data-stu-id="7157c-487">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="7157c-488">В **предварительного просмотра** щелкните **публикации**.</span><span class="sxs-lookup"><span data-stu-id="7157c-488">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="7157c-489">![Публикация веб-приложения](aspnet-mvc-4-models-and-data-access/_static/image50.png "публикации веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="7157c-489">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="7157c-490">*Публикация веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="7157c-490">*Publishing the web application*</span></span>
9. <span data-ttu-id="7157c-491">После завершения процесса публикации в браузере по умолчанию откроется опубликованного веб-узла.</span><span class="sxs-lookup"><span data-stu-id="7157c-491">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="7157c-492">Приложение в. фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="7157c-492">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="7157c-493">С помощью фрагментов кода у вас есть весь код, который требуется под рукой.</span><span class="sxs-lookup"><span data-stu-id="7157c-493">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="7157c-494">Документ лаборатории сообщает только при их использовании, как показано на следующем рисунке.</span><span class="sxs-lookup"><span data-stu-id="7157c-494">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="7157c-495">![Фрагменты кода Visual Studio для вставки кода в проекте](aspnet-mvc-4-models-and-data-access/_static/image51.png "фрагменты кода с помощью Visual Studio вставьте код в проект")</span><span class="sxs-lookup"><span data-stu-id="7157c-495">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="7157c-496">*Фрагменты кода Visual Studio для вставки кода в проекте*</span><span class="sxs-lookup"><span data-stu-id="7157c-496">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="7157c-497">***Добавление фрагмента кода, с помощью клавиатуры (только C#)***</span><span class="sxs-lookup"><span data-stu-id="7157c-497">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="7157c-498">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="7157c-498">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="7157c-499">Начните вводить имя фрагмента (без пробелов и дефисы).</span><span class="sxs-lookup"><span data-stu-id="7157c-499">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="7157c-500">Смотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="7157c-500">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="7157c-501">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="7157c-501">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="7157c-502">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="7157c-502">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="7157c-503">![Начните вводить имя фрагмента](aspnet-mvc-4-models-and-data-access/_static/image52.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="7157c-503">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="7157c-504">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="7157c-504">*Start typing the snippet name*</span></span>

<span data-ttu-id="7157c-505">![Нажмите клавишу Tab, чтобы выделить выделенный фрагмент](aspnet-mvc-4-models-and-data-access/_static/image53.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="7157c-505">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="7157c-506">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="7157c-506">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="7157c-507">![Снова нажмите клавишу Tab и фрагмент будет расширен](aspnet-mvc-4-models-and-data-access/_static/image54.png "будет расширен, снова нажмите клавишу Tab и фрагмента кода")</span><span class="sxs-lookup"><span data-stu-id="7157c-507">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="7157c-508">*Снова нажмите клавишу Tab и фрагмент будет расширен*</span><span class="sxs-lookup"><span data-stu-id="7157c-508">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="7157c-509">***Добавление фрагмента кода, с помощью мыши (C#, Visual Basic и XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="7157c-509">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="7157c-510">Щелкните правой кнопкой мыши место вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="7157c-510">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="7157c-511">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="7157c-511">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="7157c-512">Выберите соответствующий фрагмент из списка, щелкнув по ней.</span><span class="sxs-lookup"><span data-stu-id="7157c-512">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="7157c-513">![Щелкните правой кнопкой мыши место для вставки фрагмента кода и выбора вставить фрагмент](aspnet-mvc-4-models-and-data-access/_static/image55.png "место для вставки фрагмента кода и выбора вставить фрагмент кода щелкните правой кнопкой мыши")</span><span class="sxs-lookup"><span data-stu-id="7157c-513">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="7157c-514">*Щелкните правой кнопкой мыши в место вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="7157c-514">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="7157c-515">![Выберите из списка, соответствующего фрагмента, щелкнув его](aspnet-mvc-4-models-and-data-access/_static/image56.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="7157c-515">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="7157c-516">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="7157c-516">*Pick the relevant snippet from the list, by clicking on it*</span></span>
