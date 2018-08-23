---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Часть 4: Список продуктов | Документация Майкрософт'
author: JoeStagner
description: В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks. Часть 4 охватывает список продуктов с GridView контракту...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: ca7eccd684473d9a1ec4a8adfd8690b291fe702f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836692"
---
<a name="part-4-listing-products"></a><span data-ttu-id="e8e5b-104">Часть 4: Список продуктов</span><span class="sxs-lookup"><span data-stu-id="e8e5b-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="e8e5b-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e8e5b-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e8e5b-106">Tailspin Spyworks демонстрирует, как чрезвычайно прост процесс создания мощных и масштабируемых приложений для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e8e5b-107">Он демонстрирует способы использования новые возможности в ASP.NET 4 для создание Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e8e5b-108">В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e8e5b-109">Часть 4 охватывает список продуктов с помощью элемента управления GridView.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="e8e5b-110">Список продуктов с помощью элемента управления GridView</span><span class="sxs-lookup"><span data-stu-id="e8e5b-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="e8e5b-111">Давайте начнем реализация наших ProductsList.aspx страницы «Щелкните правой кнопкой» на наше решение и выбрав пункт «Добавить» и «Новый элемент».</span><span class="sxs-lookup"><span data-stu-id="e8e5b-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="e8e5b-112">Выберите «Веб-формы с помощью главного страница» и введите имя страницы ProductsList.aspx».</span><span class="sxs-lookup"><span data-stu-id="e8e5b-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="e8e5b-113">Нажмите кнопку «Добавить».</span><span class="sxs-lookup"><span data-stu-id="e8e5b-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="e8e5b-114">Затем выберите папку «Стили», где мы разместили на страницу Site.Master и выберите его из окна «Содержимое папки».</span><span class="sxs-lookup"><span data-stu-id="e8e5b-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="e8e5b-115">Нажмите кнопку «ОК», чтобы создать страницу.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="e8e5b-116">Наши базы данных заполняется данные о продуктах, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="e8e5b-117">После создания нашу страницу еще раз мы будем использовать источник данных сущности для доступа к этим данным продукта, но в данном экземпляре, нам необходимо выбрать сущности «продукт», и нам нужно ограничить элементы, которые возвращаются только теми, для выбранной категории.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="e8e5b-118">Для выполнения этой задачи мы скажем EntityDataSource для автоматического создания в предложении WHERE мы также укажем WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="e8e5b-119">Как вы помните, что когда мы создавали пунктов меню в нашей «меню категории продукта» мы динамическим образом строится ссылку, добавив CatagoryID в строку запроса для каждого канала.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="e8e5b-120">Вы будете уведомлены об источнике данных сущности для параметра WHERE являются производными этого параметра строки запроса.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="e8e5b-121">Затем мы настроим элемент управления ListView для отображения списка продуктов.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="e8e5b-122">Чтобы создать оптимальный способ осуществления покупок, мы будем сжатой несколько функций краткими каждого отдельного продукта, отображаемый в наших ListVew.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="e8e5b-123">Имя продукта будет ссылка на подробные сведения о продукте.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="e8e5b-124">Отображается цена продукта.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="e8e5b-125">Изображение продукта будет выведен список динамически выберем изображение из каталога каталог образов в нашем приложении.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="e8e5b-126">Мы добавим ссылку, чтобы сразу же добавить конкретного продукта в корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="e8e5b-127">Далее приведена разметка для наш экземпляр элемента управления ListView.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="e8e5b-128">Мы создаем динамически несколько ссылок для каждого отображаемого продукта.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="e8e5b-129">Кроме того прежде чем мы протестировать собственную новую страницу необходимо создать структуру каталогов для продукта изображений каталога следующим образом.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="e8e5b-130">После доступны наши образы продукта можно проверить страницы со списком наших продуктов.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="e8e5b-131">На домашней странице веб-узла щелкните одну из ссылок списка категорий.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="e8e5b-132">Теперь нам нужно реализовать ProductDetials.apsx страницу и функцию AddToCart.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="e8e5b-133">Используйте файл -&gt;создать, чтобы создать имя страницы ProductDetails.aspx, с помощью главной страницы узла, как это было сделано ранее.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="e8e5b-134">Мы будем использовать элемент управления EntityDataSource снова получить доступ к определенной записи продукта в базе данных, и мы будем использовать элемент управления ASP.NET FormView для отображения данных продукта следующим образом.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="e8e5b-135">Не беспокойтесь, если форматирование выглядит немного странно для вас.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="e8e5b-136">Приведенный выше код разметки оставляет место в формат вывода для несколько возможностей, которые мы реализуем позже.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="e8e5b-137">Корзина для покупок будет представлять более сложная логика в приложении.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="e8e5b-138">Чтобы приступить к работе, используйте файл -&gt;создать, чтобы создать страница с именем MyShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="e8e5b-139">Обратите внимание на то, что мы не следует выбирать имя ShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="e8e5b-140">Наши база данных содержит таблицу с именем «ShoppingCart».</span><span class="sxs-lookup"><span data-stu-id="e8e5b-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="e8e5b-141">При создании модели EDM для каждой таблицы в базе данных был создан класс.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="e8e5b-142">Таким образом в модели EDM создается класс сущностей с именем «ShoppingCart».</span><span class="sxs-lookup"><span data-stu-id="e8e5b-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="e8e5b-143">Нам удалось изменить модель так, мы может использовать это имя для нашей корзину для покупок реализации или расширить его для наших потребностей, но мы будет выбрать вместо этого просто выберите имя, которое позволит избежать конфликта.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="e8e5b-144">Также стоит отметить, что мы создадим простой корзины для покупок и внедрение корзины для покупок логику с отображением корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="e8e5b-145">Мы также можно реализовать Корзина никак не связано бизнес-уровне.</span><span class="sxs-lookup"><span data-stu-id="e8e5b-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e8e5b-146">[Назад](tailspin-spyworks-part-3.md)
> [Вперед](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="e8e5b-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
