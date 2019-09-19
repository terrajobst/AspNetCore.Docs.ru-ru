---
title: Создание веб-API с помощью ASP.NET Core и MongoDB
author: prkhandelwal
description: В этом руководстве показано, как создать веб-API ASP.NET Core с помощью базы данных NoSQL MongoDB.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 08/17/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: acf2ded8b92a8f77678af7b772ac2a69264a642c
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082373"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="8fcf4-103">Создание веб-API с помощью ASP.NET Core и MongoDB</span><span class="sxs-lookup"><span data-stu-id="8fcf4-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="8fcf4-104">Авторы [Пратик Ханделвал ](https://twitter.com/K2Prk) (Pratik Khandelwal) и [Скотт Эдди](https://twitter.com/Scott_Addie) (Scott Addie).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8fcf4-105">В этом руководстве описано, как создать веб-API, который выполняет операции создания, чтения, обновления и удаления (CRUD) в базе данных NoSQL [MongoDB](https://www.mongodb.com/what-is-mongodb).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="8fcf4-106">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8fcf4-107">Настройка MongoDB</span><span class="sxs-lookup"><span data-stu-id="8fcf4-107">Configure MongoDB</span></span>
> * <span data-ttu-id="8fcf4-108">создать базу данных MongoDB;</span><span class="sxs-lookup"><span data-stu-id="8fcf4-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="8fcf4-109">определить коллекцию и схему MongoDB;</span><span class="sxs-lookup"><span data-stu-id="8fcf4-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="8fcf4-110">выполнить операции CRUD MongoDB из веб-API.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-110">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="8fcf4-111">Настройка сериализации JSON</span><span class="sxs-lookup"><span data-stu-id="8fcf4-111">Customize JSON serialization</span></span>

<span data-ttu-id="8fcf4-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8fcf4-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8fcf4-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="8fcf4-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8fcf4-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8fcf4-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="8fcf4-115">Пакет SDK для .NET Core 3.0 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="8fcf4-115">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="8fcf4-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="8fcf4-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="8fcf4-117">MongoDB</span><span class="sxs-lookup"><span data-stu-id="8fcf4-117">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8fcf4-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="8fcf4-119">Пакет SDK для .NET Core 3.0 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="8fcf4-119">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="8fcf4-120">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="8fcf4-121">C# для Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8fcf4-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="8fcf4-122">MongoDB</span><span class="sxs-lookup"><span data-stu-id="8fcf4-122">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8fcf4-123">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="8fcf4-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="8fcf4-124">Пакет SDK для .NET Core 3.0 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="8fcf4-124">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="8fcf4-125">Visual Studio для Mac 7.7 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="8fcf4-125">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="8fcf4-126">MongoDB</span><span class="sxs-lookup"><span data-stu-id="8fcf4-126">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="8fcf4-127">Настройка MongoDB</span><span class="sxs-lookup"><span data-stu-id="8fcf4-127">Configure MongoDB</span></span>

<span data-ttu-id="8fcf4-128">Если используется Windows, MongoDB по умолчанию устанавливается в папку *C:\\Program Files\\MongoDB*.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-128">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="8fcf4-129">Добавьте *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* в переменную среды `Path`.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-129">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="8fcf4-130">Это изменение обеспечит доступ к MongoDB из любого места на компьютере разработки.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-130">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="8fcf4-131">В следующих шагах используйте интерфейс mongo Shell, чтобы создать базу данных и коллекции и сохранить документы.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-131">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="8fcf4-132">Дополнительные сведения о командах mongo Shell см. в руководстве по [работе с mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-132">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="8fcf4-133">Выберите папку на компьютере разработки для хранения данных.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-133">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="8fcf4-134">Например, *C:\\BooksData* при работе в Windows.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-134">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="8fcf4-135">Если такого каталога нет, создайте его.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-135">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="8fcf4-136">В mongo Shell нельзя создавать каталоги.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-136">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="8fcf4-137">Откройте командную оболочку.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-137">Open a command shell.</span></span> <span data-ttu-id="8fcf4-138">Выполните следующую команду для подключения к MongoDB через порт 27017, заданный по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-138">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="8fcf4-139">Не забудьте заменить `<data_directory_path>` каталогом, созданным на предыдущем этапе.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-139">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

   ```console
   mongod --dbpath <data_directory_path>
   ```

1. <span data-ttu-id="8fcf4-140">Откройте другой экземпляр командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-140">Open another command shell instance.</span></span> <span data-ttu-id="8fcf4-141">Подключитесь к тестовой базе данных по умолчанию, выполнив такую команду:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-141">Connect to the default test database by running the following command:</span></span>

   ```console
   mongo
   ```

1. <span data-ttu-id="8fcf4-142">Запустите в командной оболочке следующее:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-142">Run the following in a command shell:</span></span>

   ```console
   use BookstoreDb
   ```

   <span data-ttu-id="8fcf4-143">Будет создана база данных с именем *BookstoreDb*, если она не существует.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-143">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="8fcf4-144">Если такая база данных существует, для нее уже установлено подключение для транзакций.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-144">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="8fcf4-145">Создайте коллекцию `Books` с помощью такой команды:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-145">Create a `Books` collection using following command:</span></span>

   ```console
   db.createCollection('Books')
   ```

   <span data-ttu-id="8fcf4-146">Отобразится такой результат:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-146">The following result is displayed:</span></span>

   ```console
   { "ok" : 1 }
   ```

1. <span data-ttu-id="8fcf4-147">Определите схему для коллекции `Books` и вставьте два документа, используя следующую команду:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-147">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

   ```console
   db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
   ```

   <span data-ttu-id="8fcf4-148">Отобразится такой результат:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-148">The following result is displayed:</span></span>

   ```console
   {
     "acknowledged" : true,
     "insertedIds" : [
       ObjectId("5bfd996f7b8e48dc15ff215d"),
       ObjectId("5bfd996f7b8e48dc15ff215e")
     ]
   }
   ```
  
   > [!NOTE]
   > <span data-ttu-id="8fcf4-149">Идентификаторы в этой статье не будут соответствовать идентификаторам в запущенном примере.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-149">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="8fcf4-150">Просмотрите документы в базе данных, используя такую команду:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-150">View the documents in the database using the following command:</span></span>

   ```console
   db.Books.find({}).pretty()
   ```

   <span data-ttu-id="8fcf4-151">Отобразится такой результат:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-151">The following result is displayed:</span></span>

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

   <span data-ttu-id="8fcf4-152">Схема добавляет автоматически созданное свойство `_id` типа `ObjectId` к каждому документу.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-152">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="8fcf4-153">База данных готова к работе.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-153">The database is ready.</span></span> <span data-ttu-id="8fcf4-154">Можно приступить к созданию веб-API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-154">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="8fcf4-155">Создание проекта веб-API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8fcf4-155">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8fcf4-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8fcf4-156">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="8fcf4-157">Выберите **Файл** > **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-157">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="8fcf4-158">Выберите тип проекта **Веб-приложение ASP.NET Core** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-158">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="8fcf4-159">Задайте для проекта имя *BooksApi* и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-159">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="8fcf4-160">Выберите целевую платформу **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-160">Select the **.NET Core** target framework and **ASP.NET Core 3.0**.</span></span> <span data-ttu-id="8fcf4-161">Выберите шаблон проекта **API** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-161">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="8fcf4-162">Посетите страницу [коллекции NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/), чтобы узнать последнюю стабильную версию драйвера .NET для MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-162">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="8fcf4-163">В окне **Консоль диспетчера пакетов** перейдите в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-163">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="8fcf4-164">Выполните следующую команду, чтобы установить драйвер .NET для MongoDB:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-164">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8fcf4-165">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-165">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="8fcf4-166">В командной оболочке выполните такие команды:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-166">Run the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   <span data-ttu-id="8fcf4-167">Новый проект веб-API ASP.NET Core для работы с .NET Core будет создан и открыт в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-167">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="8fcf4-168">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from "BooksApi". Add them?** (В BooksApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-168">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="8fcf4-169">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-169">Select **Yes**.</span></span>
1. <span data-ttu-id="8fcf4-170">Посетите страницу [коллекции NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/), чтобы узнать последнюю стабильную версию драйвера .NET для MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-170">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="8fcf4-171">Откройте **Интегрированный терминал** и перейдите в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-171">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="8fcf4-172">Выполните следующую команду, чтобы установить драйвер .NET для MongoDB:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-172">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8fcf4-173">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="8fcf4-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="8fcf4-174">Выберите **Файл** > **Новое решение** > **.NET Core** > **Приложение**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-174">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="8fcf4-175">Выберите шаблон проекта **Веб-API ASP.NET Core** для C# и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-175">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="8fcf4-176">В раскрывающемся списке **Требуемая версия .NET Framework** выберите **.NET Core 3.0** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-176">Select **.NET Core 3.0** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="8fcf4-177">Введите *BooksApi* в поле **Имя проекта** и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-177">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="8fcf4-178">На панели **Решение** щелкните правой кнопкой мыши узел проекта **Зависимости** и выберите **Добавить пакеты**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-178">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="8fcf4-179">В поле поиска введите *MongoDB.Driver*, выберите пакет *MongoDB.Driver* и нажмите **Добавить пакет**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-179">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="8fcf4-180">Нажмите кнопку **Принимаю** в диалоговом окне **Принятие условий лицензионного соглашения**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-180">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="8fcf4-181">Добавление модели сущности</span><span class="sxs-lookup"><span data-stu-id="8fcf4-181">Add an entity model</span></span>

1. <span data-ttu-id="8fcf4-182">Добавьте каталог *Models* в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-182">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="8fcf4-183">Добавьте класс `Book` в каталог *Models* с помощью такого кода:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-183">Add a `Book` class to the *Models* directory with the following code:</span></span>

   ```csharp
   using MongoDB.Bson;
   using MongoDB.Bson.Serialization.Attributes;

   namespace BooksApi.Models
   {
       public class Book
       {
           [BsonId]
           [BsonRepresentation(BsonType.ObjectId)]
           public string Id { get; set; }

           [BsonElement("Name")]
           public string BookName { get; set; }

           public decimal Price { get; set; }

           public string Category { get; set; }

           public string Author { get; set; }
       }
   }
   ```

   <span data-ttu-id="8fcf4-184">В классе выше свойство `Id`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-184">In the preceding class, the `Id` property:</span></span>

   * <span data-ttu-id="8fcf4-185">требуется для сопоставления объекта среды CLR с коллекцией MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-185">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
   * <span data-ttu-id="8fcf4-186">Помечается с помощью [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) для назначения этого свойства в качестве первичного ключа документа.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-186">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
   * <span data-ttu-id="8fcf4-187">Помечается с помощью [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm), чтобы разрешить передачу параметра в качестве типа `string` вместо структуры [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-187">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="8fcf4-188">Mongo обрабатывает преобразование из `string` в `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-188">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

   <span data-ttu-id="8fcf4-189">Свойство `BookName` помечено атрибутом [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-189">The `BookName` property is annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="8fcf4-190">Значение атрибута `Name` представляет имя свойства в коллекции MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-190">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="8fcf4-191">Добавление модели конфигурации</span><span class="sxs-lookup"><span data-stu-id="8fcf4-191">Add a configuration model</span></span>

1. <span data-ttu-id="8fcf4-192">Добавьте в файл *appsettings.json* перечисленные ниже значения конфигурации базы данных.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-192">Add the following database configuration values to *appsettings.json*:</span></span>

   [!code-json[](first-mongo-app/samples/3.x/SampleApp/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="8fcf4-193">Добавьте в каталог *Models* файл *BookstoreDatabaseSettings.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-193">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   <span data-ttu-id="8fcf4-194">Предыдущий класс `BookstoreDatabaseSettings` используется для хранения значений свойств `BookstoreDatabaseSettings` файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-194">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="8fcf4-195">Свойства JSON и C# имеют одинаковые имена, что упрощает сопоставление.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-195">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="8fcf4-196">Добавьте выделенный ниже код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-196">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

   <span data-ttu-id="8fcf4-197">В приведенном выше коде:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-197">In the preceding code:</span></span>

   * <span data-ttu-id="8fcf4-198">Экземпляр конфигурации, к которому привязан раздел `BookstoreDatabaseSettings` файла *appsettings.json*, зарегистрирован в контейнере внедрения зависимостей (DI).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-198">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="8fcf4-199">Например, свойство `ConnectionString` объекта `BookstoreDatabaseSettings` заполняется свойством `BookstoreDatabaseSettings:ConnectionString` в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-199">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
   * <span data-ttu-id="8fcf4-200">Интерфейс `IBookstoreDatabaseSettings` регистрируется в DI с использованием [времени существования отдельной службы](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-200">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="8fcf4-201">При внедрении экземпляр интерфейса разрешается в объект `BookstoreDatabaseSettings`.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-201">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="8fcf4-202">Добавьте следующий код в самое начало файла *Startup.cs*, чтобы разрешить ссылки `BookstoreDatabaseSettings` и `IBookstoreDatabaseSettings`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-202">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="8fcf4-203">Добавление службы операций CRUD</span><span class="sxs-lookup"><span data-stu-id="8fcf4-203">Add a CRUD operations service</span></span>

1. <span data-ttu-id="8fcf4-204">Добавьте каталог *Services* в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-204">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="8fcf4-205">Добавьте класс `BookService` в каталог *Services* с помощью такого кода:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-205">Add a `BookService` class to the *Services* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   <span data-ttu-id="8fcf4-206">В предыдущем коде экземпляр `IBookstoreDatabaseSettings` извлекается из DI посредством внедрения конструктора.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-206">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="8fcf4-207">Таким образом обеспечивается доступ к значениям конфигурации в файле *appsettings.json*, которые были добавлены в разделе [Добавление модели конфигурации](#add-a-configuration-model).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-207">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="8fcf4-208">Добавьте выделенный ниже код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-208">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   <span data-ttu-id="8fcf4-209">В предыдущем коде класс `BookService` регистрируется в DI, чтобы обеспечить поддержку внедрения через конструктор в используемые классы.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-209">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="8fcf4-210">Время существования отдельной службы — наиболее подходящий вариант, так как `BookService` имеет прямую зависимость от `MongoClient`.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-210">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="8fcf4-211">В соответствии с официальными [правилами повторного использования клиента Mongo](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use) `MongoClient` следует регистрировать в DI с использованием времени существования отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-211">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="8fcf4-212">Добавьте следующий код в самое начало файла *Startup.cs*, чтобы разрешить ссылку `BookService`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-212">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="8fcf4-213">Класс `BookService` использует следующие члены `MongoDB.Driver` для выполнения операций CRUD в базе данных:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-213">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="8fcf4-214">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; считывает экземпляр сервера для выполнения операций с базой данных.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-214">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="8fcf4-215">Конструктор этого класса предоставляет строку подключения MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-215">The constructor of this class is provided the MongoDB connection string:</span></span>

  [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="8fcf4-216">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; представляет базу данных Mongo для выполнения операций.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-216">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="8fcf4-217">В этом руководстве используется универсальный метод [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) интерфейса для получения доступа к данным в определенной коллекции.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-217">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="8fcf4-218">Операции CRUD следует выполнять с коллекцией после вызова этого метода.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-218">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="8fcf4-219">В вызове метода `GetCollection<TDocument>(collection)`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-219">In the `GetCollection<TDocument>(collection)` method call:</span></span>

  * <span data-ttu-id="8fcf4-220">`collection` представляет имя коллекции;</span><span class="sxs-lookup"><span data-stu-id="8fcf4-220">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="8fcf4-221">`TDocument` представляет тип объекта среды CLR, хранящегося в коллекции;</span><span class="sxs-lookup"><span data-stu-id="8fcf4-221">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="8fcf4-222">`GetCollection<TDocument>(collection)` возвращает объект [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm), представляющий коллекцию.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-222">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="8fcf4-223">В этом руководстве следующие методы вызываются для коллекции:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-223">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="8fcf4-224">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; удаляет один документ, отвечающий заданным критериям поиска.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-224">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="8fcf4-225">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; возвращает все документы в коллекции, соответствующие заданным критериям поиска.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-225">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="8fcf4-226">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; вставляет предоставленный объект в виде нового документа в коллекции.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-226">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="8fcf4-227">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; заменяет один документ, отвечающий заданным критериям поиска, предоставленным объектом.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-227">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="8fcf4-228">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="8fcf4-228">Add a controller</span></span>

<span data-ttu-id="8fcf4-229">Добавьте класс `BooksController` в каталог *Controllers* с помощью такого кода:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-229">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Controllers/BooksController.cs)]

<span data-ttu-id="8fcf4-230">Предыдущий контроллер веб-API:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-230">The preceding web API controller:</span></span>

* <span data-ttu-id="8fcf4-231">использует класс `BookService` для выполнения операций CRUD;</span><span class="sxs-lookup"><span data-stu-id="8fcf4-231">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="8fcf4-232">содержит методы действий для поддержки запросов HTTP GET, POST, PUT и DELETE.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-232">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="8fcf4-233">Вызывает <xref:System.Web.Http.ApiController.CreatedAtRoute*> в методе действия `Create` для возврата ответа [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-233">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="8fcf4-234">Код состояния 201 представляет собой стандартный ответ метода HTTP POST, создающего ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-234">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="8fcf4-235">`CreatedAtRoute` также добавляет заголовок `Location` в ответ.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-235">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="8fcf4-236">Заголовок `Location` указывает универсальный код ресурса (URI) созданной книги.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-236">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="8fcf4-237">Тестирование веб-API</span><span class="sxs-lookup"><span data-stu-id="8fcf4-237">Test the web API</span></span>

1. <span data-ttu-id="8fcf4-238">Выполните сборку и запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-238">Build and run the app.</span></span>

1. <span data-ttu-id="8fcf4-239">Перейдите по адресу `http://localhost:<port>/api/books`, чтобы протестировать не имеющий параметров метод действия `Get` контроллера.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-239">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="8fcf4-240">Отобразится такой ответ JSON:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-240">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="8fcf4-241">Перейдите по адресу `http://localhost:<port>/api/books/{id here}`, чтобы протестировать перегруженный метод действия `Get` контроллера.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-241">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="8fcf4-242">Отобразится такой ответ JSON:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-242">The following JSON response is displayed:</span></span>

   ```json
   {
     "id":"{ID}",
     "bookName":"Clean Code",
     "price":43.15,
     "category":"Computers",
     "author":"Robert C. Martin"
   }
   ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="8fcf4-243">Настройка параметров сериализации JSON</span><span class="sxs-lookup"><span data-stu-id="8fcf4-243">Configure JSON serialization options</span></span>

<span data-ttu-id="8fcf4-244">Нужно изменить два параметра для ответов JSON, возвращаемых в разделе [Тестирование веб-API](#test-the-web-api):</span><span class="sxs-lookup"><span data-stu-id="8fcf4-244">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="8fcf4-245">Смешанный регистр имен свойств по умолчанию следует изменить в соответствии с регистром Pascal имен свойств объекта CLR.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-245">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="8fcf4-246">Свойство `bookName` должно возвращаться как `Name`.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-246">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="8fcf4-247">Чтобы удовлетворить эти требования, внесите следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-247">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="8fcf4-248">JSON.NET удален из общей платформы ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-248">JSON.NET has been removed from ASP.NET shared framework.</span></span> <span data-ttu-id="8fcf4-249">Добавьте ссылку на пакет в [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-249">Add a package reference to [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson).</span></span>

1. <span data-ttu-id="8fcf4-250">В `Startup.ConfigureServices` вставьте следующий выделенный код в вызов метода `AddMvc`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-250">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   <span data-ttu-id="8fcf4-251">После предыдущего изменения имена свойств в ответе сериализованного JSON веб-API соответствуют именам свойств в типе объекта CLR.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-251">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="8fcf4-252">Например, свойство `Author` класса `Book` сериализуется как `Author`.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-252">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="8fcf4-253">В *Models/Book.cs* добавьте к свойству `BookName` следующий атрибут [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm):</span><span class="sxs-lookup"><span data-stu-id="8fcf4-253">In *Models/Book.cs*, annotate the `BookName` property with the following [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   <span data-ttu-id="8fcf4-254">Значение атрибута `[JsonProperty]`, равное `Name`, представляет имя свойства в ответе сериализованного JSON веб-API.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-254">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="8fcf4-255">Добавьте следующий код в самое начало файла *Models/Book.cs*, чтобы разрешить ссылку на атрибут `[JsonProperty]`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-255">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="8fcf4-256">Повторите действия, описанные в разделе [Тестирование веб-API](#test-the-web-api).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-256">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="8fcf4-257">Обратите внимание на различие в именах свойств JSON.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-257">Notice the difference in JSON property names.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8fcf4-258">В этом руководстве описано, как создать веб-API, который выполняет операции создания, чтения, обновления и удаления (CRUD) в базе данных NoSQL [MongoDB](https://www.mongodb.com/what-is-mongodb).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-258">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="8fcf4-259">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-259">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8fcf4-260">Настройка MongoDB</span><span class="sxs-lookup"><span data-stu-id="8fcf4-260">Configure MongoDB</span></span>
> * <span data-ttu-id="8fcf4-261">создать базу данных MongoDB;</span><span class="sxs-lookup"><span data-stu-id="8fcf4-261">Create a MongoDB database</span></span>
> * <span data-ttu-id="8fcf4-262">определить коллекцию и схему MongoDB;</span><span class="sxs-lookup"><span data-stu-id="8fcf4-262">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="8fcf4-263">выполнить операции CRUD MongoDB из веб-API.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-263">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="8fcf4-264">Настройка сериализации JSON</span><span class="sxs-lookup"><span data-stu-id="8fcf4-264">Customize JSON serialization</span></span>

<span data-ttu-id="8fcf4-265">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8fcf4-265">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8fcf4-266">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="8fcf4-266">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8fcf4-267">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8fcf4-267">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="8fcf4-268">Пакет SDK для .NET Core 2.2</span><span class="sxs-lookup"><span data-stu-id="8fcf4-268">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="8fcf4-269">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="8fcf4-269">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="8fcf4-270">MongoDB</span><span class="sxs-lookup"><span data-stu-id="8fcf4-270">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8fcf4-271">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-271">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="8fcf4-272">Пакет SDK для .NET Core 2.2</span><span class="sxs-lookup"><span data-stu-id="8fcf4-272">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="8fcf4-273">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-273">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="8fcf4-274">C# для Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8fcf4-274">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="8fcf4-275">MongoDB</span><span class="sxs-lookup"><span data-stu-id="8fcf4-275">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8fcf4-276">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="8fcf4-276">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="8fcf4-277">Пакет SDK для .NET Core 2.2</span><span class="sxs-lookup"><span data-stu-id="8fcf4-277">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="8fcf4-278">Visual Studio для Mac 7.7 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="8fcf4-278">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="8fcf4-279">MongoDB</span><span class="sxs-lookup"><span data-stu-id="8fcf4-279">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="8fcf4-280">Настройка MongoDB</span><span class="sxs-lookup"><span data-stu-id="8fcf4-280">Configure MongoDB</span></span>

<span data-ttu-id="8fcf4-281">Если используется Windows, MongoDB по умолчанию устанавливается в папку *C:\\Program Files\\MongoDB*.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-281">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="8fcf4-282">Добавьте *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* в переменную среды `Path`.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-282">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="8fcf4-283">Это изменение обеспечит доступ к MongoDB из любого места на компьютере разработки.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-283">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="8fcf4-284">В следующих шагах используйте интерфейс mongo Shell, чтобы создать базу данных и коллекции и сохранить документы.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-284">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="8fcf4-285">Дополнительные сведения о командах mongo Shell см. в руководстве по [работе с mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-285">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="8fcf4-286">Выберите папку на компьютере разработки для хранения данных.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-286">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="8fcf4-287">Например, *C:\\BooksData* при работе в Windows.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-287">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="8fcf4-288">Если такого каталога нет, создайте его.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-288">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="8fcf4-289">В mongo Shell нельзя создавать каталоги.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-289">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="8fcf4-290">Откройте командную оболочку.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-290">Open a command shell.</span></span> <span data-ttu-id="8fcf4-291">Выполните следующую команду для подключения к MongoDB через порт 27017, заданный по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-291">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="8fcf4-292">Не забудьте заменить `<data_directory_path>` каталогом, созданным на предыдущем этапе.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-292">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

   ```console
   mongod --dbpath <data_directory_path>
   ```

1. <span data-ttu-id="8fcf4-293">Откройте другой экземпляр командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-293">Open another command shell instance.</span></span> <span data-ttu-id="8fcf4-294">Подключитесь к тестовой базе данных по умолчанию, выполнив такую команду:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-294">Connect to the default test database by running the following command:</span></span>

   ```console
   mongo
   ```

1. <span data-ttu-id="8fcf4-295">Запустите в командной оболочке следующее:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-295">Run the following in a command shell:</span></span>

   ```console
   use BookstoreDb
   ```

   <span data-ttu-id="8fcf4-296">Будет создана база данных с именем *BookstoreDb*, если она не существует.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-296">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="8fcf4-297">Если такая база данных существует, для нее уже установлено подключение для транзакций.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-297">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="8fcf4-298">Создайте коллекцию `Books` с помощью такой команды:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-298">Create a `Books` collection using following command:</span></span>

   ```console
   db.createCollection('Books')
   ```

   <span data-ttu-id="8fcf4-299">Отобразится такой результат:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-299">The following result is displayed:</span></span>

   ```console
   { "ok" : 1 }
   ```

1. <span data-ttu-id="8fcf4-300">Определите схему для коллекции `Books` и вставьте два документа, используя следующую команду:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-300">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

   ```console
   db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
   ```

   <span data-ttu-id="8fcf4-301">Отобразится такой результат:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-301">The following result is displayed:</span></span>

   ```console
   {
     "acknowledged" : true,
     "insertedIds" : [
       ObjectId("5bfd996f7b8e48dc15ff215d"),
       ObjectId("5bfd996f7b8e48dc15ff215e")
     ]
   }
   ```
  
   > [!NOTE]
   > <span data-ttu-id="8fcf4-302">Идентификаторы в этой статье не будут соответствовать идентификаторам в запущенном примере.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-302">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="8fcf4-303">Просмотрите документы в базе данных, используя такую команду:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-303">View the documents in the database using the following command:</span></span>

   ```console
   db.Books.find({}).pretty()
   ```

   <span data-ttu-id="8fcf4-304">Отобразится такой результат:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-304">The following result is displayed:</span></span>

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

   <span data-ttu-id="8fcf4-305">Схема добавляет автоматически созданное свойство `_id` типа `ObjectId` к каждому документу.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-305">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="8fcf4-306">База данных готова к работе.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-306">The database is ready.</span></span> <span data-ttu-id="8fcf4-307">Можно приступить к созданию веб-API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-307">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="8fcf4-308">Создание проекта веб-API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8fcf4-308">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8fcf4-309">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8fcf4-309">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="8fcf4-310">Выберите **Файл** > **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-310">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="8fcf4-311">Выберите тип проекта **Веб-приложение ASP.NET Core** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-311">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="8fcf4-312">Задайте для проекта имя *BooksApi* и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-312">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="8fcf4-313">Выберите **.NET Core** требуемой версии .NET Framework и **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-313">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="8fcf4-314">Выберите шаблон проекта **API** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-314">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="8fcf4-315">Посетите страницу [коллекции NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/), чтобы узнать последнюю стабильную версию драйвера .NET для MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-315">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="8fcf4-316">В окне **Консоль диспетчера пакетов** перейдите в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-316">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="8fcf4-317">Выполните следующую команду, чтобы установить драйвер .NET для MongoDB:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-317">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8fcf4-318">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-318">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="8fcf4-319">В командной оболочке выполните такие команды:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-319">Run the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   <span data-ttu-id="8fcf4-320">Новый проект веб-API ASP.NET Core для работы с .NET Core будет создан и открыт в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-320">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="8fcf4-321">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from "BooksApi". Add them?** (В BooksApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-321">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="8fcf4-322">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-322">Select **Yes**.</span></span>
1. <span data-ttu-id="8fcf4-323">Посетите страницу [коллекции NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/), чтобы узнать последнюю стабильную версию драйвера .NET для MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-323">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="8fcf4-324">Откройте **Интегрированный терминал** и перейдите в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-324">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="8fcf4-325">Выполните следующую команду, чтобы установить драйвер .NET для MongoDB:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-325">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8fcf4-326">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="8fcf4-326">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="8fcf4-327">Выберите **Файл** > **Новое решение** > **.NET Core** > **Приложение**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-327">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="8fcf4-328">Выберите шаблон проекта **Веб-API ASP.NET Core** для C# и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-328">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="8fcf4-329">В раскрывающемся списке **Требуемая версия .NET Framework** выберите **.NET Core 2.2** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-329">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="8fcf4-330">Введите *BooksApi* в поле **Имя проекта** и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-330">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="8fcf4-331">На панели **Решение** щелкните правой кнопкой мыши узел проекта **Зависимости** и выберите **Добавить пакеты**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-331">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="8fcf4-332">В поле поиска введите *MongoDB.Driver*, выберите пакет *MongoDB.Driver* и нажмите **Добавить пакет**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-332">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="8fcf4-333">Нажмите кнопку **Принимаю** в диалоговом окне **Принятие условий лицензионного соглашения**.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-333">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="8fcf4-334">Добавление модели сущности</span><span class="sxs-lookup"><span data-stu-id="8fcf4-334">Add an entity model</span></span>

1. <span data-ttu-id="8fcf4-335">Добавьте каталог *Models* в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-335">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="8fcf4-336">Добавьте класс `Book` в каталог *Models* с помощью такого кода:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-336">Add a `Book` class to the *Models* directory with the following code:</span></span>

   ```csharp
   using MongoDB.Bson;
   using MongoDB.Bson.Serialization.Attributes;

   namespace BooksApi.Models
   {
       public class Book
       {
           [BsonId]
           [BsonRepresentation(BsonType.ObjectId)]
           public string Id { get; set; }

           [BsonElement("Name")]
           public string BookName { get; set; }

           public decimal Price { get; set; }

           public string Category { get; set; }

           public string Author { get; set; }
       }
   }
   ```

   <span data-ttu-id="8fcf4-337">В классе выше свойство `Id`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-337">In the preceding class, the `Id` property:</span></span>

   * <span data-ttu-id="8fcf4-338">требуется для сопоставления объекта среды CLR с коллекцией MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-338">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
   * <span data-ttu-id="8fcf4-339">Помечается с помощью [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) для назначения этого свойства в качестве первичного ключа документа.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-339">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
   * <span data-ttu-id="8fcf4-340">Помечается с помощью [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm), чтобы разрешить передачу параметра в качестве типа `string` вместо структуры [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-340">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="8fcf4-341">Mongo обрабатывает преобразование из `string` в `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-341">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

   <span data-ttu-id="8fcf4-342">Свойство `BookName` помечено атрибутом [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-342">The `BookName` property is annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="8fcf4-343">Значение атрибута `Name` представляет имя свойства в коллекции MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-343">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="8fcf4-344">Добавление модели конфигурации</span><span class="sxs-lookup"><span data-stu-id="8fcf4-344">Add a configuration model</span></span>

1. <span data-ttu-id="8fcf4-345">Добавьте в файл *appsettings.json* перечисленные ниже значения конфигурации базы данных.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-345">Add the following database configuration values to *appsettings.json*:</span></span>

   [!code-json[](first-mongo-app/samples/2.x/SampleApp/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="8fcf4-346">Добавьте в каталог *Models* файл *BookstoreDatabaseSettings.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-346">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   <span data-ttu-id="8fcf4-347">Предыдущий класс `BookstoreDatabaseSettings` используется для хранения значений свойств `BookstoreDatabaseSettings` файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-347">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="8fcf4-348">Свойства JSON и C# имеют одинаковые имена, что упрощает сопоставление.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-348">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="8fcf4-349">Добавьте выделенный ниже код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-349">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

   <span data-ttu-id="8fcf4-350">В приведенном выше коде:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-350">In the preceding code:</span></span>

   * <span data-ttu-id="8fcf4-351">Экземпляр конфигурации, к которому привязан раздел `BookstoreDatabaseSettings` файла *appsettings.json*, зарегистрирован в контейнере внедрения зависимостей (DI).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-351">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="8fcf4-352">Например, свойство `ConnectionString` объекта `BookstoreDatabaseSettings` заполняется свойством `BookstoreDatabaseSettings:ConnectionString` в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-352">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
   * <span data-ttu-id="8fcf4-353">Интерфейс `IBookstoreDatabaseSettings` регистрируется в DI с использованием [времени существования отдельной службы](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-353">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="8fcf4-354">При внедрении экземпляр интерфейса разрешается в объект `BookstoreDatabaseSettings`.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-354">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="8fcf4-355">Добавьте следующий код в самое начало файла *Startup.cs*, чтобы разрешить ссылки `BookstoreDatabaseSettings` и `IBookstoreDatabaseSettings`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-355">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="8fcf4-356">Добавление службы операций CRUD</span><span class="sxs-lookup"><span data-stu-id="8fcf4-356">Add a CRUD operations service</span></span>

1. <span data-ttu-id="8fcf4-357">Добавьте каталог *Services* в корневую папку проекта.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-357">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="8fcf4-358">Добавьте класс `BookService` в каталог *Services* с помощью такого кода:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-358">Add a `BookService` class to the *Services* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   <span data-ttu-id="8fcf4-359">В предыдущем коде экземпляр `IBookstoreDatabaseSettings` извлекается из DI посредством внедрения конструктора.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-359">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="8fcf4-360">Таким образом обеспечивается доступ к значениям конфигурации в файле *appsettings.json*, которые были добавлены в разделе [Добавление модели конфигурации](#add-a-configuration-model).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-360">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="8fcf4-361">Добавьте выделенный ниже код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-361">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   <span data-ttu-id="8fcf4-362">В предыдущем коде класс `BookService` регистрируется в DI, чтобы обеспечить поддержку внедрения через конструктор в используемые классы.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-362">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="8fcf4-363">Время существования отдельной службы — наиболее подходящий вариант, так как `BookService` имеет прямую зависимость от `MongoClient`.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-363">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="8fcf4-364">В соответствии с официальными [правилами повторного использования клиента Mongo](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use) `MongoClient` следует регистрировать в DI с использованием времени существования отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-364">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="8fcf4-365">Добавьте следующий код в самое начало файла *Startup.cs*, чтобы разрешить ссылку `BookService`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-365">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="8fcf4-366">Класс `BookService` использует следующие члены `MongoDB.Driver` для выполнения операций CRUD в базе данных:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-366">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="8fcf4-367">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; считывает экземпляр сервера для выполнения операций с базой данных.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-367">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="8fcf4-368">Конструктор этого класса предоставляет строку подключения MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-368">The constructor of this class is provided the MongoDB connection string:</span></span>

  [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="8fcf4-369">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; представляет базу данных Mongo для выполнения операций.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-369">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="8fcf4-370">В этом руководстве используется универсальный метод [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) интерфейса для получения доступа к данным в определенной коллекции.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-370">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="8fcf4-371">Операции CRUD следует выполнять с коллекцией после вызова этого метода.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-371">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="8fcf4-372">В вызове метода `GetCollection<TDocument>(collection)`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-372">In the `GetCollection<TDocument>(collection)` method call:</span></span>

  * <span data-ttu-id="8fcf4-373">`collection` представляет имя коллекции;</span><span class="sxs-lookup"><span data-stu-id="8fcf4-373">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="8fcf4-374">`TDocument` представляет тип объекта среды CLR, хранящегося в коллекции;</span><span class="sxs-lookup"><span data-stu-id="8fcf4-374">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="8fcf4-375">`GetCollection<TDocument>(collection)` возвращает объект [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm), представляющий коллекцию.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-375">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="8fcf4-376">В этом руководстве следующие методы вызываются для коллекции:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-376">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="8fcf4-377">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; удаляет один документ, отвечающий заданным критериям поиска.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-377">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="8fcf4-378">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; возвращает все документы в коллекции, соответствующие заданным критериям поиска.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-378">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="8fcf4-379">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; вставляет предоставленный объект в виде нового документа в коллекции.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-379">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="8fcf4-380">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; заменяет один документ, отвечающий заданным критериям поиска, предоставленным объектом.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-380">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="8fcf4-381">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="8fcf4-381">Add a controller</span></span>

<span data-ttu-id="8fcf4-382">Добавьте класс `BooksController` в каталог *Controllers* с помощью такого кода:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-382">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Controllers/BooksController.cs)]

<span data-ttu-id="8fcf4-383">Предыдущий контроллер веб-API:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-383">The preceding web API controller:</span></span>

* <span data-ttu-id="8fcf4-384">использует класс `BookService` для выполнения операций CRUD;</span><span class="sxs-lookup"><span data-stu-id="8fcf4-384">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="8fcf4-385">содержит методы действий для поддержки запросов HTTP GET, POST, PUT и DELETE.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-385">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="8fcf4-386">Вызывает <xref:System.Web.Http.ApiController.CreatedAtRoute*> в методе действия `Create` для возврата ответа [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-386">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="8fcf4-387">Код состояния 201 представляет собой стандартный ответ метода HTTP POST, создающего ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-387">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="8fcf4-388">`CreatedAtRoute` также добавляет заголовок `Location` в ответ.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-388">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="8fcf4-389">Заголовок `Location` указывает универсальный код ресурса (URI) созданной книги.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-389">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="8fcf4-390">Тестирование веб-API</span><span class="sxs-lookup"><span data-stu-id="8fcf4-390">Test the web API</span></span>

1. <span data-ttu-id="8fcf4-391">Выполните сборку и запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-391">Build and run the app.</span></span>

1. <span data-ttu-id="8fcf4-392">Перейдите по адресу `http://localhost:<port>/api/books`, чтобы протестировать не имеющий параметров метод действия `Get` контроллера.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-392">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="8fcf4-393">Отобразится такой ответ JSON:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-393">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="8fcf4-394">Перейдите по адресу `http://localhost:<port>/api/books/{id here}`, чтобы протестировать перегруженный метод действия `Get` контроллера.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-394">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="8fcf4-395">Отобразится такой ответ JSON:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-395">The following JSON response is displayed:</span></span>

   ```json
   {
     "id":"{ID}",
     "bookName":"Clean Code",
     "price":43.15,
     "category":"Computers",
     "author":"Robert C. Martin"
   }
   ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="8fcf4-396">Настройка параметров сериализации JSON</span><span class="sxs-lookup"><span data-stu-id="8fcf4-396">Configure JSON serialization options</span></span>

<span data-ttu-id="8fcf4-397">Нужно изменить два параметра для ответов JSON, возвращаемых в разделе [Тестирование веб-API](#test-the-web-api):</span><span class="sxs-lookup"><span data-stu-id="8fcf4-397">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="8fcf4-398">Смешанный регистр имен свойств по умолчанию следует изменить в соответствии с регистром Pascal имен свойств объекта CLR.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-398">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="8fcf4-399">Свойство `bookName` должно возвращаться как `Name`.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-399">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="8fcf4-400">Чтобы удовлетворить эти требования, внесите следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-400">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="8fcf4-401">В `Startup.ConfigureServices` вставьте следующий выделенный код в вызов метода `AddMvc`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-401">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   <span data-ttu-id="8fcf4-402">После предыдущего изменения имена свойств в ответе сериализованного JSON веб-API соответствуют именам свойств в типе объекта CLR.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-402">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="8fcf4-403">Например, свойство `Author` класса `Book` сериализуется как `Author`.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-403">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="8fcf4-404">В *Models/Book.cs* добавьте к свойству `BookName` следующий атрибут [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm):</span><span class="sxs-lookup"><span data-stu-id="8fcf4-404">In *Models/Book.cs*, annotate the `BookName` property with the following [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   <span data-ttu-id="8fcf4-405">Значение атрибута `[JsonProperty]`, равное `Name`, представляет имя свойства в ответе сериализованного JSON веб-API.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-405">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="8fcf4-406">Добавьте следующий код в самое начало файла *Models/Book.cs*, чтобы разрешить ссылку на атрибут `[JsonProperty]`:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-406">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="8fcf4-407">Повторите действия, описанные в разделе [Тестирование веб-API](#test-the-web-api).</span><span class="sxs-lookup"><span data-stu-id="8fcf4-407">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="8fcf4-408">Обратите внимание на различие в именах свойств JSON.</span><span class="sxs-lookup"><span data-stu-id="8fcf4-408">Notice the difference in JSON property names.</span></span>

::: moniker-end

## <a name="next-steps"></a><span data-ttu-id="8fcf4-409">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="8fcf4-409">Next steps</span></span>

<span data-ttu-id="8fcf4-410">Дополнительные сведения о сборке веб-API ASP.NET Core см. в этих статьях:</span><span class="sxs-lookup"><span data-stu-id="8fcf4-410">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="8fcf4-411">Версия статьи на YouTube</span><span class="sxs-lookup"><span data-stu-id="8fcf4-411">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
