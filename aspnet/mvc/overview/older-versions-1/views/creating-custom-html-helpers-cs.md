---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: "Создание настраиваемых вспомогательных методов HTML (C#) | Документы Microsoft"
author: microsoft
description: "Целью данного учебника является демонстрация способы создания пользовательских вспомогательных методов HTML, который можно использовать в пределах представлений MVC. Используя преимущества вспомогательный метод HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: a0b6d67eb7aab51ba2b422fab0788e34255f2c8c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-custom-html-helpers-c"></a><span data-ttu-id="321c8-104">Создание настраиваемых вспомогательных методов HTML (C#)</span><span class="sxs-lookup"><span data-stu-id="321c8-104">Creating Custom HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="321c8-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="321c8-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="321c8-106">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="321c8-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="321c8-107">Целью данного учебника является демонстрация способы создания пользовательских вспомогательных методов HTML, который можно использовать в пределах представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="321c8-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="321c8-108">Используя преимущества вспомогательных методов HTML, можно уменьшить объем длительного ввода HTML-тегов, которые необходимо выполнить для создания стандартных HTML-страниц.</span><span class="sxs-lookup"><span data-stu-id="321c8-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="321c8-109">Целью данного учебника является демонстрация способы создания пользовательских вспомогательных методов HTML, который можно использовать в пределах представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="321c8-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="321c8-110">Используя преимущества вспомогательных методов HTML, можно уменьшить объем длительного ввода HTML-тегов, которые необходимо выполнить для создания стандартных HTML-страниц.</span><span class="sxs-lookup"><span data-stu-id="321c8-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="321c8-111">В первой части этого учебника я опишу некоторые из существующих вспомогательных методов HTML, входящий в состав платформы ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="321c8-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="321c8-112">Далее описывается создание пользовательских вспомогательных методов HTML два метода: я описаны способы создания пользовательских вспомогательных методов HTML путем создания статический метод и путем создания метода расширения.</span><span class="sxs-lookup"><span data-stu-id="321c8-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="321c8-113">Основные сведения о вспомогательных методов HTML</span><span class="sxs-lookup"><span data-stu-id="321c8-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="321c8-114">Вспомогательный метод HTML — просто это метод, который возвращает строку.</span><span class="sxs-lookup"><span data-stu-id="321c8-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="321c8-115">Строка может представлять любой тип содержимого, которое требуется.</span><span class="sxs-lookup"><span data-stu-id="321c8-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="321c8-116">Например, можно использовать вспомогательные методы HTML для подготовки к просмотру стандартные теги HTML, например HTML `<input>` и `<img>` тегов.</span><span class="sxs-lookup"><span data-stu-id="321c8-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="321c8-117">Также можно использовать вспомогательные методы HTML для отображения более сложного содержимого, например полосе вкладок или HTML-таблицы базы данных.</span><span class="sxs-lookup"><span data-stu-id="321c8-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="321c8-118">Платформа ASP.NET MVC включает следующий набор стандартных вспомогательные методы HTML (это не полный список):</span><span class="sxs-lookup"><span data-stu-id="321c8-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="321c8-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="321c8-119">Html.ActionLink()</span></span>
- <span data-ttu-id="321c8-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="321c8-120">Html.BeginForm()</span></span>
- <span data-ttu-id="321c8-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="321c8-121">Html.CheckBox()</span></span>
- <span data-ttu-id="321c8-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="321c8-122">Html.DropDownList()</span></span>
- <span data-ttu-id="321c8-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="321c8-123">Html.EndForm()</span></span>
- <span data-ttu-id="321c8-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="321c8-124">Html.Hidden()</span></span>
- <span data-ttu-id="321c8-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="321c8-125">Html.ListBox()</span></span>
- <span data-ttu-id="321c8-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="321c8-126">Html.Password()</span></span>
- <span data-ttu-id="321c8-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="321c8-127">Html.RadioButton()</span></span>
- <span data-ttu-id="321c8-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="321c8-128">Html.TextArea()</span></span>
- <span data-ttu-id="321c8-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="321c8-129">Html.TextBox()</span></span>

<span data-ttu-id="321c8-130">Например рассмотрим форму в листинге 1.</span><span class="sxs-lookup"><span data-stu-id="321c8-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="321c8-131">Эта форма подготавливается к просмотру с помощью двух стандартных вспомогательные методы HTML (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="321c8-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="321c8-132">В этой форме используются `Html.BeginForm()` и `Html.TextBox()` вспомогательные методы для подготовки к просмотру простой HTML-формы.</span><span class="sxs-lookup"><span data-stu-id="321c8-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>


<span data-ttu-id="321c8-133">[![Страницы к просмотру с помощью вспомогательных методов HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="321c8-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="321c8-134">**На рисунке 01**: страницы к просмотру с помощью вспомогательных методов HTML ([Просмотр полноразмерное изображение](creating-custom-html-helpers-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="321c8-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>


<span data-ttu-id="321c8-135">**Листинг 1.`Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="321c8-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="321c8-136">Html.BeginForm() вспомогательный метод используется для создания HTML-код, открывающий и закрывающий `<form>` тегов.</span><span class="sxs-lookup"><span data-stu-id="321c8-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="321c8-137">Обратите внимание, что `Html.BeginForm()` метод вызывается с помощью инструкции.</span><span class="sxs-lookup"><span data-stu-id="321c8-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="321c8-138">Использование инструкции гарантирует, что `<form>` тег закрывается по окончании использования блока.</span><span class="sxs-lookup"><span data-stu-id="321c8-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="321c8-139">Если вы предпочитаете, вместо создания с помощью блока, можно вызвать Html.EndForm() вспомогательный метод для закрытия `<form>` тег.</span><span class="sxs-lookup"><span data-stu-id="321c8-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="321c8-140">Использовать любой способ создания Открытие и закрытие `<form>` тег, который наиболее интуитивно понятно пользователю.</span><span class="sxs-lookup"><span data-stu-id="321c8-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="321c8-141">`Html.TextBox()` Вспомогательные методы, используемые для отображения HTML в листинге 1 `<input>` тегов.</span><span class="sxs-lookup"><span data-stu-id="321c8-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="321c8-142">Если выбрать Просмотр исходного кода в браузере появится в списке 2 источник HTML.</span><span class="sxs-lookup"><span data-stu-id="321c8-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="321c8-143">Обратите внимание, что источник содержит стандартные теги HTML.</span><span class="sxs-lookup"><span data-stu-id="321c8-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="321c8-144">Обратите внимание, что `Html.TextBox()`-HTML вспомогательный визуализируется с `<%= %>` теги вместо `<% %>` тегов.</span><span class="sxs-lookup"><span data-stu-id="321c8-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="321c8-145">Если не включить знак равенства, ничего не возвращает выводятся в браузер.</span><span class="sxs-lookup"><span data-stu-id="321c8-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="321c8-146">Платформа ASP.NET MVC содержит небольшой набор вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="321c8-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="321c8-147">Скорее всего потребуется расширить платформу MVC с помощью пользовательских вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="321c8-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="321c8-148">В оставшейся части этого учебника вы узнаете два метода создания пользовательских вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="321c8-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="321c8-149">**Листинг 2.`Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="321c8-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="321c8-150">Создание вспомогательных методов HTML с помощью статических методов</span><span class="sxs-lookup"><span data-stu-id="321c8-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="321c8-151">Самый простой способ создать новый вспомогательный метод HTML является создание статический метод, который возвращает строку.</span><span class="sxs-lookup"><span data-stu-id="321c8-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="321c8-152">Представьте себе, например, вы решили создать новый вспомогательный метод HTML, который отображает HTML- `<label>` тег.</span><span class="sxs-lookup"><span data-stu-id="321c8-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="321c8-153">Для подготовки к просмотру, можно использовать класс в списке 2 `<label>` .</span><span class="sxs-lookup"><span data-stu-id="321c8-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="321c8-154">**Листинг 2.`Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="321c8-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="321c8-155">Нет ничего особенного класса в списке 2.</span><span class="sxs-lookup"><span data-stu-id="321c8-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="321c8-156">`Label()` Метод просто возвращает строку.</span><span class="sxs-lookup"><span data-stu-id="321c8-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="321c8-157">Измененный представление индекса в списке 3 использует `LabelHelper` для отрисовки HTML- `<label>` тегов.</span><span class="sxs-lookup"><span data-stu-id="321c8-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="321c8-158">Обратите внимание, что представление включает `<%@ imports %>` директиву, которая импортирует `Application1.Helpers` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="321c8-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="321c8-159">**Листинг 2.`Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="321c8-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="321c8-160">Создание вспомогательных методов HTML с помощью методов расширения</span><span class="sxs-lookup"><span data-stu-id="321c8-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="321c8-161">Если вы хотите создать вспомогательных методов HTML, которые работают так же как стандартные вспомогательных методов HTML, включены в платформе ASP.NET MVC, вам нужно создать методы расширения.</span><span class="sxs-lookup"><span data-stu-id="321c8-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="321c8-162">Методы расширения позволяют добавлять новые методы в существующий класс.</span><span class="sxs-lookup"><span data-stu-id="321c8-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="321c8-163">При создании метода вспомогательный метод HTML, добавлять новые методы класса HtmlHelper, представленного свойства Html представления.</span><span class="sxs-lookup"><span data-stu-id="321c8-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="321c8-164">Класс в списке 3 добавляет метод расширения, позволяющий `HtmlHelper` класс с именем `Label()`.</span><span class="sxs-lookup"><span data-stu-id="321c8-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="321c8-165">Существует несколько вещей, которые можно заметить об этом классе.</span><span class="sxs-lookup"><span data-stu-id="321c8-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="321c8-166">Во-первых Обратите внимание, что класс является статическим классом.</span><span class="sxs-lookup"><span data-stu-id="321c8-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="321c8-167">Необходимо определить метод расширения с статического класса.</span><span class="sxs-lookup"><span data-stu-id="321c8-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="321c8-168">Во-вторых, обратите внимание, что первый параметр `Label()` метод предшествует ключевое слово `this`.</span><span class="sxs-lookup"><span data-stu-id="321c8-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="321c8-169">Первый параметр метода расширения указывает класс, который расширяет метод расширения.</span><span class="sxs-lookup"><span data-stu-id="321c8-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="321c8-170">**Листинг 3.`Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="321c8-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="321c8-171">После создания метода расширения и построить приложение успешно, метод расширения отображается в Intellisense в Visual Studio как и всех других методов класса (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="321c8-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="321c8-172">Единственное отличие заключается в расширения методы отображаются с символом "специальный" рядом с ними (значок со стрелкой вниз).</span><span class="sxs-lookup"><span data-stu-id="321c8-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="321c8-173">[![С помощью метода расширения Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="321c8-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="321c8-174">**На рисунке 02**: с помощью метода расширения Html.Label() ([Просмотр полноразмерное изображение](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="321c8-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>


<span data-ttu-id="321c8-175">Измененный представление Index в листинге 4 этого метод расширения Html.Label() используется для отображения всех его `<label>` тегов.</span><span class="sxs-lookup"><span data-stu-id="321c8-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="321c8-176">**Листинг 4.`Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="321c8-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="321c8-177">Сводка</span><span class="sxs-lookup"><span data-stu-id="321c8-177">Summary</span></span>

<span data-ttu-id="321c8-178">В этом учебнике вы узнали два метода создания пользовательских вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="321c8-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="321c8-179">Во-первых, вы узнали, как создать настраиваемый `Label()` вспомогательный метод HTML, создав статический метод, который возвращает строку.</span><span class="sxs-lookup"><span data-stu-id="321c8-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="321c8-180">Далее вы узнали, как создать настраиваемый `Label()` метод вспомогательный метод HTML, создав метод расширения для `HtmlHelper` класса.</span><span class="sxs-lookup"><span data-stu-id="321c8-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="321c8-181">В этом учебнике уделено построение очень простой метод вспомогательный метод HTML.</span><span class="sxs-lookup"><span data-stu-id="321c8-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="321c8-182">Имейте в виду, что вспомогательный метод HTML может быть не проще требуется.</span><span class="sxs-lookup"><span data-stu-id="321c8-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="321c8-183">Можно построить вспомогательных методов HTML, отображения сложных элементов, таких как представления в виде дерева, меню и таблицы базы данных.</span><span class="sxs-lookup"><span data-stu-id="321c8-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="321c8-184">[Назад](asp-net-mvc-views-overview-cs.md)
[Вперед](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="321c8-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
