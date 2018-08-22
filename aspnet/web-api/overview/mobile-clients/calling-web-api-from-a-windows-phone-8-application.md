---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Вызов веб-API из Windows Phone 8 приложения (C#) | Документация Майкрософт
author: rmcmurray
description: Создание полного сценария end-to-end, состоящий из приложения веб-API ASP.NET, которое содержит каталог книг в приложение Windows Phone 8.
ms.author: riande
ms.date: 10/09/2013
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: ca2b5f41f6c3bd38faacd1e15c4dee6f6210aff7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837502"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="fa296-103">Вызов веб-API из приложения Windows Phone 8 (C#)</span><span class="sxs-lookup"><span data-stu-id="fa296-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="fa296-104">по [(Robert McMurray)](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="fa296-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="fa296-105">В этом руководстве вы узнаете, как создать полного сценария end-to-end, состоящий из приложения веб-API ASP.NET, которое содержит каталог книг в приложение Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="fa296-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="fa296-106">Обзор</span><span class="sxs-lookup"><span data-stu-id="fa296-106">Overview</span></span>

<span data-ttu-id="fa296-107">Службы rESTful, например веб-API ASP.NET упрощают создание приложений на основе HTTP для разработчиков посредством абстрагирования архитектуры для серверных и клиентских приложений.</span><span class="sxs-lookup"><span data-stu-id="fa296-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="fa296-108">Вместо того чтобы создавать собственный протокол на базе сокетов для обмена данными, веб-API разработчикам просто нужно опубликовать необходимые методы HTTP для своего приложения (например: GET, POST, PUT, DELETE), и разработчики клиентских приложений необходимо только методы HTTP, которые необходимы для своего приложения.</span><span class="sxs-lookup"><span data-stu-id="fa296-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="fa296-109">В этом руководстве end-to-end вы узнаете, как использовать веб-API для создания следующих проектов:</span><span class="sxs-lookup"><span data-stu-id="fa296-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="fa296-110">В [первой части этого учебника](#STEP1), вы создадите приложение веб-API ASP.NET с поддержкой всех операций создания, чтения, обновления и удаления (CRUD) для управления каталогом книги.</span><span class="sxs-lookup"><span data-stu-id="fa296-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="fa296-111">Это приложение будет использовать [пример XML-файла (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) из MSDN.</span><span class="sxs-lookup"><span data-stu-id="fa296-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="fa296-112">В [второй части этого учебника](#STEP2), вы создадите интерактивные приложения Windows Phone 8, которое получает данные из приложения веб-API.</span><span class="sxs-lookup"><span data-stu-id="fa296-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="fa296-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="fa296-113">Prerequisites</span></span>

- <span data-ttu-id="fa296-114">Visual Studio 2013 с Windows Phone 8 установлен пакет SDK</span><span class="sxs-lookup"><span data-stu-id="fa296-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="fa296-115">Windows 8 или более поздней на 64-разрядной системы с Hyper-V установлено</span><span class="sxs-lookup"><span data-stu-id="fa296-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="fa296-116">Список дополнительных требований, см. в разделе *требования к системе* разделе [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) странице загрузки.</span><span class="sxs-lookup"><span data-stu-id="fa296-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="fa296-117">Если нужно проверить возможность подключения между веб-API и проекты Windows Phone 8 в локальной системе, необходимо будет следуйте инструкциям, приведенным в *[подключение к веб-приложений API в локальном эмуляторе Windows Phone 8 Компьютер](https://go.microsoft.com/fwlink/?LinkId=324014)* статью, чтобы настроить среду тестирования.</span><span class="sxs-lookup"><span data-stu-id="fa296-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="fa296-118">Шаг 1: Создание веб-проекта API Bookstore</span><span class="sxs-lookup"><span data-stu-id="fa296-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="fa296-119">Первым шагом данного руководства end-to-end является создание проекта веб-API, который поддерживает все операции CRUD; Обратите внимание на то, что вы добавите в проект приложения Windows Phone с этим решением в [шаг 2](#STEP2) работы с этим руководством.</span><span class="sxs-lookup"><span data-stu-id="fa296-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="fa296-120">Откройте **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="fa296-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="fa296-121">Нажмите кнопку **файл**, затем **новый**, а затем **проекта**.</span><span class="sxs-lookup"><span data-stu-id="fa296-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="fa296-122">Когда **новый проект** диалоговое окно отображается, разверните узел **установленные**, затем **шаблоны**, затем **Visual C#**, а затем **Web**.</span><span class="sxs-lookup"><span data-stu-id="fa296-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="fa296-123">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="fa296-123">Click image to expand</span></span>                                                                |


4. <span data-ttu-id="fa296-124">Выделите **веб-приложение ASP.NET**, введите **BookStore** для имени проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="fa296-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="fa296-125">Когда **новый проект ASP.NET** отображается диалоговое окно, выберите пункт **веб-API** шаблона, а затем щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="fa296-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="fa296-126">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="fa296-126">Click image to expand</span></span>                                                                |


6. <span data-ttu-id="fa296-127">Когда откроется проект веб-API, удалите пример контроллера из проекта:</span><span class="sxs-lookup"><span data-stu-id="fa296-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="fa296-128">Разверните **контроллеров** папку в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="fa296-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="fa296-129">Щелкните правой кнопкой мыши **ValuesController.cs** файла и нажмите кнопку **удалить**.</span><span class="sxs-lookup"><span data-stu-id="fa296-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="fa296-130">Нажмите кнопку **ОК** при появлении запроса на подтверждение удаления.</span><span class="sxs-lookup"><span data-stu-id="fa296-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="fa296-131">Добавление данных XML-файл в проект веб-API; Этот файл содержит содержимое каталога книжного магазина:</span><span class="sxs-lookup"><span data-stu-id="fa296-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="fa296-132">Щелкните правой кнопкой мыши **приложения\_данных** папку в обозревателе решений, затем нажмите кнопку **добавить**, а затем нажмите кнопку **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="fa296-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="fa296-133">Когда **Добавление нового элемента** диалоговое окно, выделите **XML-файл** шаблона.</span><span class="sxs-lookup"><span data-stu-id="fa296-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="fa296-134">Назовите файл **Books.xml**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="fa296-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="fa296-135">Когда **Books.xml** открывается файл, замените код в файле XML из примера **books.xml** файла на сайте MSDN:</span><span class="sxs-lookup"><span data-stu-id="fa296-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="fa296-136">Сохраните и закройте XML-файле.</span><span class="sxs-lookup"><span data-stu-id="fa296-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="fa296-137">Добавление bookstore модели в проект веб-API; Эта модель содержит логику создания, чтения, обновления и удаления (CRUD) для приложения книжного магазина:</span><span class="sxs-lookup"><span data-stu-id="fa296-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="fa296-138">Щелкните правой кнопкой мыши **моделей** папку в обозревателе решений, затем нажмите кнопку **добавить**, а затем нажмите кнопку **класс**.</span><span class="sxs-lookup"><span data-stu-id="fa296-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="fa296-139">При **Добавление нового элемента** диалоговое окно, назовите файл класса **BookDetails.cs**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="fa296-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="fa296-140">Когда **BookDetails.cs** открывается файл, замените код в файле следующим:</span><span class="sxs-lookup"><span data-stu-id="fa296-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="fa296-141">Сохраните и закройте **BookDetails.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="fa296-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="fa296-142">Добавьте в проект веб-API bookstore контроллера:</span><span class="sxs-lookup"><span data-stu-id="fa296-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="fa296-143">Щелкните правой кнопкой мыши **контроллеров** папку в обозревателе решений, затем нажмите кнопку **добавить**, а затем нажмите кнопку **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="fa296-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="fa296-144">При **Добавление шаблона** диалоговое окно, выделите **контроллер Web API 2 — пустой**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="fa296-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="fa296-145">При **Добавление контроллера** диалоговое окно, назовите контроллер **BooksController**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="fa296-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="fa296-146">Когда **BooksController.cs** открывается файл, замените код в файле следующим:</span><span class="sxs-lookup"><span data-stu-id="fa296-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="fa296-147">Сохраните и закройте **BooksController.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="fa296-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="fa296-148">Постройте приложение веб-API для проверки ошибок.</span><span class="sxs-lookup"><span data-stu-id="fa296-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="fa296-149">Шаг 2: Добавление проекта каталога книжный магазин Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="fa296-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="fa296-150">Следующий шаг в этом сценарии end-to-end — создать каталог приложения для Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="fa296-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="fa296-151">Это приложение будет использовать *Windows Phone с привязкой к данным приложения* шаблон для пользовательского интерфейса по умолчанию, который будет использовать приложение веб-API, который был создан в [шаг 1](#STEP1) этого учебника в качестве источника данных.</span><span class="sxs-lookup"><span data-stu-id="fa296-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="fa296-152">Щелкните правой кнопкой мыши **BookStore** решение в в обозревателе решений, затем нажмите кнопку **добавить**, а затем **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="fa296-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="fa296-153">Когда **новый проект** диалоговое окно отображается, разверните **установленные**, затем **Visual C#**, а затем **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="fa296-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="fa296-154">Выделите **Windows Phone с привязкой к данным приложения**, введите **BookCatalog** имя, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="fa296-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="fa296-155">Добавьте в пакет Json.NET NuGet **BookCatalog** проекта:</span><span class="sxs-lookup"><span data-stu-id="fa296-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="fa296-156">Щелкните правой кнопкой мыши **ссылки** для **BookCatalog** проекта в обозревателе решений, а затем нажмите кнопку **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fa296-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="fa296-157">Когда **управление пакетами NuGet** диалоговое окно отображается, разверните **Online** , при этом выделите **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="fa296-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="fa296-158">Введите **Json.NET** при поиске поле и щелкните значок поиска.</span><span class="sxs-lookup"><span data-stu-id="fa296-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="fa296-159">Выделите **Json.NET** в результаты поиска, а затем выберите **установить**.</span><span class="sxs-lookup"><span data-stu-id="fa296-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="fa296-160">После завершения установки, нажмите кнопку **закрыть**.</span><span class="sxs-lookup"><span data-stu-id="fa296-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="fa296-161">Добавить **BookDetails** модели **BookCatalog** проекта; этот файл содержит общую модель класса книжного магазина:</span><span class="sxs-lookup"><span data-stu-id="fa296-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="fa296-162">Щелкните правой кнопкой мыши **BookCatalog** проекта в обозревателе решений, а затем щелкните **добавить**, а затем нажмите кнопку **новую папку**.</span><span class="sxs-lookup"><span data-stu-id="fa296-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="fa296-163">Назовите новую папку **моделей**.</span><span class="sxs-lookup"><span data-stu-id="fa296-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="fa296-164">Щелкните правой кнопкой мыши **моделей** папку в обозревателе решений, затем нажмите кнопку **добавить**, а затем нажмите кнопку **класс**.</span><span class="sxs-lookup"><span data-stu-id="fa296-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="fa296-165">При **Добавление нового элемента** диалоговое окно, назовите файл класса **BookDetails.cs**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="fa296-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="fa296-166">Когда **BookDetails.cs** открывается файл, замените код в файле следующим:</span><span class="sxs-lookup"><span data-stu-id="fa296-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="fa296-167">Сохраните и закройте **BookDetails.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="fa296-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="fa296-168">Обновление **MainViewModel.cs** класса для включения функции для взаимодействия с приложением BookStore веб-API:</span><span class="sxs-lookup"><span data-stu-id="fa296-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="fa296-169">Разверните **ViewModels** папку в обозревателе решений, а затем дважды щелкните пункт **MainViewModel.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="fa296-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="fa296-170">Когда **MainViewModel.cs** файл открыт, замените код в файле следующим; Обратите внимание на то, что необходимо будет обновить значение `apiUrl` константы фактический URL-адрес веб-API:</span><span class="sxs-lookup"><span data-stu-id="fa296-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="fa296-171">Сохраните и закройте **MainViewModel.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="fa296-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="fa296-172">Обновление **MainPage.xaml** файл, чтобы настроить имя приложения:</span><span class="sxs-lookup"><span data-stu-id="fa296-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="fa296-173">Дважды щелкните **MainPage.xaml** файл в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="fa296-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="fa296-174">Когда **MainPage.xaml** файл открыт, найдите следующие строки кода:</span><span class="sxs-lookup"><span data-stu-id="fa296-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="fa296-175">Замените эти строки следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="fa296-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="fa296-176">Сохраните и закройте **MainPage.xaml** файла.</span><span class="sxs-lookup"><span data-stu-id="fa296-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="fa296-177">Обновление **DetailsPage.xaml** файл, чтобы настроить отображаемые элементы:</span><span class="sxs-lookup"><span data-stu-id="fa296-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="fa296-178">Дважды щелкните **DetailsPage.xaml** файл в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="fa296-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="fa296-179">Когда **DetailsPage.xaml** файл открыт, найдите следующие строки кода:</span><span class="sxs-lookup"><span data-stu-id="fa296-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="fa296-180">Замените эти строки следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="fa296-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="fa296-181">Сохраните и закройте **DetailsPage.xaml** файла.</span><span class="sxs-lookup"><span data-stu-id="fa296-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="fa296-182">Создание приложения Windows Phone на наличие ошибок.</span><span class="sxs-lookup"><span data-stu-id="fa296-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="fa296-183">Шаг 3: Тестирование решения End-to-End</span><span class="sxs-lookup"><span data-stu-id="fa296-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="fa296-184">Как упоминалось в *предварительные требования* раздела этого учебника, при тестировании сетевое подключение между веб-API и Windows Phone 8 проектов в локальной системе, вы должны следовать инструкциям *[ Подключение к веб-API приложения на локальном компьютере в эмуляторе Windows Phone 8](https://go.microsoft.com/fwlink/?LinkId=324014)* статью, чтобы настроить среду тестирования.</span><span class="sxs-lookup"><span data-stu-id="fa296-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="fa296-185">После настройки среды тестирования, необходимо установить как запускаемый проект приложения Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="fa296-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="fa296-186">Чтобы сделать это, выделите **BookCatalog** приложения в обозревателе решений и выберите пункт **Назначить запускаемым проектом**:</span><span class="sxs-lookup"><span data-stu-id="fa296-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="fa296-187">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="fa296-187">Click image to expand</span></span> |

<span data-ttu-id="fa296-188">При нажатии клавиши F5, Visual Studio запустит обоих Windows Phone эмулятор, который будет отображаться &quot;Подождите&quot; сообщения во время извлечения данных приложением из веб-API:</span><span class="sxs-lookup"><span data-stu-id="fa296-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="fa296-189">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="fa296-189">Click image to expand</span></span> |

<span data-ttu-id="fa296-190">Если все успешно, вы увидите, что отображается каталог:</span><span class="sxs-lookup"><span data-stu-id="fa296-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="fa296-191">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="fa296-191">Click image to expand</span></span> |

<span data-ttu-id="fa296-192">Если коснуться любой название книги, приложение будет отображать название книги:</span><span class="sxs-lookup"><span data-stu-id="fa296-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="fa296-193">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="fa296-193">Click image to expand</span></span> |

<span data-ttu-id="fa296-194">Если приложение не может обмениваться данными с веб-API, отображается сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="fa296-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="fa296-195">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="fa296-195">Click image to expand</span></span> |

<span data-ttu-id="fa296-196">Если коснуться сообщение об ошибке, отобразится дополнительная информация об ошибке:</span><span class="sxs-lookup"><span data-stu-id="fa296-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="fa296-197">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="fa296-197">Click image to expand</span></span>                                                                 |

