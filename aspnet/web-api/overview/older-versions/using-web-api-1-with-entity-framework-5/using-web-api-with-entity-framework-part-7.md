---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Часть 7: Создание основной страницы | Документы Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2018
ms.locfileid: "30869872"
---
<a name="part-7-creating-the-main-page"></a><span data-ttu-id="f322f-102">Часть 7: Создание основной страницы</span><span class="sxs-lookup"><span data-stu-id="f322f-102">Part 7: Creating the Main Page</span></span>
====================
<span data-ttu-id="f322f-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f322f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f322f-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="f322f-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="f322f-105">Создание основной страницы</span><span class="sxs-lookup"><span data-stu-id="f322f-105">Creating the Main Page</span></span>

<span data-ttu-id="f322f-106">В этом разделе вы создадите страница основного приложения.</span><span class="sxs-lookup"><span data-stu-id="f322f-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="f322f-107">Эта страница будет сложнее, чем для страницы, поэтому мы будем подхода в нескольких шагах.</span><span class="sxs-lookup"><span data-stu-id="f322f-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="f322f-108">Попутно вы увидите более сложные приемы Knockout.js.</span><span class="sxs-lookup"><span data-stu-id="f322f-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="f322f-109">Ниже приведен базовый макет страницы.</span><span class="sxs-lookup"><span data-stu-id="f322f-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="f322f-110">«Продукты» содержит массив продуктов.</span><span class="sxs-lookup"><span data-stu-id="f322f-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="f322f-111">«Корзина» содержит массив продуктов с количествами.</span><span class="sxs-lookup"><span data-stu-id="f322f-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="f322f-112">Нажав кнопку «Добавить в корзину» обновляет покупок.</span><span class="sxs-lookup"><span data-stu-id="f322f-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="f322f-113">«Orders» содержит массив идентификаторы заказов.</span><span class="sxs-lookup"><span data-stu-id="f322f-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="f322f-114">«Подробности» содержит сведения о заказе, который является массивом элементов (продукты с количествами)</span><span class="sxs-lookup"><span data-stu-id="f322f-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="f322f-115">Мы начнем путем определения некоторых базовый макет в формате HTML, без привязки данных или скрипт.</span><span class="sxs-lookup"><span data-stu-id="f322f-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="f322f-116">Откройте файл Views/Home/Index.cshtml и замените содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="f322f-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="f322f-117">Затем добавьте раздел скриптов и создать пустую модель представление:</span><span class="sxs-lookup"><span data-stu-id="f322f-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="f322f-118">На основе дизайна составила более ранних версий, нашей модели представления должен наблюдаемые объекты для продуктов, покупок, заказов и сведений.</span><span class="sxs-lookup"><span data-stu-id="f322f-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="f322f-119">Добавьте следующие переменные для `AppViewModel` объекта:</span><span class="sxs-lookup"><span data-stu-id="f322f-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="f322f-120">Пользователей можно добавить элементы из списка продуктов в корзине и удаления элементов из корзины.</span><span class="sxs-lookup"><span data-stu-id="f322f-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="f322f-121">Чтобы инкапсулировать эти функции, мы создадим другой класс модели представления, который представляет продукт.</span><span class="sxs-lookup"><span data-stu-id="f322f-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="f322f-122">Добавьте следующий код в файл `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="f322f-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="f322f-123">`ProductViewModel` Класс содержит две функции, которые используются для перемещения продукта к и из корзины: `addItemToCart` добавляет одну единицу продукта в корзину и `removeAllFromCart` удаляет все количества продукта.</span><span class="sxs-lookup"><span data-stu-id="f322f-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="f322f-124">Пользователи могут выбрать существующий заказ и получить сведения о заказе.</span><span class="sxs-lookup"><span data-stu-id="f322f-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="f322f-125">Мы будем инкапсулировать эти функции в другой модели представления:</span><span class="sxs-lookup"><span data-stu-id="f322f-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="f322f-126">`OrderDetailsViewModel` Инициализируется с заказом, и он извлекает сведения о заказе, отправив запрос AJAX на сервер.</span><span class="sxs-lookup"><span data-stu-id="f322f-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="f322f-127">Кроме того, обратите внимание, `total` свойство `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="f322f-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="f322f-128">Это свойство имеет специальный вид наблюдаемый объект называется [вычисляемые наблюдаемый объект](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="f322f-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="f322f-129">Как следует из имен, вычисляемых наблюдаемым позволяет привязки данных к вычисляемым значением&#8212;в этом случае общее стоимость заказа.</span><span class="sxs-lookup"><span data-stu-id="f322f-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="f322f-130">Добавьте эти функции для `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="f322f-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="f322f-131">`resetCart` Удаляет все элементы из корзины.</span><span class="sxs-lookup"><span data-stu-id="f322f-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="f322f-132">`getDetails` Получает сведения для заказа (с pusing новый `OrderDetailsViewModel` на `details` списка).</span><span class="sxs-lookup"><span data-stu-id="f322f-132">`getDetails` gets the details for an order (by pusing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="f322f-133">`createOrder` Создает новый порядок и очистке корзины для покупок.</span><span class="sxs-lookup"><span data-stu-id="f322f-133">`createOrder` creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="f322f-134">Наконец инициализируйте модели представления, выполняющим запросы AJAX products и orders:</span><span class="sxs-lookup"><span data-stu-id="f322f-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="f322f-135">ОК большого объема кода, но мы создали его вверх пошаговые инструкции надеемся проекта снимите флажок.</span><span class="sxs-lookup"><span data-stu-id="f322f-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="f322f-136">Теперь можно добавить некоторые Knockout.js привязки к элементу HTML.</span><span class="sxs-lookup"><span data-stu-id="f322f-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="f322f-137">**Продукты**</span><span class="sxs-lookup"><span data-stu-id="f322f-137">**Products**</span></span>

<span data-ttu-id="f322f-138">Ниже приведены привязки для списка продуктов.</span><span class="sxs-lookup"><span data-stu-id="f322f-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="f322f-139">При этом выполняет итерацию по массиву продуктов и отображаются имя и цену.</span><span class="sxs-lookup"><span data-stu-id="f322f-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="f322f-140">Кнопка «Добавить для заказа» отображается только в том случае, когда пользователь входит в систему.</span><span class="sxs-lookup"><span data-stu-id="f322f-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="f322f-141">Вызовы кнопка «Добавить для заказа» `addItemToCart` на `ProductViewModel` экземпляра для продукта.</span><span class="sxs-lookup"><span data-stu-id="f322f-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="f322f-142">Этот пример демонстрирует интересной функцией Knockout.js: Если модель представления содержит другие Просмотр моделей, привязки можно применять к внутренние модели.</span><span class="sxs-lookup"><span data-stu-id="f322f-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="f322f-143">В этом примере привязок в `foreach` применяются к каждому из `ProductViewModel` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="f322f-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="f322f-144">Этот подход гораздо очистки, помещая все функциональные возможности в одной модели представления.</span><span class="sxs-lookup"><span data-stu-id="f322f-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="f322f-145">**Корзина**</span><span class="sxs-lookup"><span data-stu-id="f322f-145">**Cart**</span></span>

<span data-ttu-id="f322f-146">Ниже приведены привязки для покупок.</span><span class="sxs-lookup"><span data-stu-id="f322f-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="f322f-147">При этом выполняет итерацию по массиву покупок и отображаются имя, цену и количество.</span><span class="sxs-lookup"><span data-stu-id="f322f-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="f322f-148">Обратите внимание, что ссылку «Удалить» и «Создание заказа» кнопка привязаны к функции модели представления.</span><span class="sxs-lookup"><span data-stu-id="f322f-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="f322f-149">**Заказы**</span><span class="sxs-lookup"><span data-stu-id="f322f-149">**Orders**</span></span>

<span data-ttu-id="f322f-150">Ниже приведены привязки для списка заказов.</span><span class="sxs-lookup"><span data-stu-id="f322f-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="f322f-151">Это выполняет итерацию по заказам и показывает идентификационный номер заказа.</span><span class="sxs-lookup"><span data-stu-id="f322f-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="f322f-152">События щелчка по ссылке, привязан к `getDetails` функции.</span><span class="sxs-lookup"><span data-stu-id="f322f-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="f322f-153">**Сведения о заказе**</span><span class="sxs-lookup"><span data-stu-id="f322f-153">**Order Details**</span></span>

<span data-ttu-id="f322f-154">Ниже приведены привязки для сведений о заказе.</span><span class="sxs-lookup"><span data-stu-id="f322f-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="f322f-155">Перебор элементов в том порядке оно отображает продукта, цену и quanity.</span><span class="sxs-lookup"><span data-stu-id="f322f-155">This iterates over the items in the order and displays the product, price, and quanity.</span></span> <span data-ttu-id="f322f-156">Окружающей div виден только в том случае, если сведения о массив содержит один или несколько элементов.</span><span class="sxs-lookup"><span data-stu-id="f322f-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="f322f-157">Заключение</span><span class="sxs-lookup"><span data-stu-id="f322f-157">Conclusion</span></span>

<span data-ttu-id="f322f-158">В этом учебнике вы создали приложение, использующее Entity Framework для взаимодействия с базой данных и веб-API ASP.NET для предоставления интерфейса общедоступный поверх уровня данных.</span><span class="sxs-lookup"><span data-stu-id="f322f-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="f322f-159">Мы используем ASP.NET MVC 4 для отрисовки HTML-страниц и Knockout.js плюс jQuery для обеспечения динамического взаимодействия без перезагрузки страницы.</span><span class="sxs-lookup"><span data-stu-id="f322f-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="f322f-160">Дополнительные ресурсы:</span><span class="sxs-lookup"><span data-stu-id="f322f-160">Additional resources:</span></span>

- [<span data-ttu-id="f322f-161">Карта содержимого для доступа к данных ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f322f-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="f322f-162">Центр разработчиков Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f322f-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="f322f-163">Назад</span><span class="sxs-lookup"><span data-stu-id="f322f-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
