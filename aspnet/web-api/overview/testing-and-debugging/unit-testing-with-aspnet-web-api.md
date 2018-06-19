---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Модульное тестирование ASP.NET Web API 2 | Документы Microsoft
author: tfitzmac
description: Это руководство и приложения показано, как создать простой модульные тесты для приложения веб-API 2. Этот учебник показывает, как включать proj модульных тестов...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d6102dd81589e41894d8ecd95bf9ddd761a65bd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042749"
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="8ac4e-104">Модульное тестирование ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="8ac4e-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="8ac4e-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8ac4e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="8ac4e-106">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="8ac4e-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="8ac4e-107">Это руководство и приложения показано, как создать простой модульные тесты для приложения веб-API 2.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="8ac4e-108">Этот учебник демонстрирует включают проект модульного теста в решении, а также написать методы теста, проверьте значения, возвращаемого из метода контроллера.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="8ac4e-109">В учебнике предполагается, что вы знакомы с основными понятиями веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="8ac4e-110">Вводное руководство см. в разделе [Приступая к работе с ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="8ac4e-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="8ac4e-111">Модульные тесты в этом разделе намеренно ограничены простых сценариев.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="8ac4e-112">Модульное тестирование более сложных сценариев, данных, в разделе [имитации Entity Framework при веб-тестировании ASP.NET единицы API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="8ac4e-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8ac4e-113">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="8ac4e-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="8ac4e-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8ac4e-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="8ac4e-115">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="8ac4e-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="8ac4e-116">Содержание раздела</span><span class="sxs-lookup"><span data-stu-id="8ac4e-116">In this topic</span></span>

<span data-ttu-id="8ac4e-117">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="8ac4e-118">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="8ac4e-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="8ac4e-119">Загрузить исходный код</span><span class="sxs-lookup"><span data-stu-id="8ac4e-119">Download code</span></span>](#download)
- [<span data-ttu-id="8ac4e-120">Создайте приложение с проект модульного теста</span><span class="sxs-lookup"><span data-stu-id="8ac4e-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="8ac4e-121">При создании приложения, добавьте проект модульного теста</span><span class="sxs-lookup"><span data-stu-id="8ac4e-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="8ac4e-122">Добавьте проект модульных тестов для существующего приложения</span><span class="sxs-lookup"><span data-stu-id="8ac4e-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="8ac4e-123">Настройка приложения веб-API 2</span><span class="sxs-lookup"><span data-stu-id="8ac4e-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="8ac4e-124">Установить пакеты NuGet в тестовый проект</span><span class="sxs-lookup"><span data-stu-id="8ac4e-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="8ac4e-125">Создание тестов</span><span class="sxs-lookup"><span data-stu-id="8ac4e-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="8ac4e-126">Выполнение тестов</span><span class="sxs-lookup"><span data-stu-id="8ac4e-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="8ac4e-127">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="8ac4e-127">Prerequisites</span></span>

<span data-ttu-id="8ac4e-128">Visual Studio 2017 г Community, Professional или Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="8ac4e-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="8ac4e-129">Загрузить исходный код</span><span class="sxs-lookup"><span data-stu-id="8ac4e-129">Download code</span></span>

<span data-ttu-id="8ac4e-130">Загрузить [завершенного проекта](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="8ac4e-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="8ac4e-131">Загружаемый проект включает в себя код модульного теста для этого раздела, а также для [имитации Entity Framework при модульного тестирования ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) раздела.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="8ac4e-132">Создайте приложение с проект модульного теста</span><span class="sxs-lookup"><span data-stu-id="8ac4e-132">Create application with unit test project</span></span>

<span data-ttu-id="8ac4e-133">Можно создать проект модульного теста, при создании приложения или добавить проект модульных тестов для существующего приложения.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="8ac4e-134">Этот учебник показывает обоих методов для создания проекта модульного теста.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="8ac4e-135">Чтобы выполнить этот учебник, можно использовать любой из этих подходов.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="8ac4e-136">При создании приложения, добавьте проект модульного теста</span><span class="sxs-lookup"><span data-stu-id="8ac4e-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="8ac4e-137">Создание нового веб-приложения ASP.NET с именем **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Создание проекта](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="8ac4e-139">В окне нового проекта ASP.NET выберите **пустой** шаблона и добавить папки и основные ссылки для веб-API.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="8ac4e-140">Выберите **Добавление модульных тестов** параметр.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="8ac4e-141">Автоматически присваивается имя проекта модульного теста **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="8ac4e-142">Можно оставить это имя.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-142">You can keep this name.</span></span>

![Создание проекта модульного теста](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="8ac4e-144">После создания приложения, вы увидите, что он содержит два проекта.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-144">After creating the application, you will see it contains two projects.</span></span>

![два проекта](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="8ac4e-146">Добавьте проект модульных тестов для существующего приложения</span><span class="sxs-lookup"><span data-stu-id="8ac4e-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="8ac4e-147">Если вы не создали проект модульного теста при создании приложения, его можно добавить в любое время.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="8ac4e-148">Например предположим, имеется приложение с именем StoreApp и вы хотите добавить модульные тесты.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="8ac4e-149">Чтобы добавить проект модульного теста, щелкните правой кнопкой мыши решение и выберите **добавить** и **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Добавить новый проект в решение](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="8ac4e-151">Выберите **тест** в левой области и выберите **проект модульного теста** для типа проекта.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="8ac4e-152">Назовите проект **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-152">Name the project **StoreApp.Tests**.</span></span>

![Добавьте проект модульного теста](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="8ac4e-154">Вы увидите проект модульного теста в решении.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="8ac4e-155">В проекте модульного теста добавьте ссылку на проект в исходном проекте.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="8ac4e-156">Настройка приложения веб-API 2</span><span class="sxs-lookup"><span data-stu-id="8ac4e-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="8ac4e-157">В проекте StoreApp добавьте файл класса для **моделей** папку с именем **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="8ac4e-158">Замените содержимое файла следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="8ac4e-159">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-159">Build the solution.</span></span>

<span data-ttu-id="8ac4e-160">Щелкните правой кнопкой мыши папку Controllers, а затем выберите **добавить** и **новый элемент формирования шаблонов**.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="8ac4e-161">Выберите **пустой контроллер — веб-API 2**.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-161">Select **Web API 2 Controller - Empty**.</span></span>

![Добавить новый контроллер](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="8ac4e-163">Задайте имя контроллера **SimpleProductController**и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Укажите контроллер](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="8ac4e-165">Замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="8ac4e-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="8ac4e-166">Чтобы упростить этот пример, данные хранятся в список, а не базы данных.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="8ac4e-167">Список, определенный в этот класс представляет производственных данных.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="8ac4e-168">Обратите внимание, что контроллер включает конструктор, который принимает список объектов продукта в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="8ac4e-169">Этот конструктор позволяет передавать данные теста при модульном тестировании.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="8ac4e-170">Контроллер также содержит две **async** методы для иллюстрации модульное тестирование асинхронных методов.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="8ac4e-171">Эти асинхронные методы были реализованы путем вызова **Task.FromResult** для минимизации лишнего кода, но обычно методы включить ресурсоемких операций.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="8ac4e-172">Метод GetProduct возвращает экземпляр **IHttpActionResult** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="8ac4e-173">IHttpActionResult является одним из новых функций в веб-API 2, и он упрощает разработки модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="8ac4e-174">Классы, реализующие интерфейс IHttpActionResult находятся в [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) пространства имен.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="8ac4e-175">Эти классы представляют возможные ответы из действия запроса, и они соответствуют коды состояния HTTP.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="8ac4e-176">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-176">Build the solution.</span></span>

<span data-ttu-id="8ac4e-177">Теперь все готово для настройки тестового проекта.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="8ac4e-178">Установить пакеты NuGet в тестовый проект</span><span class="sxs-lookup"><span data-stu-id="8ac4e-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="8ac4e-179">При использовании пустого шаблона для создания приложения проекта модульного теста (StoreApp.Tests) не включает все установленные пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="8ac4e-180">Другие шаблоны, например шаблона веб-API, включать несколько пакетов NuGet в проекте модульного теста.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="8ac4e-181">В этом учебнике необходимо включить пакет Microsoft ASP.NET Web API 2 Core в тестовый проект.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="8ac4e-182">Щелкните правой кнопкой мыши проект StoreApp.Tests и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="8ac4e-183">Необходимо выбрать проект StoreApp.Tests для добавления пакетов в проект.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Управление пакетами](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="8ac4e-185">Найти и установить пакет Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Установка основных компонентов api веб-пакета](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="8ac4e-187">Закройте окно Управление пакетами NuGet.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="8ac4e-188">Создание тестов</span><span class="sxs-lookup"><span data-stu-id="8ac4e-188">Create tests</span></span>

<span data-ttu-id="8ac4e-189">По умолчанию тестовый проект входит пустой тестовый файл UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="8ac4e-190">Этот файл показаны атрибуты, можно использовать при создании методов теста.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="8ac4e-191">Для модульных тестов можно использовать этот файл или создать свой собственный файл.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="8ac4e-193">В этом учебнике вы создадите класс теста.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="8ac4e-194">Можно удалить файл UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="8ac4e-195">Добавьте класс с именем **TestSimpleProductController.cs**и замените код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="8ac4e-196">Выполнить тесты</span><span class="sxs-lookup"><span data-stu-id="8ac4e-196">Run tests</span></span>

<span data-ttu-id="8ac4e-197">Теперь вы готовы к выполнению тестов.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-197">You are now ready to run the tests.</span></span> <span data-ttu-id="8ac4e-198">Все, которые помечены с помощью метода **TestMethod** атрибут будет проверено.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="8ac4e-199">Из **тест** пункт меню, выполнить тесты.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-199">From the **Test** menu item, run the tests.</span></span>

![Запуск тестов](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="8ac4e-201">Откройте **Test Explorer** окна и проверьте результаты тестов.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![результаты теста](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="8ac4e-203">Сводка</span><span class="sxs-lookup"><span data-stu-id="8ac4e-203">Summary</span></span>

<span data-ttu-id="8ac4e-204">Учебник успешно пройден.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-204">You have completed this tutorial.</span></span> <span data-ttu-id="8ac4e-205">Данные в этом учебнике намеренно была упрощена, чтобы сосредоточиться на модульное тестирование условия.</span><span class="sxs-lookup"><span data-stu-id="8ac4e-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="8ac4e-206">Модульное тестирование более сложных сценариев, данных, в разделе [имитации Entity Framework при веб-тестировании ASP.NET единицы API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="8ac4e-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
