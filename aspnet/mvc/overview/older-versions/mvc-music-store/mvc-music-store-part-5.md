---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Часть 5: Изменение формы и шаблонов. | Документы Microsoft'
author: jongalloway
description: Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store. Часть 5 описываются изменения форм и шаблонов.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: d584e614b5a4124044cd9decd2272192ca164643
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="part-5-edit-forms-and-templating"></a><span data-ttu-id="ba103-104">Часть 5: Изменение формы и шаблонов.</span><span class="sxs-lookup"><span data-stu-id="ba103-104">Part 5: Edit Forms and Templating</span></span>
====================
<span data-ttu-id="ba103-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="ba103-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="ba103-106">MVC Music Store является учебное приложение, которое вводятся и описываются пошаговые способы использования Visual Studio и ASP.NET MVC для веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="ba103-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="ba103-107">MVC Music Store показана реализация хранилища упрощенных образец продает велосипеды через Интернет альбомы, которое реализует Администрирование сайта основные, вход пользователей и функциональные возможности корзины покупок.</span><span class="sxs-lookup"><span data-stu-id="ba103-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="ba103-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="ba103-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="ba103-109">Часть 5 описываются изменения форм и шаблонов.</span><span class="sxs-lookup"><span data-stu-id="ba103-109">Part 5 covers Edit Forms and Templating.</span></span>


<span data-ttu-id="ba103-110">В прошлом главе мы загрузка данных из наших базы данных и его отображения.</span><span class="sxs-lookup"><span data-stu-id="ba103-110">In the past chapter, we were loading data from our database and displaying it.</span></span> <span data-ttu-id="ba103-111">В этой главе мы будем также позволяет редактировать данные.</span><span class="sxs-lookup"><span data-stu-id="ba103-111">In this chapter, we'll also enable editing the data.</span></span>

## <a name="creating-the-storemanagercontroller"></a><span data-ttu-id="ba103-112">Создание StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="ba103-112">Creating the StoreManagerController</span></span>

<span data-ttu-id="ba103-113">Для начала нужно создать новый контроллер с именем **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="ba103-113">We'll begin by creating a new controller called **StoreManagerController**.</span></span> <span data-ttu-id="ba103-114">Для этого контроллера мы будет преимущества формирование шаблонов функции, доступные в ASP.NET MVC 3 Tools Update.</span><span class="sxs-lookup"><span data-stu-id="ba103-114">For this controller, we will be taking advantage of the Scaffolding features available in the ASP.NET MVC 3 Tools Update.</span></span> <span data-ttu-id="ba103-115">Укажите параметры в диалоговом окне Добавить контроллер, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="ba103-115">Set the options for the Add Controller dialog as shown below.</span></span>

![](mvc-music-store-part-5/_static/image1.png)

<span data-ttu-id="ba103-116">При нажатии кнопки "Добавить", вы увидите, что механизм формирования шаблонов ASP.NET MVC 3 не достаточно большой объем работы для вас:</span><span class="sxs-lookup"><span data-stu-id="ba103-116">When you click the Add button, you'll see that the ASP.NET MVC 3 scaffolding mechanism does a good amount of work for you:</span></span>

- <span data-ttu-id="ba103-117">Создает новый StoreManagerController с локальной переменной Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ba103-117">It creates the new StoreManagerController with a local Entity Framework variable</span></span>
- <span data-ttu-id="ba103-118">Он добавляет StoreManager папку в папке представления проекта</span><span class="sxs-lookup"><span data-stu-id="ba103-118">It adds a StoreManager folder to the project's Views folder</span></span>
- <span data-ttu-id="ba103-119">Он добавляет Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml и Index.cshtml представление со строго типизированными в альбоме-класс</span><span class="sxs-lookup"><span data-stu-id="ba103-119">It adds Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml, and Index.cshtml view, strongly typed to the Album class</span></span>

![](mvc-music-store-part-5/_static/image2.png)

<span data-ttu-id="ba103-120">Новый класс контроллера StoreManager включает CRUD (Создание, чтение, обновление и удаление) действия контроллера, которые умеют работать с альбом класс модели и использовать наш контекст Entity Framework для доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="ba103-120">The new StoreManager controller class includes CRUD (create, read, update, delete) controller actions which know how to work with the Album model class and use our Entity Framework context for database access.</span></span>

## <a name="modifying-a-scaffolded-view"></a><span data-ttu-id="ba103-121">Изменение представления формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="ba103-121">Modifying a Scaffolded View</span></span>

<span data-ttu-id="ba103-122">Это важно помнить, что, хотя этот код был создан для нас, это стандартный код ASP.NET MVC, так же, как мы написание учебником.</span><span class="sxs-lookup"><span data-stu-id="ba103-122">It's important to remember that, while this code was generated for us, it's standard ASP.NET MVC code, just like we've been writing throughout this tutorial.</span></span> <span data-ttu-id="ba103-123">Он предназначен для сохранения будут тратить время на написание стандартный код контроллера и создание строго типизированные представления вручную, но это не тип созданного кода, вы могли увидеть предшествовало dire предупреждений в комментарии о том, как должен изменить код.</span><span class="sxs-lookup"><span data-stu-id="ba103-123">It's intended to save you the time you'd spend on writing boilerplate controller code and creating the strongly typed views manually, but this isn't the kind of generated code you may have seen prefaced with dire warnings in comments about how you mustn't change the code.</span></span> <span data-ttu-id="ba103-124">Это код, и вы должны изменить его.</span><span class="sxs-lookup"><span data-stu-id="ba103-124">This is your code, and you're expected to change it.</span></span>

<span data-ttu-id="ba103-125">Итак начнем с изменением краткое представление индекса StoreManager (/ Views/StoreManager/Index.cshtml).</span><span class="sxs-lookup"><span data-stu-id="ba103-125">So, let's start with a quick edit to the StoreManager Index view (/Views/StoreManager/Index.cshtml).</span></span> <span data-ttu-id="ba103-126">Это представление отображает таблицу, которая перечислены альбомов в нашем хранилище с редактированием / сведений / удаление связей и включает общие свойства альбома.</span><span class="sxs-lookup"><span data-stu-id="ba103-126">This view will display a table which lists the Albums in our store with Edit / Details / Delete links, and includes the Album's public properties.</span></span> <span data-ttu-id="ba103-127">Мы удалим поле AlbumArtUrl, как она не очень полезен в этом окне.</span><span class="sxs-lookup"><span data-stu-id="ba103-127">We'll remove the AlbumArtUrl field, as it's not very useful in this display.</span></span> <span data-ttu-id="ba103-128">В &lt;таблицы&gt; раздел представление кода, удалите &lt;й&gt; и &lt;td&gt; элементы вокруг AlbumArtUrl ссылки, как указано в выделенные строки ниже:</span><span class="sxs-lookup"><span data-stu-id="ba103-128">In &lt;table&gt; section of the view code, remove the &lt;th&gt; and &lt;td&gt; elements surrounding AlbumArtUrl references, as indicated by the highlighted lines below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

<span data-ttu-id="ba103-129">Измененный просмотреть код будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ba103-129">The modified view code will appear as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a><span data-ttu-id="ba103-130">Первый взгляд на диспетчер хранилища</span><span class="sxs-lookup"><span data-stu-id="ba103-130">A first look at the Store Manager</span></span>

<span data-ttu-id="ba103-131">Теперь запустите приложение и найдите/StoreManager /.</span><span class="sxs-lookup"><span data-stu-id="ba103-131">Now run the application and browse to /StoreManager/.</span></span> <span data-ttu-id="ba103-132">При этом отображаются измененную, отображая список дисках в хранилище со ссылками на, редактирования и удаления индекса диспетчер хранения.</span><span class="sxs-lookup"><span data-stu-id="ba103-132">This displays the Store Manager Index we just modified, showing a list of the albums in the store with links to Edit, Details, and Delete.</span></span>

![](mvc-music-store-part-5/_static/image3.png)

<span data-ttu-id="ba103-133">Щелкнув ссылку "Изменить" показывает изменить форму с полями альбома, включая раскрывающиеся меню для жанр и исполнитель.</span><span class="sxs-lookup"><span data-stu-id="ba103-133">Clicking the Edit link displays an edit form with fields for the Album, including dropdowns for Genre and Artist.</span></span>

![](mvc-music-store-part-5/_static/image4.png)

<span data-ttu-id="ba103-134">Щелкните ссылку внизу «Обратно в список», а затем щелкните ссылку сведений для альбома.</span><span class="sxs-lookup"><span data-stu-id="ba103-134">Click the "Back to List" link at the bottom, then click on the Details link for an Album.</span></span> <span data-ttu-id="ba103-135">Это отображение подробных сведений для отдельных альбома.</span><span class="sxs-lookup"><span data-stu-id="ba103-135">This displays the detail information for an individual Album.</span></span>

![](mvc-music-store-part-5/_static/image5.png)

<span data-ttu-id="ba103-136">Еще раз нажмите кнопку '' Назад в список ссылок, а затем щелкните ссылку, удалить.</span><span class="sxs-lookup"><span data-stu-id="ba103-136">Again, click the Back to List link, then click on a Delete link.</span></span> <span data-ttu-id="ba103-137">Откроется диалоговое окно подтверждения, показывает сведения об альбоме и запросом, если мы уверены, что мы хотим удалить его.</span><span class="sxs-lookup"><span data-stu-id="ba103-137">This displays a confirmation dialog, showing the album details and asking if we're sure we want to delete it.</span></span>

![](mvc-music-store-part-5/_static/image6.png)

<span data-ttu-id="ba103-138">Нажав кнопку «Удалить» в нижней удалить альбом и возврата на страницу индекса, которая показывает альбом удален.</span><span class="sxs-lookup"><span data-stu-id="ba103-138">Clicking the Delete button at the bottom will delete the album and return you to the Index page, which shows the album deleted.</span></span>

<span data-ttu-id="ba103-139">Не готово с помощью диспетчера хранилища, но у нас есть работа контроллера и просмотр кода для операций CRUD запуск из.</span><span class="sxs-lookup"><span data-stu-id="ba103-139">We're not done with the Store Manager, but we have working controller and view code for the CRUD operations to start from.</span></span>

## <a name="looking-at-the-store-manager-controller-code"></a><span data-ttu-id="ba103-140">Просматривая код контроллера диспетчера хранилища</span><span class="sxs-lookup"><span data-stu-id="ba103-140">Looking at the Store Manager Controller code</span></span>

<span data-ttu-id="ba103-141">Контроллер диспетчера хранилища содержит достаточно большой объем кода.</span><span class="sxs-lookup"><span data-stu-id="ba103-141">The Store Manager Controller contains a good amount of code.</span></span> <span data-ttu-id="ba103-142">Разберем это сверху вниз.</span><span class="sxs-lookup"><span data-stu-id="ba103-142">Let's go through this from top to bottom.</span></span> <span data-ttu-id="ba103-143">Контроллер содержит некоторые стандартные пространства имен для контроллера MVC, а также ссылку на нашем пространстве имен модели.</span><span class="sxs-lookup"><span data-stu-id="ba103-143">The controller includes some standard namespaces for an MVC controller, as well as a reference to our Models namespace.</span></span> <span data-ttu-id="ba103-144">Контроллер имеет закрытый экземпляр MusicStoreEntities, используемых каждым из действия контроллера для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="ba103-144">The controller has a private instance of MusicStoreEntities, used by each of the controller actions for data access.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a><span data-ttu-id="ba103-145">Хранить действия диспетчера индекса и подробные сведения</span><span class="sxs-lookup"><span data-stu-id="ba103-145">Store Manager Index and Details actions</span></span>

<span data-ttu-id="ba103-146">Представление index извлекает список альбомов, включая каждого альбома упоминаемого жанр и исполнитель сведения, как упоминалось ранее, при работе в методе Обзор хранилища.</span><span class="sxs-lookup"><span data-stu-id="ba103-146">The index view retrieves a list of Albums, including each album's referenced Genre and Artist information, as we previously saw when working on the Store Browse method.</span></span> <span data-ttu-id="ba103-147">Просмотр индекса является следующие ссылки на связанные объекты, чтобы он может отображать жанр имя каждого альбома, а также имя исполнителя, контроллер, эффективность и запросов для получения этих сведений в исходном запросе.</span><span class="sxs-lookup"><span data-stu-id="ba103-147">The Index view is following the references to the linked objects so that it can display each album's Genre name and Artist name, so the controller is being efficient and querying for this information in the original request.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

<span data-ttu-id="ba103-148">Действие контроллера сведения контроллера StoreManager работает так же, как мы писали ранее - она запрашивает альбом действие хранилища сведений о контроллере по Идентификатору с помощью метода применения Find(), возвращается в представлении.</span><span class="sxs-lookup"><span data-stu-id="ba103-148">The StoreManager Controller's Details controller action works exactly the same as the Store Controller Details action we wrote previously - it queries for the Album by ID using the Find() method, then returns it to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a><span data-ttu-id="ba103-149">Создавать методы действий</span><span class="sxs-lookup"><span data-stu-id="ba103-149">The Create Action Methods</span></span>

<span data-ttu-id="ba103-150">Создавать методы действий немного отличаются от те, которые мы наблюдали на данный момент, так как они обрабатывают входные данные формы.</span><span class="sxs-lookup"><span data-stu-id="ba103-150">The Create action methods are a little different from ones we've seen so far, because they handle form input.</span></span> <span data-ttu-id="ba103-151">При первом посещении /StoreManager/создать/они будут показаны пустую форму.</span><span class="sxs-lookup"><span data-stu-id="ba103-151">When a user first visits /StoreManager/Create/ they will be shown an empty form.</span></span> <span data-ttu-id="ba103-152">Эта страница HTML будет содержать &lt;формы&gt; элемент, который содержит раскрывающийся список и текстовое поле входных элементов, где они могут ввести сведения о диске.</span><span class="sxs-lookup"><span data-stu-id="ba103-152">This HTML page will contain a &lt;form&gt; element that contains dropdown and textbox input elements where they can enter the album's details.</span></span>

<span data-ttu-id="ba103-153">После пользователь заполняет значения формы альбома, их можно нажать кнопку «Сохранить», чтобы отправить эти изменения обратно в наше приложение для сохранения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ba103-153">After the user fills in the Album form values, they can press the "Save" button to submit these changes back to our application to save within the database.</span></span> <span data-ttu-id="ba103-154">Когда пользователь нажимает кнопку «Сохранить» &lt;формы&gt; выполнит запрос HTTP POST к /StoreManager/создать/URL-адрес и отправить &lt;формы&gt; значения как часть HTTP-запроса POST.</span><span class="sxs-lookup"><span data-stu-id="ba103-154">When the user presses the "save" button the &lt;form&gt; will perform an HTTP-POST back to the /StoreManager/Create/ URL and submit the &lt;form&gt; values as part of the HTTP-POST.</span></span>

<span data-ttu-id="ba103-155">ASP.NET MVC позволяет легко разделить логику эти два сценария вызова URL-адрес, позволяющих реализовать два отдельных метода действия «Создать» в нашем StoreManagerController класса: один для обработки начальной Обзор HTTP-GET для /StoreManager/Create / URL-адрес, а другая — для обработки HTTP-POST отправляемых изменений.</span><span class="sxs-lookup"><span data-stu-id="ba103-155">ASP.NET MVC allows us to easily split up the logic of these two URL invocation scenarios by enabling us to implement two separate "Create" action methods within our StoreManagerController class – one to handle the initial HTTP-GET browse to the /StoreManager/Create/ URL, and the other to handle the HTTP-POST of the submitted changes.</span></span>

### <a name="passing-information-to-a-view-using-viewbag"></a><span data-ttu-id="ba103-156">Представления с помощью ViewBag передачи данных</span><span class="sxs-lookup"><span data-stu-id="ba103-156">Passing information to a View using ViewBag</span></span>

<span data-ttu-id="ba103-157">Мы использовали ViewBag ранее в этом учебнике, но еще не говорили большая о нем.</span><span class="sxs-lookup"><span data-stu-id="ba103-157">We've used the ViewBag earlier in this tutorial, but haven't talked much about it.</span></span> <span data-ttu-id="ba103-158">ViewBag позволяет передавать сведения в представлении без использования объекта строго типизированные модели.</span><span class="sxs-lookup"><span data-stu-id="ba103-158">The ViewBag allows us to pass information to the view without using a strongly typed model object.</span></span> <span data-ttu-id="ba103-159">В этом случае необходимо передать список жанров и исполнители форму для заполнения в раскрывающихся списках наши действия контроллера изменить HTTP-GET, и для этого проще всего возврата их как элементы ViewBag.</span><span class="sxs-lookup"><span data-stu-id="ba103-159">In this case, our Edit HTTP-GET controller action needs to pass both a list of Genres and Artists to the form to populate the dropdowns, and the simplest way to do that is to return them as ViewBag items.</span></span>

<span data-ttu-id="ba103-160">ViewBag является динамическим объектом, это означает, что можно ввести ViewBag.Foo или ViewBag.YourNameHere без написания кода для определения этих свойств.</span><span class="sxs-lookup"><span data-stu-id="ba103-160">The ViewBag is a dynamic object, meaning that you can type ViewBag.Foo or ViewBag.YourNameHere without writing code to define those properties.</span></span> <span data-ttu-id="ba103-161">В этом случае код контроллера использует ViewBag.GenreId и ViewBag.ArtistId так, чтобы раскрывающийся список значений, отправленные с формой GenreId и ArtistId, которые являются свойства альбома, который будет устанавливаться.</span><span class="sxs-lookup"><span data-stu-id="ba103-161">In this case, the controller code uses ViewBag.GenreId and ViewBag.ArtistId so that the dropdown values submitted with the form will be GenreId and ArtistId, which are the Album properties they will be setting.</span></span>

<span data-ttu-id="ba103-162">Эти значения в раскрывающемся списке возвращаются к форме с помощью объекта SelectList, который создан для этой цели.</span><span class="sxs-lookup"><span data-stu-id="ba103-162">These dropdown values are returned to the form using the SelectList object, which is built just for that purpose.</span></span> <span data-ttu-id="ba103-163">Это делается с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="ba103-163">This is done using code like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

<span data-ttu-id="ba103-164">Как видно из кода метода действия, чтобы создать этот объект используются три параметра:</span><span class="sxs-lookup"><span data-stu-id="ba103-164">As you can see from the action method code, three parameters are being used to create this object:</span></span>

- <span data-ttu-id="ba103-165">Список элементов, которые будут применяться для отображения раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="ba103-165">The list of items the dropdown will be displaying.</span></span> <span data-ttu-id="ba103-166">Обратите внимание, что это не является строкой - передается список жанров.</span><span class="sxs-lookup"><span data-stu-id="ba103-166">Note that this isn't just a string - we're passing a list of Genres.</span></span>
- <span data-ttu-id="ba103-167">Следующий параметр, передаваемые SelectList является выбранное значение.</span><span class="sxs-lookup"><span data-stu-id="ba103-167">The next parameter being passed to the SelectList is the Selected Value.</span></span> <span data-ttu-id="ba103-168">Этот способ SelectList знает, как предварительно выбрать элемент в списке.</span><span class="sxs-lookup"><span data-stu-id="ba103-168">This how the SelectList knows how to pre-select an item in the list.</span></span> <span data-ttu-id="ba103-169">Это будет проще для понимания, если взглянуть на форме редактирования, которая очень похожа.</span><span class="sxs-lookup"><span data-stu-id="ba103-169">This will be easier to understand when we look at the Edit form, which is pretty similar.</span></span>
- <span data-ttu-id="ba103-170">Последний параметр является свойство для отображения.</span><span class="sxs-lookup"><span data-stu-id="ba103-170">The final parameter is the property to be displayed.</span></span> <span data-ttu-id="ba103-171">В этом случае это указывает, что свойство Genre.Name какие данные будут отображаться для пользователя.</span><span class="sxs-lookup"><span data-stu-id="ba103-171">In this case, this is indicating that the Genre.Name property is what will be shown to the user.</span></span>

<span data-ttu-id="ba103-172">С учетом затем очень простое действие создания HTTP-GET - ViewBag добавляются два SelectLists и ни один из объектов модели передается в форму (поскольку она не была создана ранее).</span><span class="sxs-lookup"><span data-stu-id="ba103-172">With that in mind, then, the HTTP-GET Create action is pretty simple - two SelectLists are added to the ViewBag, and no model object is passed to the form (since it hasn't been created yet).</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a><span data-ttu-id="ba103-173">Вспомогательные методы HTML для отображения списков Drop в Create View</span><span class="sxs-lookup"><span data-stu-id="ba103-173">HTML Helpers to display the Drop Downs in the Create View</span></span>

<span data-ttu-id="ba103-174">Поскольку мы обсуждали как раскрывающийся список значений передаются в представление, давайте кратко рассмотрим представление, чтобы увидеть, как отображаются значения.</span><span class="sxs-lookup"><span data-stu-id="ba103-174">Since we've talked about how the drop down values are passed to the view, let's take a quick look at the view to see how those values are displayed.</span></span> <span data-ttu-id="ba103-175">В представлении кода (/ Views/StoreManager/Create.cshtml), вы увидите следующий вызов для отображения раскрывающегося жанр вниз.</span><span class="sxs-lookup"><span data-stu-id="ba103-175">In the view code (/Views/StoreManager/Create.cshtml), you'll see the following call is made to display the Genre drop down.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

<span data-ttu-id="ba103-176">Это известно как вспомогательный метод HTML - вспомогательный метод, который выполняет типичную задачу представления.</span><span class="sxs-lookup"><span data-stu-id="ba103-176">This is known as an HTML Helper - a utility method which performs a common view task.</span></span> <span data-ttu-id="ba103-177">Вспомогательные методы HTML очень полезны в обеспечении наш код представление четкими и удобочитаемыми.</span><span class="sxs-lookup"><span data-stu-id="ba103-177">HTML Helpers are very useful in keeping our view code concise and readable.</span></span> <span data-ttu-id="ba103-178">Вспомогательный объект Html.DropDownList обеспечивается ASP.NET MVC, но как мы рассмотрим позже его можно создать собственную вспомогательные методы для представления кода, который мы будем повторно использовать в приложении.</span><span class="sxs-lookup"><span data-stu-id="ba103-178">The Html.DropDownList helper is provided by ASP.NET MVC, but as we'll see later it's possible to create our own helpers for view code we'll reuse in our application.</span></span>

<span data-ttu-id="ba103-179">Вызов Html.DropDownList просто должен знать две вещи - get списке для отображения и какие значения (если таковые имеются), необходимо предварительно выбрать.</span><span class="sxs-lookup"><span data-stu-id="ba103-179">The Html.DropDownList call just needs to be told two things - where to get the list to display, and what value (if any) should be pre-selected.</span></span> <span data-ttu-id="ba103-180">Первый параметр GenreId, сообщает DropDownList для поиска значения, с именем GenreId в модели или ViewBag.</span><span class="sxs-lookup"><span data-stu-id="ba103-180">The first parameter, GenreId, tells the DropDownList to look for a value named GenreId in either the model or ViewBag.</span></span> <span data-ttu-id="ba103-181">Второй параметр используется для указания значения, чтобы показать, как изначально выбираются в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="ba103-181">The second parameter is used to indicate the value to show as initially selected in the drop down list.</span></span> <span data-ttu-id="ba103-182">Так как эта форма является создание формы, не имеет смысла для предварительно выбранные и передать String.Empty.</span><span class="sxs-lookup"><span data-stu-id="ba103-182">Since this form is a Create form, there's no value to be preselected and String.Empty is passed.</span></span>

### <a name="handling-the-posted-form-values"></a><span data-ttu-id="ba103-183">Обработка значения отправки формы</span><span class="sxs-lookup"><span data-stu-id="ba103-183">Handling the Posted Form values</span></span>

<span data-ttu-id="ba103-184">Как уже говорилось, прежде чем существует два метода действия, связанные с каждой формы.</span><span class="sxs-lookup"><span data-stu-id="ba103-184">As we discussed before, there are two action methods associated with each form.</span></span> <span data-ttu-id="ba103-185">Первый обрабатывает запрос HTTP GET и отображает форму.</span><span class="sxs-lookup"><span data-stu-id="ba103-185">The first handles the HTTP-GET request and displays the form.</span></span> <span data-ttu-id="ba103-186">Вторая обрабатывает запрос HTTP POST, содержащий значения отправленной формы.</span><span class="sxs-lookup"><span data-stu-id="ba103-186">The second handles the HTTP-POST request, which contains the submitted form values.</span></span> <span data-ttu-id="ba103-187">Обратите внимание, что действие контроллера имеет атрибут [HttpPost], который сообщает ASP.NET MVC, он должен отвечать только на запросы HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="ba103-187">Notice that controller action has an [HttpPost] attribute, which tells ASP.NET MVC that it should only respond to HTTP-POST requests.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

<span data-ttu-id="ba103-188">Это действие имеет четыре обязанности.</span><span class="sxs-lookup"><span data-stu-id="ba103-188">This action has four responsibilities:</span></span>

- 1. <span data-ttu-id="ba103-189">Чтение значения формы</span><span class="sxs-lookup"><span data-stu-id="ba103-189">Read the form values</span></span>
- 2. <span data-ttu-id="ba103-190">Проверьте, если значения формы передайте все правила проверки</span><span class="sxs-lookup"><span data-stu-id="ba103-190">Check if the form values pass any validation rules</span></span>
- 3. <span data-ttu-id="ba103-191">При отправке формы являются допустимыми, сохраните данные и отображать обновленный список</span><span class="sxs-lookup"><span data-stu-id="ba103-191">If the form submission is valid, save the data and display the updated list</span></span>
- 4. <span data-ttu-id="ba103-192">При отправке формы является недопустимым, отобразите формы с ошибками проверки</span><span class="sxs-lookup"><span data-stu-id="ba103-192">If the form submission is not valid, redisplay the form with validation errors</span></span>

#### <a name="reading-form-values-with-model-binding"></a><span data-ttu-id="ba103-193">Считывание значений формы с привязкой модели</span><span class="sxs-lookup"><span data-stu-id="ba103-193">Reading Form Values with Model Binding</span></span>

<span data-ttu-id="ba103-194">Обрабатывает отправки формы, который включает в себя значения GenreId и ArtistId (из раскрывающегося списка) и значения текстового поля заголовка, цену и AlbumArtUrl действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="ba103-194">The controller action is processing a form submission that includes values for GenreId and ArtistId (from the drop down list) and textbox values for Title, Price, and AlbumArtUrl.</span></span> <span data-ttu-id="ba103-195">Хотя можно получить прямой доступ к значениям формы, лучшим подходом является использование возможностей привязки модели ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ba103-195">While it's possible to directly access form values, a better approach is to use the Model Binding capabilities built into ASP.NET MVC.</span></span> <span data-ttu-id="ba103-196">Когда действие контроллера принимает тип модели в качестве параметра, ASP.NET MVC будет пытаться заполнение объекта этого типа с помощью входных (а также значения маршрута и строки запроса).</span><span class="sxs-lookup"><span data-stu-id="ba103-196">When a controller action takes a model type as a parameter, ASP.NET MVC will attempt to populate an object of that type using form inputs (as well as route and querystring values).</span></span> <span data-ttu-id="ba103-197">Это делается путем поиска значений, имена которых соответствуют свойствам объекта модели, например при установке нового альбома объекта GenreId значения, он ищет входных данных с именем GenreId.</span><span class="sxs-lookup"><span data-stu-id="ba103-197">It does this by looking for values whose names match properties of the model object, e.g. when setting the new Album object's GenreId value, it looks for an input with the name GenreId.</span></span> <span data-ttu-id="ba103-198">При создании представления, используя стандартные методы в ASP.NET MVC форм всегда отображается с помощью имен свойств как имена полей ввода, поэтому это поле будет просто соответствия имен.</span><span class="sxs-lookup"><span data-stu-id="ba103-198">When you create views using the standard methods in ASP.NET MVC, the forms will always be rendered using property names as input field names, so this the field names will just match up.</span></span>

#### <a name="validating-the-model"></a><span data-ttu-id="ba103-199">Проверка модели</span><span class="sxs-lookup"><span data-stu-id="ba103-199">Validating the Model</span></span>

<span data-ttu-id="ba103-200">Проверка модели выполнена с помощью простого вызова ModelState.IsValid.</span><span class="sxs-lookup"><span data-stu-id="ba103-200">The model is validated with a simple call to ModelState.IsValid.</span></span> <span data-ttu-id="ba103-201">Мы еще не добавили все правила проверки для нашего класса альбом еще - мы сделаем, в бит — поэтому прямо сейчас эта проверка не имеет большую делать.</span><span class="sxs-lookup"><span data-stu-id="ba103-201">We haven't added any validation rules to our Album class yet - we'll do that in a bit - so right now this check doesn't have much to do.</span></span> <span data-ttu-id="ba103-202">Важно то, что эта проверка ModelStat.IsValid будет адаптировать на правила проверки, который мы поместили в нашей модели, так и изменение правила проверки не требуются никакие обновления в код действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="ba103-202">What's important is that this ModelStat.IsValid check will adapt to the validation rules we put on our model, so future changes to validation rules won't require any updates to the controller action code.</span></span>

#### <a name="saving-the-submitted-values"></a><span data-ttu-id="ba103-203">Сохранение введенные значения</span><span class="sxs-lookup"><span data-stu-id="ba103-203">Saving the submitted values</span></span>

<span data-ttu-id="ba103-204">При отправке формы проходит проверку, пришло время для сохранения значений в базу данных.</span><span class="sxs-lookup"><span data-stu-id="ba103-204">If the form submission passes validation, it's time to save the values to the database.</span></span> <span data-ttu-id="ba103-205">С Entity Framework, требуется добавление модели в коллекцию альбомы и вызов SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="ba103-205">With Entity Framework, that just requires adding the model to the Albums collection and calling SaveChanges.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

<span data-ttu-id="ba103-206">Entity Framework создает соответствующие команды SQL для сохранения значения.</span><span class="sxs-lookup"><span data-stu-id="ba103-206">Entity Framework generates the appropriate SQL commands to persist the value.</span></span> <span data-ttu-id="ba103-207">После сохранения данных, мы перенаправление обратно к списку альбомы, мы видим наши обновления.</span><span class="sxs-lookup"><span data-stu-id="ba103-207">After saving the data, we redirect back to the list of Albums so we can see our update.</span></span> <span data-ttu-id="ba103-208">Это делается путем возвращения RedirectToAction с именем действия контроллера, который мы должны отображаться.</span><span class="sxs-lookup"><span data-stu-id="ba103-208">This is done by returning RedirectToAction with the name of the controller action we want displayed.</span></span> <span data-ttu-id="ba103-209">В этом случае это метод индекса.</span><span class="sxs-lookup"><span data-stu-id="ba103-209">In this case, that's the Index method.</span></span>

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a><span data-ttu-id="ba103-210">Отображение отправок недопустимую форму с ошибками проверки</span><span class="sxs-lookup"><span data-stu-id="ba103-210">Displaying invalid form submissions with Validation Errors</span></span>

<span data-ttu-id="ba103-211">В случае ввода недопустимая форма раскрывающийся список значений добавляются ViewBag (как в случае HTTP-GET) и привязанная модель значения передаются обратно в представление для отображения.</span><span class="sxs-lookup"><span data-stu-id="ba103-211">In the case of invalid form input, the dropdown values are added to the ViewBag (as in the HTTP-GET case) and the bound model values are passed back to the view for display.</span></span> <span data-ttu-id="ba103-212">Ошибки проверки автоматически отображаются с помощью @Html.ValidationMessageFor вспомогательный метод HTML.</span><span class="sxs-lookup"><span data-stu-id="ba103-212">Validation errors are automatically displayed using the @Html.ValidationMessageFor HTML Helper.</span></span>

#### <a name="testing-the-create-form"></a><span data-ttu-id="ba103-213">Тестирование Создание формы</span><span class="sxs-lookup"><span data-stu-id="ba103-213">Testing the Create Form</span></span>

<span data-ttu-id="ba103-214">Чтобы проверить это, выполнения приложения и перейдите к /StoreManager/создать / - здесь вы найдете пустую форму, который был возвращен методом StoreController создания HTTP-GET.</span><span class="sxs-lookup"><span data-stu-id="ba103-214">To test this out, run the application and browse to /StoreManager/Create/ - this will show you the blank form which was returned by the StoreController Create HTTP-GET method.</span></span>

<span data-ttu-id="ba103-215">Заполните некоторые значения и нажмите кнопку "Создать" для отправки формы.</span><span class="sxs-lookup"><span data-stu-id="ba103-215">Fill in some values and click the Create button to submit the form.</span></span>

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a><span data-ttu-id="ba103-216">Обработка изменений</span><span class="sxs-lookup"><span data-stu-id="ba103-216">Handling Edits</span></span>

<span data-ttu-id="ba103-217">Пара действие редактирования (HTTP-GET и HTTP POST) очень похожи на создавать методы действий, которые мы только что рассмотрели.</span><span class="sxs-lookup"><span data-stu-id="ba103-217">The Edit action pair (HTTP-GET and HTTP-POST) are very similar to the Create action methods we just looked at.</span></span> <span data-ttu-id="ba103-218">Так как изменить сценарий включает работу с существующие альбома, изменить HTTP-GET метод загружает альбома, на основе параметра «идентификатор», переданной через маршрут.</span><span class="sxs-lookup"><span data-stu-id="ba103-218">Since the edit scenario involves working with an existing album, the Edit HTTP-GET method loads the Album based on the "id" parameter, passed in via the route.</span></span> <span data-ttu-id="ba103-219">Этот код для извлечения и альбом AlbumId рассматривается как мы ранее рассмотрели сведения при выполнении действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="ba103-219">This code for retrieving an album by AlbumId is the same as we've previously looked at in the Details controller action.</span></span> <span data-ttu-id="ba103-220">Как и в случае с помощью команды Create / метода HTTP GET, раскрывающийся список значений возвращаются через ViewBag.</span><span class="sxs-lookup"><span data-stu-id="ba103-220">As with the Create / HTTP-GET method, the drop down values are returned via the ViewBag.</span></span> <span data-ttu-id="ba103-221">Это позволяет вернуться альбом как нашей модели объекта в представление (которая имеет строгий тип класса альбом) во время передачи дополнительных данных (например список жанров) через ViewBag.</span><span class="sxs-lookup"><span data-stu-id="ba103-221">This allows us to return an Album as our model object to the view (which is strongly typed to the Album class) while passing additional data (e.g. a list of Genres) via the ViewBag.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

<span data-ttu-id="ba103-222">Изменить HTTP POST действие очень похоже на действие создания HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="ba103-222">The Edit HTTP-POST action is very similar to the Create HTTP-POST action.</span></span> <span data-ttu-id="ba103-223">Единственным различием является вместо добавления нового альбома к базе данных. Коллекции альбомы мы поиск текущего экземпляра альбом с помощью db. Entry(Album) и перевода ее в состояние Modified.</span><span class="sxs-lookup"><span data-stu-id="ba103-223">The only difference is that instead of adding a new album to the db.Albums collection, we're finding the current instance of the Album using db.Entry(album) and setting its state to Modified.</span></span> <span data-ttu-id="ba103-224">Это заставляет Entity Framework, мы изменяем существующий альбом вместо создания новой.</span><span class="sxs-lookup"><span data-stu-id="ba103-224">This tells Entity Framework that we are modifying an existing album as opposed to creating a new one.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

<span data-ttu-id="ba103-225">Мы смогли проверить эту возможность, работающим с приложением и просмотр/StoreManger /, а затем щелкнуть ссылку правки для альбома.</span><span class="sxs-lookup"><span data-stu-id="ba103-225">We can test this out by running the application and browsing to /StoreManger/, then clicking the Edit link for an album.</span></span>

![](mvc-music-store-part-5/_static/image9.png)

<span data-ttu-id="ba103-226">Это показанное редактирования с помощью метода изменить HTTP-GET.</span><span class="sxs-lookup"><span data-stu-id="ba103-226">This displays the Edit form shown by the Edit HTTP-GET method.</span></span> <span data-ttu-id="ba103-227">Заполните некоторые значения и нажмите кнопку "Сохранить".</span><span class="sxs-lookup"><span data-stu-id="ba103-227">Fill in some values and click the Save button.</span></span>

![](mvc-music-store-part-5/_static/image10.png)

<span data-ttu-id="ba103-228">Это отправляет форму, сохраняет значения и возвращает нас к списку альбома, показывающая, что значения были обновлены.</span><span class="sxs-lookup"><span data-stu-id="ba103-228">This posts the form, saves the values, and returns us to the Album list, showing that the values were updated.</span></span>

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a><span data-ttu-id="ba103-229">Обработка удаления</span><span class="sxs-lookup"><span data-stu-id="ba103-229">Handling Deletion</span></span>

<span data-ttu-id="ba103-230">Удаление следует той же схеме, как изменить и создать, с использованием одного контроллера действия для отображения формы подтверждения и другое действие контроллера для обработки отправки формы.</span><span class="sxs-lookup"><span data-stu-id="ba103-230">Deletion follows the same pattern as Edit and Create, using one controller action to display the confirmation form, and another controller action to handle the form submission.</span></span>

<span data-ttu-id="ba103-231">Действия HTTP GET и Delete контроллера точно совпадает нашей предыдущее действие контроллера сведения диспетчера хранилища.</span><span class="sxs-lookup"><span data-stu-id="ba103-231">The HTTP-GET Delete controller action is exactly the same as our previous Store Manager Details controller action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

<span data-ttu-id="ba103-232">Мы можем отобразить форму, которая является строго типизированным типу альбома, с помощью удаления представления содержимого шаблона.</span><span class="sxs-lookup"><span data-stu-id="ba103-232">We display a form that's strongly typed to an Album type, using the Delete view content template.</span></span>

![](mvc-music-store-part-5/_static/image12.png)

<span data-ttu-id="ba103-233">Удаление шаблона отображаются все поля для модели, но немного можно упростить, вниз.</span><span class="sxs-lookup"><span data-stu-id="ba103-233">The Delete template shows all the fields for the model, but we can simplify that down quite a bit.</span></span> <span data-ttu-id="ba103-234">Измените Просмотр кода в /Views/StoreManager/Delete.cshtml следующим образом.</span><span class="sxs-lookup"><span data-stu-id="ba103-234">Change the view code in /Views/StoreManager/Delete.cshtml to the following.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

<span data-ttu-id="ba103-235">При этом отображаются упрощенный подтверждение удаления.</span><span class="sxs-lookup"><span data-stu-id="ba103-235">This displays a simplified Delete confirmation.</span></span>

![](mvc-music-store-part-5/_static/image13.png)

<span data-ttu-id="ba103-236">Нажав кнопку «Удалить» приводит к обратной передаче на сервер, который выполняет действие DeleteConfirmed формы.</span><span class="sxs-lookup"><span data-stu-id="ba103-236">Clicking the Delete button causes the form to be posted back to the server, which executes the DeleteConfirmed action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

<span data-ttu-id="ba103-237">Наши действия контроллера HTTP POST удалить выполняет следующие действия:</span><span class="sxs-lookup"><span data-stu-id="ba103-237">Our HTTP-POST Delete Controller Action takes the following actions:</span></span>

- 1. <span data-ttu-id="ba103-238">Загружает альбом по Идентификатору</span><span class="sxs-lookup"><span data-stu-id="ba103-238">Loads the Album by ID</span></span>
- 2. <span data-ttu-id="ba103-239">Удаляет его альбом и сохранить изменения</span><span class="sxs-lookup"><span data-stu-id="ba103-239">Deletes it the album and save changes</span></span>
- 3. <span data-ttu-id="ba103-240">Перенаправление в индекс, показывающий, что альбом был удален из списка</span><span class="sxs-lookup"><span data-stu-id="ba103-240">Redirects to the Index, showing that the Album was removed from the list</span></span>

<span data-ttu-id="ba103-241">Чтобы проверить это, запустите приложение и перейдите к /StoreManager.</span><span class="sxs-lookup"><span data-stu-id="ba103-241">To test this, run the application and browse to /StoreManager.</span></span> <span data-ttu-id="ba103-242">Выберите его из списка и щелкните ссылку «Удалить».</span><span class="sxs-lookup"><span data-stu-id="ba103-242">Select an album from the list and click the Delete link.</span></span>

![](mvc-music-store-part-5/_static/image14.png)

<span data-ttu-id="ba103-243">При этом отображаются наш экран подтверждения удаления.</span><span class="sxs-lookup"><span data-stu-id="ba103-243">This displays our Delete confirmation screen.</span></span>

![](mvc-music-store-part-5/_static/image15.png)

<span data-ttu-id="ba103-244">Нажав кнопку "Удалить" Удаляет альбом и возвращает нам страницу индекса диспетчера хранилища, которая показывает, что альбом была удалена.</span><span class="sxs-lookup"><span data-stu-id="ba103-244">Clicking the Delete button removes the album and returns us to the Store Manager Index page, which shows that the album has been deleted.</span></span>

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a><span data-ttu-id="ba103-245">С помощью пользовательских вспомогательный метод HTML для усечения текста</span><span class="sxs-lookup"><span data-stu-id="ba103-245">Using a custom HTML Helper to truncate text</span></span>

<span data-ttu-id="ba103-246">У нас одна возможная проблема с нашу страницу индекса диспетчера хранилища.</span><span class="sxs-lookup"><span data-stu-id="ba103-246">We've got one potential issue with our Store Manager Index page.</span></span> <span data-ttu-id="ba103-247">Название альбома и имя исполнителя свойства могут быть достаточно большим, они могут сбросить нашей форматирование таблицы.</span><span class="sxs-lookup"><span data-stu-id="ba103-247">Our Album Title and Artist Name properties can both be long enough that they could throw off our table formatting.</span></span> <span data-ttu-id="ba103-248">Мы создадим пользовательский вспомогательный метод HTML, которые позволяют легко truncate этих и других свойств в нашем представления.</span><span class="sxs-lookup"><span data-stu-id="ba103-248">We'll create a custom HTML Helper to allow us to easily truncate these and other properties in our Views.</span></span>

![](mvc-music-store-part-5/_static/image17.png)

<span data-ttu-id="ba103-249">В Razor @helper синтаксис представляет собой довольно легко создавать собственные вспомогательные функции для использования в представления.</span><span class="sxs-lookup"><span data-stu-id="ba103-249">Razor's @helper syntax has made it pretty easy to create your own helper functions for use in your views.</span></span> <span data-ttu-id="ba103-250">Откройте представление /Views/StoreManager/Index.cshtml и добавьте следующий код сразу после @model строки.</span><span class="sxs-lookup"><span data-stu-id="ba103-250">Open the /Views/StoreManager/Index.cshtml view and add the following code directly after the @model line.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

<span data-ttu-id="ba103-251">Этот вспомогательный метод принимает строку и максимальную длину, чтобы разрешить.</span><span class="sxs-lookup"><span data-stu-id="ba103-251">This helper method takes a string and a maximum length to allow.</span></span> <span data-ttu-id="ba103-252">Если текст, который предоставляется короче указанной длины, вспомогательное приложение выводит его как-является.</span><span class="sxs-lookup"><span data-stu-id="ba103-252">If the text supplied is shorter than the length specified, the helper outputs it as-is.</span></span> <span data-ttu-id="ba103-253">Если длина его усекает текст и отображает «...», для остальных.</span><span class="sxs-lookup"><span data-stu-id="ba103-253">If it is longer, then it truncates the text and renders "…" for the remainder.</span></span>

<span data-ttu-id="ba103-254">Теперь можно использовать наши вспомогательные усечения для убедитесь, что свойства, имя исполнителя и название альбома, не более 25 символов.</span><span class="sxs-lookup"><span data-stu-id="ba103-254">Now we can use our Truncate helper to ensure that both the Album Title and Artist Name properties are less than 25 characters.</span></span> <span data-ttu-id="ba103-255">Полное представление кода, используя наш новый вспомогательный метод усечения появляется ниже.</span><span class="sxs-lookup"><span data-stu-id="ba103-255">The complete view code using our new Truncate helper appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

<span data-ttu-id="ba103-256">Теперь когда мы Обзор /StoreManager/ URL-адрес, альбомы и заголовки хранятся ниже нашей максимальной длины.</span><span class="sxs-lookup"><span data-stu-id="ba103-256">Now when we browse the /StoreManager/ URL, the albums and titles are kept below our maximum lengths.</span></span>

![](mvc-music-store-part-5/_static/image18.png)

<span data-ttu-id="ba103-257">Примечание: Здесь показан простой пример создания и использования вспомогательного класса в одном представлении.</span><span class="sxs-lookup"><span data-stu-id="ba103-257">Note: This shows the simple case of creating and using a helper in one view.</span></span> <span data-ttu-id="ba103-258">Дополнительные сведения о создании вспомогательные методы, которые можно использовать в веб-узла см. Мои записи блога: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span><span class="sxs-lookup"><span data-stu-id="ba103-258">To learn more about creating helpers that you can use throughout your site, see my blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="ba103-259">[Назад](mvc-music-store-part-4.md)
> [Вперед](mvc-music-store-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="ba103-259">[Previous](mvc-music-store-part-4.md)
[Next](mvc-music-store-part-6.md)</span></span>
