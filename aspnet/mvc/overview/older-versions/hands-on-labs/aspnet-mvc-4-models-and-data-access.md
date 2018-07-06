---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Модели ASP.NET MVC 4 и доступа к данным | Документация Майкрософт
author: rick-anderson
description: 'Примечание: Этот практический семинар предполагается, что у вас есть базовые знания о ASP.NET MVC. Если вы не использовали ASP.NET MVC перед, мы рекомендуем перейти на ASP.NET MVC 4...'
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: bbb726f9f4abb2f9c72153a8c7986c5c83a9a840
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818376"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="85f77-104">Модели ASP.NET MVC 4 и доступа к данным</span><span class="sxs-lookup"><span data-stu-id="85f77-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="85f77-105">по [Web Слышатся Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="85f77-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="85f77-106">Скачайте комплект учебных материалов по лагеря Web</span><span class="sxs-lookup"><span data-stu-id="85f77-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="85f77-107">Это Практическое лабораторное занятие предполагается, что у вас есть базовые знания о **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="85f77-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="85f77-108">Если вы не использовали **ASP.NET MVC** раньше, мы рекомендуем к превышению **ASP.NET MVC 4 Fundamentals** Практическое лабораторное занятие.</span><span class="sxs-lookup"><span data-stu-id="85f77-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="85f77-109">Это лабораторное занятие поможет усовершенствования и новые возможности, описанные ранее, применяя незначительные изменения в образец веб-приложение, в папке с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="85f77-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="85f77-110">Все примеры кода и фрагменты кода включены в Web лагеря комплект обучающих материалов, доступных в [выпуски Microsoft Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="85f77-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="85f77-111">Проект, относящиеся к этой лаборатории доступен в [ASP.NET MVC 4 модели и доступ к данным](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="85f77-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="85f77-112">В **основы ASP.NET MVC** в лаборатории вы передача жестко данных с контроллеров в шаблоны представлений.</span><span class="sxs-lookup"><span data-stu-id="85f77-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="85f77-113">Но для создания реальных веб-приложения, может потребоваться использовать реальную базу данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="85f77-114">Это Практическое лабораторное занятие будет показано, как использовать ядро СУБД, чтобы сохранять и извлекать данные, необходимые для приложения Music Store.</span><span class="sxs-lookup"><span data-stu-id="85f77-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="85f77-115">Чтобы сделать это, будет начинаться с существующей базы данных и создать модель EDM из него.</span><span class="sxs-lookup"><span data-stu-id="85f77-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="85f77-116">В этой лабораторной работе будут соответствовать **Database First** подход, а также **Code First** подход.</span><span class="sxs-lookup"><span data-stu-id="85f77-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="85f77-117">Тем не менее, можно также использовать **Model First** подход, создания той же модели, с помощью средств и затем создать базу данных из него.</span><span class="sxs-lookup"><span data-stu-id="85f77-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="85f77-118">![Vs первой базы данных. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span><span class="sxs-lookup"><span data-stu-id="85f77-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="85f77-119">*Vs первой базы данных. Model First*</span><span class="sxs-lookup"><span data-stu-id="85f77-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="85f77-120">После создания модели, вы внесете соответствующие настройки в StoreController для предоставления Store представлений с данными, извлеченными из базы данных, вместо использования жестко закодированных данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="85f77-121">Не потребуется вносить изменения в шаблоны представлений, поскольку StoreController будут возвращать же модели ViewModel в шаблоны представлений, несмотря на то, что это время будут извлекаться данные из базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="85f77-122">**Первый подход на основе кода**</span><span class="sxs-lookup"><span data-stu-id="85f77-122">**The Code First Approach**</span></span>

<span data-ttu-id="85f77-123">Подход Code First позволяет определить модели из кода без создания классов, которые обычно связаны друг с другом с помощью платформы.</span><span class="sxs-lookup"><span data-stu-id="85f77-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="85f77-124">В коде во-первых, объекты модели определяются с помощью POCO, &quot;обычные старые объекты CLR&quot;.</span><span class="sxs-lookup"><span data-stu-id="85f77-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="85f77-125">POCO являются простые обычные классы, которые не поддерживают наследование и не реализуют интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="85f77-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="85f77-126">Мы автоматически создавать базы данных из них, или чтобы использовать существующую базу данных и создать сопоставление классов из кода.</span><span class="sxs-lookup"><span data-stu-id="85f77-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="85f77-127">Преимущества использования этого подхода является то, что модели остается независимой от инфраструктуру сохранения (в данном случае Entity Framework), как классы POCO не связаны с платформу сопоставления.</span><span class="sxs-lookup"><span data-stu-id="85f77-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="85f77-128">Это лабораторное занятие основан на ASP.NET MVC 4 и версию примера приложения Music Store настроен и в свернутом состоянии в соответствии с только функции, приведенные в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="85f77-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="85f77-129">Если вы хотите просмотреть весь **Music Store** учебного приложения, его можно найти в [MVC-музыка-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="85f77-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="85f77-130">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="85f77-130">Prerequisites</span></span>

<span data-ttu-id="85f77-131">Необходимо иметь следующие элементы во укомплектовать данную лабораторию:</span><span class="sxs-lookup"><span data-stu-id="85f77-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="85f77-132">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или руководителя (чтение [приложение А](#AppendixA) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="85f77-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="85f77-133">Установка</span><span class="sxs-lookup"><span data-stu-id="85f77-133">Setup</span></span>

<span data-ttu-id="85f77-134">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="85f77-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="85f77-135">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступен как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="85f77-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="85f77-136">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="85f77-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="85f77-137">Если вы не знакомы с фрагменты кода Visual Studio и хотите научиться использовать их, можно ссылаться в приложение из этого документа &quot; [фрагменты кода с помощью приложение C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="85f77-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="85f77-138">Упражнения</span><span class="sxs-lookup"><span data-stu-id="85f77-138">Exercises</span></span>

<span data-ttu-id="85f77-139">Это Практическое лабораторное занятие включает по следующие упражнения:</span><span class="sxs-lookup"><span data-stu-id="85f77-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="85f77-140">Упражнение 1: Добавление базы данных</span><span class="sxs-lookup"><span data-stu-id="85f77-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="85f77-141">Упражнение 2: Создание базы данных с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="85f77-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="85f77-142">Упражнение 3: Запрос в базу данных с параметрами</span><span class="sxs-lookup"><span data-stu-id="85f77-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="85f77-143">Каждого упражнения сопровождается **окончания** папку, содержащую полученное решение, должен быть получен после завершения упражнения.</span><span class="sxs-lookup"><span data-stu-id="85f77-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="85f77-144">Это решение можно использовать как руководство, если вам нужна дополнительная помощь при работе с примерами.</span><span class="sxs-lookup"><span data-stu-id="85f77-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="85f77-145">Предполагаемое время для выполнения этого лабораторного: **35 минут**.</span><span class="sxs-lookup"><span data-stu-id="85f77-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="85f77-146">Упражнение 1: Добавление базы данных</span><span class="sxs-lookup"><span data-stu-id="85f77-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="85f77-147">В этом упражнении вы узнаете, как добавить базу данных с таблицами приложение MusicStore в решение, чтобы использовать его данные.</span><span class="sxs-lookup"><span data-stu-id="85f77-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="85f77-148">Когда базы данных создается с помощью модели и добавляется в решение, необходимо изменить класс StoreController, чтобы предоставить шаблон представления с данными, извлеченными из базы данных, вместо жестко заданных значений.</span><span class="sxs-lookup"><span data-stu-id="85f77-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="85f77-149">Задача 1 - Добавление базы данных</span><span class="sxs-lookup"><span data-stu-id="85f77-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="85f77-150">В этой задаче вы добавите уже созданную базу данных с основные таблицы приложения MusicStore в решение.</span><span class="sxs-lookup"><span data-stu-id="85f77-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="85f77-151">Откройте **начать** решений, расположенный **источника/сервера Ex1-AddingADatabaseDBFirst/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="85f77-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="85f77-152">Необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="85f77-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="85f77-153">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="85f77-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="85f77-154">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="85f77-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="85f77-155">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="85f77-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="85f77-156">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="85f77-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="85f77-157">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="85f77-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="85f77-158">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="85f77-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="85f77-159">Добавить **MvcMusicStore** файл базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="85f77-160">В этой лаборатории вы будете использовать уже созданную базу данных с именем **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="85f77-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="85f77-161">Чтобы сделать это, щелкните правой кнопкой мыши **приложения\_данных** папку **добавить** и нажмите кнопку **существующий элемент**.</span><span class="sxs-lookup"><span data-stu-id="85f77-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="85f77-162">Перейдите к **\Source\Assets** и выберите **MvcMusicStore.mdf** файла.</span><span class="sxs-lookup"><span data-stu-id="85f77-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="85f77-163">![Добавление существующего элемента](aspnet-mvc-4-models-and-data-access/_static/image2.png "Добавление существующего элемента")</span><span class="sxs-lookup"><span data-stu-id="85f77-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="85f77-164">*Добавление существующего элемента*</span><span class="sxs-lookup"><span data-stu-id="85f77-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="85f77-165">![Файл базы данных MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf файл базы данных")</span><span class="sxs-lookup"><span data-stu-id="85f77-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="85f77-166">*Файл базы данных MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="85f77-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="85f77-167">Базы данных был добавлен в проект.</span><span class="sxs-lookup"><span data-stu-id="85f77-167">The database has been added to the project.</span></span> <span data-ttu-id="85f77-168">Даже в том случае, если база данных находится в решении, можно запрашивать и обновлять по мере она была размещена в другой сервер базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="85f77-169">![MvcMusicStore базы данных в обозревателе решений](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore базы данных в обозревателе решений")</span><span class="sxs-lookup"><span data-stu-id="85f77-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="85f77-170">*MvcMusicStore базы данных в обозревателе решений*</span><span class="sxs-lookup"><span data-stu-id="85f77-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="85f77-171">Проверьте подключение к базе данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-171">Verify the connection to the database.</span></span> <span data-ttu-id="85f77-172">Чтобы сделать это, дважды щелкните **MvcMusicStore.mdf** для установления соединения.</span><span class="sxs-lookup"><span data-stu-id="85f77-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="85f77-173">![Подключение к MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "подключение к MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="85f77-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="85f77-174">*Подключение к MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="85f77-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="85f77-175">Задача 2 - Создание модели данных</span><span class="sxs-lookup"><span data-stu-id="85f77-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="85f77-176">В этой задаче вы создадите модель данных для взаимодействия с базой данных, добавленные в предыдущей задаче.</span><span class="sxs-lookup"><span data-stu-id="85f77-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="85f77-177">Создайте модель данных, который будет представлять базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="85f77-178">Для этого в обозревателе решений щелкните правой кнопкой мыши **моделей** папку **добавить** и нажмите кнопку **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="85f77-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="85f77-179">В **Добавление нового элемента** диалоговом окне выберите **данных** шаблон и затем **ADO.NET Entity Data Model** элемента.</span><span class="sxs-lookup"><span data-stu-id="85f77-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="85f77-180">Измените имя модели данных на **StoreDB.edmx** и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="85f77-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="85f77-181">![Добавление модели ADO.NET EDM StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "Добавление StoreDB модели EDM ADO.NET")</span><span class="sxs-lookup"><span data-stu-id="85f77-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="85f77-182">*Добавление модели ADO.NET EDM StoreDB*</span><span class="sxs-lookup"><span data-stu-id="85f77-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="85f77-183">**Мастер моделей EDM** будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="85f77-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="85f77-184">Этот мастер поможет выполнить создание слоя модели.</span><span class="sxs-lookup"><span data-stu-id="85f77-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="85f77-185">Поскольку модели должен будет создан в зависимости от существующей recentyl базы данных, которые добавлены, выберите **создать из базы данных** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="85f77-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="85f77-186">![Выбор содержимого модели](aspnet-mvc-4-models-and-data-access/_static/image7.png "Выбор содержимого модели")</span><span class="sxs-lookup"><span data-stu-id="85f77-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="85f77-187">*Выбор содержимого модели*</span><span class="sxs-lookup"><span data-stu-id="85f77-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="85f77-188">Так как при создании модели из базы данных, необходимо будет указать используемое соединение.</span><span class="sxs-lookup"><span data-stu-id="85f77-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="85f77-189">Нажмите кнопку **новое подключение**.</span><span class="sxs-lookup"><span data-stu-id="85f77-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="85f77-190">Выберите **Microsoft SQL Server Database File** и нажмите кнопку **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="85f77-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="85f77-191">![Выбрать источник данных](aspnet-mvc-4-models-and-data-access/_static/image8.png "выбрать источник данных")</span><span class="sxs-lookup"><span data-stu-id="85f77-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="85f77-192">*Выберите источник данных*</span><span class="sxs-lookup"><span data-stu-id="85f77-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="85f77-193">Нажмите кнопку **Обзор** и выберите базу данных **MvcMusicStore.mdf** найденную на **приложения\_данных** папку и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="85f77-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="85f77-194">![Свойства соединения](aspnet-mvc-4-models-and-data-access/_static/image9.png "свойства подключения")</span><span class="sxs-lookup"><span data-stu-id="85f77-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="85f77-195">*Свойства подключения*</span><span class="sxs-lookup"><span data-stu-id="85f77-195">*Connection properties*</span></span>
6. <span data-ttu-id="85f77-196">Созданный класс должен иметь имя, совпадающее с именем строка соединения сущности, поэтому измените ее имя на **MusicStoreEntities** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="85f77-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="85f77-197">![Выбор подключения к данным](aspnet-mvc-4-models-and-data-access/_static/image10.png "Выбор подключения к данным")</span><span class="sxs-lookup"><span data-stu-id="85f77-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="85f77-198">*Выбор подключения к данным*</span><span class="sxs-lookup"><span data-stu-id="85f77-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="85f77-199">Выберите объекты базы данных для использования.</span><span class="sxs-lookup"><span data-stu-id="85f77-199">Choose the database objects to use.</span></span> <span data-ttu-id="85f77-200">Как модель сущностей будет использовать только для таблиц базы данных, выберите **таблиц** параметр и убедитесь, что **включить столбцы внешнего ключа в модель** и **множественном или единственном числе создан имена объектов** выбраны параметры.</span><span class="sxs-lookup"><span data-stu-id="85f77-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="85f77-201">Изменить пространство имен модели для **MvcMusicStore.Model** и нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="85f77-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="85f77-202">![Выбор объектов базы данных](aspnet-mvc-4-models-and-data-access/_static/image11.png "Выбор объектов базы данных")</span><span class="sxs-lookup"><span data-stu-id="85f77-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="85f77-203">*Выбор объектов базы данных*</span><span class="sxs-lookup"><span data-stu-id="85f77-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="85f77-204">Если отображается диалоговое окно с предупреждением безопасности, нажмите кнопку **ОК** для выполнения шаблона и создания классов для модели сущностей.</span><span class="sxs-lookup"><span data-stu-id="85f77-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="85f77-205">Схеме сущностей для базы данных будет отображаться, пока будет создан отдельный класс, который сопоставляется каждой таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="85f77-206">Например **альбомов** таблицы будет представлен **альбома** класса, где каждый столбец в таблице сопоставит к свойству класса.</span><span class="sxs-lookup"><span data-stu-id="85f77-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="85f77-207">Это позволит вам выполнять запросы и работать с объектами, представляющими строк в базе данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="85f77-208">![Схема сущности](aspnet-mvc-4-models-and-data-access/_static/image12.png "схема сущности")</span><span class="sxs-lookup"><span data-stu-id="85f77-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="85f77-209">*Схема сущности*</span><span class="sxs-lookup"><span data-stu-id="85f77-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="85f77-210">Шаблоны T4 (.tt) запустите код для создания классов сущностей и перезапишет существующие классы с тем же именем.</span><span class="sxs-lookup"><span data-stu-id="85f77-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="85f77-211">В этом примере классы &quot;альбома&quot;, &quot;жанр&quot; и &quot;исполнителя&quot; были перезаписаны с помощью созданного кода.</span><span class="sxs-lookup"><span data-stu-id="85f77-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="85f77-212">Задача 3 - построение приложения</span><span class="sxs-lookup"><span data-stu-id="85f77-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="85f77-213">В этой задаче производится проверка, несмотря на то, что создание модели удалены **альбома**, **жанр** и **исполнителя** классов модели успешного построения проекта с помощью новые классы модели данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="85f77-214">Постройте проект, выбрав **построения** пункта меню и затем **построения MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="85f77-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="85f77-215">![Построение проекта](aspnet-mvc-4-models-and-data-access/_static/image13.png "построение проекта")</span><span class="sxs-lookup"><span data-stu-id="85f77-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="85f77-216">*Построение проекта*</span><span class="sxs-lookup"><span data-stu-id="85f77-216">*Building the project*</span></span>
2. <span data-ttu-id="85f77-217">Сборка проекта выполняется успешно.</span><span class="sxs-lookup"><span data-stu-id="85f77-217">The project builds successfully.</span></span> <span data-ttu-id="85f77-218">Почему это по-прежнему работает?</span><span class="sxs-lookup"><span data-stu-id="85f77-218">Why does it still work?</span></span> <span data-ttu-id="85f77-219">Он работает, поскольку таблицы базы данных имеют поля, которые включают свойства, которые использовались в классах удален **альбома** и **жанр**.</span><span class="sxs-lookup"><span data-stu-id="85f77-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="85f77-220">![Сборки успешно](aspnet-mvc-4-models-and-data-access/_static/image14.png "сборок успешно завершена")</span><span class="sxs-lookup"><span data-stu-id="85f77-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="85f77-221">*Выполненные сборки*</span><span class="sxs-lookup"><span data-stu-id="85f77-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="85f77-222">Хотя конструктор отображает сущности в виде схемы, они действительно классов C#.</span><span class="sxs-lookup"><span data-stu-id="85f77-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="85f77-223">Разверните **StoreDB.edmx** узел в обозревателе решений и затем **StoreDB.tt**, вы увидите созданный новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="85f77-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="85f77-224">![Созданные файлы](aspnet-mvc-4-models-and-data-access/_static/image15.png "созданные файлы")</span><span class="sxs-lookup"><span data-stu-id="85f77-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="85f77-225">*Созданные файлы*</span><span class="sxs-lookup"><span data-stu-id="85f77-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="85f77-226">Задача 4 - запрос в базу данных</span><span class="sxs-lookup"><span data-stu-id="85f77-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="85f77-227">В этой задаче будет обновить класс StoreController таким образом, чтобы вместо использования жестко закодированные данные, он будет запрашивать базы данных для получения сведений.</span><span class="sxs-lookup"><span data-stu-id="85f77-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="85f77-228">Откройте **Controllers\StoreController.cs** и добавьте следующее поле в класс для хранения экземпляра **MusicStoreEntities** класс с именем **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="85f77-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="85f77-229">(Code Snippet - *моделей и доступ к данным — storeDB сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="85f77-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="85f77-230">**MusicStoreEntities** класс предоставляет свойство коллекции для каждой таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="85f77-231">Обновление **Обзор** метода действия для получения жанр фильма со всеми **альбомов**.</span><span class="sxs-lookup"><span data-stu-id="85f77-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="85f77-232">(Code Snippet - *модели и доступ к данным — Обзор Store сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="85f77-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="85f77-233">Вы используете функцию версию.NET, называемую **LINQ** (запросы LINQ), для написания выражений запросов со строгой типизацией для этих коллекций — которые будет выполнять код в базе данных и возвращают объекты, что вы можете программировать к.</span><span class="sxs-lookup"><span data-stu-id="85f77-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="85f77-234">Дополнительные сведения о LINQ см. на [сайта msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="85f77-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="85f77-235">Обновление **индекс** метода действия для получения всех жанров.</span><span class="sxs-lookup"><span data-stu-id="85f77-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="85f77-236">(Code Snippet - *модели и доступ к данным — индекс Store сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="85f77-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="85f77-237">Обновление **индекс** метода действия, чтобы получить все жанры и преобразования коллекции в список.</span><span class="sxs-lookup"><span data-stu-id="85f77-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="85f77-238">(Code Snippet - *модели и доступ к данным — GenreMenu Store сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="85f77-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="85f77-239">Задача 5 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="85f77-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="85f77-240">В этой задаче производится проверка на страницу индекса Store теперь будет отображать жанры, хранящиеся в базе данных, а не жестко закодированное из них.</span><span class="sxs-lookup"><span data-stu-id="85f77-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="85f77-241">Чтобы изменить этот шаблон, так как нет необходимости **StoreController** возвращает те же сущности как и раньше, несмотря на то, что это время будут извлекаться данные из базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="85f77-242">Перестройте решение и нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="85f77-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="85f77-243">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="85f77-243">The project starts in the Home page.</span></span> <span data-ttu-id="85f77-244">Убедитесь, что в меню **жанров** больше не жестко списка, а данные извлекаются непосредственно из базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="85f77-246">*Просмотр жанры из базы данных*</span><span class="sxs-lookup"><span data-stu-id="85f77-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="85f77-247">Теперь перейдите к любой жанр и убедитесь, что альбомов заполняются из базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="85f77-248">![Просмотр альбомов из базы данных](aspnet-mvc-4-models-and-data-access/_static/image17.png "просмотра альбомов из базы данных")</span><span class="sxs-lookup"><span data-stu-id="85f77-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="85f77-249">*Просмотр альбомов из базы данных*</span><span class="sxs-lookup"><span data-stu-id="85f77-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="85f77-250">Упражнение 2: Создание базы данных сначала с помощью кода</span><span class="sxs-lookup"><span data-stu-id="85f77-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="85f77-251">В этом упражнении вы узнаете, как использовать подход Code First для создания базы данных с таблицами MusicStore приложения и как получить доступ к данным.</span><span class="sxs-lookup"><span data-stu-id="85f77-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="85f77-252">После создания модели, необходимо изменить StoreController для предоставления шаблона представления с данными, извлеченными из базы данных, вместо использования жестко заданные значения.</span><span class="sxs-lookup"><span data-stu-id="85f77-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="85f77-253">Если вы выполнили Упражнение 1 и ранее пользователь работал с использованием подхода Database First, вы узнаете как получить те же результаты с помощью другого процесса.</span><span class="sxs-lookup"><span data-stu-id="85f77-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="85f77-254">Чтобы облегчить ознакомление были помечены задачи, которые являются общими с Упражнение 1.</span><span class="sxs-lookup"><span data-stu-id="85f77-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="85f77-255">Если вы не были завершены упражнения 1, но нужно получить дополнительные сведения о подходе Code First, можно запустить из этого упражнения и обеспечить полную защиту этого раздела.</span><span class="sxs-lookup"><span data-stu-id="85f77-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="85f77-256">Задача 1 - заполнению примера данных</span><span class="sxs-lookup"><span data-stu-id="85f77-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="85f77-257">В этой задаче вы заполните базу данных, с демонстрационными данными при начале создается с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="85f77-257">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="85f77-258">Откройте **начать** решений, расположенный **источника/Ex2-CreatingADatabaseCodeFirst/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="85f77-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="85f77-259">В противном случае можно продолжить использование **окончания** решение получен путем выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="85f77-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="85f77-260">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="85f77-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="85f77-261">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="85f77-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="85f77-262">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="85f77-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="85f77-263">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="85f77-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="85f77-264">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="85f77-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="85f77-265">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="85f77-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="85f77-266">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="85f77-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="85f77-267">Добавить **SampleData.cs** файл **моделей** папки.</span><span class="sxs-lookup"><span data-stu-id="85f77-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="85f77-268">Чтобы сделать это, щелкните правой кнопкой мыши **моделей** папку **добавить** и нажмите кнопку **существующий элемент**.</span><span class="sxs-lookup"><span data-stu-id="85f77-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="85f77-269">Перейдите к **\Source\Assets** и выберите **SampleData.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="85f77-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="85f77-270">![Образец данных заполнения кода](aspnet-mvc-4-models-and-data-access/_static/image18.png "демонстрационные данные заполнения кода")</span><span class="sxs-lookup"><span data-stu-id="85f77-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="85f77-271">*Образец данных заполнения кода*</span><span class="sxs-lookup"><span data-stu-id="85f77-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="85f77-272">Откройте **Global.asax.cs** файл и добавьте следующие *с помощью* инструкций.</span><span class="sxs-lookup"><span data-stu-id="85f77-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="85f77-273">(Code Snippet - *модели и доступ к данным — глобальный Asax директивы Ex2*)</span><span class="sxs-lookup"><span data-stu-id="85f77-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="85f77-274">В **приложения\_Start()** метода добавьте следующую строку для задания инициализации базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="85f77-275">(Code Snippet - *модели и доступ к данным — глобальный Asax SetInitializer Ex2*)</span><span class="sxs-lookup"><span data-stu-id="85f77-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="85f77-276">Задача 2 - Настройка подключения к базе данных</span><span class="sxs-lookup"><span data-stu-id="85f77-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="85f77-277">Теперь, когда вы уже добавили в наш проект базы данных, вы напишете **Web.config** файл строку подключения.</span><span class="sxs-lookup"><span data-stu-id="85f77-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="85f77-278">Добавьте строку подключения в **Web.config**. Чтобы сделать это, откройте **Web.config** в корневую папку проекта и замените строку подключения с именем DefaultConnection со следующей строки в **&lt;connectionStrings&gt;** раздел:</span><span class="sxs-lookup"><span data-stu-id="85f77-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="85f77-279">![Расположение файла Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "расположение файла Web.config")</span><span class="sxs-lookup"><span data-stu-id="85f77-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="85f77-280">*Расположение файла Web.config*</span><span class="sxs-lookup"><span data-stu-id="85f77-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="85f77-281">Задача 3 - Работа с моделью</span><span class="sxs-lookup"><span data-stu-id="85f77-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="85f77-282">Теперь, когда вы уже настроили подключение к базе данных, вы свяжете модели с таблицами базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="85f77-283">В этой задаче будет создан класс, который будет связан в базу данных в режиме Code First.</span><span class="sxs-lookup"><span data-stu-id="85f77-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="85f77-284">Обратите внимание, что существующий класс модели POCO, который должен быть изменен.</span><span class="sxs-lookup"><span data-stu-id="85f77-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="85f77-285">Если вы выполнили упражнении 1, можно заметить, что это было выполнено мастером.</span><span class="sxs-lookup"><span data-stu-id="85f77-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="85f77-286">Выполняя Code First, будет вручную создать классы, которые будут связаны с сущностями данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="85f77-287">Откройте класс POCO модели **жанр** из **моделей** папку проекта и включают идентификатор.</span><span class="sxs-lookup"><span data-stu-id="85f77-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="85f77-288">Используйте свойство int с именем **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="85f77-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="85f77-289">(Code Snippet - *модели и доступ к данным — первый жанр Ex2 код*)</span><span class="sxs-lookup"><span data-stu-id="85f77-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="85f77-290">Для работы с соглашения Code First, этот класс жанр должен иметь свойство первичного ключа, которые определяются автоматически.</span><span class="sxs-lookup"><span data-stu-id="85f77-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="85f77-291">Дополнительные сведения о первой соглашения о написании кода, в этом [статье msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="85f77-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="85f77-292">Теперь откройте класс POCO модели **альбома** из **моделей** папку проекта и включить внешние ключи, создать свойства с именами **GenreId** и  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="85f77-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="85f77-293">Этот класс уже есть **GenreId** для первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="85f77-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="85f77-294">(Code Snippet - *модели и доступ к данным — первый альбома Ex2 код*)</span><span class="sxs-lookup"><span data-stu-id="85f77-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="85f77-295">Откройте класс POCO модели **исполнителя** и включают **ArtistId** свойство.</span><span class="sxs-lookup"><span data-stu-id="85f77-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="85f77-296">(Code Snippet - *модели и доступ к данным — первый исполнителя Ex2 код*)</span><span class="sxs-lookup"><span data-stu-id="85f77-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="85f77-297">Щелкните правой кнопкой мыши **моделей** папку проекта и выберите **Add | Класс**.</span><span class="sxs-lookup"><span data-stu-id="85f77-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="85f77-298">Назовите файл **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="85f77-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="85f77-299">Щелкните **Add.**</span><span class="sxs-lookup"><span data-stu-id="85f77-299">Then, click **Add.**</span></span>

    <span data-ttu-id="85f77-300">![Добавление класса](aspnet-mvc-4-models-and-data-access/_static/image20.png "Добавление класса")</span><span class="sxs-lookup"><span data-stu-id="85f77-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="85f77-301">*Добавление нового элемента*</span><span class="sxs-lookup"><span data-stu-id="85f77-301">*Adding a new item*</span></span>

    <span data-ttu-id="85f77-302">![Добавление класса class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Добавление класс2")</span><span class="sxs-lookup"><span data-stu-id="85f77-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="85f77-303">*Добавление класса*</span><span class="sxs-lookup"><span data-stu-id="85f77-303">*Adding a class*</span></span>
5. <span data-ttu-id="85f77-304">Откройте класс, созданный ранее, **MusicStoreEntities.cs**и включите пространства имен **System.Data.Entity** и **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="85f77-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="85f77-305">Замените объявление класса для расширения **DbContext** класса: Объявите общую **DBSet** и переопределить **OnModelCreating** метод.</span><span class="sxs-lookup"><span data-stu-id="85f77-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="85f77-306">После выполнения этого шага вы получите доменного класса, который будет связан модели с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="85f77-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="85f77-307">Для этого замените код класса следующим:</span><span class="sxs-lookup"><span data-stu-id="85f77-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="85f77-308">(Code Snippet - *модели и доступ к данным — первый MusicStoreEntities Ex2 код*)</span><span class="sxs-lookup"><span data-stu-id="85f77-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="85f77-309">С помощью Entity Framework **DbContext** и **DBSet** можно запросить класс POCO жанра.</span><span class="sxs-lookup"><span data-stu-id="85f77-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="85f77-310">Путем расширения **OnModelCreating** метод, вы задаете **кода** как жанр сопоставляется с таблицей базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="85f77-311">Дополнительные сведения о DBContext и DBSet можно найти в этой статье msdn: [ссылку](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="85f77-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="85f77-312">Задача 4 - запрос в базу данных</span><span class="sxs-lookup"><span data-stu-id="85f77-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="85f77-313">В этой задаче будет обновите класс StoreController, поэтому, вместо использования жестко закодированные данные, он будет извлекать его из базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="85f77-314">Эта задача является похожая Упражнение 1.</span><span class="sxs-lookup"><span data-stu-id="85f77-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="85f77-315">Если вы выполнили Упражнение 1 можно заметить, эти действия одинаковы в обоих методов (сначала база данных или Code first).</span><span class="sxs-lookup"><span data-stu-id="85f77-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="85f77-316">Они отличаются в том, как данные, связанные с моделью, но еще из контроллера в прозрачный доступ к сущностям данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="85f77-317">Откройте **Controllers\StoreController.cs** и добавьте следующее поле в класс для хранения экземпляра **MusicStoreEntities** класс с именем **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="85f77-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="85f77-318">(Code Snippet - *моделей и доступ к данным — storeDB сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="85f77-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="85f77-319">**MusicStoreEntities** класс предоставляет свойство коллекции для каждой таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="85f77-320">Обновление **Обзор** метода действия для получения жанр фильма со всеми **альбомов**.</span><span class="sxs-lookup"><span data-stu-id="85f77-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="85f77-321">(Code Snippet - *модели и доступ к данным — Обзор Store Ex2*)</span><span class="sxs-lookup"><span data-stu-id="85f77-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="85f77-322">Вы используете функцию версию.NET, называемую **LINQ** (запросы LINQ), для написания выражений запросов со строгой типизацией для этих коллекций — которые будет выполнять код в базе данных и возвращают объекты, что вы можете программировать к.</span><span class="sxs-lookup"><span data-stu-id="85f77-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="85f77-323">Дополнительные сведения о LINQ см. на [сайта msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="85f77-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="85f77-324">Обновление **индекс** метода действия для получения всех жанров.</span><span class="sxs-lookup"><span data-stu-id="85f77-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="85f77-325">(Code Snippet - *модели и доступ к данным — индекс Store Ex2*)</span><span class="sxs-lookup"><span data-stu-id="85f77-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="85f77-326">Обновление **индекс** метода действия, чтобы получить все жанры и преобразования коллекции в список.</span><span class="sxs-lookup"><span data-stu-id="85f77-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="85f77-327">(Code Snippet - *модели и доступ к данным — Ex2 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="85f77-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="85f77-328">Задача 5 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="85f77-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="85f77-329">В этой задаче производится проверка на страницу индекса Store теперь будет отображать жанры, хранящиеся в базе данных, а не жестко закодированное из них.</span><span class="sxs-lookup"><span data-stu-id="85f77-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="85f77-330">Чтобы изменить этот шаблон, так как нет необходимости **StoreController** возвращает же **StoreIndexViewModel** как и раньше, но на этот раз будут извлекаться данные из базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="85f77-331">Перестройте решение и нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="85f77-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="85f77-332">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="85f77-332">The project starts in the Home page.</span></span> <span data-ttu-id="85f77-333">Убедитесь, что в меню **жанров** больше не жестко списка, а данные извлекаются непосредственно из базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="85f77-335">*Просмотр жанры из базы данных*</span><span class="sxs-lookup"><span data-stu-id="85f77-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="85f77-336">Теперь перейдите к любой жанр и убедитесь, что альбомов заполняются из базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="85f77-337">![Просмотр альбомов из базы данных](aspnet-mvc-4-models-and-data-access/_static/image23.png "просмотра альбомов из базы данных")</span><span class="sxs-lookup"><span data-stu-id="85f77-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="85f77-338">*Просмотр альбомов из базы данных*</span><span class="sxs-lookup"><span data-stu-id="85f77-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="85f77-339">Упражнение 3: Запрос в базу данных с параметрами</span><span class="sxs-lookup"><span data-stu-id="85f77-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="85f77-340">В этом упражнении вы узнаете, как запросить базу данных с помощью параметров и как использовать формирование результатов запроса, это функция, которая уменьшает число базы данных обращается к извлечения данных в более эффективный способ.</span><span class="sxs-lookup"><span data-stu-id="85f77-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="85f77-341">Дополнительные сведения о формировании результат запроса, посетите следующий [статье msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="85f77-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="85f77-342">Задача 1 - изменение StoreController получить альбомов из базы данных</span><span class="sxs-lookup"><span data-stu-id="85f77-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="85f77-343">В этой задаче будет изменено **StoreController** класс для доступа к базе данных для получения альбомов из определенного жанра.</span><span class="sxs-lookup"><span data-stu-id="85f77-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="85f77-344">Откройте **начать** решений, расположенный **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** папку, если вы хотите использовать подход Code-First или **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** папку, если вы хотите использовать подход Database First.</span><span class="sxs-lookup"><span data-stu-id="85f77-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="85f77-345">В противном случае можно продолжить использование **окончания** решение получен путем выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="85f77-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="85f77-346">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="85f77-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="85f77-347">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="85f77-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="85f77-348">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="85f77-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="85f77-349">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="85f77-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="85f77-350">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="85f77-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="85f77-351">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="85f77-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="85f77-352">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="85f77-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="85f77-353">Откройте **StoreController** классе, чтобы изменить **Обзор** метода действия.</span><span class="sxs-lookup"><span data-stu-id="85f77-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="85f77-354">Для этого в **обозревателе решений**, разверните **контроллеров** папку и дважды щелкните **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="85f77-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="85f77-355">Изменение **Обзор** метода действия для получения альбомов для определенного жанра.</span><span class="sxs-lookup"><span data-stu-id="85f77-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="85f77-356">Чтобы сделать это, замените следующий код:</span><span class="sxs-lookup"><span data-stu-id="85f77-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="85f77-357">(Code Snippet - *модели и доступ к данным — Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="85f77-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="85f77-358">Чтобы заполнить коллекцию сущностей, необходимо использовать **Include** метод, чтобы указать, требуется извлечь альбомов слишком.</span><span class="sxs-lookup"><span data-stu-id="85f77-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="85f77-359">Можно использовать. **Single()** расширения в LINQ, так как в этом случае ожидается только один жанр, альбома.</span><span class="sxs-lookup"><span data-stu-id="85f77-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="85f77-360">**Single()** метод принимает лямбда-выражение как параметр, указывающий в этом случае объект жанр таким образом, чтобы его имя совпадает со значением, который определен.</span><span class="sxs-lookup"><span data-stu-id="85f77-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="85f77-361">Будет воспользоваться преимуществами функцию, которая позволяет указывать другие связанные сущности, которые требуется также загружать при извлечении объекта жанра.</span><span class="sxs-lookup"><span data-stu-id="85f77-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="85f77-362">Эта функция называется **формирование результата запроса**и позволяет сократить количество раз, необходимые для доступа к базе данных для получения сведений.</span><span class="sxs-lookup"><span data-stu-id="85f77-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="85f77-363">В этом случае требуется упреждающую выборку альбомов для жанра, извлечении.</span><span class="sxs-lookup"><span data-stu-id="85f77-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="85f77-364">Запрос включает **Genres.Include (&quot;альбомов&quot;)** чтобы указать, что вы хотите также связанные альбомы.</span><span class="sxs-lookup"><span data-stu-id="85f77-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="85f77-365">Это приведет к более эффективной работе приложения, так как он извлечет жанр и дисках данных в запросе отдельной базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="85f77-366">Задача 2 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="85f77-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="85f77-367">В этой задаче будет запустите приложение и получить фотоальбомы конкретный жанр, выбранный из базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="85f77-368">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="85f77-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="85f77-369">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="85f77-369">The project starts in the Home page.</span></span> <span data-ttu-id="85f77-370">Изменить URL-адрес **/Store/обзора? жанр = Pop** чтобы убедиться, что результаты извлекаются из базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="85f77-371">![Просмотр по жанру](aspnet-mvc-4-models-and-data-access/_static/image24.png "Просмотр по жанру")</span><span class="sxs-lookup"><span data-stu-id="85f77-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="85f77-372">*Просмотр/Store/обзора? жанр = Pop*</span><span class="sxs-lookup"><span data-stu-id="85f77-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="85f77-373">Задача 3 - доступ к альбомам, по идентификатору</span><span class="sxs-lookup"><span data-stu-id="85f77-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="85f77-374">В этой задаче будет повторяться предыдущей процедуре, чтобы получить альбомов по их идентификаторам.</span><span class="sxs-lookup"><span data-stu-id="85f77-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="85f77-375">Закройте браузер, при необходимости, чтобы вернуться в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="85f77-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="85f77-376">Откройте **StoreController** классе, чтобы изменить **сведения** метода действия.</span><span class="sxs-lookup"><span data-stu-id="85f77-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="85f77-377">Для этого в **обозревателе решений**, разверните **контроллеров** папку и дважды щелкните **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="85f77-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="85f77-378">Изменение **сведения** на основе метода действия для получения сведений о альбомов их **идентификатор**. Чтобы сделать это, замените следующий код:</span><span class="sxs-lookup"><span data-stu-id="85f77-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="85f77-379">(Code Snippet - *модели и доступ к данным — Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="85f77-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="85f77-380">Задача 4 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="85f77-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="85f77-381">В этой задаче вы запустите приложение в веб-браузер и получить сведения об альбоме по их идентификаторам.</span><span class="sxs-lookup"><span data-stu-id="85f77-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="85f77-382">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="85f77-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="85f77-383">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="85f77-383">The project starts in the Home page.</span></span> <span data-ttu-id="85f77-384">Изменить URL-адрес **/Store/Details/51** или Обзор жанры и выберите альбом, чтобы убедиться, что результаты извлекаются из базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="85f77-385">![Просмотр сведений о](aspnet-mvc-4-models-and-data-access/_static/image25.png "Просмотр сведений")</span><span class="sxs-lookup"><span data-stu-id="85f77-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="85f77-386">*Просмотр /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="85f77-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="85f77-387">Кроме того, можно развернуть это приложение на веб-сайтов Windows Azure ниже [приложении B: публикации приложения ASP.NET MVC 4 с помощью веб-развертывания](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="85f77-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="85f77-388">Сводка</span><span class="sxs-lookup"><span data-stu-id="85f77-388">Summary</span></span>

<span data-ttu-id="85f77-389">С этого занятия практических, вы узнали Основы модели ASP.NET MVC и доступа к данным, с помощью **Database First** подход, а также **Code First** подход:</span><span class="sxs-lookup"><span data-stu-id="85f77-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="85f77-390">Как добавить базу данных в решение для обработки данных</span><span class="sxs-lookup"><span data-stu-id="85f77-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="85f77-391">Обновление контроллеров для предоставления шаблонов представлений с данными, извлеченными из базы данных, а не жестко</span><span class="sxs-lookup"><span data-stu-id="85f77-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="85f77-392">Как запросить базу данных с помощью параметров</span><span class="sxs-lookup"><span data-stu-id="85f77-392">How to query the database using parameters</span></span>
- <span data-ttu-id="85f77-393">Как использовать запрос результат формирования, это функция, которая уменьшает количество обращений к базе данных, извлечение данных в более эффективный способ</span><span class="sxs-lookup"><span data-stu-id="85f77-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="85f77-394">Способы использования подхода Database First и Code First в Microsoft Entity Framework для связи с базой данных, с помощью модели</span><span class="sxs-lookup"><span data-stu-id="85f77-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="85f77-395">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="85f77-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="85f77-396">Вы можете установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию с помощью **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="85f77-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="85f77-397">Приведенные ниже инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="85f77-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="85f77-398">Перейдите к [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="85f77-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="85f77-399">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="85f77-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="85f77-400">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="85f77-400">Click on **Install Now**.</span></span> <span data-ttu-id="85f77-401">Если у вас нет **установщика веб-платформы** вы будете перенаправлены к сначала загрузить и установить его.</span><span class="sxs-lookup"><span data-stu-id="85f77-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="85f77-402">Один раз **установщика веб-платформы** открыт, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="85f77-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="85f77-403">![Установка Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="85f77-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="85f77-404">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="85f77-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="85f77-405">Прочтите лицензии и условия все продукты и нажмите кнопку **я принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="85f77-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензии](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="85f77-407">*Принятие условий лицензии*</span><span class="sxs-lookup"><span data-stu-id="85f77-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="85f77-408">Подождите, пока не завершится процесс загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="85f77-408">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="85f77-410">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="85f77-410">*Installation progress*</span></span>
6. <span data-ttu-id="85f77-411">После завершения установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="85f77-411">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="85f77-413">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="85f77-413">*Installation completed*</span></span>
7. <span data-ttu-id="85f77-414">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="85f77-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="85f77-415">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и Займитесь написанием &quot; **VS Express**&quot;, нажмите кнопку на **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="85f77-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="85f77-417">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="85f77-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="85f77-418">Приложение б. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="85f77-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="85f77-419">В этом приложении показано, как создать новый веб-сайт на портале управления Windows Azure и опубликовать приложение, полученный после лаборатории, используя преимущества компонентов публикации веб-развертывания, предоставляемые Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="85f77-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="85f77-420">Задача 1 - Создание нового веб-сайта с Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="85f77-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="85f77-421">Перейдите к [портала управления Windows Azure](https://manage.windowsazure.com/) и войдите с использованием учетных данных Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="85f77-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="85f77-422">С помощью Windows Azure можно бесплатное размещение 10 веб-сайтов ASP.NET и затем масштабировать по мере увеличения объема трафика.</span><span class="sxs-lookup"><span data-stu-id="85f77-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="85f77-423">Вы можете зарегистрироваться [здесь](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="85f77-423">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="85f77-424">![Войдите на портал Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Войдите на портал Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="85f77-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="85f77-425">*Войдите на портал управления Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="85f77-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="85f77-426">Нажмите кнопку **New** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="85f77-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="85f77-427">![Создание нового веб-сайта](aspnet-mvc-4-models-and-data-access/_static/image32.png "создания нового веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="85f77-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="85f77-428">*Создание нового веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="85f77-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="85f77-429">Нажмите кнопку **вычислений** | **веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="85f77-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="85f77-430">Затем выберите **быстрое создание** параметр.</span><span class="sxs-lookup"><span data-stu-id="85f77-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="85f77-431">Укажите URL-адрес доступен для нового веб-сайта и нажмите кнопку **создать веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="85f77-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="85f77-432">Веб-сайта Windows Azure является узлом для веб-приложение, работающее в облаке, вы можете управлять.</span><span class="sxs-lookup"><span data-stu-id="85f77-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="85f77-433">Быстрое создание позволяет развернуть завершенное веб-приложения для Windows Azure веб-сайта из вне портала.</span><span class="sxs-lookup"><span data-stu-id="85f77-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="85f77-434">Он не включает действия по настройке базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="85f77-435">![Создание нового веб-сайта, с помощью быстрого создания](aspnet-mvc-4-models-and-data-access/_static/image33.png "создания нового веб-сайта, с помощью быстрого создания")</span><span class="sxs-lookup"><span data-stu-id="85f77-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="85f77-436">*Создание нового веб-сайта, с помощью быстрого создания*</span><span class="sxs-lookup"><span data-stu-id="85f77-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="85f77-437">Подождите, пока новый **веб-сайт** создается.</span><span class="sxs-lookup"><span data-stu-id="85f77-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="85f77-438">После создания веб-сайт, щелкните ссылку под **URL-адрес** столбца.</span><span class="sxs-lookup"><span data-stu-id="85f77-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="85f77-439">Проверьте, что новый веб-сайт работает.</span><span class="sxs-lookup"><span data-stu-id="85f77-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="85f77-440">![Выбрав новый веб-сайт](aspnet-mvc-4-models-and-data-access/_static/image34.png "просмотра на новый веб-сайт")</span><span class="sxs-lookup"><span data-stu-id="85f77-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="85f77-441">*Выбрав новый веб-сайт*</span><span class="sxs-lookup"><span data-stu-id="85f77-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="85f77-442">![Веб-сайт работал](aspnet-mvc-4-models-and-data-access/_static/image35.png "веб-узлом")</span><span class="sxs-lookup"><span data-stu-id="85f77-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="85f77-443">*Веб-сайт под управлением*</span><span class="sxs-lookup"><span data-stu-id="85f77-443">*Web site running*</span></span>
6. <span data-ttu-id="85f77-444">Вернитесь на портал и щелкните имя веб-сайт в **имя** столбец для отображения страницы управления.</span><span class="sxs-lookup"><span data-stu-id="85f77-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="85f77-445">![Открытие страницы управления веб-сайт](aspnet-mvc-4-models-and-data-access/_static/image36.png "Открытие страницы управления веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="85f77-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="85f77-446">*Открытие страницы управления веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="85f77-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="85f77-447">В **панели мониторинга** раздела **быстрый обзор** щелкните **загрузить профиль публикации** ссылку.</span><span class="sxs-lookup"><span data-stu-id="85f77-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="85f77-448">*Профиль публикации* содержит все сведения, необходимые для публикации веб-приложения на веб-сайт Windows Azure для каждого разрешенного метода публикации.</span><span class="sxs-lookup"><span data-stu-id="85f77-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="85f77-449">Профиль публикации содержит URL-адреса, учетные данные пользователя и строк базы данных, необходимых для подключения и проверку подлинности в каждой из конечных точек, для которых включена метода публикации.</span><span class="sxs-lookup"><span data-stu-id="85f77-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="85f77-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express для Web** и **Microsoft Visual Studio 2012** поддерживают чтение профилей публикации для автоматизации настройки из этих программ для Публикация веб-приложений на веб-сайтах Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="85f77-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="85f77-451">![Профиль публикации веб-сайт загрузки](aspnet-mvc-4-models-and-data-access/_static/image37.png "профиль публикации веб-сайта загрузки")</span><span class="sxs-lookup"><span data-stu-id="85f77-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="85f77-452">*Профиль публикации веб-сайта загрузки*</span><span class="sxs-lookup"><span data-stu-id="85f77-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="85f77-453">Скачайте файл профиля публикации в известном месте.</span><span class="sxs-lookup"><span data-stu-id="85f77-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="85f77-454">Далее в этом упражнении показано, как использовать этот файл для публикации веб-приложения на веб-сайтах, Windows Azure из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="85f77-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="85f77-455">![Сохранение файла профиля публикации](aspnet-mvc-4-models-and-data-access/_static/image38.png "Сохранение профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="85f77-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="85f77-456">*Сохранение файла профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="85f77-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="85f77-457">Задача 2 - Настройка сервера базы данных</span><span class="sxs-lookup"><span data-stu-id="85f77-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="85f77-458">Если приложение использует SQL Server, баз данных, необходимо будет создать сервер базы данных SQL.</span><span class="sxs-lookup"><span data-stu-id="85f77-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="85f77-459">Эту задачу может пропустить, если вы хотите развернуть простое приложение, которое не использует SQL Server.</span><span class="sxs-lookup"><span data-stu-id="85f77-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="85f77-460">Вам потребуется сервер базы данных SQL для хранения базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="85f77-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="85f77-461">Серверы баз данных SQL можно просмотреть в своей подписке на портале управления Windows Azure в **баз данных Sql** | **серверы** | **сервера Панель мониторинга**.</span><span class="sxs-lookup"><span data-stu-id="85f77-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="85f77-462">Если у вас на сервер, созданный, можно создать его, используя **добавить** кнопки на панели команд.</span><span class="sxs-lookup"><span data-stu-id="85f77-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="85f77-463">Запишите **имя сервера и URL-адрес, имя входа администратора и пароль**, как их использование в следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="85f77-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="85f77-464">Не создавайте базы данных, так как он будет создан в более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="85f77-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="85f77-465">![Панель мониторинга базы данных SQL Server](aspnet-mvc-4-models-and-data-access/_static/image39.png "панель мониторинга базы данных SQL Server")</span><span class="sxs-lookup"><span data-stu-id="85f77-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="85f77-466">*Панель мониторинга базы данных SQL Server*</span><span class="sxs-lookup"><span data-stu-id="85f77-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="85f77-467">В следующей задаче вы проверите подключения к базе данных из Visual Studio по этой причине необходимо включить IP-адрес локального сервера списка **разрешенные IP-адреса**.</span><span class="sxs-lookup"><span data-stu-id="85f77-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="85f77-468">Чтобы сделать это, нажмите кнопку **Настройка**, выберите IP-адрес из **текущий IP-адрес клиента** и вставьте его на **начальный IP-адрес** и **конечный IP-адрес** текстовые поля и нажмите кнопку ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) кнопки.</span><span class="sxs-lookup"><span data-stu-id="85f77-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Добавление IP-адрес клиента](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="85f77-470">*Добавление IP-адрес клиента*</span><span class="sxs-lookup"><span data-stu-id="85f77-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="85f77-471">Один раз **IP-адрес клиента** добавляется разрешенных IP-адресов выберите на **Сохранить** для подтверждения изменения.</span><span class="sxs-lookup"><span data-stu-id="85f77-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Подтверждение изменений](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="85f77-473">*Подтверждение изменений*</span><span class="sxs-lookup"><span data-stu-id="85f77-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="85f77-474">Задача 3 - публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="85f77-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="85f77-475">Вернитесь к решению для ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="85f77-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="85f77-476">В **обозревателе решений**, щелкните правой кнопкой мыши проект веб-сайта и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="85f77-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="85f77-477">![Публикация приложения](aspnet-mvc-4-models-and-data-access/_static/image43.png "публикации приложения")</span><span class="sxs-lookup"><span data-stu-id="85f77-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="85f77-478">*Публикация веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="85f77-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="85f77-479">Импортируйте профиль публикации, сохраненный в первой задаче.</span><span class="sxs-lookup"><span data-stu-id="85f77-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="85f77-480">![Импорт профиля публикации](aspnet-mvc-4-models-and-data-access/_static/image44.png "Импорт профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="85f77-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="85f77-481">*Импорт профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="85f77-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="85f77-482">Нажмите кнопку **Проверка подключения**.</span><span class="sxs-lookup"><span data-stu-id="85f77-482">Click **Validate Connection**.</span></span> <span data-ttu-id="85f77-483">После завершения проверки нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="85f77-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="85f77-484">Проверка будет завершена, как только вы увидите зеленый флажок отображаться рядом с кнопкой проверить подключение.</span><span class="sxs-lookup"><span data-stu-id="85f77-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="85f77-485">![Проверка подключения](aspnet-mvc-4-models-and-data-access/_static/image45.png "Проверка подключения")</span><span class="sxs-lookup"><span data-stu-id="85f77-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="85f77-486">*Проверка подключения*</span><span class="sxs-lookup"><span data-stu-id="85f77-486">*Validating connection*</span></span>
4. <span data-ttu-id="85f77-487">В **параметры** раздела **баз данных** нажмите кнопку рядом с текстовое поле подключения к базе данных (т. е. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="85f77-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="85f77-488">![Конфигурация веб-развертывания](aspnet-mvc-4-models-and-data-access/_static/image46.png "конфигурация веб-развертывания")</span><span class="sxs-lookup"><span data-stu-id="85f77-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="85f77-489">*Конфигурация веб-развертывания*</span><span class="sxs-lookup"><span data-stu-id="85f77-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="85f77-490">Настройте подключение к базе данных следующим образом:</span><span class="sxs-lookup"><span data-stu-id="85f77-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="85f77-491">В **имя_сервера** введите URL-адреса серверу базы данных SQL с помощью *tcp:* префикс.</span><span class="sxs-lookup"><span data-stu-id="85f77-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="85f77-492">В **имя пользователя** введите имя входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="85f77-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="85f77-493">В **пароль** введите пароль входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="85f77-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="85f77-494">Введите новое имя базы данных.</span><span class="sxs-lookup"><span data-stu-id="85f77-494">Type a new database name.</span></span>

     <span data-ttu-id="85f77-495">![Настройка строки подключения назначения](aspnet-mvc-4-models-and-data-access/_static/image47.png "Настройка строка соединения с назначением")</span><span class="sxs-lookup"><span data-stu-id="85f77-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="85f77-496">*Настройка строки подключения назначения*</span><span class="sxs-lookup"><span data-stu-id="85f77-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="85f77-497">Затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="85f77-497">Then click **OK**.</span></span> <span data-ttu-id="85f77-498">При появлении запроса на создание базы данных нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="85f77-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="85f77-499">![Создание базы данных](aspnet-mvc-4-models-and-data-access/_static/image48.png "Создание строки базы данных")</span><span class="sxs-lookup"><span data-stu-id="85f77-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="85f77-500">*Создание базы данных*</span><span class="sxs-lookup"><span data-stu-id="85f77-500">*Creating the database*</span></span>
7. <span data-ttu-id="85f77-501">В текстовое поле подключение по умолчанию отображается строка подключения, который будет использоваться для подключения к базе данных SQL в Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="85f77-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="85f77-502">Затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="85f77-502">Then click **Next**.</span></span>

    <span data-ttu-id="85f77-503">![Строка подключения, указывающий на базу данных SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "строку подключения, указывающий на базу данных SQL")</span><span class="sxs-lookup"><span data-stu-id="85f77-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="85f77-504">*Строка подключения, указывающий на базу данных SQL*</span><span class="sxs-lookup"><span data-stu-id="85f77-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="85f77-505">В **предварительной версии** щелкните **публикации**.</span><span class="sxs-lookup"><span data-stu-id="85f77-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="85f77-506">![Публикация веб-приложения](aspnet-mvc-4-models-and-data-access/_static/image50.png "публикации веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="85f77-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="85f77-507">*Публикация веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="85f77-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="85f77-508">После завершения процесса публикации в браузере по умолчанию откроется опубликованного веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="85f77-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="85f77-509">Приложение c. фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="85f77-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="85f77-510">С помощью фрагментов кода у вас есть весь код, который требуется в вашем распоряжении.</span><span class="sxs-lookup"><span data-stu-id="85f77-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="85f77-511">Лаборатории документ поможет определить точно при их использовании, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="85f77-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="85f77-512">![Фрагменты кода Visual Studio, чтобы вставить код в проект](aspnet-mvc-4-models-and-data-access/_static/image51.png "фрагменты кода с помощью Visual Studio, чтобы вставить код в проект")</span><span class="sxs-lookup"><span data-stu-id="85f77-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="85f77-513">*Фрагменты кода Visual Studio, чтобы вставить код в проект*</span><span class="sxs-lookup"><span data-stu-id="85f77-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="85f77-514">***Чтобы добавить фрагмент кода, с помощью клавиатуры (только C#)***</span><span class="sxs-lookup"><span data-stu-id="85f77-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="85f77-515">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="85f77-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="85f77-516">Начните вводить имя фрагмента (без пробелов и дефисов).</span><span class="sxs-lookup"><span data-stu-id="85f77-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="85f77-517">Посмотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="85f77-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="85f77-518">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="85f77-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="85f77-519">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="85f77-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="85f77-520">![Начните вводить имя фрагмента](aspnet-mvc-4-models-and-data-access/_static/image52.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="85f77-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="85f77-521">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="85f77-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="85f77-522">![Нажмите клавишу Tab, чтобы выделить фрагмент в выделенный](aspnet-mvc-4-models-and-data-access/_static/image53.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="85f77-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="85f77-523">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="85f77-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="85f77-524">![Снова нажмите клавишу Tab и фрагмент будет расширяться](aspnet-mvc-4-models-and-data-access/_static/image54.png "еще раз нажмите клавишу Tab и фрагмент будет расширяться")</span><span class="sxs-lookup"><span data-stu-id="85f77-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="85f77-525">*Снова нажмите клавишу Tab и фрагмент будет расширяться*</span><span class="sxs-lookup"><span data-stu-id="85f77-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="85f77-526">***Чтобы добавить фрагмент кода, с помощью мыши (C#, Visual Basic и XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="85f77-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="85f77-527">Щелкните правой кнопкой мыши место для вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="85f77-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="85f77-528">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="85f77-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="85f77-529">Выберите соответствующий фрагмент из списка, щелкнув его.</span><span class="sxs-lookup"><span data-stu-id="85f77-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="85f77-530">![Щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент](aspnet-mvc-4-models-and-data-access/_static/image55.png "щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент")</span><span class="sxs-lookup"><span data-stu-id="85f77-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="85f77-531">*Щелкните правой кнопкой мыши место для вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="85f77-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="85f77-532">![Выберите соответствующий фрагмент из списка, щелкнув по ней](aspnet-mvc-4-models-and-data-access/_static/image56.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="85f77-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="85f77-533">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="85f77-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
