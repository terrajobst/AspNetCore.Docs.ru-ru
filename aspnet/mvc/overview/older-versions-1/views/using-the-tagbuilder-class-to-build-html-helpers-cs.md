---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Используя класс TagBuilder для создания вспомогательных методов HTML (C#) | Документация Майкрософт
author: StephenWalther
description: Стивен Вальтер представлены полезные служебный класс в платформе ASP.NET MVC с именем класс TagBuilder. Можно легко использовать класс TagBuilder для...
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 9990fc7ad8093643a564a5e02ff65264d4a7fe15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813230"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a><span data-ttu-id="af436-104">Используя класс TagBuilder для создания вспомогательных методов HTML (C#)</span><span class="sxs-lookup"><span data-stu-id="af436-104">Using the TagBuilder Class to Build HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="af436-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="af436-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="af436-106">Стивен Вальтер представлены полезные служебный класс в платформе ASP.NET MVC с именем класс TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="af436-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="af436-107">Класс TagBuilder для простого создания HTML-теги.</span><span class="sxs-lookup"><span data-stu-id="af436-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="af436-108">Платформа ASP.NET MVC включает в себя полезные служебный класс с именем класс TagBuilder, который можно использовать при создании вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="af436-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="af436-109">Класс TagBuilder, как и предполагает имя класса, позволяет легко создавать HTML-теги.</span><span class="sxs-lookup"><span data-stu-id="af436-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="af436-110">Из этого краткого руководства вам будет предоставлен Обзор класс TagBuilder и вы узнаете, как использовать этот класс при построении простой вспомогательный метод HTML, который отображает HTML &lt;img&gt; теги.</span><span class="sxs-lookup"><span data-stu-id="af436-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="af436-111">Общие сведения о классе TagBuilder</span><span class="sxs-lookup"><span data-stu-id="af436-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="af436-112">Класс TagBuilder содержится в пространстве имен System.Web.Mvc.</span><span class="sxs-lookup"><span data-stu-id="af436-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="af436-113">Он имеет пять методов:</span><span class="sxs-lookup"><span data-stu-id="af436-113">It has five methods:</span></span>

- <span data-ttu-id="af436-114">AddCssClass() - позволяет добавлять новый *класс = "»* атрибут к тегу.</span><span class="sxs-lookup"><span data-stu-id="af436-114">AddCssClass() - Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="af436-115">GenerateId() - позволяет добавлять атрибут id для тега.</span><span class="sxs-lookup"><span data-stu-id="af436-115">GenerateId() - Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="af436-116">Этот метод автоматически замещающий точки в идентификаторе (по умолчанию, точки заменяются символами подчеркивания)</span><span class="sxs-lookup"><span data-stu-id="af436-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="af436-117">MergeAttribute() - предоставляет возможность добавить атрибуты к тегу.</span><span class="sxs-lookup"><span data-stu-id="af436-117">MergeAttribute() - Enables you to add attributes to a tag.</span></span> <span data-ttu-id="af436-118">Существует несколько перегрузок этого метода.</span><span class="sxs-lookup"><span data-stu-id="af436-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="af436-119">SetInnerText() - позволяет задать внутренний текст тега.</span><span class="sxs-lookup"><span data-stu-id="af436-119">SetInnerText() - Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="af436-120">Внутренний текст — автоматически кодировать в HTML.</span><span class="sxs-lookup"><span data-stu-id="af436-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="af436-121">ToString() — обеспечивает возможность визуализации тега.</span><span class="sxs-lookup"><span data-stu-id="af436-121">ToString() - Enables you to render the tag.</span></span> <span data-ttu-id="af436-122">Можно указать, хотите ли вы создать обычный тег, открывающего тега, закрывающий тег или самозакрывающийся тег.</span><span class="sxs-lookup"><span data-stu-id="af436-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="af436-123">Класс TagBuilder имеет четыре важных свойства:</span><span class="sxs-lookup"><span data-stu-id="af436-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="af436-124">Атрибуты — представляет все атрибуты тега.</span><span class="sxs-lookup"><span data-stu-id="af436-124">Attributes - Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="af436-125">IdAttributeDotReplacement - представляет символ, используемый методом GenerateId() для замены периодов (по умолчанию — символ подчеркивания).</span><span class="sxs-lookup"><span data-stu-id="af436-125">IdAttributeDotReplacement - Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="af436-126">InnerHTML - представляет внутреннее содержимое тега.</span><span class="sxs-lookup"><span data-stu-id="af436-126">InnerHTML - Represents the inner contents of the tag.</span></span> <span data-ttu-id="af436-127">Назначить строку для этого свойства *не* HTML кодирования строки.</span><span class="sxs-lookup"><span data-stu-id="af436-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="af436-128">TagName - представляет имя тега.</span><span class="sxs-lookup"><span data-stu-id="af436-128">TagName - Represents the name of the tag.</span></span>

<span data-ttu-id="af436-129">Эти методы и свойства предоставляют все базовые методы и свойства, которые необходимы для создания HTML-тега.</span><span class="sxs-lookup"><span data-stu-id="af436-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="af436-130">Вам не обязательно использовать класс TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="af436-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="af436-131">Вместо этого можно использовать класс StringBuilder.</span><span class="sxs-lookup"><span data-stu-id="af436-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="af436-132">Тем не менее класс TagBuilder упрощает вашу жизнь немного.</span><span class="sxs-lookup"><span data-stu-id="af436-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="af436-133">Создание вспомогательного метода HTML изображения</span><span class="sxs-lookup"><span data-stu-id="af436-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="af436-134">При создании экземпляра класса TagBuilder, необходимо передать имя тега, который требуется выполнить сборку в конструктор TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="af436-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="af436-135">Затем можно вызывать методы, такие как методы AddCssClass и MergeAttribute(), чтобы изменить атрибуты тега.</span><span class="sxs-lookup"><span data-stu-id="af436-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="af436-136">Наконец вызывается метод ToString() для отображения тега.</span><span class="sxs-lookup"><span data-stu-id="af436-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="af436-137">Например в листинге 1 содержит вспомогательный метод HTML изображения.</span><span class="sxs-lookup"><span data-stu-id="af436-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="af436-138">Вспомогательная функция изображения реализуются внутренним образом с помощью TagBuilder, который представляет элемент HTML &lt;img&gt; тега.</span><span class="sxs-lookup"><span data-stu-id="af436-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="af436-139">**В листинге 1 - Helpers\ImageHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="af436-139">**Listing 1 - Helpers\ImageHelper.cs**</span></span>

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

<span data-ttu-id="af436-140">Этот класс в листинге 1 содержит два статических перегруженных методов с именем образа.</span><span class="sxs-lookup"><span data-stu-id="af436-140">The class in Listing 1 contains two static overloaded methods named Image.</span></span> <span data-ttu-id="af436-141">При вызове метода Image(), можно передать объект, представляющий набор атрибутов HTML или нет.</span><span class="sxs-lookup"><span data-stu-id="af436-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="af436-142">Обратите внимание на то, как метод TagBuilder.MergeAttribute() используется для добавления отдельных атрибутов, таких как атрибут src для TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="af436-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="af436-143">Обратите внимание на то, кроме того, как используется метод TagBuilder.MergeAttributes() добавляемый TagBuilder коллекции атрибутов.</span><span class="sxs-lookup"><span data-stu-id="af436-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="af436-144">Метод MergeAttributes() принимает словарь&lt;строка, объект&gt; параметра.</span><span class="sxs-lookup"><span data-stu-id="af436-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="af436-145">Класс RouteValueDictionary используется для преобразования в объект, представляющий коллекцию атрибутов в словарь&lt;строка, объект&gt;.</span><span class="sxs-lookup"><span data-stu-id="af436-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="af436-146">После создания вспомогательная функция изображения, можно использовать вспомогательный метод в представлениях ASP.NET MVC так же, как и любой из других стандартных вспомогательные методы HTML.</span><span class="sxs-lookup"><span data-stu-id="af436-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="af436-147">В представлении в листинге 2 используется вспомогательная функция изображения для отображения одного образа Xbox дважды (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="af436-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="af436-148">Вспомогательный метод Image() вызывается с и без коллекцию атрибутов HTML.</span><span class="sxs-lookup"><span data-stu-id="af436-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="af436-149">**В листинге 2 - Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="af436-149">**Listing 2 - Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


<span data-ttu-id="af436-150">[![В диалоговом окне нового проекта](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="af436-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="af436-151">**Рис 01**: использование вспомогательного метода образа ([Просмотр полноразмерного изображения](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="af436-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span></span>


<span data-ttu-id="af436-152">Обратите внимание на то, что необходимо импортировать пространство имен, связанное со вспомогательным методом изображение в верхней части представления Index.aspx.</span><span class="sxs-lookup"><span data-stu-id="af436-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="af436-153">Вспомогательный метод импортируется с следующую директиву:</span><span class="sxs-lookup"><span data-stu-id="af436-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> <span data-ttu-id="af436-154">[Назад](creating-custom-html-helpers-cs.md)
> [Вперед](creating-page-layouts-with-view-master-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="af436-154">[Previous](creating-custom-html-helpers-cs.md)
[Next](creating-page-layouts-with-view-master-pages-cs.md)</span></span>
