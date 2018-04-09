---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Часть 10: Окончательный обновления навигации и разработки узла, в заключение | Документы Microsoft'
author: jongalloway
description: Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store. Часть 10 охватывает окончательного обновлений для навигации и S....
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: b40d194c4d08f3564da59bacde4b5d3d7663373a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="a2b9d-104">Часть 10: Окончательный обновления навигации и разработки узла, в заключение</span><span class="sxs-lookup"><span data-stu-id="a2b9d-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="a2b9d-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a2b9d-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a2b9d-106">MVC Music Store является учебное приложение, которое вводятся и описываются пошаговые способы использования Visual Studio и ASP.NET MVC для веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a2b9d-107">MVC Music Store показана реализация хранилища упрощенных образец продает велосипеды через Интернет альбомы, которое реализует Администрирование сайта основные, вход пользователей и функциональные возможности корзины покупок.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="a2b9d-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a2b9d-109">Часть 10 охватывает окончательного обновлений для навигации и разработки узла, заключение.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="a2b9d-110">Мы выполнили все основные функции для наш сайт, но еще есть некоторые возможности для добавления узла навигации на домашней странице и странице Выбор хранилища.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="a2b9d-111">Создание сводки частичного представления покупательской корзины</span><span class="sxs-lookup"><span data-stu-id="a2b9d-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="a2b9d-112">Необходимо предоставить количество элементов в корзине пользователя на всем сайте.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="a2b9d-113">Мы легко можно реализовать путем создания частичного представления, который добавляется в нашем Site.master.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="a2b9d-114">Как показано выше, контроллер ShoppingCart включает CartSummary методу действия, который возвращает частичного представления.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="a2b9d-115">Чтобы создать CartSummary частичного представления, правой кнопкой мыши папку представления и ShoppingCart и выберите Добавить представление.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="a2b9d-116">Имя представления CartSummary и установите флажок «Создать разделяемое представление», как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="a2b9d-117">Частичное представление CartSummary очень простые — это просто ссылку на представление индекса ShoppingCart, которое показывает количество элементов в корзине.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="a2b9d-118">Ниже приведен полный код для CartSummary.cshtml:</span><span class="sxs-lookup"><span data-stu-id="a2b9d-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="a2b9d-119">В любой страницы сайта, включая основной сайт, с помощью метода Html.RenderAction, можно включить частичного представления.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="a2b9d-120">RenderAction требует указать имя действия («CartSummary») и имя контроллера («ShoppingCart») как ниже.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="a2b9d-121">Перед добавлением узла макета, также будет рассмотрено создание меню жанра, мы предоставим все наши обновления Site.master за один раз.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="a2b9d-122">Создание частичного представления жанра меню</span><span class="sxs-lookup"><span data-stu-id="a2b9d-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="a2b9d-123">Можно сделать его намного проще для пользователей для перемещения по магазину путем добавления меню жанра, который содержит список всех жанров доступны в нашем хранилище.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="a2b9d-124">Мы будем придерживаться именно действия также создать разделяемое представление GenreMenu и затем мы можем добавить оба главного узла.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="a2b9d-125">Во-первых добавьте следующее действие контроллера GenreMenu StoreController:</span><span class="sxs-lookup"><span data-stu-id="a2b9d-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="a2b9d-126">Это действие возвращает список жанров которых будет отображаться для частичного представления, который мы создадим Далее.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="a2b9d-127">*Примечание: Мы добавили атрибут [ChildActionOnly] для этого действия контроллера, которое указывает, что нам нужен только это действие для использования из частичного представления. Этот атрибут не позволит выполняется путем перехода к /Store/GenreMenu действия контроллера. Это необязательно для частичных представлений, но рекомендуется, поскольку мы хотим убедиться, что наши действия контроллера, используются как мы реализуем. Мы также возвращается PartialView, а не представление, которое позволяет обработчик представлений знать, что его не следует использовать макет для этого представления, как он включен в других представлениях.*</span><span class="sxs-lookup"><span data-stu-id="a2b9d-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="a2b9d-128">Щелкните правой кнопкой мыши на действие контроллера GenreMenu и создать частичное представление с именем GenreMenu, который является строго типизированным с помощью класса данных представления жанра, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="a2b9d-129">Обновите код представления для частичного представления GenreMenu для отображения элементов с помощью неупорядоченный список следующим образом.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="a2b9d-130">Обновление сайта макет для отображения нашей разделяемые представления</span><span class="sxs-lookup"><span data-stu-id="a2b9d-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="a2b9d-131">Макет узла можно добавить наш разделяемые представления (/представления/Общие/\_Layout.cshtml) путем вызова Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="a2b9d-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="a2b9d-132">Мы добавим их в, а также некоторые дополнительные разметку, чтобы отобразить их, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="a2b9d-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="a2b9d-133">Теперь при запуске приложения, мы увидим сводки для покупок в верхней и жанр в левой части области переходов.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="a2b9d-134">Обновите страницу Обзор хранилища</span><span class="sxs-lookup"><span data-stu-id="a2b9d-134">Update to the Store Browse page</span></span>

<span data-ttu-id="a2b9d-135">На странице Обзор хранилища работает, но выглядит не очень хорошо.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="a2b9d-136">Мы можем обновить страницу, чтобы показать альбомов в улучшения макета путем изменения представления кода (см. в /Views/Store/Browse.cshtml) следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a2b9d-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="a2b9d-137">Здесь мы выполняем используйте Url.Action вместо Html.ActionLink, чтобы мы сможем применить специальное форматирование к ссылке на включать иллюстрации альбома.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="a2b9d-138">*Примечание: Мы отображение крышка универсального альбом эти альбомов. Эта информация хранится в базе данных и для редактирования в диспетчере хранилища. Мы приглашаем вас Добавление логотипа.*</span><span class="sxs-lookup"><span data-stu-id="a2b9d-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="a2b9d-139">Теперь если мы перейти жанра, мы увидим альбомов из сетки с иллюстрацией альбома.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="a2b9d-140">Обновление домашней страницы для отображения наиболее продаваемых альбомы Top</span><span class="sxs-lookup"><span data-stu-id="a2b9d-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="a2b9d-141">Мы хотим компонентов наших наиболее продаваемых альбомов на домашней странице, чтобы увеличить продажи.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="a2b9d-142">Мы выполним некоторые обновления с нашей HomeController для обработки и добавить в некоторых дополнительных графических.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="a2b9d-143">Во-первых мы добавим свойство навигации для нашего альбом класса, чтобы EntityFramework известно, что они связаны.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="a2b9d-144">Последние три строки из наших **альбом** класс теперь должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a2b9d-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="a2b9d-145">*Примечание: Для этого потребуется добавлении с помощью инструкции для перевода в пространство имен System.Collections.Generic.*</span><span class="sxs-lookup"><span data-stu-id="a2b9d-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="a2b9d-146">Во-первых мы добавим поля storeDB и MvcMusicStore.Models, с помощью инструкций, как в нашем другие контроллеры.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="a2b9d-147">Далее мы добавим следующий метод HomeController, который запрашивает top наиболее продаваемых альбомы согласно OrderDetails нашей базы данных.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="a2b9d-148">Это закрытый метод, поскольку мы не хотим сделать ее доступной как действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="a2b9d-149">Предоставляются вместе в HomeController для простоты, но настоятельно рекомендуется переместить бизнес-логики в отдельные классы соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="a2b9d-150">С этим мы можем обновить действие контроллера индекс для запроса пяти наиболее успешных альбомы и возвращать их в представление.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="a2b9d-151">Полный код для обновленной HomeController — как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="a2b9d-152">Наконец необходимо обновить нашей Главная индекс представления так, чтобы его можно отобразить список альбомов путем обновления тип модели и добавление списка альбом нижней.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="a2b9d-153">Мы будет использовать эту возможность также добавить на страницу заголовок и раздел повышения роли.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="a2b9d-154">Теперь при запуске приложения, мы рассмотрим наши обновленные Домашняя страница с верхней наиболее продаваемых альбомы и наши рекламные сообщения.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="a2b9d-155">Заключение</span><span class="sxs-lookup"><span data-stu-id="a2b9d-155">Conclusion</span></span>

<span data-ttu-id="a2b9d-156">Мы узнали, что ASP.NET MVC упрощает создание сложных веб-сайта с помощью доступа к базе данных, членство в AJAX, и т. д.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="a2b9d-157">довольно быстро.</span><span class="sxs-lookup"><span data-stu-id="a2b9d-157">pretty quickly.</span></span> <span data-ttu-id="a2b9d-158">Надеемся, у этого учебника вы получили средства, необходимые для приступить к созданию собственного ASP.NET MVC приложений!</span><span class="sxs-lookup"><span data-stu-id="a2b9d-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="a2b9d-159">Назад</span><span class="sxs-lookup"><span data-stu-id="a2b9d-159">Previous</span></span>](mvc-music-store-part-9.md)
