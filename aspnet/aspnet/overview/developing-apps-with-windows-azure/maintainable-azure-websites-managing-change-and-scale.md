---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Практическое лабораторное занятие: простым в обслуживании веб-сайты Azure: управление изменениями и масштабированием | Документация Майкрософт'
author: rick-anderson
description: В этой лабораторной работы Узнайте, как Microsoft Azure позволяет легко создавать и развертывать веб-сайтов в рабочей среде.
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 3a118cdd7e3f3878976e4f8480ce2236b8d3ba88
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824800"
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a><span data-ttu-id="d6567-103">Практическое лабораторное занятие: простым в обслуживании веб-сайты Azure: управление изменениями и масштабированием</span><span class="sxs-lookup"><span data-stu-id="d6567-103">Hands on Lab: Maintainable Azure Websites: Managing Change and Scale</span></span>
====================
<span data-ttu-id="d6567-104">по [Web Слышатся Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="d6567-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="d6567-105">Скачайте комплект учебных материалов по лагеря Web</span><span class="sxs-lookup"><span data-stu-id="d6567-105">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="d6567-106">Microsoft Azure позволяет легко создавать и развертывать веб-сайтов в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="d6567-106">Microsoft Azure makes it easy to build and deploy websites to production.</span></span> <span data-ttu-id="d6567-107">Но вы не закончили при динамической приложения, вы только начинаете работу!</span><span class="sxs-lookup"><span data-stu-id="d6567-107">But you're not done when your application is live, you're just getting started!</span></span> <span data-ttu-id="d6567-108">Нужно будет обработать изменение требования, обновления базы данных, масштабирования и многое другое.</span><span class="sxs-lookup"><span data-stu-id="d6567-108">You'll need to handle changing requirements, database updates, scale, and more.</span></span> <span data-ttu-id="d6567-109">К счастью служба приложений Azure реализована в рамках, множество функций, которые помогут вам работать с ними бесперебойной работы.</span><span class="sxs-lookup"><span data-stu-id="d6567-109">Fortunately, Azure App Service has you covered, with plenty of features to help you keep your sites running smoothly.</span></span>
> 
> <span data-ttu-id="d6567-110">Azure предлагает безопасные и гибкие возможности разработки, развертывания и масштабирования для веб-приложений любого размера.</span><span class="sxs-lookup"><span data-stu-id="d6567-110">Azure offers secure and flexible development, deployment and scaling options for any size web application.</span></span> <span data-ttu-id="d6567-111">Используйте существующие средства создания и развертывания приложений, не заботясь об управлении инфраструктурой.</span><span class="sxs-lookup"><span data-stu-id="d6567-111">Leverage your existing tools to create and deploy applications without the hassle of managing infrastructure.</span></span>
> 
> <span data-ttu-id="d6567-112">Рабочее веб-приложение самостоятельно Подготовьте в минутах, простого развертывания содержимого, созданного с помощью привычного средства разработки.</span><span class="sxs-lookup"><span data-stu-id="d6567-112">Provision a production web application yourself in minutes by easily deploying content created using your favorite development tool.</span></span> <span data-ttu-id="d6567-113">Можно развернуть существующий сайт напрямую из системы управления версиями с поддержкой **Git**, **GitHub**, **Bitbucket**, **TFS**и даже  **DropBox**.</span><span class="sxs-lookup"><span data-stu-id="d6567-113">You can deploy an existing site directly from source control with support for **Git**, **GitHub**, **Bitbucket**, **TFS**, and even **DropBox**.</span></span> <span data-ttu-id="d6567-114">Развертывание непосредственно из избранного интерфейса IDE или скриптов, используя **PowerShell** в Windows или **CLI** средств, работающих в любой операционной системе.</span><span class="sxs-lookup"><span data-stu-id="d6567-114">Deploy directly from your favorite IDE or from scripts using **PowerShell** in Windows or **CLI** tools running on any OS.</span></span> <span data-ttu-id="d6567-115">После развертывания, своевременно веб-сайтов постоянно с поддержкой для непрерывного развертывания.</span><span class="sxs-lookup"><span data-stu-id="d6567-115">Once deployed, keep your sites constantly up-to-date with support for continuous deployment.</span></span>
> 
> <span data-ttu-id="d6567-116">Azure предоставляет масштабируемые, надежные облачные хранилища, архивации и восстановления любых данных, больших или малых.</span><span class="sxs-lookup"><span data-stu-id="d6567-116">Azure provides scalable, durable cloud storage, backup, and recovery solutions for any data, big or small.</span></span> <span data-ttu-id="d6567-117">При развертывании приложений в рабочей среде, службы хранилища, такие как таблицы, большие двоичные объекты и базы данных SQL, позволяют масштабировать приложения в облаке.</span><span class="sxs-lookup"><span data-stu-id="d6567-117">When deploying applications to a production environment, storage services such as Tables, Blobs and SQL Databases, help you scale your application in the cloud.</span></span>
> 
> <span data-ttu-id="d6567-118">В базах данных SQL важно поддерживать актуальность производительность базы данных при развертывании новых версий приложения.</span><span class="sxs-lookup"><span data-stu-id="d6567-118">With SQL Databases, it is important to keep your productive database up-to-date when deploying new versions of your application.</span></span> <span data-ttu-id="d6567-119">Выражаем благодарность **Entity Framework Code First Migrations**, разработку и развертывание модели данных был упрощен для обновления среды за несколько минут.</span><span class="sxs-lookup"><span data-stu-id="d6567-119">Thanks to **Entity Framework Code First Migrations**, the development and deployment of your data model has been simplified to update your environments in minutes.</span></span> <span data-ttu-id="d6567-120">Это Практическое занятие покажу различные темы, которые могут возникнуть при развертывании веб-приложения в рабочую среду в Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d6567-120">This hands-on lab will show you the different topics you could encounter when deploying your web app to production environments in Microsoft Azure.</span></span>
> 
> <span data-ttu-id="d6567-121">Все примеры кода и фрагменты кода включены в Web лагеря комплект обучающих материалов, доступных в [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="d6567-121">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>
> 
> <span data-ttu-id="d6567-122">Более подробные сведения в этом разделе, см. в разделе [Создание реальных облачных приложений в Azure электронная книга](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d6567-122">For more in-depth coverage of this topic, see the [Building Real-World Cloud Apps with Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="d6567-123">Обзор</span><span class="sxs-lookup"><span data-stu-id="d6567-123">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="d6567-124">Цели</span><span class="sxs-lookup"><span data-stu-id="d6567-124">Objectives</span></span>

<span data-ttu-id="d6567-125">В этой практической работу вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="d6567-125">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="d6567-126">Включить миграций Entity Framework с использованием существующей модели</span><span class="sxs-lookup"><span data-stu-id="d6567-126">Enable Entity Framework Migrations with an existing model</span></span>
- <span data-ttu-id="d6567-127">Обновите модель объектов и базы данных соответствующим образом с помощью миграций Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d6567-127">Update the object model and database accordingly using Entity Framework Migrations</span></span>
- <span data-ttu-id="d6567-128">Развертывание в службе приложений Azure с помощью Git</span><span class="sxs-lookup"><span data-stu-id="d6567-128">Deploy to Azure App Service using Git</span></span>
- <span data-ttu-id="d6567-129">Откат до предыдущего развертывания с помощью портала управления Azure</span><span class="sxs-lookup"><span data-stu-id="d6567-129">Rollback to a previous deployment using the Azure Management portal</span></span>
- <span data-ttu-id="d6567-130">Использование хранилища Azure для масштабирования веб-приложения</span><span class="sxs-lookup"><span data-stu-id="d6567-130">Use Azure Storage to scale a web app</span></span>
- <span data-ttu-id="d6567-131">Настройка автоматического масштабирования для веб-приложения с помощью портала управления Azure</span><span class="sxs-lookup"><span data-stu-id="d6567-131">Configure auto-scaling for a web app using the Azure Management Portal</span></span>
- <span data-ttu-id="d6567-132">Создание и настройка проекта нагрузочного тестирования в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6567-132">Create and configure a load test project in Visual Studio</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="d6567-133">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d6567-133">Prerequisites</span></span>

<span data-ttu-id="d6567-134">Для завершения этой практической работу требуется следующее:</span><span class="sxs-lookup"><span data-stu-id="d6567-134">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="d6567-135">[Visual Studio Express 2013 для Web](https://www.microsoft.com/visualstudio/) или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="d6567-135">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="d6567-136">Пакет Azure SDK для .NET 2.2</span><span class="sxs-lookup"><span data-stu-id="d6567-136">Azure SDK for .NET 2.2</span></span>](https://www.microsoft.com/windowsazure/sdk/)
- [<span data-ttu-id="d6567-137">Система управления версиями GIT</span><span class="sxs-lookup"><span data-stu-id="d6567-137">GIT Version Control System</span></span>](http://git-scm.com/download)
- <span data-ttu-id="d6567-138">Подписка Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="d6567-138">A Microsoft Azure subscription</span></span> 

    - <span data-ttu-id="d6567-139">Зарегистрируйтесь в службе [бесплатной пробной версии](http://aka.ms/watk-freetrial)</span><span class="sxs-lookup"><span data-stu-id="d6567-139">Sign up for a [Free Trial](http://aka.ms/watk-freetrial)</span></span>
    - <span data-ttu-id="d6567-140">Если вы являетесь Visual Studio Professional, Test Professional, Premium или Ultimate с MSDN или MSDN Platforms, активировать ваши [преимущества для подписчиков MSDN](http://aka.ms/watk-msdn) сейчас, чтобы начать разработку и тестирование в Azure</span><span class="sxs-lookup"><span data-stu-id="d6567-140">If you are a Visual Studio Professional, Test Professional, Premium or Ultimate with MSDN or MSDN Platforms subscriber, activate your [MSDN benefit](http://aka.ms/watk-msdn) now to start developing and testing on Azure</span></span>
    - <span data-ttu-id="d6567-141">[BizSpark](http://aka.ms/watk-bizspark) элементы автоматически получать Azure преимущество при приобретении Visual Studio Ultimate с подписками MSDN</span><span class="sxs-lookup"><span data-stu-id="d6567-141">[BizSpark](http://aka.ms/watk-bizspark) members automatically receive the Azure benefit through their Visual Studio Ultimate with MSDN subscriptions</span></span>
    - <span data-ttu-id="d6567-142">Членами [Microsoft Partner Network](http://aka.ms/watk-mpn) программе Cloud Essentials получают ежемесячные кредиты Azure бесплатно</span><span class="sxs-lookup"><span data-stu-id="d6567-142">Members of the [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials program receive monthly Azure credits at no charge</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="d6567-143">Установка</span><span class="sxs-lookup"><span data-stu-id="d6567-143">Setup</span></span>

<span data-ttu-id="d6567-144">Для выполнения упражнений в этой практической работу, необходимо сначала настроить среду.</span><span class="sxs-lookup"><span data-stu-id="d6567-144">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="d6567-145">Откройте Windows Explorer и перейдите к лаборатории **источника** папки.</span><span class="sxs-lookup"><span data-stu-id="d6567-145">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="d6567-146">Щелкните правой кнопкой мыши **Setup.cmd** и выберите **Запуск от имени администратора** для запуска процесса установки, будет настроить среду и установить фрагменты кода Visual Studio для этой лабораторной работы.</span><span class="sxs-lookup"><span data-stu-id="d6567-146">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="d6567-147">Если открыто диалоговое окно контроля учетных записей, подтвердите действие для продолжения.</span><span class="sxs-lookup"><span data-stu-id="d6567-147">If the User Account Control dialog is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="d6567-148">Убедитесь, что все зависимости для этой лабораторной работы вы вернули перед запуском программы установки.</span><span class="sxs-lookup"><span data-stu-id="d6567-148">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="d6567-149">Фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="d6567-149">Using the Code Snippets</span></span>

<span data-ttu-id="d6567-150">В этом документе лаборатории вам будет рекомендовано вставлять блоки кода.</span><span class="sxs-lookup"><span data-stu-id="d6567-150">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="d6567-151">Для удобства основная часть кода предоставляется как фрагменты кода Visual Studio, к которому можно получить из в Visual Studio 2013, чтобы избежать необходимости добавлять ее вручную.</span><span class="sxs-lookup"><span data-stu-id="d6567-151">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="d6567-152">Каждого упражнения сопровождается начальный решения, расположенный в **начать** папку упражнения, чему вы сможете каждого упражнения, независимо от других.</span><span class="sxs-lookup"><span data-stu-id="d6567-152">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="d6567-153">Имейте в виду, что фрагменты кода, которые добавляются во время атаки, отсутствуют эти стартовые решения и могут не работать, пока не будут выполнены упражнения.</span><span class="sxs-lookup"><span data-stu-id="d6567-153">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="d6567-154">В исходном коде для упражнения, вы также найдете **окончания** папку, содержащую решение Visual Studio с кодом, полученный в результате выполнения действий, описанных в соответствующий упражнении.</span><span class="sxs-lookup"><span data-stu-id="d6567-154">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="d6567-155">Эти решения можно использовать в качестве руководства, если вам нужна дополнительная помощь, отвечая на это Практическое занятие.</span><span class="sxs-lookup"><span data-stu-id="d6567-155">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="d6567-156">Упражнения</span><span class="sxs-lookup"><span data-stu-id="d6567-156">Exercises</span></span>

<span data-ttu-id="d6567-157">Это Практическое занятие включает в себя следующие упражнения:</span><span class="sxs-lookup"><span data-stu-id="d6567-157">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="d6567-158">С помощью миграций Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d6567-158">Using Entity Framework Migrations</span></span>](#Exercise1)
2. [<span data-ttu-id="d6567-159">Развертывание веб-приложения в промежуточную среду</span><span class="sxs-lookup"><span data-stu-id="d6567-159">Deploying a Web App to Staging</span></span>](#Exercise2)
3. [<span data-ttu-id="d6567-160">Выполняя откат развертывания в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="d6567-160">Performing Deployment Rollback in Production</span></span>](#Exercise3)
4. [<span data-ttu-id="d6567-161">Масштабирование с помощью службы хранилища Azure</span><span class="sxs-lookup"><span data-stu-id="d6567-161">Scaling Using Azure Storage</span></span>](#Exercise4)
5. <span data-ttu-id="d6567-162">[С помощью автоматического масштабирования для веб-приложений](#Exercise5) (необязательно для Visual Studio 2013 Ultimate edition)</span><span class="sxs-lookup"><span data-stu-id="d6567-162">[Using Autoscale for Web Apps](#Exercise5) (Optional for Visual Studio 2013 Ultimate edition)</span></span>

<span data-ttu-id="d6567-163">Предполагаемое время для выполнения этого лабораторного: **75 минут**</span><span class="sxs-lookup"><span data-stu-id="d6567-163">Estimated time to complete this lab: **75 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="d6567-164">При первом запуске Visual Studio, необходимо выбрать один из предварительно определенных коллекций параметров.</span><span class="sxs-lookup"><span data-stu-id="d6567-164">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="d6567-165">Каждой коллекции предопределенных соответствует конкретному стилю разработки и определяет макетов окон, поведение редактора, фрагменты кода IntelliSense и параметры диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="d6567-165">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="d6567-166">В этом лабораторном занятии описаны действия, необходимые для выполнения данной задачи в Visual Studio при использовании **обычные параметры разработки** коллекции.</span><span class="sxs-lookup"><span data-stu-id="d6567-166">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="d6567-167">При выборе другого набора параметров для среды разработки, возможно, различия в действиях, которые следует учитывать.</span><span class="sxs-lookup"><span data-stu-id="d6567-167">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a><span data-ttu-id="d6567-168">Упражнение 1: С помощью миграций Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d6567-168">Exercise 1: Using Entity Framework Migrations</span></span>

<span data-ttu-id="d6567-169">При разработке приложения, модель данных может измениться с течением времени.</span><span class="sxs-lookup"><span data-stu-id="d6567-169">When you are developing an application, your data model might change over time.</span></span> <span data-ttu-id="d6567-170">Эти изменения могут повлиять на существующей модели базы данных (при создании новой версии), и это важно для обновления базы данных во избежание ошибок.</span><span class="sxs-lookup"><span data-stu-id="d6567-170">These changes could affect the existing model in your database (if you are creating a new version) and it is important to keep your database up-to-date to prevent errors.</span></span>

<span data-ttu-id="d6567-171">Для упрощения отслеживания этих изменений в модели, **Entity Framework Code First Migrations** автоматически обнаруживать изменения, сравнивая модели с помощью схемы базы данных и создает код для обновления базы данных, Создание нового *версии* базы данных.</span><span class="sxs-lookup"><span data-stu-id="d6567-171">To simplify the tracking of these changes in your model, **Entity Framework Code First Migrations** automatically detect changes comparing your model with the database schema and generates specific code to update your database, creating new *versions* of your database.</span></span>

<span data-ttu-id="d6567-172">В этом упражнении показано, как включить **миграций** для приложения и как можно легко обнаружить и вызывают изменения для обновления баз данных.</span><span class="sxs-lookup"><span data-stu-id="d6567-172">This exercise shows you how to enable **Migrations** for your application and how you can easily detect and generate changes to update your databases.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a><span data-ttu-id="d6567-173">Задача 1 – включение миграции</span><span class="sxs-lookup"><span data-stu-id="d6567-173">Task 1 – Enabling Migrations</span></span>

<span data-ttu-id="d6567-174">В этой задаче будет пропускать все шаги включения **Entity Framework Code First Migrations** для **Quiz Компьютерщик** базы данных, изменение модели и общие сведения о том, как эти изменения отражаются в База данных.</span><span class="sxs-lookup"><span data-stu-id="d6567-174">In this task, you will go through the steps of enabling **Entity Framework Code First Migrations** to the **Geek Quiz** database, changing the model and understanding how those changes are reflected in the database.</span></span>

1. <span data-ttu-id="d6567-175">Откройте Visual Studio и откройте **GeekQuiz.sln** файл решения из **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.</span><span class="sxs-lookup"><span data-stu-id="d6567-175">Open Visual Studio and open the **GeekQuiz.sln** solution file from **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.</span></span>
2. <span data-ttu-id="d6567-176">Выполните сборку решения для загрузки и установки **NuGet** зависимости пакетов.</span><span class="sxs-lookup"><span data-stu-id="d6567-176">Build the solution in order to download and install the **NuGet** package dependencies.</span></span> <span data-ttu-id="d6567-177">Чтобы сделать это, щелкните правой кнопкой мыши решение и нажмите кнопку **построить решение** или нажмите клавишу **Ctrl + Shift + B**.</span><span class="sxs-lookup"><span data-stu-id="d6567-177">To do this, right-click the solution and click **Build Solution** or press **Ctrl + Shift + B**.</span></span>
3. <span data-ttu-id="d6567-178">Из **средства** меню в Visual Studio, выберите пункт **диспетчер пакетов библиотеки**, а затем нажмите кнопку **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="d6567-178">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
4. <span data-ttu-id="d6567-179">В **консоль диспетчера пакетов**, введите следующую команду и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="d6567-179">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="d6567-180">Будет создан на основе существующей модели первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="d6567-180">An initial migration based on the existing model will be created.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    <span data-ttu-id="d6567-181">![Включение Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "включение миграции")</span><span class="sxs-lookup"><span data-stu-id="d6567-181">![Enabling Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Enabling Migrations")</span></span>

    <span data-ttu-id="d6567-182">*Включение миграции*</span><span class="sxs-lookup"><span data-stu-id="d6567-182">*Enabling Migrations*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-183">Эта команда добавляет **миграций** папке Quiz Компьютерщик проект, который содержит файл с именем **Configuration.cs**.</span><span class="sxs-lookup"><span data-stu-id="d6567-183">This command adds a **Migrations** folder to Geek Quiz project that contains a file called **Configuration.cs**.</span></span> <span data-ttu-id="d6567-184">**Конфигурации** класс позволяет настраивать поведение миграций для контекста.</span><span class="sxs-lookup"><span data-stu-id="d6567-184">The **Configuration** class allows you to configure how Migrations behaves for your context.</span></span>
5. <span data-ttu-id="d6567-185">С помощью миграций включить, необходимо обновить **конфигурации** класс для заполнения базы данных с начальными данными, **Quiz Компьютерщик** требует.</span><span class="sxs-lookup"><span data-stu-id="d6567-185">With Migrations enabled, you need to update the **Configuration** class to populate the database with the initial data that **Geek Quiz** requires.</span></span> <span data-ttu-id="d6567-186">В разделе **миграций**, замените **Configuration.cs** файл с тем, который расположен в **Source\Assets** папку данную лабораторию.</span><span class="sxs-lookup"><span data-stu-id="d6567-186">Under **Migrations**, replace the **Configuration.cs** file with the one located in the **Source\Assets** folder of this lab.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-187">Так как **миграций** вызовет **начальное значение** метод при каждом обновлении базы данных, необходимо убедиться, что записи не повторяются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d6567-187">Since **Migrations** will call the **Seed** method with every database update, you need to be sure that records are not duplicated in the database.</span></span> <span data-ttu-id="d6567-188">**AddOrUpdate** метод поможет предотвратить дублирование данных.</span><span class="sxs-lookup"><span data-stu-id="d6567-188">The **AddOrUpdate** method will help to prevent duplicate data.</span></span>
6. <span data-ttu-id="d6567-189">Чтобы добавить первоначальную миграцию, введите следующую команду и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="d6567-189">To add an initial migration, enter the following command and then press **Enter**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-190">Убедитесь, что нет никакой базы данных с именем &quot;GeekQuizProd&quot; в экземпляре LocalDB.</span><span class="sxs-lookup"><span data-stu-id="d6567-190">Make sure that there is no database named &quot;GeekQuizProd&quot; in your LocalDB instance.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    <span data-ttu-id="d6567-191">![Добавление базовой схемы миграции](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Добавление базовой схемы миграции")</span><span class="sxs-lookup"><span data-stu-id="d6567-191">![Adding base schema migration](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Adding base schema migration")</span></span>

    <span data-ttu-id="d6567-192">*Добавление базовой схемы миграции*</span><span class="sxs-lookup"><span data-stu-id="d6567-192">*Adding base schema migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-193">**Add-Migration** будет формировать следующей миграции на основе изменений, внесенные в модель с момента создания последней миграции.</span><span class="sxs-lookup"><span data-stu-id="d6567-193">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created.</span></span> <span data-ttu-id="d6567-194">В этом случае является первой миграции проекта, добавит скриптов для создания таблиц, определенных в **TriviaContext** класса.</span><span class="sxs-lookup"><span data-stu-id="d6567-194">In this case, as it is the first migration of the project, it will add the scripts to create all the tables defined in the **TriviaContext** class.</span></span>
7. <span data-ttu-id="d6567-195">Выполнение миграции для обновления базы данных, выполнив следующую команду.</span><span class="sxs-lookup"><span data-stu-id="d6567-195">Execute the migration to update the database by running the following command.</span></span> <span data-ttu-id="d6567-196">Для этой команды необходимо указать **Verbose** флаг для просмотра инструкций SQL, применяемых к целевой базе данных.</span><span class="sxs-lookup"><span data-stu-id="d6567-196">For this command you will specify the **Verbose** flag to view the SQL statements being applied to the target database.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    <span data-ttu-id="d6567-197">![Создание исходной базы данных](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "создание исходной базы данных")</span><span class="sxs-lookup"><span data-stu-id="d6567-197">![Creating initial database](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Creating initial database")</span></span>

    <span data-ttu-id="d6567-198">*Создание исходной базы данных*</span><span class="sxs-lookup"><span data-stu-id="d6567-198">*Creating initial database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-199">**Update-Database** будет применение всех ожидающих миграции в базу данных.</span><span class="sxs-lookup"><span data-stu-id="d6567-199">**Update-Database** will apply any pending migrations to the database.</span></span> <span data-ttu-id="d6567-200">В этом случае он создаст в базе данных с помощью строки подключения, определенные в файле web.config.</span><span class="sxs-lookup"><span data-stu-id="d6567-200">In this case, it will create the database using the connection string defined in your web.config file.</span></span>
8. <span data-ttu-id="d6567-201">Перейдите к **представление** меню и откройте **обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="d6567-201">Go to **View** menu and open **SQL Server Object Explorer**.</span></span>

    <span data-ttu-id="d6567-202">![Открыть в обозревателе объектов SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "открыть в обозревателе объектов SQL Server")</span><span class="sxs-lookup"><span data-stu-id="d6567-202">![Open in SQL Server Object Explorer](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Open in SQL Server Object Explorer")</span></span>

    <span data-ttu-id="d6567-203">*Открыть в обозревателе объектов SQL Server*</span><span class="sxs-lookup"><span data-stu-id="d6567-203">*Open in SQL Server Object Explorer*</span></span>
9. <span data-ttu-id="d6567-204">В **обозреватель объектов SQL Server** окна, подключитесь к экземпляру LocalDB, щелкнув правой кнопкой мыши **SQL Server** узла и выбрав **добавить SQL Server...**  параметр.</span><span class="sxs-lookup"><span data-stu-id="d6567-204">In the **SQL Server Object Explorer** window, connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="d6567-205">![Добавление экземпляра SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Добавление экземпляра SQL Server")</span><span class="sxs-lookup"><span data-stu-id="d6567-205">![Adding a SQL Server Instance](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="d6567-206">*Добавление экземпляра SQL Server в обозревателе объектов SQL Server*</span><span class="sxs-lookup"><span data-stu-id="d6567-206">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
10. <span data-ttu-id="d6567-207">Задайте **имя_сервера** для *(localdb) \v11.0* и оставить **проверки подлинности Windows** как свой режим проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d6567-207">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="d6567-208">Нажмите кнопку **Connect** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="d6567-208">Click **Connect** to continue.</span></span>

    <span data-ttu-id="d6567-209">![Подключение к LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "подключение к LocalDB")</span><span class="sxs-lookup"><span data-stu-id="d6567-209">![Connecting to LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="d6567-210">*Подключение к LocalDB*</span><span class="sxs-lookup"><span data-stu-id="d6567-210">*Connecting to LocalDB*</span></span>
11. <span data-ttu-id="d6567-211">Откройте **GeekQuizProd** базы данных и разверните **таблиц** узла.</span><span class="sxs-lookup"><span data-stu-id="d6567-211">Open the **GeekQuizProd** database and expand the **Tables** node.</span></span> <span data-ttu-id="d6567-212">Как вы видите, **Update-Database** команда создается все таблицы, определенные в **TriviaContext** класса.</span><span class="sxs-lookup"><span data-stu-id="d6567-212">As you can see, the **Update-Database** command generated all the tables defined in the **TriviaContext** class.</span></span> <span data-ttu-id="d6567-213">Найдите **dbo. TriviaQuestions** таблицы и откройте узел «столбцы».</span><span class="sxs-lookup"><span data-stu-id="d6567-213">Locate the **dbo.TriviaQuestions** table and open the columns node.</span></span> <span data-ttu-id="d6567-214">В следующей задаче будет добавить новый столбец в эту таблицу и обновить базу данных при помощи **миграций**.</span><span class="sxs-lookup"><span data-stu-id="d6567-214">In the next task, you will add a new column to this table and update the database using **Migrations**.</span></span>

    <span data-ttu-id="d6567-215">![Занимательные вопросы столбцы](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "занимательные вопросы столбцов")</span><span class="sxs-lookup"><span data-stu-id="d6567-215">![Trivia Questions Columns](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia Questions Columns")</span></span>

    <span data-ttu-id="d6567-216">*Занимательные вопросы столбцов*</span><span class="sxs-lookup"><span data-stu-id="d6567-216">*Trivia Questions Columns*</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a><span data-ttu-id="d6567-217">Задача 2 – обновление схемы базы данных, с помощью миграций</span><span class="sxs-lookup"><span data-stu-id="d6567-217">Task 2 – Updating Database Schema Using Migrations</span></span>

<span data-ttu-id="d6567-218">В этой задаче вы воспользуетесь **Entity Framework Code First Migrations** обнаруживать изменения в модели и сформировать необходимый код для обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="d6567-218">In this task, you will use **Entity Framework Code First Migrations** to detect a change in your model and generate the necessary code to update the database.</span></span> <span data-ttu-id="d6567-219">Мы обновим **TriviaQuestions** сущности, добавив в него новое свойство.</span><span class="sxs-lookup"><span data-stu-id="d6567-219">You will update the **TriviaQuestions** entity by adding a new property to it.</span></span> <span data-ttu-id="d6567-220">Затем вы выполните команды для создания новой миграции для включения нового столбца в таблице.</span><span class="sxs-lookup"><span data-stu-id="d6567-220">Then you will run commands to create a new Migration to include the new column in the table.</span></span>

1. <span data-ttu-id="d6567-221">В **обозревателе решений**, дважды щелкните **TriviaQuestion.cs** файл, расположенный внутри **моделей** папки.</span><span class="sxs-lookup"><span data-stu-id="d6567-221">In **Solution Explorer**, double-click the **TriviaQuestion.cs** file located inside the **Models** folder.</span></span>
2. <span data-ttu-id="d6567-222">Добавить новое свойство с именем **указание**, как показано в следующем фрагменте кода.</span><span class="sxs-lookup"><span data-stu-id="d6567-222">Add a new property named **Hint**, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. <span data-ttu-id="d6567-223">В **консоль диспетчера пакетов**, введите следующую команду и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="d6567-223">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="d6567-224">Новой миграции создается отображающий изменения в нашей модели.</span><span class="sxs-lookup"><span data-stu-id="d6567-224">A new migration will be created reflecting the change in our model.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    <span data-ttu-id="d6567-225">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span><span class="sxs-lookup"><span data-stu-id="d6567-225">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span></span>

    <span data-ttu-id="d6567-226">*Add-Migration*</span><span class="sxs-lookup"><span data-stu-id="d6567-226">*Add-Migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-227">Файл миграции состоит из двух методов **вверх** и **вниз**.</span><span class="sxs-lookup"><span data-stu-id="d6567-227">A Migration file is composed of two methods, **Up** and **Down**.</span></span>
    > 
    > - <span data-ttu-id="d6567-228">**Вверх** метод будет использоваться для указания, какие изменения в текущей версии наши приложения, нужно внести в базу данных.</span><span class="sxs-lookup"><span data-stu-id="d6567-228">The **Up** method will be used to specify what changes the current version of our application need to apply to the database.</span></span>
    > - <span data-ttu-id="d6567-229">**Вниз** используется для отмены изменений, мы добавили **вверх** метод.</span><span class="sxs-lookup"><span data-stu-id="d6567-229">The **Down** is used to reverse the changes we have added to the **Up** method.</span></span>
    > 
    > <span data-ttu-id="d6567-230">При миграции базы данных обновляет базу данных, в порядке меток времени, а только те, которые не были использованы с момента последнего обновления будут применяться при всех переносах ( \_MigrationHistory таблицы следит за миграций, которые были применены).</span><span class="sxs-lookup"><span data-stu-id="d6567-230">When the Database Migration updates the database, it will run all migrations in the timestamp order, and only those that have not been used since the last update (The \_MigrationHistory table keeps track of which migrations have been applied).</span></span> <span data-ttu-id="d6567-231">**Вверх** будет вызван метод всех операций миграции и будут внесены изменения, мы указали в базу данных.</span><span class="sxs-lookup"><span data-stu-id="d6567-231">The **Up** method of all migrations will be called and will make the changes we have specified to the database.</span></span> <span data-ttu-id="d6567-232">Если мы решим, чтобы вернуться к предыдущей миграции, **вниз** метод будет вызываться, чтобы внести изменения в обратном порядке.</span><span class="sxs-lookup"><span data-stu-id="d6567-232">If we decide to go back to a previous migration, the **Down** method will be called to redo the changes in a reverse order.</span></span>
4. <span data-ttu-id="d6567-233">В **консоль диспетчера пакетов**, введите следующую команду и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="d6567-233">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. <span data-ttu-id="d6567-234">Выходные данные **Update-Database** созданную команду **Alter Table** инструкцию SQL, чтобы добавить новый столбец для **TriviaQuestions** таблицы, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="d6567-234">The output of the **Update-Database** command generated an **Alter Table** SQL statement to add a new column to the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="d6567-235">![Добавьте инструкцию SQL столбца создан](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "добавьте инструкцию SQL столбца создан")</span><span class="sxs-lookup"><span data-stu-id="d6567-235">![Add column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Add column SQL statement generated")</span></span>

    <span data-ttu-id="d6567-236">*Добавьте инструкцию SQL столбца создан*</span><span class="sxs-lookup"><span data-stu-id="d6567-236">*Add column SQL statement generated*</span></span>
6. <span data-ttu-id="d6567-237">В **обозреватель объектов SQL Server**, обновите **dbo. TriviaQuestions** таблицу и убедитесь, что новый **указание** отображается столбец.</span><span class="sxs-lookup"><span data-stu-id="d6567-237">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the new **Hint** column is displayed.</span></span>

    <span data-ttu-id="d6567-238">![Отображение нового столбца указание](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "отображение нового столбца подсказки")</span><span class="sxs-lookup"><span data-stu-id="d6567-238">![Showing the new Hint Column](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Showing the new Hint Column")</span></span>

    <span data-ttu-id="d6567-239">*Отображение нового столбца подсказки*</span><span class="sxs-lookup"><span data-stu-id="d6567-239">*Showing the new Hint Column*</span></span>
7. <span data-ttu-id="d6567-240">Вернитесь в **TriviaQuestion.cs** редакторе добавьте **StringLength** ограничение *указание* свойства, как показано в следующем фрагменте кода.</span><span class="sxs-lookup"><span data-stu-id="d6567-240">Back in the **TriviaQuestion.cs** editor, add a **StringLength** constraint to the *Hint* property, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. <span data-ttu-id="d6567-241">В **консоль диспетчера пакетов**, введите следующую команду и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="d6567-241">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. <span data-ttu-id="d6567-242">В **консоль диспетчера пакетов**, введите следующую команду и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="d6567-242">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. <span data-ttu-id="d6567-243">Выходные данные **Update-Database** созданную команду **Alter Table** инструкции SQL для обновления *указание* столбец, имеющий тип **TriviaQuestions** таблицы, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="d6567-243">The output of the **Update-Database** command generated an **Alter Table** SQL statement to update the *hint* column type of the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="d6567-244">![Инструкция ALTER column SQL создается](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "инструкция Alter column SQL создан")</span><span class="sxs-lookup"><span data-stu-id="d6567-244">![Alter column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL statement generated")</span></span>

    <span data-ttu-id="d6567-245">*Инструкция ALTER column SQL создан*</span><span class="sxs-lookup"><span data-stu-id="d6567-245">*Alter column SQL statement generated*</span></span>
11. <span data-ttu-id="d6567-246">В **обозреватель объектов SQL Server**, обновите **dbo. TriviaQuestions** таблицу и убедитесь, что **указание** столбец имеет тип **nvarchar(150)**.</span><span class="sxs-lookup"><span data-stu-id="d6567-246">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the **Hint** column type is **nvarchar(150)**.</span></span>

    <span data-ttu-id="d6567-247">![Отображение нового ограничения](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "отображается новое ограничение")</span><span class="sxs-lookup"><span data-stu-id="d6567-247">![Showing the new constraint](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Showing the new constraint")</span></span>

    <span data-ttu-id="d6567-248">*Отображение нового ограничения*</span><span class="sxs-lookup"><span data-stu-id="d6567-248">*Showing the new constraint*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a><span data-ttu-id="d6567-249">Упражнение 2: Развертывание веб-приложения в промежуточную среду</span><span class="sxs-lookup"><span data-stu-id="d6567-249">Exercise 2: Deploying a Web App to Staging</span></span>

<span data-ttu-id="d6567-250">**Веб-приложения в службе приложений Azure** позволяет выполнять промежуточную публикацию.</span><span class="sxs-lookup"><span data-stu-id="d6567-250">**Web Apps in Azure App Service** enables you to perform staged publishing.</span></span> <span data-ttu-id="d6567-251">Для промежуточной публикации создается слот промежуточного сайта для каждого рабочего сайта по умолчанию и позволяет менять слоты без задержки.</span><span class="sxs-lookup"><span data-stu-id="d6567-251">Staged publishing creates a staging site slot for each default production site and enables you to swap these slots with no down time.</span></span> <span data-ttu-id="d6567-252">Это отлично подходит для проверки изменений перед выпуском общедоступной, осуществляют пошаговую интеграцию содержимого сайта и откатывается, если изменения не работают надлежащим образом.</span><span class="sxs-lookup"><span data-stu-id="d6567-252">This is really useful to validate changes before releasing to the public, incrementally integrate site content, and roll back if changes are not working as expected.</span></span>

<span data-ttu-id="d6567-253">В этом упражнении вы развернете **Quiz Компьютерщик** приложения в промежуточной среде, веб-приложения с помощью системы управления версиями Git.</span><span class="sxs-lookup"><span data-stu-id="d6567-253">In this exercise, you will deploy the **Geek Quiz** application to the staging environment of your web app using Git source control.</span></span> <span data-ttu-id="d6567-254">Чтобы сделать это, будет создание веб-приложения и подготовить необходимые компоненты на портале управления, Настройка **Git** репозиторий и Push-уведомлений приложения исходный код с локального компьютера в промежуточном слоте.</span><span class="sxs-lookup"><span data-stu-id="d6567-254">To do this, you will create the web app and provision the required components at the management portal, configure a **Git** repository and push the application source code from your local computer to the staging slot.</span></span> <span data-ttu-id="d6567-255">Требуется обновить рабочую базу данных с помощью **Code First Migrations** вы создали в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="d6567-255">You will also update your production database with the **Code First Migrations** you created in the previous exercise.</span></span> <span data-ttu-id="d6567-256">Затем приложение будет выполняться в данной тестовой среде, чтобы проверить его работу.</span><span class="sxs-lookup"><span data-stu-id="d6567-256">You will then execute the application in this test environment to verify its operation.</span></span> <span data-ttu-id="d6567-257">Когда вы будете удовлетворены, что приложение работает в соответствии с вашим ожиданиям, будет переводить приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="d6567-257">Once you are satisfied that it is working according to your expectations, you will promote the application to production.</span></span>

> [!NOTE]
> <span data-ttu-id="d6567-258">Чтобы включить промежуточную публикацию, веб-приложения должен быть в **стандартный режим**.</span><span class="sxs-lookup"><span data-stu-id="d6567-258">To enable staged publishing, the web app must be in **Standard mode**.</span></span> <span data-ttu-id="d6567-259">Обратите внимание на то, что при изменении веб-приложения в стандартном режиме будет взиматься дополнительная плата.</span><span class="sxs-lookup"><span data-stu-id="d6567-259">Note that additional charges will be incurred if you change your web app to Standard mode.</span></span> <span data-ttu-id="d6567-260">Дополнительные сведения о ценах см. в разделе [цен на службу приложений](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="d6567-260">For more information about pricing, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a><span data-ttu-id="d6567-261">Задача 1 – Создание веб-приложения в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="d6567-261">Task 1 – Creating a Web App in Azure App Service</span></span>

<span data-ttu-id="d6567-262">В этой задаче вы создадите веб-приложения в **службе приложений Azure** на портале управления.</span><span class="sxs-lookup"><span data-stu-id="d6567-262">In this task, you will create a web app in **Azure App Service** from the management portal.</span></span> <span data-ttu-id="d6567-263">Вы также настроите **базы данных SQL** для сохранения данных приложения и Настройка локального репозитория Git для системы управления версиями.</span><span class="sxs-lookup"><span data-stu-id="d6567-263">You will also configure a **SQL Database** to persist the application data, and configure a local Git repository for source control.</span></span>

1. <span data-ttu-id="d6567-264">Перейдите к [портал управления Azure](https://manage.windowsazure.com) и войдите с использованием учетной записи Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="d6567-264">Go to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>

    ![Войдите на портал управления Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    <span data-ttu-id="d6567-266">*Войдите на портал управления Azure*</span><span class="sxs-lookup"><span data-stu-id="d6567-266">*Sign in to the Azure management portal*</span></span>
2. <span data-ttu-id="d6567-267">Нажмите кнопку **New** на панели команд в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="d6567-267">Click **New** in the command bar at the bottom of the page.</span></span>

    <span data-ttu-id="d6567-268">![Создание нового веб-приложения](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Создание нового веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="d6567-268">![Creating a new web app](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Creating a new web app")</span></span>

    <span data-ttu-id="d6567-269">*Создание нового веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="d6567-269">*Creating a new web app*</span></span>
3. <span data-ttu-id="d6567-270">Нажмите кнопку **вычислений**, **веб-сайт** и затем **настраиваемое создание**.</span><span class="sxs-lookup"><span data-stu-id="d6567-270">Click **Compute**, **Website** and then **Custom Create**.</span></span>

    <span data-ttu-id="d6567-271">![Создание нового веб-приложения, воспользуйтесь функцией настраиваемое создание](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Создание нового веб-приложения, воспользуйтесь функцией настраиваемое Создание")</span><span class="sxs-lookup"><span data-stu-id="d6567-271">![Creating a new web app using Custom Create](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Creating a new web app using Custom Create")</span></span>

    <span data-ttu-id="d6567-272">*Создание нового веб-приложения, воспользуйтесь функцией настраиваемое Создание*</span><span class="sxs-lookup"><span data-stu-id="d6567-272">*Creating a new web app using Custom Create*</span></span>
4. <span data-ttu-id="d6567-273">В **новый веб-узел — настраиваемое создание** диалоговое окно укажите свободный **URL-адрес** (например *Компьютерщик головоломки*), выберите расположение в **регион** Выберите в раскрывающемся списке и выберите **создать новую базу данных SQL** в **базы данных** стрелку раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="d6567-273">In the **New Website - Custom Create** dialog box, provide an available **URL** (e.g. *geek-quiz*), select a location in the **Region** drop-down list, and select **Create a new SQL database** in the **Database** drop-down list.</span></span> <span data-ttu-id="d6567-274">Наконец, выберите **публикации из системы управления версиями** флажок и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d6567-274">Finally, select the **Publish from source control** check box and click **Next**.</span></span>

    ![Настройка нового веб-приложения](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    <span data-ttu-id="d6567-276">*Настройка нового веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="d6567-276">*Customizing the new web app*</span></span>
5. <span data-ttu-id="d6567-277">Укажите следующие сведения для настройки базы данных:</span><span class="sxs-lookup"><span data-stu-id="d6567-277">Specify the following information for the database settings:</span></span>

   - <span data-ttu-id="d6567-278">В **имя** текста введите имя базы данных (например *geekquiz\_db*)</span><span class="sxs-lookup"><span data-stu-id="d6567-278">In the **Name** text box, enter a database name (e.g. *geekquiz\_db*)</span></span>
   - <span data-ttu-id="d6567-279">На сервере **раскрывающегося списка** выберите **сервер базы данных SQL на новый**.</span><span class="sxs-lookup"><span data-stu-id="d6567-279">In the Server **drop-down** list, select **New SQL database server**.</span></span> <span data-ttu-id="d6567-280">Кроме того можно выбрать существующий сервер.</span><span class="sxs-lookup"><span data-stu-id="d6567-280">Alternatively, you can select an existing server.</span></span>
   - <span data-ttu-id="d6567-281">В **имя пользователя базы данных** и **пароль базы данных** введите имя пользователя администратора и пароль для сервера базы данных SQL.</span><span class="sxs-lookup"><span data-stu-id="d6567-281">In the **Database username** and **Database password** boxes, enter the administrator username and password for the SQL database server.</span></span> <span data-ttu-id="d6567-282">Если после выбора сервера, вы уже создали, вам будет предложено ввести пароль.</span><span class="sxs-lookup"><span data-stu-id="d6567-282">If you select a server you have already created, you will be prompted for the password.</span></span>

     ![Задание параметров базы данных](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     <span data-ttu-id="d6567-284">*Задание параметров базы данных*</span><span class="sxs-lookup"><span data-stu-id="d6567-284">*Specifying the database settings*</span></span>
6. <span data-ttu-id="d6567-285">Чтобы продолжить, нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="d6567-285">Click **Next** to continue.</span></span>
7. <span data-ttu-id="d6567-286">Выберите **локальный репозиторий Git** для системы управления версиями, и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d6567-286">Select **Local Git repository** for the source control to use and click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-287">Вы может запрашиваться учетные данные развертывания (имя пользователя и пароль).</span><span class="sxs-lookup"><span data-stu-id="d6567-287">You may be prompted for the deployment credentials (a username and password).</span></span>

    ![Создание репозитория Git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    <span data-ttu-id="d6567-289">*Создание репозитория Git*</span><span class="sxs-lookup"><span data-stu-id="d6567-289">*Creating the Git repository*</span></span>
8. <span data-ttu-id="d6567-290">Подождите, пока создается новое веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="d6567-290">Wait until the new web app is created.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-291">По умолчанию, Azure предоставляет доменов на *azurewebsites.net* , но также дает возможность задать пользовательские домены, с помощью портала управления Azure.</span><span class="sxs-lookup"><span data-stu-id="d6567-291">By default, Azure provides domains at *azurewebsites.net* but also gives you the possibility to set custom domains using the Azure management portal.</span></span> <span data-ttu-id="d6567-292">Тем не менее можно управлять только пользовательские домены при использовании определенных режимах службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="d6567-292">However, you can only manage custom domains if you are using certain Azure App Service modes.</span></span>
    > 
    > <span data-ttu-id="d6567-293">Служба приложений Azure доступен в выпусках Free, Shared, Basic, Standard и Premium.</span><span class="sxs-lookup"><span data-stu-id="d6567-293">Azure App Service is available in Free, Shared, Basic, Standard, and Premium editions.</span></span> <span data-ttu-id="d6567-294">В режиме Free и Shared все веб-приложения выполняются в среде с несколькими клиентами и имеет квот на использование ЦП, памяти и сети.</span><span class="sxs-lookup"><span data-stu-id="d6567-294">In Free and Shared mode, all web apps run in a multi-tenant environment and have quotas for CPU, Memory, and Network usage.</span></span> <span data-ttu-id="d6567-295">Максимальное число бесплатных приложений зависит от плана.</span><span class="sxs-lookup"><span data-stu-id="d6567-295">The maximum number of free apps may vary with your plan.</span></span> <span data-ttu-id="d6567-296">В стандартном режиме выберите, какие приложения, выполните на выделенных виртуальных машинах, которые соответствуют в стандартный Azure вычислительные ресурсы.</span><span class="sxs-lookup"><span data-stu-id="d6567-296">In Standard mode, you choose which apps run on dedicated virtual machines that correspond to the standard Azure compute resources.</span></span> <span data-ttu-id="d6567-297">Можно найти конфигурацию режима веб-приложения в **масштабирования** меню веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d6567-297">You can find the web app mode configuration in the **Scale** menu of your web app.</span></span>
    > 
    > <span data-ttu-id="d6567-298">![Служба приложений Azure режимы](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "режимы службы приложений Azure")</span><span class="sxs-lookup"><span data-stu-id="d6567-298">![Azure App Service Modes](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service Modes")</span></span>
    > 
    > <span data-ttu-id="d6567-299">Если вы используете **Shared** или **стандартный** режиме, вы сможете управлять личными доменами для веб-приложения, перейдя к своему приложению **Настройка** меню и выбрав пункт **Управление доменами** под *доменных имен*.</span><span class="sxs-lookup"><span data-stu-id="d6567-299">If you are using **Shared** or **Standard** mode, you will be able to manage custom domains for your web app by going to your app's **Configure** menu and clicking **Manage Domains** under *domain names*.</span></span>
    > 
    > <span data-ttu-id="d6567-300">![Управление доменами](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Управление доменами")</span><span class="sxs-lookup"><span data-stu-id="d6567-300">![Manage Domains](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Manage Domains")</span></span>
    > 
    > <span data-ttu-id="d6567-301">![Управление пользовательскими доменами](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "управление пользовательскими доменами")</span><span class="sxs-lookup"><span data-stu-id="d6567-301">![Manage Custom Domains](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Manage Custom Domains")</span></span>
9. <span data-ttu-id="d6567-302">После создания веб-приложения щелкните ссылку под **URL-адрес** столбца, чтобы проверить выполнение нового веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d6567-302">Once the web app is created, click the link under the **URL** column to check that the new web app is running.</span></span>

    ![Просмотр новое веб-приложение](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    <span data-ttu-id="d6567-304">*Просмотр новое веб-приложение*</span><span class="sxs-lookup"><span data-stu-id="d6567-304">*Browsing to the new web app*</span></span>

    ![Выполнение веб-приложения](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    <span data-ttu-id="d6567-306">*Выполнение веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="d6567-306">*web app running*</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a><span data-ttu-id="d6567-307">Задача 2 – Создание рабочей базы данных SQL</span><span class="sxs-lookup"><span data-stu-id="d6567-307">Task 2 – Creating the Production SQL Database</span></span>

<span data-ttu-id="d6567-308">В этой задаче вы воспользуетесь **Entity Framework Code First Migrations** для создания базы данных, предназначенных для **базы данных SQL Azure** экземпляр, созданный в предыдущей задаче.</span><span class="sxs-lookup"><span data-stu-id="d6567-308">In this task, you will use the **Entity Framework Code First Migrations** to create the database targeting the **Azure SQL Database** instance you created in the previous task.</span></span>

1. <span data-ttu-id="d6567-309">На портале управления перейдите к веб-приложения, созданного в предыдущей задаче и перейти к его **панели мониторинга**.</span><span class="sxs-lookup"><span data-stu-id="d6567-309">In the Management Portal, navigate to the web app you created in the previous task and go to its **Dashboard**.</span></span>
2. <span data-ttu-id="d6567-310">В **панели мониторинга** щелкните **просмотреть строки подключения** ссылку в списке **быстрый обзор** раздел.</span><span class="sxs-lookup"><span data-stu-id="d6567-310">In the **Dashboard** page, click **View connection strings** link under the **quick glance** section.</span></span>

    <span data-ttu-id="d6567-311">![Просмотреть строки подключения](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "просмотреть строки подключения")</span><span class="sxs-lookup"><span data-stu-id="d6567-311">![View connection strings](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "View connection strings")</span></span>

    <span data-ttu-id="d6567-312">*Просмотр строк подключения*</span><span class="sxs-lookup"><span data-stu-id="d6567-312">*View connection strings*</span></span>
3. <span data-ttu-id="d6567-313">Копировать **строку подключения** значение и закрыть диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="d6567-313">Copy the **connection string** value and close the dialog box.</span></span>

    <span data-ttu-id="d6567-314">![Строка подключения на портале управления Azure](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "строку подключения на портале управления Azure")</span><span class="sxs-lookup"><span data-stu-id="d6567-314">![Connection String in Azure Management Portal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Connection String in Azure Management Portal")</span></span>

    <span data-ttu-id="d6567-315">*Строка подключения на портале управления Azure*</span><span class="sxs-lookup"><span data-stu-id="d6567-315">*Connection String in Azure Management Portal*</span></span>
4. <span data-ttu-id="d6567-316">Нажмите кнопку **баз данных SQL** для просмотра списка баз данных SQL в Azure</span><span class="sxs-lookup"><span data-stu-id="d6567-316">Click **SQL Databases** to see the list of SQL databases in Azure</span></span>

    <span data-ttu-id="d6567-317">![Меню базы данных SQL](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "меню базы данных SQL")</span><span class="sxs-lookup"><span data-stu-id="d6567-317">![SQL Database menu](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database menu")</span></span>

    <span data-ttu-id="d6567-318">*Меню базы данных SQL*</span><span class="sxs-lookup"><span data-stu-id="d6567-318">*SQL Database menu*</span></span>
5. <span data-ttu-id="d6567-319">Найдите базу данных, созданную в предыдущей задаче и нажмите кнопку на сервере.</span><span class="sxs-lookup"><span data-stu-id="d6567-319">Locate the database you created in the previous task and click on the Server.</span></span>

    <span data-ttu-id="d6567-320">![Сервер базы данных SQL](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "базы данных SQL server")</span><span class="sxs-lookup"><span data-stu-id="d6567-320">![SQL Database server](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database server")</span></span>

    <span data-ttu-id="d6567-321">*Сервер базы данных SQL*</span><span class="sxs-lookup"><span data-stu-id="d6567-321">*SQL Database server*</span></span>
6. <span data-ttu-id="d6567-322">В **быстрый запуск** страницы сервера, щелкните **Настройка**.</span><span class="sxs-lookup"><span data-stu-id="d6567-322">In the **Quick Start** page of the server, click on **Configure**.</span></span>

    <span data-ttu-id="d6567-323">![Настройка меню](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Настройка меню")</span><span class="sxs-lookup"><span data-stu-id="d6567-323">![Configure menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Configure menu")</span></span>

    <span data-ttu-id="d6567-324">*Настройка меню*</span><span class="sxs-lookup"><span data-stu-id="d6567-324">*Configure menu*</span></span>
7. <span data-ttu-id="d6567-325">В **разрешенные IP-адреса** щелкните **добавить в разрешенные IP-адреса** ссылку, чтобы включить IP-адрес для подключения к серверу базы данных SQL.</span><span class="sxs-lookup"><span data-stu-id="d6567-325">In the **Allowed IP addresses** section, click on **Add to the allowed IP addresses** link to enable your IP to connect to the SQL Database server.</span></span>

    <span data-ttu-id="d6567-326">![Разрешенные IP-адреса](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "разрешенные IP-адреса")</span><span class="sxs-lookup"><span data-stu-id="d6567-326">![Allowed IP addresses](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Allowed IP addresses")</span></span>

    <span data-ttu-id="d6567-327">*Разрешенные IP-адреса*</span><span class="sxs-lookup"><span data-stu-id="d6567-327">*Allowed IP addresses*</span></span>
8. <span data-ttu-id="d6567-328">Нажмите кнопку **Сохранить** в нижней части страницы, чтобы завершить шаг.</span><span class="sxs-lookup"><span data-stu-id="d6567-328">Click **Save** at the bottom of the page to complete the step.</span></span>
9. <span data-ttu-id="d6567-329">Переключитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6567-329">Switch back to Visual Studio.</span></span>
10. <span data-ttu-id="d6567-330">В **консоль диспетчера пакетов**, выполните следующие команды, заменив *[YOUR-CONNECTION-STRING]* заполнитель строкой подключения, скопированной из Azure</span><span class="sxs-lookup"><span data-stu-id="d6567-330">In the **Package Manager Console**, execute the following command replacing *[YOUR-CONNECTION-STRING]* placeholder with the connection string you copied from Azure</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    <span data-ttu-id="d6567-331">![Обновление базы данных, предназначенных для Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "обновления базы данных, предназначенных для базы данных SQL Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="d6567-331">![Update database targeting Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Update database targeting Windows Azure SQL Database")</span></span>

    <span data-ttu-id="d6567-332">*Обновление базы данных, предназначенных для базы данных SQL Azure*</span><span class="sxs-lookup"><span data-stu-id="d6567-332">*Update database targeting Azure SQL Database*</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a><span data-ttu-id="d6567-333">Задача 3 – развертывание Компьютерщик тест в промежуточную среду с помощью Git</span><span class="sxs-lookup"><span data-stu-id="d6567-333">Task 3 – Deploying Geek Quiz to Staging Using Git</span></span>

<span data-ttu-id="d6567-334">В этой задаче будет включить промежуточную публикацию в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="d6567-334">In this task, you will enable staged publishing in your web app.</span></span> <span data-ttu-id="d6567-335">Затем будет использовать Git для публикации приложения Quiz Компьютерщик непосредственно с локального компьютера в промежуточной среде, веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d6567-335">Then, you will use Git to publish the Geek Quiz application directly from your local computer to the staging environment of your web app.</span></span>

1. <span data-ttu-id="d6567-336">Вернитесь на портал и щелкните имя веб-приложения в разделе **имя** столбец для отображения страницы управления.</span><span class="sxs-lookup"><span data-stu-id="d6567-336">Go back to the portal and click the name of the web app under the **Name** column to display the management pages.</span></span>

    ![Открытие приложения управления веб-страниц](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    <span data-ttu-id="d6567-338">*Открытие приложения управления веб-страниц*</span><span class="sxs-lookup"><span data-stu-id="d6567-338">*Opening the web app management pages*</span></span>
2. <span data-ttu-id="d6567-339">Перейдите к **масштабирования** страницы.</span><span class="sxs-lookup"><span data-stu-id="d6567-339">Navigate to the **Scale** page.</span></span> <span data-ttu-id="d6567-340">В разделе **Общие** выберите **стандартный** конфигурации и нажмите кнопку **Сохранить** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="d6567-340">Under the **general** section, select **Standard** for the configuration and click **Save** in the command bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-341">Запускать все веб-приложения в текущей области и подписки в **стандартный** режим, оставьте **выбрать все** флажком в **выберите сайты** конфигурации.</span><span class="sxs-lookup"><span data-stu-id="d6567-341">To run all web apps in the current region and subscription in **Standard** mode, leave the **Select All** check box selected in the **Choose Sites** configuration.</span></span> <span data-ttu-id="d6567-342">В противном случае снимите **выбрать все** "флажок".</span><span class="sxs-lookup"><span data-stu-id="d6567-342">Otherwise, clear the **Select All** check box.</span></span>

    <span data-ttu-id="d6567-343">![Обновление веб-приложения до стандартного режима](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "повышения веб-приложения до стандартного режима")</span><span class="sxs-lookup"><span data-stu-id="d6567-343">![Upgrading the web app to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Upgrading the web app to Standard mode")</span></span>

    <span data-ttu-id="d6567-344">*Обновление до стандартного режима веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="d6567-344">*Upgrading the Web App to Standard mode*</span></span>
3. <span data-ttu-id="d6567-345">Нажмите кнопку **Да** для подтверждения изменения.</span><span class="sxs-lookup"><span data-stu-id="d6567-345">Click **Yes** to confirm the changes.</span></span>

    <span data-ttu-id="d6567-346">![Подтверждение изменений до стандартного режима](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "продолжить изменение режима web app")</span><span class="sxs-lookup"><span data-stu-id="d6567-346">![Confirming the change to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuing with the changing of the web app mode")</span></span>

    <span data-ttu-id="d6567-347">*Подтверждение изменений до стандартного режима*</span><span class="sxs-lookup"><span data-stu-id="d6567-347">*Confirming the change to Standard mode*</span></span>
4. <span data-ttu-id="d6567-348">Перейдите к **панели мониторинга** и нажмите кнопку **Enable промежуточная публикация** под **быстрый обзор** раздел.</span><span class="sxs-lookup"><span data-stu-id="d6567-348">Go to the **Dashboard** page and click **Enable staged publishing** under the **quick glance** section.</span></span>

    <span data-ttu-id="d6567-349">![Позволяют использовать промежуточную публикацию](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Включение промежуточная публикация")</span><span class="sxs-lookup"><span data-stu-id="d6567-349">![Enabling staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Enabling staged publishing")</span></span>

    <span data-ttu-id="d6567-350">*Позволяют использовать промежуточную публикацию*</span><span class="sxs-lookup"><span data-stu-id="d6567-350">*Enabling staged publishing*</span></span>
5. <span data-ttu-id="d6567-351">Нажмите кнопку **Да** Чтобы включить промежуточную публикацию.</span><span class="sxs-lookup"><span data-stu-id="d6567-351">Click **Yes** to enable staged publishing.</span></span>

    <span data-ttu-id="d6567-352">![Подтверждение промежуточная публикация](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "нажав кнопку Да, чтобы включить промежуточную публикацию")</span><span class="sxs-lookup"><span data-stu-id="d6567-352">![Confirming staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Clicking Yes to enable staged publishing")</span></span>

    <span data-ttu-id="d6567-353">*Подтверждение для промежуточной публикации*</span><span class="sxs-lookup"><span data-stu-id="d6567-353">*Confirming staged publishing*</span></span>
6. <span data-ttu-id="d6567-354">В списке веб-приложений разверните знак слева от вашего имени веб-приложения для отображения слот промежуточного сайта.</span><span class="sxs-lookup"><span data-stu-id="d6567-354">In the list of web apps, expand the mark to the left of your web app name to display the staging site slot.</span></span> <span data-ttu-id="d6567-355">Он имеет имя веб-приложения, за которым следует ***(промежуточный)***.</span><span class="sxs-lookup"><span data-stu-id="d6567-355">It has the name of your web app followed by ***(staging)***.</span></span> <span data-ttu-id="d6567-356">Щелкните промежуточный сайт, чтобы перейти на страницу управления.</span><span class="sxs-lookup"><span data-stu-id="d6567-356">Click the staging site to go to the management page.</span></span>

    <span data-ttu-id="d6567-357">![Переход к промежуточному веб-приложению](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "переход к промежуточному веб-приложению")</span><span class="sxs-lookup"><span data-stu-id="d6567-357">![Navigating to the staging web app](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigating to the staging web app")</span></span>

    <span data-ttu-id="d6567-358">*Переход к промежуточного приложения*</span><span class="sxs-lookup"><span data-stu-id="d6567-358">*Navigating to the staging app*</span></span>
7. <span data-ttu-id="d6567-359">Обратите внимание, что эту страницу управления он выглядит как страница управления любого другого веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d6567-359">Notice that he management page looks like any other web app's management page.</span></span> <span data-ttu-id="d6567-360">Перейдите к **развертываний** страницы и скопируйте **URL-адрес Git** значение.</span><span class="sxs-lookup"><span data-stu-id="d6567-360">Navigate to the **Deployments** page and copy the **Git URL** value.</span></span> <span data-ttu-id="d6567-361">Он понадобится позже в этом упражнении.</span><span class="sxs-lookup"><span data-stu-id="d6567-361">You will use it later in this exercise.</span></span>

    ![Копирование URL-адрес Git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    <span data-ttu-id="d6567-363">*Копирование URL-адрес Git*</span><span class="sxs-lookup"><span data-stu-id="d6567-363">*Copying the Git URL value*</span></span>
8. <span data-ttu-id="d6567-364">Откройте новую **Git Bash** консоли и выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="d6567-364">Open a new **Git Bash** console and execute the following commands.</span></span> <span data-ttu-id="d6567-365">Обновление *[YOUR-APPLICATION-PATH]* заполнитель с путем к **GeekQuiz** решения, расположенный в **Source\Ex1 DeployingWebSiteToStaging\Begin** папку Это лабораторное занятие.</span><span class="sxs-lookup"><span data-stu-id="d6567-365">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution, located in the **Source\Ex1-DeployingWebSiteToStaging\Begin** folder of this lab.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Инициализация Git и первой фиксации](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    <span data-ttu-id="d6567-367">*Инициализация Git и первой фиксации*</span><span class="sxs-lookup"><span data-stu-id="d6567-367">*Git initialization and first commit*</span></span>
9. <span data-ttu-id="d6567-368">Выполните следующую команду для отправки изменений в удаленный веб-приложения **Git** репозитория.</span><span class="sxs-lookup"><span data-stu-id="d6567-368">Run the following command to push your web app to the remote **Git** repository.</span></span> <span data-ttu-id="d6567-369">Замените заполнитель URL-адрес, полученный с портала управления.</span><span class="sxs-lookup"><span data-stu-id="d6567-369">Replace the placeholder with the URL you obtained from the management portal.</span></span> <span data-ttu-id="d6567-370">Вам будет предложено ввести пароль развертывания.</span><span class="sxs-lookup"><span data-stu-id="d6567-370">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Отправка в Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    <span data-ttu-id="d6567-372">*Отправка в Azure*</span><span class="sxs-lookup"><span data-stu-id="d6567-372">*Pushing to Azure*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-373">При развертывании содержимого на FTP-узле или репозитории GIT веб-приложения, нужно пройти аутентификацию с помощью **учетные данные развертывания** , созданный на основе веб приложения **быстрый запуск** или **панели мониторинга**  страниц управления.</span><span class="sxs-lookup"><span data-stu-id="d6567-373">When you deploy content to the FTP host or GIT repository of a web app, you must authenticate using the **deployment credentials** that you created from the web app's **Quick Start** or **Dashboard** management pages.</span></span> <span data-ttu-id="d6567-374">Если вы не знаете, учетные данные развертывания вы можете легко сбросить их с помощью портала управления.</span><span class="sxs-lookup"><span data-stu-id="d6567-374">If you do not know your deployment credentials you can easily reset them using the management portal.</span></span> <span data-ttu-id="d6567-375">Откройте веб-приложение **панели мониторинга** и нажмите кнопку **сбросить учетные данные развертывания** ссылку.</span><span class="sxs-lookup"><span data-stu-id="d6567-375">Open the web app **Dashboard** page and click the **Reset your deployment credentials** link.</span></span> <span data-ttu-id="d6567-376">Укажите новый пароль и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d6567-376">Provide a new password and click **OK**.</span></span> <span data-ttu-id="d6567-377">Учетные данные развертывания могут использоваться для всех веб-приложений, связанных с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="d6567-377">Deployment credentials are valid for use with all web apps associated with your subscription.</span></span>
10. <span data-ttu-id="d6567-378">Чтобы проверить веб-приложение было успешно отправлено в Azure, вернитесь на портал управления и нажмите кнопку **веб-сайтов**.</span><span class="sxs-lookup"><span data-stu-id="d6567-378">In order to verify the web app was successfully pushed to Azure, go back to the management portal and click **Websites**.</span></span>
11. <span data-ttu-id="d6567-379">Найдите веб-приложения и разверните запись для отображения слот промежуточного сайта.</span><span class="sxs-lookup"><span data-stu-id="d6567-379">Locate your web app and expand the entry to display the staging site slot.</span></span> <span data-ttu-id="d6567-380">Щелкните его **имя** чтобы перейти на страницу управления.</span><span class="sxs-lookup"><span data-stu-id="d6567-380">Click its **Name** to go to the management page.</span></span>
12. <span data-ttu-id="d6567-381">Нажмите кнопку **развертываний** для см. в разделе **журнала развертывания**.</span><span class="sxs-lookup"><span data-stu-id="d6567-381">Click **Deployments** to see the **deployment history**.</span></span> <span data-ttu-id="d6567-382">Убедитесь в наличии **активное развертывание** с вашей  *&quot;начальной фиксации&quot;*.</span><span class="sxs-lookup"><span data-stu-id="d6567-382">Verify that there is an **Active Deployment** with your *&quot;Initial Commit&quot;*.</span></span>

    ![Активное развертывание](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    <span data-ttu-id="d6567-384">*Активное развертывание*</span><span class="sxs-lookup"><span data-stu-id="d6567-384">*Active deployment*</span></span>
13. <span data-ttu-id="d6567-385">Наконец, нажмите кнопку **Обзор** на панели команд, чтобы перейти на веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d6567-385">Finally, click **Browse** in the command bar to go to the web app.</span></span>

    ![Обзор веб-приложения](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    <span data-ttu-id="d6567-387">*Обзор веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="d6567-387">*Browse web app*</span></span>
14. <span data-ttu-id="d6567-388">Если приложение успешно развернуто, вы увидите страницу входа Компьютерщик тест.</span><span class="sxs-lookup"><span data-stu-id="d6567-388">If the application is successfully deployed, you will see the Geek Quiz login page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-389">URL-адрес развернутого приложения содержит имя веб-приложения, за которым следует *-промежуточных*.</span><span class="sxs-lookup"><span data-stu-id="d6567-389">The address URL of the deployed application contains the name of your web app followed by *-staging*.</span></span>

    ![Приложения, работающего в промежуточной среде](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    <span data-ttu-id="d6567-391">*Приложения, работающего в промежуточной среде*</span><span class="sxs-lookup"><span data-stu-id="d6567-391">*Application running in the staging environment*</span></span>
15. <span data-ttu-id="d6567-392">Если вы хотите просмотреть приложение, нажмите кнопку **зарегистрировать** для регистрации нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="d6567-392">If you wish to explore the application, click **Register** to register a new user.</span></span> <span data-ttu-id="d6567-393">Выполните действия, учетной записи, указав имя пользователя, адрес электронной почты и пароль.</span><span class="sxs-lookup"><span data-stu-id="d6567-393">Complete the account details by entering a user name, email address and password.</span></span> <span data-ttu-id="d6567-394">Затем приложение показывает первый вопрос головоломки.</span><span class="sxs-lookup"><span data-stu-id="d6567-394">Next, the application shows the first question of the quiz.</span></span> <span data-ttu-id="d6567-395">Ответьте на несколько вопросов, чтобы убедиться в том, что оно работает должным образом.</span><span class="sxs-lookup"><span data-stu-id="d6567-395">Answer a few questions to make sure it is working as expected.</span></span>

    ![Приложения готовы к использованию](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    <span data-ttu-id="d6567-397">*Приложения готовы к использованию*</span><span class="sxs-lookup"><span data-stu-id="d6567-397">*Application ready to be used*</span></span>

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a><span data-ttu-id="d6567-398">Задача 4 — повышение уровня веб-приложения в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="d6567-398">Task 4 – Promoting the Web App to Production</span></span>

<span data-ttu-id="d6567-399">Теперь, когда вы убедились, что веб-приложение работает правильно, в промежуточной среде, можно приступать к его распространение в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="d6567-399">Now that you have verified that the web app is working correctly in the staging environment, you are ready to promote it to production.</span></span> <span data-ttu-id="d6567-400">В этой задаче будет переключить слот промежуточного сайта узла на рабочий слот.</span><span class="sxs-lookup"><span data-stu-id="d6567-400">In this task, you will swap the staging site slot with the production site slot.</span></span>

1. <span data-ttu-id="d6567-401">Вернитесь на портал управления и выберите слот промежуточного сайта.</span><span class="sxs-lookup"><span data-stu-id="d6567-401">Go back to the management portal and select the staging site slot.</span></span> <span data-ttu-id="d6567-402">Нажмите кнопку **замены** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="d6567-402">Click **Swap** in the command bar.</span></span>

    ![На производственную](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    <span data-ttu-id="d6567-404">*На производственную*</span><span class="sxs-lookup"><span data-stu-id="d6567-404">*Swap to production*</span></span>
2. <span data-ttu-id="d6567-405">Нажмите кнопку **Да** в диалоговом окне подтверждения, чтобы продолжить операцию замены.</span><span class="sxs-lookup"><span data-stu-id="d6567-405">Click **Yes** in the confirmation dialog box to proceed with the swap operation.</span></span> <span data-ttu-id="d6567-406">Azure будет немедленно Замените содержимое на рабочем сайте с помощью содержимого промежуточного сайта.</span><span class="sxs-lookup"><span data-stu-id="d6567-406">Azure will immediately swap the content of the production site with the content of the staging site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-407">Некоторые параметры из промежуточную версию будет автоматически копироваться в рабочей версии, (например строка соединения переопределений, сопоставления обработчика т. д.), но другие параметры не изменится (например конечные точки DNS, SSL-привязки, и т.д.).</span><span class="sxs-lookup"><span data-stu-id="d6567-407">Some settings from the staged version will automatically be copied to the production version (e.g. connection string overrides, handler mappings, etc.) but other settings will not change (e.g. DNS endpoints, SSL bindings, etc.).</span></span>

    ![Подтверждение операции замены](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    <span data-ttu-id="d6567-409">*Подтверждение операции замены*</span><span class="sxs-lookup"><span data-stu-id="d6567-409">*Confirming swap operation*</span></span>
3. <span data-ttu-id="d6567-410">После завершения переключения выберите рабочий слот и нажмите кнопку **Обзор** на панели команд, чтобы открыть на рабочем сайте.</span><span class="sxs-lookup"><span data-stu-id="d6567-410">Once the swap is complete, select the production slot and click **Browse** in the command bar to open the production site.</span></span> <span data-ttu-id="d6567-411">Обратите внимание, что URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="d6567-411">Notice the URL in the address bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-412">Может потребоваться обновить браузер, чтобы очистить кэш.</span><span class="sxs-lookup"><span data-stu-id="d6567-412">You might need to refresh your browser to clear cache.</span></span> <span data-ttu-id="d6567-413">В Internet Explorer, это можно сделать, нажав клавишу **CTRL + R**.</span><span class="sxs-lookup"><span data-stu-id="d6567-413">In Internet Explorer, you can do this by pressing **CTRL+R**.</span></span>

    ![Веб-приложения в рабочей среде](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. <span data-ttu-id="d6567-415">В **GitBash** консоли, обновите удаленный URL-адрес локального репозитория Git для нацеливания на рабочий слот.</span><span class="sxs-lookup"><span data-stu-id="d6567-415">In the **GitBash** console, update the remote URL for the local Git repository to target the production slot.</span></span> <span data-ttu-id="d6567-416">Чтобы сделать это, выполните следующую команду, заменив заполнители имя пользователя для развертывания и имя веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d6567-416">To do this, run the following command replacing the placeholders with your deployment username and the name of your web app.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-417">В следующих упражнениях изменения будут переданы на рабочий сайт вместо промежуточной для простоты мы лаборатории.</span><span class="sxs-lookup"><span data-stu-id="d6567-417">In the following exercises, you will push changes to the production site instead of staging just for the simplicity of the lab.</span></span> <span data-ttu-id="d6567-418">В реальной ситуации рекомендуется проверить изменения в промежуточной среде перед повышением роли в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="d6567-418">In a real-world scenario, it is recommended to verify the changes in the staging environment before promoting to production.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a><span data-ttu-id="d6567-419">Упражнение 3: Выполняя откат развертывания в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="d6567-419">Exercise 3: Performing Deployment Rollback in Production</span></span>

<span data-ttu-id="d6567-420">Существуют сценарии, где у вас нет промежуточный слот развертывания для выполнения горячей замены между промежуточной и рабочей среде, например, если вы работаете с **бесплатный** или **Shared** режим.</span><span class="sxs-lookup"><span data-stu-id="d6567-420">There are scenarios where you do not have a staging slot to perform hot swap between staging and production, for example, if you are working with **Free** or **Shared** mode.</span></span> <span data-ttu-id="d6567-421">В этих случаях следует протестировать приложение в тестовой среде — локально или на удаленном сайте — перед развертыванием в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="d6567-421">In those scenarios, you should test your application in a testing environment –either locally or in a remote site– before deploying to production.</span></span> <span data-ttu-id="d6567-422">Тем не менее существует возможность, что проблема не обнаруживается во время этапа тестирования может возникнуть на рабочем сайте.</span><span class="sxs-lookup"><span data-stu-id="d6567-422">However, it is possible that an issue not detected during the testing phase may arise in the production site.</span></span> <span data-ttu-id="d6567-423">В этом случае важно иметь механизм легко переключиться на предыдущий и стабильнее версию приложения как можно быстрее.</span><span class="sxs-lookup"><span data-stu-id="d6567-423">In this case, it is important to have a mechanism to easily switch to a previous and more stable version of the application as quickly as possible.</span></span>

<span data-ttu-id="d6567-424">В **службе приложений Azure**, непрерывное развертывание из системы управления версиями делает это возможно благодаря **повторно развернуть** действии, доступном на портале управления.</span><span class="sxs-lookup"><span data-stu-id="d6567-424">In **Azure App Service**, continuous deployment from source control makes this possible thanks to the **redeploy** action available in the management portal.</span></span> <span data-ttu-id="d6567-425">Azure отслеживает развертываний, которые связаны с фиксациями, отправлен в репозиторий и предоставляет возможность повторного развертывания приложения с помощью любого из предыдущих развертываний, в любое время.</span><span class="sxs-lookup"><span data-stu-id="d6567-425">Azure keeps track of the deployments associated with the commits pushed to the repository and provides an option to redeploy your application using any of your previous deployments, at any time.</span></span>

<span data-ttu-id="d6567-426">В этом упражнении будет выполнять изменений в код в **Quiz Компьютерщик** приложение, которое намеренно внедряет *ошибки*.</span><span class="sxs-lookup"><span data-stu-id="d6567-426">In this exercise you will perform a change to the code in the **Geek Quiz** application that intentionally injects a *bug*.</span></span> <span data-ttu-id="d6567-427">Вы развернете приложение в рабочей среде, чтобы просмотреть ошибку, и можно будет воспользоваться преимуществами функции повторного развертывания, чтобы вернуться к предыдущему состоянию.</span><span class="sxs-lookup"><span data-stu-id="d6567-427">You will deploy the application to production to see the error, and then you will take advantage of the redeploy feature to go back to the previous state.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a><span data-ttu-id="d6567-428">Задача 1 – обновление приложения Quiz Компьютерщик</span><span class="sxs-lookup"><span data-stu-id="d6567-428">Task 1 – Updating the Geek Quiz Application</span></span>

<span data-ttu-id="d6567-429">В этой задаче будет рефакторинг небольшой фрагмент кода из **TriviaController** класса для извлечения часть логики, которая извлекает выбранный тестирование из базы данных в новый метод.</span><span class="sxs-lookup"><span data-stu-id="d6567-429">In this task, you will refactor a small piece of code of the **TriviaController** class to extract part of the logic that retrieves the selected quiz option from the database into a new method.</span></span>

1. <span data-ttu-id="d6567-430">Перейдите к экземпляру Visual Studio с **GeekQuiz** решение из предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="d6567-430">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="d6567-431">В **обозревателе решений**откройте **TriviaController.cs** внутри **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="d6567-431">In **Solution Explorer**, open the **TriviaController.cs** file inside the **Controllers** folder.</span></span>
3. <span data-ttu-id="d6567-432">Найдите **StoreAsync** метод и выберите код, выделенный на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="d6567-432">Locate the **StoreAsync** method and select the code highlighted in the following figure.</span></span>

    ![Выберите код](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    <span data-ttu-id="d6567-434">*Выберите код*</span><span class="sxs-lookup"><span data-stu-id="d6567-434">*Selecting the code*</span></span>
4. <span data-ttu-id="d6567-435">Щелкните правой кнопкой мыши выделенный код, разверните узел **рефакторинг** меню и выберите **извлечение метода...** .</span><span class="sxs-lookup"><span data-stu-id="d6567-435">Right-click the selected code, expand the **Refactor** menu and select **Extract Method...**.</span></span>

    ![Извлечение кода как новый метод](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    <span data-ttu-id="d6567-437">*Выбор метода извлечения*</span><span class="sxs-lookup"><span data-stu-id="d6567-437">*Selecting Extract Method*</span></span>
5. <span data-ttu-id="d6567-438">В **извлечение метода** диалоговом окне имя нового метода *MatchesOption* и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d6567-438">In the **Extract Method** dialog box, name the new method *MatchesOption* and click **OK**.</span></span>

    ![Указывая имя метода](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    <span data-ttu-id="d6567-440">*Указание имени для извлечения метода*</span><span class="sxs-lookup"><span data-stu-id="d6567-440">*Specifying the name for the extracted method*</span></span>
6. <span data-ttu-id="d6567-441">Выделенный код затем извлекается в **MatchesOption** метод.</span><span class="sxs-lookup"><span data-stu-id="d6567-441">The selected code is then extracted into the **MatchesOption** method.</span></span> <span data-ttu-id="d6567-442">В следующем фрагменте показан итоговый код.</span><span class="sxs-lookup"><span data-stu-id="d6567-442">The resulting code is shown in the following snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. <span data-ttu-id="d6567-443">Нажмите клавишу **CTRL + S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="d6567-443">Press **CTRL + S** to save the changes.</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a><span data-ttu-id="d6567-444">Задача 2 — повторного развертывания приложения Quiz Компьютерщик</span><span class="sxs-lookup"><span data-stu-id="d6567-444">Task 2 – Redeploying the Geek Quiz Application</span></span>

<span data-ttu-id="d6567-445">Теперь будет перенести изменения, внесенные в предыдущей задаче, в хранилище, которое будет активировать новое развертывание в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="d6567-445">You will now push the changes you made in the previous task to the repository, which will trigger a new deployment to the production environment.</span></span> <span data-ttu-id="d6567-446">Затем будет устраняются проблемы с помощью **средства разработки F12** обеспечивает Internet Explorer, а затем выполнить откат для предыдущего развертывания на портале управления Azure.</span><span class="sxs-lookup"><span data-stu-id="d6567-446">Then, you will troubleshot an issue using the **F12 development tools** provided by Internet Explorer, and then perform a rollback to the previous deployment from the Azure management portal.</span></span>

1. <span data-ttu-id="d6567-447">Откройте новую **Git Bash** консоли, чтобы развернуть обновленное приложение в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="d6567-447">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
2. <span data-ttu-id="d6567-448">Выполните следующие команды для внесения изменений в Azure.</span><span class="sxs-lookup"><span data-stu-id="d6567-448">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="d6567-449">Обновление *[YOUR-APPLICATION-PATH]* заполнитель с путем к **GeekQuiz** решения.</span><span class="sxs-lookup"><span data-stu-id="d6567-449">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="d6567-450">Вам будет предложено ввести пароль развертывания.</span><span class="sxs-lookup"><span data-stu-id="d6567-450">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Отправка оптимизированный код поясняется в Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    <span data-ttu-id="d6567-452">*Отправка оптимизированный код поясняется в Azure*</span><span class="sxs-lookup"><span data-stu-id="d6567-452">*Pushing refactored code to Azure*</span></span>
3. <span data-ttu-id="d6567-453">Откройте Internet Explorer и перейдите к веб-приложения (например `http://<your-web-site>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="d6567-453">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="d6567-454">Вход с помощью ранее созданные учетные данные.</span><span class="sxs-lookup"><span data-stu-id="d6567-454">Log in using the previously created credentials.</span></span>
4. <span data-ttu-id="d6567-455">Нажмите клавишу **F12** для запуска средства разработки, выберите **сети** вкладку и нажмите кнопку **воспроизведение** кнопку, чтобы начать запись.</span><span class="sxs-lookup"><span data-stu-id="d6567-455">Press **F12** to launch the development tools, select the **Network** tab and click the **Play** button to start recording.</span></span>

    <span data-ttu-id="d6567-456">![Запуск записи сетевых](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "начиная запись сети")</span><span class="sxs-lookup"><span data-stu-id="d6567-456">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Starting network recording")</span></span>

    <span data-ttu-id="d6567-457">*Начальная запись сети*</span><span class="sxs-lookup"><span data-stu-id="d6567-457">*Starting network recording*</span></span>
5. <span data-ttu-id="d6567-458">Выберите любой параметр головоломки.</span><span class="sxs-lookup"><span data-stu-id="d6567-458">Select any option of the quiz.</span></span> <span data-ttu-id="d6567-459">Вы увидите, что ничего не происходит.</span><span class="sxs-lookup"><span data-stu-id="d6567-459">You will see that nothing happens.</span></span>
6. <span data-ttu-id="d6567-460">В **F12** окна, элемент, соответствующий HTTP-запроса POST показывает HTTP **500** результат.</span><span class="sxs-lookup"><span data-stu-id="d6567-460">In the **F12** window, the entry corresponding to the POST HTTP request shows an HTTP **500** result.</span></span>

    ![HTTP 500 ошибок](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    <span data-ttu-id="d6567-462">*HTTP 500 ошибок*</span><span class="sxs-lookup"><span data-stu-id="d6567-462">*HTTP 500 error*</span></span>
7. <span data-ttu-id="d6567-463">Выберите **консоли** вкладки. Регистрирует ошибку в журнале с описанием причины.</span><span class="sxs-lookup"><span data-stu-id="d6567-463">Select the **Console** tab. An error is logged with the details of the cause.</span></span>

    ![Записанная ошибка](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    <span data-ttu-id="d6567-465">*Записанная ошибка*</span><span class="sxs-lookup"><span data-stu-id="d6567-465">*Logged error*</span></span>
8. <span data-ttu-id="d6567-466">Найдите часть сведений об ошибке.</span><span class="sxs-lookup"><span data-stu-id="d6567-466">Locate the details part of the error.</span></span> <span data-ttu-id="d6567-467">Очевидно Эта ошибка вызвана кода, рефакторинг вы зафиксирована на предыдущих шагах.</span><span class="sxs-lookup"><span data-stu-id="d6567-467">Clearly, this error is caused by the code refactoring you committed in the previous steps.</span></span>

    <span data-ttu-id="d6567-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span><span class="sxs-lookup"><span data-stu-id="d6567-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span></span>
9. <span data-ttu-id="d6567-469">Не закрывайте браузер.</span><span class="sxs-lookup"><span data-stu-id="d6567-469">Do not close the browser.</span></span>
10. <span data-ttu-id="d6567-470">В новом экземпляре браузера, перейдите к [портал управления Azure](https://manage.windowsazure.com) и войдите с использованием учетной записи Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="d6567-470">In a new browser instance, navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
11. <span data-ttu-id="d6567-471">Выберите **веб-сайтов** и нажмите кнопку веб-приложения, созданный в упражнении 2.</span><span class="sxs-lookup"><span data-stu-id="d6567-471">Select **Websites** and click the web app you created in Exercise 2.</span></span>
12. <span data-ttu-id="d6567-472">Перейдите к **развертываний** страницы.</span><span class="sxs-lookup"><span data-stu-id="d6567-472">Navigate to the **Deployments** page.</span></span> <span data-ttu-id="d6567-473">Обратите внимание, что выполнены все фиксации, перечислены в журнале развертывания.</span><span class="sxs-lookup"><span data-stu-id="d6567-473">Notice that all the commits performed are listed in the deployment history.</span></span>

    ![Список существующих развертываний](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    <span data-ttu-id="d6567-475">*Список существующих развертываний*</span><span class="sxs-lookup"><span data-stu-id="d6567-475">*List of existing deployments*</span></span>
13. <span data-ttu-id="d6567-476">Выберите предыдущую фиксацию и нажмите кнопку **повторно развернуть** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="d6567-476">Select the previous commit and click **Redeploy** on the command bar.</span></span>

    ![Повторное развертывание предыдущую фиксацию](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    <span data-ttu-id="d6567-478">*Повторное развертывание предыдущую фиксацию*</span><span class="sxs-lookup"><span data-stu-id="d6567-478">*Redeploying the previous commit*</span></span>
14. <span data-ttu-id="d6567-479">При появлении запроса на подтверждение, щелкните **Да**.</span><span class="sxs-lookup"><span data-stu-id="d6567-479">When prompted to confirm, click **Yes**.</span></span>

    ![Подтверждение повторного развертывания](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. <span data-ttu-id="d6567-481">После завершения развертывания переключитесь обратно в экземпляре обозревателя с веб-приложения и нажмите клавишу **CTRL + F5**.</span><span class="sxs-lookup"><span data-stu-id="d6567-481">When the deployment completes, switch back to the browser instance with your web app and press **CTRL + F5**.</span></span>
16. <span data-ttu-id="d6567-482">Щелкните любой из параметров.</span><span class="sxs-lookup"><span data-stu-id="d6567-482">Click any of the options.</span></span> <span data-ttu-id="d6567-483">Подготовить и результат, теперь потребует переворачивающейся анимации (*и неуспешных*) будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="d6567-483">The flip animation will now take place and the result (*correct/incorrect*) will be displayed.</span></span>
17. <span data-ttu-id="d6567-484">(Необязательно) Переключиться в режим **Git Bash** консоли и выполните следующие команды, чтобы вернуться к предыдущей операции фиксации.</span><span class="sxs-lookup"><span data-stu-id="d6567-484">(Optional) Switch to the **Git Bash** console and execute the following commands to revert to the previous commit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-485">Эти команды создают новую фиксацию, которая отменяет все изменения в репозитории Git, которые были внесены в неверный фиксации.</span><span class="sxs-lookup"><span data-stu-id="d6567-485">These commands create a new commit that undoes all changes in the Git repository that were made in the bad commit.</span></span> <span data-ttu-id="d6567-486">Azure затем будет повторное развертывание приложения с помощью новой фиксации.</span><span class="sxs-lookup"><span data-stu-id="d6567-486">Azure will then redeploy the application using the new commit.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a><span data-ttu-id="d6567-487">Упражнение 4: Масштабирования с помощью службы хранилища Azure</span><span class="sxs-lookup"><span data-stu-id="d6567-487">Exercise 4: Scaling Using Azure Storage</span></span>

<span data-ttu-id="d6567-488">**Большие двоичные объекты** — самый простой способ хранения больших объемов неструктурированных текстовых или двоичных данных, таких как видео, аудио и изображения.</span><span class="sxs-lookup"><span data-stu-id="d6567-488">**Blobs** are the simplest way to store large amounts of unstructured text or binary data such as video, audio and images.</span></span> <span data-ttu-id="d6567-489">Перемещение статическое содержимое приложения в хранилище, позволяет масштабировать приложение, обслуживание изображений или документов непосредственно в браузере.</span><span class="sxs-lookup"><span data-stu-id="d6567-489">Moving the static content of your application to Storage, helps to scale your application by serving images or documents directly to the browser.</span></span>

<span data-ttu-id="d6567-490">В этом упражнении будет выполнено перемещение статическое содержимое приложения в контейнер больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="d6567-490">In this exercise, you will move the static content of your application to a Blob container.</span></span> <span data-ttu-id="d6567-491">Затем необходимо будет настроить приложение таким образом, чтобы добавить **правила переопределения URL-адреса ASP.NET** в **Web.config** для перенаправления содержимое в контейнер больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="d6567-491">Then you will configure your application to add an **ASP.NET URL rewrite rule** in the **Web.config** to redirect your content to the Blob container.</span></span>

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a><span data-ttu-id="d6567-492">Задача 1 – Создание учетной записи хранения Azure</span><span class="sxs-lookup"><span data-stu-id="d6567-492">Task 1 – Creating an Azure Storage Account</span></span>

<span data-ttu-id="d6567-493">В этой задаче вы узнаете, как создать учетную запись хранения с помощью портала управления.</span><span class="sxs-lookup"><span data-stu-id="d6567-493">In this task you will learn how to create a new storage account using the management portal.</span></span>

1. <span data-ttu-id="d6567-494">Перейдите к [портал управления Azure](https://manage.windowsazure.com) и войдите с использованием учетной записи Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="d6567-494">Navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
2. <span data-ttu-id="d6567-495">Выберите **New | Службы Data Services | Хранилище | Быстрое создание** Чтобы приступить к созданию новой учетной записи хранения.</span><span class="sxs-lookup"><span data-stu-id="d6567-495">Select **New | Data Services | Storage | Quick Create** to start creating a new storage account.</span></span> <span data-ttu-id="d6567-496">Введите уникальное имя для учетной записи и выберите **регион** из списка.</span><span class="sxs-lookup"><span data-stu-id="d6567-496">Enter a unique name for the account and select a **Region** from the list.</span></span> <span data-ttu-id="d6567-497">Нажмите кнопку **создать учетную запись хранения** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="d6567-497">Click **Create Storage Account** to continue.</span></span>

    <span data-ttu-id="d6567-498">![Создание учетной записи хранения](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Создание учетной записи хранения")</span><span class="sxs-lookup"><span data-stu-id="d6567-498">![Creating a new Storage Account](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Creating a new Storage Account")</span></span>

    <span data-ttu-id="d6567-499">*Создание учетной записи хранения*</span><span class="sxs-lookup"><span data-stu-id="d6567-499">*Creating a new storage account*</span></span>
3. <span data-ttu-id="d6567-500">В **хранения** разделе, подождите, пока состояние учетной записи хранения изменится на *Online* чтобы продолжить на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="d6567-500">In the **Storage** section, wait until the status of the new storage account changes to *Online* in order to continue with the following step.</span></span>

    <span data-ttu-id="d6567-501">![Созданную учетную запись хранения](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "созданной учетной записи хранения")</span><span class="sxs-lookup"><span data-stu-id="d6567-501">![Storage Account created](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Storage Account created")</span></span>

    <span data-ttu-id="d6567-502">*Созданную учетную запись хранения*</span><span class="sxs-lookup"><span data-stu-id="d6567-502">*Storage Account created*</span></span>
4. <span data-ttu-id="d6567-503">Щелкните имя учетной записи хранения и нажмите кнопку **панели мониторинга** ссылку в верхней части страницы.</span><span class="sxs-lookup"><span data-stu-id="d6567-503">Click on the storage account name and then click the **Dashboard** link at the top of the page.</span></span> <span data-ttu-id="d6567-504">**Панели мониторинга** страница содержит сведения о состоянии учетной записи и конечные точки службы, которые могут использоваться в приложениях.</span><span class="sxs-lookup"><span data-stu-id="d6567-504">The **Dashboard** page provides you with information about the status of the account and the service endpoints that can be used within your applications.</span></span>

    <span data-ttu-id="d6567-505">![Отображение панели мониторинга учетной записи хранения](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "отображение панели мониторинга учетной записи хранения")</span><span class="sxs-lookup"><span data-stu-id="d6567-505">![Displaying the Storage Account Dashboard](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Displaying the Storage Account Dashboard")</span></span>

    <span data-ttu-id="d6567-506">*Отображение панели мониторинга учетной записи хранения*</span><span class="sxs-lookup"><span data-stu-id="d6567-506">*Displaying the Storage Account Dashboard*</span></span>
5. <span data-ttu-id="d6567-507">Нажмите кнопку **управление ключами доступа** кнопки на панели навигации.</span><span class="sxs-lookup"><span data-stu-id="d6567-507">Click the **Manage Access Keys** button in the navigation bar.</span></span>

    <span data-ttu-id="d6567-508">!["Управление ключами доступа"](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "кнопку Управление ключами доступа")</span><span class="sxs-lookup"><span data-stu-id="d6567-508">![Manage Access Keys button](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Manage Access Keys button")</span></span>

    <span data-ttu-id="d6567-509">*Управление кнопкой "ключи доступа"*</span><span class="sxs-lookup"><span data-stu-id="d6567-509">*Manage Access Keys button*</span></span>
6. <span data-ttu-id="d6567-510">В **управление ключами доступа** диалоговое окно, скопируйте **имя учетной записи хранения** и **первичный ключ доступа** так, как они понадобятся в следующем упражнении.</span><span class="sxs-lookup"><span data-stu-id="d6567-510">In the **Manage Access Keys** dialog box, copy the **Storage Account Name** and **Primary Access Key** as you will need them in the following exercise.</span></span> <span data-ttu-id="d6567-511">Закройте диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="d6567-511">Then, close the dialog box.</span></span>

    <span data-ttu-id="d6567-512">![Диалоговое окно Управление ключом доступа](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "диалоговое окно «Управление ключами доступа»")</span><span class="sxs-lookup"><span data-stu-id="d6567-512">![Manage Access Key dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Manage Access Key dialog box")</span></span>

    <span data-ttu-id="d6567-513">*Ключ доступа диалоговое окно управления*</span><span class="sxs-lookup"><span data-stu-id="d6567-513">*Manage Access Key dialog box*</span></span>

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a><span data-ttu-id="d6567-514">Задача 2 – Отправка ресурс-контейнер в хранилище BLOB-объектов Azure</span><span class="sxs-lookup"><span data-stu-id="d6567-514">Task 2 – Uploading an Asset to Azure Blob Storage</span></span>

<span data-ttu-id="d6567-515">В этой задаче в окне обозревателя серверов в Visual Studio будет использовать для подключения к вашей учетной записи хранения.</span><span class="sxs-lookup"><span data-stu-id="d6567-515">In this task, you will use the Server Explorer window from Visual Studio to connect to your storage account.</span></span> <span data-ttu-id="d6567-516">Затем будет создайте контейнер больших двоичных объектов и отправить файл с логотипом Компьютерщик тест в контейнер.</span><span class="sxs-lookup"><span data-stu-id="d6567-516">You will then create a blob container and upload a file with the Geek Quiz logo to the container.</span></span>

1. <span data-ttu-id="d6567-517">Перейдите к экземпляру Visual Studio с **GeekQuiz** решение из предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="d6567-517">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="d6567-518">В строке меню выберите **представление** и нажмите кнопку **обозревателя серверов**.</span><span class="sxs-lookup"><span data-stu-id="d6567-518">From the menu bar, select **View** and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="d6567-519">В **обозревателя серверов**, щелкните правой кнопкой мыши **Azure** узел и выберите **подключение к Azure...** . Вход с учетной записью Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="d6567-519">In **Server Explorer**, right-click the **Azure** node and select **Connect to Azure...**. Sign in using the Microsoft account associated with your subscription.</span></span>

    ![Подключение к Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    <span data-ttu-id="d6567-521">*Подключение к Azure*</span><span class="sxs-lookup"><span data-stu-id="d6567-521">*Connect to Azure*</span></span>
4. <span data-ttu-id="d6567-522">Разверните **Azure** узел, щелкните правой кнопкой мыши **хранения** и выберите **присоединить внешнее хранилище...** .</span><span class="sxs-lookup"><span data-stu-id="d6567-522">Expand the **Azure** node, right-click **Storage** and select **Attach External Storage...**.</span></span>
5. <span data-ttu-id="d6567-523">В **добавить новую учетную запись хранения** диалогового окна введите **имя учетной записи** и **ключ учетной записи** полученный в предыдущей задаче и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d6567-523">In the **Add New Storage Account** dialog box, enter the **Account name** and **Account key** you obtained in the previous task and click **OK**.</span></span>

    ![Добавление новой учетной записи хранения-диалоговое окно](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    <span data-ttu-id="d6567-525">*Добавление новой учетной записи хранения-диалоговое окно*</span><span class="sxs-lookup"><span data-stu-id="d6567-525">*Add New Storage Account dialog box*</span></span>
6. <span data-ttu-id="d6567-526">Учетной записи хранения должен появиться узел **хранения** узла.</span><span class="sxs-lookup"><span data-stu-id="d6567-526">Your storage account should appear under the **Storage** node.</span></span> <span data-ttu-id="d6567-527">Разверните свою учетную запись хранения, щелкните правой кнопкой мыши **большие двоичные объекты** и выберите **создать контейнер BLOB-объектов...** .</span><span class="sxs-lookup"><span data-stu-id="d6567-527">Expand your storage account, right-click **Blobs** and select **Create Blob Container...**.</span></span>

    <span data-ttu-id="d6567-528">![Создайте контейнер больших двоичных объектов](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "создать контейнер больших двоичных объектов")</span><span class="sxs-lookup"><span data-stu-id="d6567-528">![Create Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Create Blob Container")</span></span>

    <span data-ttu-id="d6567-529">*Создайте контейнер больших двоичных объектов*</span><span class="sxs-lookup"><span data-stu-id="d6567-529">*Create Blob Container*</span></span>
7. <span data-ttu-id="d6567-530">В **создать контейнер больших двоичных объектов** диалогового окна введите имя контейнера больших двоичных объектов и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d6567-530">In the **Create Blob Container** dialog box, enter a name for the blob container and click **OK**.</span></span>

    <span data-ttu-id="d6567-531">![Диалоговое окно Создание контейнера больших двоичных объектов](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "диалоговое окно создания контейнера больших двоичных объектов")</span><span class="sxs-lookup"><span data-stu-id="d6567-531">![Create Blob Container dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Create Blob Container dialog box")</span></span>

    <span data-ttu-id="d6567-532">*Создание диалогового окна контейнера больших двоичных объектов*</span><span class="sxs-lookup"><span data-stu-id="d6567-532">*Create Blob Container dialog box*</span></span>
8. <span data-ttu-id="d6567-533">Новый контейнер больших двоичных объектов, которые должны добавляться к **большие двоичные объекты** узла.</span><span class="sxs-lookup"><span data-stu-id="d6567-533">The new blob container should be added to the **Blobs** node.</span></span> <span data-ttu-id="d6567-534">Измените разрешения доступа в контейнере, чтобы сделать контейнер общедоступным.</span><span class="sxs-lookup"><span data-stu-id="d6567-534">Change the access permissions in the container to make the container public.</span></span> <span data-ttu-id="d6567-535">Чтобы сделать это, щелкните правой кнопкой мыши **образы** контейнер и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="d6567-535">To do this, right-click the **images** container and select **Properties**.</span></span>

    <span data-ttu-id="d6567-536">![свойства контейнера изображений](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "свойства контейнера изображений")</span><span class="sxs-lookup"><span data-stu-id="d6567-536">![images container properties](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "images container properties")</span></span>

    <span data-ttu-id="d6567-537">*Свойства образа контейнера*</span><span class="sxs-lookup"><span data-stu-id="d6567-537">*Images container properties*</span></span>
9. <span data-ttu-id="d6567-538">В **свойства** окне **общий доступ на чтение** для **контейнера**.</span><span class="sxs-lookup"><span data-stu-id="d6567-538">In the **Properties** window, set the **Public Read Access** to **Container**.</span></span>

    <span data-ttu-id="d6567-539">![Изменение свойства общего доступа на чтение](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "изменение свойства общего доступа на чтение")</span><span class="sxs-lookup"><span data-stu-id="d6567-539">![Changing public read access property](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Changing public read access property")</span></span>

    <span data-ttu-id="d6567-540">*Изменение свойства общего доступа на чтение*</span><span class="sxs-lookup"><span data-stu-id="d6567-540">*Changing public read access property*</span></span>
10. <span data-ttu-id="d6567-541">При появлении запроса, если действительно требуется изменить свойства общего доступа, нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="d6567-541">When prompted if you are sure you want to change the public access property, click **Yes**.</span></span>

    <span data-ttu-id="d6567-542">![Microsoft Visual Studio предупреждение](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "предупреждение Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="d6567-542">![Microsoft Visual Studio warning](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="d6567-543">*Microsoft Visual Studio предупреждение*</span><span class="sxs-lookup"><span data-stu-id="d6567-543">*Microsoft Visual Studio warning*</span></span>
11. <span data-ttu-id="d6567-544">В **обозревателя серверов**, щелкните правой кнопкой мыши в **образы** контейнер больших двоичных объектов и выберите **контейнер больших двоичных объектов представления**.</span><span class="sxs-lookup"><span data-stu-id="d6567-544">In **Server Explorer**, right-click in the **images** blob container and select **View Blob Container**.</span></span>

    <span data-ttu-id="d6567-545">![Просмотреть контейнер больших двоичных объектов](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "просмотреть контейнер больших двоичных объектов")</span><span class="sxs-lookup"><span data-stu-id="d6567-545">![View Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "View Blob Container")</span></span>

    <span data-ttu-id="d6567-546">*Представление контейнера больших двоичных объектов*</span><span class="sxs-lookup"><span data-stu-id="d6567-546">*View Blob Container*</span></span>
12. <span data-ttu-id="d6567-547">Образы контейнеров следует открыть в новом окне и отображать условные обозначения без элементов.</span><span class="sxs-lookup"><span data-stu-id="d6567-547">The images container should open in a new window and a legend with no entries should be shown.</span></span> <span data-ttu-id="d6567-548">Нажмите кнопку **отправить** значок, чтобы отправить файл в контейнер больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="d6567-548">Click the **upload** icon to upload a file to the blob container.</span></span>

    <span data-ttu-id="d6567-549">![Образы контейнеров без элементов](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "образы контейнеров без элементов")</span><span class="sxs-lookup"><span data-stu-id="d6567-549">![Images container with no entries](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Images container with no entries")</span></span>

    <span data-ttu-id="d6567-550">*Образы контейнеров без элементов*</span><span class="sxs-lookup"><span data-stu-id="d6567-550">*Images container with no entries*</span></span>
13. <span data-ttu-id="d6567-551">В **отправки большого двоичного объекта** диалогового окна выберите **активы** папку лаборатории.</span><span class="sxs-lookup"><span data-stu-id="d6567-551">In the **Upload Blob** dialog box, navigate to the **Assets** folder of the lab.</span></span> <span data-ttu-id="d6567-552">Выберите **логотип big.png** файл и нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="d6567-552">Select the **logo-big.png** file and click **Open**.</span></span>
14. <span data-ttu-id="d6567-553">Подождите, пока файл передается.</span><span class="sxs-lookup"><span data-stu-id="d6567-553">Wait until the file is uploaded.</span></span> <span data-ttu-id="d6567-554">По завершении отправки файл должен быть указан в контейнере изображения.</span><span class="sxs-lookup"><span data-stu-id="d6567-554">When the upload completes, the file should be listed in the images container.</span></span> <span data-ttu-id="d6567-555">Щелкните правой кнопкой мыши запись файла и выберите **Копировать URL-адрес**.</span><span class="sxs-lookup"><span data-stu-id="d6567-555">Right-click the file entry and select **Copy URL**.</span></span>

    <span data-ttu-id="d6567-556">![Скопируйте URL-адрес большого двоичного объекта](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "скопируйте URL-адрес файла BLOB-объектов")</span><span class="sxs-lookup"><span data-stu-id="d6567-556">![Copy blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copy blob file URL")</span></span>

    <span data-ttu-id="d6567-557">*Скопируйте URL-адрес BLOB-объектов*</span><span class="sxs-lookup"><span data-stu-id="d6567-557">*Copy blob URL*</span></span>
15. <span data-ttu-id="d6567-558">Откройте Internet Explorer и вставьте URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="d6567-558">Open Internet Explorer and paste the URL.</span></span> <span data-ttu-id="d6567-559">В обозревателе, должно быть показано ниже.</span><span class="sxs-lookup"><span data-stu-id="d6567-559">The following image should be shown in the browser.</span></span>

    <span data-ttu-id="d6567-560">![Логотип big.png изображения из хранилища больших двоичных объектов Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "big.png логотип изображения из хранилища")</span><span class="sxs-lookup"><span data-stu-id="d6567-560">![logo-big.png image from Windows Blob Storage](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo-big.png image from storage")</span></span>

    <span data-ttu-id="d6567-561">*Логотип big.png изображения из хранилища BLOB-объектов Azure*</span><span class="sxs-lookup"><span data-stu-id="d6567-561">*logo-big.png image from Azure Blob Storage*</span></span>

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a><span data-ttu-id="d6567-562">Задача 3 – обновление решения для использования статического содержимого из хранилища BLOB-объектов Azure</span><span class="sxs-lookup"><span data-stu-id="d6567-562">Task 3 – Updating the Solution to Consume Static Content from Azure Blob Storage</span></span>

<span data-ttu-id="d6567-563">В этой задаче вы настроите **GeekQuiz** решение, чтобы использовать изображение отправлен в хранилище BLOB-объектов Azure (вместо образа, расположенного в веб-приложения), добавив правило переопределения URL-адреса ASP.NET в **web.config**файл.</span><span class="sxs-lookup"><span data-stu-id="d6567-563">In this task, you will configure the **GeekQuiz** solution to consume the image uploaded to Azure Blob Storage (instead of the image located in the web app) by adding an ASP.NET URL rewrite rule in the **web.config** file.</span></span>

1. <span data-ttu-id="d6567-564">В Visual Studio откройте **Web.config** внутри **GeekQuiz** проекта и найдите **&lt;system.webServer&gt;** элемент.</span><span class="sxs-lookup"><span data-stu-id="d6567-564">In Visual Studio, open the **Web.config** file inside the **GeekQuiz** project and locate the **&lt;system.webServer&gt;** element.</span></span>
2. <span data-ttu-id="d6567-565">Добавьте следующий код, чтобы добавить URL-адрес правила переопределения, обновление заполнитель именем учетной записи хранения.</span><span class="sxs-lookup"><span data-stu-id="d6567-565">Add the following code to add an URL rewrite rule, updating the placeholder with your storage account name.</span></span>

    <span data-ttu-id="d6567-566">(Code Snippet - *UrlRewriteRule WebSitesInProduction - Ex4 -*)</span><span class="sxs-lookup"><span data-stu-id="d6567-566">(Code Snippet - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span></span>

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > <span data-ttu-id="d6567-567">Переопределение URL-адресов — это процесс перехвата входящего веб-запроса и перенаправляет их на другой ресурс.</span><span class="sxs-lookup"><span data-stu-id="d6567-567">URL rewriting is the process of intercepting an incoming Web request and redirecting the request to a different resource.</span></span> <span data-ttu-id="d6567-568">Правила переписывания URL подсистема переписывания при необходимости перенаправить запрос и куда они должны перенаправляться.</span><span class="sxs-lookup"><span data-stu-id="d6567-568">The URL rewriting rules tells the rewriting engine when a request needs to be redirected, and where should they be redirected.</span></span> <span data-ttu-id="d6567-569">Переписывания правило состоит из двух строк: шаблон для поиска в запрошенного URL-адреса (как правило, с помощью регулярных выражений), и строка для замены шаблона, в том случае, если найдена.</span><span class="sxs-lookup"><span data-stu-id="d6567-569">A rewriting rule is composed of two strings: the pattern to look for in the requested URL (usually, using regular expressions), and the string to replace the pattern with, if found.</span></span> <span data-ttu-id="d6567-570">Дополнительные сведения см. в разделе [переписывание URL-адресов в ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span><span class="sxs-lookup"><span data-stu-id="d6567-570">For more information, see [URL Rewriting in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span></span>
3. <span data-ttu-id="d6567-571">Нажмите клавишу **CTRL + S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="d6567-571">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="d6567-572">Откройте новую **Git Bash** консоли, чтобы развернуть обновленное приложение в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="d6567-572">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
5. <span data-ttu-id="d6567-573">Выполните следующие команды для внесения изменений в Azure.</span><span class="sxs-lookup"><span data-stu-id="d6567-573">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="d6567-574">Обновление *[YOUR-APPLICATION-PATH]* заполнитель с путем к **GeekQuiz** решения.</span><span class="sxs-lookup"><span data-stu-id="d6567-574">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="d6567-575">Вам будет предложено ввести пароль развертывания.</span><span class="sxs-lookup"><span data-stu-id="d6567-575">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Развертывание обновления в Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    <span data-ttu-id="d6567-577">*Развертывание обновления в Azure*</span><span class="sxs-lookup"><span data-stu-id="d6567-577">*Deploying update to Azure*</span></span>

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a><span data-ttu-id="d6567-578">Задача 4 – Проверка</span><span class="sxs-lookup"><span data-stu-id="d6567-578">Task 4 – Verification</span></span>

<span data-ttu-id="d6567-579">В этой задаче вы воспользуетесь **Internet Explorer** для просмотра **Quiz Компьютерщик** приложения и установите флажок, что URL-адрес перепишите правило для образов сработало правильно и вы будете перенаправлены на образ, размещенный в **BLOB-объектов Azure Хранилище**.</span><span class="sxs-lookup"><span data-stu-id="d6567-579">In this task you will use **Internet Explorer** to browse the **Geek Quiz** application and check that the URL rewrite rule for images works and you are redirected to the image hosted on **Azure Blob Storage**.</span></span>

1. <span data-ttu-id="d6567-580">Откройте Internet Explorer и перейдите к веб-приложения (например `http://<your-web-site>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="d6567-580">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="d6567-581">Вход с помощью ранее созданные учетные данные.</span><span class="sxs-lookup"><span data-stu-id="d6567-581">Log in using the previously created credentials.</span></span>

    <span data-ttu-id="d6567-582">![Отображение веб-приложение Quiz Компьютерщик с изображением](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "отображение веб-приложение Quiz Компьютерщик с изображением")</span><span class="sxs-lookup"><span data-stu-id="d6567-582">![Showing the Geek Quiz web app with the image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Showing the Geek Quiz web app with the image")</span></span>

    <span data-ttu-id="d6567-583">*Отображение веб-приложение Quiz Компьютерщик с изображением*</span><span class="sxs-lookup"><span data-stu-id="d6567-583">*Showing the Geek Quiz web app with the image*</span></span>
2. <span data-ttu-id="d6567-584">Нажмите клавишу **F12** для запуска средства разработки, выберите **сети** вкладку и начните запись.</span><span class="sxs-lookup"><span data-stu-id="d6567-584">Press **F12** to launch the development tools, select the **Network** tab and start recording.</span></span>

    <span data-ttu-id="d6567-585">![Запуск записи сетевых](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "начиная запись сети")</span><span class="sxs-lookup"><span data-stu-id="d6567-585">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Starting network recording")</span></span>

    <span data-ttu-id="d6567-586">*Начальная запись сети*</span><span class="sxs-lookup"><span data-stu-id="d6567-586">*Starting network recording*</span></span>
3. <span data-ttu-id="d6567-587">Нажмите клавишу **CTRL + F5** для обновления веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="d6567-587">Press **CTRL + F5** to refresh the web page.</span></span>
4. <span data-ttu-id="d6567-588">После завершения загрузки страницы вы увидите HTTP-запроса для **/img/logo-big.png** URL-адрес с помощью HTTP **301** результат (перенаправление), а также выполнить другой запрос для `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL-адрес с помощью HTTP **200** результат.</span><span class="sxs-lookup"><span data-stu-id="d6567-588">Once the page has finished loading, you should see an HTTP request for the **/img/logo-big.png** URL with an HTTP **301** result (redirect) and another request for `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL with a HTTP **200** result.</span></span>

    <span data-ttu-id="d6567-589">![Проверка URL-адрес перенаправления](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "отображение перенаправления в средствах разработки")</span><span class="sxs-lookup"><span data-stu-id="d6567-589">![Verifying the URL redirect](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Showing the redirect in Dev Tools")</span></span>

    <span data-ttu-id="d6567-590">*Проверка URL-адрес перенаправления*</span><span class="sxs-lookup"><span data-stu-id="d6567-590">*Verifying the URL redirect*</span></span>

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a><span data-ttu-id="d6567-591">Упражнение 5: С помощью автоматического масштабирования для веб-приложений</span><span class="sxs-lookup"><span data-stu-id="d6567-591">Exercise 5: Using Autoscale for Web Apps</span></span>

> [!NOTE]
> <span data-ttu-id="d6567-592">В этом упражнении является необязательным, так как она требует поддержки веб-нагрузку &amp; тестирования производительности, который доступен только для **Visual Studio 2013 Ultimate Edition**.</span><span class="sxs-lookup"><span data-stu-id="d6567-592">This exercise is optional, since it requires support for Web Load &amp; Performance Testing which is only available for **Visual Studio 2013 Ultimate Edition**.</span></span> <span data-ttu-id="d6567-593">Дополнительные сведения об определенных функциях Visual Studio 2013 сравнить версии [здесь](https://www.microsoft.com/visualstudio/eng/products/compare).</span><span class="sxs-lookup"><span data-stu-id="d6567-593">For more information on specific Visual Studio 2013 features, compare versions [here](https://www.microsoft.com/visualstudio/eng/products/compare).</span></span>


<span data-ttu-id="d6567-594">**Azure веб-приложения службы** предоставляет функцию автомасштабирования для веб-приложений, работающих в **стандартный режим**.</span><span class="sxs-lookup"><span data-stu-id="d6567-594">**Azure App Service Web Apps** provides the Autoscale feature for web apps running in **Standard Mode**.</span></span> <span data-ttu-id="d6567-595">Автоматическое масштабирование помогает Azure автоматически масштабировала число экземпляров веб-приложения в зависимости от нагрузки.</span><span class="sxs-lookup"><span data-stu-id="d6567-595">Autoscale lets Azure automatically scale the instance count of your web app depending on the load.</span></span> <span data-ttu-id="d6567-596">Если Автомасштабирование включено, Azure проверяет нагрузку ЦП для веб-приложения один раз каждые пять минут и добавляет экземпляры при необходимости в определенный момент времени.</span><span class="sxs-lookup"><span data-stu-id="d6567-596">When Autoscale is enabled, Azure checks the CPU of your web app once every five minutes and adds instances as needed at that point in time.</span></span> <span data-ttu-id="d6567-597">Если ЦП низкая, Azure убирает экземпляры каждые два часа, чтобы убедиться, что не снижается производительность веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d6567-597">If the CPU usage is low, Azure will remove instances once every two hours to ensure that the performance of your web app is not degraded.</span></span>

<span data-ttu-id="d6567-598">В этом упражнении будут рассмотрены шаги, необходимые для настройки **автомасштабирования** возможностей **Quiz Компьютерщик** веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d6567-598">In this exercise you will go through the steps required to configure the **Autoscale** feature for the **Geek Quiz** web app.</span></span> <span data-ttu-id="d6567-599">Эта функция проверит, выполнив нагрузочного теста Visual Studio для создания достаточной нагрузки ЦП в приложении, чтобы активировать обновление экземпляра.</span><span class="sxs-lookup"><span data-stu-id="d6567-599">You will verify this feature by running a Visual Studio load test to generate enough CPU load on the application to trigger an instance upgrade.</span></span>

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a><span data-ttu-id="d6567-600">Задача 1 – Настройка автоматического масштабирования на основе метрики ЦП</span><span class="sxs-lookup"><span data-stu-id="d6567-600">Task 1 – Configuring Autoscale Based on the CPU Metric</span></span>

<span data-ttu-id="d6567-601">В этой задаче будут использовать портал управления Azure для включения функции автоматического масштабирования для веб-приложения, созданный в упражнении 2.</span><span class="sxs-lookup"><span data-stu-id="d6567-601">In this task you will use the Azure management portal to enable the Autoscale feature for the web app you created in Exercise 2.</span></span>

1. <span data-ttu-id="d6567-602">В [портал управления Azure](https://manage.windowsazure.com/)выберите **веб-сайтов** и нажмите кнопку веб-приложения, созданный в упражнении 2.</span><span class="sxs-lookup"><span data-stu-id="d6567-602">In the [Azure management portal](https://manage.windowsazure.com/), select **Websites** and click the web app you created in Exercise 2.</span></span>
2. <span data-ttu-id="d6567-603">Перейдите к **масштабирования** страницы.</span><span class="sxs-lookup"><span data-stu-id="d6567-603">Navigate to the **Scale** page.</span></span> <span data-ttu-id="d6567-604">В разделе **емкость** выберите **ЦП** для **масштабирование по метрике** конфигурации.</span><span class="sxs-lookup"><span data-stu-id="d6567-604">Under the **capacity** section, select **CPU** for the **Scale by Metric** configuration.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-605">При масштабировании по ЦП, Azure динамически регулирует количество экземпляров, которые использует приложение, если ЦП изменяется.</span><span class="sxs-lookup"><span data-stu-id="d6567-605">When scaling by CPU, Azure dynamically adjusts the number of instances that the app uses if the CPU usage changes.</span></span>

    <span data-ttu-id="d6567-606">![При выборе масштабирование по ЦП](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Выбор метрики ЦП для автоматического масштабирования")</span><span class="sxs-lookup"><span data-stu-id="d6567-606">![Selecting to scale by CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selecting the CPU metric for auto scaling")</span></span>

    <span data-ttu-id="d6567-607">*При выборе масштабирование по ЦП*</span><span class="sxs-lookup"><span data-stu-id="d6567-607">*Selecting to scale by CPU*</span></span>
3. <span data-ttu-id="d6567-608">Изменение **целевой ЦП** конфигурацию, чтобы **20**-**40** процентов.</span><span class="sxs-lookup"><span data-stu-id="d6567-608">Change the **Target CPU** configuration to **20**-**40** percent.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-609">Этот диапазон представляет среднее использование ЦП для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d6567-609">This range represents the average CPU usage for your web app.</span></span> <span data-ttu-id="d6567-610">Azure будет добавлять или удалять экземпляры, чтобы удержать веб-приложения в этом диапазоне.</span><span class="sxs-lookup"><span data-stu-id="d6567-610">Azure will add or remove instances to keep your web app in this range.</span></span> <span data-ttu-id="d6567-611">Минимальное и максимальное число экземпляров, используемых для масштабирования указывается в **число экземпляров** конфигурации.</span><span class="sxs-lookup"><span data-stu-id="d6567-611">The minimum and maximum number of instances used for scaling is specified in the **Instance Count** configuration.</span></span> <span data-ttu-id="d6567-612">Azure никогда не перейдем выше или за рамками этого ограничения.</span><span class="sxs-lookup"><span data-stu-id="d6567-612">Azure will never go above or beyond that limit.</span></span>
    > 
    > <span data-ttu-id="d6567-613">Значение по умолчанию **целевой ЦП** значения изменяются только в целях этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="d6567-613">The default **Target CPU** values are modified just for the purposes of this lab.</span></span> <span data-ttu-id="d6567-614">Настроив диапазон ЦП с небольшими значениями, расширить возможности для триггера автомасштабирования, когда Средняя нагрузка на приложение.</span><span class="sxs-lookup"><span data-stu-id="d6567-614">By configuring the CPU range with small values, you are increasing the chances to trigger Autoscale when a moderate load is placed on the application.</span></span>

    <span data-ttu-id="d6567-615">![Изменение целевой ЦП, чтобы быть в диапазоне от 20 до 40 процентов](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Изменение целевой ЦП находиться в диапазоне от 20 до 40 процентов")</span><span class="sxs-lookup"><span data-stu-id="d6567-615">![Changing the target CPU to be between 20 and 40 percent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Changing the target CPU to be between 20 and 40 percent")</span></span>

    <span data-ttu-id="d6567-616">*Изменение целевой ЦП, чтобы быть в диапазоне от 20 до 40 процентов*</span><span class="sxs-lookup"><span data-stu-id="d6567-616">*Changing the Target CPU to be between 20 and 40 percent*</span></span>
4. <span data-ttu-id="d6567-617">Нажмите кнопку **Сохранить** на панели команд, чтобы сохранить изменения.</span><span class="sxs-lookup"><span data-stu-id="d6567-617">Click **Save** in the command bar to save the changes.</span></span>

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a><span data-ttu-id="d6567-618">Задача 2 — нагрузочного тестирования с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6567-618">Task 2 – Load Testing with Visual Studio</span></span>

<span data-ttu-id="d6567-619">Теперь, когда **автомасштабирования** была настроена, вы создадите **веб-тестами производительности и загрузить тестовый проект** в Visual Studio создать нагрузку на ЦП на веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d6567-619">Now that **Autoscale** has been configured, you will create a **Web Performance and Load Test Project** in Visual Studio to generate some CPU load on your web app.</span></span>

1. <span data-ttu-id="d6567-620">Откройте **Visual Studio Ultimate 2013** и выберите **файл | Новые | Проект...**  для запуска нового решения.</span><span class="sxs-lookup"><span data-stu-id="d6567-620">Open **Visual Studio Ultimate 2013** and select **File | New | Project...** to start a new solution.</span></span>

    <span data-ttu-id="d6567-621">![Создание нового проекта](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "создания нового проекта")</span><span class="sxs-lookup"><span data-stu-id="d6567-621">![Creating a new project](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Creating a new project")</span></span>

    <span data-ttu-id="d6567-622">*Создание нового проекта*</span><span class="sxs-lookup"><span data-stu-id="d6567-622">*Creating a new project*</span></span>
2. <span data-ttu-id="d6567-623">В **новый проект** выберите **веб-тестами производительности и нагрузочных тестов** под **Visual C# | Тест** вкладки. Убедитесь, что **.NET Framework 4.5** — флажок установлен, назовите проект *WebAndLoadTestProject*, выберите **расположение** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d6567-623">In the **New Project** dialog box, select **Web Performance and Load Test Project** under the **Visual C# | Test** tab. Make sure **.NET Framework 4.5** is selected, name the project *WebAndLoadTestProject*, choose a **Location** and click **OK**.</span></span>

    <span data-ttu-id="d6567-624">![Создание проекта нагрузочного теста и веб-](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "создания нагрузочного теста и веб-проекта")</span><span class="sxs-lookup"><span data-stu-id="d6567-624">![Creating a new Web and Load Test project](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Creating a new Web and Load Test project")</span></span>

    <span data-ttu-id="d6567-625">*Создание нового проекта веб- и нагрузочного теста*</span><span class="sxs-lookup"><span data-stu-id="d6567-625">*Creating a new Web and Load Test project*</span></span>
3. <span data-ttu-id="d6567-626">В **WebTest1.webtest** щелкните правой кнопкой мыши **WebTest1** узел и нажмите кнопку **добавить запрос**.</span><span class="sxs-lookup"><span data-stu-id="d6567-626">In the **WebTest1.webtest** Right-click the **WebTest1** node and click **Add Request**.</span></span>

    <span data-ttu-id="d6567-627">![Добавление запроса в WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Добавление WebTest1 запрос")</span><span class="sxs-lookup"><span data-stu-id="d6567-627">![Adding a request to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Adding a request to WebTest1")</span></span>

    <span data-ttu-id="d6567-628">*Добавление запроса в WebTest1*</span><span class="sxs-lookup"><span data-stu-id="d6567-628">*Adding a request to WebTest1*</span></span>
4. <span data-ttu-id="d6567-629">В **свойства** окно нового запроса узла, обновить **URL-адрес** свойство, чтобы они указывали на URL-адрес веб-приложения (например *[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).</span><span class="sxs-lookup"><span data-stu-id="d6567-629">In the **Properties** window of the new request node, update the **Url** property to point to the URL of your web app (e.g. *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span></span>

    <span data-ttu-id="d6567-630">![Изменение свойства URL-адрес](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "изменения свойства URL-адрес")</span><span class="sxs-lookup"><span data-stu-id="d6567-630">![Changing the Url property](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Changing the Url property")</span></span>

    <span data-ttu-id="d6567-631">*Изменение свойства URL-адрес*</span><span class="sxs-lookup"><span data-stu-id="d6567-631">*Changing the Url property*</span></span>
5. <span data-ttu-id="d6567-632">В **WebTest1.webtest** окно, щелкните правой кнопкой мыши **WebTest1** и нажмите кнопку **добавить цикл...** .</span><span class="sxs-lookup"><span data-stu-id="d6567-632">In the **WebTest1.webtest** window, right-click **WebTest1** and click **Add Loop...**.</span></span>

    <span data-ttu-id="d6567-633">![Добавление цикла WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Добавление цикла WebTest1")</span><span class="sxs-lookup"><span data-stu-id="d6567-633">![Adding a loop to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Adding a loop to WebTest1")</span></span>

    <span data-ttu-id="d6567-634">*Добавление цикла WebTest1*</span><span class="sxs-lookup"><span data-stu-id="d6567-634">*Adding a loop to WebTest1*</span></span>
6. <span data-ttu-id="d6567-635">В **Добавьте условное правило и элементы в цикл** выберите **цикл For** правила и измените следующие свойства.</span><span class="sxs-lookup"><span data-stu-id="d6567-635">In the **Add Conditional Rule and Items to Loop** dialog box, select the **For Loop** rule and modify the following properties.</span></span>

   1. <span data-ttu-id="d6567-636">**Дифференциальное значение:** 1000</span><span class="sxs-lookup"><span data-stu-id="d6567-636">**Terminating value:** 1000</span></span>
   2. <span data-ttu-id="d6567-637">**Имя параметра контекста:** итератора</span><span class="sxs-lookup"><span data-stu-id="d6567-637">**Context Parameter Name:** Iterator</span></span>
   3. <span data-ttu-id="d6567-638">**Значение приращения:** 1</span><span class="sxs-lookup"><span data-stu-id="d6567-638">**Increment Value:** 1</span></span>

      <span data-ttu-id="d6567-639">![Выберите правило для цикла и обновления свойств](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "выбирая правило для цикла и обновление свойств")</span><span class="sxs-lookup"><span data-stu-id="d6567-639">![Selecting the For Loop rule and updating the properties](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Selecting the For Loop rule and updating the properties")</span></span>

      <span data-ttu-id="d6567-640">*Выберите правило для цикла и обновление свойств*</span><span class="sxs-lookup"><span data-stu-id="d6567-640">*Selecting the For Loop rule and updating the properties*</span></span>
7. <span data-ttu-id="d6567-641">В разделе **элементы в цикле** выберите запрос, созданный ранее первого и последнего элемента для цикла.</span><span class="sxs-lookup"><span data-stu-id="d6567-641">Under the **Items in loop** section, select the request you created previously to be the first and last item for the loop.</span></span> <span data-ttu-id="d6567-642">Нажмите кнопку **ОК** , чтобы продолжить.</span><span class="sxs-lookup"><span data-stu-id="d6567-642">Click **OK** to continue.</span></span>

    <span data-ttu-id="d6567-643">![Выбрав первый и последний элемент цикла for](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "выбрав первый и последний элемент цикла")</span><span class="sxs-lookup"><span data-stu-id="d6567-643">![Selecting the first and last items for the loop](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Selecting the first and last items for the loop")</span></span>

    <span data-ttu-id="d6567-644">*Выбрав первый и последний элемент цикла*</span><span class="sxs-lookup"><span data-stu-id="d6567-644">*Selecting the first and last items for the loop*</span></span>
8. <span data-ttu-id="d6567-645">В **обозревателе решений**, щелкните правой кнопкой мыши **WebAndLoadTestProject** проекта, разверните **добавить** меню и выберите **нагрузочный тест...** .</span><span class="sxs-lookup"><span data-stu-id="d6567-645">In **Solution Explorer**, right-click the **WebAndLoadTestProject** project, expand the **Add** menu and select **Load Test...**.</span></span>

    <span data-ttu-id="d6567-646">![Добавление нагрузочного теста в проект WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Добавление в проект WebAndLoadTestProject нагрузочного теста")</span><span class="sxs-lookup"><span data-stu-id="d6567-646">![Adding a Load Test to the WebAndLoadTestProject project](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Adding a Load Test to the WebAndLoadTestProject project")</span></span>

    <span data-ttu-id="d6567-647">*Добавление в проект WebAndLoadTestProject нагрузочного теста*</span><span class="sxs-lookup"><span data-stu-id="d6567-647">*Adding a Load Test to the WebAndLoadTestProject project*</span></span>
9. <span data-ttu-id="d6567-648">В **мастера тестовой нагрузки** диалоговом окне щелкните **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d6567-648">In the **New Load Test Wizard** dialog box, click **Next**.</span></span>

    <span data-ttu-id="d6567-649">![Мастер тестовой нагрузки](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "мастера тестовой нагрузки")</span><span class="sxs-lookup"><span data-stu-id="d6567-649">![New Load Test Wizard](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "New Load Test Wizard")</span></span>

    <span data-ttu-id="d6567-650">*Мастер тестовой нагрузки*</span><span class="sxs-lookup"><span data-stu-id="d6567-650">*New Load Test Wizard*</span></span>
10. <span data-ttu-id="d6567-651">В **сценарий** выберите **не указывайте время на обдумывание** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d6567-651">In the **Scenario** page, select **Do not use think times** and click **Next**.</span></span>

    <span data-ttu-id="d6567-652">![Выбор не следует использовать время на обдумывание](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "выбор не следует использовать время на обдумывание")</span><span class="sxs-lookup"><span data-stu-id="d6567-652">![Selecting not to use think times](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selecting not to use think times")</span></span>

    <span data-ttu-id="d6567-653">*Выбор не следует использовать время на обдумывание*</span><span class="sxs-lookup"><span data-stu-id="d6567-653">*Selecting not to use think times*</span></span>
11. <span data-ttu-id="d6567-654">В **шаблон нагрузки** странице, убедитесь, что **постоянной нагрузки** выбран параметр.</span><span class="sxs-lookup"><span data-stu-id="d6567-654">In the **Load Pattern** page, make sure that the **Constant Load** option is selected.</span></span> <span data-ttu-id="d6567-655">Изменение **число пользователей** присвоить **250** пользователей и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d6567-655">Change the **User Count** setting to **250** users and click **Next**.</span></span>

    <span data-ttu-id="d6567-656">![Изменение число пользователей до 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "изменение число пользователей на 250")</span><span class="sxs-lookup"><span data-stu-id="d6567-656">![Changing the user count to 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Changing the user count to 250")</span></span>

    <span data-ttu-id="d6567-657">*Изменить число пользователей на 250*</span><span class="sxs-lookup"><span data-stu-id="d6567-657">*Changing the user count to 250*</span></span>
12. <span data-ttu-id="d6567-658">В **модель тестового набора** выберите **основе с последовательным упорядочиванием тестов** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d6567-658">In the **Test Mix Model** page, select **Based on sequential test order** and click **Next**.</span></span>

    <span data-ttu-id="d6567-659">![Выбор модели тестового набора](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Выбор модели тестового набора")</span><span class="sxs-lookup"><span data-stu-id="d6567-659">![Selecting the test mix model](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Selecting the test mix model")</span></span>

    <span data-ttu-id="d6567-660">*Выбор модели тестового набора*</span><span class="sxs-lookup"><span data-stu-id="d6567-660">*Selecting the test mix model*</span></span>
13. <span data-ttu-id="d6567-661">В **модель тестового набора** щелкните **добавить...**  Добавление теста в набор.</span><span class="sxs-lookup"><span data-stu-id="d6567-661">In the **Test Mix Model** page, click **Add...** to add a test to the mix.</span></span>

    <span data-ttu-id="d6567-662">![Добавление теста в тестовом наборе](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Добавление теста в тестовый набор")</span><span class="sxs-lookup"><span data-stu-id="d6567-662">![Adding a test to the test mix](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Adding a test to the test mix")</span></span>

    <span data-ttu-id="d6567-663">*Добавление теста в тестовый набор*</span><span class="sxs-lookup"><span data-stu-id="d6567-663">*Adding a test to the test mix*</span></span>
14. <span data-ttu-id="d6567-664">В **добавить тесты** диалоговое окно, дважды щелкните **WebTest1** Чтобы добавить тест в **выбранные тесты** списка.</span><span class="sxs-lookup"><span data-stu-id="d6567-664">In the **Add Tests** dialog box, double-click **WebTest1** to add the test to the **Selected tests** list.</span></span> <span data-ttu-id="d6567-665">Нажмите кнопку **ОК** , чтобы продолжить.</span><span class="sxs-lookup"><span data-stu-id="d6567-665">Click **OK** to continue.</span></span>

    <span data-ttu-id="d6567-666">![Добавление теста WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Добавление WebTest1 теста")</span><span class="sxs-lookup"><span data-stu-id="d6567-666">![Adding the WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Adding the WebTest1 test")</span></span>

    <span data-ttu-id="d6567-667">*Добавление теста WebTest1*</span><span class="sxs-lookup"><span data-stu-id="d6567-667">*Adding the WebTest1 test*</span></span>
15. <span data-ttu-id="d6567-668">Вернитесь в **тестового набора** щелкните **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d6567-668">Back in the **Test Mix** page, click **Next**.</span></span>

    <span data-ttu-id="d6567-669">![Завершение работы страница тестового набора](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "завершения работы тестового набора")</span><span class="sxs-lookup"><span data-stu-id="d6567-669">![Completing the Test Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Completing the Test Mix page")</span></span>

    <span data-ttu-id="d6567-670">*Завершение работы страница тестового набора*</span><span class="sxs-lookup"><span data-stu-id="d6567-670">*Completing the Test Mix page*</span></span>
16. <span data-ttu-id="d6567-671">В **смешанного сетевого профиля** щелкните **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d6567-671">In the **Network Mix** page, click **Next**.</span></span>

    <span data-ttu-id="d6567-672">![Нажав кнопку Далее на странице смешанного сетевого профиля](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "нажав кнопку Далее на странице смешанный сетевой профиль")</span><span class="sxs-lookup"><span data-stu-id="d6567-672">![Clicking next in the Network Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Clicking next in the Network Mix page")</span></span>

    <span data-ttu-id="d6567-673">*Нажав кнопку Далее на странице смешанный сетевой профиль*</span><span class="sxs-lookup"><span data-stu-id="d6567-673">*Clicking next in the Network Mix page*</span></span>
17. <span data-ttu-id="d6567-674">В **браузеров** выберите **Internet Explorer 10.0** тип браузера и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d6567-674">In the **Browser Mix** page, select **Internet Explorer 10.0** as the browser type and click **Next**.</span></span>

    <span data-ttu-id="d6567-675">![Выбрав тип браузера](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "выбрав тип браузера")</span><span class="sxs-lookup"><span data-stu-id="d6567-675">![Selecting the browser type](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Selecting the browser type")</span></span>

    <span data-ttu-id="d6567-676">*Выбрав тип браузера*</span><span class="sxs-lookup"><span data-stu-id="d6567-676">*Selecting the browser type*</span></span>
18. <span data-ttu-id="d6567-677">В **наборы счетчиков** щелкните **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d6567-677">In the **Counter Sets** page, click **Next**.</span></span>

    <span data-ttu-id="d6567-678">![Нажав кнопку "Далее" на странице наборы счетчиков](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "нажав кнопку \"Далее\", на странице наборы счетчиков")</span><span class="sxs-lookup"><span data-stu-id="d6567-678">![Clicking Next in the Counter Sets page](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Clicking Next in the Counter Sets page")</span></span>

    <span data-ttu-id="d6567-679">*Нажав кнопку "Далее" на странице наборы счетчиков*</span><span class="sxs-lookup"><span data-stu-id="d6567-679">*Clicking Next in the Counter Sets page*</span></span>
19. <span data-ttu-id="d6567-680">В **параметры запуска** странице **длительность нагрузочного теста** для **5 минут** и нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="d6567-680">In the **Run Settings** page, set the **Load test duration** to **5 minutes** and click **Finish**.</span></span>

    <span data-ttu-id="d6567-681">![Параметр длительность нагрузочного теста до 5 минут](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "параметр длительность нагрузочного теста до 5 минут")</span><span class="sxs-lookup"><span data-stu-id="d6567-681">![Setting the load test duration to 5 minutes](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Setting the load test duration to 5 minutes")</span></span>

    <span data-ttu-id="d6567-682">*Параметр длительность нагрузочного теста до 5 минут*</span><span class="sxs-lookup"><span data-stu-id="d6567-682">*Setting the load test duration to 5 minutes*</span></span>
20. <span data-ttu-id="d6567-683">В **обозревателе решений**, дважды щелкните **Local.settings** файл для просмотра параметров тестирования.</span><span class="sxs-lookup"><span data-stu-id="d6567-683">In **Solution Explorer**, double-click the **Local.settings** file to explore the test settings.</span></span> <span data-ttu-id="d6567-684">По умолчанию Visual Studio использует локального компьютера для выполнения тестов.</span><span class="sxs-lookup"><span data-stu-id="d6567-684">By default, Visual Studio uses your local computer to run the tests.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-685">Кроме того, можно настроить проект тестов для выполнения нагрузочных тестов в облаке с помощью **Visual Studio Online (VSO)**.</span><span class="sxs-lookup"><span data-stu-id="d6567-685">Alternatively, you can configure your test project to run the load tests in the cloud using **Visual Studio Online (VSO)**.</span></span> <span data-ttu-id="d6567-686">VSO предоставляет Облачное нагрузочное тестирование служба, которая имитирует более реалистичный нагрузки, как избежать ограничений локальной среде, например мощности ЦП, объем доступной памяти и пропускной способности сети.</span><span class="sxs-lookup"><span data-stu-id="d6567-686">VSO provides a cloud-based load testing service that simulates a more realistic load, avoiding local environment constraints like CPU capacity, available memory and network bandwidth.</span></span> <span data-ttu-id="d6567-687">Дополнительные сведения об использовании VSO для выполнения нагрузочных тестов, см. в разделе [в этой статье](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span><span class="sxs-lookup"><span data-stu-id="d6567-687">For more information about using VSO to run load tests, see [this article](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span></span>

    ![Параметры тестирования](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a><span data-ttu-id="d6567-689">Задача 3 – Проверка автомасштабирования</span><span class="sxs-lookup"><span data-stu-id="d6567-689">Task 3 – Autoscale Verification</span></span>

<span data-ttu-id="d6567-690">Вы выполните нагрузочный тест, созданный в предыдущей задаче и см. в разделе, как веб-приложение ведет себя под нагрузкой.</span><span class="sxs-lookup"><span data-stu-id="d6567-690">You will now execute the load test you created in the previous task and see how your web app behaves under load.</span></span>

1. <span data-ttu-id="d6567-691">В **обозревателе решений**, дважды щелкните **LoadTest1.loadtest** открыть нагрузочного теста.</span><span class="sxs-lookup"><span data-stu-id="d6567-691">In **Solution Explorer**, double-click **LoadTest1.loadtest** to open the load test.</span></span>

    <span data-ttu-id="d6567-692">![Открыв файл LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Открытие LoadTest1.loadtest")</span><span class="sxs-lookup"><span data-stu-id="d6567-692">![Opening LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Opening LoadTest1.loadtest")</span></span>

    <span data-ttu-id="d6567-693">*Открытие LoadTest1.loadtest*</span><span class="sxs-lookup"><span data-stu-id="d6567-693">*Opening LoadTest1.loadtest*</span></span>
2. <span data-ttu-id="d6567-694">В **LoadTest1.loadtest** окно, нажмите первую кнопку на панели элементов для выполнения нагрузочного теста.</span><span class="sxs-lookup"><span data-stu-id="d6567-694">In the **LoadTest1.loadtest** window, click the first button in the toolbox to run the load test.</span></span>

    <span data-ttu-id="d6567-695">![Запуск нагрузочного теста](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "нагрузочный тест")</span><span class="sxs-lookup"><span data-stu-id="d6567-695">![Running the load test](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Running the load test")</span></span>

    <span data-ttu-id="d6567-696">*Запуск нагрузочного теста*</span><span class="sxs-lookup"><span data-stu-id="d6567-696">*Running the load test*</span></span>
3. <span data-ttu-id="d6567-697">Дождитесь завершения нагрузочного теста.</span><span class="sxs-lookup"><span data-stu-id="d6567-697">Wait until the load test completes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-698">Нагрузочный тест моделирует несколько пользователей, которые одновременно отправлять запросы в веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="d6567-698">The load test simulates multiple users that send requests to the web app simultaneously.</span></span> <span data-ttu-id="d6567-699">Если тест выполняется, вы можете отслеживать доступные счетчики для обнаружения ошибок, предупреждений или другие сведения о выполнении нагрузочного теста.</span><span class="sxs-lookup"><span data-stu-id="d6567-699">When the test is running, you can monitor the available counters to detect any errors, warnings or other information related to your load test run.</span></span>

    <span data-ttu-id="d6567-700">![Загрузить тест будет выполняться](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "дождаться завершения нагрузочного теста")</span><span class="sxs-lookup"><span data-stu-id="d6567-700">![Load test running](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Waiting until the load test completes")</span></span>

    <span data-ttu-id="d6567-701">*Если нагрузочный тест выполняется*</span><span class="sxs-lookup"><span data-stu-id="d6567-701">*Load test running*</span></span>
4. <span data-ttu-id="d6567-702">После завершения теста, вернитесь на портал управления и перейдите к **масштабирования** страницы веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d6567-702">Once the test completes, go back to the management portal and navigate to the **Scale** page of your web app.</span></span> <span data-ttu-id="d6567-703">В разделе **емкость** разделе, вы должны увидеть в графе, новый экземпляр был развернут автоматически.</span><span class="sxs-lookup"><span data-stu-id="d6567-703">Under the **capacity** section, you should see in the graph that a new instance was automatically deployed.</span></span>

    ![Автоматически развертывать новый экземпляр](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    <span data-ttu-id="d6567-705">*Автоматически развертывать новый экземпляр*</span><span class="sxs-lookup"><span data-stu-id="d6567-705">*New instance automatically deployed*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6567-706">Может занять несколько минут, чтобы изменения отобразились в графе (нажмите клавишу **CTRL + F5** периодически, чтобы обновить страницу).</span><span class="sxs-lookup"><span data-stu-id="d6567-706">It may take several minutes for the changes to appear in the graph (press **CTRL + F5** periodically to refresh the page).</span></span> <span data-ttu-id="d6567-707">Если вы не видите все изменения, можно попробовать следующее:</span><span class="sxs-lookup"><span data-stu-id="d6567-707">If you do not see any changes, you can try the following:</span></span>
    > 
    > - <span data-ttu-id="d6567-708">Увеличьте продолжительность нагрузочного теста (например, чтобы **10 минут**)</span><span class="sxs-lookup"><span data-stu-id="d6567-708">Increase the duration of the load test (e.g. to **10 minutes**)</span></span>
    > - <span data-ttu-id="d6567-709">Уменьшить максимальное и минимальное значения **целевой ЦП** диапазон в конфигурации автоматического масштабирования для веб-приложения</span><span class="sxs-lookup"><span data-stu-id="d6567-709">Reduce the maximum and minimum values of the **Target CPU** range in the Autoscale configuration of your web app</span></span>
    > - <span data-ttu-id="d6567-710">Запуск нагрузочного теста в облаке с **Visual Studio Online**.</span><span class="sxs-lookup"><span data-stu-id="d6567-710">Run the load test in the cloud with **Visual Studio Online**.</span></span> <span data-ttu-id="d6567-711">Дополнительные сведения [здесь](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span><span class="sxs-lookup"><span data-stu-id="d6567-711">More information [here](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="d6567-712">Сводка</span><span class="sxs-lookup"><span data-stu-id="d6567-712">Summary</span></span>

<span data-ttu-id="d6567-713">В этой практической работу вы узнали, как настроить и развернуть приложение в рабочей среде веб-приложения в Azure.</span><span class="sxs-lookup"><span data-stu-id="d6567-713">In this hands-on lab, you learned how to set up and deploy your application to production web apps in Azure.</span></span> <span data-ttu-id="d6567-714">Вы начали путем обнаружения и обновления баз данных, используя **Entity Framework Code First Migrations**, затем продолжает выполнение развертывание новых версий своего сайта с помощью **Git** и выполнения отката для Последняя стабильная версия веб-узла.</span><span class="sxs-lookup"><span data-stu-id="d6567-714">You started by detecting and updating your databases using **Entity Framework Code First Migrations**, then continued by deploying new versions of your site using **Git** and performing rollbacks to the latest stable version of your site.</span></span> <span data-ttu-id="d6567-715">Кроме того вы узнали, как масштабировать приложение с помощью хранилища для перемещения статическое содержимое в контейнер больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="d6567-715">Additionally, you learned how to scale your app using Storage to move your static content to a Blob container.</span></span>
