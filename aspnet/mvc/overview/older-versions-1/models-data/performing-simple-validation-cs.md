---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: Выполнение простой проверки (C#) | Документация Майкрософт
author: StephenWalther
description: Узнайте, как для выполнения проверки в приложении ASP.NET MVC. В этом руководстве Стивен Вальтер вводит вы состояние модели, а также вспомогательные проверки HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 622d4bc40b2f403c0a4efe0ee799a68f1adbe4fd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377419"
---
<a name="performing-simple-validation-c"></a><span data-ttu-id="68402-104">Выполнение простой проверки (C#)</span><span class="sxs-lookup"><span data-stu-id="68402-104">Performing Simple Validation (C#)</span></span>
====================
<span data-ttu-id="68402-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="68402-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="68402-106">Узнайте, как для выполнения проверки в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="68402-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="68402-107">В этом руководстве Стивен Вальтер вводит для состояния модели и вспомогательные функции проверки HTML.</span><span class="sxs-lookup"><span data-stu-id="68402-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="68402-108">Этот учебник призван объяснить, как выполнять проверки в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="68402-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="68402-109">Например вы узнаете, как запретить кто-то отправка формы, который не содержит значение для обязательного поля.</span><span class="sxs-lookup"><span data-stu-id="68402-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="68402-110">Вы узнаете, как использовать состояние модели и вспомогательных методов HTML проверки.</span><span class="sxs-lookup"><span data-stu-id="68402-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="68402-111">Общие сведения о состоянии модели</span><span class="sxs-lookup"><span data-stu-id="68402-111">Understanding Model State</span></span>

<span data-ttu-id="68402-112">Состояние модели — или, точнее, в словарь состояния модели — использовать для представления ошибок проверки.</span><span class="sxs-lookup"><span data-stu-id="68402-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="68402-113">Например действие Create() в листинге 1 проверяет свойства класса Product перед добавлением класс Product в базе данных.</span><span class="sxs-lookup"><span data-stu-id="68402-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="68402-114">Я не рекомендую что нужно добавить логику проверки или базы данных, к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="68402-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="68402-115">Контроллер должен содержать только логику, относящуюся к управляют потоком данных приложения.</span><span class="sxs-lookup"><span data-stu-id="68402-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="68402-116">Мы говорим об ярлык для простоты.</span><span class="sxs-lookup"><span data-stu-id="68402-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="68402-117">**В листинге 1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="68402-117">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

<span data-ttu-id="68402-118">В листинге 1 проверяются имя, описание и UnitsInStock свойства класса продукта.</span><span class="sxs-lookup"><span data-stu-id="68402-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="68402-119">Если любое из этих свойств не прошли проверку ошибка добавляется в словарь состояния модели (представленного свойством ModelState класса Controller).</span><span class="sxs-lookup"><span data-stu-id="68402-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="68402-120">Если имеются ошибки в состояние модели, свойству ModelState.IsValid возвращает false.</span><span class="sxs-lookup"><span data-stu-id="68402-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="68402-121">В этом случае отобразится форма HTML для создания нового продукта.</span><span class="sxs-lookup"><span data-stu-id="68402-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="68402-122">В противном случае при возникновении ошибки проверки, добавляется новый продукт в базу данных.</span><span class="sxs-lookup"><span data-stu-id="68402-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="68402-123">Используя вспомогательные функции проверки</span><span class="sxs-lookup"><span data-stu-id="68402-123">Using the Validation Helpers</span></span>

<span data-ttu-id="68402-124">Платформа ASP.NET MVC включает в себя две вспомогательные функции проверки: Html.ValidationMessage() вспомогательного приложения, а также вспомогательные Html.ValidationSummary().</span><span class="sxs-lookup"><span data-stu-id="68402-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="68402-125">Используйте эти две вспомогательные функции в виде для отображения сообщений об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="68402-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="68402-126">Вспомогательные функции Html.ValidationMessage() и Html.ValidationSummary() используются в представлениях Create и Edit, автоматически создаваемых формирование шаблонов ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="68402-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="68402-127">Выполните следующие действия для создания представления создания.</span><span class="sxs-lookup"><span data-stu-id="68402-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="68402-128">Щелкните правой кнопкой мыши действие Create() в контроллере продукта и выберите пункт меню **Добавление представления** (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="68402-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="68402-129">В **Добавление представления** диалоговом окне установите флажок "с меткой" **создать строго типизированное представление** (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="68402-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="68402-130">Из **просмотреть класс данных** раскрывающегося списка выберите класс Product.</span><span class="sxs-lookup"><span data-stu-id="68402-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="68402-131">Из **просматривать содержимое** раскрывающемся списке выберите создать.</span><span class="sxs-lookup"><span data-stu-id="68402-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="68402-132">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="68402-132">Click the **Add** button.</span></span>


<span data-ttu-id="68402-133">Убедитесь, что сборка приложения осуществляется перед добавлением представления.</span><span class="sxs-lookup"><span data-stu-id="68402-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="68402-134">В противном случае список классов не будет отображаться на **просмотреть класс данных** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="68402-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="68402-135">[![В диалоговом окне нового проекта](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="68402-135">[![The New Project dialog box](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span></span>

<span data-ttu-id="68402-136">**Рис 01**: Добавление представления ([Просмотр полноразмерного изображения](performing-simple-validation-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="68402-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-cs/_static/image2.png))</span></span>


<span data-ttu-id="68402-137">[![В диалоговом окне нового проекта](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="68402-137">[![The New Project dialog box](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span></span>

<span data-ttu-id="68402-138">**Рис. 02**: создание строго типизированное представление ([Просмотр полноразмерного изображения](performing-simple-validation-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="68402-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-cs/_static/image4.png))</span></span>


<span data-ttu-id="68402-139">После выполнения этих действий вы получаете представления создания в листинге 2.</span><span class="sxs-lookup"><span data-stu-id="68402-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="68402-140">**В листинге 2 - Views\Product\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="68402-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

<span data-ttu-id="68402-141">В листинге 2 Html.ValidationSummary() вспомогательного приложения сразу же вызывается выше HTML-формы.</span><span class="sxs-lookup"><span data-stu-id="68402-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="68402-142">Этого вспомогательного объекта используется для отображения списка сообщений об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="68402-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="68402-143">Вспомогательный метод Html.ValidationSummary() отображает ошибки в виде маркированного списка.</span><span class="sxs-lookup"><span data-stu-id="68402-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="68402-144">Вспомогательный метод Html.ValidationMessage() называется рядом с каждым из полей формы HTML.</span><span class="sxs-lookup"><span data-stu-id="68402-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="68402-145">Этого вспомогательного объекта используется для отображения сообщения об ошибке, рядом с полем формы.</span><span class="sxs-lookup"><span data-stu-id="68402-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="68402-146">В случае листинга 2 вспомогательный Html.ValidationMessage() отображается звездочка, когда возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="68402-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="68402-147">Страница, на рис. 3 показано сообщения об ошибках, преобразовываемый вспомогательные функции проверки при отправке формы с отсутствующих полей и недопустимые значения.</span><span class="sxs-lookup"><span data-stu-id="68402-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="68402-148">[![В диалоговом окне нового проекта](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="68402-148">[![The New Project dialog box](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span></span>

<span data-ttu-id="68402-149">**Рис 03**: The Create view, отправленных с проблемами ([Просмотр полноразмерного изображения](performing-simple-validation-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="68402-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-cs/_static/image6.png))</span></span>


<span data-ttu-id="68402-150">Обратите внимание, внешний вид HTML входные данные, что поля также изменяются при ошибке проверки.</span><span class="sxs-lookup"><span data-stu-id="68402-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="68402-151">Отображает вспомогательные Html.TextBox() *класс = «ошибка ввода проверки»* атрибута при ошибке проверки, связанного со свойством, преобразовываемый для просмотра Html.TextBox() вспомогательное приложение.</span><span class="sxs-lookup"><span data-stu-id="68402-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="68402-152">Существует три класса каскадных таблицы стилей стиль, используемый для управления внешним видом ошибки проверки:</span><span class="sxs-lookup"><span data-stu-id="68402-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="68402-153">входные данные —-ошибка проверки — применяется к &lt;ввода&gt; к просмотру Html.TextBox() вспомогательной функцией тега.</span><span class="sxs-lookup"><span data-stu-id="68402-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="68402-154">поле--Ошибка проверки — применяется к &lt;span&gt; к просмотру Html.ValidationMessage() вспомогательной функцией тега.</span><span class="sxs-lookup"><span data-stu-id="68402-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="68402-155">Сводка ошибки проверки - применяется к &lt;ul&gt; к просмотру Html.ValidationSumamry() вспомогательной функцией тега.</span><span class="sxs-lookup"><span data-stu-id="68402-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="68402-156">Можно изменить эти каскадные таблицы стилей классы стилей и таким образом изменить внешний вид ошибки проверки, изменив файл Site.css, расположенный в папке Content.</span><span class="sxs-lookup"><span data-stu-id="68402-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="68402-157">Класс HtmlHelper включает в себя статических свойств только для чтения для CSS, получение имен проверки связанных классов.</span><span class="sxs-lookup"><span data-stu-id="68402-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="68402-158">Эти статические свойства называются ValidationInputCssClassName ValidationFieldCssClassName и ValidationSummaryCssClassName.</span><span class="sxs-lookup"><span data-stu-id="68402-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="68402-159">Prebinding проверки и проверки Postbinding</span><span class="sxs-lookup"><span data-stu-id="68402-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="68402-160">Если введено недопустимое значение для поля Цена и нет значения для поля UnitsInStock отправки формы HTML для создания продукта, вы получите сообщения проверки, отображаемый на рис. 4.</span><span class="sxs-lookup"><span data-stu-id="68402-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="68402-161">Откуда берутся эти сообщения об ошибках проверки?</span><span class="sxs-lookup"><span data-stu-id="68402-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="68402-162">[![В диалоговом окне нового проекта](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="68402-162">[![The New Project dialog box](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span></span>

<span data-ttu-id="68402-163">**Рис. 04**: ошибок проверки Prebinding ([Просмотр полноразмерного изображения](performing-simple-validation-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="68402-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-cs/_static/image8.png))</span></span>


<span data-ttu-id="68402-164">Существует фактически используется два типа сообщений об ошибках проверки - созданные перед поля формы HTML привязаны к классу и их сформировано после поля формы привязываются к классу.</span><span class="sxs-lookup"><span data-stu-id="68402-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="68402-165">Другими словами, есть prebinding ошибки проверки и postbinding ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="68402-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="68402-166">Create() действию, предоставляемым контроллером продукта в листинге 1 принимает экземпляр класса Product.</span><span class="sxs-lookup"><span data-stu-id="68402-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="68402-167">Подпись метода Create выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="68402-167">The signature of the Create method looks like this:</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

<span data-ttu-id="68402-168">Значения поля формы HTML в форме создать привязаны к классу productToCreate благодаря существованию так называемых связыватель модели.</span><span class="sxs-lookup"><span data-stu-id="68402-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="68402-169">Связыватель модели по умолчанию добавляет сообщение об ошибке состояние модели автоматически при ее не удается привязать поле формы со свойством формы.</span><span class="sxs-lookup"><span data-stu-id="68402-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="68402-170">Связыватель модели по умолчанию невозможно привязать свойство цена продукта класса строка «apple».</span><span class="sxs-lookup"><span data-stu-id="68402-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="68402-171">Строка не может назначить свойства десятичного типа.</span><span class="sxs-lookup"><span data-stu-id="68402-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="68402-172">Таким образом связыватель модели добавляет ошибку состояния модели.</span><span class="sxs-lookup"><span data-stu-id="68402-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="68402-173">Связыватель модели по умолчанию также невозможно присвоить значение null к свойству, которое не допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="68402-173">The default model binder also cannot assign a null value to a property that does not accept nulls.</span></span> <span data-ttu-id="68402-174">В частности связыватель модели не удается присвоить значение null UnitsInStock свойству.</span><span class="sxs-lookup"><span data-stu-id="68402-174">In particular, the model binder cannot assign a null value to the UnitsInStock property.</span></span> <span data-ttu-id="68402-175">Опять же связыватель модели отдает и добавляет сообщение об ошибке в состояние модели.</span><span class="sxs-lookup"><span data-stu-id="68402-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="68402-176">Если вы хотите настроить внешний вид этих prebinding сообщения об ошибках, необходимо создать ресурс строки для этих сообщений.</span><span class="sxs-lookup"><span data-stu-id="68402-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="68402-177">Сводка</span><span class="sxs-lookup"><span data-stu-id="68402-177">Summary</span></span>

<span data-ttu-id="68402-178">Цель этого учебника было описаны основные принципы работы проверки в ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="68402-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="68402-179">Вы узнали, как использовать состояние модели и вспомогательных методов HTML проверки.</span><span class="sxs-lookup"><span data-stu-id="68402-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="68402-180">Мы также рассмотрели различие между prebinding и postbinding проверки.</span><span class="sxs-lookup"><span data-stu-id="68402-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="68402-181">В других руководствах мы обсудим различные стратегии перемещения код проверки из контроллеров в классах модели.</span><span class="sxs-lookup"><span data-stu-id="68402-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="68402-182">[Назад](displaying-a-table-of-database-data-cs.md)
> [Вперед](validating-with-the-idataerrorinfo-interface-cs.md)</span><span class="sxs-lookup"><span data-stu-id="68402-182">[Previous](displaying-a-table-of-database-data-cs.md)
[Next](validating-with-the-idataerrorinfo-interface-cs.md)</span></span>
