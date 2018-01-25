---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: "Создание строки подключения и работе с SQL Server LocalDB | Документы Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 25d1c1c9954baaca9ef91eff3dd3c853930a5893
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="6fc0b-102">Создание строки подключения и работе с SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="6fc0b-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="6fc0b-103">По [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="6fc0b-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="6fc0b-104">Создание строки подключения и работе с SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="6fc0b-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="6fc0b-105">`MovieDBContext` Созданный класс обрабатывает задачу подключения к базе данных и сопоставления `Movie` объектов для записи базы данных.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="6fc0b-106">Один вопрос, который может возникнуть вопрос, является, как указать базу данных, в которой будут подключаться.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="6fc0b-107">Вы фактически нет необходимости указывать базу данных, используемую, Entity Framework по умолчанию будет использовать [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="6fc0b-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="6fc0b-108">В этом разделе мы будем явным образом добавить строку подключения в *Web.config* файл приложения.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="6fc0b-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="6fc0b-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="6fc0b-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) — это облегченная версия SQL Server Express Database Engine, запускаемая по запросу и запускается в пользовательском режиме.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="6fc0b-111">LocalDB выполняется в режиме выполнения специальных SQL Server Express, позволяет работать с базами данных как *.mdf* файлов.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="6fc0b-112">Как правило, хранятся файлы базы данных LocalDB в *приложения\_данные* папку веб-проекта.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="6fc0b-113">SQL Server Express не рекомендуется для использования в рабочей среде веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="6fc0b-114">LocalDB в частности не предназначены для рабочих веб-приложению, так как он не предназначен для работы со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="6fc0b-115">Тем не менее, можно легко перенести базу данных LocalDB в SQL Server или SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="6fc0b-116">В Visual Studio 2017 г. LocalDB устанавливается по умолчанию вместе с Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="6fc0b-117">По умолчанию, Entity Framework ищет строку подключения, называются так же, как класс контекста объекта (`MovieDBContext` для этого проекта).</span><span class="sxs-lookup"><span data-stu-id="6fc0b-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="6fc0b-118">Дополнительные сведения см. [строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fc0b-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="6fc0b-119">Откройте корневой каталог приложения *Web.config* файл приведенный ниже.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="6fc0b-120">(Не *Web.config* файла в *представления* папки.)</span><span class="sxs-lookup"><span data-stu-id="6fc0b-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="6fc0b-121">Найти `<connectionStrings>` элемента:</span><span class="sxs-lookup"><span data-stu-id="6fc0b-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="6fc0b-122">Добавьте следующую строку подключения для `<connectionStrings>` элемент в *Web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="6fc0b-123">В следующем примере показано часть *Web.config* файл с добавить новую строку подключения:</span><span class="sxs-lookup"><span data-stu-id="6fc0b-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="6fc0b-124">Строки соединения двух очень похожи.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-124">The two connection strings are very similar.</span></span> <span data-ttu-id="6fc0b-125">Первая строка подключения с именем `DefaultConnection` и используется для базы данных членства для управления доступом приложения.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="6fc0b-126">В строке подключения, вы добавили указан с именем базы данных LocalDB *Movie.mdf* в *приложения\_данные* папки.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="6fc0b-127">Мы не будет использовать базу данных членства в этом учебнике, Дополнительные сведения о членстве, проверку подлинности и безопасности см. в разделе Мои учебника [создать приложение ASP.NET MVC с помощью проверки подлинности и баз данных SQL Server и развернуть в службе приложений Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="6fc0b-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="6fc0b-128">Имя строки подключения должно соответствовать имя [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) класса.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="6fc0b-129">Не требуется фактически добавить `MovieDBContext` строку подключения.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="6fc0b-130">Если не указать строку соединения, Entity Framework будет создать базу данных LocalDB в каталоге пользователи с полным именем [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) класса (в данном случае `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="6fc0b-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="6fc0b-131">Базы данных любое имя вам нравится, при условии, что он имеет *. MDF* суффикс.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="6fc0b-132">Например, мы имя базы данных *MyFilms.mdf*.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="6fc0b-133">Далее мы создадим новый `MoviesController` класс, который можно использовать для отображения данных фильма и позволяют пользователям создавать новые вхождения фильма.</span><span class="sxs-lookup"><span data-stu-id="6fc0b-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6fc0b-134">[Назад](adding-a-model.md)
[Вперед](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="6fc0b-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
