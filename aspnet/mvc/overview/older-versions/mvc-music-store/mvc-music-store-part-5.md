---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Часть 5: Изменение форм и шаблонов | Документация Майкрософт'
author: jongalloway
description: В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store. Часть 5 описывается изменение форм и шаблонов.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: c1065dcb45b6d28672edba32b95c7fc476c8b944
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838290"
---
<a name="part-5-edit-forms-and-templating"></a><span data-ttu-id="f6c36-104">Часть 5: Изменение форм и шаблонов.</span><span class="sxs-lookup"><span data-stu-id="f6c36-104">Part 5: Edit Forms and Templating</span></span>
====================
<span data-ttu-id="f6c36-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="f6c36-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="f6c36-106">Store музыки MVC является учебного приложения, которое вводятся и описываются пошаговые использование ASP.NET MVC и Visual Studio для разработки веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="f6c36-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="f6c36-107">Store музыки MVC — это реализация хранилища упрощенный пример, который продает музыкальные альбомы через Интернет и реализует базовый сайт администрирования, вход пользователя и корзины для покупок функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="f6c36-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="f6c36-108">В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="f6c36-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="f6c36-109">Часть 5 описывается изменение форм и шаблонов.</span><span class="sxs-lookup"><span data-stu-id="f6c36-109">Part 5 covers Edit Forms and Templating.</span></span>


<span data-ttu-id="f6c36-110">В последние главе мы были загрузка данных из базы данных и его отображения.</span><span class="sxs-lookup"><span data-stu-id="f6c36-110">In the past chapter, we were loading data from our database and displaying it.</span></span> <span data-ttu-id="f6c36-111">В этой главе мы также дает редактирования данных.</span><span class="sxs-lookup"><span data-stu-id="f6c36-111">In this chapter, we'll also enable editing the data.</span></span>

## <a name="creating-the-storemanagercontroller"></a><span data-ttu-id="f6c36-112">Создание StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="f6c36-112">Creating the StoreManagerController</span></span>

<span data-ttu-id="f6c36-113">Начнем с создания нового контроллера с именем **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="f6c36-113">We'll begin by creating a new controller called **StoreManagerController**.</span></span> <span data-ttu-id="f6c36-114">Для этого контроллера мы будет используют преимущества функций формирования шаблонов, доступных в обновление средств ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="f6c36-114">For this controller, we will be taking advantage of the Scaffolding features available in the ASP.NET MVC 3 Tools Update.</span></span> <span data-ttu-id="f6c36-115">Задание параметров в диалоговом окне Добавить контроллер, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="f6c36-115">Set the options for the Add Controller dialog as shown below.</span></span>

![](mvc-music-store-part-5/_static/image1.png)

<span data-ttu-id="f6c36-116">При нажатии кнопки «Добавить», вы увидите, что механизм формирования шаблонов ASP.NET MVC 3 не достаточно большой объем работы для вас:</span><span class="sxs-lookup"><span data-stu-id="f6c36-116">When you click the Add button, you'll see that the ASP.NET MVC 3 scaffolding mechanism does a good amount of work for you:</span></span>

- <span data-ttu-id="f6c36-117">Он создает новый StoreManagerController с локальной переменной Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f6c36-117">It creates the new StoreManagerController with a local Entity Framework variable</span></span>
- <span data-ttu-id="f6c36-118">Он добавляет папку StoreManager в папке представления проекта</span><span class="sxs-lookup"><span data-stu-id="f6c36-118">It adds a StoreManager folder to the project's Views folder</span></span>
- <span data-ttu-id="f6c36-119">Он добавляет представление Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml и Index.cshtml, строго типизированный к классу альбома</span><span class="sxs-lookup"><span data-stu-id="f6c36-119">It adds Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml, and Index.cshtml view, strongly typed to the Album class</span></span>

![](mvc-music-store-part-5/_static/image2.png)

<span data-ttu-id="f6c36-120">Новый класс контроллера StoreManager включает CRUD (Создание, чтение, обновление и удаление) действия контроллера, которые умеют работать с альбом класс модели и использовать наш контекст Entity Framework для доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="f6c36-120">The new StoreManager controller class includes CRUD (create, read, update, delete) controller actions which know how to work with the Album model class and use our Entity Framework context for database access.</span></span>

## <a name="modifying-a-scaffolded-view"></a><span data-ttu-id="f6c36-121">Изменение сформированные представления</span><span class="sxs-lookup"><span data-stu-id="f6c36-121">Modifying a Scaffolded View</span></span>

<span data-ttu-id="f6c36-122">Это важно помнить, что, хотя этот код был создан для нас, это стандартный код ASP.NET MVC, так же, как как мы писали в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="f6c36-122">It's important to remember that, while this code was generated for us, it's standard ASP.NET MVC code, just like we've been writing throughout this tutorial.</span></span> <span data-ttu-id="f6c36-123">Она предназначена для сохранения бы потратить время на написание стандартный код контроллера и создание строго типизированные представления вручную, но это не тип созданного кода, вы могли заметить, предваряемые фразой дыре предупреждений в комментарии о том, как вы не должны изменять код.</span><span class="sxs-lookup"><span data-stu-id="f6c36-123">It's intended to save you the time you'd spend on writing boilerplate controller code and creating the strongly typed views manually, but this isn't the kind of generated code you may have seen prefaced with dire warnings in comments about how you mustn't change the code.</span></span> <span data-ttu-id="f6c36-124">Это код, и вы должны изменить его.</span><span class="sxs-lookup"><span data-stu-id="f6c36-124">This is your code, and you're expected to change it.</span></span>

<span data-ttu-id="f6c36-125">Итак давайте начнем с Быстрое редактирование в представление StoreManager Index (/ Views/StoreManager/Index.cshtml).</span><span class="sxs-lookup"><span data-stu-id="f6c36-125">So, let's start with a quick edit to the StoreManager Index view (/Views/StoreManager/Index.cshtml).</span></span> <span data-ttu-id="f6c36-126">Это представление отображает таблицу, которая перечисляет альбомов в нашем магазине с изменением / сведения о / удаление ссылок и включает в себя свойства открытого альбома.</span><span class="sxs-lookup"><span data-stu-id="f6c36-126">This view will display a table which lists the Albums in our store with Edit / Details / Delete links, and includes the Album's public properties.</span></span> <span data-ttu-id="f6c36-127">Мы удалим поле AlbumArtUrl, так как он не очень полезны в этом окне.</span><span class="sxs-lookup"><span data-stu-id="f6c36-127">We'll remove the AlbumArtUrl field, as it's not very useful in this display.</span></span> <span data-ttu-id="f6c36-128">В &lt;таблицы&gt; раздел кода представление, удалите &lt;th&gt; и &lt;td&gt; элементы вокруг AlbumArtUrl ссылки, как указано ниже выделенные строки:</span><span class="sxs-lookup"><span data-stu-id="f6c36-128">In &lt;table&gt; section of the view code, remove the &lt;th&gt; and &lt;td&gt; elements surrounding AlbumArtUrl references, as indicated by the highlighted lines below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

<span data-ttu-id="f6c36-129">Просмотреть измененный код будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f6c36-129">The modified view code will appear as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a><span data-ttu-id="f6c36-130">Первый взгляд на диспетчер Store</span><span class="sxs-lookup"><span data-stu-id="f6c36-130">A first look at the Store Manager</span></span>

<span data-ttu-id="f6c36-131">Теперь запустите приложение и перейдите к/StoreManager /.</span><span class="sxs-lookup"><span data-stu-id="f6c36-131">Now run the application and browse to /StoreManager/.</span></span> <span data-ttu-id="f6c36-132">Здесь показаны измененные, со списком альбомов в хранилище со ссылками на редактирования и сведений и удалить индекс Store Manager.</span><span class="sxs-lookup"><span data-stu-id="f6c36-132">This displays the Store Manager Index we just modified, showing a list of the albums in the store with links to Edit, Details, and Delete.</span></span>

![](mvc-music-store-part-5/_static/image3.png)

<span data-ttu-id="f6c36-133">Щелкнув ссылку "Изменить" отображается форма редактирования с полями альбома, включая раскрывающиеся списки для жанр и исполнителя.</span><span class="sxs-lookup"><span data-stu-id="f6c36-133">Clicking the Edit link displays an edit form with fields for the Album, including dropdowns for Genre and Artist.</span></span>

![](mvc-music-store-part-5/_static/image4.png)

<span data-ttu-id="f6c36-134">Щелкните ссылку «Обратно в список» внизу, а затем щелкните ссылку сведения для альбома.</span><span class="sxs-lookup"><span data-stu-id="f6c36-134">Click the "Back to List" link at the bottom, then click on the Details link for an Album.</span></span> <span data-ttu-id="f6c36-135">Отобразится подробная информация об отдельных альбома.</span><span class="sxs-lookup"><span data-stu-id="f6c36-135">This displays the detail information for an individual Album.</span></span>

![](mvc-music-store-part-5/_static/image5.png)

<span data-ttu-id="f6c36-136">Опять же щелкните ссылки возврата в списке, а затем щелкните ссылку, удалить.</span><span class="sxs-lookup"><span data-stu-id="f6c36-136">Again, click the Back to List link, then click on a Delete link.</span></span> <span data-ttu-id="f6c36-137">Откроется диалоговое окно подтверждения, показывая сведения об альбоме и запросом, если мы уверены, что нам нужно удалить его.</span><span class="sxs-lookup"><span data-stu-id="f6c36-137">This displays a confirmation dialog, showing the album details and asking if we're sure we want to delete it.</span></span>

![](mvc-music-store-part-5/_static/image6.png)

<span data-ttu-id="f6c36-138">Нажав кнопку «Удалить» в нижней ведет к удалению альбома и вернулись к страницу индекса, которая показывает альбом, удалены.</span><span class="sxs-lookup"><span data-stu-id="f6c36-138">Clicking the Delete button at the bottom will delete the album and return you to the Index page, which shows the album deleted.</span></span>

<span data-ttu-id="f6c36-139">Мы не завершили работу с Store Manager, но у нас есть контроллер и представление кода для операций CRUD для запуска из работы.</span><span class="sxs-lookup"><span data-stu-id="f6c36-139">We're not done with the Store Manager, but we have working controller and view code for the CRUD operations to start from.</span></span>

## <a name="looking-at-the-store-manager-controller-code"></a><span data-ttu-id="f6c36-140">Просмотрев код Store Manager контроллера</span><span class="sxs-lookup"><span data-stu-id="f6c36-140">Looking at the Store Manager Controller code</span></span>

<span data-ttu-id="f6c36-141">Контроллер Store Manager содержит достаточно большой объем кода.</span><span class="sxs-lookup"><span data-stu-id="f6c36-141">The Store Manager Controller contains a good amount of code.</span></span> <span data-ttu-id="f6c36-142">Давайте разберем это сверху вниз.</span><span class="sxs-lookup"><span data-stu-id="f6c36-142">Let's go through this from top to bottom.</span></span> <span data-ttu-id="f6c36-143">Контроллер включает некоторые стандартные пространства имен для контроллера MVC, а также ссылку на пространство имен нашей модели.</span><span class="sxs-lookup"><span data-stu-id="f6c36-143">The controller includes some standard namespaces for an MVC controller, as well as a reference to our Models namespace.</span></span> <span data-ttu-id="f6c36-144">Контроллер может единичный экземпляр MusicStoreEntities, используемых каждым из действий контроллера, для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="f6c36-144">The controller has a private instance of MusicStoreEntities, used by each of the controller actions for data access.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a><span data-ttu-id="f6c36-145">Store Manager Index и Details действия</span><span class="sxs-lookup"><span data-stu-id="f6c36-145">Store Manager Index and Details actions</span></span>

<span data-ttu-id="f6c36-146">Представление index извлекает список альбомов, включая каждого альбома упоминаемого жанр и исполнителях сведения, как мы видели ранее при работе в методе Store Обзор.</span><span class="sxs-lookup"><span data-stu-id="f6c36-146">The index view retrieves a list of Albums, including each album's referenced Genre and Artist information, as we previously saw when working on the Store Browse method.</span></span> <span data-ttu-id="f6c36-147">Представление Index подписан ссылки на связанные объекты можно отображать его название жанра каждого альбома, а также имя исполнителя, чтобы контроллер, эффективные и выполнения запросов для получения этих сведений в исходном запросе.</span><span class="sxs-lookup"><span data-stu-id="f6c36-147">The Index view is following the references to the linked objects so that it can display each album's Genre name and Artist name, so the controller is being efficient and querying for this information in the original request.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

<span data-ttu-id="f6c36-148">Действие контроллера сведения контроллеру StoreManager работает так же, как на действие Details контроллера Store, мы писали раньше - он запрашивает альбома по Идентификатору, используя метод Find(), затем возвращает его представление.</span><span class="sxs-lookup"><span data-stu-id="f6c36-148">The StoreManager Controller's Details controller action works exactly the same as the Store Controller Details action we wrote previously - it queries for the Album by ID using the Find() method, then returns it to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a><span data-ttu-id="f6c36-149">Создавать методы действий</span><span class="sxs-lookup"><span data-stu-id="f6c36-149">The Create Action Methods</span></span>

<span data-ttu-id="f6c36-150">Методы действий Create — это немного отличаются от тех, которые мы уже видели, поскольку они обрабатывают входные данные формы.</span><span class="sxs-lookup"><span data-stu-id="f6c36-150">The Create action methods are a little different from ones we've seen so far, because they handle form input.</span></span> <span data-ttu-id="f6c36-151">При первом посещении /StoreManager/создать/будут показаны как пустая форма.</span><span class="sxs-lookup"><span data-stu-id="f6c36-151">When a user first visits /StoreManager/Create/ they will be shown an empty form.</span></span> <span data-ttu-id="f6c36-152">Эта страница HTML будет содержать &lt;формы&gt; элемент, который содержит раскрывающийся список и текстового поля входных элементов, где они могут ввести сведения об альбоме.</span><span class="sxs-lookup"><span data-stu-id="f6c36-152">This HTML page will contain a &lt;form&gt; element that contains dropdown and textbox input elements where they can enter the album's details.</span></span>

<span data-ttu-id="f6c36-153">После пользователь заполняет значения формы альбома, их можно нажать кнопку «Сохранить», чтобы отправить эти изменения обратно в наше приложение, чтобы сохранить в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f6c36-153">After the user fills in the Album form values, they can press the "Save" button to submit these changes back to our application to save within the database.</span></span> <span data-ttu-id="f6c36-154">Когда пользователь нажимает кнопку «Сохранить» &lt;формы&gt; выполнит запрос HTTP POST к /StoreManager/создать/URL-адрес и отправьте &lt;формы&gt; значения как часть HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="f6c36-154">When the user presses the "save" button the &lt;form&gt; will perform an HTTP-POST back to the /StoreManager/Create/ URL and submit the &lt;form&gt; values as part of the HTTP-POST.</span></span>

<span data-ttu-id="f6c36-155">ASP.NET MVC позволяет нам легко разделить логику из этих двух сценариев вызова URL-адрес, благодаря чему мы можем реализовать два отдельных метода действия «Создать» в наш класс StoreManagerController — один для обработки первоначальной обзора HTTP-GET для /StoreManager/Create / URL-адрес, а другой для обработки HTTP-POST отправляемых изменений.</span><span class="sxs-lookup"><span data-stu-id="f6c36-155">ASP.NET MVC allows us to easily split up the logic of these two URL invocation scenarios by enabling us to implement two separate "Create" action methods within our StoreManagerController class – one to handle the initial HTTP-GET browse to the /StoreManager/Create/ URL, and the other to handle the HTTP-POST of the submitted changes.</span></span>

### <a name="passing-information-to-a-view-using-viewbag"></a><span data-ttu-id="f6c36-156">Передача сведений об в представление с помощью ViewBag</span><span class="sxs-lookup"><span data-stu-id="f6c36-156">Passing information to a View using ViewBag</span></span>

<span data-ttu-id="f6c36-157">Мы использовали ViewBag ранее в этом учебнике, но еще не было сказано немного о нем.</span><span class="sxs-lookup"><span data-stu-id="f6c36-157">We've used the ViewBag earlier in this tutorial, but haven't talked much about it.</span></span> <span data-ttu-id="f6c36-158">ViewBag позволяет нам для передачи информации в представление без использования объекта строго типизированные модели.</span><span class="sxs-lookup"><span data-stu-id="f6c36-158">The ViewBag allows us to pass information to the view without using a strongly typed model object.</span></span> <span data-ttu-id="f6c36-159">В этом случае наши действия контроллера изменить HTTP-GET должен пройти обоих список жанров и исполнители в форму для заполнения раскрывающихся списков и для этого проще всего вернуть их в виде элементов ViewBag.</span><span class="sxs-lookup"><span data-stu-id="f6c36-159">In this case, our Edit HTTP-GET controller action needs to pass both a list of Genres and Artists to the form to populate the dropdowns, and the simplest way to do that is to return them as ViewBag items.</span></span>

<span data-ttu-id="f6c36-160">ViewBag является динамическим объектом, это означает, что вы можете ввести ViewBag.Foo или ViewBag.YourNameHere без написания кода для определения этих свойств.</span><span class="sxs-lookup"><span data-stu-id="f6c36-160">The ViewBag is a dynamic object, meaning that you can type ViewBag.Foo or ViewBag.YourNameHere without writing code to define those properties.</span></span> <span data-ttu-id="f6c36-161">В этом случае код контроллера использует ViewBag.GenreId и ViewBag.ArtistId, таким образом будет раскрывающийся список значений, отправленных с формой, GenreId и ArtistId, которые являются альбома свойства, которые будут настраиваться.</span><span class="sxs-lookup"><span data-stu-id="f6c36-161">In this case, the controller code uses ViewBag.GenreId and ViewBag.ArtistId so that the dropdown values submitted with the form will be GenreId and ArtistId, which are the Album properties they will be setting.</span></span>

<span data-ttu-id="f6c36-162">Эти значения раскрывающегося списка возвращаются в форму, используя объект SelectList, которая построена специально для этой цели.</span><span class="sxs-lookup"><span data-stu-id="f6c36-162">These dropdown values are returned to the form using the SelectList object, which is built just for that purpose.</span></span> <span data-ttu-id="f6c36-163">Это делается с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="f6c36-163">This is done using code like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

<span data-ttu-id="f6c36-164">Как видно из кода метода действия, для создания данного объекта используются три параметра:</span><span class="sxs-lookup"><span data-stu-id="f6c36-164">As you can see from the action method code, three parameters are being used to create this object:</span></span>

- <span data-ttu-id="f6c36-165">Список элементов, которые будут применяться для отображения раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="f6c36-165">The list of items the dropdown will be displaying.</span></span> <span data-ttu-id="f6c36-166">Обратите внимание, что это не просто строка — мы передаем список жанров.</span><span class="sxs-lookup"><span data-stu-id="f6c36-166">Note that this isn't just a string - we're passing a list of Genres.</span></span>
- <span data-ttu-id="f6c36-167">Следующий параметр, передаваемые SelectList является выбранному значению.</span><span class="sxs-lookup"><span data-stu-id="f6c36-167">The next parameter being passed to the SelectList is the Selected Value.</span></span> <span data-ttu-id="f6c36-168">Этот способ SelectList знает, как предварительно выбранный элемент в списке.</span><span class="sxs-lookup"><span data-stu-id="f6c36-168">This how the SelectList knows how to pre-select an item in the list.</span></span> <span data-ttu-id="f6c36-169">Это будет легче понять, если взглянуть на форму редактирования, который очень похож.</span><span class="sxs-lookup"><span data-stu-id="f6c36-169">This will be easier to understand when we look at the Edit form, which is pretty similar.</span></span>
- <span data-ttu-id="f6c36-170">Последний параметр является свойство для отображения.</span><span class="sxs-lookup"><span data-stu-id="f6c36-170">The final parameter is the property to be displayed.</span></span> <span data-ttu-id="f6c36-171">В этом случае это указывает, Genre.Name свойством, какие данные будут отображаться для пользователя.</span><span class="sxs-lookup"><span data-stu-id="f6c36-171">In this case, this is indicating that the Genre.Name property is what will be shown to the user.</span></span>

<span data-ttu-id="f6c36-172">С учетом этого, действие создания HTTP-GET довольно прост — ViewBag добавляются два SelectLists затем объект модели не передается в форму (так как он еще не создан еще).</span><span class="sxs-lookup"><span data-stu-id="f6c36-172">With that in mind, then, the HTTP-GET Create action is pretty simple - two SelectLists are added to the ViewBag, and no model object is passed to the form (since it hasn't been created yet).</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a><span data-ttu-id="f6c36-173">Вспомогательные методы HTML для отображения в списке Drop в Create View</span><span class="sxs-lookup"><span data-stu-id="f6c36-173">HTML Helpers to display the Drop Downs in the Create View</span></span>

<span data-ttu-id="f6c36-174">Так как мы говорили о как раскрывающийся список значения передаются в представление, давайте кратко рассмотрим представление, чтобы увидеть, как отображаются значения.</span><span class="sxs-lookup"><span data-stu-id="f6c36-174">Since we've talked about how the drop down values are passed to the view, let's take a quick look at the view to see how those values are displayed.</span></span> <span data-ttu-id="f6c36-175">В представлении кода (/ Views/StoreManager/Create.cshtml), вы увидите следующий вызов для отображения раскрывающегося жанр вниз.</span><span class="sxs-lookup"><span data-stu-id="f6c36-175">In the view code (/Views/StoreManager/Create.cshtml), you'll see the following call is made to display the Genre drop down.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

<span data-ttu-id="f6c36-176">Это называется вспомогательный метод HTML - служебный метод, который выполняет типичную задачу представления.</span><span class="sxs-lookup"><span data-stu-id="f6c36-176">This is known as an HTML Helper - a utility method which performs a common view task.</span></span> <span data-ttu-id="f6c36-177">Вспомогательные методы HTML очень полезны в обеспечении наш код представления четкими и удобочитаемыми.</span><span class="sxs-lookup"><span data-stu-id="f6c36-177">HTML Helpers are very useful in keeping our view code concise and readable.</span></span> <span data-ttu-id="f6c36-178">Вспомогательный метод Html.DropDownList обеспечивается ASP.NET MVC, но как мы увидим позже его можно создать собственные вспомогательные функции для просмотра кода, который мы будем использовать в наше приложение.</span><span class="sxs-lookup"><span data-stu-id="f6c36-178">The Html.DropDownList helper is provided by ASP.NET MVC, but as we'll see later it's possible to create our own helpers for view code we'll reuse in our application.</span></span>

<span data-ttu-id="f6c36-179">Вызов Html.DropDownList только должен знать две вещи - get отображались в списке, и какое значение (если таковые имеются) должен быть уже выбран.</span><span class="sxs-lookup"><span data-stu-id="f6c36-179">The Html.DropDownList call just needs to be told two things - where to get the list to display, and what value (if any) should be pre-selected.</span></span> <span data-ttu-id="f6c36-180">Первый параметр, GenreId, определяет DropDownList искомое значение с именем GenreId в модели или ViewBag.</span><span class="sxs-lookup"><span data-stu-id="f6c36-180">The first parameter, GenreId, tells the DropDownList to look for a value named GenreId in either the model or ViewBag.</span></span> <span data-ttu-id="f6c36-181">Второй параметр используется для указания значения, чтобы показать, как изначально выбираются в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="f6c36-181">The second parameter is used to indicate the value to show as initially selected in the drop down list.</span></span> <span data-ttu-id="f6c36-182">Так как эта форма является создание формы, нет значения, чтобы быть предустановленные и передается String.Empty.</span><span class="sxs-lookup"><span data-stu-id="f6c36-182">Since this form is a Create form, there's no value to be preselected and String.Empty is passed.</span></span>

### <a name="handling-the-posted-form-values"></a><span data-ttu-id="f6c36-183">Обработка значений учтена формы</span><span class="sxs-lookup"><span data-stu-id="f6c36-183">Handling the Posted Form values</span></span>

<span data-ttu-id="f6c36-184">Как уже говорилось, прежде чем, существует два метода действия, связанные с каждой формы.</span><span class="sxs-lookup"><span data-stu-id="f6c36-184">As we discussed before, there are two action methods associated with each form.</span></span> <span data-ttu-id="f6c36-185">Первая обрабатывает запрос HTTP GET и отображает форму.</span><span class="sxs-lookup"><span data-stu-id="f6c36-185">The first handles the HTTP-GET request and displays the form.</span></span> <span data-ttu-id="f6c36-186">Вторая обрабатывает запрос HTTP POST, который содержит значения отправленной формы.</span><span class="sxs-lookup"><span data-stu-id="f6c36-186">The second handles the HTTP-POST request, which contains the submitted form values.</span></span> <span data-ttu-id="f6c36-187">Обратите внимание на то, что действие контроллера имеет атрибут [HttpPost], который сообщает ASP.NET MVC, что он только должен отвечать на запросы HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="f6c36-187">Notice that controller action has an [HttpPost] attribute, which tells ASP.NET MVC that it should only respond to HTTP-POST requests.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

<span data-ttu-id="f6c36-188">Это действие имеет четыре обязанности:</span><span class="sxs-lookup"><span data-stu-id="f6c36-188">This action has four responsibilities:</span></span>

- 1. <span data-ttu-id="f6c36-189">Чтение значения формы</span><span class="sxs-lookup"><span data-stu-id="f6c36-189">Read the form values</span></span>
- 2. <span data-ttu-id="f6c36-190">Проверьте, если значения формы передать все правила проверки</span><span class="sxs-lookup"><span data-stu-id="f6c36-190">Check if the form values pass any validation rules</span></span>
- 3. <span data-ttu-id="f6c36-191">Если отправка формы является допустимым, для сохранения данных, так и для отображения обновленного списка</span><span class="sxs-lookup"><span data-stu-id="f6c36-191">If the form submission is valid, save the data and display the updated list</span></span>
- 4. <span data-ttu-id="f6c36-192">Если отправка формы не является допустимым, повторного вывода формы с ошибками проверки</span><span class="sxs-lookup"><span data-stu-id="f6c36-192">If the form submission is not valid, redisplay the form with validation errors</span></span>

#### <a name="reading-form-values-with-model-binding"></a><span data-ttu-id="f6c36-193">Считывание значений формы с привязкой модели</span><span class="sxs-lookup"><span data-stu-id="f6c36-193">Reading Form Values with Model Binding</span></span>

<span data-ttu-id="f6c36-194">Действие контроллера обрабатывает отправки формы, включает в себя значения GenreId и ArtistId (из раскрывающегося списка) и текстовое поле значения для заголовка, цену и AlbumArtUrl.</span><span class="sxs-lookup"><span data-stu-id="f6c36-194">The controller action is processing a form submission that includes values for GenreId and ArtistId (from the drop down list) and textbox values for Title, Price, and AlbumArtUrl.</span></span> <span data-ttu-id="f6c36-195">Хотя можно получить прямой доступ к значениям формы, лучшим подходом является использование возможностей привязки модели ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f6c36-195">While it's possible to directly access form values, a better approach is to use the Model Binding capabilities built into ASP.NET MVC.</span></span> <span data-ttu-id="f6c36-196">Когда действие контроллера принимает тип модели в качестве параметра, ASP.NET MVC попытается заполнить объект этого типа с помощью формы ввода данных (а также значения маршрута и строки запроса).</span><span class="sxs-lookup"><span data-stu-id="f6c36-196">When a controller action takes a model type as a parameter, ASP.NET MVC will attempt to populate an object of that type using form inputs (as well as route and querystring values).</span></span> <span data-ttu-id="f6c36-197">Это делается путем поиска значений, имена которых соответствуют свойствам объекта модели, например при установке нового альбома объекта GenreId значения, он ищет входных данных с именем GenreId.</span><span class="sxs-lookup"><span data-stu-id="f6c36-197">It does this by looking for values whose names match properties of the model object, e.g. when setting the new Album object's GenreId value, it looks for an input with the name GenreId.</span></span> <span data-ttu-id="f6c36-198">При создании представлений с помощью стандартных методов в ASP.NET MVC, формы всегда отображается с помощью имен свойств как имена полей ввода, поэтому это поле будет просто соответствия имен.</span><span class="sxs-lookup"><span data-stu-id="f6c36-198">When you create views using the standard methods in ASP.NET MVC, the forms will always be rendered using property names as input field names, so this the field names will just match up.</span></span>

#### <a name="validating-the-model"></a><span data-ttu-id="f6c36-199">Проверка модели</span><span class="sxs-lookup"><span data-stu-id="f6c36-199">Validating the Model</span></span>

<span data-ttu-id="f6c36-200">Модель проверяется с помощью простого вызова ModelState.IsValid.</span><span class="sxs-lookup"><span data-stu-id="f6c36-200">The model is validated with a simple call to ModelState.IsValid.</span></span> <span data-ttu-id="f6c36-201">Мы еще не добавили все правила проверки к нашему классу альбома, но — мы сделаем, через несколько минут — Теперь эта проверка не нечего делать.</span><span class="sxs-lookup"><span data-stu-id="f6c36-201">We haven't added any validation rules to our Album class yet - we'll do that in a bit - so right now this check doesn't have much to do.</span></span> <span data-ttu-id="f6c36-202">Важно то, что эта проверка ModelStat.IsValid будет адаптироваться к правила проверки, которые мы поместили в нашей модели, поэтому будущие изменения в правила проверки не требует никаких обновлений для кода действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="f6c36-202">What's important is that this ModelStat.IsValid check will adapt to the validation rules we put on our model, so future changes to validation rules won't require any updates to the controller action code.</span></span>

#### <a name="saving-the-submitted-values"></a><span data-ttu-id="f6c36-203">Сохранение предоставленными значениями</span><span class="sxs-lookup"><span data-stu-id="f6c36-203">Saving the submitted values</span></span>

<span data-ttu-id="f6c36-204">Если отправка формы проходит проверку, пришло время для сохранения значений в базу данных.</span><span class="sxs-lookup"><span data-stu-id="f6c36-204">If the form submission passes validation, it's time to save the values to the database.</span></span> <span data-ttu-id="f6c36-205">С помощью Entity Framework, требуется выполнить добавление модели в коллекцию альбомы и вызова метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="f6c36-205">With Entity Framework, that just requires adding the model to the Albums collection and calling SaveChanges.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

<span data-ttu-id="f6c36-206">Entity Framework создает соответствующие команды SQL для сохранения значения.</span><span class="sxs-lookup"><span data-stu-id="f6c36-206">Entity Framework generates the appropriate SQL commands to persist the value.</span></span> <span data-ttu-id="f6c36-207">После сохранения данных, мы перенаправления обратно к списку альбомов, мы видим наши обновления.</span><span class="sxs-lookup"><span data-stu-id="f6c36-207">After saving the data, we redirect back to the list of Albums so we can see our update.</span></span> <span data-ttu-id="f6c36-208">Это делается путем возвращения RedirectToAction с именем действия контроллера, которое должно отображаться.</span><span class="sxs-lookup"><span data-stu-id="f6c36-208">This is done by returning RedirectToAction with the name of the controller action we want displayed.</span></span> <span data-ttu-id="f6c36-209">В этом случае это метод Index.</span><span class="sxs-lookup"><span data-stu-id="f6c36-209">In this case, that's the Index method.</span></span>

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a><span data-ttu-id="f6c36-210">Отображение отправок недопустимую форму с ошибками проверки</span><span class="sxs-lookup"><span data-stu-id="f6c36-210">Displaying invalid form submissions with Validation Errors</span></span>

<span data-ttu-id="f6c36-211">В случае недопустимой форме ввода раскрывающийся список значений добавляются ViewBag (как в случае HTTP-GET) и привязанная модель они передаются обратно в представление для отображения.</span><span class="sxs-lookup"><span data-stu-id="f6c36-211">In the case of invalid form input, the dropdown values are added to the ViewBag (as in the HTTP-GET case) and the bound model values are passed back to the view for display.</span></span> <span data-ttu-id="f6c36-212">Ошибки проверки автоматически отображаются с помощью @Html.ValidationMessageFor вспомогательный метод HTML.</span><span class="sxs-lookup"><span data-stu-id="f6c36-212">Validation errors are automatically displayed using the @Html.ValidationMessageFor HTML Helper.</span></span>

#### <a name="testing-the-create-form"></a><span data-ttu-id="f6c36-213">Тестирование Создание формы</span><span class="sxs-lookup"><span data-stu-id="f6c36-213">Testing the Create Form</span></span>

<span data-ttu-id="f6c36-214">Чтобы проверить это, выполнения приложения и перейдите к /StoreManager/создать / - вы увидите пустую форму, который был возвращен методом StoreController создать HTTP-GET.</span><span class="sxs-lookup"><span data-stu-id="f6c36-214">To test this out, run the application and browse to /StoreManager/Create/ - this will show you the blank form which was returned by the StoreController Create HTTP-GET method.</span></span>

<span data-ttu-id="f6c36-215">Заполните некоторые значения и нажмите кнопку «Создать» для отправки формы.</span><span class="sxs-lookup"><span data-stu-id="f6c36-215">Fill in some values and click the Create button to submit the form.</span></span>

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a><span data-ttu-id="f6c36-216">Обработка изменений</span><span class="sxs-lookup"><span data-stu-id="f6c36-216">Handling Edits</span></span>

<span data-ttu-id="f6c36-217">Пары «изменение действия» (HTTP-GET и HTTP-POST) очень похожи на методов создания действий, которые мы только что рассмотрели.</span><span class="sxs-lookup"><span data-stu-id="f6c36-217">The Edit action pair (HTTP-GET and HTTP-POST) are very similar to the Create action methods we just looked at.</span></span> <span data-ttu-id="f6c36-218">Поскольку сценарии редактирования включает работу с существующий альбом, изменить HTTP-GET метод загружает альбома, на основе параметра «id», передан по маршруту.</span><span class="sxs-lookup"><span data-stu-id="f6c36-218">Since the edit scenario involves working with an existing album, the Edit HTTP-GET method loads the Album based on the "id" parameter, passed in via the route.</span></span> <span data-ttu-id="f6c36-219">Этот код для извлечения и альбом AlbumId совпадает с ранее рассматривались в действие контроллера Details.</span><span class="sxs-lookup"><span data-stu-id="f6c36-219">This code for retrieving an album by AlbumId is the same as we've previously looked at in the Details controller action.</span></span> <span data-ttu-id="f6c36-220">Как и в случае с помощью команды Create / метод HTTP-GET, раскрывающийся список значения возвращаются через ViewBag.</span><span class="sxs-lookup"><span data-stu-id="f6c36-220">As with the Create / HTTP-GET method, the drop down values are returned via the ViewBag.</span></span> <span data-ttu-id="f6c36-221">Это позволяет нам для возврата альбом как нашего объекта модели представления (который является строго типизированным к классу альбом) во время передачи дополнительных данных (например список жанров) через ViewBag.</span><span class="sxs-lookup"><span data-stu-id="f6c36-221">This allows us to return an Album as our model object to the view (which is strongly typed to the Album class) while passing additional data (e.g. a list of Genres) via the ViewBag.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

<span data-ttu-id="f6c36-222">Изменить HTTP-POST действие очень похоже на действие создания HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="f6c36-222">The Edit HTTP-POST action is very similar to the Create HTTP-POST action.</span></span> <span data-ttu-id="f6c36-223">Единственная разница, вместо добавления нового альбома на базу данных. Коллекции альбомов, мы поиск текущего экземпляра альбома с помощью db. Entry(Album) и перевода его в состояние Modified.</span><span class="sxs-lookup"><span data-stu-id="f6c36-223">The only difference is that instead of adding a new album to the db.Albums collection, we're finding the current instance of the Album using db.Entry(album) and setting its state to Modified.</span></span> <span data-ttu-id="f6c36-224">Это сообщает Entity Framework, что мы изменяем существующий альбом в отличие от создания нового.</span><span class="sxs-lookup"><span data-stu-id="f6c36-224">This tells Entity Framework that we are modifying an existing album as opposed to creating a new one.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

<span data-ttu-id="f6c36-225">Эту возможность можно проверить путем запуска приложения и просмотр/StoreManger /, а затем щелкнув ссылку правки для альбома.</span><span class="sxs-lookup"><span data-stu-id="f6c36-225">We can test this out by running the application and browsing to /StoreManger/, then clicking the Edit link for an album.</span></span>

![](mvc-music-store-part-5/_static/image9.png)

<span data-ttu-id="f6c36-226">Откроется показано с помощью метода HTTP-GET, изменить форму редактирования.</span><span class="sxs-lookup"><span data-stu-id="f6c36-226">This displays the Edit form shown by the Edit HTTP-GET method.</span></span> <span data-ttu-id="f6c36-227">Заполните некоторые значения и нажмите кнопку "Сохранить".</span><span class="sxs-lookup"><span data-stu-id="f6c36-227">Fill in some values and click the Save button.</span></span>

![](mvc-music-store-part-5/_static/image10.png)

<span data-ttu-id="f6c36-228">Это отправляет форму, сохраняет значения и возвращает нас к списку альбома, показывающий, что значения были обновлены.</span><span class="sxs-lookup"><span data-stu-id="f6c36-228">This posts the form, saves the values, and returns us to the Album list, showing that the values were updated.</span></span>

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a><span data-ttu-id="f6c36-229">Обработка удаления</span><span class="sxs-lookup"><span data-stu-id="f6c36-229">Handling Deletion</span></span>

<span data-ttu-id="f6c36-230">Удаление та же схема, как Edit and Create, используя действие одного контроллера для отображения формы подтверждения и другое действие контроллера для обработки отправки формы.</span><span class="sxs-lookup"><span data-stu-id="f6c36-230">Deletion follows the same pattern as Edit and Create, using one controller action to display the confirmation form, and another controller action to handle the form submission.</span></span>

<span data-ttu-id="f6c36-231">Действие контроллера удалить HTTP-GET именно так же, как наши предыдущее действие контроллера сведения Store Manager.</span><span class="sxs-lookup"><span data-stu-id="f6c36-231">The HTTP-GET Delete controller action is exactly the same as our previous Store Manager Details controller action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

<span data-ttu-id="f6c36-232">Мы показываем форму, которая является строго типизированным типа альбома, с помощью шаблона содержимого представления Delete.</span><span class="sxs-lookup"><span data-stu-id="f6c36-232">We display a form that's strongly typed to an Album type, using the Delete view content template.</span></span>

![](mvc-music-store-part-5/_static/image12.png)

<span data-ttu-id="f6c36-233">Шаблон удаления будут видны все поля для модели, но довольно можно упростить, вниз.</span><span class="sxs-lookup"><span data-stu-id="f6c36-233">The Delete template shows all the fields for the model, but we can simplify that down quite a bit.</span></span> <span data-ttu-id="f6c36-234">Измените код представления в /Views/StoreManager/Delete.cshtml следующим образом.</span><span class="sxs-lookup"><span data-stu-id="f6c36-234">Change the view code in /Views/StoreManager/Delete.cshtml to the following.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

<span data-ttu-id="f6c36-235">Здесь показаны упрощенный подтверждение удаления.</span><span class="sxs-lookup"><span data-stu-id="f6c36-235">This displays a simplified Delete confirmation.</span></span>

![](mvc-music-store-part-5/_static/image13.png)

<span data-ttu-id="f6c36-236">Нажатие кнопки удаления приводит к обратной передаче на сервер, который выполняет действие DeleteConfirmed формы.</span><span class="sxs-lookup"><span data-stu-id="f6c36-236">Clicking the Delete button causes the form to be posted back to the server, which executes the DeleteConfirmed action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

<span data-ttu-id="f6c36-237">Действии HTTP-POST удалить контроллер выполняет следующие действия:</span><span class="sxs-lookup"><span data-stu-id="f6c36-237">Our HTTP-POST Delete Controller Action takes the following actions:</span></span>

- 1. <span data-ttu-id="f6c36-238">Загружает альбом по Идентификатору</span><span class="sxs-lookup"><span data-stu-id="f6c36-238">Loads the Album by ID</span></span>
- 2. <span data-ttu-id="f6c36-239">Удаляет его альбома и сохранить изменения</span><span class="sxs-lookup"><span data-stu-id="f6c36-239">Deletes it the album and save changes</span></span>
- 3. <span data-ttu-id="f6c36-240">Перенаправление в индекс, показывающий, что альбом был удален из списка</span><span class="sxs-lookup"><span data-stu-id="f6c36-240">Redirects to the Index, showing that the Album was removed from the list</span></span>

<span data-ttu-id="f6c36-241">Чтобы проверить это, запустите приложение и перейдите к /StoreManager.</span><span class="sxs-lookup"><span data-stu-id="f6c36-241">To test this, run the application and browse to /StoreManager.</span></span> <span data-ttu-id="f6c36-242">Выберите его из списка и щелкните ссылку «Удалить».</span><span class="sxs-lookup"><span data-stu-id="f6c36-242">Select an album from the list and click the Delete link.</span></span>

![](mvc-music-store-part-5/_static/image14.png)

<span data-ttu-id="f6c36-243">Откроется экран подтверждения нашей Delete.</span><span class="sxs-lookup"><span data-stu-id="f6c36-243">This displays our Delete confirmation screen.</span></span>

![](mvc-music-store-part-5/_static/image15.png)

<span data-ttu-id="f6c36-244">Нажав кнопку Delete удаляет альбом и возвращает нам страницу индекса Store Manager, которая показывает, что альбом была удалена.</span><span class="sxs-lookup"><span data-stu-id="f6c36-244">Clicking the Delete button removes the album and returns us to the Store Manager Index page, which shows that the album has been deleted.</span></span>

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a><span data-ttu-id="f6c36-245">Использование пользовательского вспомогательного метода HTML для усечения текста</span><span class="sxs-lookup"><span data-stu-id="f6c36-245">Using a custom HTML Helper to truncate text</span></span>

<span data-ttu-id="f6c36-246">У нас есть одна потенциальная проблема с нашей страницы индекса Store Manager.</span><span class="sxs-lookup"><span data-stu-id="f6c36-246">We've got one potential issue with our Store Manager Index page.</span></span> <span data-ttu-id="f6c36-247">Название альбома и имя исполнителя свойства могут быть достаточно длинным, что они могут сбросить наших форматирование таблицы.</span><span class="sxs-lookup"><span data-stu-id="f6c36-247">Our Album Title and Artist Name properties can both be long enough that they could throw off our table formatting.</span></span> <span data-ttu-id="f6c36-248">Мы создадим пользовательский вспомогательный метод HTML, которые позволяют легко выполнить усечение этих и других свойств в наши представления.</span><span class="sxs-lookup"><span data-stu-id="f6c36-248">We'll create a custom HTML Helper to allow us to easily truncate these and other properties in our Views.</span></span>

![](mvc-music-store-part-5/_static/image17.png)

<span data-ttu-id="f6c36-249">В Razor @helper синтаксис стало довольно легко создавать собственные вспомогательные функции для использования в представлениях.</span><span class="sxs-lookup"><span data-stu-id="f6c36-249">Razor's @helper syntax has made it pretty easy to create your own helper functions for use in your views.</span></span> <span data-ttu-id="f6c36-250">Откройте представление /Views/StoreManager/Index.cshtml и добавьте следующий код сразу после @model строки.</span><span class="sxs-lookup"><span data-stu-id="f6c36-250">Open the /Views/StoreManager/Index.cshtml view and add the following code directly after the @model line.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

<span data-ttu-id="f6c36-251">Этот вспомогательный метод принимает строку и максимальную длину, чтобы разрешить.</span><span class="sxs-lookup"><span data-stu-id="f6c36-251">This helper method takes a string and a maximum length to allow.</span></span> <span data-ttu-id="f6c36-252">Если значение текста, поставляемого короче указанной длины, вспомогательное приложение выводит его как-является.</span><span class="sxs-lookup"><span data-stu-id="f6c36-252">If the text supplied is shorter than the length specified, the helper outputs it as-is.</span></span> <span data-ttu-id="f6c36-253">Если длина его усекает текст и отображает «...», далее.</span><span class="sxs-lookup"><span data-stu-id="f6c36-253">If it is longer, then it truncates the text and renders "…" for the remainder.</span></span>

<span data-ttu-id="f6c36-254">Теперь можно использовать наши усечения вспомогательный для убедитесь, что имя исполнителя и название альбома свойства меньше 25 символов.</span><span class="sxs-lookup"><span data-stu-id="f6c36-254">Now we can use our Truncate helper to ensure that both the Album Title and Artist Name properties are less than 25 characters.</span></span> <span data-ttu-id="f6c36-255">Полное представление кода, используя наш новый вспомогательный метод усечения появляется.</span><span class="sxs-lookup"><span data-stu-id="f6c36-255">The complete view code using our new Truncate helper appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

<span data-ttu-id="f6c36-256">Теперь когда мы просматривают /StoreManager/ URL-адрес, альбомы и заголовки хранятся ниже наших максимальной длины.</span><span class="sxs-lookup"><span data-stu-id="f6c36-256">Now when we browse the /StoreManager/ URL, the albums and titles are kept below our maximum lengths.</span></span>

![](mvc-music-store-part-5/_static/image18.png)

<span data-ttu-id="f6c36-257">Примечание: Это показывает простой пример создания и использования вспомогательного метода в одном представлении.</span><span class="sxs-lookup"><span data-stu-id="f6c36-257">Note: This shows the simple case of creating and using a helper in one view.</span></span> <span data-ttu-id="f6c36-258">Дополнительные сведения о создании вспомогательные функции, которые можно использовать на протяжении всего веб-узла, см. в разделе в моем блоге: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span><span class="sxs-lookup"><span data-stu-id="f6c36-258">To learn more about creating helpers that you can use throughout your site, see my blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="f6c36-259">[Назад](mvc-music-store-part-4.md)
> [Вперед](mvc-music-store-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="f6c36-259">[Previous](mvc-music-store-part-4.md)
[Next](mvc-music-store-part-6.md)</span></span>
