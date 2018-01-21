---
title: "Приступая к работе с ASP.NET Core и Entity Framework 6"
author: tdykstra
description: "В этой статье показано, как использовать Entity Framework 6 в приложении ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 51445b8c110ad618aeb680148ccf4304a45ee16e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="b1e5e-103">Приступая к работе с ASP.NET Core и Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b1e5e-103">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="b1e5e-104">По [Paweł Grudzień](https://github.com/pgrudzien12), [Pontifex Дэмьен](https://github.com/DamienPontifex), и [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b1e5e-104">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="b1e5e-105">В этой статье показано, как использовать Entity Framework 6 в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-105">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="b1e5e-106">Обзор</span><span class="sxs-lookup"><span data-stu-id="b1e5e-106">Overview</span></span>

<span data-ttu-id="b1e5e-107">Для использования платформы Entity Framework 6, проекта должно выполнять компиляцию для .NET Framework, как Entity Framework 6 не поддерживает .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-107">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="b1e5e-108">Если вам требуются такие функции кросс платформенных необходимо будет обновить до [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="b1e5e-108">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="b1e5e-109">Для использования платформы Entity Framework 6 в приложении ASP.NET Core рекомендуется поместить EF6 контекста и классы моделей в библиотеке классов проекта, предназначенного полной платформы.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-109">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="b1e5e-110">Добавьте ссылку на библиотеку классов из проекта ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-110">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="b1e5e-111">См. в образце [решения Visual Studio с проектами EF6 и ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="b1e5e-111">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="b1e5e-112">Контекст EF6 нельзя поместить в проекте ASP.NET Core, потому что .NET Core проектов не поддерживают все функциональные возможности, EF6 команды, такие как *Enable-Migrations* требуется.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-112">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="b1e5e-113">Независимо от типа проекта найдите контекстом EF6 средства EF6 командной строки работают с EF6 контекстом.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-113">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="b1e5e-114">Например `Scaffold-DbContext` доступна только в Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-114">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="b1e5e-115">Если необходимо выполнить реконструирование базы данных в модель EF6, см. раздел [Code First для существующей базы данных](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="b1e5e-115">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="b1e5e-116">Справочник по полной версии .NET framework и EF6 в проекте ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b1e5e-116">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="b1e5e-117">Проект ASP.NET Core должен ссылаться на .NET framework и EF6.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-117">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="b1e5e-118">Например *.csproj* файл проекта ASP.NET Core будет выглядеть примерно следующим образом (показаны только значимые части файла).</span><span class="sxs-lookup"><span data-stu-id="b1e5e-118">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="b1e5e-119">При создании нового проекта используйте **веб-приложения ASP.NET Core (.NET Framework)** шаблона.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-119">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="b1e5e-120">Дескриптор строки подключения</span><span class="sxs-lookup"><span data-stu-id="b1e5e-120">Handle connection strings</span></span>

<span data-ttu-id="b1e5e-121">EF6 средства командной строки, которые будут использоваться в проекте библиотеки классов EF6 требуется конструктор по умолчанию, поэтому можно создать экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-121">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="b1e5e-122">Но, возможно, потребуется указать строку подключения для использования в проекте ASP.NET Core, в этом случае в контексте конструктор должен иметь параметр, который позволяет передавать в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-122">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="b1e5e-123">Ниже приведен пример.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-123">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="b1e5e-124">Поскольку EF6 контекст не имеет конструктора без параметров, EF6 проект должен предоставить реализацию [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="b1e5e-124">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="b1e5e-125">Средства командной строки EF6 находит и использовать эту реализацию, поэтому можно создать экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-125">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="b1e5e-126">Ниже приведен пример.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-126">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="b1e5e-127">В этом образце кода `IDbContextFactory` реализация передает в строку подключения, жестко.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-127">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="b1e5e-128">Это строка подключения, который будет использовать средства командной строки.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-128">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="b1e5e-129">Может потребоваться реализовать стратегию, чтобы гарантировать, что библиотека классов используется та же строка подключения, которая использует вызывающему приложению.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-129">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="b1e5e-130">Например может получить значение из переменной среды в обоих проектах.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-130">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="b1e5e-131">Настройка внедрение зависимостей в проекте ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b1e5e-131">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="b1e5e-132">В проекте Core *файла Startup.cs* файл, настроить контекст EF6 для внедрения зависимости (DI) в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-132">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="b1e5e-133">Объекты контекста EF следует задавать для времени жизни для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-133">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="b1e5e-134">Затем можно получить экземпляр контекста в контроллеры с помощью DI.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-134">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="b1e5e-135">Код похожа на то, что можно написать для контекста EF:</span><span class="sxs-lookup"><span data-stu-id="b1e5e-135">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="b1e5e-136">Образец приложения</span><span class="sxs-lookup"><span data-stu-id="b1e5e-136">Sample application</span></span>

<span data-ttu-id="b1e5e-137">Рабочий пример приложения см. в разделе [образец решения Visual Studio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) , прилагаемый к данной статье.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-137">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="b1e5e-138">В этом примере можно создать с нуля в Visual Studio выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="b1e5e-138">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="b1e5e-139">Создайте решение.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-139">Create a solution.</span></span>

* <span data-ttu-id="b1e5e-140">**Добавление нового проекта > Web > веб-приложения ASP.NET Core (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="b1e5e-140">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="b1e5e-141">**Добавить новый проект > Windows классического > Библиотека (.NET Framework) классов**</span><span class="sxs-lookup"><span data-stu-id="b1e5e-141">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="b1e5e-142">В **консоль диспетчера пакетов** (PMC) для обоих проектов, выполните команду `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-142">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="b1e5e-143">В проекте библиотеки классов, создать классы модели данных, класс контекста и реализация `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-143">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="b1e5e-144">В PMC проект библиотеки классов, выполните команды `Enable-Migrations` и `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-144">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="b1e5e-145">Если проект ASP.NET Core установлен как автозагружаемый проект, добавить `-StartupProjectName EF6` для этих команд.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-145">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="b1e5e-146">В проекте Core добавьте ссылку на проект в проект библиотеки классов.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-146">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="b1e5e-147">В проекте ядра в *файла Startup.cs*, Регистрация контекста для DI.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-147">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="b1e5e-148">В проекте ядра в *appsettings.json*, добавьте строку подключения.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-148">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="b1e5e-149">Добавьте в проект Core, контроллера и представлений, чтобы убедиться, что могут читать и записывать данные.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-149">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="b1e5e-150">(Обратите внимание, что ASP.NET Core MVC функции формирования шаблонов не будет работать с EF6 контекст ссылки из библиотеки классов.)</span><span class="sxs-lookup"><span data-stu-id="b1e5e-150">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="b1e5e-151">Сводка</span><span class="sxs-lookup"><span data-stu-id="b1e5e-151">Summary</span></span>

<span data-ttu-id="b1e5e-152">В этой статье был дан основные рекомендации по использованию платформы Entity Framework 6 в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1e5e-152">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1e5e-153">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b1e5e-153">Additional Resources</span></span>

* [<span data-ttu-id="b1e5e-154">Entity Framework — конфигурация на основе кода</span><span class="sxs-lookup"><span data-stu-id="b1e5e-154">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
