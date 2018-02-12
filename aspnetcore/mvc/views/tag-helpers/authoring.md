---
title: "Разработка вспомогательных функций тегов в ASP.NET Core"
author: rick-anderson
description: "Сведения о разработке вспомогательных функций тегов в ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: 040c26bfccb8f258b0941bed4bc936cf7a16324a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="author-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="f42ab-103">Разработка вспомогательных функций тегов в ASP.NET Core, пошаговое руководство с примерами</span><span class="sxs-lookup"><span data-stu-id="f42ab-103">Author Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="f42ab-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="f42ab-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f42ab-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f42ab-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started-with-tag-helpers"></a><span data-ttu-id="f42ab-106">Начало работы с вспомогательными функциями тегов</span><span class="sxs-lookup"><span data-stu-id="f42ab-106">Get started with Tag Helpers</span></span>

<span data-ttu-id="f42ab-107">В этом учебнике приводятся основные сведения о программировании вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="f42ab-107">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="f42ab-108">В статье [Общие сведения о вспомогательных функциях тегов](intro.md) описываются преимущества, предоставляемые вспомогательными функциями тегов.</span><span class="sxs-lookup"><span data-stu-id="f42ab-108">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="f42ab-109">Вспомогательная функция тега — это любой класс, реализующий интерфейс `ITagHelper`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-109">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="f42ab-110">Однако при разработке вспомогательной функции тега обычно производится наследование `TagHelper`, так как это дает доступ к методу `Process`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-110">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="f42ab-111">Создайте проект ASP.NET Core с именем **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-111">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="f42ab-112">Проверка подлинности для этого проекта не потребуется.</span><span class="sxs-lookup"><span data-stu-id="f42ab-112">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="f42ab-113">Создайте папку, в которой будут помещаться вспомогательные функции тегов, с именем *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="f42ab-113">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="f42ab-114">Папка *TagHelpers* *не* является обязательной; она создается согласно принятому соглашению.</span><span class="sxs-lookup"><span data-stu-id="f42ab-114">The *TagHelpers* folder is *not* required, but it's a reasonable convention.</span></span> <span data-ttu-id="f42ab-115">Теперь приступим к написанию простейших вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="f42ab-115">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="f42ab-116">Простейшая вспомогательная функция тега</span><span class="sxs-lookup"><span data-stu-id="f42ab-116">A minimal Tag Helper</span></span>

<span data-ttu-id="f42ab-117">В этом разделе вы напишете вспомогательную функцию тега, которая обновляет тег электронной почты.</span><span class="sxs-lookup"><span data-stu-id="f42ab-117">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="f42ab-118">Пример:</span><span class="sxs-lookup"><span data-stu-id="f42ab-118">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="f42ab-119">Сервер будет использовать эту вспомогательную функцию тега для преобразования разметки следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f42ab-119">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

<span data-ttu-id="f42ab-120">Это тег привязки, который преобразует элемент в ссылку с адресом электронной почты.</span><span class="sxs-lookup"><span data-stu-id="f42ab-120">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="f42ab-121">Он может потребоваться, если вы создаете подсистему ведения блогов и вам нужно реализовать отправку электронной почты в отдел маркетинга, службу технической поддержки и на другие адреса в одном домене.</span><span class="sxs-lookup"><span data-stu-id="f42ab-121">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="f42ab-122">Добавьте приведенный ниже класс `EmailTagHelper` в папку *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="f42ab-122">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    <span data-ttu-id="f42ab-123">**Примечания.**</span><span class="sxs-lookup"><span data-stu-id="f42ab-123">**Notes:**</span></span>
    
    * <span data-ttu-id="f42ab-124">Для вспомогательных функций тегов используется соглашение об именовании в отношении элементов имени корневого класса (за исключением части *TagHelper* этого имени).</span><span class="sxs-lookup"><span data-stu-id="f42ab-124">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="f42ab-125">В этом примере корневым именем класса **Email**TagHelper является *email*, поэтому целевым является тег `<email>`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-125">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="f42ab-126">Это соглашение об именовании подходит для большинства вспомогательных функций тегов, позднее мы покажем, как переопределить его.</span><span class="sxs-lookup"><span data-stu-id="f42ab-126">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="f42ab-127">Класс `EmailTagHelper` является производным от класса `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-127">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="f42ab-128">Класс `TagHelper` предоставляет методы и свойства для написания вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="f42ab-128">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="f42ab-129">Переопределенный `Process` метод управляет действиями, выполняемыми вспомогательной функцией тега.</span><span class="sxs-lookup"><span data-stu-id="f42ab-129">The overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="f42ab-130">Класс `TagHelper` также предоставляет асинхронную версию (`ProcessAsync`) с теми же параметрами.</span><span class="sxs-lookup"><span data-stu-id="f42ab-130">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="f42ab-131">Параметр контекста метода `Process` (и `ProcessAsync`) содержит сведения, связанные с выполнением текущего HTML-тега.</span><span class="sxs-lookup"><span data-stu-id="f42ab-131">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="f42ab-132">Выходной параметр метода `Process` (и `ProcessAsync`) содержит элемент HTML с отслеживанием состояния, который представляет источник, используемый для формирования HTML-тега и содержимого.</span><span class="sxs-lookup"><span data-stu-id="f42ab-132">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="f42ab-133">В нашем случае имя класса имеет суффикс **TagHelper**, который *не* является обязательным, но считается рекомендуемым соглашением.</span><span class="sxs-lookup"><span data-stu-id="f42ab-133">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="f42ab-134">Класс можно объявить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f42ab-134">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="f42ab-135">Чтобы сделать класс `EmailTagHelper` доступным для всех представлений Razor, добавьте директиву `addTagHelper` в файл *Views/_ViewImports.cshtml*: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="f42ab-135">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="f42ab-136">В приведенном выше коде используется синтаксис с подстановочным знаком, который указывает, что будут доступны все вспомогательные функции тегов в сборке.</span><span class="sxs-lookup"><span data-stu-id="f42ab-136">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="f42ab-137">Первая строка после `@addTagHelper` указывает на загружаемую вспомогательную функцию тега (символ "\*" соответствует всем вспомогательным функциям тегов), а вторая строка ("AuthoringTagHelpers") указывает на сборку, в которой находится вспомогательная функция тега.</span><span class="sxs-lookup"><span data-stu-id="f42ab-137">The first string after `@addTagHelper` specifies the tag helper to load (Use "\*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="f42ab-138">Кроме того, обратите внимание на то, что во второй строке с помощью синтаксиса с подстановочным знаком добавляются вспомогательные функции тегов ASP.NET Core MVC (эти вспомогательные функции рассматриваются в статье [Общие сведения о вспомогательных функциях тегов](intro.md)). Вспомогательная функция тега становится доступной для представления Razor посредством директивы `@addTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-138">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="f42ab-139">Вы также можете указать полное имя вспомогательной функции тега, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="f42ab-139">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
<span data-ttu-id="f42ab-140">Чтобы добавить вспомогательную функцию тега в представление с помощью полного имени, сначала добавьте полное имя (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), а затем — имя сборки (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="f42ab-140">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="f42ab-141">Большинство разработчиков предпочитают использовать синтаксис с подстановочным знаком.</span><span class="sxs-lookup"><span data-stu-id="f42ab-141">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="f42ab-142">В статье [Общие сведения о вспомогательных функциях тегов](intro.md) подробно рассматривается добавление и удаление вспомогательных функций тегов, их иерархия и синтаксис с подстановочным знаком.</span><span class="sxs-lookup"><span data-stu-id="f42ab-142">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="f42ab-143">Внесите следующие изменения в разметку в файле *Views/Home/Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f42ab-143">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  <span data-ttu-id="f42ab-144">Запустите приложение и просмотрите исходный текст HTML в любимом браузере, чтобы проверить, были ли теги электронной почты заменены разметкой привязки (например, `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="f42ab-144">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="f42ab-145">Строки *Support* и *Marketing* отображаются как ссылки, но у них нет атрибута `href`, поэтому ссылки будут нерабочими.</span><span class="sxs-lookup"><span data-stu-id="f42ab-145">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="f42ab-146">Мы исправим это в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="f42ab-146">We'll fix that in the next section.</span></span>

<span data-ttu-id="f42ab-147">Примечание. Так же как в HTML-тегах и атрибутах, в тегах, именах классов и атрибутов в Razor и C# регистр символов не учитывается.</span><span class="sxs-lookup"><span data-stu-id="f42ab-147">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="f42ab-148">SetAttribute и SetContent</span><span class="sxs-lookup"><span data-stu-id="f42ab-148">SetAttribute and SetContent</span></span>

<span data-ttu-id="f42ab-149">В этом разделе мы изменим функцию `EmailTagHelper` так, чтобы она создавала правильный тег привязки для адреса электронной почты.</span><span class="sxs-lookup"><span data-stu-id="f42ab-149">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="f42ab-150">Она будет извлекать сведения из представления Razor (в виде атрибута `mail-to`) и использовать их для создания привязки.</span><span class="sxs-lookup"><span data-stu-id="f42ab-150">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="f42ab-151">Измените класс `EmailTagHelper` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f42ab-151">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="f42ab-152">**Примечания.**</span><span class="sxs-lookup"><span data-stu-id="f42ab-152">**Notes:**</span></span>

* <span data-ttu-id="f42ab-153">Имена классов и свойств, указанные в стиле Pascal, для вспомогательных функций тегов преобразуются в [кебаб-стиль нижнего регистра](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="f42ab-153">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="f42ab-154">Поэтому для использования атрибута `MailTo` следует использовать эквивалент `<email mail-to="value"/>`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-154">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="f42ab-155">В последней строке задается готовое содержимое для нашей простейшей функциональной вспомогательной функции тега.</span><span class="sxs-lookup"><span data-stu-id="f42ab-155">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="f42ab-156">В выделенной строке показан синтаксис добавления атрибутов:</span><span class="sxs-lookup"><span data-stu-id="f42ab-156">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="f42ab-157">Такой подход работает для атрибута href, если его в настоящее время нет в коллекции атрибутов.</span><span class="sxs-lookup"><span data-stu-id="f42ab-157">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="f42ab-158">Вы также можете добавить атрибут вспомогательной функции тега в конец коллекции атрибутов тегов с помощью метода `output.Attributes.Add`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-158">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="f42ab-159">Внесите следующие изменения в разметку в файле *Views/Home/Contact.cshtml*: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="f42ab-159">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="f42ab-160">Запустите приложение и убедитесь в том, что ссылки создаются правильно.</span><span class="sxs-lookup"><span data-stu-id="f42ab-160">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="f42ab-161">Если бы вам нужно было написать самозакрывающийся тег электронной почты (`<email mail-to="Rick" />`), выходной тег также был бы самозакрывающимся.</span><span class="sxs-lookup"><span data-stu-id="f42ab-161">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="f42ab-162">Чтобы получить возможность создавать теги с помощью только открывающего тега (`<email mail-to="Rick">`), класс необходимо декорировать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f42ab-162">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    <span data-ttu-id="f42ab-163">При использовании самозакрывающейся вспомогательной функции тега электронной почты результат будет следующим: `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-163">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="f42ab-164">Самозакрывающиеся теги привязки не являются допустимым кодом HTML, поэтому создавать их, как правило, нежелательно, но вам может потребоваться создать самозакрывающуюся вспомогательную функцию тега.</span><span class="sxs-lookup"><span data-stu-id="f42ab-164">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that's self-closing.</span></span> <span data-ttu-id="f42ab-165">Вспомогательные функции тегов задают тип свойства `TagMode` после считывания тега.</span><span class="sxs-lookup"><span data-stu-id="f42ab-165">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="f42ab-166">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="f42ab-166">ProcessAsync</span></span>

<span data-ttu-id="f42ab-167">В этом разделе мы напишем асинхронную вспомогательную функцию тега электронной почты.</span><span class="sxs-lookup"><span data-stu-id="f42ab-167">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="f42ab-168">Замените класс `EmailTagHelper` на следующий код:</span><span class="sxs-lookup"><span data-stu-id="f42ab-168">Replace the `EmailTagHelper` class with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    <span data-ttu-id="f42ab-169">**Примечания.**</span><span class="sxs-lookup"><span data-stu-id="f42ab-169">**Notes:**</span></span>

    * <span data-ttu-id="f42ab-170">В этой версии используется асинхронный метод `ProcessAsync`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-170">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="f42ab-171">Асинхронный метод `GetChildContentAsync` возвращает объект `Task`, содержащий `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-171">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="f42ab-172">Для получения содержимого элемента HTML используйте параметр `output`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-172">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="f42ab-173">Чтобы вспомогательная функция тега могла получить целевой адрес электронной почты, внесите следующее изменение в файл *Views/Home/Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f42ab-173">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  <span data-ttu-id="f42ab-174">Запустите приложение и убедитесь в том, что ссылки электронной почты создаются правильно.</span><span class="sxs-lookup"><span data-stu-id="f42ab-174">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="f42ab-175">RemoveAll, PreContent.SetHtmlContent и PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="f42ab-175">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="f42ab-176">Добавьте приведенный ниже класс `BoldTagHelper` в папку *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="f42ab-176">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    <span data-ttu-id="f42ab-177">**Примечания.**</span><span class="sxs-lookup"><span data-stu-id="f42ab-177">**Notes:**</span></span>
    
    * <span data-ttu-id="f42ab-178">Атрибут `[HtmlTargetElement]` передает параметр атрибута, который указывает, что любой элемент HTML, содержащий атрибут HTML с именем bold, будет соответствовать условиям и метод переопределения `Process` в классе будет выполняться.</span><span class="sxs-lookup"><span data-stu-id="f42ab-178">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="f42ab-179">В этом примере метод `Process` удаляет атрибут bold и заключает содержащую разметку в теги `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-179">In our sample, the `Process` method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="f42ab-180">Так как заменять существующее содержимое тега не нужно, открывающий тег `<strong>` должен записываться с помощью метода `PreContent.SetHtmlContent`, а закрывающий тег `</strong>` — с помощью метода `PostContent.SetHtmlContent`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-180">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="f42ab-181">Измените представление *About.cshtml* так, чтобы оно содержало значение атрибута `bold`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-181">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="f42ab-182">Ниже приведен готовый код.</span><span class="sxs-lookup"><span data-stu-id="f42ab-182">The completed code is shown below.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  <span data-ttu-id="f42ab-183">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f42ab-183">Run the app.</span></span> <span data-ttu-id="f42ab-184">Для проверки исходного кода и разметки можно использовать любой браузер на ваш выбор.</span><span class="sxs-lookup"><span data-stu-id="f42ab-184">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="f42ab-185">Представленный выше атрибут `[HtmlTargetElement]` предназначен только для разметки HTML, которая предоставляет имя атрибута bold.</span><span class="sxs-lookup"><span data-stu-id="f42ab-185">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="f42ab-186">Элемент `<bold>` не был изменен вспомогательной функцией тега.</span><span class="sxs-lookup"><span data-stu-id="f42ab-186">The `<bold>` element wasn't modified by the tag helper.</span></span>

4. <span data-ttu-id="f42ab-187">Если закомментировать строку с атрибутом `[HtmlTargetElement]`, функция будет нацелена по умолчанию на теги `<bold>`, то есть на разметку HTML в форме `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-187">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="f42ab-188">Помните, что в соответствии с соглашением об именовании по умолчанию имя класса **Bold**TagHelper соответствует тегам `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-188">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="f42ab-189">Запустите приложение и убедитесь в том, что тег `<bold>` обрабатывается вспомогательной функцией тега.</span><span class="sxs-lookup"><span data-stu-id="f42ab-189">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="f42ab-190">Если декорировать класс несколькими атрибутами `[HtmlTargetElement]`, к целевым тегам применяется логическая операция ИЛИ.</span><span class="sxs-lookup"><span data-stu-id="f42ab-190">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="f42ab-191">Например, в приведенном ниже коде условиям будет соответствовать тег bold или атрибут bold.</span><span class="sxs-lookup"><span data-stu-id="f42ab-191">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="f42ab-192">Если в один оператор добавляются несколько атрибутов, среда выполнения применяет к ним логическую операцию И.</span><span class="sxs-lookup"><span data-stu-id="f42ab-192">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="f42ab-193">Например, в приведенном ниже коде элемент HTML будет соответствовать условиям, если он имеет имя bold и имеет атрибут с именем bold (`<bold bold />`).</span><span class="sxs-lookup"><span data-stu-id="f42ab-193">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="f42ab-194">С помощью `[HtmlTargetElement]` можно также изменить имя целевого элемента.</span><span class="sxs-lookup"><span data-stu-id="f42ab-194">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="f42ab-195">Например, чтобы вспомогательная функция `BoldTagHelper` предназначалась для тегов `<MyBold>`, используйте следующий атрибут:</span><span class="sxs-lookup"><span data-stu-id="f42ab-195">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a><span data-ttu-id="f42ab-196">Передача модели во вспомогательную функцию тега</span><span class="sxs-lookup"><span data-stu-id="f42ab-196">Pass a model to a Tag Helper</span></span>

1.  <span data-ttu-id="f42ab-197">Добавьте папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="f42ab-197">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="f42ab-198">Добавьте следующий класс `WebsiteContext` в папку *Models*:</span><span class="sxs-lookup"><span data-stu-id="f42ab-198">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  <span data-ttu-id="f42ab-199">Добавьте приведенный ниже класс `WebsiteInformationTagHelper` в папку *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="f42ab-199">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    <span data-ttu-id="f42ab-200">**Примечания.**</span><span class="sxs-lookup"><span data-stu-id="f42ab-200">**Notes:**</span></span>
    
    * <span data-ttu-id="f42ab-201">Как было сказано ранее, имена классов и свойств C# в стиле Pascal для вспомогательных функций тегов преобразуются в [кебаб-стиль нижнего регистра](http://wiki.c2.com/?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="f42ab-201">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="f42ab-202">Поэтому для использования функции `WebsiteInformationTagHelper` в Razor необходимо написать `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-202">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="f42ab-203">Целевой элемент не определяется напрямую с помощью атрибута `[HtmlTargetElement]`, поэтому по умолчанию целевым будет элемент `website-information`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-203">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="f42ab-204">Например, вы применяете следующий атрибут (обратите внимание, что он не в стиле кебаб, но соответствует имени класса):</span><span class="sxs-lookup"><span data-stu-id="f42ab-204">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="f42ab-205">Ему не будет соответствовать тег в стиле кебаб нижнего регистра `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-205">The lower kebab case tag `<website-information />` wouldn't match.</span></span> <span data-ttu-id="f42ab-206">Чтобы использовать атрибут `[HtmlTargetElement]`, следует применить стиль кебаб, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="f42ab-206">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="f42ab-207">Самозакрывающиеся элементы не имеют содержимого.</span><span class="sxs-lookup"><span data-stu-id="f42ab-207">Elements that are self-closing have no content.</span></span> <span data-ttu-id="f42ab-208">В этом примере в разметке Razor используется самозакрывающийся тег, но вспомогательная функция тега будет создавать элемент [section](http://www.w3.org/TR/html5/sections.html#the-section-element) (который не является самозакрывающимся, и вы добавляете содержимое внутри элемента `section`).</span><span class="sxs-lookup"><span data-stu-id="f42ab-208">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which isn't self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="f42ab-209">Поэтому для записи выходных данных следует присвоить свойству `TagMode` значение `StartTagAndEndTag`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-209">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="f42ab-210">Кроме того, можно закомментировать строку, в которой задается `TagMode`, и написать разметку с закрывающим тегом.</span><span class="sxs-lookup"><span data-stu-id="f42ab-210">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="f42ab-211">(Пример разметки приводится далее в этом руководстве.)</span><span class="sxs-lookup"><span data-stu-id="f42ab-211">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="f42ab-212">Символ `$` (знак доллара) в приведенной ниже строке определяет использование [интерполированной строки](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings).</span><span class="sxs-lookup"><span data-stu-id="f42ab-212">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="f42ab-213">Добавьте приведенную ниже разметку в представление *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f42ab-213">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="f42ab-214">Выделенная разметка служит для вывода сведений о веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="f42ab-214">The highlighted markup displays the web site information.</span></span>
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > <span data-ttu-id="f42ab-215">В Razor используйте следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="f42ab-215">In the Razor markup shown below:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    ><span data-ttu-id="f42ab-216">Razor известно, что атрибут `info` — это класс, а не строка, и что вы хотите написать код C#.</span><span class="sxs-lookup"><span data-stu-id="f42ab-216">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="f42ab-217">Любой нестроковый атрибут вспомогательной функции тега должен быть записан без символа `@`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-217">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="f42ab-218">Запустите приложение и перейдите к представлению About, чтобы просмотреть сведения о веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="f42ab-218">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="f42ab-219">Вы можете использовать следующую разметку с закрывающим тегом и удалить строку со свойством `TagMode.StartTagAndEndTag` во вспомогательной функции тега:</span><span class="sxs-lookup"><span data-stu-id="f42ab-219">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="f42ab-220">Вспомогательная функция тега условия</span><span class="sxs-lookup"><span data-stu-id="f42ab-220">Condition Tag Helper</span></span>

<span data-ttu-id="f42ab-221">Вспомогательная функция тега условия преобразовывает выходные данные для просмотра, если в нее передано значение true.</span><span class="sxs-lookup"><span data-stu-id="f42ab-221">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="f42ab-222">Добавьте приведенный ниже класс `ConditionTagHelper` в папку *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="f42ab-222">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  <span data-ttu-id="f42ab-223">Замените содержимое файла *Views/Home/Index.cshtml* следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="f42ab-223">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=@Model />
        <div condition="@Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="f42ab-224">Замените метод `Index` в контроллере `Home` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="f42ab-224">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  <span data-ttu-id="f42ab-225">Запустите приложение и перейдите на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="f42ab-225">Run the app and browse to the home page.</span></span> <span data-ttu-id="f42ab-226">Разметка в условном теге `div` не будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="f42ab-226">The markup in the conditional `div` won't be rendered.</span></span> <span data-ttu-id="f42ab-227">Добавьте строку запроса `?approved=true` к URL-адресу (например, `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="f42ab-227">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="f42ab-228">`approved` принимает значение true, и условная разметка будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="f42ab-228">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="f42ab-229">Для определения целевого атрибута используйте оператор [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof), а не указывайте строку, как в случае со вспомогательной функцией тега bold:</span><span class="sxs-lookup"><span data-stu-id="f42ab-229">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
><span data-ttu-id="f42ab-230">Оператор [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) защищает код в случае его рефакторинга (имя может потребоваться изменить на `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="f42ab-230">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoid-tag-helper-conflicts"></a><span data-ttu-id="f42ab-231">Как избежать конфликтов между вспомогательными функциями тегов</span><span class="sxs-lookup"><span data-stu-id="f42ab-231">Avoid Tag Helper conflicts</span></span>

<span data-ttu-id="f42ab-232">В этом разделе вы напишете пару вспомогательных функций тегов для автоматического связывания.</span><span class="sxs-lookup"><span data-stu-id="f42ab-232">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="f42ab-233">Первая функция будет заменять разметку, которая содержит URL-адрес, начинающийся с префикса HTTP, на HTML-тег привязки, содержащий тот же URL-адрес (то есть предоставляющий ссылку на URL-адрес).</span><span class="sxs-lookup"><span data-stu-id="f42ab-233">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="f42ab-234">Вторая функция будет делать то же самое для URL-адреса, начинающегося с WWW.</span><span class="sxs-lookup"><span data-stu-id="f42ab-234">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="f42ab-235">Так как эти две вспомогательные функции тесно связаны друг с другом и в будущем может потребоваться их рефакторинг, мы поместим их в один файл.</span><span class="sxs-lookup"><span data-stu-id="f42ab-235">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="f42ab-236">Добавьте приведенный ниже класс `AutoLinkerHttpTagHelper` в папку *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="f42ab-236">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    ><span data-ttu-id="f42ab-237">Для класса `AutoLinkerHttpTagHelper` целевыми являются элементы `p`, и в нем используется [регулярное выражение](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) для создания привязки.</span><span class="sxs-lookup"><span data-stu-id="f42ab-237">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2.  <span data-ttu-id="f42ab-238">Добавьте следующую разметку в конец файла *Views/Home/Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f42ab-238">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  <span data-ttu-id="f42ab-239">Запустите приложение и проверьте, правильно ли вспомогательная функция тега отображает привязку.</span><span class="sxs-lookup"><span data-stu-id="f42ab-239">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="f42ab-240">Обновите класс `AutoLinker`, включив в него функцию `AutoLinkerWwwTagHelper`, которая будет преобразовывать текст с префиксом WWW в тег привязки, который также содержит исходный текст с префиксом WWW.</span><span class="sxs-lookup"><span data-stu-id="f42ab-240">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="f42ab-241">Обновленный код выделен ниже.</span><span class="sxs-lookup"><span data-stu-id="f42ab-241">The updated code is highlighted below:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  <span data-ttu-id="f42ab-242">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f42ab-242">Run the app.</span></span> <span data-ttu-id="f42ab-243">Обратите внимание на то, что текст с префиксом WWW отображается как ссылка, а текст с префиксом HTTP — нет.</span><span class="sxs-lookup"><span data-stu-id="f42ab-243">Notice the www text is rendered as a link but the HTTP text isn't.</span></span> <span data-ttu-id="f42ab-244">Если установить точку останова в обоих классах, то можно увидеть, что класс вспомогательной функции тега HTTP выполняется первым.</span><span class="sxs-lookup"><span data-stu-id="f42ab-244">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="f42ab-245">Проблема в том, что выходные данные вспомогательной функции тега кэшируются и, когда выполняется вспомогательная функция тега WWW, она перезаписывает кэшированные выходные данные вспомогательной функции тега HTTP.</span><span class="sxs-lookup"><span data-stu-id="f42ab-245">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="f42ab-246">Далее в этом руководстве мы узнаем, как управлять очередностью выполнения вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="f42ab-246">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="f42ab-247">Исправим код следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f42ab-247">We'll fix the code with the following:</span></span>

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    ><span data-ttu-id="f42ab-248">В первом выпуске вспомогательных функций тегов с автоматическим связыванием для получения содержимого целевых элементов использовался следующий код:</span><span class="sxs-lookup"><span data-stu-id="f42ab-248">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    ><span data-ttu-id="f42ab-249">То есть вызывался метод `GetChildContentAsync` с передачей объекта `TagHelperOutput` в метод `ProcessAsync`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-249">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="f42ab-250">Как было сказано ранее, из-за кэширования выходных данных приоритет имеет последняя выполняемая вспомогательная функция тега.</span><span class="sxs-lookup"><span data-stu-id="f42ab-250">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="f42ab-251">Эта проблема исправлялась с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="f42ab-251">You fixed that problem with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    ><span data-ttu-id="f42ab-252">Приведенный выше код проверяет, было ли содержимое изменено. Если да, он получает содержимое из выходного буфера.</span><span class="sxs-lookup"><span data-stu-id="f42ab-252">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="f42ab-253">Запустите приложение и проверьте, правильно ли работают обе ссылки.</span><span class="sxs-lookup"><span data-stu-id="f42ab-253">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="f42ab-254">Может показаться, что теперь вспомогательная функция тега с автоматическим связыванием полностью готова и работает правильно, однако есть одна деталь.</span><span class="sxs-lookup"><span data-stu-id="f42ab-254">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="f42ab-255">Если вспомогательная функция тега WWW выполняется первой, ссылки с префиксом WWW будут неправильными.</span><span class="sxs-lookup"><span data-stu-id="f42ab-255">If the WWW tag helper runs first, the www links won't be correct.</span></span> <span data-ttu-id="f42ab-256">Измените код, добавив перегрузку `Order` для управления очередностью выполнения вспомогательных функций.</span><span class="sxs-lookup"><span data-stu-id="f42ab-256">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="f42ab-257">Свойство `Order` определяет очередность выполнения относительно других вспомогательных функций тегов с тем же целевым элементом.</span><span class="sxs-lookup"><span data-stu-id="f42ab-257">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="f42ab-258">Значение очередности по умолчанию — ноль. Экземпляры с более низкими значениями выполняются в первую очередь.</span><span class="sxs-lookup"><span data-stu-id="f42ab-258">The default order value is zero and instances with lower values are executed first.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    <span data-ttu-id="f42ab-259">Приведенный выше код гарантирует, что вспомогательная функция тега HTTP будет выполняться перед вспомогательной функцией тега WWW.</span><span class="sxs-lookup"><span data-stu-id="f42ab-259">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="f42ab-260">Измените значение `Order` на `MaxValue` и убедитесь в том, что для тега WWW разметка создается неправильно.</span><span class="sxs-lookup"><span data-stu-id="f42ab-260">Change `Order` to `MaxValue` and verify that the markup generated for the WWW tag is incorrect.</span></span>

## <a name="inspect-and-retrieve-child-content"></a><span data-ttu-id="f42ab-261">Проверка и извлечение содержимого дочернего элемента</span><span class="sxs-lookup"><span data-stu-id="f42ab-261">Inspect and retrieve child content</span></span>

<span data-ttu-id="f42ab-262">Вспомогательные функции тегов предоставляют несколько свойств для извлечения содержимого.</span><span class="sxs-lookup"><span data-stu-id="f42ab-262">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="f42ab-263">Результат выполнения метода `GetChildContentAsync` можно добавить к `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-263">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="f42ab-264">Результат выполнения метода `GetChildContentAsync` можно проверить с помощью `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="f42ab-264">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="f42ab-265">При изменении `output.Content` тело функции TagHelper будет выполняться или обрабатываться, только если вызвать метод `GetChildContentAsync`, как в нашем примере автоматического связывания.</span><span class="sxs-lookup"><span data-stu-id="f42ab-265">If you modify `output.Content`, the TagHelper body won't be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="f42ab-266">При нескольких вызовах `GetChildContentAsync` возвращается одно и то же значение и тело функции `TagHelper` не выполняется повторно, если только не передать параметр false, который указывает на то, что не следует использовать кэшированный результат.</span><span class="sxs-lookup"><span data-stu-id="f42ab-266">Multiple calls to `GetChildContentAsync` returns the same value and doesn't re-execute the `TagHelper` body unless you pass in a false parameter indicating not to use the cached result.</span></span>
