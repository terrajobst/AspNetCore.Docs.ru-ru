---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Часть 7: Добавление функций | Документы Microsoft'
author: JoeStagner
description: Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Часть 7 добавляет дополнительные компоненты, например росмотреть учетной записи...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="part-7-adding-features"></a><span data-ttu-id="7d496-104">Часть 7: Добавление функции</span><span class="sxs-lookup"><span data-stu-id="7d496-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="7d496-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="7d496-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="7d496-106">Tailspin Spyworks показано, как чрезвычайно простой можно создавать мощные и масштабируемые приложения для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="7d496-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="7d496-107">Он показывает как использовать новые возможности в ASP.NET 4 для создания Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="7d496-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="7d496-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="7d496-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="7d496-109">Часть 7 добавляет дополнительные функции, такие как проверка учетной записи, обзоров продуктов и «популярных элементов» и «также приобретенных» пользовательские элементы управления.</span><span class="sxs-lookup"><span data-stu-id="7d496-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="7d496-110">Добавление компонентов</span><span class="sxs-lookup"><span data-stu-id="7d496-110">Adding Features</span></span>

<span data-ttu-id="7d496-111">Пользователи могут просматривать наш каталог, поместите элементы в его корзине и завершить процесс извлечения, но существует ряд вспомогательных функций, чтобы улучшить наш сайт будет включено.</span><span class="sxs-lookup"><span data-stu-id="7d496-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="7d496-112">Проверка учетной записи (список заказов разместить и просмотреть сведения.)</span><span class="sxs-lookup"><span data-stu-id="7d496-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="7d496-113">Добавьте содержимое определенного контекста к началу страницы.</span><span class="sxs-lookup"><span data-stu-id="7d496-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="7d496-114">Добавьте компонент, чтобы позволить пользователям просмотр продуктов в каталоге.</span><span class="sxs-lookup"><span data-stu-id="7d496-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="7d496-115">Создайте пользовательский элемент управления для отображения элементов популярных и месте, управляющие на первой странице.</span><span class="sxs-lookup"><span data-stu-id="7d496-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="7d496-116">Создание элемента управления пользователя «Также приобретенных» и добавьте его к странице сведений о продукте.</span><span class="sxs-lookup"><span data-stu-id="7d496-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="7d496-117">Добавить контакт страницы.</span><span class="sxs-lookup"><span data-stu-id="7d496-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="7d496-118">Добавить о странице.</span><span class="sxs-lookup"><span data-stu-id="7d496-118">Add an About Page.</span></span>
8. <span data-ttu-id="7d496-119">Глобальные ошибки</span><span class="sxs-lookup"><span data-stu-id="7d496-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="7d496-120">Проверка учетной записи</span><span class="sxs-lookup"><span data-stu-id="7d496-120">Account Review</span></span>

<span data-ttu-id="7d496-121">В папке «Учетная запись» создайте две страницы ASPX один именованный OrderList.aspx и именованные OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="7d496-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="7d496-122">OrderList.aspx будет использовать элементы управления GridView и EntityDataSoure так же, как у нас есть ранее.</span><span class="sxs-lookup"><span data-stu-id="7d496-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="7d496-123">EntityDataSoure Выбор записей из таблицы заказов, отфильтрованные по имени пользователя (см. WhereParameter) которого задается в переменной сеанса при входе этого пользователя.</span><span class="sxs-lookup"><span data-stu-id="7d496-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="7d496-124">Обратите внимание, эти параметры в HyperlinkField элемента управления GridView:</span><span class="sxs-lookup"><span data-stu-id="7d496-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="7d496-125">Они указывают ссылку, чтобы Просмотр подробностей заказа для каждого продукта, указывая в качестве параметра строки запроса к странице OrderDetails.aspx поле OrderID.</span><span class="sxs-lookup"><span data-stu-id="7d496-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="7d496-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="7d496-126">OrderDetails.aspx</span></span>

<span data-ttu-id="7d496-127">Мы будем использовать элемент управления EntityDataSource для доступа к заказов и FormView для отображения данных о заказе и другой EntityDataSource, с помощью GridView для отображения строк элементов заказа.</span><span class="sxs-lookup"><span data-stu-id="7d496-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="7d496-128">В файле кода программной части (OrderDetails.aspx.cs) у нас есть два мало биты обслуживания.</span><span class="sxs-lookup"><span data-stu-id="7d496-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="7d496-129">Сначала необходимо убедиться, что OrderDetails всегда получает OrderId.</span><span class="sxs-lookup"><span data-stu-id="7d496-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="7d496-130">Нам также нужно вычислить и отобразить общее из элементов строк заказа.</span><span class="sxs-lookup"><span data-stu-id="7d496-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="7d496-131">Домашняя страница</span><span class="sxs-lookup"><span data-stu-id="7d496-131">The Home Page</span></span>

<span data-ttu-id="7d496-132">Давайте добавим некоторые статическое содержимое на страницу Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="7d496-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="7d496-133">Сначала я создам папкой «Содержимое» и в ней папку Images (и будет включать изображения для использования на домашней странице).</span><span class="sxs-lookup"><span data-stu-id="7d496-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="7d496-134">В нижней заполнитель страницу Default.aspx добавьте следующую разметку.</span><span class="sxs-lookup"><span data-stu-id="7d496-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="7d496-135">Обзоров продуктов</span><span class="sxs-lookup"><span data-stu-id="7d496-135">Product Reviews</span></span>

<span data-ttu-id="7d496-136">Сначала мы добавим кнопки со ссылкой на форму, можно использовать для ввода Обзор продукта.</span><span class="sxs-lookup"><span data-stu-id="7d496-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="7d496-137">Обратите внимание, что мы передаем значение поля в строке запроса</span><span class="sxs-lookup"><span data-stu-id="7d496-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="7d496-138">Далее добавим страницу с именем ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="7d496-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="7d496-139">Эта страница будет использовать набор элементов управления ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="7d496-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="7d496-140">Если вы еще не сделали, загрузите ее с [DevExpress](http://devexpress.com/act) и рекомендации по настройке в набор инструментов для использования с Visual Studio здесь [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="7d496-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="7d496-141">В режиме конструктора перетащите элементы управления и проверяющие элементы управления из области элементов и создания формы, аналогичный приведенному ниже.</span><span class="sxs-lookup"><span data-stu-id="7d496-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="7d496-142">Разметка будет выглядеть примерно следующим образом.</span><span class="sxs-lookup"><span data-stu-id="7d496-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="7d496-143">Теперь, когда мы можем ввести обзоры, позволяет отображать эти проверки на странице продукта.</span><span class="sxs-lookup"><span data-stu-id="7d496-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="7d496-144">Добавьте этот разметку страницы ProductDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="7d496-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="7d496-145">Теперь приложение под управлением и переход к продукту показывает сведения о продукте, включая отзывы клиентов.</span><span class="sxs-lookup"><span data-stu-id="7d496-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="7d496-146">Популярные элементов управления (создания пользовательских элементов управления)</span><span class="sxs-lookup"><span data-stu-id="7d496-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="7d496-147">Чтобы увеличить продажи на веб-сайте мы добавим ваши возможности популярных или связанные продукты «непристойные продаж».</span><span class="sxs-lookup"><span data-stu-id="7d496-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="7d496-148">Первый из этих функций будет список более популярных продуктов в наш каталог продукции.</span><span class="sxs-lookup"><span data-stu-id="7d496-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="7d496-149">Мы создадим «Пользовательский элемент управления» для отображения наиболее продаваемых товаров на домашней странице приложения.</span><span class="sxs-lookup"><span data-stu-id="7d496-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="7d496-150">Поскольку это будет элемент управления, можно использовать на любой странице, просто перетащив элемент управления в конструкторе Visual Studio на любой странице, который мы.</span><span class="sxs-lookup"><span data-stu-id="7d496-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="7d496-151">В обозревателе решений Visual Studio щелкните правой кнопкой мыши имя решения и создайте новый каталог с именем «Элементы управления».</span><span class="sxs-lookup"><span data-stu-id="7d496-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="7d496-152">Не является необходимым, мы поможет сохранить наш проект по созданию всех наших пользовательских элементов управления в каталоге «Элементам управления».</span><span class="sxs-lookup"><span data-stu-id="7d496-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="7d496-153">Правой кнопкой мыши папку элементов управления и выберите «Новый элемент».</span><span class="sxs-lookup"><span data-stu-id="7d496-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="7d496-154">Укажите имя для наших управления «PopularItems».</span><span class="sxs-lookup"><span data-stu-id="7d496-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="7d496-155">Обратите внимание, что расширение файла для пользовательских элементов управления .ascx не .aspx.</span><span class="sxs-lookup"><span data-stu-id="7d496-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="7d496-156">Наш распространенных пользовательских элементов управления определяются следующим образом.</span><span class="sxs-lookup"><span data-stu-id="7d496-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="7d496-157">Здесь мы используем метод, еще не используемых в этом приложении.</span><span class="sxs-lookup"><span data-stu-id="7d496-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="7d496-158">Мы используем элементе управления повторителем и вместо использования элемента управления источником данных мы выполняем привязку элементе управления повторителем к результатам запроса LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="7d496-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="7d496-159">В коде можем контролировать нам сделать следующим образом.</span><span class="sxs-lookup"><span data-stu-id="7d496-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="7d496-160">Обратите внимание, эта важные строка в верхней части разметки можем контролировать.</span><span class="sxs-lookup"><span data-stu-id="7d496-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="7d496-161">Поскольку наиболее популярных элементы не будут изменены на основе минуты к минуте мы можем добавить директиву болью для повышения производительности приложения.</span><span class="sxs-lookup"><span data-stu-id="7d496-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="7d496-162">Эта директива вызовет код элементов управления, чтобы выполнить только после истечения срока действия кэшированного вывода элемента управления.</span><span class="sxs-lookup"><span data-stu-id="7d496-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="7d496-163">В противном случае будет использоваться кэшированная версия вывода элемента управления.</span><span class="sxs-lookup"><span data-stu-id="7d496-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="7d496-164">Теперь все, что нужно сделать — добавить наш новый элемент управления в нашу страницу Default.aspc.</span><span class="sxs-lookup"><span data-stu-id="7d496-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="7d496-165">Используйте перетаскивание для размещения экземпляра элемента управления в столбце откройте нашу форму по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7d496-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="7d496-166">Теперь когда мы запускаем наше приложение на домашней странице отображаются наиболее популярных элементы.</span><span class="sxs-lookup"><span data-stu-id="7d496-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="7d496-167">«Также Куплено» управляют (пользовательские элементы управления с параметрами)</span><span class="sxs-lookup"><span data-stu-id="7d496-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="7d496-168">Второй пользовательский элемент управления, который мы создадим займет непристойные продаж на следующий уровень, добавив специфичность контекста.</span><span class="sxs-lookup"><span data-stu-id="7d496-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="7d496-169">Логика для вычисления с основными элементами «Также приобретенных» является нетривиальной.</span><span class="sxs-lookup"><span data-stu-id="7d496-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="7d496-170">Выбирает OrderDetails записей (ранее уже приобрел) для текущего выбранного ProductID и захватите OrderIDs для каждого заказа уникальными, находящийся наш элемент управления «Также приобрести».</span><span class="sxs-lookup"><span data-stu-id="7d496-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="7d496-171">Затем мы выберем al продукты из этих заказов и количества приобретенных sum.</span><span class="sxs-lookup"><span data-stu-id="7d496-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="7d496-172">Мы отсортировать продукты по количество, сумма которых составляет и отображения пять элементов.</span><span class="sxs-lookup"><span data-stu-id="7d496-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="7d496-173">Учитывая сложность этой логики, мы будем реализовывать этот алгоритм как хранимую процедуру.</span><span class="sxs-lookup"><span data-stu-id="7d496-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="7d496-174">Ниже приведен код T-SQL для хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="7d496-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="7d496-175">Обратите внимание, что эта хранимая процедура (SelectPurchasedWithProducts) существовали в базе данных мы включили в приложении, и при создании модели EDM, указанный в дополнение к таблиц и представлений, которые нужны, модели EDM следует включить эту хранимую процедуру.</span><span class="sxs-lookup"><span data-stu-id="7d496-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="7d496-176">Для доступа к хранимой процедуре из нам нужно импортировать функция модели EDM.</span><span class="sxs-lookup"><span data-stu-id="7d496-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="7d496-177">Дважды щелкните модель данных сущности в обозревателе решений, чтобы открыть его в конструкторе и откройте обозреватель моделей, затем щелкните правой кнопкой мыши в конструкторе и выберите команду Добавить функции «импорт».</span><span class="sxs-lookup"><span data-stu-id="7d496-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="7d496-178">Таким образом будет открыть это диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="7d496-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="7d496-179">Заполните поля, как показано выше, при выборе «SelectPurchasedWithProducts» и использовать имя процедуры в качестве имени нашей импортированной функции.</span><span class="sxs-lookup"><span data-stu-id="7d496-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="7d496-180">Нажмите кнопку «ОК».</span><span class="sxs-lookup"><span data-stu-id="7d496-180">Click "Ok".</span></span>

<span data-ttu-id="7d496-181">Этого, мы просто написать программный код хранимой процедуры, как мы может любой элемент в модели.</span><span class="sxs-lookup"><span data-stu-id="7d496-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="7d496-182">Таким образом в нашем папке «Элементы управления» создайте новый пользовательский элемент управления с именем AlsoPurchased.ascx.</span><span class="sxs-lookup"><span data-stu-id="7d496-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="7d496-183">Разметку для этого элемента управления будет очень знакомыми к элементу управления PopularItems.</span><span class="sxs-lookup"><span data-stu-id="7d496-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="7d496-184">Заметное отличие состоит в том, не кэшируется выходных данных так, как должен быть подготовлен к просмотру элемента будут отличаться по продукту.</span><span class="sxs-lookup"><span data-stu-id="7d496-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="7d496-185">Значение поля будет «свойство» к элементу управления.</span><span class="sxs-lookup"><span data-stu-id="7d496-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="7d496-186">В обработчике события PreRender элемента управления мы eed для выполнения трех задач.</span><span class="sxs-lookup"><span data-stu-id="7d496-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="7d496-187">Убедитесь в том, что значение ProductID.</span><span class="sxs-lookup"><span data-stu-id="7d496-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="7d496-188">См. Если есть какие-либо продукты, приобретенные с текущей строкой.</span><span class="sxs-lookup"><span data-stu-id="7d496-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="7d496-189">Выводить некоторые элементы, как определено в #2.</span><span class="sxs-lookup"><span data-stu-id="7d496-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="7d496-190">Обратите внимание на то, как просто можно вызвать хранимую процедуру через модель.</span><span class="sxs-lookup"><span data-stu-id="7d496-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="7d496-191">Определив, что» также приобретенных» мы просто привязать повторителя результатов, возвращенных запросом.</span><span class="sxs-lookup"><span data-stu-id="7d496-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="7d496-192">Если не все элементы «также приобретенных» будет просто отобразить других популярных элементов из наших каталога.</span><span class="sxs-lookup"><span data-stu-id="7d496-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="7d496-193">Чтобы просмотреть элементы «Также приобрести», откройте страницу ProductDetails.aspx и перетащите элемент управления AlsoPurchased из обозревателя решений, чтобы он отображался в этом месте в разметке.</span><span class="sxs-lookup"><span data-stu-id="7d496-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="7d496-194">Таким образом будет создать ссылку на элемент управления в верхней части страницы ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="7d496-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="7d496-195">Так как пользовательский элемент управления AlsoPurchased требуется номер ProductId мы установим свойство ProductID наш элемент управления с помощью инструкции Eval для текущего элемента модели данных страницы.</span><span class="sxs-lookup"><span data-stu-id="7d496-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="7d496-196">Когда мы сборки и выполнения теперь и перейдите к продукту мы отображены элементы «Также приобрести».</span><span class="sxs-lookup"><span data-stu-id="7d496-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="7d496-197">[Назад](tailspin-spyworks-part-6.md)
> [Вперед](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="7d496-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
