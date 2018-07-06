---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Часть 5: Бизнес-логики | Документация Майкрософт'
author: JoeStagner
description: В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks. Часть 5 добавляет некоторые бизнес-логики.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: fdb73c01d7b6076bc292c640376aa35cc5f52ea6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827052"
---
<a name="part-5-business-logic"></a><span data-ttu-id="5cf6f-104">Часть 5: Бизнес-логики</span><span class="sxs-lookup"><span data-stu-id="5cf6f-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="5cf6f-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="5cf6f-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="5cf6f-106">Tailspin Spyworks демонстрирует, как чрезвычайно прост процесс создания мощных и масштабируемых приложений для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="5cf6f-107">Он демонстрирует способы использования новые возможности в ASP.NET 4 для создание Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="5cf6f-108">В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="5cf6f-109">Часть 5 добавляет некоторые бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="5cf6f-110">Добавление некоторых бизнес-логики</span><span class="sxs-lookup"><span data-stu-id="5cf6f-110">Adding Some Business Logic</span></span>

<span data-ttu-id="5cf6f-111">Мы хотим опыт покупок были доступны при посещении веб-узла.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="5cf6f-112">Посетители смогут просматривать и добавлять товары в корзину покупок, даже если они не зарегистрированы или не вошел в систему.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="5cf6f-113">Когда они готовы для извлечения будет предоставляться возможность проверки подлинности, и если они не еще элементов они будут иметь возможность создать учетную запись.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="5cf6f-114">Это означает, что нам потребуется реализовать логику для преобразования в корзину для покупок из анонимного состояния в состояние «Зарегистрированный пользователь».</span><span class="sxs-lookup"><span data-stu-id="5cf6f-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="5cf6f-115">Давайте создайте каталог с именем «Классы» а затем правой кнопкой мыши папку и создайте новый файл «Class» MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="5cf6f-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="5cf6f-116">Как уже говорилось выше, мы улучшим класс, реализующий MyShoppingCart.aspx страницы, и мы сделаем это с помощью. Конструкция мощные «разделяемый класс» NET.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="5cf6f-117">Созданный вызов для нашего файла MyShoppingCart.aspx.cf выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="5cf6f-118">Обратите внимание на использование ключевого слова «partial».</span><span class="sxs-lookup"><span data-stu-id="5cf6f-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="5cf6f-119">Файл класса, который мы только что созданную выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="5cf6f-120">Мы объединим наши реализации, добавив к этому файлу, а также ключевое слово partial.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="5cf6f-121">Наш новый файл класса теперь выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="5cf6f-122">Первый метод, который мы добавим к нашему классу способ «AddItem».</span><span class="sxs-lookup"><span data-stu-id="5cf6f-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="5cf6f-123">Это метод, который в конечном счете будет вызываться при нажатии пользователем ссылки «Добавить к Art» на страницах список продуктов и сведения о продукте.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="5cf6f-124">Добавьте следующее содержимое с помощью инструкций в верхней части страницы.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="5cf6f-125">И добавьте следующий метод к классу MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="5cf6f-126">Мы используем LINQ to Entities, чтобы увидеть, если элемент уже существует в корзине.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="5cf6f-127">Если таким образом, мы изменим порядок количество товара, в противном случае мы создадим новую запись для выбранного элемента</span><span class="sxs-lookup"><span data-stu-id="5cf6f-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="5cf6f-128">Для вызова этого метода мы реализуем страницу AddToCart.aspx, который не только этот метод класса, но затем отображается текущий Корзина = после добавления элемента.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="5cf6f-129">Щелкните правой кнопкой мыши имя решения в обозревателе решений и добавьте и новую страницу с именем AddToCart.aspx, как было показано ранее.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="5cf6f-130">Для отображения таких проблем низкой акций и т. д, промежуточных результатов в нашей реализации мы использовать эту страницу, страницы будет фактически не визуализации, но вместо вызывать логику «Добавить» и перенаправление.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="5cf6f-131">Для этого мы добавим следующий код на страницу\_событие Load.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="5cf6f-132">Обратите внимание на то, что, получаемых продукта, чтобы добавить в корзину покупок из параметра строки запроса и вызова метода AddItem нашего класса.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="5cf6f-133">При условии, что ошибки не обнаружены управление передается на страницу SHoppingCart.aspx, которая полностью мы реализуем рядом.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="5cf6f-134">Если следует ошибку, мы выдаем исключение.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="5cf6f-135">В настоящее время мы еще не реализована глобальный обработчик ошибок, это исключение может уйти необработанным, наше приложение, но мы будет исправить это чуть позже.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="5cf6f-136">Обратите внимание на использование инструкции (доступно через Debug.Fail() `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="5cf6f-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="5cf6f-137">— Приложение выполняется в отладчике, этот метод отображает подробные сведения о состоянии приложений, а также сообщение об ошибке, мы указываем диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="5cf6f-138">При выполнении в рабочей среде Debug.Fail(), оператор игнорируется.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="5cf6f-139">Можно заметить в коде выше вызов метода в именам класс корзину для покупок «GetShoppingCartId».</span><span class="sxs-lookup"><span data-stu-id="5cf6f-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="5cf6f-140">Добавьте код для реализации метода, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="5cf6f-141">Обратите внимание на то, что мы также добавили извлечения и обновления кнопки и метки, где мы можем отобразить корзины для покупок «итог».</span><span class="sxs-lookup"><span data-stu-id="5cf6f-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="5cf6f-142">Теперь можно добавить элементы в нашей корзину для покупок, но мы не реализовали логику для отображения в корзину, после добавления продукта.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="5cf6f-143">Таким образом на странице MyShoppingCart.aspx мы добавим элемент управления EntityDataSource и элемент управления GridVire следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="5cf6f-144">Вызовите форму в конструкторе, чтобы вы могли дважды нажмите кнопку Обновить корзину и создать обработчик событий click, который указан в объявлении в разметке.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="5cf6f-145">Мы реализуем сведения позже, но это сообщите нам построения и запуска приложения без ошибок.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="5cf6f-146">При запуске приложения и добавить элемент в корзину покупок появится следующее.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="5cf6f-147">Обратите внимание на то, что мы информирующим отображение сетки «default» путем реализации трех настраиваемых столбцов.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="5cf6f-148">Во-первых, неизменяемое, «Привязанного» поле количества:</span><span class="sxs-lookup"><span data-stu-id="5cf6f-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="5cf6f-149">Далее приведен «вычисляемый» столбец, который отображает элемент строки общее (элемент затраты времени количества, порядка).</span><span class="sxs-lookup"><span data-stu-id="5cf6f-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="5cf6f-150">Наконец у нас есть настраиваемый столбец, содержащий элемент управления CheckBox, пользователь будет использовать для указания удаления элемента из корзины диаграммы.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="5cf6f-151">Как вы видите, порядок линия Total является пустым, так что давайте добавить логику для вычисления итог заказа.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="5cf6f-152">Сначала мы реализуем метод «GetTotal», используемую для нашего класса MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="5cf6f-153">В файле MyShoppingCart.cs добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="5cf6f-154">Затем на странице\_можно вызовем наш метод GetTotal, используемую обработчик события загрузки.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="5cf6f-155">В то же время мы добавим теста, пуста ли корзина для покупок и регулирует отображение соответствующим образом в том случае, если это.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="5cf6f-156">Если корзина покупок пуста мы получаем это:</span><span class="sxs-lookup"><span data-stu-id="5cf6f-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="5cf6f-157">И если это не так, мы видим, наши всего.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="5cf6f-158">Тем не менее эта страница еще не завершено.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="5cf6f-159">Нам понадобится дополнительная логика повторного вычисления в корзину для покупок, удаляя элементы, помеченные для удаления и определите новые значения количества, как некоторые будет изменено в сетке пользователем.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="5cf6f-160">Позволяет добавить метод «RemoveItem» наш класс покупательской корзины в MyShoppingCart.cs для обработки случая, когда пользователь отмечает элемент для удаления.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="5cf6f-161">Теперь давайте ad метод для обработки ситуации, когда пользователь изменяет просто качеству упорядочиваемых в GridView.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="5cf6f-162">Основные компоненты удаление и обновление на месте можно реализовать логику, которая обновляется в корзину в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="5cf6f-163">(В MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="5cf6f-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="5cf6f-164">Можно заметить, что этот метод ожидает два параметра.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="5cf6f-165">Один идентификатор Корзина для покупок и другой массив объектов, определяемых пользователем типа.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="5cf6f-166">Таким образом, чтобы свести к минимуму зависимости логику от специфики интерфейс пользователя, мы определили структуру данных, который можно использовать для передачи элементам корзины покупок в наш код без нашего метода, не требуется непосредственно доступ к элемента управления GridView.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="5cf6f-167">В наш файл MyShoppingCart.aspx.cs можно использовать эту структуру в наш обработчик события Click кнопки обновления следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="5cf6f-168">Обратите внимание, что помимо обновления в корзину мы обновим также общего количества покупок.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="5cf6f-169">Обратите внимание, с помощью особый интерес представляет эту строку кода:</span><span class="sxs-lookup"><span data-stu-id="5cf6f-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="5cf6f-170">GetValues() — это специальные вспомогательная функция, который мы реализуем в MyShoppingCart.aspx.cs, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="5cf6f-171">Это предоставляет чистый способ доступа к значениям элементов привязки в наш элемент управления GridView.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="5cf6f-172">Так как наш элемент управления CheckBox «Удалить элемент» не привязан мы будем доступ к ней через метод FindControl().</span><span class="sxs-lookup"><span data-stu-id="5cf6f-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="5cf6f-173">На этом этапе разработки проекта мы готовимся реализовать процесс подсчета стоимости сначала.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="5cf6f-174">Прежде чем таким образом, давайте используйте Visual Studio для создания базы данных членства и Добавление пользователя в хранилище данных членства.</span><span class="sxs-lookup"><span data-stu-id="5cf6f-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5cf6f-175">[Назад](tailspin-spyworks-part-4.md)
> [Вперед](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="5cf6f-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
