---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Макетирование Entity Framework при модульном тестировании ASP.NET Web API 2 | Документация Майкрософт
author: Rick-Anderson
description: Это руководство и приложения показано, как создавать модульные тесты для приложения веб-API 2, использующий Entity Framework. Показано, как изменить...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f1799b3f9d698053c397e57da3f33ff900ec4013
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021031"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="c8696-104">Макетирование Entity Framework при модульном тестировании ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="c8696-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="c8696-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c8696-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="c8696-106">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="c8696-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="c8696-107">Это руководство и приложения показано, как создавать модульные тесты для приложения веб-API 2, использующий Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c8696-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="c8696-108">Нем показано, как изменить сформированный контроллера для включения передачи объекта контекста для тестирования и создают тестовые объекты, которые работают с Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c8696-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
>
> <span data-ttu-id="c8696-109">Введение в модульное тестирование с помощью веб-API ASP.NET, см. в разделе [модульное тестирование с помощью ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c8696-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
>
> <span data-ttu-id="c8696-110">Предполагается, что вы знакомы с основными понятиями веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c8696-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="c8696-111">Вводное руководство см. в разделе [Приступая к работе с веб-API ASP.NET 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c8696-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c8696-112">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="c8696-112">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="c8696-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c8696-113">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="c8696-114">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="c8696-114">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="c8696-115">Содержание раздела</span><span class="sxs-lookup"><span data-stu-id="c8696-115">In this topic</span></span>

<span data-ttu-id="c8696-116">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="c8696-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="c8696-117">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="c8696-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="c8696-118">Скачать код</span><span class="sxs-lookup"><span data-stu-id="c8696-118">Download code</span></span>](#download)
- [<span data-ttu-id="c8696-119">Создание приложения с помощью проекта модульного теста</span><span class="sxs-lookup"><span data-stu-id="c8696-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="c8696-120">Создание класса модели</span><span class="sxs-lookup"><span data-stu-id="c8696-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="c8696-121">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="c8696-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="c8696-122">Добавление внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="c8696-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="c8696-123">Установите пакеты NuGet в проекте теста</span><span class="sxs-lookup"><span data-stu-id="c8696-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="c8696-124">Создание контекста теста</span><span class="sxs-lookup"><span data-stu-id="c8696-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="c8696-125">Создание тестов</span><span class="sxs-lookup"><span data-stu-id="c8696-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="c8696-126">Выполнение тестов</span><span class="sxs-lookup"><span data-stu-id="c8696-126">Run tests</span></span>](#runtests)

<span data-ttu-id="c8696-127">Если вы уже выполнили действия, описанные в [модульное тестирование с помощью ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), можно перейти к разделу [добавить контроллер](#controller).</span><span class="sxs-lookup"><span data-stu-id="c8696-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="c8696-128">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c8696-128">Prerequisites</span></span>

<span data-ttu-id="c8696-129">Visual Studio 2017 Community, Professional или Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="c8696-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="c8696-130">Скачать код</span><span class="sxs-lookup"><span data-stu-id="c8696-130">Download code</span></span>

<span data-ttu-id="c8696-131">Скачайте [завершенного проекта](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="c8696-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="c8696-132">Загружаемый проект включает в себя код модульного теста для этого раздела, а также для [модульного тестирования ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) раздела.</span><span class="sxs-lookup"><span data-stu-id="c8696-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="c8696-133">Создание приложения с помощью проекта модульного теста</span><span class="sxs-lookup"><span data-stu-id="c8696-133">Create application with unit test project</span></span>

<span data-ttu-id="c8696-134">Можно создать проект модульного теста, при создании приложения или добавить проект модульных тестов для существующего приложения.</span><span class="sxs-lookup"><span data-stu-id="c8696-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="c8696-135">Этом руководстве показано создание проекта модульного теста, при создании приложения.</span><span class="sxs-lookup"><span data-stu-id="c8696-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="c8696-136">Создание нового веб-приложения ASP.NET с именем **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="c8696-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="c8696-137">В окне нового проекта ASP.NET выберите **пустой** шаблона и добавить папки и основные ссылки для веб-API.</span><span class="sxs-lookup"><span data-stu-id="c8696-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="c8696-138">Выберите **Добавление модульных тестов** параметр.</span><span class="sxs-lookup"><span data-stu-id="c8696-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="c8696-139">Автоматически присваивается имя проекта модульного теста **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="c8696-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="c8696-140">Можно оставить это имя.</span><span class="sxs-lookup"><span data-stu-id="c8696-140">You can keep this name.</span></span>

![Создание проекта модульного теста](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="c8696-142">После создания приложения, вы увидите, он содержит два проекта — **StoreApp** и **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="c8696-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="c8696-143">Создание класса модели</span><span class="sxs-lookup"><span data-stu-id="c8696-143">Create the model class</span></span>

<span data-ttu-id="c8696-144">В проекте StoreApp добавьте файл класса для **моделей** папку с именем **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="c8696-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="c8696-145">Замените содержимое файла следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="c8696-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="c8696-146">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="c8696-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="c8696-147">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="c8696-147">Add the controller</span></span>

<span data-ttu-id="c8696-148">Щелкните правой кнопкой мыши папку Controllers и выберите **добавить** и **создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="c8696-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="c8696-149">Выберите контроллер Web API 2 с действиями, использующий Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c8696-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Добавление нового контроллера](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="c8696-151">Установите следующие значения:</span><span class="sxs-lookup"><span data-stu-id="c8696-151">Set the following values:</span></span>

- <span data-ttu-id="c8696-152">Имя контроллера: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="c8696-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="c8696-153">Класс модели: **продукта**</span><span class="sxs-lookup"><span data-stu-id="c8696-153">Model class: **Product**</span></span>
- <span data-ttu-id="c8696-154">Класс контекста данных: [выберите **новый контекст данных** кнопки, который заполняет значения, показанный ниже]</span><span class="sxs-lookup"><span data-stu-id="c8696-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![Укажите контроллер](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="c8696-156">Нажмите кнопку **добавить** для создания контроллера с автоматически созданным кодом.</span><span class="sxs-lookup"><span data-stu-id="c8696-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="c8696-157">Код содержит методы для создания, извлечения, обновления и удаления экземпляров класса Product.</span><span class="sxs-lookup"><span data-stu-id="c8696-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="c8696-158">В следующем коде показано метод для добавления продукта.</span><span class="sxs-lookup"><span data-stu-id="c8696-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="c8696-159">Обратите внимание на то, что метод возвращает экземпляр **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="c8696-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="c8696-160">IHttpActionResult является одним из новых функций в веб-API 2, и он упрощает разработки модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="c8696-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="c8696-161">В следующем разделе будет изменять созданный код для упрощения передачи объектов тестирования к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="c8696-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="c8696-162">Добавление внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="c8696-162">Add dependency injection</span></span>

<span data-ttu-id="c8696-163">В настоящее время класс ProductController жестко заданной необходимости использовать экземпляр класса StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="c8696-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="c8696-164">Для изменения приложения и удалить эту жестко заданную зависимость будет использовать шаблон под названием внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="c8696-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="c8696-165">Разрывая эту зависимость, можно передать в макет объекта при тестировании.</span><span class="sxs-lookup"><span data-stu-id="c8696-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="c8696-166">Щелкните правой кнопкой мыши **моделей** папки и добавьте новый интерфейс с именем **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="c8696-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="c8696-167">Замените код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="c8696-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="c8696-168">Откройте файл StoreAppContext.cs и внесите следующие выделенные изменения.</span><span class="sxs-lookup"><span data-stu-id="c8696-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="c8696-169">Важные изменения, следует отметить, —:</span><span class="sxs-lookup"><span data-stu-id="c8696-169">The important changes to note are:</span></span>

- <span data-ttu-id="c8696-170">Класс StoreAppContext реализует интерфейс IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="c8696-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="c8696-171">Метод MarkAsModified реализуется</span><span class="sxs-lookup"><span data-stu-id="c8696-171">MarkAsModified method is implemented</span></span>


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="c8696-172">Откройте файл ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="c8696-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="c8696-173">Измените существующий код в соответствии с выделенный код.</span><span class="sxs-lookup"><span data-stu-id="c8696-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="c8696-174">Эти изменения нарушить зависимость на StoreAppContext и включить другие классы для передачи в другом объекте класса контекста.</span><span class="sxs-lookup"><span data-stu-id="c8696-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="c8696-175">Это изменение позволит передать контекст теста во время модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="c8696-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="c8696-176">Есть еще одно изменение, которые необходимо внести в ProductController.</span><span class="sxs-lookup"><span data-stu-id="c8696-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="c8696-177">В **PutProduct** метод, замените строку, которая задает состояние сущности изменять с помощью вызова метода MarkAsModified.</span><span class="sxs-lookup"><span data-stu-id="c8696-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="c8696-178">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="c8696-178">Build the solution.</span></span>

<span data-ttu-id="c8696-179">Теперь вы готовы настроить тестовый проект.</span><span class="sxs-lookup"><span data-stu-id="c8696-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="c8696-180">Установите пакеты NuGet в проекте теста</span><span class="sxs-lookup"><span data-stu-id="c8696-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="c8696-181">При использовании пустого шаблона для создания приложения, проект модульного теста (StoreApp.Tests) не включает все установленные пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="c8696-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="c8696-182">Другие шаблоны, такие как шаблон веб-API, включают некоторые пакеты NuGet в проект модульного теста.</span><span class="sxs-lookup"><span data-stu-id="c8696-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="c8696-183">Для этого руководства необходимо включить пакета Entity Framework и пакета Microsoft ASP.NET Web API 2 Core для тестового проекта.</span><span class="sxs-lookup"><span data-stu-id="c8696-183">For this tutorial, you must include the Entity Framework packge and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="c8696-184">Щелкните правой кнопкой мыши проект StoreApp.Tests и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c8696-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="c8696-185">Необходимо выбрать проект StoreApp.Tests, чтобы добавить пакеты для этого проекта.</span><span class="sxs-lookup"><span data-stu-id="c8696-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Управление пакетами](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="c8696-187">Из Online пакетов найдите и установите пакет EntityFramework (версии 6.0 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="c8696-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="c8696-188">Если кажется, что EntityFramework пакет уже установлен, возможно, выбрана в StoreApp проект, а не StoreApp.Tests проекта.</span><span class="sxs-lookup"><span data-stu-id="c8696-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Добавление платформы Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="c8696-190">Найдите и установите пакет Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="c8696-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Установите пакет core web api](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="c8696-192">Закройте окно Управление пакетами NuGet.</span><span class="sxs-lookup"><span data-stu-id="c8696-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="c8696-193">Создание контекста теста</span><span class="sxs-lookup"><span data-stu-id="c8696-193">Create test context</span></span>

<span data-ttu-id="c8696-194">Добавьте класс с именем **TestDbSet** в тестовый проект.</span><span class="sxs-lookup"><span data-stu-id="c8696-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="c8696-195">Этот класс служит базовым классом для проверочного набора данных.</span><span class="sxs-lookup"><span data-stu-id="c8696-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="c8696-196">Замените код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="c8696-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="c8696-197">Добавьте класс с именем **TestProductDbSet** в тестовый проект, который содержит следующий код.</span><span class="sxs-lookup"><span data-stu-id="c8696-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="c8696-198">Добавьте класс с именем **TestStoreAppContext** и замените существующий код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="c8696-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="c8696-199">Создание тестов</span><span class="sxs-lookup"><span data-stu-id="c8696-199">Create tests</span></span>

<span data-ttu-id="c8696-200">По умолчанию тестовый проект содержит файл пустой тест с именем **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="c8696-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="c8696-201">Этот файл представлены атрибуты, можно использовать для создания методов теста.</span><span class="sxs-lookup"><span data-stu-id="c8696-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="c8696-202">В этом руководстве можно удалить файл, так как вы добавите нового тестового класса.</span><span class="sxs-lookup"><span data-stu-id="c8696-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="c8696-203">Добавьте класс с именем **TestProductController** в тестовый проект.</span><span class="sxs-lookup"><span data-stu-id="c8696-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="c8696-204">Замените код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="c8696-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="c8696-205">Выполнить тесты</span><span class="sxs-lookup"><span data-stu-id="c8696-205">Run tests</span></span>

<span data-ttu-id="c8696-206">Теперь вы готовы для выполнения тестов.</span><span class="sxs-lookup"><span data-stu-id="c8696-206">You are now ready to run the tests.</span></span> <span data-ttu-id="c8696-207">Все, которые помечены с помощью метода **TestMethod** атрибут будет проверено.</span><span class="sxs-lookup"><span data-stu-id="c8696-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="c8696-208">Из **теста** пункта меню, выполните тесты.</span><span class="sxs-lookup"><span data-stu-id="c8696-208">From the **Test** menu item, run the tests.</span></span>

![Запуск тестов](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="c8696-210">Откройте **обозреватель тестов** окно и обратите внимание, что результаты тестов.</span><span class="sxs-lookup"><span data-stu-id="c8696-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![результаты теста](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
