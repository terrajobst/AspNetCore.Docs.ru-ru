---
title: Сборка веб-API с использованием ASP.NET Core и MongoDB
author: prkhandelwal
description: В руководстве показано, как выполнять сборку веб-API ASP.NET Core с помощью базы данных NoSQL MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 01/31/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 91d8be6cd9160eefe56731d23d5dc7ba18eb6a8f
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665461"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>Создание веб-API с помощью ASP.NET Core и MongoDB

Авторы [Пратик Ханделвал ](https://twitter.com/K2Prk) (Pratik Khandelwal) и [Скотт Эдди](https://twitter.com/Scott_Addie) (Scott Addie).

В этом руководстве описано, как создать веб-API, который выполняет операции создания, чтения, обновления и удаления (CRUD) в базе данных NoSQL [MongoDB](https://www.mongodb.com/what-is-mongodb).

В этом руководстве вы узнаете, как:

> [!div class="checklist"]
> * Настройка MongoDB
> * создать базу данных MongoDB;
> * определить коллекцию и схему MongoDB;
> * выполнить операции CRUD MongoDB из веб-API.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Предварительные требования

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Пакет SDK для .NET Core 2.2 или более поздней версии](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017 15.9 или более поздней версии](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) с рабочей нагрузкой **ASP.NET и веб-разработка**
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

* [Пакет SDK для .NET Core 2.2 или более поздней версии](https://www.microsoft.com/net/download/all)
* [Visual Studio Code.](https://code.visualstudio.com/download)
* [C# для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [MongoDB](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* [Пакет SDK для .NET Core 2.2 или более поздней версии](https://www.microsoft.com/net/download/all)
* [Visual Studio для Mac 7.7 или более поздней версии](https://www.visualstudio.com/downloads/)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a>Настройка MongoDB

Если используется Windows, MongoDB по умолчанию устанавливается в папку *C:\\Program Files\\MongoDB*. Добавьте *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* в переменную среды `Path`. Это изменение обеспечит доступ к MongoDB из любого места на компьютере разработки.

В следующих шагах используйте интерфейс mongo Shell, чтобы создать базу данных и коллекции и сохранить документы. Дополнительные сведения о командах mongo Shell см. в руководстве по [работе с mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).

1. Выберите папку на компьютере разработки для хранения данных. Например, *C:\\BooksData* при работе в Windows. Если такого каталога нет, создайте его. В mongo Shell нельзя создавать каталоги.
1. Откройте командную оболочку. Выполните следующую команду для подключения к MongoDB через порт 27017, заданный по умолчанию. Не забудьте заменить `<data_directory_path>` каталогом, созданным на предыдущем этапе.

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. Откройте другой экземпляр командной оболочки. Подключитесь к тестовой базе данных по умолчанию, выполнив такую команду:

    ```console
    mongo
    ```

1. Запустите в командной оболочке следующее:

    ```console
    use BookstoreDb
    ```

    Будет создана база данных с именем *BookstoreDb*, если она не существует. Если такая база данных существует, для нее уже установлено подключение для транзакций.

1. Создайте коллекцию `Books` с помощью такой команды:

    ```console
    db.createCollection('Books')
    ```

    Отобразится такой результат:

    ```console
    { "ok" : 1 }
    ```

1. Определите схему для коллекции `Books` и вставьте два документа, используя следующую команду:

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    Отобразится такой результат:

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. Просмотрите документы в базе данных, используя такую команду:

    ```console
    db.Books.find({}).pretty()
    ```

    Отобразится такой результат:

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

    Схема добавляет автоматически созданное свойство `_id` типа `ObjectId` к каждому документу.

База данных готова к работе. Можно приступить к созданию веб-API ASP.NET Core.

## <a name="create-the-aspnet-core-web-api-project"></a>Создание проекта веб-API ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Откройте **Файл** > **Создать** > **Проект**.
1. Выберите **Веб-приложение ASP.NET Core**, назовите проект *BooksApi* и нажмите кнопку **ОК**.
1. Выберите **.NET Core** требуемой версии .NET Framework и **ASP.NET Core 2.2**. Выберите шаблон проекта **API** и нажмите кнопку **ОК**.
1. Посетите страницу [коллекции NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/), чтобы узнать последнюю стабильную версию драйвера .NET для MongoDB. В окне **Консоль диспетчера пакетов** перейдите в корневую папку проекта. Выполните следующую команду, чтобы установить драйвер .NET для MongoDB:

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

1. В командной оболочке выполните такие команды:

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    Новый проект веб-API ASP.NET Core для работы с .NET Core будет создан и открыт в Visual Studio Code.

1. Нажмите **Yes** (Да), когда отобразится уведомление *Required assets to build and debug are missing from 'BooksApi'. Add them?* (В BooksApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?).
1. Посетите страницу [коллекции NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/), чтобы узнать последнюю стабильную версию драйвера .NET для MongoDB. Откройте **Интегрированный терминал** и перейдите в корневую папку проекта. Выполните следующую команду, чтобы установить драйвер .NET для MongoDB:

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

1. Откройте **Файл** > **Создать решение** > **.NET Core** > **Приложение**.
1. Выберите шаблон проекта **Веб-API ASP.NET Core** для C# и нажмите **Далее**.
1. В раскрывающемся списке **Требуемая версия .NET Framework** выберите **.NET Core 2.2** и нажмите **Далее**.
1. Введите *BooksApi* в поле **Имя проекта** и нажмите **Создать**.
1. На панели **Решение** щелкните правой кнопкой мыши узел проекта **Зависимости** и выберите **Добавить пакеты**.
1. Введите *MongoDB.Driver* в поле поиска, выберите пакет *MongoDB.Driver* и нажмите **Добавить пакет**.
1. Нажмите кнопку **Принимаю** в диалоговом окне **Принятие условий лицензионного соглашения**.

---

## <a name="add-a-model"></a>Добавление модели

1. Добавьте каталог *Models* в корневую папку проекта.
1. Добавьте класс `Book` в каталог *Models* с помощью такого кода:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

В классе выше свойство `Id`:

* требуется для сопоставления объекта среды CLR с коллекцией MongoDB.
* Помечается с помощью `[BsonId]` для назначения этого свойства в качестве первичного ключа документа.
* Помечается с помощью `[BsonRepresentation(BsonType.ObjectId)]`, чтобы разрешить передачу параметра в качестве типа `string` вместо `ObjectId`. Mongo обрабатывает преобразование из `string` в `ObjectId`.

Другие свойства в классе помечаются с использованием атрибута `[BsonElement]`. Значение атрибута представляет имя свойства в коллекции MongoDB.

## <a name="add-a-crud-operations-class"></a>Добавление класса операций CRUD

1. Добавьте каталог *Services* в корневую папку проекта.
1. Добавьте класс `BookService` в каталог *Services* с помощью такого кода:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. Добавьте строку подключения MongoDB в файл *appsettings.json*:

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    Обращение к предыдущему свойству `BookstoreDb` выполняется в конструкторе класса `BookService`.

1. Зарегистрируйте класс `BookService` в `Startup.ConfigureServices` с помощью системы внедрения зависимостей:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    Это необходимо, чтобы включить поддержку внедрения через конструктор в используемые классы.

Класс `BookService` использует следующие члены `MongoDB.Driver` для выполнения операций CRUD в базе данных:

* `MongoClient` &ndash; считывает экземпляр сервера для выполнения операций с базой данных. Конструктор этого класса предоставляет строку подключения MongoDB.

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* `IMongoDatabase` &ndash; представляет базу данных Mongo для выполнения операций. В этом руководстве используется универсальный метод `GetCollection<T>(collection)` в интерфейсе для получения доступа к данным в определенной коллекции. Операции CRUD могут выполняться с коллекцией после вызова этого метода. В вызове метода `GetCollection<T>(collection)`:
  * `collection` представляет имя коллекции;
  * `T` представляет тип объекта среды CLR, хранящегося в коллекции;

`GetCollection<T>(collection)` возвращает объект `MongoCollection`, представляющий коллекцию. В этом руководстве следующие методы вызываются для коллекции:

* `Find<T>` &ndash; возвращает все документы в коллекции, соответствующие заданным критериям поиска.
* `InsertOne` &ndash; вставляет предоставленный объект в виде нового документа в коллекции.
* `ReplaceOne` &ndash; заменяет один документ, отвечающий заданным критериям поиска, предоставленным объектом.
* `DeleteOne` &ndash; удаляет один документ, отвечающий заданным критериям поиска.

## <a name="add-a-controller"></a>Добавление контроллера

1. Добавьте класс `BooksController` в каталог *Controllers* с помощью такого кода:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    Предыдущий контроллер веб-API:

    * использует класс `BookService` для выполнения операций CRUD;
    * содержит методы действий для поддержки запросов HTTP GET, POST, PUT и DELETE.
1. Выполните сборку и запуск приложения.
1. В браузере перейдите в `http://localhost:<port>/api/books`. Отобразится такой ответ JSON:

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

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о сборке веб-API ASP.NET Core см. в этих статьях:

* <xref:web-api/index>
* <xref:web-api/action-return-types>
