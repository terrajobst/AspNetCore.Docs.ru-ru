---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Часть 7: Добавление функций | Документация Майкрософт'
author: JoeStagner
description: В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks. Часть 7 добавляет дополнительные функции, например росмотреть учетной записи...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: cada8d9aee649e4f2a5afc1ca2b46863ea458207
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835397"
---
<a name="part-7-adding-features"></a><span data-ttu-id="16619-104">Часть 7: Добавление функций</span><span class="sxs-lookup"><span data-stu-id="16619-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="16619-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="16619-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="16619-106">Tailspin Spyworks демонстрирует, как чрезвычайно прост процесс создания мощных и масштабируемых приложений для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="16619-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="16619-107">Он демонстрирует способы использования новые возможности в ASP.NET 4 для создание Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="16619-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="16619-108">В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="16619-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="16619-109">Часть 7 добавляет дополнительные функции, такие как проверка учетной записи, обзоры продуктов и «популярных элементов» и «также приобретенных» пользовательские элементы управления.</span><span class="sxs-lookup"><span data-stu-id="16619-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="16619-110">Добавление функций</span><span class="sxs-lookup"><span data-stu-id="16619-110">Adding Features</span></span>

<span data-ttu-id="16619-111">Пользователи могут просматривать наш каталог, поместить элементы в его корзине и завершить процесс подсчета стоимости покупок, но существует ряд вспомогательных функций, что будет включено для улучшения наш сайт.</span><span class="sxs-lookup"><span data-stu-id="16619-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="16619-112">Проверка учетной записи (список заказов разместить и просмотреть подробные сведения.)</span><span class="sxs-lookup"><span data-stu-id="16619-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="16619-113">Добавьте в страницу содержимое определенного контекста.</span><span class="sxs-lookup"><span data-stu-id="16619-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="16619-114">Добавьте функцию, которая позволит пользователям просмотр продуктов в каталоге.</span><span class="sxs-lookup"><span data-stu-id="16619-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="16619-115">Создайте пользовательский элемент управления для отображения популярных элементов и место, которые управляют на первой странице.</span><span class="sxs-lookup"><span data-stu-id="16619-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="16619-116">Создание элемента управления пользователя «Также приобрели» и добавьте его к странице сведений о продукте.</span><span class="sxs-lookup"><span data-stu-id="16619-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="16619-117">Добавление контакта страницы.</span><span class="sxs-lookup"><span data-stu-id="16619-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="16619-118">Добавить о странице.</span><span class="sxs-lookup"><span data-stu-id="16619-118">Add an About Page.</span></span>
8. <span data-ttu-id="16619-119">Глобальных ошибок</span><span class="sxs-lookup"><span data-stu-id="16619-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="16619-120">Проверка учетной записи</span><span class="sxs-lookup"><span data-stu-id="16619-120">Account Review</span></span>

<span data-ttu-id="16619-121">В папке «Учетная запись» создайте две страницы ASPX, один именованный OrderList.aspx и другие именованные OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="16619-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="16619-122">OrderList.aspx будет использовать элементы управления GridView и EntityDataSoure так же, как у нас есть ранее.</span><span class="sxs-lookup"><span data-stu-id="16619-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="16619-123">EntityDataSoure Выбор записей из таблицы Orders, отфильтрованные по имени пользователя (см. в разделе WhereParameter) которого задается в переменной сеанса при журнале пользователя.</span><span class="sxs-lookup"><span data-stu-id="16619-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="16619-124">Обратите внимание, эти параметры в поле HyperlinkField элемента управления GridView:</span><span class="sxs-lookup"><span data-stu-id="16619-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="16619-125">Они указывают ссылки в представлении сведений о порядке для каждого продукта, указав в качестве параметра строки запроса к странице OrderDetails.aspx поле OrderID.</span><span class="sxs-lookup"><span data-stu-id="16619-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="16619-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="16619-126">OrderDetails.aspx</span></span>

<span data-ttu-id="16619-127">Мы будем использовать элемент управления EntityDataSource для доступа к заказы и FormView для отображения данных о заказах и другой EntityDataSource, с помощью элемента управления GridView для отображения строк элементов заказа.</span><span class="sxs-lookup"><span data-stu-id="16619-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="16619-128">В файле кода программной части (OrderDetails.aspx.cs) у нас есть два немного биты по обслуживанию.</span><span class="sxs-lookup"><span data-stu-id="16619-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="16619-129">Сначала необходимо убедиться, что OrderDetails всегда получает OrderId.</span><span class="sxs-lookup"><span data-stu-id="16619-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="16619-130">Нам также нужно вычисления и отображения итоговая из элементов строк.</span><span class="sxs-lookup"><span data-stu-id="16619-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="16619-131">На домашней странице</span><span class="sxs-lookup"><span data-stu-id="16619-131">The Home Page</span></span>

<span data-ttu-id="16619-132">Давайте добавим некоторые статического содержимого на страницу Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="16619-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="16619-133">Сначала я создам папку «Содержимое» и в ней папку Images (и я включил изображение, используемое на домашней странице).</span><span class="sxs-lookup"><span data-stu-id="16619-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="16619-134">На место заполнителя нижней части страницы Default.aspx добавьте следующую разметку.</span><span class="sxs-lookup"><span data-stu-id="16619-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="16619-135">Обзоры продуктов</span><span class="sxs-lookup"><span data-stu-id="16619-135">Product Reviews</span></span>

<span data-ttu-id="16619-136">Сначала мы добавим кнопку со ссылкой на форму, который можно использовать для ввода Обзор продукта.</span><span class="sxs-lookup"><span data-stu-id="16619-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="16619-137">Обратите внимание на то, что мы передаем значение поля в строке запроса</span><span class="sxs-lookup"><span data-stu-id="16619-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="16619-138">Далее добавим страницу с именем ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="16619-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="16619-139">Эта страница будет использовать ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="16619-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="16619-140">Если вы еще не сделали, скачав его из [DevExpress](http://devexpress.com/act) и есть рекомендации по настройке в набор средств для работы с Visual Studio здесь [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="16619-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="16619-141">В режиме конструктора перетащите элементы управления и проверяющие элементы управления из области элементов и создание формы, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="16619-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="16619-142">Разметка будет выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="16619-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="16619-143">Теперь, когда мы можем ввести имя проверки, позволяет отобразить отзывы пользователей на странице продукта.</span><span class="sxs-lookup"><span data-stu-id="16619-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="16619-144">Добавьте эту разметку страницы ProductDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="16619-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="16619-145">Под управлением Теперь приложение, последовательно выбрав пункты продукта показывает сведения о продукте, включая отзывы клиентов.</span><span class="sxs-lookup"><span data-stu-id="16619-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="16619-146">Популярных элементов управления (Создание пользовательских элементов управления)</span><span class="sxs-lookup"><span data-stu-id="16619-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="16619-147">Для увеличения продаж на веб-сайт мы добавим несколько возможностей «непристойные sell» популярные или связанных продуктов.</span><span class="sxs-lookup"><span data-stu-id="16619-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="16619-148">Первый из этих функций будет представлять собой список наиболее популярных продуктов в наш каталог продукции.</span><span class="sxs-lookup"><span data-stu-id="16619-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="16619-149">Мы создадим «Пользовательский элемент управления» для отображения наиболее продаваемых элементы на домашней странице приложения.</span><span class="sxs-lookup"><span data-stu-id="16619-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="16619-150">Так как это будет элемент управления, мы используем его на любой странице, просто перетащив элемент управления в конструкторе Visual Studio на любой странице, нам хотелось.</span><span class="sxs-lookup"><span data-stu-id="16619-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="16619-151">В обозревателе решений Visual Studio щелкните правой кнопкой мыши имя решения и создайте новый каталог с именем «Элементов управления».</span><span class="sxs-lookup"><span data-stu-id="16619-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="16619-152">Хотя нет необходимости сделать это, мы поможет сохранить наш проект, упорядоченные по Создание всех наших пользовательских элементов управления в каталоге «Элементов управления».</span><span class="sxs-lookup"><span data-stu-id="16619-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="16619-153">Элементы управления папку правой кнопкой мыши и выберите «Новый элемент»:</span><span class="sxs-lookup"><span data-stu-id="16619-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="16619-154">Укажите имя для наших управления «PopularItems».</span><span class="sxs-lookup"><span data-stu-id="16619-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="16619-155">Обратите внимание, что расширение файла для пользовательских элементов управления .ascx не .aspx.</span><span class="sxs-lookup"><span data-stu-id="16619-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="16619-156">Наши распространенных пользовательских элементов управления определяются следующим образом.</span><span class="sxs-lookup"><span data-stu-id="16619-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="16619-157">Здесь мы используем метод, который мы не использовали еще в этом приложении.</span><span class="sxs-lookup"><span data-stu-id="16619-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="16619-158">Мы используем элемент управления repeater и вместо того чтобы использовать элемент управления источником данных мы привязываем элемент управления Repeater к результатам запроса LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="16619-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="16619-159">В коде программной части наш элемент управления этого следующим образом.</span><span class="sxs-lookup"><span data-stu-id="16619-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="16619-160">Обратите внимание, это важный этап в верхней части разметки наш элемент управления.</span><span class="sxs-lookup"><span data-stu-id="16619-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="16619-161">Так как не будет изменять самыми популярными материалами по принципу минуты мы можем добавить директиву болью для повышения производительности приложения.</span><span class="sxs-lookup"><span data-stu-id="16619-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="16619-162">Эта директива вызовет код элементов управления для должны выполняться по истечении срока действия кэшированных выходных данных элемента управления.</span><span class="sxs-lookup"><span data-stu-id="16619-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="16619-163">В противном случае будет использоваться кэшированная версия выходные данные элемента управления.</span><span class="sxs-lookup"><span data-stu-id="16619-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="16619-164">Теперь нам нужно всего лишь включить наш новый элемент управления в страницу Default.aspc.</span><span class="sxs-lookup"><span data-stu-id="16619-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="16619-165">Используйте перетаскивание для размещения экземпляра элемента управления в столбце откройте нашу форму по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="16619-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="16619-166">Теперь при выполнении приложения на домашней странице отображаются наиболее популярных элементов.</span><span class="sxs-lookup"><span data-stu-id="16619-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="16619-167">«Также приобрели» управления (элементы управления пользователя с параметрами)</span><span class="sxs-lookup"><span data-stu-id="16619-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="16619-168">Второй пользовательский элемент управления, который мы создадим займет непристойные продаж на следующий уровень, добавив специфичность контекста.</span><span class="sxs-lookup"><span data-stu-id="16619-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="16619-169">Логика для вычисления верхних элементов «Также приобрели» является нетривиальным.</span><span class="sxs-lookup"><span data-stu-id="16619-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="16619-170">Наш элемент управления «Также приобрели» будет выбора OrderDetails записей (ранее приобретенной) для текущего выбранного ProductID и захватите OrderIDs для каждого заказа уникальный, которую можно найти.</span><span class="sxs-lookup"><span data-stu-id="16619-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="16619-171">Затем мы выберем al продукты из всех этих заказов и суммы, количества приобретенных.</span><span class="sxs-lookup"><span data-stu-id="16619-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="16619-172">Мы продукты сортируются по количество, сумма которых составляет и отобразить первые пять элементов.</span><span class="sxs-lookup"><span data-stu-id="16619-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="16619-173">Учитывая сложность этой логики, мы реализуем этот алгоритм как хранимую процедуру.</span><span class="sxs-lookup"><span data-stu-id="16619-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="16619-174">Ниже приведен код T-SQL для хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="16619-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="16619-175">Обратите внимание, эта хранимая процедура (SelectPurchasedWithProducts) существовали в базе данных, когда мы включили в наше приложение, и при создании модели EDM, мы указали в дополнение к таблицы и представления, которые нам нужно модели EDM следует включить эту хранимую процедуру.</span><span class="sxs-lookup"><span data-stu-id="16619-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="16619-176">Для доступа к хранимую процедуру из модели EDM, нам нужно импортировать функцию.</span><span class="sxs-lookup"><span data-stu-id="16619-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="16619-177">Дважды щелкните на модели EDM в обозревателе решений, чтобы открыть его в конструкторе и откройте браузер моделей, а затем щелкните правой кнопкой мыши в конструкторе и выберите «Добавить импорт функции».</span><span class="sxs-lookup"><span data-stu-id="16619-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="16619-178">Таким образом будет открыть это диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="16619-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="16619-179">Заполните поля, как показано выше, выбрав «SelectPurchasedWithProducts» и используйте имя процедуры в качестве имени наших импортированной функции.</span><span class="sxs-lookup"><span data-stu-id="16619-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="16619-180">Нажмите кнопку «ОК».</span><span class="sxs-lookup"><span data-stu-id="16619-180">Click "Ok".</span></span>

<span data-ttu-id="16619-181">Этого, мы просто могут программировать для хранимой процедуры, как мы может любой другой элемент в модели.</span><span class="sxs-lookup"><span data-stu-id="16619-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="16619-182">Таким образом в папке «Элементы управления» создайте новый пользовательский элемент управления с именем AlsoPurchased.ascx.</span><span class="sxs-lookup"><span data-stu-id="16619-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="16619-183">Разметка для этого элемента управления покажется очень PopularItems элемента управления.</span><span class="sxs-lookup"><span data-stu-id="16619-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="16619-184">Важное отличие состоит в том, не являются кэширования выходных данных, поскольку для отображения элемента будут отличаться по продукту.</span><span class="sxs-lookup"><span data-stu-id="16619-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="16619-185">Значение поля будет «свойство» к элементу управления.</span><span class="sxs-lookup"><span data-stu-id="16619-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="16619-186">В обработчике события PreRender элемента управления мы eed для выполнения трех задач.</span><span class="sxs-lookup"><span data-stu-id="16619-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="16619-187">Убедитесь, что значение ProductID.</span><span class="sxs-lookup"><span data-stu-id="16619-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="16619-188">См. в статье, есть ли все продукты, которые были приобретены с текущей.</span><span class="sxs-lookup"><span data-stu-id="16619-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="16619-189">Выводить некоторые элементы, как определено в #2.</span><span class="sxs-lookup"><span data-stu-id="16619-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="16619-190">Обратите внимание на то, насколько это просто вызов хранимой процедуры через модель.</span><span class="sxs-lookup"><span data-stu-id="16619-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="16619-191">Определив, существует «также приобретаются» мы просто связать элементу управления repeater к результатам, возвращаемым запросом.</span><span class="sxs-lookup"><span data-stu-id="16619-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="16619-192">Если не все элементы «также приобрели» мы просто отобразим других популярных элементов из наших каталога.</span><span class="sxs-lookup"><span data-stu-id="16619-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="16619-193">Чтобы просмотреть элементы «Также приобрели», откройте страницу ProductDetails.aspx и перетащите его AlsoPurchased из обозревателя решений, чтобы он отображался в данном положении в разметке.</span><span class="sxs-lookup"><span data-stu-id="16619-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="16619-194">Таким образом будет создать ссылку на элемент управления в верхней части страницы ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="16619-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="16619-195">Так как пользовательский элемент управления AlsoPurchased требуется номер ProductId, мы определим свойство ProductID наш элемент управления, используя оператор Eval от текущего элемента модели данных страницы.</span><span class="sxs-lookup"><span data-stu-id="16619-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="16619-196">Когда мы построить и запустить сейчас и просмотреть в продукт мы см. в разделе «Также приобрели» элементов.</span><span class="sxs-lookup"><span data-stu-id="16619-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="16619-197">[Назад](tailspin-spyworks-part-6.md)
> [Вперед](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="16619-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
