---
title: "Создание вспомогательных функций тегов в ASP.NET Core"
author: rick-anderson
description: "Сведения о разработке вспомогательных функций тегов в ASP.NET Core."
keywords: "ASP.NET Core, вспомогательных функций тегов"
ms.author: riande
manager: wpickett
ms.date: 6/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f16af1184a29b891a9aab0b38ab833836c326c44
ms.sourcegitcommit: e6a8f171f26fab1b2195a2d7f14e7d258a2e690e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/23/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="9c40b-104">Создание вспомогательных функций тегов в ASP.NET Core, пошаговое руководство с примерами</span><span class="sxs-lookup"><span data-stu-id="9c40b-104">Authoring Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="9c40b-105">По [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9c40b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="9c40b-106">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="9c40b-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)

## <a name="getting-started-with-tag-helpers"></a><span data-ttu-id="9c40b-107">Приступая к работе с вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="9c40b-107">Getting started with Tag Helpers</span></span>

<span data-ttu-id="9c40b-108">Этот учебник содержит вводные сведения о программирования вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="9c40b-108">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="9c40b-109">[Общие сведения о вспомогательных функций тегов](intro.md) описываются преимущества, которые предоставляют вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="9c40b-109">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="9c40b-110">Вспомогательный объект тег является любой класс, реализующий `ITagHelper` интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9c40b-110">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="9c40b-111">Тем не менее, при создании вспомогательных тег вы обычно являются производными от `TagHelper`, выполнив Да предоставляет вам доступ к `Process` метод.</span><span class="sxs-lookup"><span data-stu-id="9c40b-111">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="9c40b-112">Создайте новый проект ASP.NET Core с именем **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="9c40b-112">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="9c40b-113">Нет необходимости проверки подлинности для этого проекта.</span><span class="sxs-lookup"><span data-stu-id="9c40b-113">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="9c40b-114">Создайте папку для хранения вспомогательных функций тегов, которая называется *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="9c40b-114">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="9c40b-115">*TagHelpers* папка *не* обязательно, но это разумного соглашение.</span><span class="sxs-lookup"><span data-stu-id="9c40b-115">The *TagHelpers* folder is *not* required, but it is a reasonable convention.</span></span> <span data-ttu-id="9c40b-116">Теперь давайте приступить к написанию Некоторые помощники тега в краткой форме.</span><span class="sxs-lookup"><span data-stu-id="9c40b-116">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="9c40b-117">Вспомогательный объект минимальной тега</span><span class="sxs-lookup"><span data-stu-id="9c40b-117">A minimal Tag Helper</span></span>

<span data-ttu-id="9c40b-118">В этом разделе запись помощник тег, который обновляет тег электронной почты.</span><span class="sxs-lookup"><span data-stu-id="9c40b-118">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="9c40b-119">Пример:</span><span class="sxs-lookup"><span data-stu-id="9c40b-119">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="9c40b-120">Сервер будет использовать наши вспомогательные тег электронной почты для преобразования, разметку в следующее:</span><span class="sxs-lookup"><span data-stu-id="9c40b-120">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

<span data-ttu-id="9c40b-121">То есть тег, делает это ссылку в электронной почте.</span><span class="sxs-lookup"><span data-stu-id="9c40b-121">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="9c40b-122">Может потребоваться в случае, если вы пишете механизмом поддержки блогов, для отправки электронной почты для отдела маркетинга, поддержки и другие контакты все в одном домене.</span><span class="sxs-lookup"><span data-stu-id="9c40b-122">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="9c40b-123">Добавьте следующие `EmailTagHelper` класса *TagHelpers* папки.</span><span class="sxs-lookup"><span data-stu-id="9c40b-123">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="9c40b-124">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-124">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]</span></span>
    
    <span data-ttu-id="9c40b-125">**Примечания.**</span><span class="sxs-lookup"><span data-stu-id="9c40b-125">**Notes:**</span></span>
    
    * <span data-ttu-id="9c40b-126">Тег вспомогательные методы используют соглашение об именовании, предназначенного элементы корневого имени класса (минус *вспомогательной функции тегов* часть имени класса).</span><span class="sxs-lookup"><span data-stu-id="9c40b-126">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="9c40b-127">В этом примере имя корневой папки **электронной почты**является вспомогательной функции тегов *электронной почты*, поэтому `<email>` целевые тег.</span><span class="sxs-lookup"><span data-stu-id="9c40b-127">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="9c40b-128">Это соглашение об именовании подойдут для большинства вспомогательных функций тегов, впоследствии будет показано как переопределить его.</span><span class="sxs-lookup"><span data-stu-id="9c40b-128">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="9c40b-129">Класс `EmailTagHelper` является производным от класса `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="9c40b-129">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="9c40b-130">`TagHelper` Класс предоставляет методы и свойства для записи вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="9c40b-130">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="9c40b-131">Переопределенный `Process` метод управляет назначение вспомогательный тег при выполнении.</span><span class="sxs-lookup"><span data-stu-id="9c40b-131">The  overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="9c40b-132">`TagHelper` Класс также предоставляет асинхронную версию (`ProcessAsync`) с теми же параметрами.</span><span class="sxs-lookup"><span data-stu-id="9c40b-132">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="9c40b-133">Параметр контекста для `Process` (и `ProcessAsync`) содержит сведения, связанные с выполнением текущего HTML-тега.</span><span class="sxs-lookup"><span data-stu-id="9c40b-133">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="9c40b-134">Выходной параметр, чтобы `Process` (и `ProcessAsync`) содержит элемент с отслеживанием состояния HTML отражает исходного источника, используемый для создания HTML-тег и содержимое.</span><span class="sxs-lookup"><span data-stu-id="9c40b-134">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="9c40b-135">Наш имя класса содержит суффикс **вспомогательной функции тегов**, который является *не* требуется, но он считается наилучшим соглашением рекомендаций.</span><span class="sxs-lookup"><span data-stu-id="9c40b-135">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="9c40b-136">Можно объявить класс как:</span><span class="sxs-lookup"><span data-stu-id="9c40b-136">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="9c40b-137">Чтобы сделать `EmailTagHelper` класса для наших представлений Razor, добавьте `addTagHelper` директиву *Views/_ViewImports.cshtml* файла: [!code-html [Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-137">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="9c40b-138">Приведенный выше код использует синтаксисе знаков подстановки для указания всех вспомогательных функций тегов в нашем сборки будет доступно.</span><span class="sxs-lookup"><span data-stu-id="9c40b-138">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="9c40b-139">Первая строка после `@addTagHelper` указывает тег вспомогательное приложение для загрузки (используйте «*» для всех вспомогательных функций тегов), и вторая строка «AuthoringTagHelpers» указывает тег модуль сборки.</span><span class="sxs-lookup"><span data-stu-id="9c40b-139">The first string after `@addTagHelper` specifies the tag helper to load (Use "*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="9c40b-140">Кроме того, обратите внимание, что вторая строка переносит вспомогательных функций тегов Core ASP.NET MVC с помощью синтаксиса подстановочный знак (Эти вспомогательные методы обсуждаются в [Общие сведения о вспомогательных функций тегов](intro.md).) Это `@addTagHelper` директиву, которая делает доступными для представления Razor вспомогательный тег.</span><span class="sxs-lookup"><span data-stu-id="9c40b-140">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="9c40b-141">Кроме того можно указать полное доменное имя (FQN) тега вспомогательного метода, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="9c40b-141">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
    <span data-ttu-id="9c40b-142">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-142">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]</span></span>
    
    <span data-ttu-id="9c40b-143">Чтобы добавить тег вспомогательный класс для представления с помощью FQN, сначала добавьте FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), а затем имя сборки (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="9c40b-143">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="9c40b-144">Большинство разработчиков будет предпочитают использовать синтаксисе знаков подстановки.</span><span class="sxs-lookup"><span data-stu-id="9c40b-144">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="9c40b-145">[Общие сведения о вспомогательных функций тегов](intro.md) попадают в подробности синтаксиса тегов вспомогательный добавления, удаления, иерархии и подстановочный знак.</span><span class="sxs-lookup"><span data-stu-id="9c40b-145">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="9c40b-146">Обновить разметки в *Views/Home/Contact.cshtml* файла с учетом внесенных изменений:</span><span class="sxs-lookup"><span data-stu-id="9c40b-146">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    <span data-ttu-id="9c40b-147">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-147">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span></span>

4.  <span data-ttu-id="9c40b-148">Запустите приложение и использовать любимом браузере для просмотра исходного кода HTML, чтобы можно было удостовериться, что теги электронной почты заменяются разметки привязки (например, `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="9c40b-148">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="9c40b-149">*Поддержка* и *маркетинга* отображаются как ссылки, но они не имеют `href` атрибут, чтобы сделать их работы.</span><span class="sxs-lookup"><span data-stu-id="9c40b-149">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="9c40b-150">Мы исправим это в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="9c40b-150">We'll fix that in the next section.</span></span>

<span data-ttu-id="9c40b-151">Примечание: Как HTML-теги и атрибуты, теги, имена классов и атрибутов в Razor и C# не учитывают регистр.</span><span class="sxs-lookup"><span data-stu-id="9c40b-151">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="9c40b-152">SetAttribute и SetContent</span><span class="sxs-lookup"><span data-stu-id="9c40b-152">SetAttribute and SetContent</span></span>

<span data-ttu-id="9c40b-153">В этом разделе мы обновим `EmailTagHelper` , чтобы он создает допустимый тег для электронной почты.</span><span class="sxs-lookup"><span data-stu-id="9c40b-153">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="9c40b-154">Мы обновим принимают данные из представления Razor (в виде `mail-to` атрибут) и использовать его при создании привязки.</span><span class="sxs-lookup"><span data-stu-id="9c40b-154">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="9c40b-155">Обновление `EmailTagHelper` класса следующими строками:</span><span class="sxs-lookup"><span data-stu-id="9c40b-155">Update the `EmailTagHelper` class with the following:</span></span>

<span data-ttu-id="9c40b-156">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-156">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]</span></span>

<span data-ttu-id="9c40b-157">**Примечания.**</span><span class="sxs-lookup"><span data-stu-id="9c40b-157">**Notes:**</span></span>

* <span data-ttu-id="9c40b-158">Преобразуется в стиле Pascal имена классов и свойств для вспомогательных функций тегов их [нижний регистр kebab](http://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101#12273101).</span><span class="sxs-lookup"><span data-stu-id="9c40b-158">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](http://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101#12273101).</span></span> <span data-ttu-id="9c40b-159">Таким образом Чтобы использовать `MailTo` атрибут, вы воспользуетесь `<email mail-to="value"/>` эквивалент.</span><span class="sxs-lookup"><span data-stu-id="9c40b-159">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="9c40b-160">Последняя строка задает завершенного содержимое для наших минимально функциональной тег вспомогательного метода.</span><span class="sxs-lookup"><span data-stu-id="9c40b-160">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="9c40b-161">Выделенная строка показан синтаксис для добавления атрибутов:</span><span class="sxs-lookup"><span data-stu-id="9c40b-161">The highlighted line shows the syntax for adding attributes:</span></span>

<span data-ttu-id="9c40b-162">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-162">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]</span></span>

<span data-ttu-id="9c40b-163">Этот способ подходит для атрибута «href», пока он не существует в настоящее время в коллекции атрибутов.</span><span class="sxs-lookup"><span data-stu-id="9c40b-163">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="9c40b-164">Можно также использовать `output.Attributes.Add` метод, чтобы добавить атрибут вспомогательной функции тегов в конец коллекции атрибутов тега.</span><span class="sxs-lookup"><span data-stu-id="9c40b-164">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="9c40b-165">Обновить разметки в *Views/Home/Contact.cshtml* файла с учетом внесенных изменений: [!code-html [Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-165">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="9c40b-166">Запустите приложение и убедитесь, что он создает правильные ссылки.</span><span class="sxs-lookup"><span data-stu-id="9c40b-166">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="9c40b-167">В случае записи тег электронной почты самостоятельно закрытие (`<email mail-to="Rick" />`), также будут самозакрывающегося конечного результата.</span><span class="sxs-lookup"><span data-stu-id="9c40b-167">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="9c40b-168">Чтобы включить возможность записать тег с помощью открывающего тега (`<email mail-to="Rick">`) необходимо дополнить класса на следующий:</span><span class="sxs-lookup"><span data-stu-id="9c40b-168">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > <span data-ttu-id="9c40b-169">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-169">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]</span></span>
    
    <span data-ttu-id="9c40b-170">С самозакрывающегося тега вспомогательный электронной почты, результатом будет `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="9c40b-170">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="9c40b-171">Требуется закрывать теги привязки не допустимый HTML-код, поэтому не нужно создавать его, но может потребоваться создать помощник тег, который не требуется закрывать.</span><span class="sxs-lookup"><span data-stu-id="9c40b-171">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that is self-closing.</span></span> <span data-ttu-id="9c40b-172">Вспомогательных функций тегов задать тип `TagMode` свойство после прочтения тег.</span><span class="sxs-lookup"><span data-stu-id="9c40b-172">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="9c40b-173">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="9c40b-173">ProcessAsync</span></span>

<span data-ttu-id="9c40b-174">В этом разделе мы записываем вспомогательный асинхронной электронной почты.</span><span class="sxs-lookup"><span data-stu-id="9c40b-174">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="9c40b-175">Замените `EmailTagHelper` класса следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="9c40b-175">Replace the `EmailTagHelper` class with the following code:</span></span>

    <span data-ttu-id="9c40b-176">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-176">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]</span></span>

    <span data-ttu-id="9c40b-177">**Примечания.**</span><span class="sxs-lookup"><span data-stu-id="9c40b-177">**Notes:**</span></span>

    * <span data-ttu-id="9c40b-178">Эта версия использует асинхронную `ProcessAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="9c40b-178">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="9c40b-179">Асинхронная `GetChildContentAsync` возвращает `Task` содержащий `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="9c40b-179">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="9c40b-180">Используйте `output` для получения содержимого элемента HTML.</span><span class="sxs-lookup"><span data-stu-id="9c40b-180">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="9c40b-181">Внесите следующее изменение для *Views/Home/Contact.cshtml* файл, поэтому вспомогательные тег можно получить по электронной почте целевой.</span><span class="sxs-lookup"><span data-stu-id="9c40b-181">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    <span data-ttu-id="9c40b-182">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-182">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span></span>

3.  <span data-ttu-id="9c40b-183">Запустите приложение и убедитесь, он создает ссылки на допустимый адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="9c40b-183">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="9c40b-184">RemoveAll, PreContent.SetHtmlContent и PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="9c40b-184">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="9c40b-185">Добавьте следующие `BoldTagHelper` класса *TagHelpers* папки.</span><span class="sxs-lookup"><span data-stu-id="9c40b-185">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="9c40b-186">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-186">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]</span></span>

    <span data-ttu-id="9c40b-187">**Примечания.**</span><span class="sxs-lookup"><span data-stu-id="9c40b-187">**Notes:**</span></span>
    
    * <span data-ttu-id="9c40b-188">`[HtmlTargetElement]` Атрибут передает параметр атрибута, указывающее, что любой HTML-элемент, который содержит HTML-атрибут с именем «bold» будет соответствовать, а `Process` переопределяющий метод в классе будет выполняться.</span><span class="sxs-lookup"><span data-stu-id="9c40b-188">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="9c40b-189">В нашем примере `Process` метод удаляет атрибут «полужирный» и окружает содержащего разметку с `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="9c40b-189">In our sample, the `Process`  method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="9c40b-190">Поскольку вы не хотите заменить существующие теги содержимого, необходимо написать открывающий `<strong>` тег с `PreContent.SetHtmlContent` метод и закрытие `</strong>` тег с `PostContent.SetHtmlContent` метод.</span><span class="sxs-lookup"><span data-stu-id="9c40b-190">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="9c40b-191">Изменить *About.cshtml* представление для размещения `bold` значение атрибута.</span><span class="sxs-lookup"><span data-stu-id="9c40b-191">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="9c40b-192">Ниже приведен полный код.</span><span class="sxs-lookup"><span data-stu-id="9c40b-192">The completed code is shown below.</span></span>

    <span data-ttu-id="9c40b-193">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-193">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]</span></span>

3.  <span data-ttu-id="9c40b-194">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="9c40b-194">Run the app.</span></span> <span data-ttu-id="9c40b-195">Любимом браузере можно использовать для проверки источника и проверьте разметку.</span><span class="sxs-lookup"><span data-stu-id="9c40b-195">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="9c40b-196">`[HtmlTargetElement]` Атрибут выше предназначен только для разметки HTML, который предоставляет имя атрибута «полужирный».</span><span class="sxs-lookup"><span data-stu-id="9c40b-196">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="9c40b-197">`<bold>` Элемент не был изменен тег вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="9c40b-197">The `<bold>` element was not modified by the tag helper.</span></span>

4. <span data-ttu-id="9c40b-198">Закомментируйте `[HtmlTargetElement]` строку атрибута и он будет по умолчанию для различных версий `<bold>` теги, то есть HTML-разметку формы `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="9c40b-198">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="9c40b-199">Помните, что именования по умолчанию будет совпадать с именем класса **Полужирный**вспомогательной функции тегов для `<bold>` тегов.</span><span class="sxs-lookup"><span data-stu-id="9c40b-199">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="9c40b-200">Запустите приложение и убедитесь, что `<bold>` тег обрабатывается тег вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="9c40b-200">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="9c40b-201">Декорирования классов с несколькими `[HtmlTargetElement]` атрибуты результаты логической операцией или целевых объектов.</span><span class="sxs-lookup"><span data-stu-id="9c40b-201">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="9c40b-202">Например используя приведенный ниже код, полужирным тега или атрибута bold будет соответствовать.</span><span class="sxs-lookup"><span data-stu-id="9c40b-202">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

<span data-ttu-id="9c40b-203">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-203">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]</span></span>

<span data-ttu-id="9c40b-204">При добавлении нескольких атрибутов той же инструкции, среда выполнения рассматривает их как логическое и.</span><span class="sxs-lookup"><span data-stu-id="9c40b-204">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="9c40b-205">Например, в следующем коде HTML-элемент должен иметь имя «bold» с атрибутом с именем «bold» (`<bold bold />`) для сопоставления.</span><span class="sxs-lookup"><span data-stu-id="9c40b-205">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="9c40b-206">Можно также использовать `[HtmlTargetElement]` для изменения имени целевого элемента.</span><span class="sxs-lookup"><span data-stu-id="9c40b-206">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="9c40b-207">Например, если вы хотите, чтобы `BoldTagHelper` целевой `<MyBold>` тегов, используется следующий атрибут:</span><span class="sxs-lookup"><span data-stu-id="9c40b-207">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a><span data-ttu-id="9c40b-208">Передача модели помощникам тега</span><span class="sxs-lookup"><span data-stu-id="9c40b-208">Passing a model to a Tag Helper</span></span>

1.  <span data-ttu-id="9c40b-209">Добавить *моделей* папки.</span><span class="sxs-lookup"><span data-stu-id="9c40b-209">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="9c40b-210">Добавьте следующие `WebsiteContext` класса *моделей* папки:</span><span class="sxs-lookup"><span data-stu-id="9c40b-210">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    <span data-ttu-id="9c40b-211">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-211">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]</span></span>

3.  <span data-ttu-id="9c40b-212">Добавьте следующие `WebsiteInformationTagHelper` класса *TagHelpers* папки.</span><span class="sxs-lookup"><span data-stu-id="9c40b-212">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="9c40b-213">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-213">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]</span></span>
    
    <span data-ttu-id="9c40b-214">**Примечания.**</span><span class="sxs-lookup"><span data-stu-id="9c40b-214">**Notes:**</span></span>
    
    * <span data-ttu-id="9c40b-215">Как упоминалось ранее, вспомогательных функций тегов преобразует имена классов в стиле Pascal C# и свойства для вспомогательных функций тегов в [нижний регистр kebab](http://c2.com/cgi/wiki?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="9c40b-215">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://c2.com/cgi/wiki?KebabCase).</span></span> <span data-ttu-id="9c40b-216">Таким образом Чтобы использовать `WebsiteInformationTagHelper` в Razor, вы напишете `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="9c40b-216">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="9c40b-217">Вы явным образом не идентифицируете целевой элемент с `[HtmlTargetElement]` атрибута, поэтому по умолчанию `website-information` будут применяться.</span><span class="sxs-lookup"><span data-stu-id="9c40b-217">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="9c40b-218">Если вы применили (Примечание не kebab так, но совпадает с именем класса) следующий атрибут:</span><span class="sxs-lookup"><span data-stu-id="9c40b-218">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="9c40b-219">Ниже тег варианта kebab `<website-information />` не будет совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c40b-219">The lower kebab case tag `<website-information />` would not match.</span></span> <span data-ttu-id="9c40b-220">Если вы хотите `[HtmlTargetElement]` атрибут, используйте kebab случая, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="9c40b-220">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="9c40b-221">Элементы, которые являются самозакрывающегося не имеет содержимого.</span><span class="sxs-lookup"><span data-stu-id="9c40b-221">Elements that are self-closing have no content.</span></span> <span data-ttu-id="9c40b-222">В этом примере разметки Razor будет использоваться самостоятельно закрывающегося тега, но будут создавать вспомогательные тег [раздел](http://www.w3.org/TR/html5/sections.html#the-section-element) элемент (который не требуется закрывать и вы пишете содержимое внутри `section` элемент).</span><span class="sxs-lookup"><span data-stu-id="9c40b-222">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which is not self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="9c40b-223">Таким образом, необходимо задать `TagMode` для `StartTagAndEndTag` для записи выходных данных.</span><span class="sxs-lookup"><span data-stu-id="9c40b-223">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="9c40b-224">Кроме того, вы можете закомментировать строку, устанавливающую `TagMode` и записи разметки закрывающим тегом.</span><span class="sxs-lookup"><span data-stu-id="9c40b-224">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="9c40b-225">(Пример разметки указан далее в этом руководстве.)</span><span class="sxs-lookup"><span data-stu-id="9c40b-225">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="9c40b-226">`$` (Знак доллара) в следующей строке использует [интерполируются строка](https://msdn.microsoft.com/library/Dn961160.aspx):</span><span class="sxs-lookup"><span data-stu-id="9c40b-226">The `$` (dollar sign) in the following line uses an [interpolated string](https://msdn.microsoft.com/library/Dn961160.aspx):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="9c40b-227">Добавьте следующую разметку, чтобы *About.cshtml* представления.</span><span class="sxs-lookup"><span data-stu-id="9c40b-227">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="9c40b-228">Выделенную разметку сведения о веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="9c40b-228">The highlighted markup displays the web site information.</span></span>
    
    <span data-ttu-id="9c40b-229">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-229">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]</span></span>
    
    >[!NOTE]
    > <span data-ttu-id="9c40b-230">В разметке Razor, показано ниже:</span><span class="sxs-lookup"><span data-stu-id="9c40b-230">In the Razor markup shown below:</span></span>
    >
    ><span data-ttu-id="9c40b-231">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-231">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]</span></span>
    > 
    ><span data-ttu-id="9c40b-232">Знает Razor `info` атрибут — это класс, не является строкой, и необходимо написать код C#.</span><span class="sxs-lookup"><span data-stu-id="9c40b-232">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="9c40b-233">Любой атрибут вспомогательной функции нестроковые тег должен быть записан без `@` символов.</span><span class="sxs-lookup"><span data-stu-id="9c40b-233">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="9c40b-234">Запустите приложение и перейдите в представление о программе, чтобы просмотреть сведения веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="9c40b-234">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="9c40b-235">Можно использовать следующую разметку с закрывающий тег и удалите строку с `TagMode.StartTagAndEndTag` во вспомогательном методе тег:</span><span class="sxs-lookup"><span data-stu-id="9c40b-235">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    ><span data-ttu-id="9c40b-236">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-236">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]</span></span>

## <a name="condition-tag-helper"></a><span data-ttu-id="9c40b-237">Вспомогательный тег условие</span><span class="sxs-lookup"><span data-stu-id="9c40b-237">Condition Tag Helper</span></span>

<span data-ttu-id="9c40b-238">Вспомогательный тег условие отображает выходные данные при передаче значения true.</span><span class="sxs-lookup"><span data-stu-id="9c40b-238">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="9c40b-239">Добавьте следующие `ConditionTagHelper` класса *TagHelpers* папки.</span><span class="sxs-lookup"><span data-stu-id="9c40b-239">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="9c40b-240">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-240">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]</span></span>

2.  <span data-ttu-id="9c40b-241">Замените содержимое *Views/Home/Index.cshtml* файла следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="9c40b-241">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    <!-- literal_block {"xml:space": "preserve", "source": "mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml", "ids": [], "linenos": false, "highlight_args": {"linenostart": 1}} -->
    
    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=Model />
        <div condition="Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="9c40b-242">Замените `Index` метод в `Home` контроллера с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="9c40b-242">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    <span data-ttu-id="9c40b-243">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-243">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]</span></span>

4.  <span data-ttu-id="9c40b-244">Запустите приложение и перейдите на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="9c40b-244">Run the app and browse to the home page.</span></span> <span data-ttu-id="9c40b-245">Разметка в условной `div` не отображался.</span><span class="sxs-lookup"><span data-stu-id="9c40b-245">The markup in the conditional `div` will not be rendered.</span></span> <span data-ttu-id="9c40b-246">Добавить строку запроса `?approved=true` URL-адрес (например, `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="9c40b-246">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="9c40b-247">`approved`имеет значение true, а также условия отображения разметки.</span><span class="sxs-lookup"><span data-stu-id="9c40b-247">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="9c40b-248">Используйте [nameof](https://msdn.microsoft.com/library/dn986596.aspx) оператор, чтобы указать атрибут целевой вместо указания строки, как в случае со вспомогательным методом тег полужирным шрифтом:</span><span class="sxs-lookup"><span data-stu-id="9c40b-248">Use the [nameof](https://msdn.microsoft.com/library/dn986596.aspx) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
><span data-ttu-id="9c40b-249">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-249">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]</span></span>
>
><span data-ttu-id="9c40b-250">[Nameof](https://msdn.microsoft.com/library/dn986596.aspx) оператор будет защищать код должен его постоянно оптимизируемого (мы может возникнуть необходимость изменить имя, `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="9c40b-250">The [nameof](https://msdn.microsoft.com/library/dn986596.aspx) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoiding-tag-helper-conflicts"></a><span data-ttu-id="9c40b-251">Предотвращение конфликтов вспомогательный тега</span><span class="sxs-lookup"><span data-stu-id="9c40b-251">Avoiding Tag Helper conflicts</span></span>

<span data-ttu-id="9c40b-252">В этом разделе запись пару автоматическое связывание вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="9c40b-252">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="9c40b-253">Первый заменяет разметку, содержащую URL-адрес, начинающийся с HTTP HTML привязки тег содержащую же URL-адрес (и тем самым давая ссылку на URL-адрес).</span><span class="sxs-lookup"><span data-stu-id="9c40b-253">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="9c40b-254">Второй будет то же сделайте для URL-адреса начиная с веб-публикации.</span><span class="sxs-lookup"><span data-stu-id="9c40b-254">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="9c40b-255">Поскольку тесно связаны эти две вспомогательные функции и их может рефакторинг в будущем, мы будем держать их в том же файле.</span><span class="sxs-lookup"><span data-stu-id="9c40b-255">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="9c40b-256">Добавьте следующие `AutoLinkerHttpTagHelper` класса *TagHelpers* папки.</span><span class="sxs-lookup"><span data-stu-id="9c40b-256">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="9c40b-257">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-257">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]</span></span>

    >[!NOTE]
    ><span data-ttu-id="9c40b-258">`AutoLinkerHttpTagHelper` Класса цели `p` элементы и использует [Regex](https://msdn.microsoft.com/library/system.text.regularexpressions.regex.aspx) для создания привязки.</span><span class="sxs-lookup"><span data-stu-id="9c40b-258">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://msdn.microsoft.com/library/system.text.regularexpressions.regex.aspx) to create the anchor.</span></span>

2.  <span data-ttu-id="9c40b-259">Добавьте следующую разметку в конец *Views/Home/Contact.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="9c40b-259">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    <span data-ttu-id="9c40b-260">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-260">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]</span></span>

3.  <span data-ttu-id="9c40b-261">Запустите приложение и проверьте, правильно отображает вспомогательный тег привязки.</span><span class="sxs-lookup"><span data-stu-id="9c40b-261">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="9c40b-262">Обновление `AutoLinker` класса для включения `AutoLinkerWwwTagHelper` которого преобразует www текст в тег, который содержит исходный текст веб-публикации.</span><span class="sxs-lookup"><span data-stu-id="9c40b-262">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="9c40b-263">Обновленный код выделяется ниже:</span><span class="sxs-lookup"><span data-stu-id="9c40b-263">The updated code is highlighted below:</span></span>

    <span data-ttu-id="9c40b-264">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-264">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]</span></span>

5.  <span data-ttu-id="9c40b-265">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="9c40b-265">Run the app.</span></span> <span data-ttu-id="9c40b-266">Обратите внимание, www текст отображается в виде ссылки, но не является текстом HTTP.</span><span class="sxs-lookup"><span data-stu-id="9c40b-266">Notice the www text is rendered as a link but the HTTP text is not.</span></span> <span data-ttu-id="9c40b-267">Если установить точку останова в обоих классах, вы увидите, вспомогательный класс тег HTTP выполняется первой.</span><span class="sxs-lookup"><span data-stu-id="9c40b-267">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="9c40b-268">Проблема заключается в кэширования вывода вспомогательный тегов, что при запуске вспомогательного тег WWW она перезаписывает кэшированные выходные данные модуля поддержки тег HTTP.</span><span class="sxs-lookup"><span data-stu-id="9c40b-268">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="9c40b-269">Далее в этом учебнике будет показано, как можно управлять порядком выполнения вспомогательных функций тегов в.</span><span class="sxs-lookup"><span data-stu-id="9c40b-269">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="9c40b-270">Нам нужно исправить код с помощью следующего:</span><span class="sxs-lookup"><span data-stu-id="9c40b-270">We'll fix the code with the following:</span></span>

    <span data-ttu-id="9c40b-271">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-271">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]</span></span>

    >[!NOTE]
    ><span data-ttu-id="9c40b-272">В первом выпуске автоматическое связывание вспомогательных функций тегов полученный содержимое целевого объекта с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="9c40b-272">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    ><span data-ttu-id="9c40b-273">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-273">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]</span></span>
    >
    ><span data-ttu-id="9c40b-274">То есть, можно вызвать `GetChildContentAsync` с помощью `TagHelperOutput` переданные `ProcessAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="9c40b-274">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="9c40b-275">Как упоминалось ранее, так как выходные данные кэшируются, последний тег вспомогательный метод для запуска wins.</span><span class="sxs-lookup"><span data-stu-id="9c40b-275">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="9c40b-276">Вы фиксированной этой проблеме с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="9c40b-276">You fixed that problem with the following code:</span></span>
    >
    ><span data-ttu-id="9c40b-277">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-277">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]</span></span>
    >
    ><span data-ttu-id="9c40b-278">Приведенный выше код проверяет, если содержимое было изменено, и если да, он получает содержимое из выходного буфера.</span><span class="sxs-lookup"><span data-stu-id="9c40b-278">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="9c40b-279">Запустите приложение и убедиться, что две ссылки работают должным образом.</span><span class="sxs-lookup"><span data-stu-id="9c40b-279">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="9c40b-280">Хотя может показаться, что наши вспомогательные тег компоновщика автоматически будет полным и правильным, у него есть незначительные проблемы.</span><span class="sxs-lookup"><span data-stu-id="9c40b-280">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="9c40b-281">Если вспомогательные тег WWW выполняется первой, веб-ссылки не будут правильными.</span><span class="sxs-lookup"><span data-stu-id="9c40b-281">If the WWW tag helper runs first, the www links will not be correct.</span></span> <span data-ttu-id="9c40b-282">Обновление кода, добавив `Order` перегрузку, чтобы управлять порядком, запускаемую в теге.</span><span class="sxs-lookup"><span data-stu-id="9c40b-282">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="9c40b-283">`Order` Свойство определяет порядок выполнения относительно других вспомогательных функций тегов, предназначенные для одного элемента.</span><span class="sxs-lookup"><span data-stu-id="9c40b-283">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="9c40b-284">Значение порядка по умолчанию равно нулю, и экземпляры с более низкими значениями выполняются первыми.</span><span class="sxs-lookup"><span data-stu-id="9c40b-284">The default order value is zero and instances with lower values are executed first.</span></span>

    <span data-ttu-id="9c40b-285">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-285">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]</span></span>
    
    <span data-ttu-id="9c40b-286">Приведенный выше код гарантирует, вспомогательные тег HTTP выполняется перед вспомогательный тег WWW.</span><span class="sxs-lookup"><span data-stu-id="9c40b-286">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="9c40b-287">Изменение `Order` для `MaxValue` и убедитесь, что разметки, созданной для тега WWW неверен.</span><span class="sxs-lookup"><span data-stu-id="9c40b-287">Change `Order` to `MaxValue` and verify that the markup generated for the  WWW tag is incorrect.</span></span>

## <a name="inspecting-and-retrieving-child-content"></a><span data-ttu-id="9c40b-288">Проверки и извлечения содержимого дочернего элемента</span><span class="sxs-lookup"><span data-stu-id="9c40b-288">Inspecting and retrieving child content</span></span>

<span data-ttu-id="9c40b-289">Вспомогательных функций тегов предоставляют несколько свойств, чтобы извлечь содержимое.</span><span class="sxs-lookup"><span data-stu-id="9c40b-289">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="9c40b-290">Результат `GetChildContentAsync` можно добавить `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="9c40b-290">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="9c40b-291">Вы можете проверить результат `GetChildContentAsync` с `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="9c40b-291">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="9c40b-292">При изменении `output.Content`, тексте вспомогательной функции тегов не будет выполнена или не визуализуется, если вызывается `GetChildContentAsync` как в нашем примере компоновщик автоматическое:</span><span class="sxs-lookup"><span data-stu-id="9c40b-292">If you modify `output.Content`, the TagHelper body will not be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

<span data-ttu-id="9c40b-293">[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]</span><span class="sxs-lookup"><span data-stu-id="9c40b-293">[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]</span></span>

-  <span data-ttu-id="9c40b-294">Несколько вызовов `GetChildContentAsync` будет возвращать то же значение и не повторно выполните `TagHelper` body только при проведении в значение false, указывающее, не используйте параметр кэшированный результат.</span><span class="sxs-lookup"><span data-stu-id="9c40b-294">Multiple calls to `GetChildContentAsync` will return the same value and will not re-execute the `TagHelper` body unless you pass in a false parameter indicating  not use the cached result.</span></span>
