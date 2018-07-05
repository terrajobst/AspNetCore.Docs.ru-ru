---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Часть 9: Регистрация и оформление заказа | Документация Майкрософт'
author: jongalloway
description: В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store. Часть 9 рассматривается регистрация и оформление заказа.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 935729be0dce790c6fce2e2e982ee063318d64dd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367014"
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="6afb6-104">Часть 9: Регистрация и оформление заказа</span><span class="sxs-lookup"><span data-stu-id="6afb6-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="6afb6-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="6afb6-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="6afb6-106">Store музыки MVC является учебного приложения, которое вводятся и описываются пошаговые использование ASP.NET MVC и Visual Studio для разработки веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="6afb6-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="6afb6-107">Store музыки MVC — это реализация хранилища упрощенный пример, который продает музыкальные альбомы через Интернет и реализует базовый сайт администрирования, вход пользователя и корзины для покупок функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="6afb6-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="6afb6-108">В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="6afb6-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="6afb6-109">Часть 9 рассматривается регистрация и оформление заказа.</span><span class="sxs-lookup"><span data-stu-id="6afb6-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="6afb6-110">В этом разделе мы создадим CheckoutController, который соберет покупателя адрес и сведения об оплате.</span><span class="sxs-lookup"><span data-stu-id="6afb6-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="6afb6-111">Мы будет необходимо зарегистрировать в наш сайт перед извлечением, поэтому этот контроллер будет требовать авторизации.</span><span class="sxs-lookup"><span data-stu-id="6afb6-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="6afb6-112">Пользователи, чтобы перейти процесс подсчета стоимости покупок в корзину, нажав кнопку «Извлечь».</span><span class="sxs-lookup"><span data-stu-id="6afb6-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="6afb6-113">Если пользователь не вошел, им будет предложено.</span><span class="sxs-lookup"><span data-stu-id="6afb6-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="6afb6-114">После успешного входа пользователь затем отображается представление адрес и оплаты.</span><span class="sxs-lookup"><span data-stu-id="6afb6-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="6afb6-115">Как только они заполнения формы и подтверждением заказа, они будут показаны экране подтверждения заказа.</span><span class="sxs-lookup"><span data-stu-id="6afb6-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="6afb6-116">Попытка просмотреть порядок несуществующий или заказа, не принадлежит покажет представлении ошибок.</span><span class="sxs-lookup"><span data-stu-id="6afb6-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="6afb6-117">Миграция в корзину для покупок</span><span class="sxs-lookup"><span data-stu-id="6afb6-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="6afb6-118">Хотя процесс покупки анонимен, когда пользователь нажимает кнопку извлечения, ему придется пройти для регистрации и входа.</span><span class="sxs-lookup"><span data-stu-id="6afb6-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="6afb6-119">Пользователи ожидают, что мы будет поддерживать их о корзине покупок посещениях, поэтому будет необходимо связать с пользователем о корзине покупок, после завершения регистрации или входа.</span><span class="sxs-lookup"><span data-stu-id="6afb6-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="6afb6-120">Это фактически очень легко сделать, так как наш класс ShoppingCart уже есть метод, который будет связывать все элементы в текущем корзины для покупок с именем пользователя.</span><span class="sxs-lookup"><span data-stu-id="6afb6-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="6afb6-121">Нам просто нужно вызвать этот метод, когда пользователь завершает регистрации или входа.</span><span class="sxs-lookup"><span data-stu-id="6afb6-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="6afb6-122">Откройте **AccountController** класс, который мы добавили, настраивая мы членство и авторизация.</span><span class="sxs-lookup"><span data-stu-id="6afb6-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="6afb6-123">Добавить с помощью инструкции, ссылающиеся на MvcMusicStore.Models, затем добавьте следующий метод MigrateShoppingCart:</span><span class="sxs-lookup"><span data-stu-id="6afb6-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="6afb6-124">Затем измените действия после входа в систему для вызова MigrateShoppingCart после проверки пользователя, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="6afb6-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="6afb6-125">Сделать то же изменение в регистр действий, выполняемых после, сразу после успешного создания учетной записи пользователя:</span><span class="sxs-lookup"><span data-stu-id="6afb6-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="6afb6-126">Вот и все — теперь анонимный корзину будут автоматически передаваться учетную запись пользователя после успешной регистрации или входа.</span><span class="sxs-lookup"><span data-stu-id="6afb6-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="6afb6-127">Создание CheckoutController</span><span class="sxs-lookup"><span data-stu-id="6afb6-127">Creating the CheckoutController</span></span>

<span data-ttu-id="6afb6-128">Щелкните правой кнопкой мыши папку Controllers и добавите новый контроллер в проект с именем CheckoutController, с помощью шаблона пустой контроллер.</span><span class="sxs-lookup"><span data-stu-id="6afb6-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="6afb6-129">Во-первых добавьте атрибут Authorize над объявлением класса контроллера требуется использовать Зарегистрируйтесь до извлечения.</span><span class="sxs-lookup"><span data-stu-id="6afb6-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="6afb6-130">*Примечание: Это похоже на изменение, которое мы ранее сделанные для StoreManagerController, но в этом случае атрибут Authorize требуется, чтобы пользователь был в роль администратора. В контроллере извлечения, мы при необходимости пользователю войти систему, но не требуется, чтобы они были администраторов.*</span><span class="sxs-lookup"><span data-stu-id="6afb6-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="6afb6-131">Для простоты мы не будет иметь дело с платежной информации в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="6afb6-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="6afb6-132">Вместо этого мы позволяем пользователям извлекать, указав код акции.</span><span class="sxs-lookup"><span data-stu-id="6afb6-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="6afb6-133">Мы сохраним этот код акции, с помощью константу с именем Промокода.</span><span class="sxs-lookup"><span data-stu-id="6afb6-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="6afb6-134">Как и в StoreController я объявлю поле, содержащее экземпляр класса MusicStoreEntities, с именем storeDB.</span><span class="sxs-lookup"><span data-stu-id="6afb6-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="6afb6-135">Чтобы использовать класс MusicStoreEntities, нам потребуется добавить с помощью инструкции для пространства имен MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="6afb6-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="6afb6-136">Появляется вверху нашего контроллера для выписки.</span><span class="sxs-lookup"><span data-stu-id="6afb6-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="6afb6-137">CheckoutController будет иметь следующее контроллера:</span><span class="sxs-lookup"><span data-stu-id="6afb6-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="6afb6-138">**(Метод GET) AddressAndPayment** форма позволяет пользователю ввести данные.</span><span class="sxs-lookup"><span data-stu-id="6afb6-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="6afb6-139">**(Метод POST) AddressAndPayment** проверяет входные данные и обработки заказа.</span><span class="sxs-lookup"><span data-stu-id="6afb6-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="6afb6-140">**Полный** будет отображаться после пользователь успешно завершил процесс подсчета стоимости сначала.</span><span class="sxs-lookup"><span data-stu-id="6afb6-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="6afb6-141">В этом представлении будет включать номер заказа пользователя, в качестве подтверждения.</span><span class="sxs-lookup"><span data-stu-id="6afb6-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="6afb6-142">Во-первых для AddressAndPayment переименуем действия контроллера индекса (который был создан при создании контроллера).</span><span class="sxs-lookup"><span data-stu-id="6afb6-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="6afb6-143">Это действие контроллера просто отображает форму извлечения, чтобы он не требовал любые сведения о модели.</span><span class="sxs-lookup"><span data-stu-id="6afb6-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="6afb6-144">Наш метод AddressAndPayment POST будет тот же шаблон, мы использовали в StoreManagerController: он будет пытаться принять отправки формы и завершить порядок и повторного отображения формы в случае неудачи.</span><span class="sxs-lookup"><span data-stu-id="6afb6-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="6afb6-145">После проверки входных данных формы отвечает требованиям наших проверки заказа, проверяется значение формы промокод напрямую.</span><span class="sxs-lookup"><span data-stu-id="6afb6-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="6afb6-146">Предположим, что все работает правильно, мы сохраним обновленную информацию с заказом, заставить объект ShoppingCart выполнить процесс заказа и перенаправления для завершения действия.</span><span class="sxs-lookup"><span data-stu-id="6afb6-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="6afb6-147">После успешного завершения процесса извлечения пользователи будут перенаправляться к действию полный контроллера.</span><span class="sxs-lookup"><span data-stu-id="6afb6-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="6afb6-148">Это действие будет выполнять простую проверку, чтобы проверить, что порядок действительно принадлежат вошедшего в систему пользователя перед отображением в качестве подтверждения номер заказа.</span><span class="sxs-lookup"><span data-stu-id="6afb6-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="6afb6-149">*Примечание: Представлении ошибок был автоматически создан для нас в папке /Views/Shared когда мы начали работать проект.*</span><span class="sxs-lookup"><span data-stu-id="6afb6-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="6afb6-150">Полный код CheckoutController выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6afb6-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="6afb6-151">Добавление представления AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="6afb6-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="6afb6-152">Теперь давайте создадим AddressAndPayment представления.</span><span class="sxs-lookup"><span data-stu-id="6afb6-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="6afb6-153">Щелкните правой кнопкой мыши на одно из действий контроллера AddressAndPayment и добавьте представление с именем AddressAndPayment, строго типизируется как заказ и используется изменить шаблон, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6afb6-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="6afb6-154">В этом представлении сделает использовать из двух методов, мы рассмотрели при построении StoreManagerEdit представления:</span><span class="sxs-lookup"><span data-stu-id="6afb6-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="6afb6-155">Мы будем использовать Html.EditorForModel() для отображения поля формы для модели заказа</span><span class="sxs-lookup"><span data-stu-id="6afb6-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="6afb6-156">Мы будем использовать правила проверки с помощью класса Order с атрибутами проверки</span><span class="sxs-lookup"><span data-stu-id="6afb6-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="6afb6-157">Мы начнем, обновив код формы для использования Html.EditorForModel(), следуют дополнительные textbox для промо-код.</span><span class="sxs-lookup"><span data-stu-id="6afb6-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="6afb6-158">Ниже приведен полный код для представления AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="6afb6-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="6afb6-159">Определение правила проверки для заказа</span><span class="sxs-lookup"><span data-stu-id="6afb6-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="6afb6-160">Теперь, когда нашего представления настроена, мы настроим правила проверки для нашей модели порядок как это делалось ранее для модели альбома.</span><span class="sxs-lookup"><span data-stu-id="6afb6-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="6afb6-161">Щелкните правой кнопкой мыши папку Models и добавьте класс с именем заказа.</span><span class="sxs-lookup"><span data-stu-id="6afb6-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="6afb6-162">Помимо атрибутов проверки, которые мы использовали ранее альбома мы также используем регулярное выражение для проверки адреса электронной почты пользователя.</span><span class="sxs-lookup"><span data-stu-id="6afb6-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="6afb6-163">Попытка отправить форму с отсутствующим или недопустимых данных, появится сообщение об ошибке с помощью проверки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="6afb6-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="6afb6-164">Итак мы выполнили большинство часть тяжелой работы для процесса извлечения; Мы просто выбрали несколько заканчивается и вероятность завершения.</span><span class="sxs-lookup"><span data-stu-id="6afb6-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="6afb6-165">Нам нужно добавить два простых представления, а нам нужно позаботиться о получения информации для покупок, при входе в систему.</span><span class="sxs-lookup"><span data-stu-id="6afb6-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="6afb6-166">Добавление завершения извлечения представления</span><span class="sxs-lookup"><span data-stu-id="6afb6-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="6afb6-167">Извлечение полного представления довольно прост, так как он просто должен отображать номер заказа.</span><span class="sxs-lookup"><span data-stu-id="6afb6-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="6afb6-168">Щелкните правой кнопкой мыши на действие завершения контроллера и добавить представление с именем завершить, который является строго типизированным как целочисленное значение</span><span class="sxs-lookup"><span data-stu-id="6afb6-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="6afb6-169">Теперь мы будем обновлять код представления для отображения идентификатор заказа, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6afb6-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="6afb6-170">Обновление представления ошибки</span><span class="sxs-lookup"><span data-stu-id="6afb6-170">Updating The Error view</span></span>

<span data-ttu-id="6afb6-171">Шаблон по умолчанию включает в себя представление "ошибок" в папке views Shared таким образом, чтобы его можно использовать повторно в другом месте на сайте.</span><span class="sxs-lookup"><span data-stu-id="6afb6-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="6afb6-172">В этом представлении ошибка содержит ошибку очень простой и не использует наш сайт макета, поэтому мы обновим его.</span><span class="sxs-lookup"><span data-stu-id="6afb6-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="6afb6-173">Так как это типовой страницы ошибки, содержимое очень прост.</span><span class="sxs-lookup"><span data-stu-id="6afb6-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="6afb6-174">Мы будем включать сообщение и ссылку для перехода на предыдущую страницу в журнале, если пользователь хочет повторить попытку их действия.</span><span class="sxs-lookup"><span data-stu-id="6afb6-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> <span data-ttu-id="6afb6-175">[Назад](mvc-music-store-part-8.md)
> [Вперед](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="6afb6-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
