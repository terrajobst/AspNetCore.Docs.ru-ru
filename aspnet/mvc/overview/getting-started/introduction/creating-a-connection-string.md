---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Создание строки подключения и работа с SQL Server LocalDB | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 8c8db3fc121b2ff263ab033404211e167e19cd71
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38168380"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="ab533-102">Создание строки подключения и работа с SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="ab533-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="ab533-103">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="ab533-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="ab533-104">Создание строки подключения и работа с SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="ab533-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="ab533-105">`MovieDBContext` Созданный класс обрабатывает задачи подключения к базе данных и сопоставления `Movie` объектов для записи базы данных.</span><span class="sxs-lookup"><span data-stu-id="ab533-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="ab533-106">Один вопрос, на который вы можете спросить, однако, как указать базу данных, которая подключается к.</span><span class="sxs-lookup"><span data-stu-id="ab533-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="ab533-107">Вы фактически нет необходимости указывать базу данных использовать, Entity Framework по умолчанию будет использовать [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="ab533-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="ab533-108">В этом разделе мы будем явно добавить строку подключения в *Web.config* файл приложения.</span><span class="sxs-lookup"><span data-stu-id="ab533-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="ab533-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="ab533-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="ab533-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) — это облегченная версия SQL Server Express Database Engine, запускаемая по запросу и работает в пользовательском режиме.</span><span class="sxs-lookup"><span data-stu-id="ab533-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="ab533-111">LocalDB выполняется в специального режима выполнения SQL Server Express, которая позволяет работать с базами данных как *.mdf* файлов.</span><span class="sxs-lookup"><span data-stu-id="ab533-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="ab533-112">Как правило, хранятся файлы базы данных LocalDB в *приложения\_данных* папку веб-проекта.</span><span class="sxs-lookup"><span data-stu-id="ab533-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="ab533-113">SQL Server Express не рекомендуется для использования в рабочей среде веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="ab533-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="ab533-114">LocalDB в частности не следует для рабочей среды веб-приложению, так как он не предназначен для работы со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="ab533-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="ab533-115">Тем не менее базу данных LocalDB можно легко перенести в SQL Server или SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="ab533-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="ab533-116">В Visual Studio 2017 LocalDB устанавливается по умолчанию с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ab533-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="ab533-117">По умолчанию, Entity Framework ищет строку подключения с именем, так же, как класс контекста объекта (`MovieDBContext` для этого проекта).</span><span class="sxs-lookup"><span data-stu-id="ab533-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="ab533-118">Дополнительные сведения см. в разделе [строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="ab533-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="ab533-119">Откройте корневой каталог приложения *Web.config* файл, показанный ниже.</span><span class="sxs-lookup"><span data-stu-id="ab533-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="ab533-120">(Не *Web.config* файл *представления* папки.)</span><span class="sxs-lookup"><span data-stu-id="ab533-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="ab533-121">Найти `<connectionStrings>` элемент:</span><span class="sxs-lookup"><span data-stu-id="ab533-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="ab533-122">Добавьте следующую строку подключения для `<connectionStrings>` элемент в *Web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="ab533-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="ab533-123">В следующем примере показано часть *Web.config* файл с добавить новую строку подключения:</span><span class="sxs-lookup"><span data-stu-id="ab533-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="ab533-124">Две строки подключения, очень похожи.</span><span class="sxs-lookup"><span data-stu-id="ab533-124">The two connection strings are very similar.</span></span> <span data-ttu-id="ab533-125">Первая строка подключения имеет имя `DefaultConnection` и используется для базы данных членства для контроля пользователей, которые могут работать с приложением.</span><span class="sxs-lookup"><span data-stu-id="ab533-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="ab533-126">В строке подключения, вы добавили указан с именем базы данных LocalDB *Movie.mdf* в *приложения\_данных* папки.</span><span class="sxs-lookup"><span data-stu-id="ab533-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="ab533-127">Мы не использовать базы данных членства в этом руководстве, Дополнительные сведения о членстве, проверка подлинности и безопасности, см. в разделе my руководства [Создание приложения ASP.NET MVC с проверкой подлинности и база данных SQL и развертывание в службе приложений Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="ab533-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="ab533-128">Имя строки подключения должно соответствовать имя [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) класса.</span><span class="sxs-lookup"><span data-stu-id="ab533-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="ab533-129">Вам не нужен добавить `MovieDBContext` строку подключения.</span><span class="sxs-lookup"><span data-stu-id="ab533-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="ab533-130">Если не указать строку подключения, Entity Framework создаст базу данных LocalDB в каталоге пользователи с полным именем из [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) класса (в данном случае `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="ab533-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="ab533-131">Вы можно назвать базы данных вам нравится, до тех пор, пока он имеет *. MDF* суффикс.</span><span class="sxs-lookup"><span data-stu-id="ab533-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="ab533-132">Например, можно назвать базе *MyFilms.mdf*.</span><span class="sxs-lookup"><span data-stu-id="ab533-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="ab533-133">Далее предстоит создать новый `MoviesController` класс, который можно использовать для отображения данных фильма и разрешить пользователям создавать новые вхождения фильма.</span><span class="sxs-lookup"><span data-stu-id="ab533-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ab533-134">[Назад](adding-a-model.md)
> [Вперед](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="ab533-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
