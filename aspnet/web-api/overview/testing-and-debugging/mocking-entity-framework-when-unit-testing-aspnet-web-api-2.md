---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: "Макетирование Entity Framework при модульного тестирования ASP.NET Web API 2 | Документы Microsoft"
author: tfitzmac
description: "Это руководство и приложения показано, как создавать модульные тесты для приложения веб-API 2, использующего платформу Entity Framework. Показано, как изменить..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 2d8a3df94c91d2fac79006916375764c2b90dc85
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="fc45e-104">Макетирование Entity Framework при модульного тестирования ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="fc45e-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="fc45e-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fc45e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="fc45e-106">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="fc45e-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="fc45e-107">Это руководство и приложения показано, как создавать модульные тесты для приложения веб-API 2, использующего платформу Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fc45e-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="fc45e-108">Он показывает способы изменения контроллеру формирования шаблонов для включения передачи объекта контекста для тестирования и создают тестовые объекты, которые работают с Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fc45e-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
> 
> <span data-ttu-id="fc45e-109">Введение модульное тестирование с помощью веб-API ASP.NET см. в разделе [модульное тестирование с помощью ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="fc45e-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
> 
> <span data-ttu-id="fc45e-110">В учебнике предполагается, что вы знакомы с основными понятиями веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fc45e-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="fc45e-111">Вводное руководство см. в разделе [Приступая к работе с ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="fc45e-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fc45e-112">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="fc45e-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="fc45e-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="fc45e-113">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="fc45e-114">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="fc45e-114">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="fc45e-115">Содержание раздела</span><span class="sxs-lookup"><span data-stu-id="fc45e-115">In this topic</span></span>

<span data-ttu-id="fc45e-116">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="fc45e-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="fc45e-117">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="fc45e-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="fc45e-118">Загрузить исходный код</span><span class="sxs-lookup"><span data-stu-id="fc45e-118">Download code</span></span>](#download)
- [<span data-ttu-id="fc45e-119">Создайте приложение с проект модульного теста</span><span class="sxs-lookup"><span data-stu-id="fc45e-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="fc45e-120">Создание класса модели</span><span class="sxs-lookup"><span data-stu-id="fc45e-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="fc45e-121">Добавить контроллер</span><span class="sxs-lookup"><span data-stu-id="fc45e-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="fc45e-122">Добавить внедрения зависимости</span><span class="sxs-lookup"><span data-stu-id="fc45e-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="fc45e-123">Установить пакеты NuGet в тестовый проект</span><span class="sxs-lookup"><span data-stu-id="fc45e-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="fc45e-124">Создание контекста теста</span><span class="sxs-lookup"><span data-stu-id="fc45e-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="fc45e-125">Создание тестов</span><span class="sxs-lookup"><span data-stu-id="fc45e-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="fc45e-126">Выполнение тестов</span><span class="sxs-lookup"><span data-stu-id="fc45e-126">Run tests</span></span>](#runtests)

<span data-ttu-id="fc45e-127">Если вы уже выполнили действия, описанные в [модульное тестирование с помощью ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), можно перейти к разделу [добавить контроллер](#controller).</span><span class="sxs-lookup"><span data-stu-id="fc45e-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="fc45e-128">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="fc45e-128">Prerequisites</span></span>

<span data-ttu-id="fc45e-129">Visual Studio 2017 г Community, Professional или Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="fc45e-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="fc45e-130">Загрузить исходный код</span><span class="sxs-lookup"><span data-stu-id="fc45e-130">Download code</span></span>

<span data-ttu-id="fc45e-131">Загрузить [завершенного проекта](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="fc45e-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="fc45e-132">Загружаемый проект включает в себя код модульного теста для этого раздела, а также для [веб-тестирования ASP.NET единицы API 2](unit-testing-with-aspnet-web-api.md) раздела.</span><span class="sxs-lookup"><span data-stu-id="fc45e-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="fc45e-133">Создайте приложение с проект модульного теста</span><span class="sxs-lookup"><span data-stu-id="fc45e-133">Create application with unit test project</span></span>

<span data-ttu-id="fc45e-134">Можно создать проект модульного теста, при создании приложения или добавить проект модульных тестов для существующего приложения.</span><span class="sxs-lookup"><span data-stu-id="fc45e-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="fc45e-135">В этом учебнике показано создание проекта модульного теста при создании приложения.</span><span class="sxs-lookup"><span data-stu-id="fc45e-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="fc45e-136">Создание нового веб-приложения ASP.NET с именем **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="fc45e-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="fc45e-137">В окне нового проекта ASP.NET выберите **пустой** шаблона и добавить папки и основные ссылки для веб-API.</span><span class="sxs-lookup"><span data-stu-id="fc45e-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="fc45e-138">Выберите **Добавление модульных тестов** параметр.</span><span class="sxs-lookup"><span data-stu-id="fc45e-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="fc45e-139">Автоматически присваивается имя проекта модульного теста **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="fc45e-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="fc45e-140">Можно оставить это имя.</span><span class="sxs-lookup"><span data-stu-id="fc45e-140">You can keep this name.</span></span>

![Создание проекта модульного теста](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="fc45e-142">После создания приложения, вы увидите два проекта — **StoreApp** и **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="fc45e-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="fc45e-143">Создание класса модели</span><span class="sxs-lookup"><span data-stu-id="fc45e-143">Create the model class</span></span>

<span data-ttu-id="fc45e-144">В проекте StoreApp добавьте файл класса для **моделей** папку с именем **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="fc45e-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="fc45e-145">Замените содержимое файла следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="fc45e-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="fc45e-146">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="fc45e-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="fc45e-147">Добавить контроллер</span><span class="sxs-lookup"><span data-stu-id="fc45e-147">Add the controller</span></span>

<span data-ttu-id="fc45e-148">Щелкните правой кнопкой мыши папку Controllers, а затем выберите **добавить** и **новый элемент формирования шаблонов**.</span><span class="sxs-lookup"><span data-stu-id="fc45e-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="fc45e-149">Выберите Web API 2 контроллер с действиями, использующий Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fc45e-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Добавить новый контроллер](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="fc45e-151">Установите следующие значения:</span><span class="sxs-lookup"><span data-stu-id="fc45e-151">Set the following values:</span></span>

- <span data-ttu-id="fc45e-152">Имя контроллера: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="fc45e-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="fc45e-153">Класс модели: **продукта**</span><span class="sxs-lookup"><span data-stu-id="fc45e-153">Model class: **Product**</span></span>
- <span data-ttu-id="fc45e-154">Класс контекста данных: [выберите **новый контекст данных** кнопку, которая заполняет значения, показано ниже]</span><span class="sxs-lookup"><span data-stu-id="fc45e-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![Укажите контроллер](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="fc45e-156">Нажмите кнопку **добавить** для создания контроллера с автоматически созданного кода.</span><span class="sxs-lookup"><span data-stu-id="fc45e-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="fc45e-157">Код включает методы для создания, извлечения, обновления и удаления экземпляров класса Product.</span><span class="sxs-lookup"><span data-stu-id="fc45e-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="fc45e-158">В следующем коде показан метод для добавления продукта.</span><span class="sxs-lookup"><span data-stu-id="fc45e-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="fc45e-159">Обратите внимание, что метод возвращает экземпляр **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="fc45e-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="fc45e-160">IHttpActionResult является одним из новых функций в веб-API 2, и он упрощает разработки модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="fc45e-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="fc45e-161">В следующем разделе будет настройки создаваемого кода для упрощения передачи объектов тестирования к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="fc45e-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="fc45e-162">Добавить внедрения зависимости</span><span class="sxs-lookup"><span data-stu-id="fc45e-162">Add dependency injection</span></span>

<span data-ttu-id="fc45e-163">В настоящее время класс ProductController жестко запрограммировано на использование экземпляра класса StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="fc45e-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="fc45e-164">Для изменения приложения и удалить зависимость жестко будет использовать шаблон под названием внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="fc45e-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="fc45e-165">Разрывая эту зависимость, можно передать в макет объекта при тестировании.</span><span class="sxs-lookup"><span data-stu-id="fc45e-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="fc45e-166">Щелкните правой кнопкой мыши **моделей** папки и добавьте новый интерфейс с именем **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="fc45e-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="fc45e-167">Замените код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="fc45e-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="fc45e-168">Откройте файл StoreAppContext.cs и внесите следующие изменения выделенный.</span><span class="sxs-lookup"><span data-stu-id="fc45e-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="fc45e-169">Ниже перечислены важные изменения, обратите внимание</span><span class="sxs-lookup"><span data-stu-id="fc45e-169">The important changes to note are:</span></span>

- <span data-ttu-id="fc45e-170">Класс StoreAppContext реализует интерфейс IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="fc45e-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="fc45e-171">Реализация метода MarkAsModified</span><span class="sxs-lookup"><span data-stu-id="fc45e-171">MarkAsModified method is implemented</span></span>


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="fc45e-172">Откройте файл ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="fc45e-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="fc45e-173">Измените существующий код в соответствии выделенный код.</span><span class="sxs-lookup"><span data-stu-id="fc45e-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="fc45e-174">Эти изменения нарушить зависимость на StoreAppContext и включить другие классы для передачи в другом объекте класс контекста.</span><span class="sxs-lookup"><span data-stu-id="fc45e-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="fc45e-175">Это изменение будет позволяют передавать в контексте теста во время модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="fc45e-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="fc45e-176">Имеется один дополнительные изменения, необходимые в ProductController.</span><span class="sxs-lookup"><span data-stu-id="fc45e-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="fc45e-177">В **PutProduct** метод, изменить строки, которое устанавливает состояние сущности с помощью вызова метода MarkAsModified замены.</span><span class="sxs-lookup"><span data-stu-id="fc45e-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="fc45e-178">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="fc45e-178">Build the solution.</span></span>

<span data-ttu-id="fc45e-179">Теперь все готово для настройки тестового проекта.</span><span class="sxs-lookup"><span data-stu-id="fc45e-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="fc45e-180">Установить пакеты NuGet в тестовый проект</span><span class="sxs-lookup"><span data-stu-id="fc45e-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="fc45e-181">При использовании пустого шаблона для создания приложения проекта модульного теста (StoreApp.Tests) не включает все установленные пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="fc45e-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="fc45e-182">Другие шаблоны, например шаблона веб-API, включать несколько пакетов NuGet в проекте модульного теста.</span><span class="sxs-lookup"><span data-stu-id="fc45e-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="fc45e-183">В этом учебнике необходимо включить packge Entity Framework и Microsoft ASP.NET Web API 2 Core пакета в тестовый проект.</span><span class="sxs-lookup"><span data-stu-id="fc45e-183">For this tutorial, you must include the Entity Framework packge and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="fc45e-184">Щелкните правой кнопкой мыши проект StoreApp.Tests и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fc45e-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="fc45e-185">Необходимо выбрать проект StoreApp.Tests для добавления пакетов в проект.</span><span class="sxs-lookup"><span data-stu-id="fc45e-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Управление пакетами](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="fc45e-187">Из сети пакетов находить и устанавливать пакет EntityFramework (версии 6.0 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="fc45e-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="fc45e-188">Если предполагается, что EntityFramework пакет уже установлен, возможно, выбрана проекта StoreApp вместо StoreApp.Tests проекта.</span><span class="sxs-lookup"><span data-stu-id="fc45e-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the the StoreApp.Tests project.</span></span>

![Добавление платформы Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="fc45e-190">Найти и установить пакет Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="fc45e-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Установка основных компонентов api веб-пакета](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="fc45e-192">Закройте окно Управление пакетами NuGet.</span><span class="sxs-lookup"><span data-stu-id="fc45e-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="fc45e-193">Создание контекста теста</span><span class="sxs-lookup"><span data-stu-id="fc45e-193">Create test context</span></span>

<span data-ttu-id="fc45e-194">Добавьте класс с именем **TestDbSet** в тестовый проект.</span><span class="sxs-lookup"><span data-stu-id="fc45e-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="fc45e-195">Этот класс служит базовым классом для проверочного набора данных.</span><span class="sxs-lookup"><span data-stu-id="fc45e-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="fc45e-196">Замените код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="fc45e-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="fc45e-197">Добавьте класс с именем **TestProductDbSet** в тестовый проект, который содержит следующий код.</span><span class="sxs-lookup"><span data-stu-id="fc45e-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="fc45e-198">Добавьте класс с именем **TestStoreAppContext** и замените существующий код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="fc45e-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="fc45e-199">Создание тестов</span><span class="sxs-lookup"><span data-stu-id="fc45e-199">Create tests</span></span>

<span data-ttu-id="fc45e-200">По умолчанию тестовый проект содержит файл пустой тест с именем **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="fc45e-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="fc45e-201">Этот файл показаны атрибуты, можно использовать при создании методов теста.</span><span class="sxs-lookup"><span data-stu-id="fc45e-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="fc45e-202">В этом учебнике можно удалить этот файл, поскольку будет добавлен новый класс теста.</span><span class="sxs-lookup"><span data-stu-id="fc45e-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="fc45e-203">Добавьте класс с именем **TestProductController** в тестовый проект.</span><span class="sxs-lookup"><span data-stu-id="fc45e-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="fc45e-204">Замените код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="fc45e-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="fc45e-205">Выполнить тесты</span><span class="sxs-lookup"><span data-stu-id="fc45e-205">Run tests</span></span>

<span data-ttu-id="fc45e-206">Теперь вы готовы к выполнению тестов.</span><span class="sxs-lookup"><span data-stu-id="fc45e-206">You are now ready to run the tests.</span></span> <span data-ttu-id="fc45e-207">Все, которые помечены с помощью метода **TestMethod** атрибут будет проверено.</span><span class="sxs-lookup"><span data-stu-id="fc45e-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="fc45e-208">Из **тест** пункт меню, выполнить тесты.</span><span class="sxs-lookup"><span data-stu-id="fc45e-208">From the **Test** menu item, run the tests.</span></span>

![Запуск тестов](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="fc45e-210">Откройте **Test Explorer** окна и проверьте результаты тестов.</span><span class="sxs-lookup"><span data-stu-id="fc45e-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![результаты теста](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
