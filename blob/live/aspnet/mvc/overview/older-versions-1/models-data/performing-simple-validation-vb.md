---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: "Выполняет простую проверку (VB) | Документы Microsoft"
author: StephenWalther
description: "Узнайте, как выполнять проверку в приложении ASP.NET MVC. В этом учебнике Стивен Вальтер вводит вы состояние модели и вспомогательный класс проверки HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bc4cdbcd267bcdd3e71abc4c52664ae62c5c02e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="performing-simple-validation-vb"></a><span data-ttu-id="17b0d-104">Выполняет простую проверку (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="17b0d-104">Performing Simple Validation (VB)</span></span>
====================
<span data-ttu-id="17b0d-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="17b0d-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="17b0d-106">Узнайте, как выполнять проверку в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="17b0d-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="17b0d-107">В этом учебнике Стивен Вальтер вводит для состояния модели и вспомогательные методы проверки HTML.</span><span class="sxs-lookup"><span data-stu-id="17b0d-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="17b0d-108">Целью данного учебника является объясняется, как можно выполнить проверку в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="17b0d-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="17b0d-109">Например вы научитесь допустить Отправка формы, который не содержит значение для обязательного поля.</span><span class="sxs-lookup"><span data-stu-id="17b0d-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="17b0d-110">Вы узнаете, как использовать состояние модели и вспомогательных методов HTML проверки.</span><span class="sxs-lookup"><span data-stu-id="17b0d-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="17b0d-111">Общие сведения о состоянии модели</span><span class="sxs-lookup"><span data-stu-id="17b0d-111">Understanding Model State</span></span>

<span data-ttu-id="17b0d-112">Используйте состояние модели - или более точно в словарь состояния модели — для представления ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="17b0d-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="17b0d-113">Например действие Create() в список 1 проверяет свойства класса Product перед добавлением класс продукта в базе данных.</span><span class="sxs-lookup"><span data-stu-id="17b0d-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="17b0d-114">Я не рекомендую добавить логику проверки или базы данных, к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="17b0d-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="17b0d-115">Контроллер должен содержать только процедуры, относящиеся к управления потоком приложения.</span><span class="sxs-lookup"><span data-stu-id="17b0d-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="17b0d-116">Мы воспользуемся ярлык для простоты.</span><span class="sxs-lookup"><span data-stu-id="17b0d-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="17b0d-117">**Листинг 1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="17b0d-117">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

<span data-ttu-id="17b0d-118">В список 1 проверяются имя, описание и UnitsInStock свойства класса продукта.</span><span class="sxs-lookup"><span data-stu-id="17b0d-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="17b0d-119">Если эти свойства не проходят проверку коллекцию ошибок добавляется в словарь состояния модели (представленные свойством ModelState класса контроллера).</span><span class="sxs-lookup"><span data-stu-id="17b0d-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="17b0d-120">Если имеются ошибки в состояние модели ModelState.IsValid свойство возвращает значение false.</span><span class="sxs-lookup"><span data-stu-id="17b0d-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="17b0d-121">В этом случае отобразится форма HTML для создания нового продукта.</span><span class="sxs-lookup"><span data-stu-id="17b0d-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="17b0d-122">В противном случае — в случае ошибки проверки не нового продукта добавляется в базу данных.</span><span class="sxs-lookup"><span data-stu-id="17b0d-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="17b0d-123">Использование вспомогательных методов проверки</span><span class="sxs-lookup"><span data-stu-id="17b0d-123">Using the Validation Helpers</span></span>

<span data-ttu-id="17b0d-124">Платформа ASP.NET MVC включает две вспомогательные методы проверки: Html.ValidationMessage() поддержки и вспомогательные Html.ValidationSummary().</span><span class="sxs-lookup"><span data-stu-id="17b0d-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="17b0d-125">Используйте эти две вспомогательные функции в представлении для отображения сообщений об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="17b0d-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="17b0d-126">Создание и изменение представления, которые автоматически формируются формирование шаблонов ASP.NET MVC используются вспомогательные методы Html.ValidationMessage() и Html.ValidationSummary().</span><span class="sxs-lookup"><span data-stu-id="17b0d-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="17b0d-127">Выполните следующие действия для создания представления создания.</span><span class="sxs-lookup"><span data-stu-id="17b0d-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="17b0d-128">Щелкните правой кнопкой мыши действие Create() в контроллере продукта и выбрать пункт меню **добавить представление** (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="17b0d-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="17b0d-129">В **добавить представление** диалогового окна, установите флажок "с меткой" **создать строго типизированное представление** (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="17b0d-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="17b0d-130">Из **просматривать данные класс** раскрывающегося списка выберите класс продукта.</span><span class="sxs-lookup"><span data-stu-id="17b0d-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="17b0d-131">Из **просматривать содержимое** раскрывающийся список, выберите создать.</span><span class="sxs-lookup"><span data-stu-id="17b0d-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="17b0d-132">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="17b0d-132">Click the **Add** button.</span></span>


<span data-ttu-id="17b0d-133">Убедитесь, что сборка приложения перед добавлением представления.</span><span class="sxs-lookup"><span data-stu-id="17b0d-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="17b0d-134">В противном случае не будет отображаться в список классов **просматривать данные класс** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="17b0d-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="17b0d-135">[![Диалоговое окно нового проекта](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="17b0d-135">[![The New Project dialog box](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)</span></span>

<span data-ttu-id="17b0d-136">**На рисунке 01**: Добавление представления ([Просмотр полноразмерное изображение](performing-simple-validation-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="17b0d-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-vb/_static/image2.png))</span></span>


<span data-ttu-id="17b0d-137">[![Диалоговое окно нового проекта](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="17b0d-137">[![The New Project dialog box](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)</span></span>

<span data-ttu-id="17b0d-138">**На рисунке 02**: создание строго типизированное представление ([Просмотр полноразмерное изображение](performing-simple-validation-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="17b0d-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-vb/_static/image4.png))</span></span>


<span data-ttu-id="17b0d-139">После выполнения этих действий вы получаете создать представление в списке 2.</span><span class="sxs-lookup"><span data-stu-id="17b0d-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="17b0d-140">**Листинг 2 - Views\Product\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="17b0d-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

<span data-ttu-id="17b0d-141">В списке 2 вспомогательный Html.ValidationSummary() вызывается немедленно выше HTML-формы.</span><span class="sxs-lookup"><span data-stu-id="17b0d-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="17b0d-142">Этого вспомогательного объекта используется для отображения списка сообщений об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="17b0d-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="17b0d-143">Вспомогательный объект Html.ValidationSummary() отображает ошибки в виде маркированного списка.</span><span class="sxs-lookup"><span data-stu-id="17b0d-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="17b0d-144">Вспомогательный объект Html.ValidationMessage() называется радом с каждым из полей формы HTML.</span><span class="sxs-lookup"><span data-stu-id="17b0d-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="17b0d-145">Этого вспомогательного объекта используется для отображения сообщения об ошибке, рядом с полем формы.</span><span class="sxs-lookup"><span data-stu-id="17b0d-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="17b0d-146">В случае листинг 2 вспомогательный Html.ValidationMessage() отображается звездочка, при наличии ошибки.</span><span class="sxs-lookup"><span data-stu-id="17b0d-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="17b0d-147">Страницы на рисунке 3 показано сообщения об ошибках, отрисовываемый помощников проверки при отправке формы с отсутствующих полей и недопустимые значения.</span><span class="sxs-lookup"><span data-stu-id="17b0d-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="17b0d-148">[![Диалоговое окно нового проекта](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="17b0d-148">[![The New Project dialog box](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)</span></span>

<span data-ttu-id="17b0d-149">**На рисунке 03**: Create view, отправленные с проблемами ([Просмотр полноразмерное изображение](performing-simple-validation-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="17b0d-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-vb/_static/image6.png))</span></span>


<span data-ttu-id="17b0d-150">Обратите внимание на внешний вид HTML введенных поля также изменяются при ошибке проверки.</span><span class="sxs-lookup"><span data-stu-id="17b0d-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="17b0d-151">Отображает вспомогательные Html.TextBox() *класс = «ошибка ввода проверки»* атрибут при ошибке проверки, связанное со свойством к просмотру Html.TextBox() вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="17b0d-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="17b0d-152">Имеется три класса каскадные таблицы стиля, используется для управления внешним видом ошибки проверки:</span><span class="sxs-lookup"><span data-stu-id="17b0d-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="17b0d-153">входные данные — проверка ошибка — применить к &lt;ввода&gt; тег, представляемый Html.TextBox() вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="17b0d-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="17b0d-154">поле ошибке проверки - применены к &lt;span&gt; тег, представляемый Html.ValidationMessage() вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="17b0d-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="17b0d-155">Сводка ошибки проверки - применены к &lt;ul&gt; тег, представляемый Html.ValidationSumamry() вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="17b0d-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="17b0d-156">Можно изменить эти каскадные таблицы классы стилей и таким образом изменить внешний вид ошибки проверки, изменив файл Site.css, расположенный в папке содержимого.</span><span class="sxs-lookup"><span data-stu-id="17b0d-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="17b0d-157">Класс HtmlHelper включает только для чтения статических свойств CSS, получение имен проверки связанных классов.</span><span class="sxs-lookup"><span data-stu-id="17b0d-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="17b0d-158">Эти статические свойства называются ValidationInputCssClassName, ValidationFieldCssClassName и ValidationSummaryCssClassName.</span><span class="sxs-lookup"><span data-stu-id="17b0d-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="17b0d-159">Prebinding проверки и проверки Postbinding</span><span class="sxs-lookup"><span data-stu-id="17b0d-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="17b0d-160">Если введено недопустимое значение в поле «Цена» и значение для поля UnitsInStock отправки формы HTML для создания продукта, вы сможете получить проверки сообщений, отображаемых на рис. 4.</span><span class="sxs-lookup"><span data-stu-id="17b0d-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="17b0d-161">Откуда берутся эти сообщения об ошибках проверки?</span><span class="sxs-lookup"><span data-stu-id="17b0d-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="17b0d-162">[![Диалоговое окно нового проекта](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="17b0d-162">[![The New Project dialog box](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)</span></span>

<span data-ttu-id="17b0d-163">**На рисунке 04**: ошибок проверки Prebinding ([Просмотр полноразмерное изображение](performing-simple-validation-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="17b0d-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-vb/_static/image8.png))</span></span>


<span data-ttu-id="17b0d-164">Существует фактически два типа сообщений об ошибках проверки - созданные перед поля формы HTML привязаны к классу и те, созданных после поля формы привязаны к классу.</span><span class="sxs-lookup"><span data-stu-id="17b0d-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="17b0d-165">Другими словами, имеются ошибки проверки prebinding и postbinding ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="17b0d-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="17b0d-166">Create() действию, предоставляемым контроллером продукта в листинге 1 принимает экземпляр класса Product.</span><span class="sxs-lookup"><span data-stu-id="17b0d-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="17b0d-167">Сигнатура метода создания выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="17b0d-167">The signature of the Create method looks like this:</span></span>

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

<span data-ttu-id="17b0d-168">Значения поля формы HTML в форме создания привязаны к productToCreate класса, называемое связыватель модели.</span><span class="sxs-lookup"><span data-stu-id="17b0d-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="17b0d-169">Связыватель модели по умолчанию добавляет сообщение об ошибке состояния модели автоматически при его невозможно привязать поле формы со свойством формы.</span><span class="sxs-lookup"><span data-stu-id="17b0d-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="17b0d-170">Связыватель модели по умолчанию не удается привязать свойство цена продукта класса строку «apple».</span><span class="sxs-lookup"><span data-stu-id="17b0d-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="17b0d-171">Строка не может назначить свойства десятичного типа.</span><span class="sxs-lookup"><span data-stu-id="17b0d-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="17b0d-172">Таким образом связывателя модели добавляет ошибку в состояние модели.</span><span class="sxs-lookup"><span data-stu-id="17b0d-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="17b0d-173">Связыватель модели по умолчанию также не удается присвоить значение Nothing свойство, которое не принимает значение Nothing.</span><span class="sxs-lookup"><span data-stu-id="17b0d-173">The default model binder also cannot assign the value Nothing to a property that does not accept the value Nothing.</span></span> <span data-ttu-id="17b0d-174">В частности связывателя модели нельзя назначить значение Nothing UnitsInStock свойство.</span><span class="sxs-lookup"><span data-stu-id="17b0d-174">In particular, the model binder cannot assign the value Nothing to the UnitsInStock property.</span></span> <span data-ttu-id="17b0d-175">Опять же связывателя модели отдает и добавляет сообщение об ошибке состояния модели.</span><span class="sxs-lookup"><span data-stu-id="17b0d-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="17b0d-176">Если вы хотите настроить внешний вид этих prebinding сообщения об ошибках, необходимо создать ресурс строки для этих сообщений.</span><span class="sxs-lookup"><span data-stu-id="17b0d-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="17b0d-177">Сводка</span><span class="sxs-lookup"><span data-stu-id="17b0d-177">Summary</span></span>

<span data-ttu-id="17b0d-178">Целью данного учебника было описаны основные принципы работы проверки в ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="17b0d-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="17b0d-179">Вы узнали, как использовать состояние модели и вспомогательных методов HTML проверки.</span><span class="sxs-lookup"><span data-stu-id="17b0d-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="17b0d-180">Также мы рассмотрели различие между prebinding и postbinding проверки.</span><span class="sxs-lookup"><span data-stu-id="17b0d-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="17b0d-181">В других учебниках мы обсудим различные стратегии перемещения код проверки из контроллеров в классами модели.</span><span class="sxs-lookup"><span data-stu-id="17b0d-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="17b0d-182">[Назад](displaying-a-table-of-database-data-vb.md)
[Вперед](validating-with-the-idataerrorinfo-interface-vb.md)</span><span class="sxs-lookup"><span data-stu-id="17b0d-182">[Previous](displaying-a-table-of-database-data-vb.md)
[Next](validating-with-the-idataerrorinfo-interface-vb.md)</span></span>
