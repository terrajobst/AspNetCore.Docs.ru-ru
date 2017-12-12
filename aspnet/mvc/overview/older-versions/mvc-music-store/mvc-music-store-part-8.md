---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: "Часть 8: Корзина для покупок с Ajax-обновления | Документы Microsoft"
author: jongalloway
description: "Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store. Часть 8 охватывает корзину для покупок с Ajax-обновления."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 75e1dff96f8b56d74c28ff9d522f4766fbad669f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="ec924-104">Часть 8: Корзине для покупок с Ajax-обновления</span><span class="sxs-lookup"><span data-stu-id="ec924-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="ec924-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="ec924-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="ec924-106">MVC Music Store является учебное приложение, которое вводятся и описываются пошаговые способы использования Visual Studio и ASP.NET MVC для веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="ec924-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="ec924-107">MVC Music Store показана реализация хранилища упрощенных образец продает велосипеды через Интернет альбомы, которое реализует Администрирование сайта основные, вход пользователей и функциональные возможности корзины покупок.</span><span class="sxs-lookup"><span data-stu-id="ec924-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="ec924-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="ec924-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="ec924-109">Часть 8 охватывает корзину для покупок с Ajax-обновления.</span><span class="sxs-lookup"><span data-stu-id="ec924-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="ec924-110">Мы будем разрешить пользователям поместите альбомов в их покупок без регистрации, но им необходимо зарегистрировать в качестве гостей завершения извлечения.</span><span class="sxs-lookup"><span data-stu-id="ec924-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="ec924-111">В процессе покупки и извлечения будут разделены на два контроллера: контроллер ShoppingCart, который позволяет добавлять элементы в корзину анонимно и контроллер извлечения, который обрабатывает процесс извлечения.</span><span class="sxs-lookup"><span data-stu-id="ec924-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="ec924-112">Мы будет начинаться с корзины в этом разделе, а затем построить процесс извлечения в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="ec924-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="ec924-113">Добавление классов модели покупок, Order и OrderDetail</span><span class="sxs-lookup"><span data-stu-id="ec924-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="ec924-114">Наши процессы извлечения и Корзина сделает использовать некоторые новые классы.</span><span class="sxs-lookup"><span data-stu-id="ec924-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="ec924-115">Щелкните правой кнопкой мыши папку модели и добавьте класс покупок (Cart.cs) следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="ec924-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="ec924-116">Этот класс является довольно аналогично другим пользователям, мы использовали таким образом, за исключением атрибута [Key] RecordId свойства.</span><span class="sxs-lookup"><span data-stu-id="ec924-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="ec924-117">Наш покупок элементы будут иметь строковый идентификатор, с именем CartID, чтобы разрешить анонимные покупок, но таблица содержит первичный ключ целое число со знаком, с именем RecordId.</span><span class="sxs-lookup"><span data-stu-id="ec924-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="ec924-118">По соглашению сначала с Entity Framework код ожидает, что первичный ключ для таблицы с именем покупок будет CartId или идентификатор, что мы легко переопределить, через код или заметок при необходимости.</span><span class="sxs-lookup"><span data-stu-id="ec924-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="ec924-119">Ниже приведен пример того, как мы можно использовать простой соглашения в Entity Framework сначала код при их соответствии нам, но мы вы не ограничены их при необходимости.</span><span class="sxs-lookup"><span data-stu-id="ec924-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="ec924-120">Добавьте класс Order (Order.cs) следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="ec924-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="ec924-121">Этот класс отслеживает сведения сводки и доставки для заказа.</span><span class="sxs-lookup"><span data-stu-id="ec924-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="ec924-122">**Он не может быть скомпилирован еще**, поскольку он имеет свойство навигации OrderDetails, зависящее от класса, мы еще не созданы.</span><span class="sxs-lookup"><span data-stu-id="ec924-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="ec924-123">Исправим, что теперь, добавив класс с именем OrderDetail.cs, добавив следующий код.</span><span class="sxs-lookup"><span data-stu-id="ec924-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="ec924-124">Мы выполним одно последнее обновление с нашей MusicStoreEntities класса для включения DbSets которого предоставлять эти новые классы модели, включая DbSet также&lt;исполнителя&gt;.</span><span class="sxs-lookup"><span data-stu-id="ec924-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="ec924-125">Обновленный MusicStoreEntities класс отображается как ниже.</span><span class="sxs-lookup"><span data-stu-id="ec924-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="ec924-126">Управление бизнес-логики корзины</span><span class="sxs-lookup"><span data-stu-id="ec924-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="ec924-127">Далее мы создадим ShoppingCart класс в папке «Models».</span><span class="sxs-lookup"><span data-stu-id="ec924-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="ec924-128">Модель ShoppingCart обрабатывает доступ к данным в таблице покупок.</span><span class="sxs-lookup"><span data-stu-id="ec924-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="ec924-129">Кроме того он будет обрабатывать бизнес-логику для добавления и удаления элементов из корзины.</span><span class="sxs-lookup"><span data-stu-id="ec924-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="ec924-130">Так как мы не хотим требовать от пользователей зарегистрировать учетную запись только для добавления элементов в его корзине, мы назначит пользователям временные уникальный идентификатор (используя GUID или глобальный уникальный идентификатор) при обращении к покупательской корзине.</span><span class="sxs-lookup"><span data-stu-id="ec924-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="ec924-131">Мы будем хранить этот идентификатор, с помощью класса сеанса ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ec924-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="ec924-132">*Примечание: Сеанс ASP.NET удобно хранить сведения о пользователе, который истекает после окончания сайта. Во время использования состояния сеанса влияют на производительность для больших сайтов, светлой использование будет хорошо подходят для демонстрационных целей.*</span><span class="sxs-lookup"><span data-stu-id="ec924-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="ec924-133">Класс ShoppingCart предоставляет следующие методы:</span><span class="sxs-lookup"><span data-stu-id="ec924-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="ec924-134">**AddToCart** принимает альбом как параметр и добавляет ее в корзину пользователя.</span><span class="sxs-lookup"><span data-stu-id="ec924-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="ec924-135">Так как в таблице покупок, отслеживает количество для каждого альбома содержит логику при необходимости создайте новую строку, или просто увеличить количество, если пользователь уже упорядочен одной копии альбома.</span><span class="sxs-lookup"><span data-stu-id="ec924-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="ec924-136">**RemoveFromCart** принимает идентификатор альбома и удаляет его из корзины пользователя.</span><span class="sxs-lookup"><span data-stu-id="ec924-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="ec924-137">Если пользователь в их покупок были только одна копия альбома, строка удаляется.</span><span class="sxs-lookup"><span data-stu-id="ec924-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="ec924-138">**EmptyCart** удаляет все элементы из корзины пользователя.</span><span class="sxs-lookup"><span data-stu-id="ec924-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="ec924-139">**GetCartItems** извлекает список CartItems для отображения и обработки.</span><span class="sxs-lookup"><span data-stu-id="ec924-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="ec924-140">**GetCount** возвращает общее количество альбомы пользователем в его корзине.</span><span class="sxs-lookup"><span data-stu-id="ec924-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="ec924-141">**GetTotal, используемую** вычисляет стоимость всех элементов в корзине.</span><span class="sxs-lookup"><span data-stu-id="ec924-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="ec924-142">**CreateOrder** преобразуется в корзину заказа на этапе оформления заказа.</span><span class="sxs-lookup"><span data-stu-id="ec924-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="ec924-143">**GetCart** имеет статический метод, который позволяет нашей контроллерах, чтобы получить объект покупок.</span><span class="sxs-lookup"><span data-stu-id="ec924-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="ec924-144">Она использует **GetCartId** метод для обработки чтения CartId из сеанса пользователя.</span><span class="sxs-lookup"><span data-stu-id="ec924-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="ec924-145">Метод GetCartId требует HttpContextBase, чтобы он мог читать CartId пользователя из сеанса пользователя.</span><span class="sxs-lookup"><span data-stu-id="ec924-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="ec924-146">Ниже приведен полный **класс ShoppingCart**:</span><span class="sxs-lookup"><span data-stu-id="ec924-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="ec924-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="ec924-147">ViewModels</span></span>

<span data-ttu-id="ec924-148">Наш контроллер корзины покупок необходимо сообщить некоторые сложные сведения для его представления, которой не полностью сопоставлен нашей модели объектов.</span><span class="sxs-lookup"><span data-stu-id="ec924-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="ec924-149">Мы не хотим изменить наши модели в соответствии с нашей представлений; Классы модели должно представлять нашей домена не пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="ec924-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="ec924-150">Одним из решений будет передачи сведений в нашем представления с помощью класса ViewBag, как мы сделали раскрывающийся список сведениями директор магазина, однако передача больших данных через ViewBag возвращает трудности в управлении.</span><span class="sxs-lookup"><span data-stu-id="ec924-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="ec924-151">Решением является использование *ViewModel* шаблон.</span><span class="sxs-lookup"><span data-stu-id="ec924-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="ec924-152">При использовании этого шаблона создается строго типизированные классы, оптимизированных для сценариев определенных представлений, который предоставляют свойства для динамического значения или содержимого, необходимые для наших шаблонов представлений.</span><span class="sxs-lookup"><span data-stu-id="ec924-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="ec924-153">Наши классы контроллера можно заполнить и передать нашей Просмотр шаблона для использования этих классов, оптимизированными для представления.</span><span class="sxs-lookup"><span data-stu-id="ec924-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="ec924-154">Это позволяет безопасность типа, проверка во время компиляции и редактор IntelliSense внутри шаблонов представлений.</span><span class="sxs-lookup"><span data-stu-id="ec924-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="ec924-155">Мы создадим две модели представления для использования в нашем контроллер корзины: ShoppingCartViewModel будет хранить содержимое в корзину пользователя и ShoppingCartRemoveViewModel будет использоваться для отображения сведений подтверждения, когда пользователь удаляет нечто с их покупок.</span><span class="sxs-lookup"><span data-stu-id="ec924-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="ec924-156">Давайте создадим новую папку ViewModels в корне проекта для Систематизация.</span><span class="sxs-lookup"><span data-stu-id="ec924-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="ec924-157">Щелкните правой кнопкой мыши проект, выберите пункт Добавить или создать папку.</span><span class="sxs-lookup"><span data-stu-id="ec924-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="ec924-158">Имя папки ViewModels.</span><span class="sxs-lookup"><span data-stu-id="ec924-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="ec924-159">Добавьте класс ShoppingCartViewModel в папке ViewModels.</span><span class="sxs-lookup"><span data-stu-id="ec924-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="ec924-160">Он имеет два свойства: список элементов для покупок и десятичное значение для хранения совокупная стоимость для всех элементов в корзине.</span><span class="sxs-lookup"><span data-stu-id="ec924-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="ec924-161">Теперь добавьте ShoppingCartRemoveViewModel папке ViewModels с следующие четыре свойства.</span><span class="sxs-lookup"><span data-stu-id="ec924-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="ec924-162">Контроллер корзины</span><span class="sxs-lookup"><span data-stu-id="ec924-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="ec924-163">Контроллер корзины имеет три основные функции: Добавление элементов в корзину, удаление элементов из корзины и просмотре элементов в корзине.</span><span class="sxs-lookup"><span data-stu-id="ec924-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="ec924-164">Это сделает использовать из трех классов, мы только что создали: ShoppingCartViewModel, ShoppingCartRemoveViewModel и ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="ec924-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="ec924-165">Как и StoreController и StoreManagerController мы добавим поле для хранения экземпляра MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="ec924-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="ec924-166">Добавьте новый контроллер корзины в проект с помощью шаблона пустой контроллер.</span><span class="sxs-lookup"><span data-stu-id="ec924-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="ec924-167">Ниже приведен полный ShoppingCart контроллера.</span><span class="sxs-lookup"><span data-stu-id="ec924-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="ec924-168">Действия индекса и добавления контроллера похожа на.</span><span class="sxs-lookup"><span data-stu-id="ec924-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="ec924-169">Удаление и CartSummary действия контроллера обрабатывать два особых случаев, которые будут рассмотрены в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="ec924-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="ec924-170">JQuery AJAX-обновления</span><span class="sxs-lookup"><span data-stu-id="ec924-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="ec924-171">Далее мы создадим страницу индекса корзину покупок, строго типизированными ShoppingCartViewModel и использует шаблон представления списка, используя тот же метод.</span><span class="sxs-lookup"><span data-stu-id="ec924-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="ec924-172">Однако вместо Html.ActionLink для удаления элементов из корзины, мы будем использовать jQuery для «подключения» событие click для всех связей в этом представлении, имеющие класс HTML RemoveLink.</span><span class="sxs-lookup"><span data-stu-id="ec924-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="ec924-173">Вместо того чтобы учет формы, этот обработчик событий click просто сделает обратный вызов AJAX для наших RemoveFromCart действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="ec924-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="ec924-174">RemoveFromCart возвращает результат сериализации JSON, нашей обратного вызова jQuery анализирующей и затем выполняет четыре быстрого обновления с помощью jQuery:</span><span class="sxs-lookup"><span data-stu-id="ec924-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="ec924-175">Удаляет из списка удаленных альбом</span><span class="sxs-lookup"><span data-stu-id="ec924-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="ec924-176">Обновляет счетчик для покупок в заголовке</span><span class="sxs-lookup"><span data-stu-id="ec924-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="ec924-177">Отображает сообщение об обновлении для пользователя</span><span class="sxs-lookup"><span data-stu-id="ec924-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="ec924-178">Обновляет совокупная стоимость корзины</span><span class="sxs-lookup"><span data-stu-id="ec924-178">Updates the cart total price</span></span>

<span data-ttu-id="ec924-179">Поскольку сценарий удаления обрабатываются обратный вызов Ajax в представлении индекса, нам не нужен дополнительного представления для RemoveFromCart действия.</span><span class="sxs-lookup"><span data-stu-id="ec924-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="ec924-180">Ниже приведен полный код для представления /ShoppingCart/Index:</span><span class="sxs-lookup"><span data-stu-id="ec924-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="ec924-181">Чтобы убедиться, нам нужно иметь возможность добавить элементы в наш список покупок.</span><span class="sxs-lookup"><span data-stu-id="ec924-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="ec924-182">Мы обновим наши **сведения о хранилище** представление, чтобы включить кнопку «Добавить в корзину».</span><span class="sxs-lookup"><span data-stu-id="ec924-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="ec924-183">Хотя мы его, мы перечислены некоторые дополнительные сведения о диске, который мы добавили с момента последнего мы обновления в этом представлении: жанра, исполнителя, цены и альбома.</span><span class="sxs-lookup"><span data-stu-id="ec924-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="ec924-184">Обновленный код представления сведения о хранилище отображается, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="ec924-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="ec924-185">Теперь мы щелкните через магазин и тестирования, добавление и удаление альбомы и из наших корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="ec924-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="ec924-186">Запустите приложение и найдите индекс хранилища.</span><span class="sxs-lookup"><span data-stu-id="ec924-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="ec924-187">Нажмите кнопку жанра, чтобы просмотреть список альбомов.</span><span class="sxs-lookup"><span data-stu-id="ec924-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="ec924-188">Теперь щелкните название альбома отобразить наш обновленные сведения об альбоме представления, включая кнопку «Добавить в корзину».</span><span class="sxs-lookup"><span data-stu-id="ec924-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="ec924-189">Нажав кнопку «Добавить в корзину» показано нашей представление индекса корзину для покупок с сводном списке корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="ec924-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="ec924-190">После загрузки копии корзине для покупок, можно щелкнуть удалить из корзины ссылку для просмотра обновлений Ajax в корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="ec924-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="ec924-191">Мы создали ожидания рабочего Корзина для покупок, что позволяет отменить регистрацию пользователей для добавления элементов в корзину.</span><span class="sxs-lookup"><span data-stu-id="ec924-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="ec924-192">В следующем разделе будет разрешить им для регистрации и завершить процесс извлечения.</span><span class="sxs-lookup"><span data-stu-id="ec924-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


>[!div class="step-by-step"]
<span data-ttu-id="ec924-193">[Назад](mvc-music-store-part-7.md)
[Вперед](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="ec924-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
