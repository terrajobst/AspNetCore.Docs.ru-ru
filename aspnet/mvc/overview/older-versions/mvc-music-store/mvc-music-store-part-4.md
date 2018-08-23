---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Часть 4: Модели и доступ к данным | Документация Майкрософт'
author: jongalloway
description: В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store. Часть 4 описывает моделей и доступа к данным.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 6a07bf6c8a3fb926ae25fe1f6c9359e64cd7a290
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837866"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="98c14-104">Часть 4: Модели и доступ к данным</span><span class="sxs-lookup"><span data-stu-id="98c14-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="98c14-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="98c14-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="98c14-106">Store музыки MVC является учебного приложения, которое вводятся и описываются пошаговые использование ASP.NET MVC и Visual Studio для разработки веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="98c14-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="98c14-107">Store музыки MVC — это реализация хранилища упрощенный пример, который продает музыкальные альбомы через Интернет и реализует базовый сайт администрирования, вход пользователя и корзины для покупок функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="98c14-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="98c14-108">В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="98c14-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="98c14-109">Часть 4 описывает моделей и доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="98c14-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="98c14-110">На данный момент мы просто передача «фиктивных данных» с нашей контроллеров в наших шаблонов представлений.</span><span class="sxs-lookup"><span data-stu-id="98c14-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="98c14-111">Теперь мы готовы для подключения к любой настоящей базе данных.</span><span class="sxs-lookup"><span data-stu-id="98c14-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="98c14-112">В этом руководстве мы будем рассматривать способы использования SQL Server Compact Edition (часто называемые SQL CE) как наша подсистема базы данных.</span><span class="sxs-lookup"><span data-stu-id="98c14-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="98c14-113">SQL CE — это бесплатная, внедренная, файловая база данных, не требующий любой установки или настройки, что делает его очень удобным для локальной разработки.</span><span class="sxs-lookup"><span data-stu-id="98c14-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="98c14-114">Доступ к базе данных с Entity Framework Code-First</span><span class="sxs-lookup"><span data-stu-id="98c14-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="98c14-115">Мы будем использовать поддержки Entity Framework (EF), который включен в проектах ASP.NET MVC 3 для запроса и обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="98c14-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="98c14-116">EF является гибкой объектом реляционного сопоставления (ORM) данных API, который позволяет разработчикам запрашивать и обновлять данные, хранящиеся в базе данных, в объектно ориентированному подходу.</span><span class="sxs-lookup"><span data-stu-id="98c14-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="98c14-117">Платформа Entity Framework версии 4 поддерживает парадигму разработки, называется code first.</span><span class="sxs-lookup"><span data-stu-id="98c14-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="98c14-118">Code first позволяет создать объект модели, написав простые классы (также известный как POCO из объектов среды CLR «plain old») и даже может создать базу данных в режиме реального времени из классов.</span><span class="sxs-lookup"><span data-stu-id="98c14-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="98c14-119">Изменения в классах модели</span><span class="sxs-lookup"><span data-stu-id="98c14-119">Changes to our Model Classes</span></span>

<span data-ttu-id="98c14-120">Мы будем использовать функцию создания базы данных в Entity Framework в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="98c14-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="98c14-121">Перед этим, однако сделаем с незначительными изменениями для наших классов модели, чтобы добавить несколько действий, которые мы будем использовать позже.</span><span class="sxs-lookup"><span data-stu-id="98c14-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="98c14-122">Добавление классов модели исполнителя</span><span class="sxs-lookup"><span data-stu-id="98c14-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="98c14-123">Наши альбомов будет связан с исполнители, поэтому мы добавим простой класс модели для описания художника.</span><span class="sxs-lookup"><span data-stu-id="98c14-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="98c14-124">Добавьте новый класс с именем Artist.cs, используя приведенный ниже код папке «модели».</span><span class="sxs-lookup"><span data-stu-id="98c14-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="98c14-125">Обновление наших классов модели</span><span class="sxs-lookup"><span data-stu-id="98c14-125">Updating our Model Classes</span></span>

<span data-ttu-id="98c14-126">Обновите класс альбома, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="98c14-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="98c14-127">Затем внесите следующие изменения в класс жанра.</span><span class="sxs-lookup"><span data-stu-id="98c14-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="98c14-128">Добавление приложения\_папка данных</span><span class="sxs-lookup"><span data-stu-id="98c14-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="98c14-129">Мы добавим приложение\_каталог данных для нашего проекта для хранения файлов базы данных SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="98c14-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="98c14-130">Приложение\_данных представляет собой специальный каталог, в ASP.NET, который уже имеет разрешения безопасности доступа для доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="98c14-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="98c14-131">В меню «Проект» выберите Добавить папку ASP.NET, а затем приложение\_данных.</span><span class="sxs-lookup"><span data-stu-id="98c14-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="98c14-132">Создание строки подключения в файле web.config</span><span class="sxs-lookup"><span data-stu-id="98c14-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="98c14-133">Мы добавим несколько строк в файле конфигурации веб сайта, чтобы платформа Entity Framework знает, как подключиться к базе данных.</span><span class="sxs-lookup"><span data-stu-id="98c14-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="98c14-134">Дважды щелкните файл Web.config, расположенный в корневой папке проекта.</span><span class="sxs-lookup"><span data-stu-id="98c14-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="98c14-135">Прокрутите до нижней части этого файла и добавьте &lt;connectionStrings&gt; разделе непосредственно над последней строки, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="98c14-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="98c14-136">Добавление класса контекста</span><span class="sxs-lookup"><span data-stu-id="98c14-136">Adding a Context Class</span></span>

<span data-ttu-id="98c14-137">Щелкните правой кнопкой мыши папку Models и добавьте новый класс с именем MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="98c14-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="98c14-138">Этот класс будет представляет контекст базы данных Entity Framework и будет обрабатывать наших создание, чтение, обновление и операций удаления для нас.</span><span class="sxs-lookup"><span data-stu-id="98c14-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="98c14-139">Ниже приведен код для этого класса.</span><span class="sxs-lookup"><span data-stu-id="98c14-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="98c14-140">Вот и все — нет, не другие конфигурации, специальных интерфейсов, и т.д. Путем расширения базового класса DbContext, наш класс MusicStoreEntities возможность обрабатывать операций базы данных для нас.</span><span class="sxs-lookup"><span data-stu-id="98c14-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="98c14-141">Теперь, когда у нас есть, подключить, давайте добавим несколько свойств для наших классов модели, чтобы воспользоваться преимуществами некоторых дополнительных сведений в базе данных.</span><span class="sxs-lookup"><span data-stu-id="98c14-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="98c14-142">Добавление наших хранения данных каталога</span><span class="sxs-lookup"><span data-stu-id="98c14-142">Adding our store catalog data</span></span>

<span data-ttu-id="98c14-143">Мы будет воспользоваться функцией в Entity Framework, который добавляет «начальное значение» данных вновь созданную базу данных.</span><span class="sxs-lookup"><span data-stu-id="98c14-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="98c14-144">Это будет предварительно указано наш каталог хранилища со списком жанров, исполнителей и альбомы.</span><span class="sxs-lookup"><span data-stu-id="98c14-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="98c14-145">Скачивание MvcMusicStore Assets.zip - включавший наши файлы узла разработки, использованные ранее в этом руководстве - имеет файл класса с этими данными начальное значение, расположенный в папке кода.</span><span class="sxs-lookup"><span data-stu-id="98c14-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="98c14-146">В коде / папку Models, найдите файл SampleData.cs и поместите его в папку Models в нашем проекте, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="98c14-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="98c14-147">Теперь необходимо добавить одну строку кода, нужно сообщить об этом классе SampleData Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="98c14-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="98c14-148">Дважды щелкните файл Global.asax в корневой папке проекта, чтобы открыть его и добавьте следующие строки в начало приложения\_метод Start.</span><span class="sxs-lookup"><span data-stu-id="98c14-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="98c14-149">На этом этапе мы завершили работу действия, необходимые для настройки Entity Framework для проекта.</span><span class="sxs-lookup"><span data-stu-id="98c14-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="98c14-150">Запрос к базе данных</span><span class="sxs-lookup"><span data-stu-id="98c14-150">Querying the Database</span></span>

<span data-ttu-id="98c14-151">Теперь давайте обновим наши StoreController таким образом, вместо того чтобы использовать «пустой данных» вместо этого он вызывает в нашу базу данных, чтобы запросить все его данные.</span><span class="sxs-lookup"><span data-stu-id="98c14-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="98c14-152">Мы начнем с объявления поля на **StoreController** для хранения экземпляра класса MusicStoreEntities, с именем storeDB:</span><span class="sxs-lookup"><span data-stu-id="98c14-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="98c14-153">Обновления индекса в базе данных Store</span><span class="sxs-lookup"><span data-stu-id="98c14-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="98c14-154">Класс MusicStoreEntities поддерживается платформой Entity Framework и предоставляет свойство коллекции для каждой таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="98c14-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="98c14-155">Давайте обновим наши StoreController действие индекса для получения всех жанров в нашей базе данных.</span><span class="sxs-lookup"><span data-stu-id="98c14-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="98c14-156">Ранее мы сделали это жесткое программирование строковых данных.</span><span class="sxs-lookup"><span data-stu-id="98c14-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="98c14-157">Теперь можно вместо этого просто использовать контекст Entity Framework Generes коллекции:</span><span class="sxs-lookup"><span data-stu-id="98c14-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="98c14-158">Не требуется изменения случаться к шаблону представления, поскольку мы по-прежнему возвращая же StoreIndexViewModel, мы вернули перед — мы просто возвращая реальные данные из базы данных теперь.</span><span class="sxs-lookup"><span data-stu-id="98c14-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="98c14-159">Мы снова запустите проект и посетите URL-адреса «/ Store», мы теперь отображается список всех жанров в нашей базе данных:</span><span class="sxs-lookup"><span data-stu-id="98c14-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="98c14-160">Обновление Store Обзор и сведения, чтобы использовать актуальные данные</span><span class="sxs-lookup"><span data-stu-id="98c14-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="98c14-161">С помощью/Store/обзора? жанр =*[некоторые жанр]* метода действия, мы ищем жанр фильма по имени.</span><span class="sxs-lookup"><span data-stu-id="98c14-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="98c14-162">Ожидается только один результат, так как мы никогда не должны иметь две записи того же имени жанра, поэтому мы можем использовать. Расширение Single() в LINQ для запроса к соответствующему объекту жанра, следующим образом (не следует вводить это еще):</span><span class="sxs-lookup"><span data-stu-id="98c14-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="98c14-163">Один метод принимает как параметр, указывающий, что мы хотим объекта жанр, таким образом, чтобы его имя совпадает со значением, которое мы определили лямбда-выражение.</span><span class="sxs-lookup"><span data-stu-id="98c14-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="98c14-164">В приведенном выше примере мы загружаем объекта жанр с именем значение, соответствующее Disco.</span><span class="sxs-lookup"><span data-stu-id="98c14-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="98c14-165">Мы рассмотрим преимущества функцию Entity Framework, которая позволяет указать другие связанные сущности, которые требуется загрузить также при извлечении объекта жанра.</span><span class="sxs-lookup"><span data-stu-id="98c14-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="98c14-166">Эта функция вызывается формирование результатов запроса и позволило сократить количество раз, нам нужно получить доступ к базе данных для получения всех данных, что нужно.</span><span class="sxs-lookup"><span data-stu-id="98c14-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="98c14-167">Мы хотим упреждающую выборку альбомов для жанра, мы получаем, поэтому мы обновим наш запрос для включения Genres.Include("Albums"), чтобы указать, что мы хотим, а также связанные альбомы.</span><span class="sxs-lookup"><span data-stu-id="98c14-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="98c14-168">Это более эффективно, так как он извлечет как наш жанра, так и на дисках данных в запросе отдельной базы данных.</span><span class="sxs-lookup"><span data-stu-id="98c14-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="98c14-169">С объяснениями в сторону Вот, как выглядит нашей обновленной действие контроллера просмотра:</span><span class="sxs-lookup"><span data-stu-id="98c14-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="98c14-170">Теперь можно обновлять Store Обзор представления для отображения альбомы, которые доступны в каждом жанре.</span><span class="sxs-lookup"><span data-stu-id="98c14-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="98c14-171">Откройте этот шаблон (в /Views/Store/Browse.cshtml) и добавьте маркированный список альбомов, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="98c14-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="98c14-172">Запуск приложения и выбрав/Store/обзора? жанр = Jazz показывает, наши результаты теперь извлекаемых из базы данных, отображение всех альбомов в нашей выбранного жанра.</span><span class="sxs-lookup"><span data-stu-id="98c14-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="98c14-173">Сделаем те же изменения наш/Store/сведения / [id] URL-адрес и замените нашей фиктивными данными запроса к базе данных, который загружает альбом, идентификатор которого соответствует значению параметра.</span><span class="sxs-lookup"><span data-stu-id="98c14-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="98c14-174">Запуск приложения и выбрав /Store/Details/1 показывает, что результаты теперь извлекаемых из базы данных.</span><span class="sxs-lookup"><span data-stu-id="98c14-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="98c14-175">Теперь, когда нашей странице сведений о Store предназначена для отображения альбом по Идентификатору альбома, давайте обновим **Обзор** представление для связи в представлении сведений.</span><span class="sxs-lookup"><span data-stu-id="98c14-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="98c14-176">Мы будем использовать Html.ActionLink, именно так, как мы сделали ссылки из Store индекс для просмотра Store в конце предыдущего раздела.</span><span class="sxs-lookup"><span data-stu-id="98c14-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="98c14-177">Полный вариант исходного кода для представления обзора появляется.</span><span class="sxs-lookup"><span data-stu-id="98c14-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="98c14-178">Мы теперь возможность перейти с нашей страницы Store к странице жанра, которые перечислены доступные альбомов, и, щелкнув альбом можно просмотреть подробные сведения об этом альбоме.</span><span class="sxs-lookup"><span data-stu-id="98c14-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="98c14-179">[Назад](mvc-music-store-part-3.md)
> [Вперед](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="98c14-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
