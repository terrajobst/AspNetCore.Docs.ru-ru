---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: "Часть 4: Модели и доступа к данным | Документы Microsoft"
author: jongalloway
description: "Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store. Часть 4 описывает моделей и доступа к данным."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 727336344493b439130b2cf0ec6e7b5925dd5637
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="3ff51-104">Часть 4: Модели и доступа к данным</span><span class="sxs-lookup"><span data-stu-id="3ff51-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="3ff51-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="3ff51-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="3ff51-106">MVC Music Store является учебное приложение, которое вводятся и описываются пошаговые способы использования Visual Studio и ASP.NET MVC для веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="3ff51-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="3ff51-107">MVC Music Store показана реализация хранилища упрощенных образец продает велосипеды через Интернет альбомы, которое реализует Администрирование сайта основные, вход пользователей и функциональные возможности корзины покупок.</span><span class="sxs-lookup"><span data-stu-id="3ff51-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="3ff51-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="3ff51-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="3ff51-109">Часть 4 описывает моделей и доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="3ff51-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="3ff51-110">Таким образом мы только что передача «фиктивные данные» из наших контроллеров в наших шаблонов представлений.</span><span class="sxs-lookup"><span data-stu-id="3ff51-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="3ff51-111">Теперь все готово для подключения к базе данных real.</span><span class="sxs-lookup"><span data-stu-id="3ff51-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="3ff51-112">В этом учебнике мы будем рассматривать как использовать SQL Server Compact Edition (часто называемые SQL CE) как нашей компонента database engine.</span><span class="sxs-lookup"><span data-stu-id="3ff51-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="3ff51-113">SQL CE — это бесплатная, внедренная, файловая база не требует установки ни конфигурацию, что делает его очень удобным для локальной разработки.</span><span class="sxs-lookup"><span data-stu-id="3ff51-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="3ff51-114">Доступ к базе данных с Entity Framework сначала код</span><span class="sxs-lookup"><span data-stu-id="3ff51-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="3ff51-115">Мы будем использовать поддержка Entity Framework (EF), включенный в проектах ASP.NET MVC 3 для запроса и обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="3ff51-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="3ff51-116">EF является объектом гибкой реляционного сопоставления (ORM) данных API, который позволяет разработчикам запрашивать и обновлять данные, хранящиеся в базе данных в объектно ориентированным способом.</span><span class="sxs-lookup"><span data-stu-id="3ff51-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="3ff51-117">Платформа Entity Framework версии 4 поддерживает парадигмы разработки, вызывается сначала код.</span><span class="sxs-lookup"><span data-stu-id="3ff51-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="3ff51-118">Сначала код позволяет создавать объект модели, написав простых классов (также известные как POCO из объектов среды CLR «plain old») и даже может создать базу данных на лету из классов.</span><span class="sxs-lookup"><span data-stu-id="3ff51-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="3ff51-119">Изменения в наших классов модели</span><span class="sxs-lookup"><span data-stu-id="3ff51-119">Changes to our Model Classes</span></span>

<span data-ttu-id="3ff51-120">Мы будет использование функции создания базы данных в Entity Framework в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="3ff51-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="3ff51-121">Перед этим, давайте внести несколько незначительные изменения в наших классов модели для добавления в несколько действий, которые мы будем использовать позже.</span><span class="sxs-lookup"><span data-stu-id="3ff51-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="3ff51-122">Добавление классов модели исполнителя</span><span class="sxs-lookup"><span data-stu-id="3ff51-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="3ff51-123">Наш альбомы будут связаны с исполнители, поэтому мы добавим простой класс модели для описания художник.</span><span class="sxs-lookup"><span data-stu-id="3ff51-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="3ff51-124">Добавьте новый класс в папке «модели» с именем Artist.cs, используя приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="3ff51-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="3ff51-125">Обновление наших классов модели</span><span class="sxs-lookup"><span data-stu-id="3ff51-125">Updating our Model Classes</span></span>

<span data-ttu-id="3ff51-126">Обновите класс альбома, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="3ff51-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="3ff51-127">Затем выполните следующие обновления в класс жанра.</span><span class="sxs-lookup"><span data-stu-id="3ff51-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="3ff51-128">Добавление приложения\_папки данных</span><span class="sxs-lookup"><span data-stu-id="3ff51-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="3ff51-129">Мы добавим приложение\_каталог данных для нашего проекта для хранения нашей файлы базы данных SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="3ff51-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="3ff51-130">Приложение\_данных представляет собой специальный каталог в ASP.NET, который уже имеет правильные права доступа для доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3ff51-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="3ff51-131">В меню Проект выберите Добавить папку ASP.NET, то приложение\_данных.</span><span class="sxs-lookup"><span data-stu-id="3ff51-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="3ff51-132">Создание строки подключения в файле web.config</span><span class="sxs-lookup"><span data-stu-id="3ff51-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="3ff51-133">Мы добавим несколько строк в файле конфигурации веб-сайта, чтобы платформа Entity Framework известно, как подключиться к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3ff51-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="3ff51-134">Дважды щелкните файл Web.config, расположенный в корневой папке проекта.</span><span class="sxs-lookup"><span data-stu-id="3ff51-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="3ff51-135">Прокрутите в конец этого файла и добавьте &lt;connectionStrings&gt; статьи непосредственно над последней строки, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="3ff51-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="3ff51-136">Добавление класса контекста</span><span class="sxs-lookup"><span data-stu-id="3ff51-136">Adding a Context Class</span></span>

<span data-ttu-id="3ff51-137">Щелкните правой кнопкой мыши папку модели и добавьте новый класс с именем MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="3ff51-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="3ff51-138">Этот класс будет представляет контекст базы данных Entity Framework и будет обрабатывать нашей создание, чтение, обновление и операции удаления для нас.</span><span class="sxs-lookup"><span data-stu-id="3ff51-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="3ff51-139">Ниже приведен код для этого класса.</span><span class="sxs-lookup"><span data-stu-id="3ff51-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="3ff51-140">Вот так - у нет другие конфигурации, специальные интерфейсы и т. д. Расширив базовый класс DbContext, наш класс MusicStoreEntities обрабатывать нам операций базы данных.</span><span class="sxs-lookup"><span data-stu-id="3ff51-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="3ff51-141">Теперь, когда у нас есть, подключено, добавим несколько свойств для наших классов модели, чтобы воспользоваться преимуществами некоторых дополнительных сведений в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3ff51-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="3ff51-142">Добавление хранилища данных каталога</span><span class="sxs-lookup"><span data-stu-id="3ff51-142">Adding our store catalog data</span></span>

<span data-ttu-id="3ff51-143">Мы будет использовать преимущества компонента в Entity Framework, который добавляет вновь созданную базу данных «seed».</span><span class="sxs-lookup"><span data-stu-id="3ff51-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="3ff51-144">Наш каталог магазина список жанров, исполнителей и альбомам будет заполнить.</span><span class="sxs-lookup"><span data-stu-id="3ff51-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="3ff51-145">Загрузка MvcMusicStore Assets.zip - которой включены файлы разработки нашей узла, использованные ранее в этом учебнике - имеет файл класса с данным начальное значение, расположенном в папке с именем кода.</span><span class="sxs-lookup"><span data-stu-id="3ff51-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="3ff51-146">В коде или в папку Models, найдите файл SampleData.cs и поместите его в папку модели в проект, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="3ff51-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="3ff51-147">Теперь нужно добавить одну строку кода, чтобы сообщить о SampleData класса Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3ff51-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="3ff51-148">Дважды щелкните файл Global.asax в корневой папке проекта, чтобы открыть его и добавьте следующие строки в начало приложение\_Start-метод.</span><span class="sxs-lookup"><span data-stu-id="3ff51-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="3ff51-149">На этом этапе мы выполнили действия, необходимые для настройки Entity Framework для проекта.</span><span class="sxs-lookup"><span data-stu-id="3ff51-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="3ff51-150">Запрос к базе данных</span><span class="sxs-lookup"><span data-stu-id="3ff51-150">Querying the Database</span></span>

<span data-ttu-id="3ff51-151">Теперь давайте обновить нашей StoreController таким образом, чтобы вместо «пустой данных» вместо этого он вызывает в базу данных для запроса всех ее данных.</span><span class="sxs-lookup"><span data-stu-id="3ff51-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="3ff51-152">Мы начнем посредством объявления поля в **StoreController** для хранения экземпляра класса MusicStoreEntities, с именем storeDB:</span><span class="sxs-lookup"><span data-stu-id="3ff51-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="3ff51-153">Обновление индекса хранилища запросов к базе данных</span><span class="sxs-lookup"><span data-stu-id="3ff51-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="3ff51-154">Класс MusicStoreEntities поддерживается платформой Entity Framework и предоставляет доступ к свойству коллекции для каждой таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3ff51-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="3ff51-155">Давайте обновить действие нашей StoreController индекса для получения всех жанров в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3ff51-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="3ff51-156">Ранее мы сделали это, жестко запрограммированные строки данных.</span><span class="sxs-lookup"><span data-stu-id="3ff51-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="3ff51-157">Теперь мы просто воспользуйтесь контекста Entity Framework Generes коллекции:</span><span class="sxs-lookup"><span data-stu-id="3ff51-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="3ff51-158">Изменения не должны проходить нашей Просмотр шаблона, так как мы по-прежнему возвращая же StoreIndexViewModel, мы возвращены до того, — мы просто возвращая динамических данных в базе данных теперь.</span><span class="sxs-lookup"><span data-stu-id="3ff51-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="3ff51-159">Когда мы снова запустите проект и посетите URL-адрес «/ хранения», мы теперь увидите список всех жанров в базе данных:</span><span class="sxs-lookup"><span data-stu-id="3ff51-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="3ff51-160">Обновление хранилища Обзор и сведения для использования динамических данных</span><span class="sxs-lookup"><span data-stu-id="3ff51-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="3ff51-161">С/Store/обзора? жанр =*[некоторые жанр]* метод действия, мы ищем жанра по имени.</span><span class="sxs-lookup"><span data-stu-id="3ff51-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="3ff51-162">Только ожидается один результат, так как мы никогда не должны иметь две записи того же имени жанра, поэтому мы можем использовать. Расширение Single() в LINQ запрос на соответствующий объект жанр следующим образом (не следует вводить это еще):</span><span class="sxs-lookup"><span data-stu-id="3ff51-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="3ff51-163">Один метод принимает как параметр, который указывает, что мы объекта жанр таким образом, что его имя совпадает со значением, которое мы определили лямбда-выражения.</span><span class="sxs-lookup"><span data-stu-id="3ff51-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="3ff51-164">В случае выше мы загрузке объекта жанр со значением имя сопоставления Disco.</span><span class="sxs-lookup"><span data-stu-id="3ff51-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="3ff51-165">Мы будет использовать преимущества компонента платформы Entity Framework, позволяющий указывать другие связанные сущности, нужным нам также загрузить при извлечении объекта жанра.</span><span class="sxs-lookup"><span data-stu-id="3ff51-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="3ff51-166">Эта функция называется формирование результата запроса и позволило сократить число попыток, нам нужно обращаться к базе данных для получения всех сведений, которые необходимы.</span><span class="sxs-lookup"><span data-stu-id="3ff51-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="3ff51-167">Мы хотим упреждающую выборку альбомы жанра, получим, поэтому мы обновим наши запрос, чтобы включить из Genres.Include("Albums"), чтобы указать, что мы также связанные альбомы.</span><span class="sxs-lookup"><span data-stu-id="3ff51-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="3ff51-168">Это более эффективно, поскольку извлечет нашей жанр и альбом данные в запросе на одну базу данных.</span><span class="sxs-lookup"><span data-stu-id="3ff51-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="3ff51-169">В сторону там в этом разделе представлен нашей обновленные действие контроллера просмотра:</span><span class="sxs-lookup"><span data-stu-id="3ff51-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="3ff51-170">Теперь мы можем обновить хранилище Просмотр представления для отображения альбомы, которые доступны в каждом жанре.</span><span class="sxs-lookup"><span data-stu-id="3ff51-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="3ff51-171">Откройте шаблон представления (найден в /Views/Store/Browse.cshtml) и добавьте маркированный список альбомов, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="3ff51-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="3ff51-172">Запуск приложения и перейдите в/Store/обзора? жанр = Jazz показывает, из базы данных, отображение всех альбомов нашей выбранного жанра теперь извлекаемых результатов.</span><span class="sxs-lookup"><span data-stu-id="3ff51-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="3ff51-173">Мы выполним те же изменения наших "/ store" / Подробности / [id] URL-адрес и замените нашей фиктивные данные запроса к базе данных, который загружает альбома, идентификатор которого соответствует значению параметра.</span><span class="sxs-lookup"><span data-stu-id="3ff51-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="3ff51-174">Запуск приложения и перейдя к /Store/Details/1 показывает, что наши результаты теперь извлекаемых из базы данных.</span><span class="sxs-lookup"><span data-stu-id="3ff51-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="3ff51-175">Теперь, когда Настройка нашу страницу сведения о хранилище для отображения альбом с помощью идентификатора альбома, давайте обновление **Обзор** представление для связи в представлении сведений.</span><span class="sxs-lookup"><span data-stu-id="3ff51-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="3ff51-176">Мы будем использовать Html.ActionLink, точно так, как мы сделали ссылки из индекса хранилища для просмотра хранилища в конце предыдущего раздела.</span><span class="sxs-lookup"><span data-stu-id="3ff51-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="3ff51-177">Исходные данные в режиме просмотра отображается ниже.</span><span class="sxs-lookup"><span data-stu-id="3ff51-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="3ff51-178">Мы можем Теперь перейдите из нашей странице хранилища странице жанра, которые перечислены доступные диски, и щелкнув альбом можно просмотреть подробные сведения об этом альбоме.</span><span class="sxs-lookup"><span data-stu-id="3ff51-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

>[!div class="step-by-step"]
<span data-ttu-id="3ff51-179">[Назад](mvc-music-store-part-3.md)
[Вперед](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="3ff51-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
