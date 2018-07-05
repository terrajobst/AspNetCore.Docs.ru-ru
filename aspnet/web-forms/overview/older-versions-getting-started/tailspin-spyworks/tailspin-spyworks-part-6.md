---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Часть 6: Членства ASP.NET | Документация Майкрософт'
author: JoeStagner
description: В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks. Часть 6 добавляет членства ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6d01a745ca1428f065f564f676d483ee807eb52e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368477"
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="6982f-104">Часть 6: Членства ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6982f-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="6982f-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="6982f-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="6982f-106">Tailspin Spyworks демонстрирует, как чрезвычайно прост процесс создания мощных и масштабируемых приложений для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="6982f-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="6982f-107">Он демонстрирует способы использования новые возможности в ASP.NET 4 для создание Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="6982f-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="6982f-108">В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="6982f-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="6982f-109">Часть 6 добавляет членства ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6982f-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="6982f-110">Работа с ASP.NET Membership</span><span class="sxs-lookup"><span data-stu-id="6982f-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="6982f-111">Нажмите кнопку безопасности</span><span class="sxs-lookup"><span data-stu-id="6982f-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="6982f-112">Убедитесь, что мы используем проверку подлинности форм.</span><span class="sxs-lookup"><span data-stu-id="6982f-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="6982f-113">Используйте ссылку «Создать пользователя» для создания нескольких пользователей.</span><span class="sxs-lookup"><span data-stu-id="6982f-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="6982f-114">После этого, см. в окне обозревателя решений и обновить представление.</span><span class="sxs-lookup"><span data-stu-id="6982f-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="6982f-115">Обратите внимание, что ASPNETDB. Будет создана MDF нормально.</span><span class="sxs-lookup"><span data-stu-id="6982f-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="6982f-116">Этот файл содержит таблицы для поддержки служб ASP.NET core как членства.</span><span class="sxs-lookup"><span data-stu-id="6982f-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="6982f-117">Теперь мы можем начать процесс подсчета стоимости покупок реализации.</span><span class="sxs-lookup"><span data-stu-id="6982f-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="6982f-118">Начните с создания CheckOut.aspx страницы.</span><span class="sxs-lookup"><span data-stu-id="6982f-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="6982f-119">На странице CheckOut.aspx только должны быть доступны для пользователей, которые выполнили вход, поэтому мы будем ограничивать доступ к пользователям и перенаправляет пользователей, которые не вошли на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="6982f-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="6982f-120">Для этого мы добавим следующее в раздел конфигурации из нашего файла web.config.</span><span class="sxs-lookup"><span data-stu-id="6982f-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="6982f-121">Шаблон для приложений ASP.NET Web Forms автоматически добавить раздел проверки подлинности в наш файл web.config и установить страницы входа по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6982f-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="6982f-122">Нам необходимо изменить Login.aspx файл кода программной части для переноса анонимный корзины, при входе пользователя.</span><span class="sxs-lookup"><span data-stu-id="6982f-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="6982f-123">Изменение страницы\_загрузить событий следующим образом.</span><span class="sxs-lookup"><span data-stu-id="6982f-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="6982f-124">Затем добавьте обработчик событий «LoggedIn» следующим образом присвоено вновь пользователь имя сеанса и изменить его временного сеанса в корзине пользователя путем вызова метода MigrateCart в нашем классе MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="6982f-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="6982f-125">(Реализованный в CS-файл)</span><span class="sxs-lookup"><span data-stu-id="6982f-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="6982f-126">Реализуйте метод MigrateCart() следующим образом.</span><span class="sxs-lookup"><span data-stu-id="6982f-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="6982f-127">В checkout.aspx мы будем использовать EntityDataSource и GridView в нашей извлечение страницы так же, как мы делали в нашей корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="6982f-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="6982f-128">Обратите внимание, что наш элемент управления GridView указывает обработчик событий «ondatabound», с именем MyList\_RowDataBound так что давайте реализуем этот обработчик событий следующим образом.</span><span class="sxs-lookup"><span data-stu-id="6982f-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="6982f-129">Этот метод сохраняет, промежуточных итогов по покупок корзину, как каждая строка привязан и обновляет нижняя строка GridView.</span><span class="sxs-lookup"><span data-stu-id="6982f-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="6982f-130">На этом этапе мы реализовали заказа должна быть помещена презентации «review».</span><span class="sxs-lookup"><span data-stu-id="6982f-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="6982f-131">Давайте обрабатывать сценарии пустой корзины для покупок, добавив несколько строк кода к нашей странице\_событие загрузки:</span><span class="sxs-lookup"><span data-stu-id="6982f-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="6982f-132">Когда пользователь нажимает кнопку «Отправить» мы выполним следующий код в обработчике события Click кнопки отправки.</span><span class="sxs-lookup"><span data-stu-id="6982f-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="6982f-133">Для реализации в метод SubmitOrder() нашего класса MyShoppingCart является «Основу» процесс отправки заказа.</span><span class="sxs-lookup"><span data-stu-id="6982f-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="6982f-134">SubmitOrder выполняет следующие действия:</span><span class="sxs-lookup"><span data-stu-id="6982f-134">SubmitOrder will:</span></span>

- <span data-ttu-id="6982f-135">Примите все элементы в Корзина для покупок и использовать их для создания новой записи заказа и связанные записи OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="6982f-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="6982f-136">Вычисления, дата отгрузки.</span><span class="sxs-lookup"><span data-stu-id="6982f-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="6982f-137">Очистите корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="6982f-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="6982f-138">Для целей этого примера приложения мы вычисляем Дата отгрузки, просто добавляя двух дней до текущей даты.</span><span class="sxs-lookup"><span data-stu-id="6982f-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="6982f-139">Запуск приложения теперь позволит нам протестировать корзину процесс от начала до конца.</span><span class="sxs-lookup"><span data-stu-id="6982f-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6982f-140">[Назад](tailspin-spyworks-part-5.md)
> [Вперед](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="6982f-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
