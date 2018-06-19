---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: Проверка с помощью средства проверки данных заметки (C#) | Документы Microsoft
author: microsoft
description: Воспользуйтесь преимуществами связывателя модели данных заметок для выполнения проверки в приложении ASP.NET MVC. Сведения об использовании различных типов проверяющий элемент управления...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 0aca9472094e6a54c7b7cb4ad4f12df64fe12db2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870197"
---
<a name="validation-with-the-data-annotation-validators-c"></a><span data-ttu-id="4102c-104">Проверка с помощью средства проверки данных заметки (C#)</span><span class="sxs-lookup"><span data-stu-id="4102c-104">Validation with the Data Annotation Validators (C#)</span></span>
====================
<span data-ttu-id="4102c-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="4102c-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="4102c-106">Воспользуйтесь преимуществами связывателя модели данных заметок для выполнения проверки в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4102c-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="4102c-107">Узнайте, как использовать различные типы атрибутов, проверяющий элемент управления и работы с ними в Entity Framework корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="4102c-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="4102c-108">В этом учебнике вы узнаете, как использовать заметки к данным проверяющие элементы управления для выполнения проверки в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4102c-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="4102c-109">Преимуществом использования проверяющие элементы управления заметки данных является, что они позволяют выполнять проверку, просто добавив один или несколько атрибутов — например, необходимая или атрибут StringLength — свойства класса.</span><span class="sxs-lookup"><span data-stu-id="4102c-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="4102c-110">Прежде чем использовать заметки к данным проверяющие элементы управления, необходимо загрузить связывателя модели данных заметок.</span><span class="sxs-lookup"><span data-stu-id="4102c-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="4102c-111">Примером связывателя модели данных заметок можно загрузить с веб-сайта CodePlex, щелкнув [здесь](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span><span class="sxs-lookup"><span data-stu-id="4102c-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="4102c-112">Важно понимать, что связывателя модели данных заметок не официальной частью платформы Microsoft ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4102c-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="4102c-113">Несмотря на то, что связывателя модели данных заметок создан командой Microsoft ASP.NET MVC, Майкрософт не осуществляет Официальная поддержка для связывателя модели данных заметок описанные и используемые в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="4102c-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="4102c-114">С помощью связывателя модели данных заметок</span><span class="sxs-lookup"><span data-stu-id="4102c-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="4102c-115">Чтобы использовать связывателя модели данных заметок в MVC-приложениях ASP.NET, необходимо сначала добавить ссылку на сборку Microsoft.Web.Mvc.DataAnnotations.dll и сборки библиотеку System.ComponentModel.DataAnnotations.dll.</span><span class="sxs-lookup"><span data-stu-id="4102c-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="4102c-116">Выберите пункт меню в **проект, добавить ссылку**.</span><span class="sxs-lookup"><span data-stu-id="4102c-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="4102c-117">Далее щелкните **Обзор** вкладку и перейдите к расположению, в которую был загружен (и распаковываются) в образце связыватель модели данных заметок (в разделе **рис. 1**).</span><span class="sxs-lookup"><span data-stu-id="4102c-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

<span data-ttu-id="4102c-118">**Рис. 1**: добавить ссылку на связыватель модели данных заметок ([Просмотр полноразмерное изображение](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4102c-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span></span>

<span data-ttu-id="4102c-119">Выберите сборку Microsoft.Web.Mvc.DataAnnotations.dll и библиотеку System.ComponentModel.DataAnnotations.dll сборку и нажмите кнопку **ОК** кнопки.</span><span class="sxs-lookup"><span data-stu-id="4102c-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="4102c-120">Нельзя использовать библиотеку System.ComponentModel.DataAnnotations.dll сборки, входящий в состав .NET Framework с пакетом обновления 1 связывателя модели данных заметок.</span><span class="sxs-lookup"><span data-stu-id="4102c-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="4102c-121">Необходимо использовать версию сборки библиотеку System.ComponentModel.DataAnnotations.dll, входящий в состав загрузки образца связывателя модели данных заметок.</span><span class="sxs-lookup"><span data-stu-id="4102c-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="4102c-122">Наконец необходимо зарегистрировать DataAnnotations связыватель модели в файле Global.asax.</span><span class="sxs-lookup"><span data-stu-id="4102c-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="4102c-123">Добавьте следующую строку кода в приложение\_Start() обработчик событий, чтобы приложение\_метод Start() выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4102c-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

<span data-ttu-id="4102c-124">Эта строка кода регистрирует ataAnnotationsModelBinder как связыватель модели по умолчанию для всего приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4102c-124">This line of code registers the ataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="4102c-125">С помощью атрибуты данных заметок проверяющий элемент управления</span><span class="sxs-lookup"><span data-stu-id="4102c-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="4102c-126">При использовании связывателя модели данных заметок, можно использовать атрибуты проверяющий элемент управления для выполнения проверки.</span><span class="sxs-lookup"><span data-stu-id="4102c-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="4102c-127">Пространство имен System.ComponentModel.DataAnnotations включает следующие атрибуты оператора проверки:</span><span class="sxs-lookup"><span data-stu-id="4102c-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="4102c-128">Диапазон — позволяет проверить, является ли значение свойства находится в указанном диапазоне значений.</span><span class="sxs-lookup"><span data-stu-id="4102c-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="4102c-129">Регулярное выражение – позволяет проверить, соответствует ли значение свойства заданного шаблона регулярного выражения.</span><span class="sxs-lookup"><span data-stu-id="4102c-129">RegularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="4102c-130">Требуется — позволяет помечать свойство как обязательное.</span><span class="sxs-lookup"><span data-stu-id="4102c-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="4102c-131">StringLength — позволяет указать максимальную длину для свойства строки.</span><span class="sxs-lookup"><span data-stu-id="4102c-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="4102c-132">Проверка — базовый класс для всех атрибутов проверки.</span><span class="sxs-lookup"><span data-stu-id="4102c-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="4102c-133">Если вашим требованиям проверки не выполняются на любой из стандартных проверяющие элементы управления затем всегда имеется возможность создания атрибута настраиваемый проверяющий элемент управления путем наследования от базового атрибута проверки нового атрибута проверяющего элемента управления.</span><span class="sxs-lookup"><span data-stu-id="4102c-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="4102c-134">Класс продукта в **список 1** показано, как использовать эти атрибуты проверяющий элемент управления.</span><span class="sxs-lookup"><span data-stu-id="4102c-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="4102c-135">Имя, описание и UnitPrice свойства, которые отмечены как требуется.</span><span class="sxs-lookup"><span data-stu-id="4102c-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="4102c-136">Свойство Name должно иметь строку, длина которой составляет менее 10 символов.</span><span class="sxs-lookup"><span data-stu-id="4102c-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="4102c-137">Наконец свойство UnitPrice должно соответствовать регулярному выражению, которая представляет денежную сумму.</span><span class="sxs-lookup"><span data-stu-id="4102c-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

<span data-ttu-id="4102c-138">**Листинг 1**: Models\Product.cs</span><span class="sxs-lookup"><span data-stu-id="4102c-138">**Listing 1**: Models\Product.cs</span></span>

<span data-ttu-id="4102c-139">Класс продукта показано, как использовать один дополнительный атрибут: атрибут DisplayName.</span><span class="sxs-lookup"><span data-stu-id="4102c-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="4102c-140">Атрибут DisplayName позволяет изменять имя свойства, если свойство отображается в сообщении об ошибке.</span><span class="sxs-lookup"><span data-stu-id="4102c-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="4102c-141">Вместо отображения сообщение об ошибке «поле «Цена» требуется» можно отобразить сообщение об ошибке «поле «Цена» является обязательным».</span><span class="sxs-lookup"><span data-stu-id="4102c-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="4102c-142">Если вы хотите полностью настроить сообщение об ошибке, проверяющий элемент управления можно назначить пользовательское сообщение об ошибке в свойство ErrorMessage проверяющего элемента управления следующим образом: `<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="4102c-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="4102c-143">Можно использовать класс продукта в **список 1** с Create() действия контроллера в **листинг 2**.</span><span class="sxs-lookup"><span data-stu-id="4102c-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="4102c-144">Это действие контроллера повторно отображает представление создания при состояния модели содержит все ошибки.</span><span class="sxs-lookup"><span data-stu-id="4102c-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

<span data-ttu-id="4102c-145">**Листинг 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="4102c-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="4102c-146">Наконец, можно создать представление в **листинге 3** , щелкнув правой кнопкой мыши действие Create() и выбрав пункт меню **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="4102c-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="4102c-147">Создайте строго типизированное представление с продукта класс как класс модели.</span><span class="sxs-lookup"><span data-stu-id="4102c-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="4102c-148">Выберите **создать** из представления содержимого раскрывающегося списка (см. **рис. 2**).</span><span class="sxs-lookup"><span data-stu-id="4102c-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

<span data-ttu-id="4102c-149">**На рисунке 2**: Добавление представления создания</span><span class="sxs-lookup"><span data-stu-id="4102c-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

<span data-ttu-id="4102c-150">**Листинг 3**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="4102c-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="4102c-151">Удалить поле из Создание формы, созданные **добавить представление** пункт меню.</span><span class="sxs-lookup"><span data-stu-id="4102c-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="4102c-152">Так как поле «идентификатор» соответствует столбец идентификаторов, вы не хотите разрешить пользователям вводить значение для этого поля.</span><span class="sxs-lookup"><span data-stu-id="4102c-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="4102c-153">Если отправки формы для создания продукта и не вводите значения для обязательных полей, а затем выдает сообщение об ошибке проверки **рис. 3** отображаются.</span><span class="sxs-lookup"><span data-stu-id="4102c-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

<span data-ttu-id="4102c-154">**Рис. 3**: отсутствуют обязательные поля</span><span class="sxs-lookup"><span data-stu-id="4102c-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="4102c-155">Если ввести недопустимые валюты суммы, то сообщение об ошибке в **рис. 4** отображается.</span><span class="sxs-lookup"><span data-stu-id="4102c-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

<span data-ttu-id="4102c-156">**Рис. 4**: Недопустимая валюта сумма</span><span class="sxs-lookup"><span data-stu-id="4102c-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="4102c-157">С помощью данных заметки проверяющие элементы управления с платформой Entity Framework</span><span class="sxs-lookup"><span data-stu-id="4102c-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="4102c-158">Если вы используете для создания классов модели данных Microsoft Entity Framework не удается применить атрибуты оператора проверки к классам.</span><span class="sxs-lookup"><span data-stu-id="4102c-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="4102c-159">Поскольку Entity Framework Designer создает классы модели, все изменения, внесенные в классы модели будет перезаписана при очередном вы внесли изменения в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="4102c-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="4102c-160">Если вы хотите использовать проверяющие элементы управления с классами, созданные платформой Entity Framework, необходимо создать классы данных метаданных.</span><span class="sxs-lookup"><span data-stu-id="4102c-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="4102c-161">Класс метаданных данных вместо применения проверяющие элементы управления на фактический класс применяются проверяющие элементы управления.</span><span class="sxs-lookup"><span data-stu-id="4102c-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="4102c-162">Например, предположим, что вы создали класс фильма платформы Entity Framework (см. **рис. 5**).</span><span class="sxs-lookup"><span data-stu-id="4102c-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="4102c-163">Представьте себе, кроме того, вы хотите сделать название фильма и директора необходимые свойства для свойства.</span><span class="sxs-lookup"><span data-stu-id="4102c-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="4102c-164">В этом случае можно создать разделяемый класс и класс данных метаданных в **листинг 4**.</span><span class="sxs-lookup"><span data-stu-id="4102c-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

<span data-ttu-id="4102c-165">**Рис. 5**: класс фильм, созданный платформой Entity Framework</span><span class="sxs-lookup"><span data-stu-id="4102c-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

<span data-ttu-id="4102c-166">**Листинг 4**: Models\Movie.cs</span><span class="sxs-lookup"><span data-stu-id="4102c-166">**Listing 4**: Models\Movie.cs</span></span>

<span data-ttu-id="4102c-167">Файл в **листинг 4** содержит два класса с именем фильма и MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="4102c-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="4102c-168">Класс фильма является частичным классом.</span><span class="sxs-lookup"><span data-stu-id="4102c-168">The Movie class is a partial class.</span></span> <span data-ttu-id="4102c-169">Он соответствует разделяемый класс, созданный платформой Entity Framework, которая содержится в файле DataModel.Designer.vb.</span><span class="sxs-lookup"><span data-stu-id="4102c-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="4102c-170">В настоящее время платформа .NET framework не поддерживает частичное свойства.</span><span class="sxs-lookup"><span data-stu-id="4102c-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="4102c-171">Таким образом, нет возможности применение атрибутов проверяющий элемент управления к свойствам класса фильм, определенные в файле DataModel.Designer.vb путем применения атрибутов проверяющий элемент управления к свойствам класса фильма, определенные в файле в **листинг 4**.</span><span class="sxs-lookup"><span data-stu-id="4102c-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="4102c-172">Обратите внимание, что разделяемый класс фильма помечено атрибутом MetadataType, указывающий на класс MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="4102c-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="4102c-173">Класс MovieMetaData содержит свойства прокси-сервера для свойства класса фильма.</span><span class="sxs-lookup"><span data-stu-id="4102c-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="4102c-174">Атрибуты оператора проверки применяются к свойствам класса MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="4102c-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="4102c-175">Свойства Title, директор и DateReleased помечаются как обязательные свойства.</span><span class="sxs-lookup"><span data-stu-id="4102c-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="4102c-176">Свойство директора необходимо назначить строку, содержащую не более 5 символов.</span><span class="sxs-lookup"><span data-stu-id="4102c-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="4102c-177">Наконец атрибут DisplayName DateReleased свойства для отображения сообщения об ошибке, как «поле даты выпуска является обязательным.»</span><span class="sxs-lookup"><span data-stu-id="4102c-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="4102c-178">вместо ошибки «DateReleased поле является обязательным.»</span><span class="sxs-lookup"><span data-stu-id="4102c-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="4102c-179">Обратите внимание, что свойства в классе MovieMetaData прокси-сервера не должны представлять совпадает с типом свойства соответствующего класса фильма.</span><span class="sxs-lookup"><span data-stu-id="4102c-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="4102c-180">Например свойство директор является строковое свойство в классе фильма и свойства объекта в классе MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="4102c-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="4102c-181">Страницы в **рис. 6** иллюстрирует сообщения об ошибках, если ввести недопустимые значения для свойств фильма.</span><span class="sxs-lookup"><span data-stu-id="4102c-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

<span data-ttu-id="4102c-182">**Рис. 6**: с помощью проверяющие элементы управления с платформой Entity Framework ([Просмотр полноразмерное изображение](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="4102c-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="4102c-183">Сводка</span><span class="sxs-lookup"><span data-stu-id="4102c-183">Summary</span></span>

<span data-ttu-id="4102c-184">В этом учебнике вы узнали, как пользоваться преимуществами связывателя модели данных заметок для выполнения проверки в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4102c-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="4102c-185">Вы узнали, как использовать различные типы атрибутов проверяющий элемент управления, таких как обязательный и StringLength атрибутов.</span><span class="sxs-lookup"><span data-stu-id="4102c-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="4102c-186">Вы также узнали, как использовать эти атрибуты при работе с платформой Entity Framework корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="4102c-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4102c-187">[Назад](validating-with-a-service-layer-cs.md)
> [Вперед](creating-model-classes-with-the-entity-framework-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4102c-187">[Previous](validating-with-a-service-layer-cs.md)
[Next](creating-model-classes-with-the-entity-framework-vb.md)</span></span>
