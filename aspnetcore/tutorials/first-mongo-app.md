---
title: Сборка веб-API с использованием ASP.NET Core и MongoDB
author: prkhandelwal
description: В руководстве показано, как выполнять сборку веб-API ASP.NET Core с помощью базы данных NoSQL MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 01/31/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: f593a8d2d06897736b12f49f25c6049ea994a88a
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610616"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="969bc-103">Создание веб-API с помощью ASP.NET Core и MongoDB</span><span class="sxs-lookup"><span data-stu-id="969bc-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="969bc-104">Авторы [Пратик Ханделвал ](https://twitter.com/K2Prk) (Pratik Khandelwal) и [Скотт Эдди](https://twitter.com/Scott_Addie) (Scott Addie).</span><span class="sxs-lookup"><span data-stu-id="969bc-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="969bc-105">В этом руководстве описано, как создать веб-API, который выполняет операции создания, чтения, обновления и удаления (CRUD) в базе данных NoSQL [MongoDB](https://www.mongodb.com/what-is-mongodb).</span><span class="sxs-lookup"><span data-stu-id="969bc-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="969bc-106">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="969bc-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="969bc-107">Настройка MongoDB</span><span class="sxs-lookup"><span data-stu-id="969bc-107">Configure MongoDB</span></span>
> * <span data-ttu-id="969bc-108">создать базу данных MongoDB;</span><span class="sxs-lookup"><span data-stu-id="969bc-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="969bc-109">определить коллекцию и схему MongoDB;</span><span class="sxs-lookup"><span data-stu-id="969bc-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="969bc-110">выполнить операции CRUD MongoDB из веб-API.</span><span class="sxs-lookup"><span data-stu-id="969bc-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="969bc-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="969bc-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="969bc-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="969bc-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="969bc-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="969bc-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="969bc-114">Пакет SDK для .NET Core 2.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="969bc-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="969bc-115">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="969bc-115">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="969bc-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="969bc-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="969bc-117">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="969bc-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="969bc-118">Пакет SDK для .NET Core 2.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="969bc-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="969bc-119">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="969bc-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="969bc-120">C# для Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="969bc-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="969bc-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="969bc-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="969bc-122">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="969bc-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="969bc-123">Пакет SDK для .NET Core 2.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="969bc-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="969bc-124">Visual Studio для Mac 7.7 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="969bc-124">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="969bc-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="969bc-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="969bc-126">Настройка MongoDB</span><span class="sxs-lookup"><span data-stu-id="969bc-126">Configure MongoDB</span></span>

<span data-ttu-id="969bc-127">Если используется Windows, MongoDB по умолчанию устанавливается в папку *C:\\Program Files\\MongoDB*.</span><span class="sxs-lookup"><span data-stu-id="969bc-127">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="969bc-128">Добавьте *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* в переменную среды `Path`.</span><span class="sxs-lookup"><span data-stu-id="969bc-128">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="969bc-129">Это изменение обеспечит доступ к MongoDB из любого места на компьютере разработки.</span><span class="sxs-lookup"><span data-stu-id="969bc-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="969bc-130">В следующих шагах используйте интерфейс mongo Shell, чтобы создать базу данных и коллекции и сохранить документы.</span><span class="sxs-lookup"><span data-stu-id="969bc-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="969bc-131">Дополнительные сведения о командах mongo Shell см. в руководстве по [работе с mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="969bc-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="969bc-132">Выберите папку на компьютере разработки для хранения данных.</span><span class="sxs-lookup"><span data-stu-id="969bc-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="969bc-133">Например, *C:\\BooksData* при работе в Windows.</span><span class="sxs-lookup"><span data-stu-id="969bc-133">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="969bc-134">Если такого каталога нет, создайте его.</span><span class="sxs-lookup"><span data-stu-id="969bc-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="969bc-135">В mongo Shell нельзя создавать каталоги.</span><span class="sxs-lookup"><span data-stu-id="969bc-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="969bc-136">Откройте командную оболочку.</span><span class="sxs-lookup"><span data-stu-id="969bc-136">Open a command shell.</span></span> <span data-ttu-id="969bc-137">Выполните следующую команду для подключения к MongoDB через порт 27017, заданный по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="969bc-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="969bc-138">Не забудьте заменить `<data_directory_path>` каталогом, созданным на предыдущем этапе.</span><span class="sxs-lookup"><span data-stu-id="969bc-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="969bc-139">Откройте другой экземпляр командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="969bc-139">Open another command shell instance.</span></span> <span data-ttu-id="969bc-140">Подключитесь к тестовой базе данных по умолчанию, выполнив такую команду:</span><span class="sxs-lookup"><span data-stu-id="969bc-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="969bc-141">Запустите в командной оболочке следующее:</span><span class="sxs-lookup"><span data-stu-id="969bc-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="969bc-142">Будет создана база данных с именем *BookstoreDb*, если она не существует.</span><span class="sxs-lookup"><span data-stu-id="969bc-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="969bc-143">Если такая база данных существует, для нее уже установлено подключение для транзакций.</span><span class="sxs-lookup"><span data-stu-id="969bc-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="969bc-144">Создайте коллекцию `Books` с помощью такой команды:</span><span class="sxs-lookup"><span data-stu-id="969bc-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="969bc-145">Отобразится такой результат:</span><span class="sxs-lookup"><span data-stu-id="969bc-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="969bc-146">Определите схему для коллекции `Books` и вставьте два документа, используя следующую команду:</span><span class="sxs-lookup"><span data-stu-id="969bc-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="969bc-147">Отобразится такой результат:</span><span class="sxs-lookup"><span data-stu-id="969bc-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="969bc-148">Просмотрите документы в базе данных, используя такую команду:</span><span class="sxs-lookup"><span data-stu-id="969bc-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="969bc-149">Отобразится такой результат:</span><span class="sxs-lookup"><span data-stu-id="969bc-149">The following result is displayed:</span></span>

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

    <span data-ttu-id="969bc-150">Схема добавляет автоматически созданное свойство `_id` типа `ObjectId` к каждому документу.</span><span class="sxs-lookup"><span data-stu-id="969bc-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="969bc-151">База данных готова к работе.</span><span class="sxs-lookup"><span data-stu-id="969bc-151">The database is ready.</span></span> <span data-ttu-id="969bc-152">Можно приступить к созданию веб-API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="969bc-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="969bc-153">Создание проекта веб-API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="969bc-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="969bc-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="969bc-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="969bc-155">Откройте **Файл** > **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="969bc-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="969bc-156">Выберите **Веб-приложение ASP.NET Core**, назовите проект *BooksApi* и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="969bc-156">Select **ASP.NET Core Web Application**, name the project *BooksApi*, and click **OK**.</span></span>
1. <span data-ttu-id="969bc-157">Выберите **.NET Core** требуемой версии .NET Framework и **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="969bc-157">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="969bc-158">Выберите шаблон проекта **API** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="969bc-158">Select the **API** project template, and click **OK**:</span></span>
1. <span data-ttu-id="969bc-159">Посетите страницу [коллекции NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/), чтобы узнать последнюю стабильную версию драйвера .NET для MongoDB.</span><span class="sxs-lookup"><span data-stu-id="969bc-159">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="969bc-160">В окне **Консоль диспетчера пакетов** перейдите в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="969bc-160">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="969bc-161">Выполните следующую команду, чтобы установить драйвер .NET для MongoDB:</span><span class="sxs-lookup"><span data-stu-id="969bc-161">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="969bc-162">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="969bc-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="969bc-163">В командной оболочке выполните такие команды:</span><span class="sxs-lookup"><span data-stu-id="969bc-163">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="969bc-164">Новый проект веб-API ASP.NET Core для работы с .NET Core будет создан и открыт в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="969bc-164">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="969bc-165">Нажмите **Yes** (Да), когда отобразится уведомление *Required assets to build and debug are missing from 'BooksApi'. Add them?* (В BooksApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?).</span><span class="sxs-lookup"><span data-stu-id="969bc-165">Click **Yes** when the *Required assets to build and debug are missing from 'BooksApi'. Add them?* notification appears.</span></span>
1. <span data-ttu-id="969bc-166">Посетите страницу [коллекции NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/), чтобы узнать последнюю стабильную версию драйвера .NET для MongoDB.</span><span class="sxs-lookup"><span data-stu-id="969bc-166">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="969bc-167">Откройте **Интегрированный терминал** и перейдите в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="969bc-167">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="969bc-168">Выполните следующую команду, чтобы установить драйвер .NET для MongoDB:</span><span class="sxs-lookup"><span data-stu-id="969bc-168">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="969bc-169">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="969bc-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="969bc-170">Откройте **Файл** > **Создать решение** > **.NET Core** > **Приложение**.</span><span class="sxs-lookup"><span data-stu-id="969bc-170">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="969bc-171">Выберите шаблон проекта **Веб-API ASP.NET Core** для C# и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="969bc-171">Select the **ASP.NET Core Web API** C# project template, and click **Next**.</span></span>
1. <span data-ttu-id="969bc-172">В раскрывающемся списке **Требуемая версия .NET Framework** выберите **.NET Core 2.2** и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="969bc-172">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and click **Next**.</span></span>
1. <span data-ttu-id="969bc-173">Введите *BooksApi* в поле **Имя проекта** и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="969bc-173">Enter *BooksApi* for the **Project Name**, and click **Create**.</span></span>
1. <span data-ttu-id="969bc-174">На панели **Решение** щелкните правой кнопкой мыши узел проекта **Зависимости** и выберите **Добавить пакеты**.</span><span class="sxs-lookup"><span data-stu-id="969bc-174">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="969bc-175">Введите *MongoDB.Driver* в поле поиска, выберите пакет *MongoDB.Driver* и нажмите **Добавить пакет**.</span><span class="sxs-lookup"><span data-stu-id="969bc-175">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and click **Add Package**.</span></span>
1. <span data-ttu-id="969bc-176">Нажмите кнопку **Принимаю** в диалоговом окне **Принятие условий лицензионного соглашения**.</span><span class="sxs-lookup"><span data-stu-id="969bc-176">Click the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-a-model"></a><span data-ttu-id="969bc-177">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="969bc-177">Add a model</span></span>

1. <span data-ttu-id="969bc-178">Добавьте каталог *Models* в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="969bc-178">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="969bc-179">Добавьте класс `Book` в каталог *Models* с помощью такого кода:</span><span class="sxs-lookup"><span data-stu-id="969bc-179">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

<span data-ttu-id="969bc-180">В классе выше свойство `Id`:</span><span class="sxs-lookup"><span data-stu-id="969bc-180">In the preceding class, the `Id` property:</span></span>

* <span data-ttu-id="969bc-181">требуется для сопоставления объекта среды CLR с коллекцией MongoDB.</span><span class="sxs-lookup"><span data-stu-id="969bc-181">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
* <span data-ttu-id="969bc-182">Помечается с помощью `[BsonId]` для назначения этого свойства в качестве первичного ключа документа.</span><span class="sxs-lookup"><span data-stu-id="969bc-182">Is annotated with `[BsonId]` to designate this property as the document's primary key.</span></span>
* <span data-ttu-id="969bc-183">Помечается с помощью `[BsonRepresentation(BsonType.ObjectId)]`, чтобы разрешить передачу параметра в качестве типа `string` вместо `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="969bc-183">Is annotated with `[BsonRepresentation(BsonType.ObjectId)]` to allow passing the parameter as type `string` instead of `ObjectId`.</span></span> <span data-ttu-id="969bc-184">Mongo обрабатывает преобразование из `string` в `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="969bc-184">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

<span data-ttu-id="969bc-185">Другие свойства в классе помечаются с использованием атрибута `[BsonElement]`.</span><span class="sxs-lookup"><span data-stu-id="969bc-185">Other properties in the class are annotated with the `[BsonElement]` attribute.</span></span> <span data-ttu-id="969bc-186">Значение атрибута представляет имя свойства в коллекции MongoDB.</span><span class="sxs-lookup"><span data-stu-id="969bc-186">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-crud-operations-class"></a><span data-ttu-id="969bc-187">Добавление класса операций CRUD</span><span class="sxs-lookup"><span data-stu-id="969bc-187">Add a CRUD operations class</span></span>

1. <span data-ttu-id="969bc-188">Добавьте каталог *Services* в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="969bc-188">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="969bc-189">Добавьте класс `BookService` в каталог *Services* с помощью такого кода:</span><span class="sxs-lookup"><span data-stu-id="969bc-189">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. <span data-ttu-id="969bc-190">Добавьте строку подключения MongoDB в файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="969bc-190">Add the MongoDB connection string to *appsettings.json*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    <span data-ttu-id="969bc-191">Обращение к предыдущему свойству `BookstoreDb` выполняется в конструкторе класса `BookService`.</span><span class="sxs-lookup"><span data-stu-id="969bc-191">The preceding `BookstoreDb` property is accessed in the `BookService` class constructor.</span></span>

1. <span data-ttu-id="969bc-192">Зарегистрируйте класс `BookService` в `Startup.ConfigureServices` с помощью системы внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="969bc-192">In `Startup.ConfigureServices`, register the `BookService` class with the Dependency Injection system:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    <span data-ttu-id="969bc-193">Это необходимо, чтобы включить поддержку внедрения через конструктор в используемые классы.</span><span class="sxs-lookup"><span data-stu-id="969bc-193">The preceding service registration is necessary to support constructor injection in consuming classes.</span></span>

<span data-ttu-id="969bc-194">Класс `BookService` использует следующие члены `MongoDB.Driver` для выполнения операций CRUD в базе данных:</span><span class="sxs-lookup"><span data-stu-id="969bc-194">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="969bc-195">`MongoClient` &ndash; считывает экземпляр сервера для выполнения операций с базой данных.</span><span class="sxs-lookup"><span data-stu-id="969bc-195">`MongoClient` &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="969bc-196">Конструктор этого класса предоставляет строку подключения MongoDB.</span><span class="sxs-lookup"><span data-stu-id="969bc-196">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="969bc-197">`IMongoDatabase` &ndash; представляет базу данных Mongo для выполнения операций.</span><span class="sxs-lookup"><span data-stu-id="969bc-197">`IMongoDatabase` &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="969bc-198">В этом руководстве используется универсальный метод `GetCollection<T>(collection)` в интерфейсе для получения доступа к данным в определенной коллекции.</span><span class="sxs-lookup"><span data-stu-id="969bc-198">This tutorial uses the generic `GetCollection<T>(collection)` method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="969bc-199">Операции CRUD могут выполняться с коллекцией после вызова этого метода.</span><span class="sxs-lookup"><span data-stu-id="969bc-199">CRUD operations can be performed against the collection after this method is called.</span></span> <span data-ttu-id="969bc-200">В вызове метода `GetCollection<T>(collection)`:</span><span class="sxs-lookup"><span data-stu-id="969bc-200">In the `GetCollection<T>(collection)` method call:</span></span>
  * <span data-ttu-id="969bc-201">`collection` представляет имя коллекции;</span><span class="sxs-lookup"><span data-stu-id="969bc-201">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="969bc-202">`T` представляет тип объекта среды CLR, хранящегося в коллекции;</span><span class="sxs-lookup"><span data-stu-id="969bc-202">`T` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="969bc-203">`GetCollection<T>(collection)` возвращает объект `MongoCollection`, представляющий коллекцию.</span><span class="sxs-lookup"><span data-stu-id="969bc-203">`GetCollection<T>(collection)` returns a `MongoCollection` object representing the collection.</span></span> <span data-ttu-id="969bc-204">В этом руководстве следующие методы вызываются для коллекции:</span><span class="sxs-lookup"><span data-stu-id="969bc-204">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="969bc-205">`Find<T>` &ndash; возвращает все документы в коллекции, соответствующие заданным критериям поиска.</span><span class="sxs-lookup"><span data-stu-id="969bc-205">`Find<T>` &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="969bc-206">`InsertOne` &ndash; вставляет предоставленный объект в виде нового документа в коллекции.</span><span class="sxs-lookup"><span data-stu-id="969bc-206">`InsertOne` &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="969bc-207">`ReplaceOne` &ndash; заменяет один документ, отвечающий заданным критериям поиска, предоставленным объектом.</span><span class="sxs-lookup"><span data-stu-id="969bc-207">`ReplaceOne` &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>
* <span data-ttu-id="969bc-208">`DeleteOne` &ndash; удаляет один документ, отвечающий заданным критериям поиска.</span><span class="sxs-lookup"><span data-stu-id="969bc-208">`DeleteOne` &ndash; Deletes a single document matching the provided search criteria.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="969bc-209">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="969bc-209">Add a controller</span></span>

1. <span data-ttu-id="969bc-210">Добавьте класс `BooksController` в каталог *Controllers* с помощью такого кода:</span><span class="sxs-lookup"><span data-stu-id="969bc-210">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    <span data-ttu-id="969bc-211">Предыдущий контроллер веб-API:</span><span class="sxs-lookup"><span data-stu-id="969bc-211">The preceding web API controller:</span></span>

    * <span data-ttu-id="969bc-212">использует класс `BookService` для выполнения операций CRUD;</span><span class="sxs-lookup"><span data-stu-id="969bc-212">Uses the `BookService` class to perform CRUD operations.</span></span>
    * <span data-ttu-id="969bc-213">содержит методы действий для поддержки запросов HTTP GET, POST, PUT и DELETE.</span><span class="sxs-lookup"><span data-stu-id="969bc-213">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
    * <span data-ttu-id="969bc-214">Метод <xref:System.Web.Http.ApiController.CreatedAtRoute*> возвращает ответ 201, который представляет собой стандартный ответ для метода HTTP POST, создающий новый ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="969bc-214">The <xref:System.Web.Http.ApiController.CreatedAtRoute*> method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="969bc-215">`CreatedAtRoute` также добавляет в ответ заголовок расположения.</span><span class="sxs-lookup"><span data-stu-id="969bc-215">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="969bc-216">Заголовок расположения указывает URI вновь созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="969bc-216">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="969bc-217">См. раздел [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="969bc-217">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
1. <span data-ttu-id="969bc-218">Выполните сборку и запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="969bc-218">Build and run the app.</span></span>
1. <span data-ttu-id="969bc-219">В браузере перейдите в `http://localhost:<port>/api/books`.</span><span class="sxs-lookup"><span data-stu-id="969bc-219">Navigate to `http://localhost:<port>/api/books` in your browser.</span></span> <span data-ttu-id="969bc-220">Отобразится такой ответ JSON:</span><span class="sxs-lookup"><span data-stu-id="969bc-220">The following JSON response is displayed:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="969bc-221">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="969bc-221">Next steps</span></span>

<span data-ttu-id="969bc-222">Дополнительные сведения о сборке веб-API ASP.NET Core см. в этих статьях:</span><span class="sxs-lookup"><span data-stu-id="969bc-222">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="969bc-223">Версия статьи на YouTube</span><span class="sxs-lookup"><span data-stu-id="969bc-223">Youtube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
