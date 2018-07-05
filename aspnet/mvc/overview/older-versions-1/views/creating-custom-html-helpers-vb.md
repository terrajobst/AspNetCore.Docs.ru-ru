---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Создание настраиваемых вспомогательных методов HTML (Visual Basic) | Документация Майкрософт
author: microsoft
description: Целью данного учебника — продемонстрировать, как можно создать пользовательские вспомогательных методов HTML, который можно использовать внутри представлений MVC. Используя преимущества вспомогательный метод HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 0b1f4a6afc62eb23d4591d515e973298da4630f9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396602"
---
<a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="8f35d-104">Создание настраиваемых вспомогательных методов HTML (VB)</span><span class="sxs-lookup"><span data-stu-id="8f35d-104">Creating Custom HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="8f35d-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8f35d-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="8f35d-106">Загрузить PDF-файл</span><span class="sxs-lookup"><span data-stu-id="8f35d-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="8f35d-107">Целью данного учебника — продемонстрировать, как можно создать пользовательские вспомогательных методов HTML, который можно использовать внутри представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="8f35d-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="8f35d-108">Используя преимущества вспомогательных методов HTML, можно уменьшить объем длительного ввода HTML-тегов, что необходимо выполнить, чтобы создать стандартную страницу HTML.</span><span class="sxs-lookup"><span data-stu-id="8f35d-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="8f35d-109">Целью данного учебника — продемонстрировать, как можно создать пользовательские вспомогательных методов HTML, который можно использовать внутри представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="8f35d-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="8f35d-110">Используя преимущества вспомогательных методов HTML, можно уменьшить объем длительного ввода HTML-тегов, что необходимо выполнить, чтобы создать стандартную страницу HTML.</span><span class="sxs-lookup"><span data-stu-id="8f35d-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="8f35d-111">В первой части этого руководства я опишу некоторые из существующих вспомогательных методов HTML, включенный в состав ASP.NET MVC framework.</span><span class="sxs-lookup"><span data-stu-id="8f35d-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="8f35d-112">Затем я опишу два метода создания настраиваемых вспомогательных методов HTML: я описывается создание настраиваемых вспомогательных методов HTML, создавая общий метод и путем создания метода расширения.</span><span class="sxs-lookup"><span data-stu-id="8f35d-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="8f35d-113">Основные сведения о вспомогательных методов HTML</span><span class="sxs-lookup"><span data-stu-id="8f35d-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="8f35d-114">Вспомогательный метод HTML — это метод, возвращает строку.</span><span class="sxs-lookup"><span data-stu-id="8f35d-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="8f35d-115">Строка может представлять любой тип содержимого, которое.</span><span class="sxs-lookup"><span data-stu-id="8f35d-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="8f35d-116">Например, можно использовать вспомогательные методы HTML для визуализации стандартный HTML-теги HTML `<input>` и `<img>` теги.</span><span class="sxs-lookup"><span data-stu-id="8f35d-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="8f35d-117">Также можно использовать вспомогательные методы HTML для отображения более сложного содержимого, например с вкладками или HTML-таблицы базы данных.</span><span class="sxs-lookup"><span data-stu-id="8f35d-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="8f35d-118">Платформа ASP.NET MVC включает следующий набор стандартных вспомогательных методов HTML (это не полный список):</span><span class="sxs-lookup"><span data-stu-id="8f35d-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="8f35d-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="8f35d-119">Html.ActionLink()</span></span>
- <span data-ttu-id="8f35d-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="8f35d-120">Html.BeginForm()</span></span>
- <span data-ttu-id="8f35d-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="8f35d-121">Html.CheckBox()</span></span>
- <span data-ttu-id="8f35d-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="8f35d-122">Html.DropDownList()</span></span>
- <span data-ttu-id="8f35d-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="8f35d-123">Html.EndForm()</span></span>
- <span data-ttu-id="8f35d-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="8f35d-124">Html.Hidden()</span></span>
- <span data-ttu-id="8f35d-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="8f35d-125">Html.ListBox()</span></span>
- <span data-ttu-id="8f35d-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="8f35d-126">Html.Password()</span></span>
- <span data-ttu-id="8f35d-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="8f35d-127">Html.RadioButton()</span></span>
- <span data-ttu-id="8f35d-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="8f35d-128">Html.TextArea()</span></span>
- <span data-ttu-id="8f35d-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="8f35d-129">Html.TextBox()</span></span>

<span data-ttu-id="8f35d-130">Например рассмотрим формы в листинге 1.</span><span class="sxs-lookup"><span data-stu-id="8f35d-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="8f35d-131">Эта форма подготавливается к просмотру с помощью двух стандартных вспомогательных методов HTML (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="8f35d-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="8f35d-132">Используемый этой формой `Html.BeginForm()` и `Html.TextBox()` вспомогательные методы.</span><span class="sxs-lookup"><span data-stu-id="8f35d-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>


<span data-ttu-id="8f35d-133">[![Страница отображается с помощью вспомогательных методов HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8f35d-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="8f35d-134">**Рис 01**: страница отображается с помощью вспомогательных методов HTML ([Просмотр полноразмерного изображения](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8f35d-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>


<span data-ttu-id="8f35d-135">**Листинг 1. `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="8f35d-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="8f35d-136">`Html.BeginForm()` Вспомогательный метод используется для создания HTML-код открывающей и закрывающей `<form>` теги.</span><span class="sxs-lookup"><span data-stu-id="8f35d-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="8f35d-137">Обратите внимание, что `Html.BeginForm()` был вызван в с помощью инструкции.</span><span class="sxs-lookup"><span data-stu-id="8f35d-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="8f35d-138">Оператор using гарантирует, что `<form>` тег закрывается в конце, используя блок.</span><span class="sxs-lookup"><span data-stu-id="8f35d-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="8f35d-139">Если вы предпочитаете, вместо создания с помощью блока, можно вызвать Html.EndForm() вспомогательный метод для закрытия `<form>` тега.</span><span class="sxs-lookup"><span data-stu-id="8f35d-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="8f35d-140">Использование подхода к созданию открывающий и закрытие `<form>` тег, который наиболее интуитивно понятно вам.</span><span class="sxs-lookup"><span data-stu-id="8f35d-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="8f35d-141">`Html.TextBox()` Вспомогательные методы, используются для визуализации HTML в листинге 1 `<input>` теги.</span><span class="sxs-lookup"><span data-stu-id="8f35d-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="8f35d-142">Если выбрать Просмотр исходного кода в браузере отобразится исходный код HTML в листинге 2.</span><span class="sxs-lookup"><span data-stu-id="8f35d-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="8f35d-143">Обратите внимание на то, что источник содержит стандартный HTML-теги.</span><span class="sxs-lookup"><span data-stu-id="8f35d-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8f35d-144">Обратите внимание, что `Html.TextBox()`-HTML вспомогательный визуализируется с `<%= %>` теги вместо `<% %>` теги.</span><span class="sxs-lookup"><span data-stu-id="8f35d-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="8f35d-145">Если вы не включаете знак равенства, ничего не возвращает выводятся в браузер.</span><span class="sxs-lookup"><span data-stu-id="8f35d-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="8f35d-146">Платформа ASP.NET MVC содержит небольшой набор вспомогательных функций.</span><span class="sxs-lookup"><span data-stu-id="8f35d-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="8f35d-147">Скорее всего потребуется расширить платформа MVC с помощью пользовательских вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="8f35d-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="8f35d-148">В оставшейся части этого руководства вы узнаете два метода создания настраиваемых вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="8f35d-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="8f35d-149">**Листинг 2. `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="8f35d-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="8f35d-150">Создание вспомогательных методов HTML с помощью общих методов</span><span class="sxs-lookup"><span data-stu-id="8f35d-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="8f35d-151">Чтобы создать новый вспомогательный метод HTML проще всего создать общий метод, который возвращает строку.</span><span class="sxs-lookup"><span data-stu-id="8f35d-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="8f35d-152">Представим, к примеру, что вам потребуется создать новый вспомогательный метод HTML, который выводит HTML- `<label>` тега.</span><span class="sxs-lookup"><span data-stu-id="8f35d-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="8f35d-153">Для подготовки к просмотру, можно использовать класс в листинге 2 `<label>`.</span><span class="sxs-lookup"><span data-stu-id="8f35d-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="8f35d-154">**Листинг 2. `Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="8f35d-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="8f35d-155">Нет ничего особенного классу в листинге 2.</span><span class="sxs-lookup"><span data-stu-id="8f35d-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="8f35d-156">`Label()` Метод просто возвращает строку.</span><span class="sxs-lookup"><span data-stu-id="8f35d-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="8f35d-157">Представление измененного индекса в листинге 3 использует `LabelHelper` для отрисовки HTML `<label>` теги.</span><span class="sxs-lookup"><span data-stu-id="8f35d-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="8f35d-158">Обратите внимание, что представление содержит `<%@ imports %>` директива, которая импортирует пространство имен Application1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="8f35d-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="8f35d-159">**Листинг 2. `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="8f35d-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="8f35d-160">Создание вспомогательных методов HTML с помощью методов расширения</span><span class="sxs-lookup"><span data-stu-id="8f35d-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="8f35d-161">Если вы хотите создать вспомогательные методы HTML, которые работают так же как стандартный вспомогательных методов HTML, включены в платформе ASP.NET MVC, то необходимо создать методы расширения.</span><span class="sxs-lookup"><span data-stu-id="8f35d-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="8f35d-162">Методы расширения позволяют добавлять новые методы в существующий класс.</span><span class="sxs-lookup"><span data-stu-id="8f35d-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="8f35d-163">При создании метода вспомогательный метод HTML, можно добавить новые методы `HtmlHelper` класса, представленного параметром свойства Html представления.</span><span class="sxs-lookup"><span data-stu-id="8f35d-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="8f35d-164">Модуль Visual Basic в листинге 3 добавляет метод расширения с именем `Label()` для `HtmlHelper` класса.</span><span class="sxs-lookup"><span data-stu-id="8f35d-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="8f35d-165">Существует несколько моментов, которые следует заметить, сведения об этом модуле.</span><span class="sxs-lookup"><span data-stu-id="8f35d-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="8f35d-166">Во-первых, обратите внимание, что модуль дополняется `<Extension()>` атрибута.</span><span class="sxs-lookup"><span data-stu-id="8f35d-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="8f35d-167">Чтобы использовать этот атрибут, необходимо импортировать `System.Runtime.CompilerServices` пространства имен</span><span class="sxs-lookup"><span data-stu-id="8f35d-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="8f35d-168">Во-вторых, обратите внимание, что первый параметр `Label()` представляет метод `HtmlHelper` класса.</span><span class="sxs-lookup"><span data-stu-id="8f35d-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="8f35d-169">Первый параметр метода расширения указывает класс, который расширяет метод расширения.</span><span class="sxs-lookup"><span data-stu-id="8f35d-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="8f35d-170">**Листинг 3. `Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="8f35d-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="8f35d-171">После создания метода расширения и успешной сборки приложения, метод расширения отображается в Intellisense в Visual Studio как и все другие методы класса (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="8f35d-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="8f35d-172">Единственная разница в расширения методы отображаются с символом "специальный" рядом с ними (значок стрелки вниз).</span><span class="sxs-lookup"><span data-stu-id="8f35d-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="8f35d-173">[![С помощью метода расширения Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="8f35d-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="8f35d-174">**Рис. 02**: с помощью метода расширения Html.Label() ([Просмотр полноразмерного изображения](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="8f35d-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>


<span data-ttu-id="8f35d-175">Измененный представление Index в листинге 4 используется метод расширения Html.Label() для подготовки к просмотру все его &lt;метка&gt; теги.</span><span class="sxs-lookup"><span data-stu-id="8f35d-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="8f35d-176">**Листинг 4. `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="8f35d-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="8f35d-177">Сводка</span><span class="sxs-lookup"><span data-stu-id="8f35d-177">Summary</span></span>

<span data-ttu-id="8f35d-178">В этом руководстве вы узнали, два метода создания настраиваемых вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="8f35d-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="8f35d-179">Во-первых, вы узнали, как можно создавать пользовательские `Label()` вспомогательный метод HTML, создав общий метод, который возвращает строку.</span><span class="sxs-lookup"><span data-stu-id="8f35d-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="8f35d-180">Далее вы узнали, как можно создавать пользовательские `Label()` метод вспомогательный метод HTML, создав метод расширения для `HtmlHelper` класса.</span><span class="sxs-lookup"><span data-stu-id="8f35d-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="8f35d-181">В этом учебнике я сосредоточился на создании метод очень простой вспомогательный метод HTML.</span><span class="sxs-lookup"><span data-stu-id="8f35d-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="8f35d-182">Учтите, что вспомогательный метод HTML могут быть настолько сложный, как требуется.</span><span class="sxs-lookup"><span data-stu-id="8f35d-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="8f35d-183">Вы можете создавать вспомогательные функции HTML, отображения форматированное содержимое, например представления в виде дерева, меню или таблиц базы данных.</span><span class="sxs-lookup"><span data-stu-id="8f35d-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8f35d-184">[Назад](asp-net-mvc-views-overview-vb.md)
> [Вперед](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8f35d-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
