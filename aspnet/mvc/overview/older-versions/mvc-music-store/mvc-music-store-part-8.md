---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Часть 8: Корзина для покупок с обновлениями Ajax | Документация Майкрософт'
author: jongalloway
description: В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store. Часть 8 рассматриваются корзину для покупок с обновлениями Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 327b7ee4e302188323c229c231ae750cbed709a1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369800"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="56cdf-104">Часть 8: Корзина для покупок с обновлениями Ajax</span><span class="sxs-lookup"><span data-stu-id="56cdf-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="56cdf-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="56cdf-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="56cdf-106">Store музыки MVC является учебного приложения, которое вводятся и описываются пошаговые использование ASP.NET MVC и Visual Studio для разработки веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="56cdf-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="56cdf-107">Store музыки MVC — это реализация хранилища упрощенный пример, который продает музыкальные альбомы через Интернет и реализует базовый сайт администрирования, вход пользователя и корзины для покупок функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="56cdf-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="56cdf-108">В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="56cdf-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="56cdf-109">Часть 8 рассматриваются корзину для покупок с обновлениями Ajax.</span><span class="sxs-lookup"><span data-stu-id="56cdf-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="56cdf-110">Я разрешу пользователям помещать альбомов в их для покупок без регистрации, но им нужно будет зарегистрировать в качестве гостей для завершения извлечения.</span><span class="sxs-lookup"><span data-stu-id="56cdf-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="56cdf-111">В процессе покупки и извлечения будут разделены на два контроллера: контроллер ShoppingCart, который разрешает анонимно Добавление элементов в корзину и извлечение контроллер, который обрабатывает процесс подсчета стоимости сначала.</span><span class="sxs-lookup"><span data-stu-id="56cdf-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="56cdf-112">Мы будем приступить к корзине для покупок в этом разделе, а затем сформируйте процесс подсчета стоимости покупок в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="56cdf-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="56cdf-113">Добавление классов модели для покупок, Order и OrderDetail</span><span class="sxs-lookup"><span data-stu-id="56cdf-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="56cdf-114">Сделает наши процессы корзину для покупок и извлечение использование некоторых новых классов.</span><span class="sxs-lookup"><span data-stu-id="56cdf-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="56cdf-115">Щелкните правой кнопкой мыши папку Models и добавьте класс корзины для покупок (Cart.cs) следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="56cdf-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="56cdf-116">Этот класс является довольно аналогично другим пользователям, которые мы использовали на данный момент, за исключением атрибута [Key] для свойства RecordId.</span><span class="sxs-lookup"><span data-stu-id="56cdf-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="56cdf-117">Наши элементам корзины покупок будет иметь идентификатор строки с именем CartID, чтобы разрешить анонимный покупок, но в таблице содержатся целочисленного первичного ключа с именем RecordId.</span><span class="sxs-lookup"><span data-stu-id="56cdf-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="56cdf-118">По соглашению Entity Framework Code First ожидает, что первичный ключ для таблицы с именем для покупок будет CartId или идентификатор, что мы можно легко изменить, с помощью заметок или кода при необходимости.</span><span class="sxs-lookup"><span data-stu-id="56cdf-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="56cdf-119">Ниже приведен пример того, как мы можно использовать простой соглашения в Entity Framework Code-First при соответствии им с нами, но мы не являетесь ограничен типом их, когда они не.</span><span class="sxs-lookup"><span data-stu-id="56cdf-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="56cdf-120">Добавьте класс Order (Order.cs) следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="56cdf-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="56cdf-121">Этот класс отслеживает сведения сводки и доставки для заказа.</span><span class="sxs-lookup"><span data-stu-id="56cdf-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="56cdf-122">**Она не будет компилироваться еще**, так как он имеет свойство навигации OrderDetails зависящее от класса, мы еще не созданы.</span><span class="sxs-lookup"><span data-stu-id="56cdf-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="56cdf-123">Исправим внимание на то, что теперь, добавив класс с именем OrderDetail.cs, добавив следующий код.</span><span class="sxs-lookup"><span data-stu-id="56cdf-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="56cdf-124">Мы внесем одно последнее обновление к нашему классу MusicStoreEntities для включения DbSets, которую позднее предоставите эти новые классы модели, включая DbSet также&lt;исполнителя&gt;.</span><span class="sxs-lookup"><span data-stu-id="56cdf-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="56cdf-125">Обновленный класс MusicStoreEntities отображается как ниже.</span><span class="sxs-lookup"><span data-stu-id="56cdf-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="56cdf-126">Управление бизнес-логики корзину для покупок</span><span class="sxs-lookup"><span data-stu-id="56cdf-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="56cdf-127">Далее мы создадим класс ShoppingCart в папку Models.</span><span class="sxs-lookup"><span data-stu-id="56cdf-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="56cdf-128">Модель ShoppingCart обрабатывает доступ к данным в таблицу для покупок.</span><span class="sxs-lookup"><span data-stu-id="56cdf-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="56cdf-129">Кроме того он будет обрабатывать бизнес-логику для добавления и удаления элементов из корзины.</span><span class="sxs-lookup"><span data-stu-id="56cdf-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="56cdf-130">Так как мы не хотим пользователям необходимо зарегистрировать учетную запись только для добавления элементов в корзину, будут назначены пользователям временный уникальный идентификатор (используя GUID или глобальный уникальный идентификатор) при получении доступа к корзине для покупок.</span><span class="sxs-lookup"><span data-stu-id="56cdf-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="56cdf-131">Мы будем хранить этот идентификатор, с помощью класса сеанса ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="56cdf-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="56cdf-132">*Примечание: В сеансе ASP.NET — это удобное место для хранения данных конкретного пользователя, которой истечет после увольнения сайта. Хотя неправильное использование состояния сеанса может негативно влиять на производительность на больших сайтах, света использование хорошо работает для демонстрационных целей.*</span><span class="sxs-lookup"><span data-stu-id="56cdf-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="56cdf-133">Класс ShoppingCart предоставляет следующие методы:</span><span class="sxs-lookup"><span data-stu-id="56cdf-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="56cdf-134">**AddToCart** принимает альбом в качестве параметра и добавляет его в корзину пользователя.</span><span class="sxs-lookup"><span data-stu-id="56cdf-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="56cdf-135">Так как в таблице покупок, отслеживает количество для каждого альбома содержит логику для создания новой строки, при необходимости или просто увеличить количество, если пользователь уже заказать одну копию альбома.</span><span class="sxs-lookup"><span data-stu-id="56cdf-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="56cdf-136">**RemoveFromCart** принимает идентификатор альбома и удаляет его из корзины пользователя.</span><span class="sxs-lookup"><span data-stu-id="56cdf-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="56cdf-137">Если пользователь только одна копия альбом в их для покупок, то строка удаляется.</span><span class="sxs-lookup"><span data-stu-id="56cdf-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="56cdf-138">**EmptyCart** удаляет все элементы из корзины пользователя.</span><span class="sxs-lookup"><span data-stu-id="56cdf-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="56cdf-139">**GetCartItems** извлекает список CartItems для отображения или обработки.</span><span class="sxs-lookup"><span data-stu-id="56cdf-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="56cdf-140">**GetCount** извлекает общее число альбомов, у пользователя в его корзине.</span><span class="sxs-lookup"><span data-stu-id="56cdf-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="56cdf-141">**GetTotal, используемую** вычисляет общую стоимость всех элементов в корзине.</span><span class="sxs-lookup"><span data-stu-id="56cdf-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="56cdf-142">**CreateOrder** преобразует Корзина для покупок в заказ на этапе оформления заказа.</span><span class="sxs-lookup"><span data-stu-id="56cdf-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="56cdf-143">**GetCart** — это статический метод, позволяющий наших контроллерах, чтобы получить объект корзины для покупок.</span><span class="sxs-lookup"><span data-stu-id="56cdf-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="56cdf-144">Она использует **GetCartId** метод для обработки чтения CartId из сеанса пользователя.</span><span class="sxs-lookup"><span data-stu-id="56cdf-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="56cdf-145">Метод GetCartId требует HttpContextBase таким образом, он может считывать CartId пользователя из сеанса пользователя.</span><span class="sxs-lookup"><span data-stu-id="56cdf-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="56cdf-146">Ниже приведен полный **класс ShoppingCart**:</span><span class="sxs-lookup"><span data-stu-id="56cdf-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="56cdf-147">Модели представлений</span><span class="sxs-lookup"><span data-stu-id="56cdf-147">ViewModels</span></span>

<span data-ttu-id="56cdf-148">Контроллер корзины для покупок, потребуется некоторые сложную информацию для своих представлений, который не полностью сопоставлен с нашей модели объектов.</span><span class="sxs-lookup"><span data-stu-id="56cdf-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="56cdf-149">Мы не хотим изменить наши модели в соответствии с наши представления; Классы модели должна представлять наш домен, а не пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="56cdf-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="56cdf-150">Одним из решений будет для передачи информации в наши представления, с помощью класса ViewBag, как мы сделали с информацией раскрывающийся список Store Manager, однако передача больших данных с помощью ViewBag получает сложнее в управлении.</span><span class="sxs-lookup"><span data-stu-id="56cdf-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="56cdf-151">Решением является использование *ViewModel* шаблон.</span><span class="sxs-lookup"><span data-stu-id="56cdf-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="56cdf-152">При использовании этого шаблона, мы создаем классы со строгой типизацией, оптимизированных для наших вариантов использования конкретного представления и предоставляют свойства для динамического значения или содержимого необходимые наших шаблонов представлений.</span><span class="sxs-lookup"><span data-stu-id="56cdf-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="56cdf-153">Наши классы контроллера можно заполнить и передать шаблон представления для использования этих классов, оптимизированный для представления.</span><span class="sxs-lookup"><span data-stu-id="56cdf-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="56cdf-154">Благодаря этому безопасность типов, проверки во время компиляции и редактор IntelliSense в шаблоны представлений.</span><span class="sxs-lookup"><span data-stu-id="56cdf-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="56cdf-155">Мы создадим две модели представления для использования в своем контроллере корзину для покупок: ShoppingCartViewModel будет содержать содержимое корзины пользователя, и чтобы отобразить сведения о подтверждении, когда пользователь удаляет что-то будет использоваться ShoppingCartRemoveViewModel из корзины покупателя.</span><span class="sxs-lookup"><span data-stu-id="56cdf-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="56cdf-156">Давайте создадим новую папку ViewModels в корне нашего проекта, чтобы не усложнять организации.</span><span class="sxs-lookup"><span data-stu-id="56cdf-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="56cdf-157">Щелкните правой кнопкой мыши проект, выберите Add / новую папку.</span><span class="sxs-lookup"><span data-stu-id="56cdf-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="56cdf-158">Назовите папку ViewModels.</span><span class="sxs-lookup"><span data-stu-id="56cdf-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="56cdf-159">Добавьте в класс ShoppingCartViewModel в папку ViewModels.</span><span class="sxs-lookup"><span data-stu-id="56cdf-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="56cdf-160">Он имеет два свойства: список элементов корзины покупок и десятичное значение для хранения итоговая цена для всех элементов в корзине.</span><span class="sxs-lookup"><span data-stu-id="56cdf-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="56cdf-161">Теперь добавьте ShoppingCartRemoveViewModel в папку ViewModels с следующие четыре свойства.</span><span class="sxs-lookup"><span data-stu-id="56cdf-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="56cdf-162">Контроллер корзины</span><span class="sxs-lookup"><span data-stu-id="56cdf-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="56cdf-163">Корзина для покупок контроллер имеет три основные функции: Добавление элементов в корзину, удаление элементов из корзины и просмотр элементов в корзине.</span><span class="sxs-lookup"><span data-stu-id="56cdf-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="56cdf-164">Это сделает использовать из трех классов, мы только что создали: ShoppingCartViewModel ShoppingCartRemoveViewModel и ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="56cdf-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="56cdf-165">Как и в StoreController и StoreManagerController мы добавим поле, содержащее экземпляр MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="56cdf-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="56cdf-166">Добавите новый контроллер корзину для покупок в проект с помощью шаблона пустой контроллер.</span><span class="sxs-lookup"><span data-stu-id="56cdf-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="56cdf-167">Ниже приведен полный контроллер ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="56cdf-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="56cdf-168">Действия индекса и добавление контроллера покажется очень знакомой.</span><span class="sxs-lookup"><span data-stu-id="56cdf-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="56cdf-169">Удаление и CartSummary действия контроллера обрабатывать два особых случаев, которые мы обсудим в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="56cdf-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="56cdf-170">AJAX-обновления с jQuery</span><span class="sxs-lookup"><span data-stu-id="56cdf-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="56cdf-171">Далее мы создадим страницу индекса Корзина для покупок, строго типизируется с ShoppingCartViewModel и использует шаблон представления списка, используя тот же метод.</span><span class="sxs-lookup"><span data-stu-id="56cdf-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="56cdf-172">Тем не менее вместо использования Html.ActionLink для удаления элементов из корзины, мы будем использовать jQuery для «подключения» события щелчка для всех ссылок в этом представлении, имеющие класс HTML RemoveLink.</span><span class="sxs-lookup"><span data-stu-id="56cdf-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="56cdf-173">Вместо того чтобы обратной передачи формы, этот обработчик событий click просто сделает обратный вызов AJAX к действию контроллера наших RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="56cdf-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="56cdf-174">RemoveFromCart возвращает результат сериализации JSON, который затем выполняет синтаксический анализ наш jQuery обратного вызова и выполняет четыре быстрого обновления страницы с помощью jQuery:</span><span class="sxs-lookup"><span data-stu-id="56cdf-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="56cdf-175">Удаляет из списка удаленных альбома</span><span class="sxs-lookup"><span data-stu-id="56cdf-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="56cdf-176">Обновляет счетчик для покупок в заголовке</span><span class="sxs-lookup"><span data-stu-id="56cdf-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="56cdf-177">Отображает сообщение об обновлении для пользователя</span><span class="sxs-lookup"><span data-stu-id="56cdf-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="56cdf-178">Обновляет итоговая цена для покупок</span><span class="sxs-lookup"><span data-stu-id="56cdf-178">Updates the cart total price</span></span>

<span data-ttu-id="56cdf-179">Так как в сценарии remove обрабатываются обратного вызова Ajax в представлении индекса, мы не требуется дополнительного представления для RemoveFromCart действие.</span><span class="sxs-lookup"><span data-stu-id="56cdf-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="56cdf-180">Ниже приведен полный код для представления /ShoppingCart/Index:</span><span class="sxs-lookup"><span data-stu-id="56cdf-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="56cdf-181">Чтобы убедиться, нам нужно иметь возможность добавлять элементы в нашей корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="56cdf-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="56cdf-182">Мы обновим наши **сведения Store** представление, включив кнопка «Добавить в корзину».</span><span class="sxs-lookup"><span data-stu-id="56cdf-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="56cdf-183">Хотя мы взялись за это, мы может включать некоторые дополнительные сведения о диске, который мы добавили так, как мы последнего обновления в этом представлении: жанра, исполнителя, цены и альбома.</span><span class="sxs-lookup"><span data-stu-id="56cdf-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="56cdf-184">Обновленные сведения Store код представления выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="56cdf-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="56cdf-185">Теперь мы помогают выбрать хранилище и тестирования, добавление и удаление альбомов в и из наших корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="56cdf-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="56cdf-186">Запустите приложение и перейдите к индексу Store.</span><span class="sxs-lookup"><span data-stu-id="56cdf-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="56cdf-187">Далее щелкните жанра, чтобы просмотреть список альбомов.</span><span class="sxs-lookup"><span data-stu-id="56cdf-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="56cdf-188">Теперь щелкните название альбома отобразить нашего обновленные сведения об альбоме представления, включая кнопка «Добавить в корзину».</span><span class="sxs-lookup"><span data-stu-id="56cdf-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="56cdf-189">Нажатие кнопки «Добавить в корзину» показывает нашего представления индекса корзину для покупок в сводном списке корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="56cdf-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="56cdf-190">После загрузки вашей корзины для покупок, можно щелкнуть удалить из корзины для покупок ссылку, чтобы увидеть обновление Ajax в корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="56cdf-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="56cdf-191">Мы создали out рабочее Корзина для покупок, что позволяет незарегистрированных пользователей для добавления элементов в корзину.</span><span class="sxs-lookup"><span data-stu-id="56cdf-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="56cdf-192">В следующем разделе будет разрешить их регистрации и завершить процесс подсчета стоимости сначала.</span><span class="sxs-lookup"><span data-stu-id="56cdf-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="56cdf-193">[Назад](mvc-music-store-part-7.md)
> [Вперед](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="56cdf-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
