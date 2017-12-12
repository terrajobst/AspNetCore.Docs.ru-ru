---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: "Часть 4: Листинг продукты | Документы Microsoft"
author: JoeStagner
description: "Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Часть 4 содержит список продуктов с контракту GridView..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 420cdbcc002bcbfff619d399a7a374009999d754
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="part-4-listing-products"></a><span data-ttu-id="287ee-104">Часть 4: Список продуктов</span><span class="sxs-lookup"><span data-stu-id="287ee-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="287ee-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="287ee-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="287ee-106">Tailspin Spyworks показано, как чрезвычайно простой можно создавать мощные и масштабируемые приложения для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="287ee-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="287ee-107">Он показывает как использовать новые возможности в ASP.NET 4 для создания Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="287ee-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="287ee-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="287ee-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="287ee-109">Часть 4 описывает список продуктов с помощью элемента управления GridView.</span><span class="sxs-lookup"><span data-stu-id="287ee-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a><span data-ttu-id="287ee-110">Список продуктов с помощью элемента управления GridView</span><span class="sxs-lookup"><span data-stu-id="287ee-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="287ee-111">Давайте начнем реализации нашу страницу ProductsList.aspx «Щелкните правой кнопкой мыши» на наших решений и выбрав пункт «Добавить» и «Новый элемент».</span><span class="sxs-lookup"><span data-stu-id="287ee-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="287ee-112">Выберите «Веб-формы с помощью главного страница» и введите имя страницы ProductsList.aspx».</span><span class="sxs-lookup"><span data-stu-id="287ee-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="287ee-113">Нажмите кнопку «Добавить».</span><span class="sxs-lookup"><span data-stu-id="287ee-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="287ee-114">Затем выберите папку «Стили», где мы разместили страницу Site.Master и выберите его из окна «Содержимое папки».</span><span class="sxs-lookup"><span data-stu-id="287ee-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="287ee-115">Нажмите кнопку «ОК», чтобы создать на странице.</span><span class="sxs-lookup"><span data-stu-id="287ee-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="287ee-116">Заполнение нашей базы данных с данными о продуктах, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="287ee-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="287ee-117">После создания нашу страницу снова мы будем использовать источник данных сущности, доступ к данным этого продукта, но в данном экземпляре, необходимо выбрать сущности «продукт», и нам необходимо ограничить элементы, которые возвращаются только те для выбранной категории.</span><span class="sxs-lookup"><span data-stu-id="287ee-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="287ee-118">Для этого мы расскажем EntityDataSource, чтобы автоматически создавать предложение WHERE, и мы будем указывать WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="287ee-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="287ee-119">Как вы помните, что когда мы создавали пунктов меню в нашем» меню категории продукта» мы динамически построенные по ссылке, добавив в строку запроса для каждого канала CatagoryID.</span><span class="sxs-lookup"><span data-stu-id="287ee-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="287ee-120">Вы будете уведомлены об источнике данных сущности для наследования параметров WHERE этого параметра строки запроса.</span><span class="sxs-lookup"><span data-stu-id="287ee-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="287ee-121">Далее мы настроим элемент управления ListView для отображения списка продуктов.</span><span class="sxs-lookup"><span data-stu-id="287ee-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="287ee-122">Для создания оптимальной производительности покупок, мы будем сжатой несколько четкими возможностей каждого отдельного продукта, отображаемое в нашем ListVew.</span><span class="sxs-lookup"><span data-stu-id="287ee-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="287ee-123">Название продукта будет ссылку продукта подробное представление.</span><span class="sxs-lookup"><span data-stu-id="287ee-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="287ee-124">Будут отображены цену продукта.</span><span class="sxs-lookup"><span data-stu-id="287ee-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="287ee-125">Изображение продукта, будут отображены, и мы будет динамически выбирать изображение из каталога изображений в приложении.</span><span class="sxs-lookup"><span data-stu-id="287ee-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="287ee-126">Мы будет включать ссылку, чтобы сразу же добавить конкретного продукта в корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="287ee-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="287ee-127">Ниже показана разметка для наших экземпляр элемента управления ListView.</span><span class="sxs-lookup"><span data-stu-id="287ee-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="287ee-128">Несколько ссылок для каждого продукта, отображаемых строится динамически.</span><span class="sxs-lookup"><span data-stu-id="287ee-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="287ee-129">Кроме того прежде чем мы тестируем собственные новой страницы необходимо создайте структуру папок для продукта изображений каталога следующим образом.</span><span class="sxs-lookup"><span data-stu-id="287ee-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="287ee-130">После изображения наших продуктов доступны можно проверить нашу страницу списка продуктов.</span><span class="sxs-lookup"><span data-stu-id="287ee-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="287ee-131">Выберите домашнюю страницу, на одну из ссылок список категорий.</span><span class="sxs-lookup"><span data-stu-id="287ee-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="287ee-132">Теперь нам нужно реализовать ProductDetials.apsx страницы и функциональность AddToCart.</span><span class="sxs-lookup"><span data-stu-id="287ee-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="287ee-133">Используйте файл -&gt;создать, чтобы создать имя страницы ProductDetails.aspx, с помощью сайта главной страницы, как это делалось ранее.</span><span class="sxs-lookup"><span data-stu-id="287ee-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="287ee-134">Мы снова будет использовать элемент управления EntityDataSource доступа к определенному продукту записи в базе данных и мы будем использовать элемент управления ASP.NET FormView для отображения данных продукта следующим образом.</span><span class="sxs-lookup"><span data-stu-id="287ee-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="287ee-135">Не беспокойтесь, если форматирование немного странно выглядит для вас.</span><span class="sxs-lookup"><span data-stu-id="287ee-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="287ee-136">Показанная выше разметка оставляет место в макете отображения для нескольких компонентов, которые мы будет реализован в более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="287ee-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="287ee-137">Корзине окажутся более сложной логики в приложении.</span><span class="sxs-lookup"><span data-stu-id="287ee-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="287ee-138">Чтобы приступить к работе, используйте файл -&gt;создать, чтобы создать страницу с именем MyShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="287ee-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="287ee-139">Обратите внимание, что мы не следует выбирать имя ShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="287ee-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="287ee-140">Наш база данных содержит таблицу с именем «ShoppingCart».</span><span class="sxs-lookup"><span data-stu-id="287ee-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="287ee-141">При создании модели EDM для каждой таблицы в базе данных был создан класс.</span><span class="sxs-lookup"><span data-stu-id="287ee-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="287ee-142">Таким образом класс сущностей с именем «ShoppingCart» сформировать модель данных сущности.</span><span class="sxs-lookup"><span data-stu-id="287ee-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="287ee-143">Нам удалось изменить модели, что мы может использовать это имя для реализацию покупательской корзины, а также расширять их для наших потребностей, но мы будет выбрать вместо просто выберите имя, которое позволит избежать конфликта.</span><span class="sxs-lookup"><span data-stu-id="287ee-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="287ee-144">Также стоит отметить, что мы создадим простой покупательской корзины и внедрение покупательской корзины логики с отображением корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="287ee-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="287ee-145">Мы также можно реализовать наш никак не связана бизнес-уровне.</span><span class="sxs-lookup"><span data-stu-id="287ee-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="287ee-146">[Назад](tailspin-spyworks-part-3.md)
[Вперед](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="287ee-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
