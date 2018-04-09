---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Добавление модели | Документы Microsoft
author: Rick-Anderson
description: 'Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасный, гораздо проще выполните и демонстрационных...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 562a06e22aad62b6982aca3316a2dfe18a6eba2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model"></a><span data-ttu-id="cfb5d-104">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="cfb5d-104">Adding a Model</span></span>
====================
<span data-ttu-id="cfb5d-105">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="cfb5d-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="cfb5d-106">Доступна обновленная версия этого учебника [здесь](../../getting-started/introduction/getting-started.md) , с использованием ASP.NET MVC 5 и Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="cfb5d-107">Он является более безопасны, выполните гораздо проще и показаны дополнительные возможности.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="cfb5d-108">В этом разделе вы добавите некоторые классы для управления фильмов в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="cfb5d-109">Эти классы будет &quot;модели&quot; частью приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="cfb5d-110">Используемой технологии доступа к данным .NET Framework, известный как [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) для определения и работы с этими классами модели.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="cfb5d-111">Поддерживает платформы Entity Framework (часто обозначается как EF), который называется парадигмы разработки *Code First*.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="cfb5d-112">Код сначала позволяет создавать объекты модели путем написания простых классов.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="cfb5d-113">(Они также известны как классов POCO из &quot;объектов plain old CLR.&quot;) Затем можно установить базу данных создан автоматически из классов, который позволяет очень простой и быстрой разработки рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="cfb5d-114">Добавление классов модели</span><span class="sxs-lookup"><span data-stu-id="cfb5d-114">Adding Model Classes</span></span>

<span data-ttu-id="cfb5d-115">В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* выберите **добавить**и выберите **класса**.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="cfb5d-116">Введите *класса* имя &quot;фильма&quot;.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="cfb5d-117">Добавьте следующие пять свойства `Movie` класса:</span><span class="sxs-lookup"><span data-stu-id="cfb5d-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="cfb5d-118">Мы будем использовать `Movie` класс для представления фильмов в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="cfb5d-119">Каждый экземпляр `Movie` объекта будет соответствовать строки в таблицу базы данных и каждое свойство `Movie` класса сопоставляются со столбцом в таблице.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="cfb5d-120">В том же файле добавьте следующий `MovieDBContext` класса:</span><span class="sxs-lookup"><span data-stu-id="cfb5d-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="cfb5d-121">`MovieDBContext` Класс представляет контекст базы данных Entity Framework фильм, который обрабатывает выборка, хранения и обновления `Movie` класса экземпляров в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="cfb5d-122">`MovieDBContext` Является производным от `DbContext` базовый класс, предоставляемый платформой Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="cfb5d-123">Чтобы иметь возможность ссылаться на `DbContext` и `DbSet`, необходимо добавить следующие `using` инструкции в верхней части файла:</span><span class="sxs-lookup"><span data-stu-id="cfb5d-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="cfb5d-124">Полный *Movie.cs* файла показан ниже.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="cfb5d-125">(Несколько с помощью инструкций, которые не требуется были удалены.)</span><span class="sxs-lookup"><span data-stu-id="cfb5d-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="cfb5d-126">Создание строки подключения и работе с SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="cfb5d-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="cfb5d-127">`MovieDBContext` Созданный класс обрабатывает задачу подключения к базе данных и сопоставления `Movie` объектов для записи базы данных.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="cfb5d-128">Один вопрос, который может возникнуть вопрос, является, как указать базу данных, в которой будут подключаться.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="cfb5d-129">Который предстоит выполнить путем добавления сведений о соединении в *Web.config* файл приложения.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="cfb5d-130">Откройте корневой каталог приложения *Web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="cfb5d-131">(Не *Web.config* файла в *представления* папки.) Откройте *Web.config* файл выделено красным цветом.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="cfb5d-132">Добавьте следующую строку подключения для `<connectionStrings>` элемент в *Web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="cfb5d-133">В следующем примере показано часть *Web.config* файл с добавить новую строку подключения:</span><span class="sxs-lookup"><span data-stu-id="cfb5d-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="cfb5d-134">Этот небольшой объем кода и XML предоставляет все необходимое для записи для представления и хранить в базе данных фильма.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="cfb5d-135">Далее мы создадим новый `MoviesController` класс, который можно использовать для отображения данных фильма и позволяют пользователям создавать новые вхождения фильма.</span><span class="sxs-lookup"><span data-stu-id="cfb5d-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cfb5d-136">[Назад](adding-a-view.md)
> [Вперед](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="cfb5d-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
