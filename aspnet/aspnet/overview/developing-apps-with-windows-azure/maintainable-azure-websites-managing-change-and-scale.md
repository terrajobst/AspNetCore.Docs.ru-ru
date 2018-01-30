---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: "Практического занятия: поддерживаемого веб-сайтов Azure: управление изменениями и масштаб | Документы Microsoft"
author: rick-anderson
description: "В этой лабораторной работы Узнайте, как Microsoft Azure позволяет легко создавать и развертывать веб-сайтов в рабочей среде."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 4bce02b2c592ff04e0dbce78d18004c69268e4fd
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a><span data-ttu-id="bda2a-103">Практического занятия: поддерживаемого веб-сайтов Azure: управление изменениями и масштабирования</span><span class="sxs-lookup"><span data-stu-id="bda2a-103">Hands on Lab: Maintainable Azure Websites: Managing Change and Scale</span></span>
====================
<span data-ttu-id="bda2a-104">по [Web лагеря команды](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="bda2a-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="bda2a-105">Загрузите комплект учебных материалов лагеря Web</span><span class="sxs-lookup"><span data-stu-id="bda2a-105">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="bda2a-106">Microsoft Azure позволяет легко создавать и развертывать веб-сайтов в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="bda2a-106">Microsoft Azure makes it easy to build and deploy websites to production.</span></span> <span data-ttu-id="bda2a-107">Но не закончите при динамической приложения, вы новичок!</span><span class="sxs-lookup"><span data-stu-id="bda2a-107">But you're not done when your application is live, you're just getting started!</span></span> <span data-ttu-id="bda2a-108">Необходимо для обработки изменяющихся требования, обновления базы данных, масштаб и многое другое.</span><span class="sxs-lookup"><span data-stu-id="bda2a-108">You'll need to handle changing requirements, database updates, scale, and more.</span></span> <span data-ttu-id="bda2a-109">К счастью службы приложения Azure имеется связанная со множеством возможностей помогает защищать бесперебойной сайты.</span><span class="sxs-lookup"><span data-stu-id="bda2a-109">Fortunately, Azure App Service has you covered, with plenty of features to help you keep your sites running smoothly.</span></span>
> 
> <span data-ttu-id="bda2a-110">Azure предлагает безопасные и гибкие возможности разработки, развертывания и масштабирования для веб-приложения любого размера.</span><span class="sxs-lookup"><span data-stu-id="bda2a-110">Azure offers secure and flexible development, deployment and scaling options for any size web application.</span></span> <span data-ttu-id="bda2a-111">Используйте существующие средства для создания и развертывания приложений, не загружая управляющую инфраструктуру.</span><span class="sxs-lookup"><span data-stu-id="bda2a-111">Leverage your existing tools to create and deploy applications without the hassle of managing infrastructure.</span></span>
> 
> <span data-ttu-id="bda2a-112">Предоставьте производственного веб-приложение самостоятельно в минутах простого развертывания содержимое, созданное с помощью вашей другого средства разработки.</span><span class="sxs-lookup"><span data-stu-id="bda2a-112">Provision a production web application yourself in minutes by easily deploying content created using your favorite development tool.</span></span> <span data-ttu-id="bda2a-113">Можно развернуть существующий сайт напрямую из системы управления версиями с поддержкой **Git**, **GitHub**, **Bitbucket**, **TFS**и даже  **Общий банк данных**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-113">You can deploy an existing site directly from source control with support for **Git**, **GitHub**, **Bitbucket**, **TFS**, and even **DropBox**.</span></span> <span data-ttu-id="bda2a-114">Развертывание непосредственно из вашего избранного интегрированной среды разработки или скриптов с использованием **PowerShell** в Windows или **CLI** средств, работающих в любой операционной системе.</span><span class="sxs-lookup"><span data-stu-id="bda2a-114">Deploy directly from your favorite IDE or from scripts using **PowerShell** in Windows or **CLI** tools running on any OS.</span></span> <span data-ttu-id="bda2a-115">После развертывания актуальность сайты постоянно поддержки для непрерывного развертывания.</span><span class="sxs-lookup"><span data-stu-id="bda2a-115">Once deployed, keep your sites constantly up-to-date with support for continuous deployment.</span></span>
> 
> <span data-ttu-id="bda2a-116">Azure предоставляет масштабируемые и устойчивые облачного хранилища, резервного копирования и восстановления решений для любых данных, большие или малой.</span><span class="sxs-lookup"><span data-stu-id="bda2a-116">Azure provides scalable, durable cloud storage, backup, and recovery solutions for any data, big or small.</span></span> <span data-ttu-id="bda2a-117">При развертывании приложений в производственной среде, службы хранилища, например таблиц, больших двоичных объектов и баз данных SQL, позволяют масштабировать приложения в облаке.</span><span class="sxs-lookup"><span data-stu-id="bda2a-117">When deploying applications to a production environment, storage services such as Tables, Blobs and SQL Databases, help you scale your application in the cloud.</span></span>
> 
> <span data-ttu-id="bda2a-118">С базами данных SQL очень важно обновлять производительность базы данных, при развертывании новых версий приложения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-118">With SQL Databases, it is important to keep your productive database up-to-date when deploying new versions of your application.</span></span> <span data-ttu-id="bda2a-119">Благодаря **Entity Framework Code First Migrations**, разработка и развертывание модели данных, был упрощен для обновления среды в минутах.</span><span class="sxs-lookup"><span data-stu-id="bda2a-119">Thanks to **Entity Framework Code First Migrations**, the development and deployment of your data model has been simplified to update your environments in minutes.</span></span> <span data-ttu-id="bda2a-120">Данная практическая работа Показать различных разделов, которые могут возникнуть при развертывании веб-приложения в рабочей среде в Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="bda2a-120">This hands-on lab will show you the different topics you could encounter when deploying your web app to production environments in Microsoft Azure.</span></span>
> 
> <span data-ttu-id="bda2a-121">Все образцы кода и фрагменты кода включаются в Web лагеря комплект учебных материалов, доступных в [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="bda2a-121">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>
> 
> <span data-ttu-id="bda2a-122">Более подробные сведения этого раздела см. в разделе [Создание реальных облачных приложений с Azure электронная книга](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bda2a-122">For more in-depth coverage of this topic, see the [Building Real-World Cloud Apps with Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="bda2a-123">Обзор</span><span class="sxs-lookup"><span data-stu-id="bda2a-123">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="bda2a-124">Цели</span><span class="sxs-lookup"><span data-stu-id="bda2a-124">Objectives</span></span>

<span data-ttu-id="bda2a-125">В этой практической вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="bda2a-125">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="bda2a-126">Включить Entity Framework миграции с существующей модели</span><span class="sxs-lookup"><span data-stu-id="bda2a-126">Enable Entity Framework Migrations with an existing model</span></span>
- <span data-ttu-id="bda2a-127">Обновить модель объектов и базы данных с использованием Entity Framework миграции</span><span class="sxs-lookup"><span data-stu-id="bda2a-127">Update the object model and database accordingly using Entity Framework Migrations</span></span>
- <span data-ttu-id="bda2a-128">Развертывание на службе приложений Azure с помощью Git</span><span class="sxs-lookup"><span data-stu-id="bda2a-128">Deploy to Azure App Service using Git</span></span>
- <span data-ttu-id="bda2a-129">Откат до предыдущего развертывания с помощью портала управления Azure</span><span class="sxs-lookup"><span data-stu-id="bda2a-129">Rollback to a previous deployment using the Azure Management portal</span></span>
- <span data-ttu-id="bda2a-130">Используйте хранилище Azure, чтобы масштабировать веб-приложение</span><span class="sxs-lookup"><span data-stu-id="bda2a-130">Use Azure Storage to scale a web app</span></span>
- <span data-ttu-id="bda2a-131">Настройка автоматического масштабирования для веб-приложения с помощью портала управления Azure</span><span class="sxs-lookup"><span data-stu-id="bda2a-131">Configure auto-scaling for a web app using the Azure Management Portal</span></span>
- <span data-ttu-id="bda2a-132">Создать и настроить проект нагрузочного теста в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bda2a-132">Create and configure a load test project in Visual Studio</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="bda2a-133">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="bda2a-133">Prerequisites</span></span>

<span data-ttu-id="bda2a-134">Для завершения этой практической требуется следующее:</span><span class="sxs-lookup"><span data-stu-id="bda2a-134">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="bda2a-135">[Visual Studio Express 2013 для Web](https://www.microsoft.com/visualstudio/) или выше</span><span class="sxs-lookup"><span data-stu-id="bda2a-135">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="bda2a-136">Azure SDK для .NET 2.2</span><span class="sxs-lookup"><span data-stu-id="bda2a-136">Azure SDK for .NET 2.2</span></span>](https://www.microsoft.com/windowsazure/sdk/)
- [<span data-ttu-id="bda2a-137">Система управления версиями GIT</span><span class="sxs-lookup"><span data-stu-id="bda2a-137">GIT Version Control System</span></span>](http://git-scm.com/download)
- <span data-ttu-id="bda2a-138">Подписки Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="bda2a-138">A Microsoft Azure subscription</span></span> 

    - <span data-ttu-id="bda2a-139">Зарегистрироваться в службе [бесплатной пробной версии](http://aka.ms/watk-freetrial)</span><span class="sxs-lookup"><span data-stu-id="bda2a-139">Sign up for a [Free Trial](http://aka.ms/watk-freetrial)</span></span>
    - <span data-ttu-id="bda2a-140">При наличии Visual Studio Professional, Test Professional, Premium или Ultimate с подписчиком MSDN или платформы MSDN активации вашей [преимущество MSDN](http://aka.ms/watk-msdn) сейчас, чтобы начать разработку и тестирование на платформе Azure</span><span class="sxs-lookup"><span data-stu-id="bda2a-140">If you are a Visual Studio Professional, Test Professional, Premium or Ultimate with MSDN or MSDN Platforms subscriber, activate your [MSDN benefit](http://aka.ms/watk-msdn) now to start developing and testing on Azure</span></span>
    - <span data-ttu-id="bda2a-141">[BizSpark](http://aka.ms/watk-bizspark) элементы автоматически получать Azure преимущество по их Visual Studio Ultimate с подпиской MSDN</span><span class="sxs-lookup"><span data-stu-id="bda2a-141">[BizSpark](http://aka.ms/watk-bizspark) members automatically receive the Azure benefit through their Visual Studio Ultimate with MSDN subscriptions</span></span>
    - <span data-ttu-id="bda2a-142">Члены [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials программа получать ежемесячные кредиты Azure бесплатно</span><span class="sxs-lookup"><span data-stu-id="bda2a-142">Members of the [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials program receive monthly Azure credits at no charge</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="bda2a-143">Установка</span><span class="sxs-lookup"><span data-stu-id="bda2a-143">Setup</span></span>

<span data-ttu-id="bda2a-144">Чтобы выполнить упражнения в этой практической, необходимо настроить среду.</span><span class="sxs-lookup"><span data-stu-id="bda2a-144">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="bda2a-145">Откройте проводник и найдите в лаборатории **источника** папки.</span><span class="sxs-lookup"><span data-stu-id="bda2a-145">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="bda2a-146">Щелкните правой кнопкой мыши **Setup.cmd** и выберите **Запуск от имени администратора** для запуска процесса установки, будет настроить среду и установить фрагменты кода Visual Studio для этой лабораторной работы.</span><span class="sxs-lookup"><span data-stu-id="bda2a-146">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="bda2a-147">Если отображается диалоговое окно контроля учетных записей, подтвердите действие для продолжения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-147">If the User Account Control dialog is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="bda2a-148">Убедитесь, что все зависимости для этой лабораторной работы вы вернули перед запуском программы установки.</span><span class="sxs-lookup"><span data-stu-id="bda2a-148">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="bda2a-149">Фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="bda2a-149">Using the Code Snippets</span></span>

<span data-ttu-id="bda2a-150">В документе лаборатории будет предложено вставлять блоки кода.</span><span class="sxs-lookup"><span data-stu-id="bda2a-150">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="bda2a-151">Visual Studio к фрагментам кода, к которому можно получить из в Visual Studio 2013, чтобы избежать необходимости вручную добавить большая часть кода предоставляется для удобства.</span><span class="sxs-lookup"><span data-stu-id="bda2a-151">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="bda2a-152">Каждого упражнения сопровождается начальный решений, расположенный в **начать** папку расчетов, позволяющий выполнить каждого упражнения независимо от других.</span><span class="sxs-lookup"><span data-stu-id="bda2a-152">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="bda2a-153">Имейте в виду, что фрагменты кода, которые добавляются во время упражнения, отсутствуют на их запуск решения и могут не работать до завершения этого упражнения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-153">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="bda2a-154">В исходном коде для упражнения, вы также найдете **окончания** папку, содержащую решение Visual Studio с кодом, полученный в результате выполнения действий в соответствующий упражнении.</span><span class="sxs-lookup"><span data-stu-id="bda2a-154">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="bda2a-155">Эти решения можно использовать как рекомендации, если вам нужна дополнительная помощь, отвечая на этой практической.</span><span class="sxs-lookup"><span data-stu-id="bda2a-155">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="bda2a-156">Упражнения</span><span class="sxs-lookup"><span data-stu-id="bda2a-156">Exercises</span></span>

<span data-ttu-id="bda2a-157">Данная практическая работа включает следующие упражнения:</span><span class="sxs-lookup"><span data-stu-id="bda2a-157">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="bda2a-158">С помощью Entity Framework миграции</span><span class="sxs-lookup"><span data-stu-id="bda2a-158">Using Entity Framework Migrations</span></span>](#Exercise1)
2. [<span data-ttu-id="bda2a-159">Развертывание веб-приложения для промежуточного хранения</span><span class="sxs-lookup"><span data-stu-id="bda2a-159">Deploying a Web App to Staging</span></span>](#Exercise2)
3. [<span data-ttu-id="bda2a-160">Выполнение отката развертывания в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="bda2a-160">Performing Deployment Rollback in Production</span></span>](#Exercise3)
4. [<span data-ttu-id="bda2a-161">С помощью хранилища Azure</span><span class="sxs-lookup"><span data-stu-id="bda2a-161">Scaling Using Azure Storage</span></span>](#Exercise4)
5. <span data-ttu-id="bda2a-162">[С помощью автоматического масштабирования для веб-приложений](#Exercise5) (необязательно для Visual Studio 2013 Ultimate edition)</span><span class="sxs-lookup"><span data-stu-id="bda2a-162">[Using Autoscale for Web Apps](#Exercise5) (Optional for Visual Studio 2013 Ultimate edition)</span></span>

<span data-ttu-id="bda2a-163">Предполагаемое время для выполнения этого занятия: **75 минут**</span><span class="sxs-lookup"><span data-stu-id="bda2a-163">Estimated time to complete this lab: **75 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="bda2a-164">При первом запуске Visual Studio, необходимо выбрать одну из коллекций предварительно определенных параметров.</span><span class="sxs-lookup"><span data-stu-id="bda2a-164">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="bda2a-165">Каждая предопределенная коллекция соответствует конкретному стилю разработки и определяет макетов окон, поведение редактора, фрагменты кода IntelliSense и параметры диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="bda2a-165">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="bda2a-166">В этой лаборатории описаны действия, необходимые для выполнения данной задачи в Visual Studio при использовании **обычные параметры разработки** коллекции.</span><span class="sxs-lookup"><span data-stu-id="bda2a-166">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="bda2a-167">При выборе другого набора параметров для среды разработки может быть отличия в этапах, которые следует принимать во внимание.</span><span class="sxs-lookup"><span data-stu-id="bda2a-167">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a><span data-ttu-id="bda2a-168">Упражнение 1: Использовать Entity Framework миграции</span><span class="sxs-lookup"><span data-stu-id="bda2a-168">Exercise 1: Using Entity Framework Migrations</span></span>

<span data-ttu-id="bda2a-169">При разработке приложения, модели данных могут измениться с течением времени.</span><span class="sxs-lookup"><span data-stu-id="bda2a-169">When you are developing an application, your data model might change over time.</span></span> <span data-ttu-id="bda2a-170">Эти изменения могут повлиять на существующей модели в базе данных (при создании новой версии) и очень важно для поддержания актуального состояния, чтобы избежать ошибок базы данных.</span><span class="sxs-lookup"><span data-stu-id="bda2a-170">These changes could affect the existing model in your database (if you are creating a new version) and it is important to keep your database up-to-date to prevent errors.</span></span>

<span data-ttu-id="bda2a-171">Для упрощения отслеживания этих изменений в модели, **Entity Framework Code First Migrations** автоматически обнаруживает изменения, сравнение модели с помощью схемы базы данных и приводит к возникновению ошибки конкретного кода для обновления базы данных, Создание нового *версии* базы данных.</span><span class="sxs-lookup"><span data-stu-id="bda2a-171">To simplify the tracking of these changes in your model, **Entity Framework Code First Migrations** automatically detect changes comparing your model with the database schema and generates specific code to update your database, creating new *versions* of your database.</span></span>

<span data-ttu-id="bda2a-172">В этом упражнении показано, как включить **миграций** для приложения и как можно легко определить и создать изменения для обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="bda2a-172">This exercise shows you how to enable **Migrations** for your application and how you can easily detect and generate changes to update your databases.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a><span data-ttu-id="bda2a-173">Задача 1 – включение миграции</span><span class="sxs-lookup"><span data-stu-id="bda2a-173">Task 1 – Enabling Migrations</span></span>

<span data-ttu-id="bda2a-174">В этой задаче будет выполняться действия по включению **Entity Framework Code First Migrations** для **компьютерный фанат тест** базы данных, изменение модели и общие сведения о том, как эти изменения отражаются в База данных.</span><span class="sxs-lookup"><span data-stu-id="bda2a-174">In this task, you will go through the steps of enabling **Entity Framework Code First Migrations** to the **Geek Quiz** database, changing the model and understanding how those changes are reflected in the database.</span></span>

1. <span data-ttu-id="bda2a-175">Откройте Visual Studio и откройте **GeekQuiz.sln** файл решения из **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-175">Open Visual Studio and open the **GeekQuiz.sln** solution file from **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.</span></span>
2. <span data-ttu-id="bda2a-176">Постройте решение, чтобы загрузить и установить **NuGet** зависимости пакетов.</span><span class="sxs-lookup"><span data-stu-id="bda2a-176">Build the solution in order to download and install the **NuGet** package dependencies.</span></span> <span data-ttu-id="bda2a-177">Для этого щелкните правой кнопкой мыши решение и выберите **построить решение** или нажмите клавишу **Ctrl + Shift + B**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-177">To do this, right-click the solution and click **Build Solution** or press **Ctrl + Shift + B**.</span></span>
3. <span data-ttu-id="bda2a-178">Из **средства** в Visual Studio, выберите пункт меню **диспетчер пакетов библиотеки**, а затем нажмите кнопку **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-178">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
4. <span data-ttu-id="bda2a-179">В **консоль диспетчера пакетов**, введите следующую команду и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-179">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="bda2a-180">Будет создана первоначальной миграции, на основе существующей модели.</span><span class="sxs-lookup"><span data-stu-id="bda2a-180">An initial migration based on the existing model will be created.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    <span data-ttu-id="bda2a-181">![Включение миграции](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "включение миграции")</span><span class="sxs-lookup"><span data-stu-id="bda2a-181">![Enabling Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Enabling Migrations")</span></span>

    <span data-ttu-id="bda2a-182">*Включение миграции*</span><span class="sxs-lookup"><span data-stu-id="bda2a-182">*Enabling Migrations*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-183">Эта команда добавляет **миграций** папка головоломка компьютерный фанат проект, содержащий файл называется **Configuration.cs**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-183">This command adds a **Migrations** folder to Geek Quiz project that contains a file called **Configuration.cs**.</span></span> <span data-ttu-id="bda2a-184">**Конфигурации** позволяет определить поведение миграции для текущего контекста.</span><span class="sxs-lookup"><span data-stu-id="bda2a-184">The **Configuration** class allows you to configure how Migrations behaves for your context.</span></span>
5. <span data-ttu-id="bda2a-185">С помощью миграций и включена, необходимо обновить **конфигурации** класс, чтобы заполнить базу данных с начальными данными, **компьютерный фанат тест** требует.</span><span class="sxs-lookup"><span data-stu-id="bda2a-185">With Migrations enabled, you need to update the **Configuration** class to populate the database with the initial data that **Geek Quiz** requires.</span></span> <span data-ttu-id="bda2a-186">В разделе **миграций**, замените **Configuration.cs** файл с тем, который расположен в **Source\Assets** папку части этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="bda2a-186">Under **Migrations**, replace the **Configuration.cs** file with the one located in the **Source\Assets** folder of this lab.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-187">Поскольку **миграций** вызовет **начальное значение** метод при каждом обновлении базы данных, необходимо убедиться, что записи не повторяются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="bda2a-187">Since **Migrations** will call the **Seed** method with every database update, you need to be sure that records are not duplicated in the database.</span></span> <span data-ttu-id="bda2a-188">**AddOrUpdate** метод поможет предотвратить дублирование данных.</span><span class="sxs-lookup"><span data-stu-id="bda2a-188">The **AddOrUpdate** method will help to prevent duplicate data.</span></span>
6. <span data-ttu-id="bda2a-189">Чтобы добавить первоначальной миграции, введите следующую команду и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-189">To add an initial migration, enter the following command and then press **Enter**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-190">Убедитесь, что нет базы данных с именем &quot;GeekQuizProd&quot; в экземпляре LocalDB.</span><span class="sxs-lookup"><span data-stu-id="bda2a-190">Make sure that there is no database named &quot;GeekQuizProd&quot; in your LocalDB instance.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    <span data-ttu-id="bda2a-191">![Добавление основной схемы миграции](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Добавление базовой схемы миграции")</span><span class="sxs-lookup"><span data-stu-id="bda2a-191">![Adding base schema migration](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Adding base schema migration")</span></span>

    <span data-ttu-id="bda2a-192">*Добавление основной схемы миграции*</span><span class="sxs-lookup"><span data-stu-id="bda2a-192">*Adding base schema migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-193">**Добавить миграции** будет формировать следующую миграцию на основе изменений, внесенные в модель с момента создания последней миграции.</span><span class="sxs-lookup"><span data-stu-id="bda2a-193">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created.</span></span> <span data-ttu-id="bda2a-194">В этом случае это первой миграции проекта будет добавлен скрипты для создания всех таблиц, определенных в **TriviaContext** класса.</span><span class="sxs-lookup"><span data-stu-id="bda2a-194">In this case, as it is the first migration of the project, it will add the scripts to create all the tables defined in the **TriviaContext** class.</span></span>
7. <span data-ttu-id="bda2a-195">Выполнение миграции для обновления базы данных, выполнив следующую команду.</span><span class="sxs-lookup"><span data-stu-id="bda2a-195">Execute the migration to update the database by running the following command.</span></span> <span data-ttu-id="bda2a-196">Для этой команды необходимо указать **Verbose** флаг, чтобы просмотреть инструкции SQL, применяемые к целевой базе данных.</span><span class="sxs-lookup"><span data-stu-id="bda2a-196">For this command you will specify the **Verbose** flag to view the SQL statements being applied to the target database.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    <span data-ttu-id="bda2a-197">![Создание начальной базы данных](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Создание начальной базы данных")</span><span class="sxs-lookup"><span data-stu-id="bda2a-197">![Creating initial database](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Creating initial database")</span></span>

    <span data-ttu-id="bda2a-198">*Создание начальной базы данных*</span><span class="sxs-lookup"><span data-stu-id="bda2a-198">*Creating initial database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-199">**Update-Database** будет применять любые незавершенные миграции к базе данных.</span><span class="sxs-lookup"><span data-stu-id="bda2a-199">**Update-Database** will apply any pending migrations to the database.</span></span> <span data-ttu-id="bda2a-200">В этом случае она будет создана база данных, с помощью строки подключения, определенные в файле web.config.</span><span class="sxs-lookup"><span data-stu-id="bda2a-200">In this case, it will create the database using the connection string defined in your web.config file.</span></span>
8. <span data-ttu-id="bda2a-201">Последовательно выберите пункты **представление** меню и открыть **обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-201">Go to **View** menu and open **SQL Server Object Explorer**.</span></span>

    <span data-ttu-id="bda2a-202">![Открыть в обозревателе объектов SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "открыть в обозревателе объектов SQL Server")</span><span class="sxs-lookup"><span data-stu-id="bda2a-202">![Open in SQL Server Object Explorer](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Open in SQL Server Object Explorer")</span></span>

    <span data-ttu-id="bda2a-203">*Открыть в обозревателе объектов SQL Server*</span><span class="sxs-lookup"><span data-stu-id="bda2a-203">*Open in SQL Server Object Explorer*</span></span>
9. <span data-ttu-id="bda2a-204">В **обозреватель объектов SQL Server** окна, подключиться к своему экземпляру LocalDB, щелкнув правой кнопкой мыши **SQL Server** узла и выбрав **добавить SQL Server...**  параметр.</span><span class="sxs-lookup"><span data-stu-id="bda2a-204">In the **SQL Server Object Explorer** window, connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="bda2a-205">![Добавление экземпляра SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Добавление экземпляра SQL Server")</span><span class="sxs-lookup"><span data-stu-id="bda2a-205">![Adding a SQL Server Instance](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="bda2a-206">*Добавление экземпляра SQL Server в обозревателе объектов SQL Server*</span><span class="sxs-lookup"><span data-stu-id="bda2a-206">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
10. <span data-ttu-id="bda2a-207">Задать **имя сервера** для *(localdb) \v11.0* и оставить **проверки подлинности Windows** как свой режим проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="bda2a-207">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="bda2a-208">Нажмите кнопку **Connect** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-208">Click **Connect** to continue.</span></span>

    <span data-ttu-id="bda2a-209">![Подключение к LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "подключение к LocalDB")</span><span class="sxs-lookup"><span data-stu-id="bda2a-209">![Connecting to LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="bda2a-210">*Подключение к LocalDB*</span><span class="sxs-lookup"><span data-stu-id="bda2a-210">*Connecting to LocalDB*</span></span>
11. <span data-ttu-id="bda2a-211">Откройте **GeekQuizProd** базы данных и разверните **таблиц** узла.</span><span class="sxs-lookup"><span data-stu-id="bda2a-211">Open the **GeekQuizProd** database and expand the **Tables** node.</span></span> <span data-ttu-id="bda2a-212">Как видите, **Update-Database** команда создается все таблицы, определенные в **TriviaContext** класса.</span><span class="sxs-lookup"><span data-stu-id="bda2a-212">As you can see, the **Update-Database** command generated all the tables defined in the **TriviaContext** class.</span></span> <span data-ttu-id="bda2a-213">Найдите **dbo. TriviaQuestions** таблицы и откройте узел «столбцы».</span><span class="sxs-lookup"><span data-stu-id="bda2a-213">Locate the **dbo.TriviaQuestions** table and open the columns node.</span></span> <span data-ttu-id="bda2a-214">В следующей задаче будет добавить новый столбец в этой таблице и обновлять базу данных при помощи **миграций**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-214">In the next task, you will add a new column to this table and update the database using **Migrations**.</span></span>

    <span data-ttu-id="bda2a-215">![Trivia вопросы столбцы](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia вопросы столбцов")</span><span class="sxs-lookup"><span data-stu-id="bda2a-215">![Trivia Questions Columns](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia Questions Columns")</span></span>

    <span data-ttu-id="bda2a-216">*Trivia вопросы столбцов*</span><span class="sxs-lookup"><span data-stu-id="bda2a-216">*Trivia Questions Columns*</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a><span data-ttu-id="bda2a-217">Задача 2 – обновление схемы базы данных с помощью миграции</span><span class="sxs-lookup"><span data-stu-id="bda2a-217">Task 2 – Updating Database Schema Using Migrations</span></span>

<span data-ttu-id="bda2a-218">В этой задаче будет использоваться **Entity Framework Code First Migrations** обнаруживать изменения в модели и создания необходимого кода для обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="bda2a-218">In this task, you will use **Entity Framework Code First Migrations** to detect a change in your model and generate the necessary code to update the database.</span></span> <span data-ttu-id="bda2a-219">Потребуется обновить **TriviaQuestions** сущности, добавив новое свойство.</span><span class="sxs-lookup"><span data-stu-id="bda2a-219">You will update the **TriviaQuestions** entity by adding a new property to it.</span></span> <span data-ttu-id="bda2a-220">Затем можно будет выполнить команды, чтобы создать новую миграцию для включения нового столбца в таблице.</span><span class="sxs-lookup"><span data-stu-id="bda2a-220">Then you will run commands to create a new Migration to include the new column in the table.</span></span>

1. <span data-ttu-id="bda2a-221">В **обозревателе решений**, дважды щелкните **TriviaQuestion.cs** файл, расположенный внутри **моделей** папки.</span><span class="sxs-lookup"><span data-stu-id="bda2a-221">In **Solution Explorer**, double-click the **TriviaQuestion.cs** file located inside the **Models** folder.</span></span>
2. <span data-ttu-id="bda2a-222">Добавить новое свойство с именем **указание**, как показано в следующем фрагменте кода.</span><span class="sxs-lookup"><span data-stu-id="bda2a-222">Add a new property named **Hint**, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. <span data-ttu-id="bda2a-223">В **консоль диспетчера пакетов**, введите следующую команду и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-223">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="bda2a-224">Будет создан новый миграции отражения изменений в нашей модели.</span><span class="sxs-lookup"><span data-stu-id="bda2a-224">A new migration will be created reflecting the change in our model.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    <span data-ttu-id="bda2a-225">![Добавить миграции](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "добавьте миграции")</span><span class="sxs-lookup"><span data-stu-id="bda2a-225">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span></span>

    <span data-ttu-id="bda2a-226">*Добавить миграции*</span><span class="sxs-lookup"><span data-stu-id="bda2a-226">*Add-Migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-227">Файл для миграции состоит из двух методов **копирование** и **работу**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-227">A Migration file is composed of two methods, **Up** and **Down**.</span></span>
    > 
    > - <span data-ttu-id="bda2a-228">**Копирование** метод будет использоваться для указания того, какие изменения в текущей версии наших потребностей приложения для применения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="bda2a-228">The **Up** method will be used to specify what changes the current version of our application need to apply to the database.</span></span>
    > - <span data-ttu-id="bda2a-229">**Работу** используется для отмены изменений, мы добавили **копирование** метод.</span><span class="sxs-lookup"><span data-stu-id="bda2a-229">The **Down** is used to reverse the changes we have added to the **Up** method.</span></span>
    > 
    > <span data-ttu-id="bda2a-230">При миграции базы данных обновляет базу данных, оно будет выполнено при всех переносах в порядке отметки времени и только те, которые не были использованы с момента последнего обновления ( \_MigrationHistory таблица отслеживает отслеживания объекта были применены какие миграции).</span><span class="sxs-lookup"><span data-stu-id="bda2a-230">When the Database Migration updates the database, it will run all migrations in the timestamp order, and only those that have not been used since the last update (The \_MigrationHistory table keeps track of which migrations have been applied).</span></span> <span data-ttu-id="bda2a-231">**Копирование** будет вызван метод всех операций миграции и будут внесены изменения, заданных в базу данных.</span><span class="sxs-lookup"><span data-stu-id="bda2a-231">The **Up** method of all migrations will be called and will make the changes we have specified to the database.</span></span> <span data-ttu-id="bda2a-232">Если мы решили вернуться к предыдущей миграции, **работу** метод будет вызываться, чтобы внести изменения в обратном порядке.</span><span class="sxs-lookup"><span data-stu-id="bda2a-232">If we decide to go back to a previous migration, the **Down** method will be called to redo the changes in a reverse order.</span></span>
4. <span data-ttu-id="bda2a-233">В **консоль диспетчера пакетов**, введите следующую команду и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-233">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. <span data-ttu-id="bda2a-234">Выходные данные **Update-Database** созданную команду **Alter Table** инструкции SQL, чтобы добавить новый столбец в **TriviaQuestions** таблицы, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="bda2a-234">The output of the **Update-Database** command generated an **Alter Table** SQL statement to add a new column to the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="bda2a-235">![Добавьте инструкцию SQL столбца создается](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "добавьте инструкцию SQL столбца создан")</span><span class="sxs-lookup"><span data-stu-id="bda2a-235">![Add column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Add column SQL statement generated")</span></span>

    <span data-ttu-id="bda2a-236">*Добавьте инструкцию SQL столбца создан*</span><span class="sxs-lookup"><span data-stu-id="bda2a-236">*Add column SQL statement generated*</span></span>
6. <span data-ttu-id="bda2a-237">В **обозреватель объектов SQL Server**, обновите **dbo. TriviaQuestions** таблицу и убедитесь, что новый **указание** отображается столбец.</span><span class="sxs-lookup"><span data-stu-id="bda2a-237">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the new **Hint** column is displayed.</span></span>

    <span data-ttu-id="bda2a-238">![Отображение нового столбца указание](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "отображение нового столбца подсказки")</span><span class="sxs-lookup"><span data-stu-id="bda2a-238">![Showing the new Hint Column](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Showing the new Hint Column")</span></span>

    <span data-ttu-id="bda2a-239">*Отображение нового столбца подсказки*</span><span class="sxs-lookup"><span data-stu-id="bda2a-239">*Showing the new Hint Column*</span></span>
7. <span data-ttu-id="bda2a-240">В **TriviaQuestion.cs** редактор добавьте **StringLength** ограничение *указание* свойства, как показано в следующем фрагменте кода.</span><span class="sxs-lookup"><span data-stu-id="bda2a-240">Back in the **TriviaQuestion.cs** editor, add a **StringLength** constraint to the *Hint* property, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. <span data-ttu-id="bda2a-241">В **консоль диспетчера пакетов**, введите следующую команду и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-241">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. <span data-ttu-id="bda2a-242">В **консоль диспетчера пакетов**, введите следующую команду и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-242">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. <span data-ttu-id="bda2a-243">Выходные данные **Update-Database** созданную команду **Alter Table** инструкции SQL для обновления *указание* столбец, имеющий тип **TriviaQuestions** таблицы, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="bda2a-243">The output of the **Update-Database** command generated an **Alter Table** SQL statement to update the *hint* column type of the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="bda2a-244">![Инструкции изменения столбцов SQL создается](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "инструкции изменения столбцов SQL создается")</span><span class="sxs-lookup"><span data-stu-id="bda2a-244">![Alter column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL statement generated")</span></span>

    <span data-ttu-id="bda2a-245">*Инструкции изменения столбцов SQL создается*</span><span class="sxs-lookup"><span data-stu-id="bda2a-245">*Alter column SQL statement generated*</span></span>
11. <span data-ttu-id="bda2a-246">В **обозреватель объектов SQL Server**, обновите **dbo. TriviaQuestions** таблицу и убедитесь, что **указание** столбец имеет тип **nvarchar(150)**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-246">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the **Hint** column type is **nvarchar(150)**.</span></span>

    <span data-ttu-id="bda2a-247">![Отображение нового ограничения](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "отображение нового ограничения")</span><span class="sxs-lookup"><span data-stu-id="bda2a-247">![Showing the new constraint](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Showing the new constraint")</span></span>

    <span data-ttu-id="bda2a-248">*Отображение нового ограничения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-248">*Showing the new constraint*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a><span data-ttu-id="bda2a-249">Упражнение 2: Развертывание веб-приложения для промежуточного хранения</span><span class="sxs-lookup"><span data-stu-id="bda2a-249">Exercise 2: Deploying a Web App to Staging</span></span>

<span data-ttu-id="bda2a-250">**Веб-приложений в службе приложений Azure** позволяет выполнять промежуточную публикацию.</span><span class="sxs-lookup"><span data-stu-id="bda2a-250">**Web Apps in Azure App Service** enables you to perform staged publishing.</span></span> <span data-ttu-id="bda2a-251">Промежуточную публикацию создает промежуточный слот сайта для каждого рабочего сайта по умолчанию и позволяет менять слоты без простоев.</span><span class="sxs-lookup"><span data-stu-id="bda2a-251">Staged publishing creates a staging site slot for each default production site and enables you to swap these slots with no down time.</span></span> <span data-ttu-id="bda2a-252">Это очень полезны для проверки изменений перед освобождением общедоступным, постепенно интегрировать содержимое сайта и откатывается, если изменения не работают надлежащим образом.</span><span class="sxs-lookup"><span data-stu-id="bda2a-252">This is really useful to validate changes before releasing to the public, incrementally integrate site content, and roll back if changes are not working as expected.</span></span>

<span data-ttu-id="bda2a-253">В этом упражнении мы Развернем **компьютерный фанат тест** приложение в промежуточной среде веб-приложения с помощью системы управления версиями Git.</span><span class="sxs-lookup"><span data-stu-id="bda2a-253">In this exercise, you will deploy the **Geek Quiz** application to the staging environment of your web app using Git source control.</span></span> <span data-ttu-id="bda2a-254">Чтобы сделать это, необходимо создать веб-приложение и подготовить все необходимые компоненты на портале управления, настроить **Git** репозитория и отправка приложения в исходном коде на локальном компьютере для промежуточного слота.</span><span class="sxs-lookup"><span data-stu-id="bda2a-254">To do this, you will create the web app and provision the required components at the management portal, configure a **Git** repository and push the application source code from your local computer to the staging slot.</span></span> <span data-ttu-id="bda2a-255">Обновит рабочей базы данных с **Code First Migrations** в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="bda2a-255">You will also update your production database with the **Code First Migrations** you created in the previous exercise.</span></span> <span data-ttu-id="bda2a-256">Затем будет выполняться приложение в данной тестовой среды для проверки своей работы.</span><span class="sxs-lookup"><span data-stu-id="bda2a-256">You will then execute the application in this test environment to verify its operation.</span></span> <span data-ttu-id="bda2a-257">После проверки, что приложение работает согласно ожиданиям, переходе приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="bda2a-257">Once you are satisfied that it is working according to your expectations, you will promote the application to production.</span></span>

> [!NOTE]
> <span data-ttu-id="bda2a-258">Чтобы включить промежуточную публикацию, должен быть веб-приложения в **стандартный режим**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-258">To enable staged publishing, the web app must be in **Standard mode**.</span></span> <span data-ttu-id="bda2a-259">Обратите внимание, что при изменении веб-приложения в стандартном режиме будет взиматься дополнительная плата.</span><span class="sxs-lookup"><span data-stu-id="bda2a-259">Note that additional charges will be incurred if you change your web app to Standard mode.</span></span> <span data-ttu-id="bda2a-260">Дополнительные сведения о ценах см. в разделе [цены на службу приложения](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="bda2a-260">For more information about pricing, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a><span data-ttu-id="bda2a-261">Задача 1 – Создание веб-приложения в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="bda2a-261">Task 1 – Creating a Web App in Azure App Service</span></span>

<span data-ttu-id="bda2a-262">В этой задаче вы создадите веб-приложения в **службе приложений Azure** на портале управления.</span><span class="sxs-lookup"><span data-stu-id="bda2a-262">In this task, you will create a web app in **Azure App Service** from the management portal.</span></span> <span data-ttu-id="bda2a-263">Необходимо также настроить **базы данных SQL** для сохранения данных приложения и настройки локального репозитория Git для системы управления версиями.</span><span class="sxs-lookup"><span data-stu-id="bda2a-263">You will also configure a **SQL Database** to persist the application data, and configure a local Git repository for source control.</span></span>

1. <span data-ttu-id="bda2a-264">Последовательно выберите пункты [портала управления Azure](https://manage.windowsazure.com) и войдите с использованием учетной записи Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="bda2a-264">Go to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>

    ![Войдите в портал управления Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    <span data-ttu-id="bda2a-266">*Войдите в портал управления Azure*</span><span class="sxs-lookup"><span data-stu-id="bda2a-266">*Sign in to the Azure management portal*</span></span>
2. <span data-ttu-id="bda2a-267">Нажмите кнопку **New** на панели команд в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="bda2a-267">Click **New** in the command bar at the bottom of the page.</span></span>

    <span data-ttu-id="bda2a-268">![Создание нового веб-приложения](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Создание нового веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="bda2a-268">![Creating a new web app](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Creating a new web app")</span></span>

    <span data-ttu-id="bda2a-269">*Создание нового веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-269">*Creating a new web app*</span></span>
3. <span data-ttu-id="bda2a-270">Нажмите кнопку **вычислений**, **веб-сайт** и затем **настраиваемое создание**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-270">Click **Compute**, **Website** and then **Custom Create**.</span></span>

    <span data-ttu-id="bda2a-271">![Создание нового веб-приложения с помощью настраиваемое создание](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Создание нового веб-приложения с помощью настраиваемое Создание")</span><span class="sxs-lookup"><span data-stu-id="bda2a-271">![Creating a new web app using Custom Create](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Creating a new web app using Custom Create")</span></span>

    <span data-ttu-id="bda2a-272">*Создание нового веб-приложения с помощью настраиваемое Создание*</span><span class="sxs-lookup"><span data-stu-id="bda2a-272">*Creating a new web app using Custom Create*</span></span>
4. <span data-ttu-id="bda2a-273">В **новый веб-узел - настраиваемое создание** диалогового окна введите доступный **URL-адрес** (например *головоломки компьютерный фанат*), выберите расположение, в **область** раскрывающегося списка, а затем выберите **создать новую базу данных SQL** в **базы данных** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="bda2a-273">In the **New Website - Custom Create** dialog box, provide an available **URL** (e.g. *geek-quiz*), select a location in the **Region** drop-down list, and select **Create a new SQL database** in the **Database** drop-down list.</span></span> <span data-ttu-id="bda2a-274">Наконец, выберите **публикации из системы управления версиями** флажок и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-274">Finally, select the **Publish from source control** check box and click **Next**.</span></span>

    ![Настройка нового веб-приложения](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    <span data-ttu-id="bda2a-276">*Настройка нового веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-276">*Customizing the new web app*</span></span>
5. <span data-ttu-id="bda2a-277">Укажите следующие сведения для настройки базы данных:</span><span class="sxs-lookup"><span data-stu-id="bda2a-277">Specify the following information for the database settings:</span></span>

    - <span data-ttu-id="bda2a-278">В **имя** текста введите имя базы данных (например *geekquiz\_db*)</span><span class="sxs-lookup"><span data-stu-id="bda2a-278">In the **Name** text box, enter a database name (e.g. *geekquiz\_db*)</span></span>
    - <span data-ttu-id="bda2a-279">На сервере **раскрывающемся** выберите **сервера базы данных SQL, новый**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-279">In the Server **drop-down** list, select **New SQL database server**.</span></span> <span data-ttu-id="bda2a-280">Кроме того можно выбрать существующий сервер.</span><span class="sxs-lookup"><span data-stu-id="bda2a-280">Alternatively, you can select an existing server.</span></span>
    - <span data-ttu-id="bda2a-281">В **имя пользователя базы данных** и **пароль базы данных** , введите имя пользователя администратора и пароль для базы данных SQL server.</span><span class="sxs-lookup"><span data-stu-id="bda2a-281">In the **Database username** and **Database password** boxes, enter the administrator username and password for the SQL database server.</span></span> <span data-ttu-id="bda2a-282">Если выбрать сервер уже создан, предложит ввести пароль.</span><span class="sxs-lookup"><span data-stu-id="bda2a-282">If you select a server you have already created, you will be prompted for the password.</span></span>

    ![Задание параметров базы данных](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

    <span data-ttu-id="bda2a-284">*Задание параметров базы данных*</span><span class="sxs-lookup"><span data-stu-id="bda2a-284">*Specifying the database settings*</span></span>
6. <span data-ttu-id="bda2a-285">Чтобы продолжить, нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="bda2a-285">Click **Next** to continue.</span></span>
7. <span data-ttu-id="bda2a-286">Выберите **локального репозитория Git** для системы управления версиями, и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-286">Select **Local Git repository** for the source control to use and click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-287">Вы может запрашиваться учетные данные развертывания (имя пользователя и пароль).</span><span class="sxs-lookup"><span data-stu-id="bda2a-287">You may be prompted for the deployment credentials (a username and password).</span></span>

    ![Создание репозитория Git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    <span data-ttu-id="bda2a-289">*Создание репозитория Git*</span><span class="sxs-lookup"><span data-stu-id="bda2a-289">*Creating the Git repository*</span></span>
8. <span data-ttu-id="bda2a-290">Подождите, пока не будет создан новый веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-290">Wait until the new web app is created.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-291">По умолчанию, Azure предоставляет доменах на *azurewebsites.net* , но также предоставляет возможность задать пользовательские домены, с помощью портала управления Azure.</span><span class="sxs-lookup"><span data-stu-id="bda2a-291">By default, Azure provides domains at *azurewebsites.net* but also gives you the possibility to set custom domains using the Azure management portal.</span></span> <span data-ttu-id="bda2a-292">Тем не менее можно управлять только пользовательских доменов при использовании определенных режимах службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="bda2a-292">However, you can only manage custom domains if you are using certain Azure App Service modes.</span></span>
    > 
    > <span data-ttu-id="bda2a-293">Служба приложений Azure доступен в выпусках Free, Shared, Basic, Standard и Premium.</span><span class="sxs-lookup"><span data-stu-id="bda2a-293">Azure App Service is available in Free, Shared, Basic, Standard, and Premium editions.</span></span> <span data-ttu-id="bda2a-294">В режиме Free и Shared все веб-приложения выполняются в многопользовательской среде и имеет квот на использование ЦП, памяти и сети.</span><span class="sxs-lookup"><span data-stu-id="bda2a-294">In Free and Shared mode, all web apps run in a multi-tenant environment and have quotas for CPU, Memory, and Network usage.</span></span> <span data-ttu-id="bda2a-295">Максимальное число бесплатных приложений зависит от плана.</span><span class="sxs-lookup"><span data-stu-id="bda2a-295">The maximum number of free apps may vary with your plan.</span></span> <span data-ttu-id="bda2a-296">В стандартном режиме выберите, какие приложения запустить на выделенных виртуальных машинах, которые соответствуют стандартной Azure вычислительные ресурсы.</span><span class="sxs-lookup"><span data-stu-id="bda2a-296">In Standard mode, you choose which apps run on dedicated virtual machines that correspond to the standard Azure compute resources.</span></span> <span data-ttu-id="bda2a-297">Можно найти конфигурацию в режиме web app в **шкалы** меню веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-297">You can find the web app mode configuration in the **Scale** menu of your web app.</span></span>
    > 
    > <span data-ttu-id="bda2a-298">![Служба приложений Azure режимы](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "режимы службы приложений Azure")</span><span class="sxs-lookup"><span data-stu-id="bda2a-298">![Azure App Service Modes](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service Modes")</span></span>
    > 
    > <span data-ttu-id="bda2a-299">Если вы используете **Shared** или **Стандартная** режиме, вы сможете управлять пользовательских доменов для веб-приложения, перейдя в свое приложение **Настройка** меню и выбрав пункт **Управление доменами** под *доменных имен*.</span><span class="sxs-lookup"><span data-stu-id="bda2a-299">If you are using **Shared** or **Standard** mode, you will be able to manage custom domains for your web app by going to your app's **Configure** menu and clicking **Manage Domains** under *domain names*.</span></span>
    > 
    > <span data-ttu-id="bda2a-300">![Управление доменами](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Управление доменами")</span><span class="sxs-lookup"><span data-stu-id="bda2a-300">![Manage Domains](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Manage Domains")</span></span>
    > 
    > <span data-ttu-id="bda2a-301">![Управление пользовательскими доменами](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "управление пользовательскими доменами")</span><span class="sxs-lookup"><span data-stu-id="bda2a-301">![Manage Custom Domains](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Manage Custom Domains")</span></span>
9. <span data-ttu-id="bda2a-302">После создания веб-приложения по ссылке **URL-адрес** столбец для проверки того, что новое веб-приложение работает.</span><span class="sxs-lookup"><span data-stu-id="bda2a-302">Once the web app is created, click the link under the **URL** column to check that the new web app is running.</span></span>

    ![Просмотр нового веб-приложения](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    <span data-ttu-id="bda2a-304">*Просмотр нового веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-304">*Browsing to the new web app*</span></span>

    ![Запуск веб-приложения](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    <span data-ttu-id="bda2a-306">*Запуск веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-306">*web app running*</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a><span data-ttu-id="bda2a-307">Задача 2 – Создание рабочей базы данных SQL</span><span class="sxs-lookup"><span data-stu-id="bda2a-307">Task 2 – Creating the Production SQL Database</span></span>

<span data-ttu-id="bda2a-308">В этой задаче будет использоваться **Entity Framework Code First Migrations** для создания базы данных, предназначенных для **базы данных SQL Azure** экземпляром, созданным в предыдущей задаче.</span><span class="sxs-lookup"><span data-stu-id="bda2a-308">In this task, you will use the **Entity Framework Code First Migrations** to create the database targeting the **Azure SQL Database** instance you created in the previous task.</span></span>

1. <span data-ttu-id="bda2a-309">На портале управления перейдите к веб-приложения, созданного в предыдущей задаче и перейти к его **мониторинга**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-309">In the Management Portal, navigate to the web app you created in the previous task and go to its **Dashboard**.</span></span>
2. <span data-ttu-id="bda2a-310">В **мониторинга** щелкните **Просмотр строк подключения** ссылку в списке **быстрый обзор** раздела.</span><span class="sxs-lookup"><span data-stu-id="bda2a-310">In the **Dashboard** page, click **View connection strings** link under the **quick glance** section.</span></span>

    <span data-ttu-id="bda2a-311">![Просмотреть строки подключения](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "Просмотр строк подключения")</span><span class="sxs-lookup"><span data-stu-id="bda2a-311">![View connection strings](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "View connection strings")</span></span>

    <span data-ttu-id="bda2a-312">*Просмотр строк подключения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-312">*View connection strings*</span></span>
3. <span data-ttu-id="bda2a-313">Копировать **строка подключения** значение и закрыть диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="bda2a-313">Copy the **connection string** value and close the dialog box.</span></span>

    <span data-ttu-id="bda2a-314">![Строка подключения на портале управления Azure](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "строку подключения на портале управления Azure")</span><span class="sxs-lookup"><span data-stu-id="bda2a-314">![Connection String in Azure Management Portal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Connection String in Azure Management Portal")</span></span>

    <span data-ttu-id="bda2a-315">*Строка подключения на портале управления Azure*</span><span class="sxs-lookup"><span data-stu-id="bda2a-315">*Connection String in Azure Management Portal*</span></span>
4. <span data-ttu-id="bda2a-316">Нажмите кнопку **баз данных SQL** для просмотра списка баз данных SQL в Azure</span><span class="sxs-lookup"><span data-stu-id="bda2a-316">Click **SQL Databases** to see the list of SQL databases in Azure</span></span>

    <span data-ttu-id="bda2a-317">![База данных SQL меню](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "меню базы данных SQL")</span><span class="sxs-lookup"><span data-stu-id="bda2a-317">![SQL Database menu](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database menu")</span></span>

    <span data-ttu-id="bda2a-318">*Меню базы данных SQL*</span><span class="sxs-lookup"><span data-stu-id="bda2a-318">*SQL Database menu*</span></span>
5. <span data-ttu-id="bda2a-319">Найдите базу данных, созданный в предыдущей задаче и нажмите кнопку на сервере.</span><span class="sxs-lookup"><span data-stu-id="bda2a-319">Locate the database you created in the previous task and click on the Server.</span></span>

    <span data-ttu-id="bda2a-320">![База данных SQL server](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "базы данных SQL server")</span><span class="sxs-lookup"><span data-stu-id="bda2a-320">![SQL Database server](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database server")</span></span>

    <span data-ttu-id="bda2a-321">*База данных SQL server*</span><span class="sxs-lookup"><span data-stu-id="bda2a-321">*SQL Database server*</span></span>
6. <span data-ttu-id="bda2a-322">В **быстрый запуск** страницы сервера, нажмите кнопку **Настройка**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-322">In the **Quick Start** page of the server, click on **Configure**.</span></span>

    <span data-ttu-id="bda2a-323">![Настройка меню](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Настройка меню")</span><span class="sxs-lookup"><span data-stu-id="bda2a-323">![Configure menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Configure menu")</span></span>

    <span data-ttu-id="bda2a-324">*Настройка меню*</span><span class="sxs-lookup"><span data-stu-id="bda2a-324">*Configure menu*</span></span>
7. <span data-ttu-id="bda2a-325">В **допускается IP-адреса** щелкните **добавить в разрешенные IP-адреса** ссылку, чтобы включить на IP-адрес для подключения к базе данных SQL server.</span><span class="sxs-lookup"><span data-stu-id="bda2a-325">In the **Allowed IP addresses** section, click on **Add to the allowed IP addresses** link to enable your IP to connect to the SQL Database server.</span></span>

    <span data-ttu-id="bda2a-326">![Разрешенные IP-адреса](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "допускается IP-адресов")</span><span class="sxs-lookup"><span data-stu-id="bda2a-326">![Allowed IP addresses](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Allowed IP addresses")</span></span>

    <span data-ttu-id="bda2a-327">*Разрешенные IP-адреса*</span><span class="sxs-lookup"><span data-stu-id="bda2a-327">*Allowed IP addresses*</span></span>
8. <span data-ttu-id="bda2a-328">Нажмите кнопку **Сохранить** в нижней части страницы для выполнения шага.</span><span class="sxs-lookup"><span data-stu-id="bda2a-328">Click **Save** at the bottom of the page to complete the step.</span></span>
9. <span data-ttu-id="bda2a-329">Переключитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bda2a-329">Switch back to Visual Studio.</span></span>
10. <span data-ttu-id="bda2a-330">В **консоль диспетчера пакетов**, выполните следующие команды, заменив *[строка YOUR подключения]* заполнителя в строке подключения, скопированные из Azure</span><span class="sxs-lookup"><span data-stu-id="bda2a-330">In the **Package Manager Console**, execute the following command replacing *[YOUR-CONNECTION-STRING]* placeholder with the connection string you copied from Azure</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    <span data-ttu-id="bda2a-331">![Обновление базы данных, предназначенных для Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "обновление базы данных, предназначенных для базы данных SQL Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="bda2a-331">![Update database targeting Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Update database targeting Windows Azure SQL Database")</span></span>

    <span data-ttu-id="bda2a-332">*Обновление базы данных, предназначенных для базы данных SQL Azure*</span><span class="sxs-lookup"><span data-stu-id="bda2a-332">*Update database targeting Azure SQL Database*</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a><span data-ttu-id="bda2a-333">Задача 3 – развертывание компьютерный фанат тест для промежуточного хранения, с помощью Git</span><span class="sxs-lookup"><span data-stu-id="bda2a-333">Task 3 – Deploying Geek Quiz to Staging Using Git</span></span>

<span data-ttu-id="bda2a-334">В этой задаче будет включить промежуточную публикацию в веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-334">In this task, you will enable staged publishing in your web app.</span></span> <span data-ttu-id="bda2a-335">После этого вы используете Git для публикации приложения головоломка компьютерный фанат непосредственно с локального компьютера в промежуточной среде веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-335">Then, you will use Git to publish the Geek Quiz application directly from your local computer to the staging environment of your web app.</span></span>

1. <span data-ttu-id="bda2a-336">Вернитесь на портал и щелкните имя веб-приложения в разделе **имя** столбец для отображения страницы управления.</span><span class="sxs-lookup"><span data-stu-id="bda2a-336">Go back to the portal and click the name of the web app under the **Name** column to display the management pages.</span></span>

    ![Открытие приложения управления веб-страниц](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    <span data-ttu-id="bda2a-338">*Открытие приложения управления веб-страниц*</span><span class="sxs-lookup"><span data-stu-id="bda2a-338">*Opening the web app management pages*</span></span>
2. <span data-ttu-id="bda2a-339">Перейдите к **шкалы** страницы.</span><span class="sxs-lookup"><span data-stu-id="bda2a-339">Navigate to the **Scale** page.</span></span> <span data-ttu-id="bda2a-340">В разделе **Общие** выберите **Стандартная** конфигурации и нажмите кнопку **Сохранить** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="bda2a-340">Under the **general** section, select **Standard** for the configuration and click **Save** in the command bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-341">Для выполнения всех веб-приложений в текущей области и подписки в **Стандартная** режиме, оставьте **выделить все** флажком в **выберите сайты** конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bda2a-341">To run all web apps in the current region and subscription in **Standard** mode, leave the **Select All** check box selected in the **Choose Sites** configuration.</span></span> <span data-ttu-id="bda2a-342">В противном случае снимите **выделить все** флажок.</span><span class="sxs-lookup"><span data-stu-id="bda2a-342">Otherwise, clear the **Select All** check box.</span></span>

    <span data-ttu-id="bda2a-343">![Обновление веб-приложения в стандартном режиме](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "обновление до стандартного режима веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="bda2a-343">![Upgrading the web app to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Upgrading the web app to Standard mode")</span></span>

    <span data-ttu-id="bda2a-344">*Обновление до стандартного режима веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-344">*Upgrading the Web App to Standard mode*</span></span>
3. <span data-ttu-id="bda2a-345">Нажмите кнопку **Да** для подтверждения изменения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-345">Click **Yes** to confirm the changes.</span></span>

    <span data-ttu-id="bda2a-346">![Подтверждение изменений в стандартном режиме](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "продолжить изменение режима приложения веб-сервере")</span><span class="sxs-lookup"><span data-stu-id="bda2a-346">![Confirming the change to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuing with the changing of the web app mode")</span></span>

    <span data-ttu-id="bda2a-347">*Подтверждение изменений в стандартном режиме*</span><span class="sxs-lookup"><span data-stu-id="bda2a-347">*Confirming the change to Standard mode*</span></span>
4. <span data-ttu-id="bda2a-348">Последовательно выберите пункты **мониторинга** и нажмите кнопку **Enable промежуточная публикация** под **быстрый обзор** раздела.</span><span class="sxs-lookup"><span data-stu-id="bda2a-348">Go to the **Dashboard** page and click **Enable staged publishing** under the **quick glance** section.</span></span>

    <span data-ttu-id="bda2a-349">![Включить промежуточную публикацию](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Включение промежуточная публикация")</span><span class="sxs-lookup"><span data-stu-id="bda2a-349">![Enabling staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Enabling staged publishing")</span></span>

    <span data-ttu-id="bda2a-350">*Включить промежуточную публикацию*</span><span class="sxs-lookup"><span data-stu-id="bda2a-350">*Enabling staged publishing*</span></span>
5. <span data-ttu-id="bda2a-351">Нажмите кнопку **Да** хотите включить промежуточную публикацию.</span><span class="sxs-lookup"><span data-stu-id="bda2a-351">Click **Yes** to enable staged publishing.</span></span>

    <span data-ttu-id="bda2a-352">![Подтверждение промежуточная публикация](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "щелкните Да, чтобы включить промежуточную публикацию")</span><span class="sxs-lookup"><span data-stu-id="bda2a-352">![Confirming staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Clicking Yes to enable staged publishing")</span></span>

    <span data-ttu-id="bda2a-353">*Подтверждение промежуточную публикацию*</span><span class="sxs-lookup"><span data-stu-id="bda2a-353">*Confirming staged publishing*</span></span>
6. <span data-ttu-id="bda2a-354">В списке веб-приложений разверните знак слева от имя вашего веб-приложения для отображения промежуточный слот сайта.</span><span class="sxs-lookup"><span data-stu-id="bda2a-354">In the list of web apps, expand the mark to the left of your web app name to display the staging site slot.</span></span> <span data-ttu-id="bda2a-355">Он имеет имя веб-приложения, за которым следует ***(промежуточный)***.</span><span class="sxs-lookup"><span data-stu-id="bda2a-355">It has the name of your web app followed by ***(staging)***.</span></span> <span data-ttu-id="bda2a-356">Щелкните промежуточный сайт, чтобы перейти на страницу управления.</span><span class="sxs-lookup"><span data-stu-id="bda2a-356">Click the staging site to go to the management page.</span></span>

    <span data-ttu-id="bda2a-357">![Переход к промежуточной веб-приложения](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "переход к промежуточной веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="bda2a-357">![Navigating to the staging web app](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigating to the staging web app")</span></span>

    <span data-ttu-id="bda2a-358">*Переход к промежуточного приложения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-358">*Navigating to the staging app*</span></span>
7. <span data-ttu-id="bda2a-359">Обратите внимание, что эта страница управления он выглядит как страница управления любые другие веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-359">Notice that he management page looks like any other web app's management page.</span></span> <span data-ttu-id="bda2a-360">Перейдите к **развертываний** страницы и скопируйте **URL-адрес Git** значение.</span><span class="sxs-lookup"><span data-stu-id="bda2a-360">Navigate to the **Deployments** page and copy the **Git URL** value.</span></span> <span data-ttu-id="bda2a-361">Далее в этом упражнении будет использовать его.</span><span class="sxs-lookup"><span data-stu-id="bda2a-361">You will use it later in this exercise.</span></span>

    ![При копировании значения URL-адрес Git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    <span data-ttu-id="bda2a-363">*При копировании значения URL-адрес Git*</span><span class="sxs-lookup"><span data-stu-id="bda2a-363">*Copying the Git URL value*</span></span>
8. <span data-ttu-id="bda2a-364">Откройте новый **Git Bash** консоли и выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="bda2a-364">Open a new **Git Bash** console and execute the following commands.</span></span> <span data-ttu-id="bda2a-365">Обновление *[путь ВАШЕГО приложения]* заполнитель с путем к **GeekQuiz** решений, расположенный в **Source\Ex1 DeployingWebSiteToStaging\Begin** папки Эта лаборатория.</span><span class="sxs-lookup"><span data-stu-id="bda2a-365">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution, located in the **Source\Ex1-DeployingWebSiteToStaging\Begin** folder of this lab.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Инициализация Git и первой сборки](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    <span data-ttu-id="bda2a-367">*Инициализация Git и первой сборки*</span><span class="sxs-lookup"><span data-stu-id="bda2a-367">*Git initialization and first commit*</span></span>
9. <span data-ttu-id="bda2a-368">Выполните следующую команду для принудительной отправки веб-приложения на удаленном **Git** репозитория.</span><span class="sxs-lookup"><span data-stu-id="bda2a-368">Run the following command to push your web app to the remote **Git** repository.</span></span> <span data-ttu-id="bda2a-369">Замените заполнитель URL-адрес, полученный из портала управления.</span><span class="sxs-lookup"><span data-stu-id="bda2a-369">Replace the placeholder with the URL you obtained from the management portal.</span></span> <span data-ttu-id="bda2a-370">Будет приглашение ввести пароль развертывания.</span><span class="sxs-lookup"><span data-stu-id="bda2a-370">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Помещает в Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    <span data-ttu-id="bda2a-372">*Отправка в Azure*</span><span class="sxs-lookup"><span data-stu-id="bda2a-372">*Pushing to Azure*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-373">При развертывании содержимого на FTP-узле или репозитории GIT веб-приложения, необходимо пройти подлинность с помощью **учетные данные развертывания** , созданный на основе веб-приложения **быстрый запуск** или **панели мониторинга**  страниц управления.</span><span class="sxs-lookup"><span data-stu-id="bda2a-373">When you deploy content to the FTP host or GIT repository of a web app, you must authenticate using the **deployment credentials** that you created from the web app's **Quick Start** or **Dashboard** management pages.</span></span> <span data-ttu-id="bda2a-374">Если вы не знаете, учетные данные развертывания можно легко сбросить их с помощью портала управления.</span><span class="sxs-lookup"><span data-stu-id="bda2a-374">If you do not know your deployment credentials you can easily reset them using the management portal.</span></span> <span data-ttu-id="bda2a-375">Откройте веб-приложения **мониторинга** и нажмите кнопку **Сброс учетных данных развертывания** ссылку.</span><span class="sxs-lookup"><span data-stu-id="bda2a-375">Open the web app **Dashboard** page and click the **Reset your deployment credentials** link.</span></span> <span data-ttu-id="bda2a-376">Введите новый пароль и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-376">Provide a new password and click **OK**.</span></span> <span data-ttu-id="bda2a-377">Учетные данные развертывания можно использовать для всех веб-приложений, связанных с подпиской.</span><span class="sxs-lookup"><span data-stu-id="bda2a-377">Deployment credentials are valid for use with all web apps associated with your subscription.</span></span>
10. <span data-ttu-id="bda2a-378">Чтобы проверить веб-приложения был успешно отправлен в Azure, вернитесь на портал управления и нажмите кнопку **веб-сайтов**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-378">In order to verify the web app was successfully pushed to Azure, go back to the management portal and click **Websites**.</span></span>
11. <span data-ttu-id="bda2a-379">Найдите веб-приложения и разверните запись для отображения промежуточный слот сайта.</span><span class="sxs-lookup"><span data-stu-id="bda2a-379">Locate your web app and expand the entry to display the staging site slot.</span></span> <span data-ttu-id="bda2a-380">Щелкните его **имя** переход на страницу управления.</span><span class="sxs-lookup"><span data-stu-id="bda2a-380">Click its **Name** to go to the management page.</span></span>
12. <span data-ttu-id="bda2a-381">Нажмите кнопку **развертываний** для просмотра **журнала развертывания**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-381">Click **Deployments** to see the **deployment history**.</span></span> <span data-ttu-id="bda2a-382">Убедитесь, что **активное развертывание** с вашей  *&quot;начальной зафиксировать&quot;*.</span><span class="sxs-lookup"><span data-stu-id="bda2a-382">Verify that there is an **Active Deployment** with your *&quot;Initial Commit&quot;*.</span></span>

    ![Активное развертывание](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    <span data-ttu-id="bda2a-384">*Активное развертывание*</span><span class="sxs-lookup"><span data-stu-id="bda2a-384">*Active deployment*</span></span>
13. <span data-ttu-id="bda2a-385">Наконец, нажмите кнопку **Обзор** на панели команд, чтобы перейти на веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-385">Finally, click **Browse** in the command bar to go to the web app.</span></span>

    ![Обзор веб-приложения](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    <span data-ttu-id="bda2a-387">*Обзор веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-387">*Browse web app*</span></span>
14. <span data-ttu-id="bda2a-388">Если приложение успешно развернуто, вы увидите страницу входа компьютерный фанат тест.</span><span class="sxs-lookup"><span data-stu-id="bda2a-388">If the application is successfully deployed, you will see the Geek Quiz login page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-389">URL-адрес развертываемом приложении содержит имя веб-приложения, за которым следует *-промежуточных*.</span><span class="sxs-lookup"><span data-stu-id="bda2a-389">The address URL of the deployed application contains the name of your web app followed by *-staging*.</span></span>

    ![Приложение, работающее в среде промежуточного хранения](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    <span data-ttu-id="bda2a-391">*Приложение, работающее в среде промежуточного хранения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-391">*Application running in the staging environment*</span></span>
15. <span data-ttu-id="bda2a-392">Если вы хотите исследовать приложения, нажмите кнопку **зарегистрировать** для регистрации нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="bda2a-392">If you wish to explore the application, click **Register** to register a new user.</span></span> <span data-ttu-id="bda2a-393">Завершите сведения учетной записи, указав имя пользователя, адрес электронной почты и пароль.</span><span class="sxs-lookup"><span data-stu-id="bda2a-393">Complete the account details by entering a user name, email address and password.</span></span> <span data-ttu-id="bda2a-394">После этого приложение показывает первый вопрос головоломки.</span><span class="sxs-lookup"><span data-stu-id="bda2a-394">Next, the application shows the first question of the quiz.</span></span> <span data-ttu-id="bda2a-395">Ответьте на несколько вопросов, чтобы убедиться в том, что он работает должным образом.</span><span class="sxs-lookup"><span data-stu-id="bda2a-395">Answer a few questions to make sure it is working as expected.</span></span>

    ![Приложение можно будет использовать](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    <span data-ttu-id="bda2a-397">*Приложение можно будет использовать*</span><span class="sxs-lookup"><span data-stu-id="bda2a-397">*Application ready to be used*</span></span>

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a><span data-ttu-id="bda2a-398">Задача 4 — повышение уровня веб-приложения в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="bda2a-398">Task 4 – Promoting the Web App to Production</span></span>

<span data-ttu-id="bda2a-399">Теперь, когда вы убедились, что веб-приложение работает правильно, в промежуточной среде, можно приступать к его распространение в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="bda2a-399">Now that you have verified that the web app is working correctly in the staging environment, you are ready to promote it to production.</span></span> <span data-ttu-id="bda2a-400">В этой задаче будет переключить промежуточный слот сайта на производственный слот сайта.</span><span class="sxs-lookup"><span data-stu-id="bda2a-400">In this task, you will swap the staging site slot with the production site slot.</span></span>

1. <span data-ttu-id="bda2a-401">Вернитесь на портал управления и выберите промежуточный слот сайта.</span><span class="sxs-lookup"><span data-stu-id="bda2a-401">Go back to the management portal and select the staging site slot.</span></span> <span data-ttu-id="bda2a-402">Нажмите кнопку **замены** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="bda2a-402">Click **Swap** in the command bar.</span></span>

    ![Переключение в рабочей среде](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    <span data-ttu-id="bda2a-404">*Переключение в рабочей среде*</span><span class="sxs-lookup"><span data-stu-id="bda2a-404">*Swap to production*</span></span>
2. <span data-ttu-id="bda2a-405">Нажмите кнопку **Да** в диалоговом окне подтверждения, чтобы продолжить операцию замены.</span><span class="sxs-lookup"><span data-stu-id="bda2a-405">Click **Yes** in the confirmation dialog box to proceed with the swap operation.</span></span> <span data-ttu-id="bda2a-406">Azure будет немедленно замены содержимого рабочего сайта с помощью содержимого промежуточный сайт.</span><span class="sxs-lookup"><span data-stu-id="bda2a-406">Azure will immediately swap the content of the production site with the content of the staging site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-407">Некоторые параметры из промежуточных версии будет автоматически копироваться в рабочей версии (например строка соединения переопределений, сопоставления обработчика, т. д.), но другие параметры не изменится (например DNS конечные точки, привязки SSL, и т. д.).</span><span class="sxs-lookup"><span data-stu-id="bda2a-407">Some settings from the staged version will automatically be copied to the production version (e.g. connection string overrides, handler mappings, etc.) but other settings will not change (e.g. DNS endpoints, SSL bindings, etc.).</span></span>

    ![Подтверждение операции замены](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    <span data-ttu-id="bda2a-409">*Подтверждение операции замены*</span><span class="sxs-lookup"><span data-stu-id="bda2a-409">*Confirming swap operation*</span></span>
3. <span data-ttu-id="bda2a-410">После завершения замены выберите производственный слот и нажмите кнопку **Обзор** на панели команд, чтобы открыть рабочего сайта.</span><span class="sxs-lookup"><span data-stu-id="bda2a-410">Once the swap is complete, select the production slot and click **Browse** in the command bar to open the production site.</span></span> <span data-ttu-id="bda2a-411">Обратите внимание, URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="bda2a-411">Notice the URL in the address bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-412">Может потребоваться обновить браузер, чтобы очистить кэш.</span><span class="sxs-lookup"><span data-stu-id="bda2a-412">You might need to refresh your browser to clear cache.</span></span> <span data-ttu-id="bda2a-413">В Internet Explorer, это можно сделать, нажав клавишу **CTRL + R**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-413">In Internet Explorer, you can do this by pressing **CTRL+R**.</span></span>

    ![Веб-приложения в рабочей среде](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. <span data-ttu-id="bda2a-415">В **GitBash** консоли, обновить удаленный URL-адрес локального репозитория Git ориентироваться на производственный слот.</span><span class="sxs-lookup"><span data-stu-id="bda2a-415">In the **GitBash** console, update the remote URL for the local Git repository to target the production slot.</span></span> <span data-ttu-id="bda2a-416">Чтобы сделать это, выполните следующую команду, заменив заполнители с вашим именем пользователя развертывания и имя веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-416">To do this, run the following command replacing the placeholders with your deployment username and the name of your web app.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-417">В следующих упражнениях будет отправьте изменения рабочего сайта вместо промежуточной только для простоты лаборатории.</span><span class="sxs-lookup"><span data-stu-id="bda2a-417">In the following exercises, you will push changes to the production site instead of staging just for the simplicity of the lab.</span></span> <span data-ttu-id="bda2a-418">В реальном сценарии рекомендуется проверить изменения в промежуточной среде перед повышением роли в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="bda2a-418">In a real-world scenario, it is recommended to verify the changes in the staging environment before promoting to production.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a><span data-ttu-id="bda2a-419">Упражнение 3: Выполнение отката развертывания в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="bda2a-419">Exercise 3: Performing Deployment Rollback in Production</span></span>

<span data-ttu-id="bda2a-420">Существуют сценарии, где у вас промежуточный слот для выполнения горячей замены между промежуточной и производственной среде, например, если вы работаете с **Free** или **Shared** режим.</span><span class="sxs-lookup"><span data-stu-id="bda2a-420">There are scenarios where you do not have a staging slot to perform hot swap between staging and production, for example, if you are working with **Free** or **Shared** mode.</span></span> <span data-ttu-id="bda2a-421">В этих сценариях следует протестировать приложение в тестовой среде — локально или на удаленном узле — перед развертыванием в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="bda2a-421">In those scenarios, you should test your application in a testing environment –either locally or in a remote site– before deploying to production.</span></span> <span data-ttu-id="bda2a-422">Тем не менее существует возможность, что проблемы не обнаружены на этапе тестирования может возникнуть в рабочего сайта.</span><span class="sxs-lookup"><span data-stu-id="bda2a-422">However, it is possible that an issue not detected during the testing phase may arise in the production site.</span></span> <span data-ttu-id="bda2a-423">В этом случае важно иметь механизм для простоты перехода предыдущий и более стабильной версии приложения, как можно быстрее.</span><span class="sxs-lookup"><span data-stu-id="bda2a-423">In this case, it is important to have a mechanism to easily switch to a previous and more stable version of the application as quickly as possible.</span></span>

<span data-ttu-id="bda2a-424">В **службе приложений Azure**, непрерывное развертывание из системы управления версиями делает это возможным благодаря для **повторно развернуть** действия, доступные на портале управления.</span><span class="sxs-lookup"><span data-stu-id="bda2a-424">In **Azure App Service**, continuous deployment from source control makes this possible thanks to the **redeploy** action available in the management portal.</span></span> <span data-ttu-id="bda2a-425">Azure отслеживает развертываний, связанных с фиксаций передаче в репозиторий и предоставляет возможность повторного развертывания приложения с помощью любой из предыдущих развертываний, в любое время.</span><span class="sxs-lookup"><span data-stu-id="bda2a-425">Azure keeps track of the deployments associated with the commits pushed to the repository and provides an option to redeploy your application using any of your previous deployments, at any time.</span></span>

<span data-ttu-id="bda2a-426">В этом упражнении будет выполнять изменений в код в **головоломка компьютерный фанат** приложения, которое вставляет намеренно *ошибки*.</span><span class="sxs-lookup"><span data-stu-id="bda2a-426">In this exercise you will perform a change to the code in the **Geek Quiz** application that intentionally injects a *bug*.</span></span> <span data-ttu-id="bda2a-427">Будет развертывание приложения в производственную среду, чтобы увидеть ошибку и затем будет воспользоваться преимуществами функции повторного развертывания, чтобы вернуться к предыдущему состоянию.</span><span class="sxs-lookup"><span data-stu-id="bda2a-427">You will deploy the application to production to see the error, and then you will take advantage of the redeploy feature to go back to the previous state.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a><span data-ttu-id="bda2a-428">Задача 1 – обновление приложения компьютерный фанат головоломки</span><span class="sxs-lookup"><span data-stu-id="bda2a-428">Task 1 – Updating the Geek Quiz Application</span></span>

<span data-ttu-id="bda2a-429">В этой задаче рефакторинга небольшая часть кода **TriviaController** класс для извлечения часть логики, которая извлекает головоломки выбранный параметр из базы данных в новый метод.</span><span class="sxs-lookup"><span data-stu-id="bda2a-429">In this task, you will refactor a small piece of code of the **TriviaController** class to extract part of the logic that retrieves the selected quiz option from the database into a new method.</span></span>

1. <span data-ttu-id="bda2a-430">Перейдите к экземпляру Visual Studio с **GeekQuiz** решения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="bda2a-430">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="bda2a-431">В **обозревателе решений**откройте **TriviaController.cs** файла внутри **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="bda2a-431">In **Solution Explorer**, open the **TriviaController.cs** file inside the **Controllers** folder.</span></span>
3. <span data-ttu-id="bda2a-432">Найдите **StoreAsync** метод и выберите код выделяются на следующем рисунке.</span><span class="sxs-lookup"><span data-stu-id="bda2a-432">Locate the **StoreAsync** method and select the code highlighted in the following figure.</span></span>

    ![При выборе в коде](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    <span data-ttu-id="bda2a-434">*При выборе в коде*</span><span class="sxs-lookup"><span data-stu-id="bda2a-434">*Selecting the code*</span></span>
4. <span data-ttu-id="bda2a-435">Щелкните правой кнопкой мыши выделенный код, разверните **рефакторинг** и выбрать пункт **метода извлечения...** .</span><span class="sxs-lookup"><span data-stu-id="bda2a-435">Right-click the selected code, expand the **Refactor** menu and select **Extract Method...**.</span></span>

    ![Извлечение как новый метод в коде](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    <span data-ttu-id="bda2a-437">*Выбор способа извлечения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-437">*Selecting Extract Method*</span></span>
5. <span data-ttu-id="bda2a-438">В **извлечение метода** диалоговом имя нового метода *MatchesOption* и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-438">In the **Extract Method** dialog box, name the new method *MatchesOption* and click **OK**.</span></span>

    ![Указывая имя метода](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    <span data-ttu-id="bda2a-440">*Указание имени для извлечения метода*</span><span class="sxs-lookup"><span data-stu-id="bda2a-440">*Specifying the name for the extracted method*</span></span>
6. <span data-ttu-id="bda2a-441">Выделенный код затем извлекается в **MatchesOption** метод.</span><span class="sxs-lookup"><span data-stu-id="bda2a-441">The selected code is then extracted into the **MatchesOption** method.</span></span> <span data-ttu-id="bda2a-442">В следующем фрагменте показан итоговый код.</span><span class="sxs-lookup"><span data-stu-id="bda2a-442">The resulting code is shown in the following snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. <span data-ttu-id="bda2a-443">Нажмите клавишу **сочетание клавиш CTRL + S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="bda2a-443">Press **CTRL + S** to save the changes.</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a><span data-ttu-id="bda2a-444">Задача 2 — повторного развертывания приложения компьютерный фанат головоломки</span><span class="sxs-lookup"><span data-stu-id="bda2a-444">Task 2 – Redeploying the Geek Quiz Application</span></span>

<span data-ttu-id="bda2a-445">Теперь будет передавать изменения, внесенные в предыдущей задаче репозитория, который будет активировать новое развертывание в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="bda2a-445">You will now push the changes you made in the previous task to the repository, which will trigger a new deployment to the production environment.</span></span> <span data-ttu-id="bda2a-446">После этого будет и устранение проблем проблему с помощью **средства разработки F12** предоставляемое Internet Explorer и затем выполнить откат предыдущего развертывания на портале управления Azure.</span><span class="sxs-lookup"><span data-stu-id="bda2a-446">Then, you will troubleshot an issue using the **F12 development tools** provided by Internet Explorer, and then perform a rollback to the previous deployment from the Azure management portal.</span></span>

1. <span data-ttu-id="bda2a-447">Откройте новый **Git Bash** консоль для развертывания обновленного приложения в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="bda2a-447">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
2. <span data-ttu-id="bda2a-448">Выполните следующие команды для изменения в Azure.</span><span class="sxs-lookup"><span data-stu-id="bda2a-448">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="bda2a-449">Обновление *[путь ВАШЕГО приложения]* заполнитель с путем к **GeekQuiz** решения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-449">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="bda2a-450">Будет приглашение ввести пароль развертывания.</span><span class="sxs-lookup"><span data-stu-id="bda2a-450">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Передача рефакторингу кода на Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    <span data-ttu-id="bda2a-452">*Передача рефакторингу кода на Azure*</span><span class="sxs-lookup"><span data-stu-id="bda2a-452">*Pushing refactored code to Azure*</span></span>
3. <span data-ttu-id="bda2a-453">Откройте Internet Explorer и перейдите на веб-приложение (например `http://<your-web-site>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="bda2a-453">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="bda2a-454">Вход с помощью ранее созданного учетные данные.</span><span class="sxs-lookup"><span data-stu-id="bda2a-454">Log in using the previously created credentials.</span></span>
4. <span data-ttu-id="bda2a-455">Нажмите клавишу **F12** для запуска средства разработки, выберите **сети** и нажмите кнопку **воспроизведение** кнопку, чтобы начать запись.</span><span class="sxs-lookup"><span data-stu-id="bda2a-455">Press **F12** to launch the development tools, select the **Network** tab and click the **Play** button to start recording.</span></span>

    <span data-ttu-id="bda2a-456">![Начиная с сеть записи](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "начала записи в сети")</span><span class="sxs-lookup"><span data-stu-id="bda2a-456">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Starting network recording")</span></span>

    <span data-ttu-id="bda2a-457">*Запуск записи сети*</span><span class="sxs-lookup"><span data-stu-id="bda2a-457">*Starting network recording*</span></span>
5. <span data-ttu-id="bda2a-458">Выберите любой параметр головоломку.</span><span class="sxs-lookup"><span data-stu-id="bda2a-458">Select any option of the quiz.</span></span> <span data-ttu-id="bda2a-459">Вы увидите, что ничего не происходит.</span><span class="sxs-lookup"><span data-stu-id="bda2a-459">You will see that nothing happens.</span></span>
6. <span data-ttu-id="bda2a-460">В **F12** окно, элемент, соответствующий запрос POST HTTP показывает HTTP **500** результат.</span><span class="sxs-lookup"><span data-stu-id="bda2a-460">In the **F12** window, the entry corresponding to the POST HTTP request shows an HTTP **500** result.</span></span>

    ![HTTP 500 ошибок](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    <span data-ttu-id="bda2a-462">*HTTP 500 error*</span><span class="sxs-lookup"><span data-stu-id="bda2a-462">*HTTP 500 error*</span></span>
7. <span data-ttu-id="bda2a-463">Выберите **консоли** вкладки. В журнал подробные сведения о причину.</span><span class="sxs-lookup"><span data-stu-id="bda2a-463">Select the **Console** tab. An error is logged with the details of the cause.</span></span>

    ![Журнал ошибок](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    <span data-ttu-id="bda2a-465">*Журнал ошибок*</span><span class="sxs-lookup"><span data-stu-id="bda2a-465">*Logged error*</span></span>
8. <span data-ttu-id="bda2a-466">Найдите часть сведений об ошибке.</span><span class="sxs-lookup"><span data-stu-id="bda2a-466">Locate the details part of the error.</span></span> <span data-ttu-id="bda2a-467">Очевидно Эта ошибка вызвана кодом, рефакторинг вы были зафиксированы на предыдущих шагах.</span><span class="sxs-lookup"><span data-stu-id="bda2a-467">Clearly, this error is caused by the code refactoring you committed in the previous steps.</span></span>

    <span data-ttu-id="bda2a-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span><span class="sxs-lookup"><span data-stu-id="bda2a-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span></span>
9. <span data-ttu-id="bda2a-469">Не закрывайте браузер.</span><span class="sxs-lookup"><span data-stu-id="bda2a-469">Do not close the browser.</span></span>
10. <span data-ttu-id="bda2a-470">В новом экземпляре браузера, перейдите к [портала управления Azure](https://manage.windowsazure.com) и войдите с использованием учетной записи Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="bda2a-470">In a new browser instance, navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
11. <span data-ttu-id="bda2a-471">Выберите **веб-сайтов** и щелкните веб-приложения, созданного в упражнении 2.</span><span class="sxs-lookup"><span data-stu-id="bda2a-471">Select **Websites** and click the web app you created in Exercise 2.</span></span>
12. <span data-ttu-id="bda2a-472">Перейдите к **развертываний** страницы.</span><span class="sxs-lookup"><span data-stu-id="bda2a-472">Navigate to the **Deployments** page.</span></span> <span data-ttu-id="bda2a-473">Обратите внимание, что выполнены все фиксации, перечислены в журнал развертывания.</span><span class="sxs-lookup"><span data-stu-id="bda2a-473">Notice that all the commits performed are listed in the deployment history.</span></span>

    ![Список существующих развертываний](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    <span data-ttu-id="bda2a-475">*Список существующих развертываний*</span><span class="sxs-lookup"><span data-stu-id="bda2a-475">*List of existing deployments*</span></span>
13. <span data-ttu-id="bda2a-476">Выберите предыдущую фиксацию и нажмите кнопку **повторно развернуть** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="bda2a-476">Select the previous commit and click **Redeploy** on the command bar.</span></span>

    ![Повторное развертывание предыдущую фиксацию](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    <span data-ttu-id="bda2a-478">*Повторное развертывание предыдущую фиксацию*</span><span class="sxs-lookup"><span data-stu-id="bda2a-478">*Redeploying the previous commit*</span></span>
14. <span data-ttu-id="bda2a-479">При появлении запроса на подтверждение нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-479">When prompted to confirm, click **Yes**.</span></span>

    ![Подтверждение повторное развертывание](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. <span data-ttu-id="bda2a-481">После завершения развертывания переключитесь обратно на экземпляр обозревателя с веб-приложения и нажмите клавишу **сочетание клавиш CTRL + F5**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-481">When the deployment completes, switch back to the browser instance with your web app and press **CTRL + F5**.</span></span>
16. <span data-ttu-id="bda2a-482">Щелкните любой из параметров.</span><span class="sxs-lookup"><span data-stu-id="bda2a-482">Click any of the options.</span></span> <span data-ttu-id="bda2a-483">Переворачивающейся анимации теперь займет месте и результат (*успешных и неуспешных попыток*) будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="bda2a-483">The flip animation will now take place and the result (*correct/incorrect*) will be displayed.</span></span>
17. <span data-ttu-id="bda2a-484">(Необязательно) Переключитесь в **Git Bash** консоли и выполните следующие команды, чтобы вернуться к предыдущей операции фиксации.</span><span class="sxs-lookup"><span data-stu-id="bda2a-484">(Optional) Switch to the **Git Bash** console and execute the following commands to revert to the previous commit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-485">Эти команды создают новые фиксации, которая отменяет все изменения в репозиторий Git, внесенные в неверный фиксации.</span><span class="sxs-lookup"><span data-stu-id="bda2a-485">These commands create a new commit that undoes all changes in the Git repository that were made in the bad commit.</span></span> <span data-ttu-id="bda2a-486">Azure затем выполнить повторное развертывание приложения с помощью нового фиксации.</span><span class="sxs-lookup"><span data-stu-id="bda2a-486">Azure will then redeploy the application using the new commit.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a><span data-ttu-id="bda2a-487">Упражнение 4: Масштабирования с помощью хранилища Azure</span><span class="sxs-lookup"><span data-stu-id="bda2a-487">Exercise 4: Scaling Using Azure Storage</span></span>

<span data-ttu-id="bda2a-488">**Большие двоичные объекты** — это самый простой способ хранить большие объемы неструктурированных текстовых или двоичных данных, таких как видео, аудио и изображения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-488">**Blobs** are the simplest way to store large amounts of unstructured text or binary data such as video, audio and images.</span></span> <span data-ttu-id="bda2a-489">Перемещение к статическому содержимому приложения в хранилище, позволяет масштабировать приложения, выполняющих изображения или документы непосредственно в браузере.</span><span class="sxs-lookup"><span data-stu-id="bda2a-489">Moving the static content of your application to Storage, helps to scale your application by serving images or documents directly to the browser.</span></span>

<span data-ttu-id="bda2a-490">В этом упражнении будет переместить статическое содержимое приложения в контейнер больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="bda2a-490">In this exercise, you will move the static content of your application to a Blob container.</span></span> <span data-ttu-id="bda2a-491">Затем будет настроить приложение, чтобы добавить **правил переопределения URL-адресов ASP.NET** в **Web.config** для перенаправления содержимого в контейнере больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="bda2a-491">Then you will configure your application to add an **ASP.NET URL rewrite rule** in the **Web.config** to redirect your content to the Blob container.</span></span>

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a><span data-ttu-id="bda2a-492">Задача 1 – Создание учетной записи хранилища Azure</span><span class="sxs-lookup"><span data-stu-id="bda2a-492">Task 1 – Creating an Azure Storage Account</span></span>

<span data-ttu-id="bda2a-493">В этой задаче вы узнаете, как создать новую учетную запись хранилища с помощью портала управления.</span><span class="sxs-lookup"><span data-stu-id="bda2a-493">In this task you will learn how to create a new storage account using the management portal.</span></span>

1. <span data-ttu-id="bda2a-494">Перейдите к [портала управления Azure](https://manage.windowsazure.com) и войдите с использованием учетной записи Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="bda2a-494">Navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
2. <span data-ttu-id="bda2a-495">Выберите **новые | Службы данных | Хранилище | Быстрое создание** Чтобы приступить к созданию новой учетной записи хранения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-495">Select **New | Data Services | Storage | Quick Create** to start creating a new storage account.</span></span> <span data-ttu-id="bda2a-496">Введите уникальное имя для учетной записи и выберите **область** из списка.</span><span class="sxs-lookup"><span data-stu-id="bda2a-496">Enter a unique name for the account and select a **Region** from the list.</span></span> <span data-ttu-id="bda2a-497">Нажмите кнопку **создать учетную запись хранилища** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-497">Click **Create Storage Account** to continue.</span></span>

    <span data-ttu-id="bda2a-498">![Создание новой учетной записи хранения](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Создание новой учетной записи хранения")</span><span class="sxs-lookup"><span data-stu-id="bda2a-498">![Creating a new Storage Account](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Creating a new Storage Account")</span></span>

    <span data-ttu-id="bda2a-499">*Создание новой учетной записи хранения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-499">*Creating a new storage account*</span></span>
3. <span data-ttu-id="bda2a-500">В **хранения** статьи, подождите, пока не изменится состояние учетной записи хранения на *Online* чтобы продолжить на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="bda2a-500">In the **Storage** section, wait until the status of the new storage account changes to *Online* in order to continue with the following step.</span></span>

    <span data-ttu-id="bda2a-501">![Создается учетная запись хранения](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "создается учетная запись хранения")</span><span class="sxs-lookup"><span data-stu-id="bda2a-501">![Storage Account created](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Storage Account created")</span></span>

    <span data-ttu-id="bda2a-502">*Создается учетная запись хранения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-502">*Storage Account created*</span></span>
4. <span data-ttu-id="bda2a-503">Щелкните имя учетной записи хранилища, а затем нажмите кнопку **мониторинга** ссылку в верхней части страницы.</span><span class="sxs-lookup"><span data-stu-id="bda2a-503">Click on the storage account name and then click the **Dashboard** link at the top of the page.</span></span> <span data-ttu-id="bda2a-504">**Мониторинга** страница содержит сведения о состоянии учетной записи и конечные точки службы, которые могут использоваться в приложениях.</span><span class="sxs-lookup"><span data-stu-id="bda2a-504">The **Dashboard** page provides you with information about the status of the account and the service endpoints that can be used within your applications.</span></span>

    <span data-ttu-id="bda2a-505">![Отображение панели мониторинга учетной записи хранения](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "отображение панели мониторинга учетной записи хранения")</span><span class="sxs-lookup"><span data-stu-id="bda2a-505">![Displaying the Storage Account Dashboard](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Displaying the Storage Account Dashboard")</span></span>

    <span data-ttu-id="bda2a-506">*Отображение панели мониторинга учетной записи хранения*</span><span class="sxs-lookup"><span data-stu-id="bda2a-506">*Displaying the Storage Account Dashboard*</span></span>
5. <span data-ttu-id="bda2a-507">Нажмите кнопку **управление ключами доступа** кнопки на панели навигации.</span><span class="sxs-lookup"><span data-stu-id="bda2a-507">Click the **Manage Access Keys** button in the navigation bar.</span></span>

    <span data-ttu-id="bda2a-508">![Управление ключами доступа кнопка](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "кнопку Управление ключами доступа")</span><span class="sxs-lookup"><span data-stu-id="bda2a-508">![Manage Access Keys button](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Manage Access Keys button")</span></span>

    <span data-ttu-id="bda2a-509">*Управление кнопку клавиши доступа*</span><span class="sxs-lookup"><span data-stu-id="bda2a-509">*Manage Access Keys button*</span></span>
6. <span data-ttu-id="bda2a-510">В **управление ключами доступа** диалоговое окно, скопируйте **имя учетной записи хранения** и **первичный ключ доступа** как они понадобятся вам в следующем упражнении.</span><span class="sxs-lookup"><span data-stu-id="bda2a-510">In the **Manage Access Keys** dialog box, copy the **Storage Account Name** and **Primary Access Key** as you will need them in the following exercise.</span></span> <span data-ttu-id="bda2a-511">Закройте диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="bda2a-511">Then, close the dialog box.</span></span>

    <span data-ttu-id="bda2a-512">![Диалоговое окно Управление ключом доступа](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "диалоговое окно «Управление ключами доступа»")</span><span class="sxs-lookup"><span data-stu-id="bda2a-512">![Manage Access Key dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Manage Access Key dialog box")</span></span>

    <span data-ttu-id="bda2a-513">*Ключ доступа диалоговое окно Управление*</span><span class="sxs-lookup"><span data-stu-id="bda2a-513">*Manage Access Key dialog box*</span></span>

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a><span data-ttu-id="bda2a-514">Задача 2 — Отправка актива в хранилище BLOB-объектов Azure</span><span class="sxs-lookup"><span data-stu-id="bda2a-514">Task 2 – Uploading an Asset to Azure Blob Storage</span></span>

<span data-ttu-id="bda2a-515">В этой задаче будет использовать окно обозревателя серверов из Visual Studio для подключения к учетной записи хранилища.</span><span class="sxs-lookup"><span data-stu-id="bda2a-515">In this task, you will use the Server Explorer window from Visual Studio to connect to your storage account.</span></span> <span data-ttu-id="bda2a-516">Затем создайте контейнер больших двоичных объектов и отправить файл с эмблемой компьютерный фанат головоломка в контейнер.</span><span class="sxs-lookup"><span data-stu-id="bda2a-516">You will then create a blob container and upload a file with the Geek Quiz logo to the container.</span></span>

1. <span data-ttu-id="bda2a-517">Перейдите к экземпляру Visual Studio с **GeekQuiz** решения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="bda2a-517">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="bda2a-518">В строке меню выберите **представление** и нажмите кнопку **обозревателя серверов**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-518">From the menu bar, select **View** and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="bda2a-519">В **обозревателя серверов**, щелкните правой кнопкой мыши **Azure** , а затем выберите **подключение к Azure...** . Вход с использованием учетной записи Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="bda2a-519">In **Server Explorer**, right-click the **Azure** node and select **Connect to Azure...**. Sign in using the Microsoft account associated with your subscription.</span></span>

    ![Подключиться к Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    <span data-ttu-id="bda2a-521">*Подключиться к Azure*</span><span class="sxs-lookup"><span data-stu-id="bda2a-521">*Connect to Azure*</span></span>
4. <span data-ttu-id="bda2a-522">Разверните **Azure** узел, щелкните правой кнопкой мыши **хранения** и выберите **присоединить внешнее хранилище...** .</span><span class="sxs-lookup"><span data-stu-id="bda2a-522">Expand the **Azure** node, right-click **Storage** and select **Attach External Storage...**.</span></span>
5. <span data-ttu-id="bda2a-523">В **добавить новую учетную запись хранения** диалогового окна введите **имя учетной записи** и **ключ учетной записи** вы получили в предыдущей задаче и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-523">In the **Add New Storage Account** dialog box, enter the **Account name** and **Account key** you obtained in the previous task and click **OK**.</span></span>

    ![Добавление новой учетной записи хранения-диалоговое окно](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    <span data-ttu-id="bda2a-525">*Добавление новой учетной записи хранения-диалоговое окно*</span><span class="sxs-lookup"><span data-stu-id="bda2a-525">*Add New Storage Account dialog box*</span></span>
6. <span data-ttu-id="bda2a-526">Учетной записи хранилища будут отображаться под **хранения** узла.</span><span class="sxs-lookup"><span data-stu-id="bda2a-526">Your storage account should appear under the **Storage** node.</span></span> <span data-ttu-id="bda2a-527">Ваша учетная запись хранения, щелкните правой кнопкой **большие двоичные объекты** и выберите **создать контейнер больших двоичных объектов...** .</span><span class="sxs-lookup"><span data-stu-id="bda2a-527">Expand your storage account, right-click **Blobs** and select **Create Blob Container...**.</span></span>

    <span data-ttu-id="bda2a-528">![Создание контейнера больших двоичных объектов](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "создать контейнер больших двоичных объектов")</span><span class="sxs-lookup"><span data-stu-id="bda2a-528">![Create Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Create Blob Container")</span></span>

    <span data-ttu-id="bda2a-529">*Создание контейнера больших двоичных объектов*</span><span class="sxs-lookup"><span data-stu-id="bda2a-529">*Create Blob Container*</span></span>
7. <span data-ttu-id="bda2a-530">В **создать контейнер больших двоичных объектов** диалогового окна введите имя контейнера больших двоичных объектов и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-530">In the **Create Blob Container** dialog box, enter a name for the blob container and click **OK**.</span></span>

    <span data-ttu-id="bda2a-531">![Диалоговое окно Создание контейнера больших двоичных объектов](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "диалоговое окно «Создание контейнера больших двоичных объектов»")</span><span class="sxs-lookup"><span data-stu-id="bda2a-531">![Create Blob Container dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Create Blob Container dialog box")</span></span>

    <span data-ttu-id="bda2a-532">*Создать контейнер больших двоичных объектов-диалоговое окно*</span><span class="sxs-lookup"><span data-stu-id="bda2a-532">*Create Blob Container dialog box*</span></span>
8. <span data-ttu-id="bda2a-533">Новый контейнер больших двоичных объектов, которые должны быть добавлены в **большие двоичные объекты** узла.</span><span class="sxs-lookup"><span data-stu-id="bda2a-533">The new blob container should be added to the **Blobs** node.</span></span> <span data-ttu-id="bda2a-534">Изменение разрешений доступа в контейнере сделать контейнер общедоступным.</span><span class="sxs-lookup"><span data-stu-id="bda2a-534">Change the access permissions in the container to make the container public.</span></span> <span data-ttu-id="bda2a-535">Для этого щелкните правой кнопкой мыши **изображения** контейнер и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-535">To do this, right-click the **images** container and select **Properties**.</span></span>

    <span data-ttu-id="bda2a-536">![свойства контейнера изображений](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "свойства контейнера изображений")</span><span class="sxs-lookup"><span data-stu-id="bda2a-536">![images container properties](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "images container properties")</span></span>

    <span data-ttu-id="bda2a-537">*Свойства контейнера изображений*</span><span class="sxs-lookup"><span data-stu-id="bda2a-537">*Images container properties*</span></span>
9. <span data-ttu-id="bda2a-538">В **свойства** задайте **общего доступа на чтение** для **контейнер**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-538">In the **Properties** window, set the **Public Read Access** to **Container**.</span></span>

    <span data-ttu-id="bda2a-539">![Изменение свойства общего доступа на чтение](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "изменение свойства общего доступа на чтение")</span><span class="sxs-lookup"><span data-stu-id="bda2a-539">![Changing public read access property](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Changing public read access property")</span></span>

    <span data-ttu-id="bda2a-540">*Изменение свойства общего доступа на чтение*</span><span class="sxs-lookup"><span data-stu-id="bda2a-540">*Changing public read access property*</span></span>
10. <span data-ttu-id="bda2a-541">При появлении запроса, если вы уверены, необходимо изменить свойство общего доступа, нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-541">When prompted if you are sure you want to change the public access property, click **Yes**.</span></span>

    <span data-ttu-id="bda2a-542">![Microsoft Visual Studio предупреждение](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "предупреждение Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="bda2a-542">![Microsoft Visual Studio warning](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="bda2a-543">*Предупреждение Microsoft Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="bda2a-543">*Microsoft Visual Studio warning*</span></span>
11. <span data-ttu-id="bda2a-544">В **обозревателя серверов**, щелкните правой кнопкой мыши **изображения** контейнер больших двоичных объектов и выберите **контейнер больших двоичных объектов представления**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-544">In **Server Explorer**, right-click in the **images** blob container and select **View Blob Container**.</span></span>

    <span data-ttu-id="bda2a-545">![Просмотреть контейнер больших двоичных объектов](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "просмотреть контейнер больших двоичных объектов")</span><span class="sxs-lookup"><span data-stu-id="bda2a-545">![View Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "View Blob Container")</span></span>

    <span data-ttu-id="bda2a-546">*Контейнер больших двоичных объектов представления*</span><span class="sxs-lookup"><span data-stu-id="bda2a-546">*View Blob Container*</span></span>
12. <span data-ttu-id="bda2a-547">Контейнер образов следует открыть в новом окне, и без элементов условных обозначений должно быть показано.</span><span class="sxs-lookup"><span data-stu-id="bda2a-547">The images container should open in a new window and a legend with no entries should be shown.</span></span> <span data-ttu-id="bda2a-548">Нажмите кнопку **отправить** значок, чтобы загрузить файл в контейнер больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="bda2a-548">Click the **upload** icon to upload a file to the blob container.</span></span>

    <span data-ttu-id="bda2a-549">![Контейнер образов с записями](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "изображения контейнер без элементов")</span><span class="sxs-lookup"><span data-stu-id="bda2a-549">![Images container with no entries](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Images container with no entries")</span></span>

    <span data-ttu-id="bda2a-550">*Контейнер образов без элементов*</span><span class="sxs-lookup"><span data-stu-id="bda2a-550">*Images container with no entries*</span></span>
13. <span data-ttu-id="bda2a-551">В **отправить BLOB-объект** диалоговом окне перейдите к **активы** папку лаборатории.</span><span class="sxs-lookup"><span data-stu-id="bda2a-551">In the **Upload Blob** dialog box, navigate to the **Assets** folder of the lab.</span></span> <span data-ttu-id="bda2a-552">Выберите **логотип big.png** файла и нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-552">Select the **logo-big.png** file and click **Open**.</span></span>
14. <span data-ttu-id="bda2a-553">Дождитесь завершения передачи файла.</span><span class="sxs-lookup"><span data-stu-id="bda2a-553">Wait until the file is uploaded.</span></span> <span data-ttu-id="bda2a-554">По завершении отправки файла должен быть указан в контейнере изображения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-554">When the upload completes, the file should be listed in the images container.</span></span> <span data-ttu-id="bda2a-555">Щелкните правой кнопкой мыши запись файла и выберите **скопировать URL-адрес**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-555">Right-click the file entry and select **Copy URL**.</span></span>

    <span data-ttu-id="bda2a-556">![Скопируйте URL-адрес большого двоичного объекта](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "скопируйте URL-адрес файла большого двоичного объекта")</span><span class="sxs-lookup"><span data-stu-id="bda2a-556">![Copy blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copy blob file URL")</span></span>

    <span data-ttu-id="bda2a-557">*Скопируйте URL-адрес большого двоичного объекта*</span><span class="sxs-lookup"><span data-stu-id="bda2a-557">*Copy blob URL*</span></span>
15. <span data-ttu-id="bda2a-558">Откройте Internet Explorer и вставьте URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="bda2a-558">Open Internet Explorer and paste the URL.</span></span> <span data-ttu-id="bda2a-559">Следующие изображения должен отображаться в браузере.</span><span class="sxs-lookup"><span data-stu-id="bda2a-559">The following image should be shown in the browser.</span></span>

    <span data-ttu-id="bda2a-560">![Эмблема big.png изображения из хранилища больших двоичных объектов Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "big.png логотип изображения из хранилища")</span><span class="sxs-lookup"><span data-stu-id="bda2a-560">![logo-big.png image from Windows Blob Storage](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo-big.png image from storage")</span></span>

    <span data-ttu-id="bda2a-561">*Эмблема big.png изображения из хранилища больших двоичных объектов*</span><span class="sxs-lookup"><span data-stu-id="bda2a-561">*logo-big.png image from Azure Blob Storage*</span></span>

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a><span data-ttu-id="bda2a-562">Задача 3 – обновление решения для использования статического содержимого из хранилища BLOB-объектов Azure</span><span class="sxs-lookup"><span data-stu-id="bda2a-562">Task 3 – Updating the Solution to Consume Static Content from Azure Blob Storage</span></span>

<span data-ttu-id="bda2a-563">В этой задаче вы настроите **GeekQuiz** решение, чтобы использовать изображение переданы хранилища больших двоичных объектов (вместо образа, расположенного в веб-приложения), добавив правило подстановки URL-адреса ASP.NET в **web.config**файла.</span><span class="sxs-lookup"><span data-stu-id="bda2a-563">In this task, you will configure the **GeekQuiz** solution to consume the image uploaded to Azure Blob Storage (instead of the image located in the web app) by adding an ASP.NET URL rewrite rule in the **web.config** file.</span></span>

1. <span data-ttu-id="bda2a-564">В Visual Studio откройте **Web.config** файла внутри **GeekQuiz** проекта и найдите  **&lt;system.webServer&gt;**  элемента.</span><span class="sxs-lookup"><span data-stu-id="bda2a-564">In Visual Studio, open the **Web.config** file inside the **GeekQuiz** project and locate the **&lt;system.webServer&gt;** element.</span></span>
2. <span data-ttu-id="bda2a-565">Добавьте следующий код для добавления URL-адрес перепишите правило, обновление заполнитель с вашей учетной записи хранилища.</span><span class="sxs-lookup"><span data-stu-id="bda2a-565">Add the following code to add an URL rewrite rule, updating the placeholder with your storage account name.</span></span>

    <span data-ttu-id="bda2a-566">(Фрагмент - кода *UrlRewriteRule WebSitesInProduction - Ex4 -*)</span><span class="sxs-lookup"><span data-stu-id="bda2a-566">(Code Snippet - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span></span>

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > <span data-ttu-id="bda2a-567">Перезапись URL-адрес — это процесс перехвата входящего веб-запроса и перенаправляет их на другой ресурс.</span><span class="sxs-lookup"><span data-stu-id="bda2a-567">URL rewriting is the process of intercepting an incoming Web request and redirecting the request to a different resource.</span></span> <span data-ttu-id="bda2a-568">URL-адрес правила перезаписи подсистема переписывания при необходимости перенаправить запрос, и где они должны перенаправляться.</span><span class="sxs-lookup"><span data-stu-id="bda2a-568">The URL rewriting rules tells the rewriting engine when a request needs to be redirected, and where should they be redirected.</span></span> <span data-ttu-id="bda2a-569">Перезаписи правило состоит из двух строк: шаблон для поиска запрошенного URL-адреса (как правило, с помощью регулярных выражений), и строка для замены с шаблоном, если он найден.</span><span class="sxs-lookup"><span data-stu-id="bda2a-569">A rewriting rule is composed of two strings: the pattern to look for in the requested URL (usually, using regular expressions), and the string to replace the pattern with, if found.</span></span> <span data-ttu-id="bda2a-570">Дополнительные сведения см. в разделе [перезаписи URL-адресов в ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span><span class="sxs-lookup"><span data-stu-id="bda2a-570">For more information, see [URL Rewriting in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span></span>
3. <span data-ttu-id="bda2a-571">Нажмите клавишу **сочетание клавиш CTRL + S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="bda2a-571">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="bda2a-572">Откройте новый **Git Bash** консоль для развертывания обновленного приложения в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="bda2a-572">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
5. <span data-ttu-id="bda2a-573">Выполните следующие команды для изменения в Azure.</span><span class="sxs-lookup"><span data-stu-id="bda2a-573">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="bda2a-574">Обновление *[путь ВАШЕГО приложения]* заполнитель с путем к **GeekQuiz** решения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-574">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="bda2a-575">Будет приглашение ввести пароль развертывания.</span><span class="sxs-lookup"><span data-stu-id="bda2a-575">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Развертывание обновления для Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    <span data-ttu-id="bda2a-577">*Развертывание обновления для Azure*</span><span class="sxs-lookup"><span data-stu-id="bda2a-577">*Deploying update to Azure*</span></span>

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a><span data-ttu-id="bda2a-578">Задача 4 – Проверка</span><span class="sxs-lookup"><span data-stu-id="bda2a-578">Task 4 – Verification</span></span>

<span data-ttu-id="bda2a-579">В этой задаче будут использованы **Internet Explorer** для просмотра **головоломка компьютерный фанат** приложение и проверьте, что URL-адрес перепишите правило для образов работает и вы будете перенаправлены изображения, размещенные в **больших двоичных объектов Azure Хранилище**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-579">In this task you will use **Internet Explorer** to browse the **Geek Quiz** application and check that the URL rewrite rule for images works and you are redirected to the image hosted on **Azure Blob Storage**.</span></span>

1. <span data-ttu-id="bda2a-580">Откройте Internet Explorer и перейдите на веб-приложение (например `http://<your-web-site>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="bda2a-580">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="bda2a-581">Вход с помощью ранее созданного учетные данные.</span><span class="sxs-lookup"><span data-stu-id="bda2a-581">Log in using the previously created credentials.</span></span>

    <span data-ttu-id="bda2a-582">![Отображение компьютерный фанат тест веб-приложения с изображением](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "отображающий компьютерный фанат тест веб-приложение с изображением")</span><span class="sxs-lookup"><span data-stu-id="bda2a-582">![Showing the Geek Quiz web app with the image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Showing the Geek Quiz web app with the image")</span></span>

    <span data-ttu-id="bda2a-583">*Отображение компьютерный фанат тест веб-приложения с изображением*</span><span class="sxs-lookup"><span data-stu-id="bda2a-583">*Showing the Geek Quiz web app with the image*</span></span>
2. <span data-ttu-id="bda2a-584">Нажмите клавишу **F12** для запуска средства разработки, выберите **сети** вкладку и начать запись.</span><span class="sxs-lookup"><span data-stu-id="bda2a-584">Press **F12** to launch the development tools, select the **Network** tab and start recording.</span></span>

    <span data-ttu-id="bda2a-585">![Начиная с сеть записи](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "начала записи в сети")</span><span class="sxs-lookup"><span data-stu-id="bda2a-585">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Starting network recording")</span></span>

    <span data-ttu-id="bda2a-586">*Запуск записи сети*</span><span class="sxs-lookup"><span data-stu-id="bda2a-586">*Starting network recording*</span></span>
3. <span data-ttu-id="bda2a-587">Нажмите клавишу **сочетание клавиш CTRL + F5** для обновления веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="bda2a-587">Press **CTRL + F5** to refresh the web page.</span></span>
4. <span data-ttu-id="bda2a-588">После завершения загрузки страницы, вы увидите HTTP-запроса для **/img/logo-big.png** URL-адрес с HTTP **301** результат (перенаправление) и другим запросом `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL-адрес с HTTP **200** результат.</span><span class="sxs-lookup"><span data-stu-id="bda2a-588">Once the page has finished loading, you should see an HTTP request for the **/img/logo-big.png** URL with an HTTP **301** result (redirect) and another request for `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL with a HTTP **200** result.</span></span>

    <span data-ttu-id="bda2a-589">![Проверка URL-адрес перенаправления](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "отображение перенаправления в средствах разработки")</span><span class="sxs-lookup"><span data-stu-id="bda2a-589">![Verifying the URL redirect](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Showing the redirect in Dev Tools")</span></span>

    <span data-ttu-id="bda2a-590">*Проверка URL-адрес перенаправления*</span><span class="sxs-lookup"><span data-stu-id="bda2a-590">*Verifying the URL redirect*</span></span>

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a><span data-ttu-id="bda2a-591">Упражнение 5: С помощью автоматического масштабирования для веб-приложений</span><span class="sxs-lookup"><span data-stu-id="bda2a-591">Exercise 5: Using Autoscale for Web Apps</span></span>

> [!NOTE]
> <span data-ttu-id="bda2a-592">В этом упражнении необязателен, так как он требует поддержки веб-нагрузку &amp; тестирования производительности, который доступен только для **Visual Studio 2013 Ultimate Edition**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-592">This exercise is optional, since it requires support for Web Load &amp; Performance Testing which is only available for **Visual Studio 2013 Ultimate Edition**.</span></span> <span data-ttu-id="bda2a-593">Дополнительные сведения об определенных функциях Visual Studio 2013 сравнить версии [здесь](https://www.microsoft.com/visualstudio/eng/products/compare).</span><span class="sxs-lookup"><span data-stu-id="bda2a-593">For more information on specific Visual Studio 2013 features, compare versions [here](https://www.microsoft.com/visualstudio/eng/products/compare).</span></span>


<span data-ttu-id="bda2a-594">**Приложение службы веб-приложения Azure** предоставляет возможность автоматического масштабирования для веб-приложений, работающих в **стандартный режим**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-594">**Azure App Service Web Apps** provides the Autoscale feature for web apps running in **Standard Mode**.</span></span> <span data-ttu-id="bda2a-595">Автоматического масштабирования позволяет Azure автоматически масштабировала число экземпляров веб-приложения в зависимости от нагрузки.</span><span class="sxs-lookup"><span data-stu-id="bda2a-595">Autoscale lets Azure automatically scale the instance count of your web app depending on the load.</span></span> <span data-ttu-id="bda2a-596">Если Автомасштабирование включено, Azure проверяет нагрузку ЦП для веб-приложения каждые пять минут и добавляет экземпляры при необходимости на момент времени.</span><span class="sxs-lookup"><span data-stu-id="bda2a-596">When Autoscale is enabled, Azure checks the CPU of your web app once every five minutes and adds instances as needed at that point in time.</span></span> <span data-ttu-id="bda2a-597">В случае низкой ЦП Azure убирает экземпляры каждые 2 часа, чтобы убедиться, что не снижается производительность веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-597">If the CPU usage is low, Azure will remove instances once every two hours to ensure that the performance of your web app is not degraded.</span></span>

<span data-ttu-id="bda2a-598">В этом упражнении вы перейдете выполнить шаги, необходимые для настройки **автомасштабирования** возможностей **компьютерный фанат тест** веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-598">In this exercise you will go through the steps required to configure the **Autoscale** feature for the **Geek Quiz** web app.</span></span> <span data-ttu-id="bda2a-599">Этот компонент будет проверить, выполнив нагрузочного теста Visual Studio для создания достаточной нагрузки ЦП в приложение, чтобы запустить обновление экземпляра.</span><span class="sxs-lookup"><span data-stu-id="bda2a-599">You will verify this feature by running a Visual Studio load test to generate enough CPU load on the application to trigger an instance upgrade.</span></span>

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a><span data-ttu-id="bda2a-600">Задача 1 — Настройка автоматического масштабирования, в зависимости от метрики ЦП</span><span class="sxs-lookup"><span data-stu-id="bda2a-600">Task 1 – Configuring Autoscale Based on the CPU Metric</span></span>

<span data-ttu-id="bda2a-601">В этой задаче будет использовать портал управления Azure, чтобы включить функцию автоматического масштабирования для веб-приложения, созданный в упражнении 2.</span><span class="sxs-lookup"><span data-stu-id="bda2a-601">In this task you will use the Azure management portal to enable the Autoscale feature for the web app you created in Exercise 2.</span></span>

1. <span data-ttu-id="bda2a-602">В [портала управления Azure](https://manage.windowsazure.com/)выберите **веб-сайтов** и щелкните веб-приложения, созданного в упражнении 2.</span><span class="sxs-lookup"><span data-stu-id="bda2a-602">In the [Azure management portal](https://manage.windowsazure.com/), select **Websites** and click the web app you created in Exercise 2.</span></span>
2. <span data-ttu-id="bda2a-603">Перейдите к **шкалы** страницы.</span><span class="sxs-lookup"><span data-stu-id="bda2a-603">Navigate to the **Scale** page.</span></span> <span data-ttu-id="bda2a-604">В разделе **емкость** выберите **ЦП** для **масштабирование по метрике** конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bda2a-604">Under the **capacity** section, select **CPU** for the **Scale by Metric** configuration.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-605">При масштабировании по ЦП Azure динамически регулирует количество экземпляров, которые приложение использует при изменении ЦП.</span><span class="sxs-lookup"><span data-stu-id="bda2a-605">When scaling by CPU, Azure dynamically adjusts the number of instances that the app uses if the CPU usage changes.</span></span>

    <span data-ttu-id="bda2a-606">![При выборе масштабирование по ЦП](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "выбора метрики ЦП для автоматического масштабирования")</span><span class="sxs-lookup"><span data-stu-id="bda2a-606">![Selecting to scale by CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selecting the CPU metric for auto scaling")</span></span>

    <span data-ttu-id="bda2a-607">*При выборе масштабирование по ЦП*</span><span class="sxs-lookup"><span data-stu-id="bda2a-607">*Selecting to scale by CPU*</span></span>
3. <span data-ttu-id="bda2a-608">Изменение **целевой Процессор** конфигурации для **20**-**40** процентов.</span><span class="sxs-lookup"><span data-stu-id="bda2a-608">Change the **Target CPU** configuration to **20**-**40** percent.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-609">Этот диапазон представляет среднее использование ЦП для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-609">This range represents the average CPU usage for your web app.</span></span> <span data-ttu-id="bda2a-610">Azure будет добавлять или удалять экземпляры, чтобы удержать веб-приложения в этом диапазоне.</span><span class="sxs-lookup"><span data-stu-id="bda2a-610">Azure will add or remove instances to keep your web app in this range.</span></span> <span data-ttu-id="bda2a-611">Минимальное и максимальное число экземпляров, используемых для масштабирования указывается в **число экземпляров** конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bda2a-611">The minimum and maximum number of instances used for scaling is specified in the **Instance Count** configuration.</span></span> <span data-ttu-id="bda2a-612">Azure будет никогда не выйдут за установленные за рамками этого ограничения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-612">Azure will never go above or beyond that limit.</span></span>
    > 
    > <span data-ttu-id="bda2a-613">Значение по умолчанию **целевой Процессор** значения изменяются только в целях этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="bda2a-613">The default **Target CPU** values are modified just for the purposes of this lab.</span></span> <span data-ttu-id="bda2a-614">Настроив диапазон ЦП с небольшими значениями, вы увеличиваете вероятность для запуска автоматического масштабирования при помещении умеренной нагрузки на приложение.</span><span class="sxs-lookup"><span data-stu-id="bda2a-614">By configuring the CPU range with small values, you are increasing the chances to trigger Autoscale when a moderate load is placed on the application.</span></span>

    <span data-ttu-id="bda2a-615">![Изменение целевой ЦП, чтобы быть в диапазоне от 20 до 40 процентов](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Изменение целевой ЦП, чтобы быть в диапазоне от 20 до 40 процентов")</span><span class="sxs-lookup"><span data-stu-id="bda2a-615">![Changing the target CPU to be between 20 and 40 percent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Changing the target CPU to be between 20 and 40 percent")</span></span>

    <span data-ttu-id="bda2a-616">*Изменение целевой ЦП, чтобы быть в диапазоне от 20 до 40 процентов*</span><span class="sxs-lookup"><span data-stu-id="bda2a-616">*Changing the Target CPU to be between 20 and 40 percent*</span></span>
4. <span data-ttu-id="bda2a-617">Нажмите кнопку **Сохранить** на панели команд, чтобы сохранить изменения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-617">Click **Save** in the command bar to save the changes.</span></span>

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a><span data-ttu-id="bda2a-618">Задача 2 — нагрузочного тестирования с Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bda2a-618">Task 2 – Load Testing with Visual Studio</span></span>

<span data-ttu-id="bda2a-619">Теперь, когда **автомасштабирования** была настроена, вы создадите **веб-тестов производительности и загрузить тестовый проект** в Visual Studio для создания некоторых загрузку ЦП на веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-619">Now that **Autoscale** has been configured, you will create a **Web Performance and Load Test Project** in Visual Studio to generate some CPU load on your web app.</span></span>

1. <span data-ttu-id="bda2a-620">Откройте **Visual Studio Ultimate 2013** и выберите **файл | Новые | Проект...**  для запуска нового решения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-620">Open **Visual Studio Ultimate 2013** and select **File | New | Project...** to start a new solution.</span></span>

    <span data-ttu-id="bda2a-621">![Создание нового проекта](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Создание нового проекта")</span><span class="sxs-lookup"><span data-stu-id="bda2a-621">![Creating a new project](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Creating a new project")</span></span>

    <span data-ttu-id="bda2a-622">*Создание нового проекта*</span><span class="sxs-lookup"><span data-stu-id="bda2a-622">*Creating a new project*</span></span>
2. <span data-ttu-id="bda2a-623">В **новый проект** выберите **веб-тестов производительности и нагрузки тестовый проект** под **Visual C# | Тест** вкладки. Убедитесь, что **.NET Framework 4.5** — флажок установлен, назовите проект *WebAndLoadTestProject*, выберите **расположение** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-623">In the **New Project** dialog box, select **Web Performance and Load Test Project** under the **Visual C# | Test** tab. Make sure **.NET Framework 4.5** is selected, name the project *WebAndLoadTestProject*, choose a **Location** and click **OK**.</span></span>

    <span data-ttu-id="bda2a-624">![Создание нового проекта веб- и нагрузочного теста](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "создания нагрузочных тестов и веб-проекта")</span><span class="sxs-lookup"><span data-stu-id="bda2a-624">![Creating a new Web and Load Test project](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Creating a new Web and Load Test project")</span></span>

    <span data-ttu-id="bda2a-625">*Создание нового проекта веб- и нагрузочного теста*</span><span class="sxs-lookup"><span data-stu-id="bda2a-625">*Creating a new Web and Load Test project*</span></span>
3. <span data-ttu-id="bda2a-626">В **WebTest1.webtest** щелкните правой кнопкой мыши **WebTest1** и выберите команду **добавить запрос**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-626">In the **WebTest1.webtest** Right-click the **WebTest1** node and click **Add Request**.</span></span>

    <span data-ttu-id="bda2a-627">![Запрос на добавление WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "запрос на добавление WebTest1")</span><span class="sxs-lookup"><span data-stu-id="bda2a-627">![Adding a request to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Adding a request to WebTest1")</span></span>

    <span data-ttu-id="bda2a-628">*Запрос на добавление WebTest1*</span><span class="sxs-lookup"><span data-stu-id="bda2a-628">*Adding a request to WebTest1*</span></span>
4. <span data-ttu-id="bda2a-629">В **свойства** окно нового узла запроса обновления **URL-адрес** свойство, чтобы указать URL-адрес веб-приложения (например  *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)* ).</span><span class="sxs-lookup"><span data-stu-id="bda2a-629">In the **Properties** window of the new request node, update the **Url** property to point to the URL of your web app (e.g. *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span></span>

    <span data-ttu-id="bda2a-630">![Изменение свойства URL-адрес](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "изменения свойства в URL-адрес")</span><span class="sxs-lookup"><span data-stu-id="bda2a-630">![Changing the Url property](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Changing the Url property")</span></span>

    <span data-ttu-id="bda2a-631">*Изменение свойства URL-адрес*</span><span class="sxs-lookup"><span data-stu-id="bda2a-631">*Changing the Url property*</span></span>
5. <span data-ttu-id="bda2a-632">В **WebTest1.webtest** окно, щелкните правой кнопкой мыши **WebTest1** и нажмите кнопку **добавьте цикл...** .</span><span class="sxs-lookup"><span data-stu-id="bda2a-632">In the **WebTest1.webtest** window, right-click **WebTest1** and click **Add Loop...**.</span></span>

    <span data-ttu-id="bda2a-633">![Добавление цикла WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Добавление цикла WebTest1")</span><span class="sxs-lookup"><span data-stu-id="bda2a-633">![Adding a loop to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Adding a loop to WebTest1")</span></span>

    <span data-ttu-id="bda2a-634">*Добавление цикла WebTest1*</span><span class="sxs-lookup"><span data-stu-id="bda2a-634">*Adding a loop to WebTest1*</span></span>
6. <span data-ttu-id="bda2a-635">В **Добавьте условное правило и элементы в цикл** выберите **цикл For** правила и измените следующие свойства.</span><span class="sxs-lookup"><span data-stu-id="bda2a-635">In the **Add Conditional Rule and Items to Loop** dialog box, select the **For Loop** rule and modify the following properties.</span></span>

    1. <span data-ttu-id="bda2a-636">**Дифференциальное значение:** 1000</span><span class="sxs-lookup"><span data-stu-id="bda2a-636">**Terminating value:** 1000</span></span>
    2. <span data-ttu-id="bda2a-637">**Имя параметра контекста:** итератора</span><span class="sxs-lookup"><span data-stu-id="bda2a-637">**Context Parameter Name:** Iterator</span></span>
    3. <span data-ttu-id="bda2a-638">**Значение приращения:** 1</span><span class="sxs-lookup"><span data-stu-id="bda2a-638">**Increment Value:** 1</span></span>

    <span data-ttu-id="bda2a-639">![Выберите правило для цикла и обновление свойств](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "выберите правило для цикла и обновление свойств")</span><span class="sxs-lookup"><span data-stu-id="bda2a-639">![Selecting the For Loop rule and updating the properties](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Selecting the For Loop rule and updating the properties")</span></span>

    <span data-ttu-id="bda2a-640">*Выберите правило для цикла и обновление свойств*</span><span class="sxs-lookup"><span data-stu-id="bda2a-640">*Selecting the For Loop rule and updating the properties*</span></span>
7. <span data-ttu-id="bda2a-641">В разделе **элементы в цикле** выберите запрос, созданный ранее первого и последнего элемента для цикла.</span><span class="sxs-lookup"><span data-stu-id="bda2a-641">Under the **Items in loop** section, select the request you created previously to be the first and last item for the loop.</span></span> <span data-ttu-id="bda2a-642">Нажмите кнопку **ОК** , чтобы продолжить.</span><span class="sxs-lookup"><span data-stu-id="bda2a-642">Click **OK** to continue.</span></span>

    <span data-ttu-id="bda2a-643">![При выборе первый и последний элемент цикла](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "при выборе первый и последний элемент цикла")</span><span class="sxs-lookup"><span data-stu-id="bda2a-643">![Selecting the first and last items for the loop](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Selecting the first and last items for the loop")</span></span>

    <span data-ttu-id="bda2a-644">*При выборе первый и последний элемент цикла*</span><span class="sxs-lookup"><span data-stu-id="bda2a-644">*Selecting the first and last items for the loop*</span></span>
8. <span data-ttu-id="bda2a-645">В **обозревателе решений**, щелкните правой кнопкой мыши **WebAndLoadTestProject** проекта, разверните **добавить** и выбрать пункт **нагрузочного теста...** .</span><span class="sxs-lookup"><span data-stu-id="bda2a-645">In **Solution Explorer**, right-click the **WebAndLoadTestProject** project, expand the **Add** menu and select **Load Test...**.</span></span>

    <span data-ttu-id="bda2a-646">![Добавление нагрузочного теста в проект WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Добавление в проект WebAndLoadTestProject нагрузочного теста")</span><span class="sxs-lookup"><span data-stu-id="bda2a-646">![Adding a Load Test to the WebAndLoadTestProject project](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Adding a Load Test to the WebAndLoadTestProject project")</span></span>

    <span data-ttu-id="bda2a-647">*Добавление в проект WebAndLoadTestProject нагрузочного теста*</span><span class="sxs-lookup"><span data-stu-id="bda2a-647">*Adding a Load Test to the WebAndLoadTestProject project*</span></span>
9. <span data-ttu-id="bda2a-648">В **мастера тестовой нагрузки** диалоговое окно, нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-648">In the **New Load Test Wizard** dialog box, click **Next**.</span></span>

    <span data-ttu-id="bda2a-649">![Мастер тестовой нагрузки](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "мастера тестовой нагрузки")</span><span class="sxs-lookup"><span data-stu-id="bda2a-649">![New Load Test Wizard](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "New Load Test Wizard")</span></span>

    <span data-ttu-id="bda2a-650">*Мастер тестовой нагрузки*</span><span class="sxs-lookup"><span data-stu-id="bda2a-650">*New Load Test Wizard*</span></span>
10. <span data-ttu-id="bda2a-651">В **сценарий** выберите **не использовать время на обдумывание** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-651">In the **Scenario** page, select **Do not use think times** and click **Next**.</span></span>

    <span data-ttu-id="bda2a-652">![При выборе отказаться от использования времени на обдумывание](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "при выборе отказаться от использования времени на обдумывание")</span><span class="sxs-lookup"><span data-stu-id="bda2a-652">![Selecting not to use think times](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selecting not to use think times")</span></span>

    <span data-ttu-id="bda2a-653">*При выборе отказаться от использования времени на обдумывание*</span><span class="sxs-lookup"><span data-stu-id="bda2a-653">*Selecting not to use think times*</span></span>
11. <span data-ttu-id="bda2a-654">В **шаблона нагрузки** убедитесь, что **постоянной нагрузки** выбран параметр.</span><span class="sxs-lookup"><span data-stu-id="bda2a-654">In the **Load Pattern** page, make sure that the **Constant Load** option is selected.</span></span> <span data-ttu-id="bda2a-655">Изменение **число пользователей** параметру **250** пользователей и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-655">Change the **User Count** setting to **250** users and click **Next**.</span></span>

    <span data-ttu-id="bda2a-656">![Изменение числа пользователей на 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "изменение числа пользователей на 250")</span><span class="sxs-lookup"><span data-stu-id="bda2a-656">![Changing the user count to 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Changing the user count to 250")</span></span>

    <span data-ttu-id="bda2a-657">*Изменение числа пользователей на 250*</span><span class="sxs-lookup"><span data-stu-id="bda2a-657">*Changing the user count to 250*</span></span>
12. <span data-ttu-id="bda2a-658">В **модель тестового набора** выберите **на основе порядка теста для последовательных** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-658">In the **Test Mix Model** page, select **Based on sequential test order** and click **Next**.</span></span>

    <span data-ttu-id="bda2a-659">![Выбор модели тестового набора](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Выбор модели тестового набора")</span><span class="sxs-lookup"><span data-stu-id="bda2a-659">![Selecting the test mix model](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Selecting the test mix model")</span></span>

    <span data-ttu-id="bda2a-660">*Выбор модели тестового набора*</span><span class="sxs-lookup"><span data-stu-id="bda2a-660">*Selecting the test mix model*</span></span>
13. <span data-ttu-id="bda2a-661">В **модель тестового набора** щелкните **добавить...**  для добавления в набор тестов.</span><span class="sxs-lookup"><span data-stu-id="bda2a-661">In the **Test Mix Model** page, click **Add...** to add a test to the mix.</span></span>

    <span data-ttu-id="bda2a-662">![Добавление теста в тестовый набор](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Добавление теста в тестовый набор")</span><span class="sxs-lookup"><span data-stu-id="bda2a-662">![Adding a test to the test mix](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Adding a test to the test mix")</span></span>

    <span data-ttu-id="bda2a-663">*Добавление теста в тестовый набор*</span><span class="sxs-lookup"><span data-stu-id="bda2a-663">*Adding a test to the test mix*</span></span>
14. <span data-ttu-id="bda2a-664">В **добавить тесты** диалоговое окно, дважды щелкните **WebTest1** Чтобы добавить тест в **выбранные тесты** списка.</span><span class="sxs-lookup"><span data-stu-id="bda2a-664">In the **Add Tests** dialog box, double-click **WebTest1** to add the test to the **Selected tests** list.</span></span> <span data-ttu-id="bda2a-665">Нажмите кнопку **ОК** , чтобы продолжить.</span><span class="sxs-lookup"><span data-stu-id="bda2a-665">Click **OK** to continue.</span></span>

    <span data-ttu-id="bda2a-666">![Добавление теста WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "добавления WebTest1 теста")</span><span class="sxs-lookup"><span data-stu-id="bda2a-666">![Adding the WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Adding the WebTest1 test")</span></span>

    <span data-ttu-id="bda2a-667">*Добавление теста WebTest1*</span><span class="sxs-lookup"><span data-stu-id="bda2a-667">*Adding the WebTest1 test*</span></span>
15. <span data-ttu-id="bda2a-668">В **тестового набора** щелкните **Далее**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-668">Back in the **Test Mix** page, click **Next**.</span></span>

    <span data-ttu-id="bda2a-669">![Завершение работы страница тестового набора](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "завершение страница тестового набора")</span><span class="sxs-lookup"><span data-stu-id="bda2a-669">![Completing the Test Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Completing the Test Mix page")</span></span>

    <span data-ttu-id="bda2a-670">*Завершение работы страница тестового набора*</span><span class="sxs-lookup"><span data-stu-id="bda2a-670">*Completing the Test Mix page*</span></span>
16. <span data-ttu-id="bda2a-671">В **смешанного сетевого профиля** щелкните **Далее**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-671">In the **Network Mix** page, click **Next**.</span></span>

    <span data-ttu-id="bda2a-672">![Нажав кнопку Далее на странице смешанного сетевого профиля](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "нажатии кнопки рядом на странице смешанный сетевой профиль")</span><span class="sxs-lookup"><span data-stu-id="bda2a-672">![Clicking next in the Network Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Clicking next in the Network Mix page")</span></span>

    <span data-ttu-id="bda2a-673">*Нажмите кнопку Далее на странице смешанный сетевой профиль*</span><span class="sxs-lookup"><span data-stu-id="bda2a-673">*Clicking next in the Network Mix page*</span></span>
17. <span data-ttu-id="bda2a-674">В **набора браузеров** выберите **Internet Explorer 10.0** тип браузера и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-674">In the **Browser Mix** page, select **Internet Explorer 10.0** as the browser type and click **Next**.</span></span>

    <span data-ttu-id="bda2a-675">![При выборе типа обозревателя](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "при выборе типа браузера")</span><span class="sxs-lookup"><span data-stu-id="bda2a-675">![Selecting the browser type](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Selecting the browser type")</span></span>

    <span data-ttu-id="bda2a-676">*Выбрав тип браузера*</span><span class="sxs-lookup"><span data-stu-id="bda2a-676">*Selecting the browser type*</span></span>
18. <span data-ttu-id="bda2a-677">В **наборы счетчиков** щелкните **Далее**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-677">In the **Counter Sets** page, click **Next**.</span></span>

    <span data-ttu-id="bda2a-678">![Нажав кнопку "Далее" на странице наборы счетчиков](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "нажмите кнопку "Далее", на странице наборы счетчиков")</span><span class="sxs-lookup"><span data-stu-id="bda2a-678">![Clicking Next in the Counter Sets page](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Clicking Next in the Counter Sets page")</span></span>

    <span data-ttu-id="bda2a-679">*Нажав кнопку "Далее" на странице наборы счетчиков*</span><span class="sxs-lookup"><span data-stu-id="bda2a-679">*Clicking Next in the Counter Sets page*</span></span>
19. <span data-ttu-id="bda2a-680">В **параметры запуска** задайте **длительность нагрузочного теста** для **5 минут** и нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-680">In the **Run Settings** page, set the **Load test duration** to **5 minutes** and click **Finish**.</span></span>

    <span data-ttu-id="bda2a-681">![Длительность нагрузочного теста установите значение 5 минут](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "параметр длительность нагрузочного теста в 5 минут")</span><span class="sxs-lookup"><span data-stu-id="bda2a-681">![Setting the load test duration to 5 minutes](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Setting the load test duration to 5 minutes")</span></span>

    <span data-ttu-id="bda2a-682">*Длительность нагрузочного теста установите значение 5 минут*</span><span class="sxs-lookup"><span data-stu-id="bda2a-682">*Setting the load test duration to 5 minutes*</span></span>
20. <span data-ttu-id="bda2a-683">В **обозревателе решений**, дважды щелкните **Local.settings** файл для просмотра параметров тестирования.</span><span class="sxs-lookup"><span data-stu-id="bda2a-683">In **Solution Explorer**, double-click the **Local.settings** file to explore the test settings.</span></span> <span data-ttu-id="bda2a-684">По умолчанию Visual Studio использует локального компьютера для выполнения тестов.</span><span class="sxs-lookup"><span data-stu-id="bda2a-684">By default, Visual Studio uses your local computer to run the tests.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-685">Кроме того, можно настроить тестовый проект для выполнения нагрузочных тестов в облаке, используя **Visual Studio Online (VSO)**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-685">Alternatively, you can configure your test project to run the load tests in the cloud using **Visual Studio Online (VSO)**.</span></span> <span data-ttu-id="bda2a-686">VSO предоставляет Облачное нагрузочное тестирование службы, которая имитирует нагрузки более реалистичным, как избежать ограничений локальной среде, как ЦП, объем доступной памяти и пропускной способности сети.</span><span class="sxs-lookup"><span data-stu-id="bda2a-686">VSO provides a cloud-based load testing service that simulates a more realistic load, avoiding local environment constraints like CPU capacity, available memory and network bandwidth.</span></span> <span data-ttu-id="bda2a-687">Дополнительные сведения об использовании VSO для запуска нагрузочных тестов см. в разделе [в этой статье](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span><span class="sxs-lookup"><span data-stu-id="bda2a-687">For more information about using VSO to run load tests, see [this article](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span></span>

    ![Параметры тестирования](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a><span data-ttu-id="bda2a-689">Задача 3 – Проверка автоматического масштабирования</span><span class="sxs-lookup"><span data-stu-id="bda2a-689">Task 3 – Autoscale Verification</span></span>

<span data-ttu-id="bda2a-690">Необходимо выполнить нагрузочный тест, созданный в предыдущей задаче и увидеть, как ведет себя веб-приложения под нагрузкой.</span><span class="sxs-lookup"><span data-stu-id="bda2a-690">You will now execute the load test you created in the previous task and see how your web app behaves under load.</span></span>

1. <span data-ttu-id="bda2a-691">В **обозревателе решений**, дважды щелкните **LoadTest1.loadtest** Открытие нагрузочного теста.</span><span class="sxs-lookup"><span data-stu-id="bda2a-691">In **Solution Explorer**, double-click **LoadTest1.loadtest** to open the load test.</span></span>

    <span data-ttu-id="bda2a-692">![Открытие LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Открытие LoadTest1.loadtest")</span><span class="sxs-lookup"><span data-stu-id="bda2a-692">![Opening LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Opening LoadTest1.loadtest")</span></span>

    <span data-ttu-id="bda2a-693">*Открытие LoadTest1.loadtest*</span><span class="sxs-lookup"><span data-stu-id="bda2a-693">*Opening LoadTest1.loadtest*</span></span>
2. <span data-ttu-id="bda2a-694">В **LoadTest1.loadtest** окно, нажмите первую кнопку на панели элементов для выполнения нагрузочного теста.</span><span class="sxs-lookup"><span data-stu-id="bda2a-694">In the **LoadTest1.loadtest** window, click the first button in the toolbox to run the load test.</span></span>

    <span data-ttu-id="bda2a-695">![Запуск нагрузочного теста](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "нагрузочных тестов")</span><span class="sxs-lookup"><span data-stu-id="bda2a-695">![Running the load test](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Running the load test")</span></span>

    <span data-ttu-id="bda2a-696">*Запуск нагрузочного теста*</span><span class="sxs-lookup"><span data-stu-id="bda2a-696">*Running the load test*</span></span>
3. <span data-ttu-id="bda2a-697">Дождитесь завершения нагрузочного теста.</span><span class="sxs-lookup"><span data-stu-id="bda2a-697">Wait until the load test completes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-698">Нагрузочный тест моделируется нескольких пользователей, которые одновременно отправлять запросы к веб-приложению.</span><span class="sxs-lookup"><span data-stu-id="bda2a-698">The load test simulates multiple users that send requests to the web app simultaneously.</span></span> <span data-ttu-id="bda2a-699">При выполнении теста, вы можете отслеживать доступные счетчики для обнаружения ошибок, предупреждений или другие сведения, относящиеся к запусков нагрузочных тестов.</span><span class="sxs-lookup"><span data-stu-id="bda2a-699">When the test is running, you can monitor the available counters to detect any errors, warnings or other information related to your load test run.</span></span>

    <span data-ttu-id="bda2a-700">![Загрузить тест будет выполняться](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "дождаться завершения нагрузочного теста")</span><span class="sxs-lookup"><span data-stu-id="bda2a-700">![Load test running](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Waiting until the load test completes")</span></span>

    <span data-ttu-id="bda2a-701">*Нагрузочный тест будет выполняться*</span><span class="sxs-lookup"><span data-stu-id="bda2a-701">*Load test running*</span></span>
4. <span data-ttu-id="bda2a-702">После завершения теста, вернитесь на портал управления и перейдите к **шкалы** страницы веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bda2a-702">Once the test completes, go back to the management portal and navigate to the **Scale** page of your web app.</span></span> <span data-ttu-id="bda2a-703">В разделе **емкость** раздел, вы увидите на графике новый экземпляр был развернут автоматически.</span><span class="sxs-lookup"><span data-stu-id="bda2a-703">Under the **capacity** section, you should see in the graph that a new instance was automatically deployed.</span></span>

    ![Автоматическое развертывание нового экземпляра](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    <span data-ttu-id="bda2a-705">*Автоматическое развертывание нового экземпляра*</span><span class="sxs-lookup"><span data-stu-id="bda2a-705">*New instance automatically deployed*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bda2a-706">Он может занять несколько минут для изменения отображаются на диаграмме (нажмите клавишу **сочетание клавиш CTRL + F5** периодически, чтобы обновить страницу).</span><span class="sxs-lookup"><span data-stu-id="bda2a-706">It may take several minutes for the changes to appear in the graph (press **CTRL + F5** periodically to refresh the page).</span></span> <span data-ttu-id="bda2a-707">Если вы не видите все изменения, то можно выполнить следующие действия:</span><span class="sxs-lookup"><span data-stu-id="bda2a-707">If you do not see any changes, you can try the following:</span></span>
    > 
    > - <span data-ttu-id="bda2a-708">Увеличьте продолжительность нагрузочного теста (например, чтобы **10 минут**)</span><span class="sxs-lookup"><span data-stu-id="bda2a-708">Increase the duration of the load test (e.g. to **10 minutes**)</span></span>
    > - <span data-ttu-id="bda2a-709">Уменьшить максимальное и минимальное значения **целевой Процессор** диапазона в настройке автомасштабирования веб-приложения</span><span class="sxs-lookup"><span data-stu-id="bda2a-709">Reduce the maximum and minimum values of the **Target CPU** range in the Autoscale configuration of your web app</span></span>
    > - <span data-ttu-id="bda2a-710">Запуск нагрузочного теста в облаке **Visual Studio Online**.</span><span class="sxs-lookup"><span data-stu-id="bda2a-710">Run the load test in the cloud with **Visual Studio Online**.</span></span> <span data-ttu-id="bda2a-711">Дополнительные сведения о [здесь](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span><span class="sxs-lookup"><span data-stu-id="bda2a-711">More information [here](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="bda2a-712">Сводка</span><span class="sxs-lookup"><span data-stu-id="bda2a-712">Summary</span></span>

<span data-ttu-id="bda2a-713">В этой практической вы узнали, как настроить и развернуть приложение в рабочей среде веб-приложений в Azure.</span><span class="sxs-lookup"><span data-stu-id="bda2a-713">In this hands-on lab, you learned how to set up and deploy your application to production web apps in Azure.</span></span> <span data-ttu-id="bda2a-714">Запущен путем обнаружения и обновления баз данных, используя **Entity Framework Code First Migrations**, затем продолжает, развернув узел с помощью новых версий **Git** и выполнения отката для Последняя стабильная версия веб-узла.</span><span class="sxs-lookup"><span data-stu-id="bda2a-714">You started by detecting and updating your databases using **Entity Framework Code First Migrations**, then continued by deploying new versions of your site using **Git** and performing rollbacks to the latest stable version of your site.</span></span> <span data-ttu-id="bda2a-715">Кроме того вы узнали, как масштабировать приложение с помощью хранилища для перемещения статического содержимого контейнера BLOB-объекта.</span><span class="sxs-lookup"><span data-stu-id="bda2a-715">Additionally, you learned how to scale your app using Storage to move your static content to a Blob container.</span></span>
