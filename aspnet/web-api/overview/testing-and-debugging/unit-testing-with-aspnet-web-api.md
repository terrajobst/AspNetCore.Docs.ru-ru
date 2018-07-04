---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Модульное тестирование ASP.NET Web API 2 | Документация Майкрософт
author: tfitzmac
description: Это руководство и приложение демонстрируется создание простого модульных тестов для приложения веб-API 2. Этом руководстве показано, как включать proj модульного теста...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: da56b38809faf760b7c390eb76ac9c4556d635c6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376178"
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="719cc-104">Модульное тестирование ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="719cc-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="719cc-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="719cc-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="719cc-106">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="719cc-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="719cc-107">Это руководство и приложение демонстрируется создание простого модульных тестов для приложения веб-API 2.</span><span class="sxs-lookup"><span data-stu-id="719cc-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="719cc-108">Этом руководстве показано, как включить проект модульного теста в решении, а написание методов теста, которые проверяют возвращаемые из метода контроллера.</span><span class="sxs-lookup"><span data-stu-id="719cc-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="719cc-109">Предполагается, что вы знакомы с основными понятиями веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="719cc-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="719cc-110">Вводное руководство см. в разделе [Приступая к работе с веб-API ASP.NET 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="719cc-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="719cc-111">В этом разделе модульные тесты намеренно ограничены простых сценариев.</span><span class="sxs-lookup"><span data-stu-id="719cc-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="719cc-112">Модульное тестирование более сложных сценариев данных, см. в разделе [макетирование Entity Framework при модульного тестирования ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="719cc-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="719cc-113">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="719cc-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="719cc-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="719cc-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="719cc-115">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="719cc-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="719cc-116">Содержание раздела</span><span class="sxs-lookup"><span data-stu-id="719cc-116">In this topic</span></span>

<span data-ttu-id="719cc-117">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="719cc-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="719cc-118">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="719cc-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="719cc-119">Скачать код</span><span class="sxs-lookup"><span data-stu-id="719cc-119">Download code</span></span>](#download)
- [<span data-ttu-id="719cc-120">Создание приложения с помощью проекта модульного теста</span><span class="sxs-lookup"><span data-stu-id="719cc-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="719cc-121">Добавьте проект модульного теста, при создании приложения</span><span class="sxs-lookup"><span data-stu-id="719cc-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="719cc-122">Добавление проекта модульного тестирование в существующее приложение</span><span class="sxs-lookup"><span data-stu-id="719cc-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="719cc-123">Настройка приложения веб-API 2</span><span class="sxs-lookup"><span data-stu-id="719cc-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="719cc-124">Установите пакеты NuGet в проекте теста</span><span class="sxs-lookup"><span data-stu-id="719cc-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="719cc-125">Создание тестов</span><span class="sxs-lookup"><span data-stu-id="719cc-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="719cc-126">Выполнение тестов</span><span class="sxs-lookup"><span data-stu-id="719cc-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="719cc-127">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="719cc-127">Prerequisites</span></span>

<span data-ttu-id="719cc-128">Visual Studio 2017 Community, Professional или Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="719cc-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="719cc-129">Скачать код</span><span class="sxs-lookup"><span data-stu-id="719cc-129">Download code</span></span>

<span data-ttu-id="719cc-130">Скачайте [завершенного проекта](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="719cc-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="719cc-131">Загружаемый проект включает в себя код модульного теста для этого раздела, а также для [макетирование Entity Framework при модульного тестирования веб-API ASP.NET](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) раздела.</span><span class="sxs-lookup"><span data-stu-id="719cc-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="719cc-132">Создание приложения с помощью проекта модульного теста</span><span class="sxs-lookup"><span data-stu-id="719cc-132">Create application with unit test project</span></span>

<span data-ttu-id="719cc-133">Можно создать проект модульного теста, при создании приложения или добавить проект модульных тестов для существующего приложения.</span><span class="sxs-lookup"><span data-stu-id="719cc-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="719cc-134">Мы продемонстрируем оба метода для создания проекта модульного теста.</span><span class="sxs-lookup"><span data-stu-id="719cc-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="719cc-135">Чтобы выполнить это руководство, можно использовать любой из этих подходов.</span><span class="sxs-lookup"><span data-stu-id="719cc-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="719cc-136">Добавьте проект модульного теста, при создании приложения</span><span class="sxs-lookup"><span data-stu-id="719cc-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="719cc-137">Создание нового веб-приложения ASP.NET с именем **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="719cc-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Создание проекта](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="719cc-139">В окне нового проекта ASP.NET выберите **пустой** шаблона и добавить папки и основные ссылки для веб-API.</span><span class="sxs-lookup"><span data-stu-id="719cc-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="719cc-140">Выберите **Добавление модульных тестов** параметр.</span><span class="sxs-lookup"><span data-stu-id="719cc-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="719cc-141">Автоматически присваивается имя проекта модульного теста **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="719cc-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="719cc-142">Можно оставить это имя.</span><span class="sxs-lookup"><span data-stu-id="719cc-142">You can keep this name.</span></span>

![Создание проекта модульного теста](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="719cc-144">После создания приложения, вы увидите, что она содержит два проекта.</span><span class="sxs-lookup"><span data-stu-id="719cc-144">After creating the application, you will see it contains two projects.</span></span>

![два проекта](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="719cc-146">Добавление проекта модульного тестирование в существующее приложение</span><span class="sxs-lookup"><span data-stu-id="719cc-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="719cc-147">Если вы не создали проект модульного теста при создании приложения, его можно добавить в любое время.</span><span class="sxs-lookup"><span data-stu-id="719cc-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="719cc-148">Например, предположим, что у вас уже есть приложение с именем StoreApp и вы хотите добавить модульные тесты.</span><span class="sxs-lookup"><span data-stu-id="719cc-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="719cc-149">Чтобы добавить проект модульного теста, щелкните правой кнопкой мыши решение и выберите **добавить** и **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="719cc-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Добавление нового проекта к решению](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="719cc-151">Выберите **теста** в левой панели и выберите **проект модульного теста** для типа проекта.</span><span class="sxs-lookup"><span data-stu-id="719cc-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="719cc-152">Назовите проект **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="719cc-152">Name the project **StoreApp.Tests**.</span></span>

![Добавьте проект модульного теста](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="719cc-154">Вы увидите проект модульного теста в решении.</span><span class="sxs-lookup"><span data-stu-id="719cc-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="719cc-155">В проект модульного теста добавьте ссылку на проект в исходный проект.</span><span class="sxs-lookup"><span data-stu-id="719cc-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="719cc-156">Настройка приложения веб-API 2</span><span class="sxs-lookup"><span data-stu-id="719cc-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="719cc-157">В проекте StoreApp добавьте файл класса для **моделей** папку с именем **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="719cc-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="719cc-158">Замените содержимое файла следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="719cc-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="719cc-159">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="719cc-159">Build the solution.</span></span>

<span data-ttu-id="719cc-160">Щелкните правой кнопкой мыши папку Controllers и выберите **добавить** и **создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="719cc-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="719cc-161">Выберите **контроллер - пустой веб-API 2**.</span><span class="sxs-lookup"><span data-stu-id="719cc-161">Select **Web API 2 Controller - Empty**.</span></span>

![Добавление нового контроллера](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="719cc-163">Укажите имя контроллера **SimpleProductController**и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="719cc-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Укажите контроллер](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="719cc-165">Замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="719cc-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="719cc-166">Чтобы упростить этот пример, данные хранятся в список, а не базы данных.</span><span class="sxs-lookup"><span data-stu-id="719cc-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="719cc-167">Список, определенный в этот класс представляет рабочих данных.</span><span class="sxs-lookup"><span data-stu-id="719cc-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="719cc-168">Обратите внимание на то, что контроллер включает конструктор, который принимает в качестве параметра список объектов продукта.</span><span class="sxs-lookup"><span data-stu-id="719cc-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="719cc-169">Этот конструктор позволяет передать тестовые данные при модульном тестировании.</span><span class="sxs-lookup"><span data-stu-id="719cc-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="719cc-170">Контроллер также включает две **async** методы, чтобы проиллюстрировать модульное тестирование асинхронных методов.</span><span class="sxs-lookup"><span data-stu-id="719cc-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="719cc-171">Эти асинхронные методы были реализованы путем вызова **Task.FromResult** для минимизации лишнего кода, но обычно методы следует использовать ресурсоемкими операциями.</span><span class="sxs-lookup"><span data-stu-id="719cc-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="719cc-172">Метод GetProduct возвращает экземпляр **IHttpActionResult** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="719cc-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="719cc-173">IHttpActionResult является одним из новых функций в веб-API 2, и он упрощает разработки модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="719cc-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="719cc-174">Классы, реализующие интерфейс IHttpActionResult находятся в [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) пространства имен.</span><span class="sxs-lookup"><span data-stu-id="719cc-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="719cc-175">Эти классы представляют возможные ответы на запрос действие, и они соответствуют коды состояния HTTP.</span><span class="sxs-lookup"><span data-stu-id="719cc-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="719cc-176">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="719cc-176">Build the solution.</span></span>

<span data-ttu-id="719cc-177">Теперь вы готовы настроить тестовый проект.</span><span class="sxs-lookup"><span data-stu-id="719cc-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="719cc-178">Установите пакеты NuGet в проекте теста</span><span class="sxs-lookup"><span data-stu-id="719cc-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="719cc-179">При использовании пустого шаблона для создания приложения, проект модульного теста (StoreApp.Tests) не включает все установленные пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="719cc-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="719cc-180">Другие шаблоны, такие как шаблон веб-API, включают некоторые пакеты NuGet в проект модульного теста.</span><span class="sxs-lookup"><span data-stu-id="719cc-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="719cc-181">Для этого руководства необходимо включить пакет Microsoft ASP.NET Web API 2 Core для тестового проекта.</span><span class="sxs-lookup"><span data-stu-id="719cc-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="719cc-182">Щелкните правой кнопкой мыши проект StoreApp.Tests и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="719cc-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="719cc-183">Необходимо выбрать проект StoreApp.Tests, чтобы добавить пакеты для этого проекта.</span><span class="sxs-lookup"><span data-stu-id="719cc-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Управление пакетами](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="719cc-185">Найдите и установите пакет Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="719cc-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Установите пакет core web api](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="719cc-187">Закройте окно Управление пакетами NuGet.</span><span class="sxs-lookup"><span data-stu-id="719cc-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="719cc-188">Создание тестов</span><span class="sxs-lookup"><span data-stu-id="719cc-188">Create tests</span></span>

<span data-ttu-id="719cc-189">По умолчанию тестовый проект входит пустой тестовый файл UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="719cc-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="719cc-190">Этот файл представлены атрибуты, можно использовать для создания методов теста.</span><span class="sxs-lookup"><span data-stu-id="719cc-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="719cc-191">Для модульных тестов можно использовать этот файл или создать свой собственный файл.</span><span class="sxs-lookup"><span data-stu-id="719cc-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="719cc-193">В этом руководстве вы создадите свой собственный класс теста.</span><span class="sxs-lookup"><span data-stu-id="719cc-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="719cc-194">Можно удалить файл UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="719cc-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="719cc-195">Добавьте класс с именем **TestSimpleProductController.cs**и замените код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="719cc-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="719cc-196">Выполнить тесты</span><span class="sxs-lookup"><span data-stu-id="719cc-196">Run tests</span></span>

<span data-ttu-id="719cc-197">Теперь вы готовы для выполнения тестов.</span><span class="sxs-lookup"><span data-stu-id="719cc-197">You are now ready to run the tests.</span></span> <span data-ttu-id="719cc-198">Все, которые помечены с помощью метода **TestMethod** атрибут будет проверено.</span><span class="sxs-lookup"><span data-stu-id="719cc-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="719cc-199">Из **теста** пункта меню, выполните тесты.</span><span class="sxs-lookup"><span data-stu-id="719cc-199">From the **Test** menu item, run the tests.</span></span>

![Запуск тестов](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="719cc-201">Откройте **обозреватель тестов** окно и обратите внимание, что результаты тестов.</span><span class="sxs-lookup"><span data-stu-id="719cc-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![результаты теста](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="719cc-203">Сводка</span><span class="sxs-lookup"><span data-stu-id="719cc-203">Summary</span></span>

<span data-ttu-id="719cc-204">Вы завершили этот учебник.</span><span class="sxs-lookup"><span data-stu-id="719cc-204">You have completed this tutorial.</span></span> <span data-ttu-id="719cc-205">Данные в этом учебнике намеренно была упрощена, чтобы сосредоточиться на модульное тестирование условия.</span><span class="sxs-lookup"><span data-stu-id="719cc-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="719cc-206">Модульное тестирование более сложных сценариев данных, см. в разделе [макетирование Entity Framework при модульного тестирования ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="719cc-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
