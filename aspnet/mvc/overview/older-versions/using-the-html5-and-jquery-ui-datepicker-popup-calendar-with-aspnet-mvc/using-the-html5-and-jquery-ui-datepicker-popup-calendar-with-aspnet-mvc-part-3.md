---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Использование HTML5 и раскрывающегося календаря с выбором дат jQuery с ASP.NET MVC — часть 3 | Документы Microsoft
author: Rick-Anderson
description: Этот учебник поможет узнать основные принципы работы с редактор шаблонов, шаблоны отображения и пользовательского интерфейса jQuery datepicker раскрывающегося календаря в MV ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: fd1ae746f4f134b779c7eee50cf6c840bbb7068e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="d23a6-103">Использование HTML5 и раскрывающегося календаря с выбором дат jQuery с ASP.NET MVC — часть 3</span><span class="sxs-lookup"><span data-stu-id="d23a6-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="d23a6-104">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d23a6-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="d23a6-105">Этот учебник поможет узнать основные принципы работы с редактор шаблонов, шаблоны отображения и пользовательского интерфейса jQuery datepicker раскрывающегося календаря в приложении MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d23a6-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="d23a6-106">Работа со сложными типами</span><span class="sxs-lookup"><span data-stu-id="d23a6-106">Working with Complex Types</span></span>

<span data-ttu-id="d23a6-107">В этом разделе будет создать класс адрес и сведения о создании шаблона для его отображения.</span><span class="sxs-lookup"><span data-stu-id="d23a6-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="d23a6-108">В *моделей* папки, создайте новый файл класса с именем *Person.cs* размещения двух типов: `Person` класса и `Address` класса.</span><span class="sxs-lookup"><span data-stu-id="d23a6-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="d23a6-109">`Person` Класс будет содержать свойство, которое имеет тип `Address`.</span><span class="sxs-lookup"><span data-stu-id="d23a6-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="d23a6-110">`Address` Тип — это сложный тип, то есть он не является одним из встроенных типов, таких как `int`, `string`, или `double`.</span><span class="sxs-lookup"><span data-stu-id="d23a6-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="d23a6-111">Вместо этого он имеет несколько свойств.</span><span class="sxs-lookup"><span data-stu-id="d23a6-111">Instead, it has several properties.</span></span> <span data-ttu-id="d23a6-112">Код для новых классов выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d23a6-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="d23a6-113">В `Movie` контроллера, добавьте следующий `PersonDetail` действие для отображения экземпляр пользователь:</span><span class="sxs-lookup"><span data-stu-id="d23a6-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="d23a6-114">Затем добавьте следующий код в `Movie` контроллера для заполнения `Person` модель с образцами данных:</span><span class="sxs-lookup"><span data-stu-id="d23a6-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="d23a6-115">Откройте *Views\Movies\PersonDetail.cshtml* и добавьте следующую разметку для файла `PersonDetail` представления.</span><span class="sxs-lookup"><span data-stu-id="d23a6-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="d23a6-116">Нажмите клавиши Ctrl + F5 для запуска приложения и перейдите к *фильмов/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="d23a6-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="d23a6-117">`PersonDetail` Представление не содержит `Address` сложный тип, как показано на этом снимке экрана.</span><span class="sxs-lookup"><span data-stu-id="d23a6-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="d23a6-118">(Адрес не отображается).</span><span class="sxs-lookup"><span data-stu-id="d23a6-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="d23a6-119">`Address` Модели данных не отображаются, так как это сложный тип.</span><span class="sxs-lookup"><span data-stu-id="d23a6-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="d23a6-120">Для отображения информации об адресе, откройте *Views\Movies\PersonDetail.cshtml* попытку и добавьте следующую разметку.</span><span class="sxs-lookup"><span data-stu-id="d23a6-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="d23a6-121">Полную разметку для `PersonDetail` теперь представление выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d23a6-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="d23a6-122">Снова запустите приложение и отображения `PersonDetail` представления.</span><span class="sxs-lookup"><span data-stu-id="d23a6-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="d23a6-123">Сведения об адресе теперь отображается:</span><span class="sxs-lookup"><span data-stu-id="d23a6-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="d23a6-124">Создание шаблона для комплексного типа</span><span class="sxs-lookup"><span data-stu-id="d23a6-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="d23a6-125">В этом разделе вы создадите шаблон, который будет использоваться для подготовки к просмотру `Address` сложного типа.</span><span class="sxs-lookup"><span data-stu-id="d23a6-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="d23a6-126">При создании шаблона для `Address` типа, ASP.NET MVC можно автоматически использовать его для форматирования модели адресов в любом месте приложения.</span><span class="sxs-lookup"><span data-stu-id="d23a6-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="d23a6-127">Это дает возможность управлять отрисовку `Address` тип в одном месте, в приложении.</span><span class="sxs-lookup"><span data-stu-id="d23a6-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="d23a6-128">В *Views\Shared\DisplayTemplates* папки, создайте строго типизированным представлением с именем **адрес**:</span><span class="sxs-lookup"><span data-stu-id="d23a6-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="d23a6-129">Нажмите кнопку **добавить**, а затем откройте новый *Views\Shared\DisplayTemplates\Address.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="d23a6-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="d23a6-130">Новое представление содержит созданный следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="d23a6-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="d23a6-131">Запустите приложение и отображения `PersonDetail` представления.</span><span class="sxs-lookup"><span data-stu-id="d23a6-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="d23a6-132">На этот раз `Address` только что созданный шаблон используется для отображения `Address` сложный тип, поэтому отображение выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d23a6-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="d23a6-133">Сводка: Способов для указания формата отображения для модели и шаблона</span><span class="sxs-lookup"><span data-stu-id="d23a6-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="d23a6-134">Вы видели, что можно указать формат или шаблон для свойства модели с помощью следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="d23a6-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="d23a6-135">Применение `DisplayFormat` атрибут свойства в модели.</span><span class="sxs-lookup"><span data-stu-id="d23a6-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="d23a6-136">Например приведенный ниже код вызывает даты для отображения без времени:</span><span class="sxs-lookup"><span data-stu-id="d23a6-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="d23a6-137">Применение [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) для свойства в модели, а также указать тип данных атрибута.</span><span class="sxs-lookup"><span data-stu-id="d23a6-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="d23a6-138">Например приведенный ниже код вызывает даты для отображения без времени.</span><span class="sxs-lookup"><span data-stu-id="d23a6-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="d23a6-139">Если приложение содержит *date.cshtml* шаблона в *Views\Shared\DisplayTemplates* папки или *Views\Movies\DisplayTemplates* папки, этот шаблон будет использоваться для подготовки к просмотру `DateTime` свойство.</span><span class="sxs-lookup"><span data-stu-id="d23a6-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="d23a6-140">В противном случае встроенную систему шаблонов ASP.NET отображает свойство как дата.</span><span class="sxs-lookup"><span data-stu-id="d23a6-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="d23a6-141">Создание шаблона экрана в *Views\Shared\DisplayTemplates* папки или *Views\Movies\DisplayTemplates* папки, имя которого совпадает с типом данных, которую требуется отформатировать.</span><span class="sxs-lookup"><span data-stu-id="d23a6-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="d23a6-142">Например, вы видели, *Views\Shared\DisplayTemplates\DateTime.cshtml* использовался для подготовки к просмотру `DateTime` свойства в модели, без добавления атрибута в модель и Добавление разметки представления.</span><span class="sxs-lookup"><span data-stu-id="d23a6-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="d23a6-143">С помощью [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) атрибут для модели, чтобы указать шаблон для отображения свойства модели.</span><span class="sxs-lookup"><span data-stu-id="d23a6-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="d23a6-144">Явное добавление отображаемое имя шаблона для [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) вызвать в представлении.</span><span class="sxs-lookup"><span data-stu-id="d23a6-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="d23a6-145">Используется подход зависит от того, что нужно делать в приложении.</span><span class="sxs-lookup"><span data-stu-id="d23a6-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="d23a6-146">Довольно часто для подхода для получения именно такие форматирование, необходимый.</span><span class="sxs-lookup"><span data-stu-id="d23a6-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="d23a6-147">В следующем разделе мы перейдем немного рассказ и перемещение из Настройка способа отображения для настройки способа ввода данных.</span><span class="sxs-lookup"><span data-stu-id="d23a6-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="d23a6-148">Будете подключать datepicker jQuery с представлениями изменения в приложение, чтобы предоставить называется способ указания даты.</span><span class="sxs-lookup"><span data-stu-id="d23a6-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d23a6-149">[Назад](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Вперед](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="d23a6-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
