---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: "Вызов веб-API из Windows Phone 8 приложения (C#) | Документы Microsoft"
author: rmcmurray
description: "Создание полного сценария начала до конца, состоящий из приложения веб-API ASP.NET, которое содержит каталог книг для приложения Windows Phone 8."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 2025f31f369153b93cd293884880c97635fc8ab8
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="beff1-103">Вызов веб-API из приложения Windows Phone 8 (C#)</span><span class="sxs-lookup"><span data-stu-id="beff1-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="beff1-104">по [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="beff1-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="beff1-105">В этом учебнике вы узнаете, как создать полного сценария начала до конца, состоящий из приложения веб-API ASP.NET, которое содержит каталог книг для приложения Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="beff1-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="beff1-106">Обзор</span><span class="sxs-lookup"><span data-stu-id="beff1-106">Overview</span></span>

<span data-ttu-id="beff1-107">RESTful служб, таких как веб-API ASP.NET упрощают создание приложений на основе HTTP для разработчиков посредством абстрагирования архитектуры для серверных и клиентских приложений.</span><span class="sxs-lookup"><span data-stu-id="beff1-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="beff1-108">Вместо создания связи собственный протокол на основе сокетов, разработчиков веб-API просто должен быть опубликован необходимые методы HTTP для своего приложения (например: GET, POST, PUT, DELETE), и только нужно использовать разработчики клиентских приложений методы HTTP, которые необходимы для своего приложения.</span><span class="sxs-lookup"><span data-stu-id="beff1-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="beff1-109">В этом учебнике начала до конца вы узнаете, как использовать веб-API для создания следующих проектов:</span><span class="sxs-lookup"><span data-stu-id="beff1-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="beff1-110">В [первой части этого учебника](#STEP1), вы создадите приложение веб-API ASP.NET, которое поддерживает все операции создания, чтения, обновления и удаления (CRUD) для управления каталогом книги.</span><span class="sxs-lookup"><span data-stu-id="beff1-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="beff1-111">Это приложение будет использовать [пример XML-файла (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) с MSDN.</span><span class="sxs-lookup"><span data-stu-id="beff1-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="beff1-112">В [второй части этого учебника](#STEP2), вы создадите интерактивным приложением Windows Phone 8, извлекает данные из приложения веб-API.</span><span class="sxs-lookup"><span data-stu-id="beff1-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="beff1-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="beff1-113">Prerequisites</span></span>

- <span data-ttu-id="beff1-114">Visual Studio 2013 с Windows Phone 8 установленный пакет SDK</span><span class="sxs-lookup"><span data-stu-id="beff1-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="beff1-115">Windows 8 или более поздней версии на 64-разрядной системы с Hyper-V установлено</span><span class="sxs-lookup"><span data-stu-id="beff1-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="beff1-116">Список дополнительных требованиях см. в разделе *требования к системе для* статьи на [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) странице загрузки.</span><span class="sxs-lookup"><span data-stu-id="beff1-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="beff1-117">Если проверка подключения между веб-API и проектов Windows Phone 8 в локальной системе, необходимо будет следуйте инструкциям в  *[подключение к веб-API приложения на локальном эмуляторе Windows Phone 8 Компьютер](https://go.microsoft.com/fwlink/?LinkId=324014)*  статьи для настройки тестовой среды.</span><span class="sxs-lookup"><span data-stu-id="beff1-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="beff1-118">Шаг 1: Создание веб-проекта API Bookstore</span><span class="sxs-lookup"><span data-stu-id="beff1-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="beff1-119">Первым шагом данного руководства конца в конец является создание проекта веб-API, который поддерживает все операции CRUD; Обратите внимание, что вы добавите проект приложения Windows Phone в это решение в [шаг 2](#STEP2) этого учебника.</span><span class="sxs-lookup"><span data-stu-id="beff1-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="beff1-120">Откройте **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="beff1-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="beff1-121">Нажмите кнопку **файл**, затем **новый**, а затем **проекта**.</span><span class="sxs-lookup"><span data-stu-id="beff1-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="beff1-122">При **новый проект** диалоговое окно отображается, разверните **установленные**, затем **шаблоны**, затем **Visual C#**, а затем **Web**.</span><span class="sxs-lookup"><span data-stu-id="beff1-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>

    | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
    | --- |
    | <span data-ttu-id="beff1-123">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="beff1-123">Click image to expand</span></span> |
4. <span data-ttu-id="beff1-124">Выделите **веб-приложение ASP.NET**, введите **BookStore** для имени проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="beff1-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="beff1-125">Когда **новый проект ASP.NET** отображается диалоговое окно, выберите пункт **веб-API** шаблона и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="beff1-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>

    | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
    | --- |
    | <span data-ttu-id="beff1-126">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="beff1-126">Click image to expand</span></span> |
6. <span data-ttu-id="beff1-127">При открытии проекта веб-API, удалите из проекта пример контроллера:</span><span class="sxs-lookup"><span data-stu-id="beff1-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="beff1-128">Разверните **контроллеров** папки в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="beff1-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="beff1-129">Щелкните правой кнопкой мыши **ValuesController.cs** файла и нажмите кнопку **удалить**.</span><span class="sxs-lookup"><span data-stu-id="beff1-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="beff1-130">Нажмите кнопку **ОК** при появлении запроса на подтверждение удаления.</span><span class="sxs-lookup"><span data-stu-id="beff1-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="beff1-131">Добавьте файл XML-данных в проект веб-API; Этот файл содержит содержимое каталога bookstore.</span><span class="sxs-lookup"><span data-stu-id="beff1-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

    1. <span data-ttu-id="beff1-132">Щелкните правой кнопкой мыши **приложения\_данные** папки в обозревателе решений щелкните **добавить**и нажмите кнопку **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="beff1-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
    2. <span data-ttu-id="beff1-133">Когда **Добавление нового элемента** диалоговое окно, выделите **XML-файл** шаблона.</span><span class="sxs-lookup"><span data-stu-id="beff1-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
    3. <span data-ttu-id="beff1-134">Назовите файл **Books.xml**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="beff1-134">Name the file **Books.xml**, and then click **Add**.</span></span>
    4. <span data-ttu-id="beff1-135">Когда **Books.xml** открыть файл, замените код в файле XML-кодом из примера **books.xml** файла на сайте MSDN:</span><span class="sxs-lookup"><span data-stu-id="beff1-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
    5. <span data-ttu-id="beff1-136">Сохраните и закройте XML-файл.</span><span class="sxs-lookup"><span data-stu-id="beff1-136">Save and close the XML file.</span></span>
8. <span data-ttu-id="beff1-137">Добавить модель bookstore проект веб-API; Эта модель содержит логику для приложение bookstore создания, чтения, обновления и удаления (CRUD):</span><span class="sxs-lookup"><span data-stu-id="beff1-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

    1. <span data-ttu-id="beff1-138">Щелкните правой кнопкой мыши **моделей** папки в обозревателе решений щелкните **добавить**и нажмите кнопку **класса**.</span><span class="sxs-lookup"><span data-stu-id="beff1-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
    2. <span data-ttu-id="beff1-139">При **Добавление нового элемента** диалоговое окно, введите имя файла класса **BookDetails.cs**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="beff1-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
    3. <span data-ttu-id="beff1-140">Когда **BookDetails.cs** открыть файл, замените код в файле следующим:</span><span class="sxs-lookup"><span data-stu-id="beff1-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
    4. <span data-ttu-id="beff1-141">Сохраните и закройте **BookDetails.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="beff1-141">Save and close the **BookDetails.cs** file.</span></span>
9. <span data-ttu-id="beff1-142">Добавьте в проект веб-API bookstore контроллера:</span><span class="sxs-lookup"><span data-stu-id="beff1-142">Add the bookstore controller to the Web API project:</span></span>

    1. <span data-ttu-id="beff1-143">Щелкните правой кнопкой мыши **контроллеров** папки в обозревателе решений щелкните **добавить**и нажмите кнопку **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="beff1-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
    2. <span data-ttu-id="beff1-144">При **Добавление формирования шаблонов** диалоговое окно, выделите **контроллер 2 API веб - пустой**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="beff1-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
    3. <span data-ttu-id="beff1-145">При **добавления контроллера** диалоговое окно, введите имя контроллера **BooksController**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="beff1-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
    4. <span data-ttu-id="beff1-146">Когда **BooksController.cs** открыть файл, замените код в файле следующим:</span><span class="sxs-lookup"><span data-stu-id="beff1-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
    5. <span data-ttu-id="beff1-147">Сохраните и закройте **BooksController.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="beff1-147">Save and close the **BooksController.cs** file.</span></span>
10. <span data-ttu-id="beff1-148">Постройте приложение веб-API для проверки ошибок.</span><span class="sxs-lookup"><span data-stu-id="beff1-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="beff1-149">Шаг 2: Добавление проекта Windows Phone 8 Bookstore каталога</span><span class="sxs-lookup"><span data-stu-id="beff1-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="beff1-150">Этот сценарий конца в конец Далее необходимо для создания каталога приложения для Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="beff1-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="beff1-151">Это приложение будет использовать *с привязкой к данным приложения Windows Phone* шаблон для пользовательского интерфейса по умолчанию, который будет использовать веб-API приложения, созданный в [шаг 1](#STEP1) этого учебника в качестве источника данных.</span><span class="sxs-lookup"><span data-stu-id="beff1-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="beff1-152">Щелкните правой кнопкой мыши **BookStore** в решение в обозревателе решений, а затем нажмите **добавить**, а затем **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="beff1-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="beff1-153">При **новый проект** диалоговое окно отображается, разверните **установленные**, затем **Visual C#**, а затем **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="beff1-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="beff1-154">Выделите **с привязкой к данным приложения Windows Phone**, введите **BookCatalog** для имени и затем **ОК**.</span><span class="sxs-lookup"><span data-stu-id="beff1-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="beff1-155">Добавьте в пакет Json.NET NuGet **BookCatalog** проекта:</span><span class="sxs-lookup"><span data-stu-id="beff1-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="beff1-156">Щелкните правой кнопкой мыши **ссылки** для **BookCatalog** проекта в обозревателе решений и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="beff1-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="beff1-157">Когда **управление пакетами NuGet** диалоговое окно отображается, разверните **Online** раздела и выделите **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="beff1-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="beff1-158">Введите **Json.NET** в поиск поля и щелкните значок поиска.</span><span class="sxs-lookup"><span data-stu-id="beff1-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="beff1-159">Выделите **Json.NET** в результаты поиска, а затем выберите **установить**.</span><span class="sxs-lookup"><span data-stu-id="beff1-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="beff1-160">После завершения установки, нажмите кнопку **закрыть**.</span><span class="sxs-lookup"><span data-stu-id="beff1-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="beff1-161">Добавить **BookDetails** модели **BookCatalog** проекта; содержит общую модель bookstore класса:</span><span class="sxs-lookup"><span data-stu-id="beff1-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

    1. <span data-ttu-id="beff1-162">Щелкните правой кнопкой мыши **BookCatalog** проекта в обозревателе решений, а затем нажмите кнопку **добавить**, а затем нажмите кнопку **новую папку**.</span><span class="sxs-lookup"><span data-stu-id="beff1-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
    2. <span data-ttu-id="beff1-163">Имя новой папки **моделей**.</span><span class="sxs-lookup"><span data-stu-id="beff1-163">Name the new folder **Models**.</span></span>
    3. <span data-ttu-id="beff1-164">Щелкните правой кнопкой мыши **моделей** папки в обозревателе решений щелкните **добавить**и нажмите кнопку **класса**.</span><span class="sxs-lookup"><span data-stu-id="beff1-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
    4. <span data-ttu-id="beff1-165">При **Добавление нового элемента** диалоговое окно, введите имя файла класса **BookDetails.cs**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="beff1-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
    5. <span data-ttu-id="beff1-166">Когда **BookDetails.cs** открыть файл, замените код в файле следующим:</span><span class="sxs-lookup"><span data-stu-id="beff1-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
    6. <span data-ttu-id="beff1-167">Сохраните и закройте **BookDetails.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="beff1-167">Save and close the **BookDetails.cs** file.</span></span>
6. <span data-ttu-id="beff1-168">Обновление **MainViewModel.cs** класса для включения функции для взаимодействия с приложением BookStore веб-API:</span><span class="sxs-lookup"><span data-stu-id="beff1-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

    1. <span data-ttu-id="beff1-169">Разверните **ViewModels** папки в обозревателе решений, а затем дважды щелкните **MainViewModel.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="beff1-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
    2. <span data-ttu-id="beff1-170">Когда **MainViewModel.cs** файл открыт, замените код в файле следующим; Обратите внимание, что необходимо обновить значение `apiUrl` постоянное фактический URL-адрес веб-API:</span><span class="sxs-lookup"><span data-stu-id="beff1-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
    3. <span data-ttu-id="beff1-171">Сохраните и закройте **MainViewModel.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="beff1-171">Save and close the **MainViewModel.cs** file.</span></span>
7. <span data-ttu-id="beff1-172">Обновление **MainPage.xaml** файл, чтобы настроить имя приложения:</span><span class="sxs-lookup"><span data-stu-id="beff1-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

    1. <span data-ttu-id="beff1-173">Дважды щелкните **MainPage.xaml** файл в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="beff1-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
    2. <span data-ttu-id="beff1-174">Когда **MainPage.xaml** файл открыт, найдите следующие строки кода:</span><span class="sxs-lookup"><span data-stu-id="beff1-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
    3. <span data-ttu-id="beff1-175">Замените эти строки со следующими:</span><span class="sxs-lookup"><span data-stu-id="beff1-175">Replace those lines with the following:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
    4. <span data-ttu-id="beff1-176">Сохраните и закройте **MainPage.xaml** файла.</span><span class="sxs-lookup"><span data-stu-id="beff1-176">Save and close the **MainPage.xaml** file.</span></span>
8. <span data-ttu-id="beff1-177">Обновление **DetailsPage.xaml** файл, чтобы настроить отображаемые элементы:</span><span class="sxs-lookup"><span data-stu-id="beff1-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

    1. <span data-ttu-id="beff1-178">Дважды щелкните **DetailsPage.xaml** файл в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="beff1-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
    2. <span data-ttu-id="beff1-179">Когда **DetailsPage.xaml** файл открыт, найдите следующие строки кода:</span><span class="sxs-lookup"><span data-stu-id="beff1-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
    3. <span data-ttu-id="beff1-180">Замените эти строки со следующими:</span><span class="sxs-lookup"><span data-stu-id="beff1-180">Replace those lines with the following:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
    4. <span data-ttu-id="beff1-181">Сохраните и закройте **DetailsPage.xaml** файла.</span><span class="sxs-lookup"><span data-stu-id="beff1-181">Save and close the **DetailsPage.xaml** file.</span></span>
9. <span data-ttu-id="beff1-182">Создание приложения Windows Phone, проверка на наличие ошибок.</span><span class="sxs-lookup"><span data-stu-id="beff1-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="beff1-183">Шаг 3: Тестирование решения конца в конец</span><span class="sxs-lookup"><span data-stu-id="beff1-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="beff1-184">Как упоминалось в *необходимые компоненты* проекты разделе этого учебника, при тестировании подключения между веб-API и Windows Phone 8 в локальной системе, вы должны будете следовать инструкциям в разделе  *[ Подключение к веб-API приложения на локальном компьютере в эмуляторе Windows Phone 8](https://go.microsoft.com/fwlink/?LinkId=324014)*  статьи для настройки тестовой среды.</span><span class="sxs-lookup"><span data-stu-id="beff1-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="beff1-185">После настройки среды тестирования, необходимо установить как запускаемый проект приложения Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="beff1-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="beff1-186">Чтобы сделать это, выделите **BookCatalog** приложения в обозревателе решений, а затем выберите **Назначить запускаемым проектом**:</span><span class="sxs-lookup"><span data-stu-id="beff1-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="beff1-187">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="beff1-187">Click image to expand</span></span> |

<span data-ttu-id="beff1-188">При нажатии клавиши F5, Visual Studio запускает оба Windows Phone эмулятор, который будет отображать &quot;Подождите&quot; сообщения при получении данных приложения из веб-API:</span><span class="sxs-lookup"><span data-stu-id="beff1-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="beff1-189">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="beff1-189">Click image to expand</span></span> |

<span data-ttu-id="beff1-190">Если все успешно, вы увидите каталог отображается:</span><span class="sxs-lookup"><span data-stu-id="beff1-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="beff1-191">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="beff1-191">Click image to expand</span></span> |

<span data-ttu-id="beff1-192">Если коснуться любой название книги, в приложении отображается описание книги:</span><span class="sxs-lookup"><span data-stu-id="beff1-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="beff1-193">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="beff1-193">Click image to expand</span></span> |

<span data-ttu-id="beff1-194">Если приложение не может взаимодействовать с веб-API, отображается сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="beff1-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="beff1-195">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="beff1-195">Click image to expand</span></span> |

<span data-ttu-id="beff1-196">Если коснуться сообщение об ошибке, будет отображаться дополнительная информация об ошибке:</span><span class="sxs-lookup"><span data-stu-id="beff1-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
| --- |
| <span data-ttu-id="beff1-197">Щелкните изображение, чтобы развернуть</span><span class="sxs-lookup"><span data-stu-id="beff1-197">Click image to expand</span></span> |
