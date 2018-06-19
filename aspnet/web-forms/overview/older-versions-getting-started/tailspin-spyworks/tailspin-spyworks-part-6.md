---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Часть 6: Членства в ASP.NET | Документы Microsoft'
author: JoeStagner
description: Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Часть 6 добавляет членства ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 83e9bc780ea8face3e0f55fdf8c00e13b60f80a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886814"
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="5ce6c-104">Часть 6: Членства в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5ce6c-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="5ce6c-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="5ce6c-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="5ce6c-106">Tailspin Spyworks показано, как чрезвычайно простой можно создавать мощные и масштабируемые приложения для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="5ce6c-107">Он показывает как использовать новые возможности в ASP.NET 4 для создания Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="5ce6c-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="5ce6c-109">Часть 6 добавляет членства ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="5ce6c-110">Работа с помощью членства ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5ce6c-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="5ce6c-111">Перейдите на вкладку Безопасность</span><span class="sxs-lookup"><span data-stu-id="5ce6c-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="5ce6c-112">Убедитесь в том, что мы используем проверку подлинности форм.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="5ce6c-113">Используйте ссылку «Создать пользователя» для создания нескольких пользователей.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="5ce6c-114">После этого, см. в окне обозревателя решений и обновить представление.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="5ce6c-115">Обратите внимание, что ASPNETDB. Был создан MDF нормально.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="5ce6c-116">Этот файл содержит таблицы, для поддержки базовых служб ASP.NET, как членство.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="5ce6c-117">Теперь мы можем начать реализации процесса извлечения.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="5ce6c-118">Начните с создания CheckOut.aspx страницы.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="5ce6c-119">На странице CheckOut.aspx только должны быть доступны пользователям, выполнившим вход, мы будет ограничивать доступ к вход пользователей и перенаправляет пользователей, которые не вошли на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="5ce6c-120">Для этого мы добавим следующие раздел конфигурации наш файл web.config.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="5ce6c-121">Шаблон для приложений веб-форм ASP.NET автоматически в наш файл web.config, добавлен раздел проверки подлинности и установить страницы входа по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="5ce6c-122">Нам необходимо изменить Login.aspx файле кода программной части для переноса анонимный корзину при входе пользователя.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="5ce6c-123">Изменить страницу\_загрузить событие следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="5ce6c-124">Затем добавьте обработчик событий «LoggedIn» как этот параметр, чтобы задать имя сеанса для вновь зарегистрированного в системе пользователя и изменить код временного сеанса в корзину для пользователя путем вызова метода MigrateCart в нашем классе MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="5ce6c-125">(Реализованный в CS-файл)</span><span class="sxs-lookup"><span data-stu-id="5ce6c-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="5ce6c-126">Реализуйте метод MigrateCart() следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="5ce6c-127">В checkout.aspx мы будем использовать EntityDataSource и GridView в нашем извлечение страницы так же, как было сделано в нашу страницу отзывов корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="5ce6c-128">Обратите внимание, что наш элемент управления GridView указывает обработчик событий «ondatabound» с именем экземпляры MyList\_RowDataBound давайте реализации этого обработчика событий следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="5ce6c-129">Этот метод позволяет сохранить корзину покупок промежуточных итогов как каждая строка привязан и обновляет в нижней строке элемента управления GridView.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="5ce6c-130">На этом этапе мы применили заказа, чтобы разместить презентации «отзыв».</span><span class="sxs-lookup"><span data-stu-id="5ce6c-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="5ce6c-131">Давайте обрабатывать сценарий пустой корзины для покупок, добавив несколько строк кода нашу страницу\_событие загрузки:</span><span class="sxs-lookup"><span data-stu-id="5ce6c-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="5ce6c-132">При нажатии кнопки «Отправить» мы будет выполните следующий код в обработчик события Click кнопки отправки.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="5ce6c-133">Для реализации в метод SubmitOrder() класса MyShoppingCart — «Молоко» процесс отправки заказа.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="5ce6c-134">SubmitOrder выполняет следующие действия:</span><span class="sxs-lookup"><span data-stu-id="5ce6c-134">SubmitOrder will:</span></span>

- <span data-ttu-id="5ce6c-135">Принимать все элементы в корзину и использовать их для создания новой записи заказа и связанные записи OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="5ce6c-136">Вычисления, дата отгрузки.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="5ce6c-137">Очистите корзину.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="5ce6c-138">Для целей этого образца приложения будет вычислять Дата отгрузки, просто добавляя двух дней до текущей даты.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="5ce6c-139">Запуск приложения теперь позволит нам для тестирования процесса покупок от начала до конца.</span><span class="sxs-lookup"><span data-stu-id="5ce6c-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5ce6c-140">[Назад](tailspin-spyworks-part-5.md)
> [Вперед](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="5ce6c-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
