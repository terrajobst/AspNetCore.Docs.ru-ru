---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: "Часть 9: Регистрация и извлечение | Документы Microsoft"
author: jongalloway
description: "Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store. Часть 9 охватывает извлечения и регистрации."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 71f87043be064d24bdfb203380fb6cf651527e30
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="51f2a-104">Часть 9: Регистрация и извлечение</span><span class="sxs-lookup"><span data-stu-id="51f2a-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="51f2a-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="51f2a-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="51f2a-106">MVC Music Store является учебное приложение, которое вводятся и описываются пошаговые способы использования Visual Studio и ASP.NET MVC для веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="51f2a-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="51f2a-107">MVC Music Store показана реализация хранилища упрощенных образец продает велосипеды через Интернет альбомы, которое реализует Администрирование сайта основные, вход пользователей и функциональные возможности корзины покупок.</span><span class="sxs-lookup"><span data-stu-id="51f2a-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="51f2a-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="51f2a-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="51f2a-109">Часть 9 охватывает извлечения и регистрации.</span><span class="sxs-lookup"><span data-stu-id="51f2a-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="51f2a-110">В этом разделе мы создадим CheckoutController, который будет собирать покупателя адрес и сведения о платеже.</span><span class="sxs-lookup"><span data-stu-id="51f2a-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="51f2a-111">Нам потребуется пользователям зарегистрировать в нашем сайте перед возвратом, поэтому этот контроллер будет требовать авторизации.</span><span class="sxs-lookup"><span data-stu-id="51f2a-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="51f2a-112">Пользователи, чтобы перейти извлечение из покупательской корзины, нажав кнопку «Оформление заказа».</span><span class="sxs-lookup"><span data-stu-id="51f2a-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="51f2a-113">Если пользователь не вошел, их работа будет предложено.</span><span class="sxs-lookup"><span data-stu-id="51f2a-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="51f2a-114">После успешного входа пользователя затем отображается представление адрес "и" Оплата ".</span><span class="sxs-lookup"><span data-stu-id="51f2a-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="51f2a-115">Как только они заполнения формы и подтверждением заказа, они будут показаны экране подтверждения заказа.</span><span class="sxs-lookup"><span data-stu-id="51f2a-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="51f2a-116">Попытка просмотреть порядок несуществующий или заказа, который не принадлежит покажет представлении ошибок.</span><span class="sxs-lookup"><span data-stu-id="51f2a-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="51f2a-117">Миграция в корзину для покупок</span><span class="sxs-lookup"><span data-stu-id="51f2a-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="51f2a-118">Хотя процесс покупательской анонимный доступ, при нажатии на кнопку извлечения, они будут необходимы для регистрации и входа в систему.</span><span class="sxs-lookup"><span data-stu-id="51f2a-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="51f2a-119">Пользователи ожидают, что необходимо поддерживать их покупательской корзины сведений между посещений, поэтому будет необходимо связать с пользователем данные покупательской корзины, завершении регистрации или входа.</span><span class="sxs-lookup"><span data-stu-id="51f2a-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="51f2a-120">Это действительно очень простой сделать, наш класс ShoppingCart уже есть метод, который будет связывать все элементы в текущем покупок с именем пользователя.</span><span class="sxs-lookup"><span data-stu-id="51f2a-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="51f2a-121">Только что нам потребуется вызвать этот метод, когда пользователь завершает регистрации или входа в систему.</span><span class="sxs-lookup"><span data-stu-id="51f2a-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="51f2a-122">Откройте **AccountController** класс, который мы добавили, когда мы Настройка членства и авторизации.</span><span class="sxs-lookup"><span data-stu-id="51f2a-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="51f2a-123">Добавление с помощью инструкции, ссылающиеся на MvcMusicStore.Models, затем добавьте следующий метод MigrateShoppingCart:</span><span class="sxs-lookup"><span data-stu-id="51f2a-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="51f2a-124">Затем измените действия после входа в систему для вызова MigrateShoppingCart после проверки пользователя, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="51f2a-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="51f2a-125">Внесите такие же изменения в регистр действий, выполняемых после, сразу после успешного создания учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="51f2a-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="51f2a-126">Вот и все — теперь анонимный корзину будет автоматически переносится на учетную запись пользователя после успешной регистрации или входа.</span><span class="sxs-lookup"><span data-stu-id="51f2a-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="51f2a-127">Создание CheckoutController</span><span class="sxs-lookup"><span data-stu-id="51f2a-127">Creating the CheckoutController</span></span>

<span data-ttu-id="51f2a-128">Щелкните правой кнопкой мыши папку Controllers и добавить новый контроллер в проект с именем CheckoutController, с помощью шаблона пустой контроллер.</span><span class="sxs-lookup"><span data-stu-id="51f2a-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="51f2a-129">Во-первых добавьте атрибут Authorize, перед объявлением класса контроллера требовать от пользователей зарегистрировать до извлечения:</span><span class="sxs-lookup"><span data-stu-id="51f2a-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="51f2a-130">*Примечание: Это похоже на изменение, которое мы внесли StoreManagerController ранее, но в этом случае атрибут авторизации требуется, чтобы пользователь был в роль администратора. В контроллере извлечения мы требование входа пользователя, но не требуется, они должны администраторов.*</span><span class="sxs-lookup"><span data-stu-id="51f2a-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="51f2a-131">Для простоты мы не дело с платежную информацию в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="51f2a-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="51f2a-132">Вместо этого мы, позволяя пользователям извлекать, используя код рекламной акции.</span><span class="sxs-lookup"><span data-stu-id="51f2a-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="51f2a-133">Этот код рекламной акции, с помощью константу с именем PromoCode сохраняется.</span><span class="sxs-lookup"><span data-stu-id="51f2a-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="51f2a-134">Как StoreController мы будем объявления поля для хранения экземпляра класса MusicStoreEntities, с именем storeDB.</span><span class="sxs-lookup"><span data-stu-id="51f2a-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="51f2a-135">Чтобы использовать класс MusicStoreEntities, вам потребуется добавить с помощью инструкции для пространства имен MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="51f2a-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="51f2a-136">Появляется вверху нашей извлечения контроллера.</span><span class="sxs-lookup"><span data-stu-id="51f2a-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="51f2a-137">CheckoutController, будет содержать следующие действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="51f2a-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="51f2a-138">**AddressAndPayment (метод GET)** форма позволяет пользователю вводить свои данные.</span><span class="sxs-lookup"><span data-stu-id="51f2a-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="51f2a-139">**AddressAndPayment (метод POST)** будет проверки входных данных и обработки заказа.</span><span class="sxs-lookup"><span data-stu-id="51f2a-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="51f2a-140">**Полный** будет отображаться после пользователь успешно завершил процесс извлечения.</span><span class="sxs-lookup"><span data-stu-id="51f2a-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="51f2a-141">Это представление будет содержать пользователя порядковый номер, как подтверждение.</span><span class="sxs-lookup"><span data-stu-id="51f2a-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="51f2a-142">Во-первых для AddressAndPayment переименуем действие контроллера индекса (которая была создана, когда мы создавали контроллера).</span><span class="sxs-lookup"><span data-stu-id="51f2a-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="51f2a-143">Это действие контроллера только отображает форму извлечения, поэтому он не требует сведений о любой модели.</span><span class="sxs-lookup"><span data-stu-id="51f2a-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="51f2a-144">Наш метод AddressAndPayment POST будут следовать шаблону, используется в StoreManagerController: он будет пытаться принять отправки формы и завершить порядок и повторного отображения формы в случае неудачи.</span><span class="sxs-lookup"><span data-stu-id="51f2a-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="51f2a-145">После проверки входных данных формы отвечает требованиям наших проверки заказа, мы будет выполнена проверка PromoCode форму напрямую.</span><span class="sxs-lookup"><span data-stu-id="51f2a-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="51f2a-146">Предположим, что все работает правильно, будет сохранить обновленную информацию с порядком, указать объекту ShoppingCart для завершения процесса заказа и перенаправления для завершения действия.</span><span class="sxs-lookup"><span data-stu-id="51f2a-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="51f2a-147">После успешного завершения процесса извлечения пользователи будут перенаправляться в действие завершения контроллера.</span><span class="sxs-lookup"><span data-stu-id="51f2a-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="51f2a-148">Это действие выполнит простую проверку, чтобы проверить, что порядок действительно принадлежат вошедшего в систему пользователя перед отображением порядковый номер в качестве подтверждения.</span><span class="sxs-lookup"><span data-stu-id="51f2a-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="51f2a-149">*Примечание: Ошибка представления был автоматически создан для нас в папке /Views/Shared начале проекта.*</span><span class="sxs-lookup"><span data-stu-id="51f2a-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="51f2a-150">Ниже приведен полный код CheckoutController:</span><span class="sxs-lookup"><span data-stu-id="51f2a-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="51f2a-151">Добавление представления AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="51f2a-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="51f2a-152">Теперь давайте создадим AddressAndPayment представления.</span><span class="sxs-lookup"><span data-stu-id="51f2a-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="51f2a-153">Щелкните правой кнопкой мыши на одно из действий контроллера AddressAndPayment и добавить представление с именем AddressAndPayment, строго типизируется как заказа и использующему изменить шаблон, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="51f2a-153">Right-click on one of the the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="51f2a-154">В этом представлении сделает использование двух методов, мы рассмотрели при построении StoreManagerEdit представления:</span><span class="sxs-lookup"><span data-stu-id="51f2a-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="51f2a-155">Мы будем использовать Html.EditorForModel() для отображения полей формы для модели заказа</span><span class="sxs-lookup"><span data-stu-id="51f2a-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="51f2a-156">Мы будет использовать правила проверки, используя класс Order с атрибутов проверки</span><span class="sxs-lookup"><span data-stu-id="51f2a-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="51f2a-157">Мы начнем путем обновления в код формы для использования Html.EditorForModel(), следуют дополнительные textbox для этот промокод.</span><span class="sxs-lookup"><span data-stu-id="51f2a-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="51f2a-158">Ниже приведен полный код для представления AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="51f2a-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="51f2a-159">Определение правила проверки для заказа</span><span class="sxs-lookup"><span data-stu-id="51f2a-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="51f2a-160">Теперь, когда Настройка наши представления, мы настроите правила проверки для нашей модели заказ как это делалось ранее для модели альбома.</span><span class="sxs-lookup"><span data-stu-id="51f2a-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="51f2a-161">Щелкните правой кнопкой мыши в папке «Models» и добавьте класс с именем заказа.</span><span class="sxs-lookup"><span data-stu-id="51f2a-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="51f2a-162">В дополнение к атрибутам проверки, использовавшегося ранее альбома будет также использоваться регулярное выражение для проверки адреса электронной почты пользователя.</span><span class="sxs-lookup"><span data-stu-id="51f2a-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="51f2a-163">Попытка отправки формы с отсутствующим или недопустимых данных теперь будет отображаться сообщение об ошибке с помощью проверки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="51f2a-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="51f2a-164">Итак мы использовали большую часть тяжелой работы для процесса извлечения; только у нас есть несколько элементов и вероятность завершения.</span><span class="sxs-lookup"><span data-stu-id="51f2a-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="51f2a-165">Необходимо добавить два простых представления, а нам нужны для выполнения операций передачи данных покупок при входе в систему.</span><span class="sxs-lookup"><span data-stu-id="51f2a-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="51f2a-166">Добавление представления завершения извлечения</span><span class="sxs-lookup"><span data-stu-id="51f2a-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="51f2a-167">Извлечение полного представления довольно прост, сколько необходимо для отображения, номер заказа.</span><span class="sxs-lookup"><span data-stu-id="51f2a-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="51f2a-168">Щелкните правой кнопкой мыши на действие завершения контроллера и добавить представление с именем завершить которого строго типизируется как целочисленное значение.</span><span class="sxs-lookup"><span data-stu-id="51f2a-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="51f2a-169">Теперь мы обновим просмотреть код, чтобы отобразить идентификатор заказа, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="51f2a-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="51f2a-170">Обновление представления ошибки</span><span class="sxs-lookup"><span data-stu-id="51f2a-170">Updating The Error view</span></span>

<span data-ttu-id="51f2a-171">Шаблон по умолчанию включает представление ошибки в папку Shared представления, чтобы его можно было использовать повторно в другом месте на сайте.</span><span class="sxs-lookup"><span data-stu-id="51f2a-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="51f2a-172">В этом представлении ошибка содержит ошибку очень простой и не использует наш сайт макета, поэтому мы обновим.</span><span class="sxs-lookup"><span data-stu-id="51f2a-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="51f2a-173">Так как это общая ошибка страницы содержимого очень прост.</span><span class="sxs-lookup"><span data-stu-id="51f2a-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="51f2a-174">Мы будем включать сообщение и ссылка для перехода на предыдущую страницу в журнале, если пользователю требуется повторить их действия.</span><span class="sxs-lookup"><span data-stu-id="51f2a-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


>[!div class="step-by-step"]
<span data-ttu-id="51f2a-175">[Назад](mvc-music-store-part-8.md)
[Вперед](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="51f2a-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
