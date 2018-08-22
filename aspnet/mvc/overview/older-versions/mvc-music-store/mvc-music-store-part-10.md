---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Часть 10: Окончательные обновления структуры навигации и проекта сайта, заключение | Документация Майкрософт'
author: jongalloway
description: В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store. Часть 10 охватывает окончательные обновления структуры навигации и S...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f32509701dd112053aa4f31d6552601f961c7413
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827947"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="ff4f6-104">Часть 10: Окончательные обновления структуры навигации и проекта сайта, заключение</span><span class="sxs-lookup"><span data-stu-id="ff4f6-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="ff4f6-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="ff4f6-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="ff4f6-106">Store музыки MVC является учебного приложения, которое вводятся и описываются пошаговые использование ASP.NET MVC и Visual Studio для разработки веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="ff4f6-107">Store музыки MVC — это реализация хранилища упрощенный пример, который продает музыкальные альбомы через Интернет и реализует базовый сайт администрирования, вход пользователя и корзины для покупок функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="ff4f6-108">В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="ff4f6-109">Часть 10 охватывает окончательные обновления структуры навигации и проекта сайта, заключение.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="ff4f6-110">Мы завершили все основные функции по нашем сайте, но по-прежнему у нас есть некоторые возможности для добавления в структуру переходов узла, на домашней странице и странице Обзор Store.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="ff4f6-111">Создание сводки частичного представления корзину для покупок</span><span class="sxs-lookup"><span data-stu-id="ff4f6-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="ff4f6-112">Необходимо предоставить количество элементов в корзине пользователя на всем сайте.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="ff4f6-113">Можно легко реализовать это путем создания частичного представления, который добавляется в нашей Site.master.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="ff4f6-114">Как было показано ранее, контроллер ShoppingCart включает метод действия CartSummary, который возвращает частичного представления:</span><span class="sxs-lookup"><span data-stu-id="ff4f6-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="ff4f6-115">Чтобы создать CartSummary частичного представления, представления/ShoppingCart папку правой кнопкой мыши и выберите Add View.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="ff4f6-116">Имя представления CartSummary, а также установите флажок «Создать частичное представление», как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="ff4f6-117">Частичное представление CartSummary выполняется очень просто — это просто ссылка в представление ShoppingCart Index, показывающий количество элементов в корзине.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="ff4f6-118">Полный код для CartSummary.cshtml выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ff4f6-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="ff4f6-119">В любой страницы в узле, включая главный узел, с помощью метода Html.RenderAction, можно включить частичное представление.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="ff4f6-120">RenderAction требует указать имя действия («CartSummary») и имя контроллера («ShoppingCart») как ниже.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="ff4f6-121">Перед добавлением узла макета, мы также создадим меню жанр для принятия всех новостей Site.master за один раз.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="ff4f6-122">Создание меню частичного представления жанра</span><span class="sxs-lookup"><span data-stu-id="ff4f6-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="ff4f6-123">Нам сделать его намного проще для наших пользователей перейти в магазине, добавив меню жанра, в которой перечислены все жанры доступны в нашем магазине.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="ff4f6-124">Мы будем следовать же действия также создать GenreMenu частичного представления, а затем можно добавить оба главного узла.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="ff4f6-125">Во-первых добавьте следующее действие контроллера GenreMenu StoreController:</span><span class="sxs-lookup"><span data-stu-id="ff4f6-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="ff4f6-126">Это действие возвращает список жанров, которые будут отображаться с частичного представления, который мы создадим Далее.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="ff4f6-127">*Примечание: Мы добавили атрибут [ChildActionOnly] для этого действия контроллера, который указывает, что нам нужен только это действие для использования из частичного представления. Этот атрибут не позволит выполнить, перейдя по адресу /Store/GenreMenu действия контроллера. Это не является обязательным для частичных представлений, но рекомендуется, поскольку мы хотим убедиться, что наши действия контроллера используются как планировалось. Мы также возвращается PartialView, а не представление, позволяющее знать, что его не следует использовать макет для этого представления, так как он включен в других представлениях обработчик представлений.*</span><span class="sxs-lookup"><span data-stu-id="ff4f6-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="ff4f6-128">Щелкните правой кнопкой мыши на действие контроллера GenreMenu и создать частичное представление с именем GenreMenu, который является строго типизированным с помощью класса данных представления жанра, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="ff4f6-129">Обновите код представления для частичного представления GenreMenu для отображения элементов, с помощью неупорядоченный список следующим образом.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="ff4f6-130">Обновление сайта макет для отображения наши частичного представления</span><span class="sxs-lookup"><span data-stu-id="ff4f6-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="ff4f6-131">Мы можем добавить наш частичные представления макета веб-узла (/представления/Общие/\_Layout.cshtml) путем вызова Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="ff4f6-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="ff4f6-132">Мы добавим их обоих в, а также некоторые дополнительные разметки, чтобы отобразить их, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="ff4f6-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="ff4f6-133">Теперь при запуске приложения, мы увидим сводки для покупок в верхней и жанр в области навигации слева.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="ff4f6-134">Обновите страницу Обзор Store</span><span class="sxs-lookup"><span data-stu-id="ff4f6-134">Update to the Store Browse page</span></span>

<span data-ttu-id="ff4f6-135">На странице Обзор Store работает, но выглядит не очень хорошо.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="ff4f6-136">Мы можем обновить страницу, чтобы показать альбомов в улучшения макета, обновив код представления (находится в /Views/Store/Browse.cshtml) следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ff4f6-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="ff4f6-137">Здесь мы выполняем использовать класс Url.Action, а не Html.ActionLink, что мы применить специальное форматирование к ссылку, чтобы включить изображения альбома.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="ff4f6-138">*Примечание: Мы при отображении универсального обложкам эти альбомов. Эта информация хранится в базе данных и можно изменять с помощью Store Manager. Можно также добавить собственные рисунки.*</span><span class="sxs-lookup"><span data-stu-id="ff4f6-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="ff4f6-139">Теперь при просмотре мы жанр фильма, мы увидим альбомов, из сетки с иллюстрацией альбома.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="ff4f6-140">Обновление домашней страницы для отображения наиболее продаваемых альбомов верхней</span><span class="sxs-lookup"><span data-stu-id="ff4f6-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="ff4f6-141">Мы хотим компонентов наши наиболее продаваемые альбомов на домашней странице для увеличения продаж.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="ff4f6-142">Сделаем некоторые обновления для наших HomeController для обработки и добавить некоторые дополнительные графики.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="ff4f6-143">Во-первых мы добавим свойство навигации к нашему классу альбома, чтобы EntityFramework знал, что они связаны.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="ff4f6-144">Последние три строки из наших **альбома** класс теперь должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ff4f6-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="ff4f6-145">*Примечание: Для этого потребуется добавить с помощью инструкции, чтобы объединить в пространстве имен System.Collections.Generic.*</span><span class="sxs-lookup"><span data-stu-id="ff4f6-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="ff4f6-146">Во-первых мы добавим поле storeDB и MvcMusicStore.Models, с помощью инструкций, как в нашем другие контроллеры.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="ff4f6-147">Далее мы добавим следующий метод HomeController, который запрашивает top наиболее продаваемых альбомов в соответствии с OrderDetails нашей базы данных.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="ff4f6-148">Это частный метод, так как мы не хотим сделать ее доступной как действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="ff4f6-149">Мы его включением в HomeController для простоты, но рекомендуется для перемещения бизнес-логику в отдельную службу классы соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="ff4f6-150">С этим мы можем обновить действие контроллера индекс для запроса пять наиболее популярных альбомы и возвращать их в представление.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="ff4f6-151">Полный код для обновленных HomeController является, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="ff4f6-152">Наконец нам потребуется обновить нашего Главная индекс представления так, чтобы отобразить список альбомов путем обновления типа модели и добавлении списка альбома нижней.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="ff4f6-153">Мы будет использовать эту возможность также добавить на страницу заголовка и раздела повышения роли.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="ff4f6-154">Теперь при запуске приложения, мы увидим обновленные домашней страницы с верхней наиболее продаваемых альбомы и наши рекламные сообщения.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="ff4f6-155">Заключение</span><span class="sxs-lookup"><span data-stu-id="ff4f6-155">Conclusion</span></span>

<span data-ttu-id="ff4f6-156">Мы уже видели, что ASP.NET MVC упрощает создание сложных веб-сайт с помощью доступа к базе данных, членство, AJAX, и т.д.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="ff4f6-157">довольно быстро.</span><span class="sxs-lookup"><span data-stu-id="ff4f6-157">pretty quickly.</span></span> <span data-ttu-id="ff4f6-158">Будем надеяться, что приведенная в этом учебнике вам инструменты, которые необходимо приступить к созданию собственных ASP.NET MVC приложений!</span><span class="sxs-lookup"><span data-stu-id="ff4f6-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="ff4f6-159">Назад</span><span class="sxs-lookup"><span data-stu-id="ff4f6-159">Previous</span></span>](mvc-music-store-part-9.md)
