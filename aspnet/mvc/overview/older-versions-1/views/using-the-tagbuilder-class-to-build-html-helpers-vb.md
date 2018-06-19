---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: С помощью класса TagBuilder можно построить вспомогательные методы HTML (VB) | Документы Microsoft
author: StephenWalther
description: Стивен Вальтер приведены полезные служебный класс в платформе ASP.NET MVC с именем класса TagBuilder. Легко можно использовать класс TagBuilder...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b72e08dff646f66252f210543230186cab6e641
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872235"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a><span data-ttu-id="4fca1-104">С помощью класса TagBuilder можно построить вспомогательные методы HTML (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="4fca1-104">Using the TagBuilder Class to Build HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="4fca1-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="4fca1-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="4fca1-106">Стивен Вальтер приведены полезные служебный класс в платформе ASP.NET MVC с именем класса TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="4fca1-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="4fca1-107">Класс TagBuilder можно использовать для облегчения создания HTML-теги.</span><span class="sxs-lookup"><span data-stu-id="4fca1-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="4fca1-108">Платформа ASP.NET MVC включает полезные служебный класс с именем класса TagBuilder, который можно использовать при создании вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="4fca1-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="4fca1-109">Класс TagBuilder, как видно из имени класса, можно легко создавать HTML-теги.</span><span class="sxs-lookup"><span data-stu-id="4fca1-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="4fca1-110">На этом краткий учебник предоставляются общие сведения о классе TagBuilder и вы узнаете, как использовать этот класс при создании простого вспомогательный метод HTML, который прорисовывает HTML- &lt;img&gt; тегов.</span><span class="sxs-lookup"><span data-stu-id="4fca1-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="4fca1-111">Обзор класса TagBuilder</span><span class="sxs-lookup"><span data-stu-id="4fca1-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="4fca1-112">Класс TagBuilder содержится в пространстве имен System.Web.Mvc.</span><span class="sxs-lookup"><span data-stu-id="4fca1-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="4fca1-113">Он имеет пять методов:</span><span class="sxs-lookup"><span data-stu-id="4fca1-113">It has five methods:</span></span>

- <span data-ttu-id="4fca1-114">AddCssClass() — предоставляет возможность добавить новую *класс = "»* атрибут к тегу.</span><span class="sxs-lookup"><span data-stu-id="4fca1-114">AddCssClass() – Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="4fca1-115">GenerateId() — можно добавить атрибут id тега.</span><span class="sxs-lookup"><span data-stu-id="4fca1-115">GenerateId() – Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="4fca1-116">Этот метод автоматически замещающий точки в идентификаторе (по умолчанию точки заменяются символами подчеркивания)</span><span class="sxs-lookup"><span data-stu-id="4fca1-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="4fca1-117">MergeAttribute() — можно добавить атрибуты к тегу.</span><span class="sxs-lookup"><span data-stu-id="4fca1-117">MergeAttribute() – Enables you to add attributes to a tag.</span></span> <span data-ttu-id="4fca1-118">Существует несколько перегрузок данного метода.</span><span class="sxs-lookup"><span data-stu-id="4fca1-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="4fca1-119">SetInnerText() — позволяет задать внутренний текст тега.</span><span class="sxs-lookup"><span data-stu-id="4fca1-119">SetInnerText() – Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="4fca1-120">Внутренний текст не кодировка HTML.</span><span class="sxs-lookup"><span data-stu-id="4fca1-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="4fca1-121">ToString() — позволяет отображать тег.</span><span class="sxs-lookup"><span data-stu-id="4fca1-121">ToString() – Enables you to render the tag.</span></span> <span data-ttu-id="4fca1-122">Можно указать, хотите ли вы создать обычный тег, открывающего тега, закрывающего тега или самозакрывающегося тега.</span><span class="sxs-lookup"><span data-stu-id="4fca1-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="4fca1-123">Класс TagBuilder имеет четыре важных свойства:</span><span class="sxs-lookup"><span data-stu-id="4fca1-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="4fca1-124">Атрибуты — представляет все атрибуты тега.</span><span class="sxs-lookup"><span data-stu-id="4fca1-124">Attributes – Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="4fca1-125">IdAttributeDotReplacement — представляет символ, используемый методом GenerateId() для замены периодов (по умолчанию — символ подчеркивания).</span><span class="sxs-lookup"><span data-stu-id="4fca1-125">IdAttributeDotReplacement – Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="4fca1-126">InnerHTML — представляет внутреннее содержимое тега.</span><span class="sxs-lookup"><span data-stu-id="4fca1-126">InnerHTML – Represents the inner contents of the tag.</span></span> <span data-ttu-id="4fca1-127">Присвоение этому свойству строки *не* HTML-кодирования строки.</span><span class="sxs-lookup"><span data-stu-id="4fca1-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="4fca1-128">TagName — представляет имя тега.</span><span class="sxs-lookup"><span data-stu-id="4fca1-128">TagName – Represents the name of the tag.</span></span>

<span data-ttu-id="4fca1-129">Эти методы и свойства предоставляют все основные методы и свойства, необходимые для построения тег HTML.</span><span class="sxs-lookup"><span data-stu-id="4fca1-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="4fca1-130">Действительно не требуется использовать класс TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="4fca1-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="4fca1-131">Вместо этого можно использовать класс StringBuilder.</span><span class="sxs-lookup"><span data-stu-id="4fca1-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="4fca1-132">Однако класс TagBuilder упрощает жизнь немного.</span><span class="sxs-lookup"><span data-stu-id="4fca1-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="4fca1-133">Создание вспомогательного метода HTML изображения</span><span class="sxs-lookup"><span data-stu-id="4fca1-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="4fca1-134">При создании экземпляра класса TagBuilder, необходимо передать имя тега, который требуется выполнить сборку в конструктор TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="4fca1-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="4fca1-135">Затем можно вызывать методы, как методы AddCssClass и MergeAttribute(), чтобы изменить атрибуты тега.</span><span class="sxs-lookup"><span data-stu-id="4fca1-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="4fca1-136">Наконец вызвать метод ToString() для визуализации тега.</span><span class="sxs-lookup"><span data-stu-id="4fca1-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="4fca1-137">Например список 1 содержит вспомогательный метод HTML изображения.</span><span class="sxs-lookup"><span data-stu-id="4fca1-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="4fca1-138">Реализуется внутри поддержки изображений с TagBuilder, который представляет элемент HTML &lt;img&gt; тег.</span><span class="sxs-lookup"><span data-stu-id="4fca1-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="4fca1-139">**Перечисление Helpers\ImageHelper.vb. 1.**</span><span class="sxs-lookup"><span data-stu-id="4fca1-139">**Listing 1 – Helpers\ImageHelper.vb**</span></span>

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

<span data-ttu-id="4fca1-140">Модуль в список 1 содержит два перегруженных методов с именем Image().</span><span class="sxs-lookup"><span data-stu-id="4fca1-140">The module in Listing 1 contains two overloaded methods named Image().</span></span> <span data-ttu-id="4fca1-141">При вызове метода Image(), можно передать объект, представляющий набор атрибутов HTML или нет.</span><span class="sxs-lookup"><span data-stu-id="4fca1-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="4fca1-142">Обратите внимание на то, как метод TagBuilder.MergeAttribute() используется для добавления отдельных атрибутов, таких как атрибут src для TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="4fca1-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="4fca1-143">Обратите внимание на то, кроме того, как используется метод TagBuilder.MergeAttributes() Добавление TagBuilder коллекцию атрибутов.</span><span class="sxs-lookup"><span data-stu-id="4fca1-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="4fca1-144">Метод MergeAttributes() принимает словарь&lt;строка, объект&gt; параметра.</span><span class="sxs-lookup"><span data-stu-id="4fca1-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="4fca1-145">Класс RouteValueDictionary используется для преобразования в объект, представляющий коллекцию атрибутов в словарь&lt;строка, объект&gt;.</span><span class="sxs-lookup"><span data-stu-id="4fca1-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="4fca1-146">После создания поддержки изображений можно использовать вспомогательный объект в представлений ASP.NET MVC так же, как любые другие стандартные вспомогательные методы HTML.</span><span class="sxs-lookup"><span data-stu-id="4fca1-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="4fca1-147">Представление в списке 2 использует поддержки изображений для отображения один и тот же образ Xbox дважды (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="4fca1-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="4fca1-148">Вспомогательный объект Image() вызывается как с и без коллекции атрибутов HTML.</span><span class="sxs-lookup"><span data-stu-id="4fca1-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="4fca1-149">**Перечисление Home\Index.aspx. 2.**</span><span class="sxs-lookup"><span data-stu-id="4fca1-149">**Listing 2 – Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


<span data-ttu-id="4fca1-150">[![Диалоговое окно нового проекта](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4fca1-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="4fca1-151">**На рисунке 01**: использование вспомогательного метода изображения ([Просмотр полноразмерное изображение](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="4fca1-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span></span>


<span data-ttu-id="4fca1-152">Обратите внимание, что необходимо импортировать пространство имен, связанное со вспомогательным методом изображения в верхней части представления Index.aspx.</span><span class="sxs-lookup"><span data-stu-id="4fca1-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="4fca1-153">Вспомогательный объект импортируется с следующую директиву:</span><span class="sxs-lookup"><span data-stu-id="4fca1-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

<span data-ttu-id="4fca1-154">В приложениях Visual Basic пространства имен по умолчанию совпадает с именем приложения.</span><span class="sxs-lookup"><span data-stu-id="4fca1-154">In a Visual Basic application, the default namespace is the same as the name of the application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4fca1-155">[Назад](creating-custom-html-helpers-vb.md)
> [Вперед](creating-page-layouts-with-view-master-pages-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4fca1-155">[Previous](creating-custom-html-helpers-vb.md)
[Next](creating-page-layouts-with-view-master-pages-vb.md)</span></span>
