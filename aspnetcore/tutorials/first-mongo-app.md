---
title: Сборка веб-API с использованием ASP.NET Core и MongoDB
author: prkhandelwal
description: В руководстве показано, как выполнять сборку веб-API ASP.NET Core с помощью базы данных NoSQL MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 11/29/2018
uid: tutorials/first-mongo-app
ms.openlocfilehash: bd9a36c5eb06542c820e71e937b8da10f735a0f8
ms.sourcegitcommit: 68a3081dd175d6518d1bfa31b4712bd8a2dd3864
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/18/2018
ms.locfileid: "53577842"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="32fdf-103">Создание веб-API с помощью ASP.NET Core и MongoDB</span><span class="sxs-lookup"><span data-stu-id="32fdf-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="32fdf-104">Авторы [Пратик Ханделвал ](https://twitter.com/K2Prk) (Pratik Khandelwal) и [Скотт Эдди](https://twitter.com/Scott_Addie) (Scott Addie).</span><span class="sxs-lookup"><span data-stu-id="32fdf-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="32fdf-105">В этом руководстве описано, как создать веб-API, который выполняет операции создания, чтения, обновления и удаления (CRUD) в базе данных NoSQL [MongoDB](https://www.mongodb.com/what-is-mongodb).</span><span class="sxs-lookup"><span data-stu-id="32fdf-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="32fdf-106">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="32fdf-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="32fdf-107">Настройка MongoDB</span><span class="sxs-lookup"><span data-stu-id="32fdf-107">Configure MongoDB</span></span>
> * <span data-ttu-id="32fdf-108">создать базу данных MongoDB;</span><span class="sxs-lookup"><span data-stu-id="32fdf-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="32fdf-109">определить коллекцию и схему MongoDB;</span><span class="sxs-lookup"><span data-stu-id="32fdf-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="32fdf-110">выполнить операции CRUD MongoDB из веб-API.</span><span class="sxs-lookup"><span data-stu-id="32fdf-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="32fdf-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="32fdf-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="32fdf-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="32fdf-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32fdf-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32fdf-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="32fdf-114">Пакет SDK для .NET Core 2.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="32fdf-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="32fdf-115">[Visual Studio 2017 15.9 или более поздней версии](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="32fdf-115">[Visual Studio 2017 version 15.9 or later](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="32fdf-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="32fdf-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="32fdf-117">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="32fdf-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="32fdf-118">Пакет SDK для .NET Core 2.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="32fdf-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="32fdf-119">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="32fdf-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="32fdf-120">C# для Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="32fdf-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="32fdf-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="32fdf-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="32fdf-122">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="32fdf-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="32fdf-123">Пакет SDK для .NET Core 2.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="32fdf-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="32fdf-124">Visual Studio для Mac 7.7 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="32fdf-124">Visual Studio for Mac version 7.7 or later</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="32fdf-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="32fdf-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="32fdf-126">Настройка MongoDB</span><span class="sxs-lookup"><span data-stu-id="32fdf-126">Configure MongoDB</span></span>

<span data-ttu-id="32fdf-127">Если используется Windows, MongoDB по умолчанию устанавливается в папку *C:\Program Files\MongoDB*.</span><span class="sxs-lookup"><span data-stu-id="32fdf-127">If using Windows, MongoDB is installed at *C:\Program Files\MongoDB* by default.</span></span> <span data-ttu-id="32fdf-128">В переменную среды `Path` добавьте *C:\Program Files\MongoDB\Server\<version_number>\bin*.</span><span class="sxs-lookup"><span data-stu-id="32fdf-128">Add *C:\Program Files\MongoDB\Server\<version_number>\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="32fdf-129">Это изменение обеспечит доступ к MongoDB из любого места на компьютере разработки.</span><span class="sxs-lookup"><span data-stu-id="32fdf-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="32fdf-130">В следующих шагах используйте интерфейс mongo Shell, чтобы создать базу данных и коллекции и сохранить документы.</span><span class="sxs-lookup"><span data-stu-id="32fdf-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="32fdf-131">Дополнительные сведения о командах mongo Shell см. в руководстве по [работе с mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="32fdf-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="32fdf-132">Выберите папку на компьютере разработки для хранения данных.</span><span class="sxs-lookup"><span data-stu-id="32fdf-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="32fdf-133">Например *C:\BooksData* при работе в Windows.</span><span class="sxs-lookup"><span data-stu-id="32fdf-133">For example, *C:\BooksData* on Windows.</span></span> <span data-ttu-id="32fdf-134">Если такого каталога нет, создайте его.</span><span class="sxs-lookup"><span data-stu-id="32fdf-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="32fdf-135">В mongo Shell нельзя создавать каталоги.</span><span class="sxs-lookup"><span data-stu-id="32fdf-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="32fdf-136">Откройте командную оболочку.</span><span class="sxs-lookup"><span data-stu-id="32fdf-136">Open a command shell.</span></span> <span data-ttu-id="32fdf-137">Выполните следующую команду для подключения к MongoDB через порт 27017, заданный по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="32fdf-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="32fdf-138">Не забудьте заменить `<data_directory_path>` каталогом, созданным на предыдущем этапе.</span><span class="sxs-lookup"><span data-stu-id="32fdf-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="32fdf-139">Откройте другой экземпляр командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="32fdf-139">Open another command shell instance.</span></span> <span data-ttu-id="32fdf-140">Подключитесь к тестовой базе данных по умолчанию, выполнив такую команду:</span><span class="sxs-lookup"><span data-stu-id="32fdf-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="32fdf-141">Запустите в командной оболочке следующее:</span><span class="sxs-lookup"><span data-stu-id="32fdf-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="32fdf-142">Будет создана база данных с именем *BookstoreDb*, если она не существует.</span><span class="sxs-lookup"><span data-stu-id="32fdf-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="32fdf-143">Если такая база данных существует, для нее уже установлено подключение для транзакций.</span><span class="sxs-lookup"><span data-stu-id="32fdf-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="32fdf-144">Создайте коллекцию `Books` с помощью такой команды:</span><span class="sxs-lookup"><span data-stu-id="32fdf-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="32fdf-145">Отобразится такой результат:</span><span class="sxs-lookup"><span data-stu-id="32fdf-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="32fdf-146">Определите схему для коллекции `Books` и вставьте два документа, используя следующую команду:</span><span class="sxs-lookup"><span data-stu-id="32fdf-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="32fdf-147">Отобразится такой результат:</span><span class="sxs-lookup"><span data-stu-id="32fdf-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="32fdf-148">Просмотрите документы в базе данных, используя такую команду:</span><span class="sxs-lookup"><span data-stu-id="32fdf-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="32fdf-149">Отобразится такой результат:</span><span class="sxs-lookup"><span data-stu-id="32fdf-149">The following result is displayed:</span></span>

    ```console
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215d"),
      "Name" : "Design Patterns",
      "Price" : 54.93,
      "Category" : "Computers",
      "Author" : "Ralph Johnson"
    }
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215e"),
      "Name" : "Clean Code",
      "Price" : 43.15,
      "Category" : "Computers",
      "Author" : "Robert C. Martin"
    }
    ```

    <span data-ttu-id="32fdf-150">Схема добавляет автоматически созданное свойство `_id` типа `ObjectId` к каждому документу.</span><span class="sxs-lookup"><span data-stu-id="32fdf-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="32fdf-151">База данных готова к работе.</span><span class="sxs-lookup"><span data-stu-id="32fdf-151">The database is ready.</span></span> <span data-ttu-id="32fdf-152">Можно приступить к созданию веб-API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32fdf-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="32fdf-153">Создание проекта веб-API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32fdf-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32fdf-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32fdf-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="32fdf-155">Откройте **Файл** > **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="32fdf-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="32fdf-156">Выберите **Веб-приложение ASP.NET Core**, назовите проект *BooksApi* и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="32fdf-156">Select **ASP.NET Core Web Application**, name the project *BooksApi*, and click **OK**.</span></span>
1. <span data-ttu-id="32fdf-157">Выберите **.NET Core** требуемой версии .NET Framework и **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="32fdf-157">Select the **.NET Core** target framework and **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="32fdf-158">Выберите шаблон проекта **API** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="32fdf-158">Select the **API** project template, and click **OK**:</span></span>
1. <span data-ttu-id="32fdf-159">В окне **Консоль диспетчера пакетов** перейдите в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="32fdf-159">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="32fdf-160">Выполните следующую команду, чтобы установить драйвер .NET для MongoDB:</span><span class="sxs-lookup"><span data-stu-id="32fdf-160">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version 2.7.2
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="32fdf-161">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="32fdf-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="32fdf-162">В командной оболочке выполните такие команды:</span><span class="sxs-lookup"><span data-stu-id="32fdf-162">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="32fdf-163">Новый проект веб-API ASP.NET Core для работы с .NET Core будет создан и открыт в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="32fdf-163">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="32fdf-164">Нажмите **Yes** (Да), когда отобразится уведомление *Required assets to build and debug are missing from 'BooksApi'. Add them?* (В BooksApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?).</span><span class="sxs-lookup"><span data-stu-id="32fdf-164">Click **Yes** when the *Required assets to build and debug are missing from 'BooksApi'. Add them?* notification appears.</span></span>
1. <span data-ttu-id="32fdf-165">Откройте **Интегрированный терминал** и перейдите в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="32fdf-165">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="32fdf-166">Выполните следующую команду, чтобы установить драйвер .NET для MongoDB:</span><span class="sxs-lookup"><span data-stu-id="32fdf-166">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v 2.7.2
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="32fdf-167">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="32fdf-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="32fdf-168">Откройте **Файл** > **Создать решение** > **.NET Core** > **Приложение**.</span><span class="sxs-lookup"><span data-stu-id="32fdf-168">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="32fdf-169">Выберите шаблон проекта **Веб-API ASP.NET Core** для C# и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="32fdf-169">Select the **ASP.NET Core Web API** C# project template, and click **Next**.</span></span>
1. <span data-ttu-id="32fdf-170">В раскрывающемся списке **Требуемая версия .NET Framework** выберите **.NET Core 2.2** и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="32fdf-170">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and click **Next**.</span></span>
1. <span data-ttu-id="32fdf-171">Введите *BooksApi* в поле **Имя проекта** и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="32fdf-171">Enter *BooksApi* for the **Project Name**, and click **Create**.</span></span>
1. <span data-ttu-id="32fdf-172">На панели **Решение** щелкните правой кнопкой мыши узел проекта **Зависимости** и выберите **Добавить пакеты**.</span><span class="sxs-lookup"><span data-stu-id="32fdf-172">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="32fdf-173">Введите *MongoDB.Driver* в поле поиска, выберите пакет *MongoDB.Driver* и нажмите **Добавить пакет**.</span><span class="sxs-lookup"><span data-stu-id="32fdf-173">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and click **Add Package**.</span></span>
1. <span data-ttu-id="32fdf-174">Нажмите кнопку **Принимаю** в диалоговом окне **Принятие условий лицензионного соглашения**.</span><span class="sxs-lookup"><span data-stu-id="32fdf-174">Click the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-a-model"></a><span data-ttu-id="32fdf-175">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="32fdf-175">Add a model</span></span>

1. <span data-ttu-id="32fdf-176">Добавьте каталог *Models* в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="32fdf-176">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="32fdf-177">Добавьте класс `Book` в каталог *Models* с помощью такого кода:</span><span class="sxs-lookup"><span data-stu-id="32fdf-177">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

<span data-ttu-id="32fdf-178">В предыдущем классе требуется свойство `Id` для сопоставления объекта среды CLR с коллекцией MongoDB.</span><span class="sxs-lookup"><span data-stu-id="32fdf-178">In the preceding class, the `Id` property is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span> <span data-ttu-id="32fdf-179">Другие свойства в классе оформляются с использованием атрибута `[BsonElement]`.</span><span class="sxs-lookup"><span data-stu-id="32fdf-179">Other properties in the class are decorated with the `[BsonElement]` attribute.</span></span> <span data-ttu-id="32fdf-180">Значение атрибута представляет имя свойства в коллекции MongoDB.</span><span class="sxs-lookup"><span data-stu-id="32fdf-180">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-crud-operations-class"></a><span data-ttu-id="32fdf-181">Добавление класса операций CRUD</span><span class="sxs-lookup"><span data-stu-id="32fdf-181">Add a CRUD operations class</span></span>

1. <span data-ttu-id="32fdf-182">Добавьте каталог *Services* в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="32fdf-182">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="32fdf-183">Добавьте класс `BookService` в каталог *Services* с помощью такого кода:</span><span class="sxs-lookup"><span data-stu-id="32fdf-183">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. <span data-ttu-id="32fdf-184">Добавьте строку подключения MongoDB в файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="32fdf-184">Add the MongoDB connection string to *appsettings.json*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    <span data-ttu-id="32fdf-185">Обращение к предыдущему свойству `BookstoreDb` выполняется в конструкторе класса `BookService`.</span><span class="sxs-lookup"><span data-stu-id="32fdf-185">The preceding `BookstoreDb` property is accessed in the `BookService` class constructor.</span></span>

1. <span data-ttu-id="32fdf-186">Зарегистрируйте класс `BookService` в `Startup.ConfigureServices` с помощью системы внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="32fdf-186">In `Startup.ConfigureServices`, register the `BookService` class with the Dependency Injection system:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    <span data-ttu-id="32fdf-187">Это необходимо, чтобы включить поддержку внедрения через конструктор в используемые классы.</span><span class="sxs-lookup"><span data-stu-id="32fdf-187">The preceding service registration is necessary to support constructor injection in consuming classes.</span></span>

<span data-ttu-id="32fdf-188">Класс `BookService` использует следующие члены `MongoDB.Driver` для выполнения операций CRUD в базе данных:</span><span class="sxs-lookup"><span data-stu-id="32fdf-188">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="32fdf-189">`MongoClient` &ndash; считывает экземпляр сервера для выполнения операций с базой данных.</span><span class="sxs-lookup"><span data-stu-id="32fdf-189">`MongoClient` &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="32fdf-190">Конструктор этого класса предоставляет строку подключения MongoDB.</span><span class="sxs-lookup"><span data-stu-id="32fdf-190">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="32fdf-191">`IMongoDatabase` &ndash; представляет базу данных Mongo для выполнения операций.</span><span class="sxs-lookup"><span data-stu-id="32fdf-191">`IMongoDatabase` &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="32fdf-192">В этом руководстве используется универсальный метод `GetCollection<T>(collection)` в интерфейсе для получения доступа к данным в определенной коллекции.</span><span class="sxs-lookup"><span data-stu-id="32fdf-192">This tutorial uses the generic `GetCollection<T>(collection)` method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="32fdf-193">Операции CRUD могут выполняться с коллекцией после вызова этого метода.</span><span class="sxs-lookup"><span data-stu-id="32fdf-193">CRUD operations can be performed against the collection after this method is called.</span></span> <span data-ttu-id="32fdf-194">В вызове метода `GetCollection<T>(collection)`:</span><span class="sxs-lookup"><span data-stu-id="32fdf-194">In the `GetCollection<T>(collection)` method call:</span></span>
  * <span data-ttu-id="32fdf-195">`collection` представляет имя коллекции;</span><span class="sxs-lookup"><span data-stu-id="32fdf-195">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="32fdf-196">`T` представляет тип объекта среды CLR, хранящегося в коллекции;</span><span class="sxs-lookup"><span data-stu-id="32fdf-196">`T` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="32fdf-197">`GetCollection<T>(collection)` возвращает объект `MongoCollection`, представляющий коллекцию.</span><span class="sxs-lookup"><span data-stu-id="32fdf-197">`GetCollection<T>(collection)` returns a `MongoCollection` object representing the collection.</span></span> <span data-ttu-id="32fdf-198">В этом руководстве следующие методы вызываются для коллекции:</span><span class="sxs-lookup"><span data-stu-id="32fdf-198">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="32fdf-199">`Find<T>` &ndash; возвращает все документы в коллекции, соответствующие заданным критериям поиска.</span><span class="sxs-lookup"><span data-stu-id="32fdf-199">`Find<T>` &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="32fdf-200">`InsertOne` &ndash; вставляет предоставленный объект в виде нового документа в коллекции.</span><span class="sxs-lookup"><span data-stu-id="32fdf-200">`InsertOne` &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="32fdf-201">`ReplaceOne` &ndash; заменяет один документ, отвечающий заданным критериям поиска, предоставленным объектом.</span><span class="sxs-lookup"><span data-stu-id="32fdf-201">`ReplaceOne` &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>
* <span data-ttu-id="32fdf-202">`DeleteOne` &ndash; удаляет один документ, отвечающий заданным критериям поиска.</span><span class="sxs-lookup"><span data-stu-id="32fdf-202">`DeleteOne` &ndash; Deletes a single document matching the provided search criteria.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="32fdf-203">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="32fdf-203">Add a controller</span></span>

1. <span data-ttu-id="32fdf-204">Добавьте класс `BooksController` в каталог *Controllers* с помощью такого кода:</span><span class="sxs-lookup"><span data-stu-id="32fdf-204">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    <span data-ttu-id="32fdf-205">Предыдущий контроллер веб-API:</span><span class="sxs-lookup"><span data-stu-id="32fdf-205">The preceding web API controller:</span></span>

    * <span data-ttu-id="32fdf-206">использует класс `BookService` для выполнения операций CRUD;</span><span class="sxs-lookup"><span data-stu-id="32fdf-206">Uses the `BookService` class to perform CRUD operations.</span></span>
    * <span data-ttu-id="32fdf-207">содержит методы действий для поддержки запросов HTTP GET, POST, PUT и DELETE.</span><span class="sxs-lookup"><span data-stu-id="32fdf-207">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
1. <span data-ttu-id="32fdf-208">Выполните сборку и запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="32fdf-208">Build and run the app.</span></span>
1. <span data-ttu-id="32fdf-209">В браузере перейдите в `http://localhost:<port>/api/books`.</span><span class="sxs-lookup"><span data-stu-id="32fdf-209">Navigate to `http://localhost:<port>/api/books` in your browser.</span></span> <span data-ttu-id="32fdf-210">Отобразится такой ответ JSON:</span><span class="sxs-lookup"><span data-stu-id="32fdf-210">The following JSON response is displayed:</span></span>

    ```json
    [
      {
        "id":"5bfd996f7b8e48dc15ff215d",
        "bookName":"Design Patterns",
        "price":54.93,
        "category":"Computers",
        "author":"Ralph Johnson"
      },
      {
        "id":"5bfd996f7b8e48dc15ff215e",
        "bookName":"Clean Code",
        "price":43.15,
        "category":"Computers",
        "author":"Robert C. Martin"
      }
    ]
    ```

## <a name="next-steps"></a><span data-ttu-id="32fdf-211">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="32fdf-211">Next steps</span></span>

<span data-ttu-id="32fdf-212">Дополнительные сведения о сборке веб-API ASP.NET Core см. в этих статьях:</span><span class="sxs-lookup"><span data-stu-id="32fdf-212">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
