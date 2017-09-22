---
title: "Вспомогательных функций тегов в ASP.NET Core"
author: rick-anderson
description: "Что такое вспомогательных функций тегов и способ их использования в ASP.NET Core."
keywords: "ASP.NET Core, вспомогательных функций тегов"
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 10471210075dc8a5366b7d5170d6594c2e66ce94
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a><span data-ttu-id="d3c92-104">Общие сведения о вспомогательных функций тегов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3c92-104">Introduction to Tag Helpers in ASP.NET Core</span></span> 

<span data-ttu-id="d3c92-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="d3c92-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="d3c92-106">Что такое вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="d3c92-106">What are Tag Helpers?</span></span>

<span data-ttu-id="d3c92-107">Вспомогательных функций тегов включить серверный код принять участие в создании и подготовке к просмотру элементов HTML в файлах Razor.</span><span class="sxs-lookup"><span data-stu-id="d3c92-107">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="d3c92-108">Например, встроенная `ImageTagHelper` можно добавить номер версии имени образа.</span><span class="sxs-lookup"><span data-stu-id="d3c92-108">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="d3c92-109">При каждом изменении образа сервера создается новая уникальный версия изображения, поэтому клиенты гарантированно получить текущее изображение (вместо устаревших кэшированное изображение).</span><span class="sxs-lookup"><span data-stu-id="d3c92-109">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="d3c92-110">Существует множество встроенных вспомогательных функций тегов для общих задач — например для создания формы, ссылки, загрузки средств и пакетов дополнительные - и еще более доступные из открытых репозиториев GitHub и в качестве NuGet.</span><span class="sxs-lookup"><span data-stu-id="d3c92-110">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="d3c92-111">Вспомогательных функций тегов разрабатываются в C#, и они предназначены для HTML-элементов на основе имени элемента, имя атрибута или родительского тега.</span><span class="sxs-lookup"><span data-stu-id="d3c92-111">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="d3c92-112">Например, встроенная `LabelTagHelper` можно выбрать целевую HTML `<label>` элемент при `LabelTagHelper` атрибутов.</span><span class="sxs-lookup"><span data-stu-id="d3c92-112">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="d3c92-113">Если вы знакомы с [вспомогательных методов HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), вспомогательных функций тегов уменьшить явную переходы между HTML и C# в представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="d3c92-113">If you're familiar with [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="d3c92-114">Во многих случаях вспомогательные методы HTML предоставляют альтернативный подход для определенных вспомогательных тег, но это следует отметить, что вспомогательных функций тегов не заменяют вспомогательных методов HTML, а не существует тег вспомогательный класс для каждого вспомогательный метод HTML.</span><span class="sxs-lookup"><span data-stu-id="d3c92-114">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="d3c92-115">[По сравнению с вспомогательных методов HTML вспомогательных функций тегов](#tag-helpers-compared-to-html-helpers) описаны различия более подробно.</span><span class="sxs-lookup"><span data-stu-id="d3c92-115">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="d3c92-116">Предоставления вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="d3c92-116">What Tag Helpers provide</span></span>

<span data-ttu-id="d3c92-117">**Процесс разработки понятного имени HTML** в большинстве случаев разметки Razor с помощью вспомогательных функций тегов выглядит как стандартный код HTML.</span><span class="sxs-lookup"><span data-stu-id="d3c92-117">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="d3c92-118">Интерфейсный конструкторы представление о HTML, CSS и JavaScript можно редактировать Razor не зная синтаксиса Razor C#.</span><span class="sxs-lookup"><span data-stu-id="d3c92-118">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="d3c92-119">**IntelliSense среду с широкими возможностями для создания разметки HTML и Razor** это резко отличается от для вспомогательных методов HTML, описанный подход для создания серверных разметки в представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="d3c92-119">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="d3c92-120">[По сравнению с вспомогательных методов HTML вспомогательных функций тегов](#tag-helpers-compared-to-html-helpers) описаны различия более подробно.</span><span class="sxs-lookup"><span data-stu-id="d3c92-120">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="d3c92-121">[Поддержка IntelliSense для вспомогательных функций тегов](#intellisense-support-for-tag-helpers) объясняет среды IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="d3c92-121">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="d3c92-122">Даже разработчики, возникших с синтаксисом Razor C#, эффективнее с помощью вспомогательных функций тегов, чем написание разметки Razor C#.</span><span class="sxs-lookup"><span data-stu-id="d3c92-122">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="d3c92-123">**Чтобы сделать вам повысить производительность и воспроизводимых более надежным и надежных и простоты поддержки кода с помощью сведения доступны только на сервере** например, раньше мантра на обновление образов была для изменения имени изображения при изменении изображение.</span><span class="sxs-lookup"><span data-stu-id="d3c92-123">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="d3c92-124">Изображения должны активно кэшироваться для повышения производительности, и если не изменить имя изображения, то существует риск копию устаревших клиентов.</span><span class="sxs-lookup"><span data-stu-id="d3c92-124">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="d3c92-125">Исторически после изображения был изменен, имя была изменена, и каждая ссылка на изображение в веб-приложения не требуется обновлять.</span><span class="sxs-lookup"><span data-stu-id="d3c92-125">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="d3c92-126">Это не только очень трудовые служб с интенсивными вычислениями, кроме того, он подвержен ошибкам (может пропустить ссылку, ввод Неверная строка, и т. д.) Встроенная `ImageTagHelper` это можно сделать для вас автоматически.</span><span class="sxs-lookup"><span data-stu-id="d3c92-126">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="d3c92-127">`ImageTagHelper` Можно добавить версию число имя образа, при каждом изменении образа сервера автоматически создается новая версия уникальным для изображения.</span><span class="sxs-lookup"><span data-stu-id="d3c92-127">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="d3c92-128">Клиенты гарантированно получить текущее изображение.</span><span class="sxs-lookup"><span data-stu-id="d3c92-128">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="d3c92-129">Это устойчивость и труда экономии по существу свободного с помощью `ImageTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="d3c92-129">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="d3c92-130">Большинство встроенных вспомогательных функций тегов целевой существующих элементов HTML и предоставить серверные атрибуты для элемента.</span><span class="sxs-lookup"><span data-stu-id="d3c92-130">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="d3c92-131">Например `<input>` элемента, используемого во множестве представлений в *представления и учетная запись* папка содержит `asp-for` атрибут, который извлекает имя свойства указанной модели в создаваемом коде HTML.</span><span class="sxs-lookup"><span data-stu-id="d3c92-131">For example, the `<input>` element used in many of the views in the *Views/Account* folder contains the `asp-for` attribute, which extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="d3c92-132">Приведенная ниже разметка Razor:</span><span class="sxs-lookup"><span data-stu-id="d3c92-132">The following Razor markup:</span></span>

```html
<label asp-for="Email"></label>
```

<span data-ttu-id="d3c92-133">Создает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="d3c92-133">Generates the following HTML:</span></span>

```html
<label for="Email">Email</label>
```

<span data-ttu-id="d3c92-134">`asp-for` Атрибут становятся доступными по `For` свойства `LabelTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="d3c92-134">The `asp-for` attribute is made available by the `For` property in the `LabelTagHelper`.</span></span> <span data-ttu-id="d3c92-135">В разделе [разработки вспомогательных функций тегов](authoring.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="d3c92-135">See [Authoring Tag Helpers](authoring.md) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="d3c92-136">Управление вспомогательный тег области</span><span class="sxs-lookup"><span data-stu-id="d3c92-136">Managing Tag Helper scope</span></span>

<span data-ttu-id="d3c92-137">Вспомогательные объекты области тег управляется сочетание `@addTagHelper`, `@removeTagHelper`и «!» символ отказаться от участия.</span><span class="sxs-lookup"><span data-stu-id="d3c92-137">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name=add-helper-label></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="d3c92-138">`@addTagHelper`делает доступными вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="d3c92-138">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="d3c92-139">При создании нового веб-приложения ASP.NET Core с именем *AuthoringTagHelpers* (с без проверки подлинности), следующие *Views/_ViewImports.cshtml* файл будет добавлен в проект:</span><span class="sxs-lookup"><span data-stu-id="d3c92-139">If you create a new ASP.NET Core web app named *AuthoringTagHelpers* (with no authentication), the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="d3c92-140">`@addTagHelper` Директива поставляет вспомогательных функций тегов в представлении.</span><span class="sxs-lookup"><span data-stu-id="d3c92-140">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="d3c92-141">В этом случае файл представления является *Views/_ViewImports.cshtml*, который по умолчанию наследуется просмотреть все файлы, в *представления* папка и вложенные каталоги; предоставления вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="d3c92-141">In this case, the view file is *Views/_ViewImports.cshtml*, which by default is inherited by all view files in the *Views* folder and sub-directories; making Tag Helpers available.</span></span> <span data-ttu-id="d3c92-142">Приведенный выше код использует синтаксис подстановочный знак (»\*») позволяет указать, что все вспомогательных функций тегов в указанной сборке (*Microsoft.AspNetCore.Mvc.TagHelpers*) будет доступен для каждого файла представления в *представления* каталога или вложенный каталог.</span><span class="sxs-lookup"><span data-stu-id="d3c92-142">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or sub-directory.</span></span> <span data-ttu-id="d3c92-143">Первый параметр после `@addTagHelper` указывает вспомогательных функций тегов для загрузки (мы используем «\*» для всех вспомогательных функций тегов), и второй параметр «Microsoft.AspNetCore.Mvc.TagHelpers» указывает на сборку, содержащую вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="d3c92-143">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="d3c92-144">*Microsoft.AspNetCore.Mvc.TagHelpers* — это сборка встроенных вспомогательных функций тегов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d3c92-144">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="d3c92-145">Для предоставления вспомогательных функций тегов в этом проекте все (который создает сборку с именем *AuthoringTagHelpers*), следует ввести следующее:</span><span class="sxs-lookup"><span data-stu-id="d3c92-145">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="d3c92-146">Если проект содержит `EmailTagHelper` пространства имен по умолчанию (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), можно указать полное имя (FQN) тега вспомогательного метода:</span><span class="sxs-lookup"><span data-stu-id="d3c92-146">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3]}} -->

```html
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="d3c92-147">Чтобы добавить тег вспомогательный класс для представления с помощью FQN, сначала добавьте FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), а затем имя сборки (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="d3c92-147">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="d3c92-148">Многие разработчики предпочитают использовать «\*» синтаксисе знаков подстановки.</span><span class="sxs-lookup"><span data-stu-id="d3c92-148">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="d3c92-149">О синтаксисе знаков подстановки можно вставлять символ-шаблон «\*» как суффикс в FQN.</span><span class="sxs-lookup"><span data-stu-id="d3c92-149">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="d3c92-150">Например, любой из следующих директив, добавит `EmailTagHelper`:</span><span class="sxs-lookup"><span data-stu-id="d3c92-150">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```csharp
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="d3c92-151">Как упоминалось ранее, добавление `@addTagHelper` директиву *Views/_ViewImports.cshtml* файла делает доступными для всех Просмотр файлов в тег вспомогательный *представления* каталога и вложенные каталоги.</span><span class="sxs-lookup"><span data-stu-id="d3c92-151">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and sub-directories.</span></span> <span data-ttu-id="d3c92-152">Можно использовать `@addTagHelper` директив в файлах определенных представлений, чтобы согласиться на предоставление тег модуля поддержки только те представления.</span><span class="sxs-lookup"><span data-stu-id="d3c92-152">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name=remove-razor-directives-label></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="d3c92-153">`@removeTagHelper`Удаляет вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="d3c92-153">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="d3c92-154">`@removeTagHelper` Имеет те же два параметра, как `@addTagHelper`, и он удаляет помощник тег, который был добавлен ранее.</span><span class="sxs-lookup"><span data-stu-id="d3c92-154">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="d3c92-155">Например `@removeTagHelper` применяться к определенному представлению Удаляет указанный вспомогательный метод тег из представления.</span><span class="sxs-lookup"><span data-stu-id="d3c92-155">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="d3c92-156">С помощью `@removeTagHelper` в *Views/Folder/_ViewImports.cshtml* файл удаляет указанный вспомогательный метод тег из всех представлений в *папки*.</span><span class="sxs-lookup"><span data-stu-id="d3c92-156">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="d3c92-157">Управление области вспомогательный тег с *_ViewImports.cshtml* файла</span><span class="sxs-lookup"><span data-stu-id="d3c92-157">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="d3c92-158">Можно добавить *_ViewImports.cshtml* для любой папки представление и представление модуль применяет директив из обоих этого файла и *Views/_ViewImports.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="d3c92-158">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="d3c92-159">При добавлении пустой *Views/Home/_ViewImports.cshtml* в файле *Главная* представления, то бы без изменений из-за *_ViewImports.cshtml* файл аддитивный характер.</span><span class="sxs-lookup"><span data-stu-id="d3c92-159">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="d3c92-160">Любой `@addTagHelper` директив можно добавить *Views/Home/_ViewImports.cshtml* файла (, не входящих в значение по умолчанию *Views/_ViewImports.cshtml* файла) будут предоставлять эти вспомогательных функций тегов для представления только в *Главная* папки.</span><span class="sxs-lookup"><span data-stu-id="d3c92-160">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="d3c92-161">Отказ от отдельных элементов</span><span class="sxs-lookup"><span data-stu-id="d3c92-161">Opting out of individual elements</span></span>

<span data-ttu-id="d3c92-162">Можно отключить вспомогательный тег на уровне элемента символом отказаться вспомогательный тег («!»).</span><span class="sxs-lookup"><span data-stu-id="d3c92-162">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="d3c92-163">Например `Email` проверка отключена в `<span>` символом отказаться вспомогательный тег:</span><span class="sxs-lookup"><span data-stu-id="d3c92-163">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```csharp
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="d3c92-164">Необходимо применить тег вспомогательный символ отказаться открывающий и закрывающий теги.</span><span class="sxs-lookup"><span data-stu-id="d3c92-164">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="d3c92-165">(В редакторе Visual Studio автоматически добавляет символ отказа закрывающий тег при добавлении одного открывающий тег).</span><span class="sxs-lookup"><span data-stu-id="d3c92-165">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="d3c92-166">После добавления символа отказа элемента и атрибуты тега вспомогательный больше не отображаются в отдельных шрифт.</span><span class="sxs-lookup"><span data-stu-id="d3c92-166">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name=prefix-razor-directives-label></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="d3c92-167">С помощью `@tagHelperPrefix` вносить вспомогательный тег использования явное</span><span class="sxs-lookup"><span data-stu-id="d3c92-167">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="d3c92-168">`@tagHelperPrefix` Директивы можно указать строку префикса тега для включения поддержка вспомогательных функций тегов и явно объявить использование вспомогательного тег.</span><span class="sxs-lookup"><span data-stu-id="d3c92-168">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="d3c92-169">Например, можно добавить следующую разметку, чтобы *Views/_ViewImports.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="d3c92-169">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```html
@tagHelperPrefix th:
```
<span data-ttu-id="d3c92-170">На приведенном ниже рисунке кода присвоено префикс тега вспомогательный `th:`, так что только элементы, с помощью префикса `th:` поддержка вспомогательных функций тегов (включен в тег вспомогательные элементы имеют различение шрифта).</span><span class="sxs-lookup"><span data-stu-id="d3c92-170">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="d3c92-171">`<label>` И `<input>` элементы имеют префикс тега вспомогательный и включен в тег вспомогательный, при `<span>` не содержит элемент.</span><span class="sxs-lookup"><span data-stu-id="d3c92-171">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element does not.</span></span>

![изображение](intro/_static/thp.png)

<span data-ttu-id="d3c92-173">Те же правила иерархии, которые применяются к `@addTagHelper` также применяются к `@tagHelperPrefix`.</span><span class="sxs-lookup"><span data-stu-id="d3c92-173">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="d3c92-174">Поддержка IntelliSense для вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="d3c92-174">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="d3c92-175">При создании нового веб-приложения ASP.NET в Visual Studio, он добавляет пакет NuGet «Microsoft.AspNetCore.Razor.Tools».</span><span class="sxs-lookup"><span data-stu-id="d3c92-175">When you create a new ASP.NET web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="d3c92-176">Это пакет, который добавляет тег вспомогательные средства.</span><span class="sxs-lookup"><span data-stu-id="d3c92-176">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="d3c92-177">Следует рассмотреть возможность записи HTML `<label>` элемента.</span><span class="sxs-lookup"><span data-stu-id="d3c92-177">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="d3c92-178">Как только вы введете `<l` в редакторе Visual Studio IntelliSense отображает соответствующие элементы:</span><span class="sxs-lookup"><span data-stu-id="d3c92-178">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![изображение](intro/_static/label.png)

<span data-ttu-id="d3c92-180">Не только получить Справка в формате HTML, но значок («@» символ с «<>», в нем).</span><span class="sxs-lookup"><span data-stu-id="d3c92-180">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![изображение](intro/_static/tagSym.png)

<span data-ttu-id="d3c92-182">идентифицирует элемент как целевой, вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="d3c92-182">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="d3c92-183">Чистый HTML-элементы (такие как `fieldset`) отображается значок «<>».</span><span class="sxs-lookup"><span data-stu-id="d3c92-183">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="d3c92-184">Чистый HTML-код `<label>` тег отображает HTML-тега (с цветовую тему по умолчанию Visual Studio) для brown шрифта атрибуты красным цветом, и атрибут значения синим цветом.</span><span class="sxs-lookup"><span data-stu-id="d3c92-184">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![изображение](intro/_static/LableHtmlTag.png)

<span data-ttu-id="d3c92-186">После ввода `<label`, IntelliSense выводит списки доступных атрибутов HTML и CSS и атрибуты целевые вспомогательный тег:</span><span class="sxs-lookup"><span data-stu-id="d3c92-186">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![изображение](intro/_static/labelattr.png)

<span data-ttu-id="d3c92-188">Завершение операторов IntelliSense позволяет вводить клавишу tab, чтобы завершить инструкцию со значением:</span><span class="sxs-lookup"><span data-stu-id="d3c92-188">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![изображение](intro/_static/stmtcomplete.png)

<span data-ttu-id="d3c92-190">Как только вводится в атрибут вспомогательной функции тегов, изменить шрифты тегов и атрибутов.</span><span class="sxs-lookup"><span data-stu-id="d3c92-190">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="d3c92-191">Используется по умолчанию Visual Studio «Blue» или «Light» цветовая тема, шрифт является фиолетовый полужирным шрифтом.</span><span class="sxs-lookup"><span data-stu-id="d3c92-191">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="d3c92-192">Если вы используете «Темной» темой шрифт является полужирным сине-зеленый.</span><span class="sxs-lookup"><span data-stu-id="d3c92-192">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="d3c92-193">Изображения в этом документе были выполнены с помощью тему по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d3c92-193">The images in this document were taken using the default theme.</span></span>

![изображение](intro/_static/labelaspfor2.png)

<span data-ttu-id="d3c92-195">Visual Studio можно ввести *CompleteWord* ярлык (Ctrl + Пробел является [по умолчанию](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) внутри двойных кавычек ("»), и теперь вы находитесь в C#, так же, как будет находиться в классе C#.</span><span class="sxs-lookup"><span data-stu-id="d3c92-195">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="d3c92-196">IntelliSense отображает все методы и свойства по модели страницы.</span><span class="sxs-lookup"><span data-stu-id="d3c92-196">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="d3c92-197">Методы и свойства недоступны, так как тип свойства `ModelExpression`.</span><span class="sxs-lookup"><span data-stu-id="d3c92-197">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="d3c92-198">На рисунке ниже, я редактирования `Register` представления, поэтому `RegisterViewModel` доступен.</span><span class="sxs-lookup"><span data-stu-id="d3c92-198">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![изображение](intro/_static/intellemail.png)

<span data-ttu-id="d3c92-200">IntelliSense выводит списки свойств и методов, доступных для модели на странице.</span><span class="sxs-lookup"><span data-stu-id="d3c92-200">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="d3c92-201">Широкие возможности среды IntelliSense помогает выбрать класс CSS:</span><span class="sxs-lookup"><span data-stu-id="d3c92-201">The rich IntelliSense environment helps you select the CSS class:</span></span>

![изображение](intro/_static/iclass.png)

![изображение](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="d3c92-204">Вспомогательных функций тегов по сравнению с вспомогательных методов HTML</span><span class="sxs-lookup"><span data-stu-id="d3c92-204">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="d3c92-205">Вспомогательных функций тегов прикрепить к элементам HTML в представлений Razor, пока [вспомогательных методов HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) , вызываются как методы смешивается с HTML в представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="d3c92-205">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="d3c92-206">Рассмотрим следующую разметку Razor, которая создает метку HTML с «заголовок» класса CSS.</span><span class="sxs-lookup"><span data-stu-id="d3c92-206">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```html
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="d3c92-207">В (`@`) символ дает Razor находится в начале кода.</span><span class="sxs-lookup"><span data-stu-id="d3c92-207">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="d3c92-208">Следующие два параметра («FirstName» и «имя:») представляют собой строки, поэтому [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) не может им помочь.</span><span class="sxs-lookup"><span data-stu-id="d3c92-208">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="d3c92-209">Последний аргумент:</span><span class="sxs-lookup"><span data-stu-id="d3c92-209">The last argument:</span></span>

```html
new {@class="caption"}
```

<span data-ttu-id="d3c92-210">Анонимный объект, используемый для представления атрибутов.</span><span class="sxs-lookup"><span data-stu-id="d3c92-210">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="d3c92-211">Поскольку **класса** является зарезервированным ключевым словом в C# используется `@` символ для принудительного C# для интерпретации "@class=» как символ (имя свойства).</span><span class="sxs-lookup"><span data-stu-id="d3c92-211">Because **class** is a reserved keyword in C#, you use the `@` symbol to force C# to interpret "@class=" as a symbol (property name).</span></span> <span data-ttu-id="d3c92-212">Для интерфейса конструктора (кто-то знакомы с HTML, CSS, JavaScript и другие клиентские технологии, но не знакомы с C# и Razor), большую часть строки является внешней.</span><span class="sxs-lookup"><span data-stu-id="d3c92-212">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="d3c92-213">С помощью IntelliSense не должен быть создан всю строку.</span><span class="sxs-lookup"><span data-stu-id="d3c92-213">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="d3c92-214">С помощью `LabelTagHelper`, же разметки могут быть написаны как:</span><span class="sxs-lookup"><span data-stu-id="d3c92-214">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

![изображение](intro/_static/label2.png)

<span data-ttu-id="d3c92-216">Тег вспомогательный версией, как вы введете `<l` в редакторе Visual Studio IntelliSense отображает соответствующие элементы:</span><span class="sxs-lookup"><span data-stu-id="d3c92-216">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![изображение](intro/_static/label.png)

<span data-ttu-id="d3c92-218">IntelliSense помогает писать всю строку.</span><span class="sxs-lookup"><span data-stu-id="d3c92-218">IntelliSense helps you write the entire line.</span></span> <span data-ttu-id="d3c92-219">`LabelTagHelper` Также по умолчанию используется значение параметра содержимое `asp-for` («FirstName») значения атрибута «Имя»; Он преобразует свойства в стиле Camel предложения, состоящие из имени свойства с места, где происходит каждой новой буквы верхнего регистра.</span><span class="sxs-lookup"><span data-stu-id="d3c92-219">The `LabelTagHelper` also defaults to setting the content of the `asp-for` attribute value ("FirstName") to "First Name"; It converts camel-cased properties to a sentence composed of the property name with a space where each new upper-case letter occurs.</span></span> <span data-ttu-id="d3c92-220">В следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="d3c92-220">In the following markup:</span></span>

![изображение](intro/_static/label2.png)

<span data-ttu-id="d3c92-222">приводит к возникновению ошибки:</span><span class="sxs-lookup"><span data-stu-id="d3c92-222">generates:</span></span>

```html
<label class="caption" for="FirstName">First Name</label>
```

<span data-ttu-id="d3c92-223">Стиле Camel к содержимому предложения регистр не используется, если добавить содержимое `<label>`.</span><span class="sxs-lookup"><span data-stu-id="d3c92-223">The camel-cased to sentence-cased content is not used if you add content to the `<label>`.</span></span> <span data-ttu-id="d3c92-224">Пример:</span><span class="sxs-lookup"><span data-stu-id="d3c92-224">For example:</span></span>

![изображение](intro/_static/1stName.png)

<span data-ttu-id="d3c92-226">приводит к возникновению ошибки:</span><span class="sxs-lookup"><span data-stu-id="d3c92-226">generates:</span></span>

```html
<label class="caption" for="FirstName">Name First</label>
```

<span data-ttu-id="d3c92-227">На следующем рисунке кода показана часть формы *Views/Account/Register.cshtml* представления Razor, созданных из прежних версий шаблона MVC 4.5.x ASP.NET, входящий в состав Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="d3c92-227">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the legacy ASP.NET 4.5.x MVC template included with Visual Studio 2015.</span></span>

![изображение](intro/_static/regCS.png)

<span data-ttu-id="d3c92-229">В редакторе Visual Studio отображает код C# с серым фоном.</span><span class="sxs-lookup"><span data-stu-id="d3c92-229">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="d3c92-230">Например `AntiForgeryToken` вспомогательный метод HTML:</span><span class="sxs-lookup"><span data-stu-id="d3c92-230">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```html
@Html.AntiForgeryToken()
```

<span data-ttu-id="d3c92-231">отображается с серым фоном.</span><span class="sxs-lookup"><span data-stu-id="d3c92-231">is displayed with a grey background.</span></span> <span data-ttu-id="d3c92-232">Большая часть разметку в представлении регистр — C#.</span><span class="sxs-lookup"><span data-stu-id="d3c92-232">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="d3c92-233">Сравните это с эквивалентный подход, с помощью вспомогательных функций тегов:</span><span class="sxs-lookup"><span data-stu-id="d3c92-233">Compare that to the equivalent approach using Tag Helpers:</span></span>

![изображение](intro/_static/regTH.png)

<span data-ttu-id="d3c92-235">Разметка гораздо более понятным и удобным для чтения, редактирование и обслуживание, чем подход вспомогательные методы HTML.</span><span class="sxs-lookup"><span data-stu-id="d3c92-235">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="d3c92-236">Код C# уменьшается до, как минимум, необходимо знать о сервере.</span><span class="sxs-lookup"><span data-stu-id="d3c92-236">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="d3c92-237">В редакторе Visual Studio отображает разметки мишенью вспомогательный тег различение шрифтом.</span><span class="sxs-lookup"><span data-stu-id="d3c92-237">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="d3c92-238">Рассмотрим *электронной почты* группы:</span><span class="sxs-lookup"><span data-stu-id="d3c92-238">Consider the *Email* group:</span></span>

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="d3c92-239">Каждый из атрибутов «asp-» имеет значение «Email», но не является строкой «Email».</span><span class="sxs-lookup"><span data-stu-id="d3c92-239">Each of the "asp-" attributes has a value of "Email", but "Email" is not a string.</span></span> <span data-ttu-id="d3c92-240">В этом контексте «Email» является C# выражение свойство модели для `RegisterViewModel`.</span><span class="sxs-lookup"><span data-stu-id="d3c92-240">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="d3c92-241">В редакторе Visual Studio помогает в написании **все** разметки в подход вспомогательный тег формы регистра, пока Visual Studio предоставляет отсутствует Справка для большей части кода в подход вспомогательные методы HTML.</span><span class="sxs-lookup"><span data-stu-id="d3c92-241">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="d3c92-242">[Поддержка IntelliSense для вспомогательных функций тегов](#intellisense-support-for-tag-helpers) переходит в сведения о работе с вспомогательных функций тегов в редакторе Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3c92-242">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="d3c92-243">По сравнению с веб-сервера управления вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="d3c92-243">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="d3c92-244">Вспомогательных функций тегов не являетесь его владельцем элемента, с которыми они связаны. они просто участвовать в визуализации элемента и содержимого.</span><span class="sxs-lookup"><span data-stu-id="d3c92-244">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="d3c92-245">ASP.NET [веб-элементы управления](https://msdn.microsoft.com/library/7698y1f0.aspx) объявляются и вызывается на странице.</span><span class="sxs-lookup"><span data-stu-id="d3c92-245">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="d3c92-246">[Элементы управления сервера веб-](https://msdn.microsoft.com/library/zsyt68f1.aspx) имеют нетривиальной сроки, которые могут затруднить разработки и отладки.</span><span class="sxs-lookup"><span data-stu-id="d3c92-246">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="d3c92-247">Серверные элементы управления позволяют добавить функциональные возможности клиентские элементы объектной модели документа (DOM) с помощью клиентского элемента управления.</span><span class="sxs-lookup"><span data-stu-id="d3c92-247">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="d3c92-248">Вспомогательных функций тегов имеют не DOM.</span><span class="sxs-lookup"><span data-stu-id="d3c92-248">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="d3c92-249">Серверные веб-элементы включают автоматическое обнаружение браузера.</span><span class="sxs-lookup"><span data-stu-id="d3c92-249">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="d3c92-250">Вспомогательных функций тегов не имеют сведений о браузере.</span><span class="sxs-lookup"><span data-stu-id="d3c92-250">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="d3c92-251">Несколько вспомогательных функций тегов может действовать в том же элементе (см. [вспомогательный тег предотвращение конфликтов](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ), хотя обычно не удается составить элементов управления веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="d3c92-251">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="d3c92-252">Вспомогательных функций тегов можно изменять тег и содержимое HTML-элементов, которые они относятся, но не изменяйте ничего на странице.</span><span class="sxs-lookup"><span data-stu-id="d3c92-252">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="d3c92-253">Элемент управления имеет более обобщенным области и можно выполнять действия, которые влияют на другие части страницы; Включение непредвиденные побочные эффекты.</span><span class="sxs-lookup"><span data-stu-id="d3c92-253">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="d3c92-254">Элемент управления использовать преобразователь типов для преобразования строк в объекты.</span><span class="sxs-lookup"><span data-stu-id="d3c92-254">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="d3c92-255">С помощью вспомогательных функций тегов вы работаете, изначально в C#, поэтому преобразование типов не требуется.</span><span class="sxs-lookup"><span data-stu-id="d3c92-255">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="d3c92-256">Веб-элементы управления используйте [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) для реализации поведения компонентов и элементов управления во время выполнения и во время разработки.</span><span class="sxs-lookup"><span data-stu-id="d3c92-256">Web Server controls use [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="d3c92-257">`System.ComponentModel`включает базовые классы и интерфейсы для реализации атрибутов и преобразователей типов, привязки к источникам данных и лицензирования компонентов.</span><span class="sxs-lookup"><span data-stu-id="d3c92-257">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="d3c92-258">Сравните это вспомогательных функций тегов, который обычно являются производными от `TagHelper`и `TagHelper` базовый класс предоставляет только два метода, `Process` и `ProcessAsync`.</span><span class="sxs-lookup"><span data-stu-id="d3c92-258">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="d3c92-259">Настройка шрифта элемента вспомогательный тега</span><span class="sxs-lookup"><span data-stu-id="d3c92-259">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="d3c92-260">Можно настроить шрифт и цветовая схема из **средства** > **параметры** > **среды** > **шрифтов Цвета и**:</span><span class="sxs-lookup"><span data-stu-id="d3c92-260">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![изображение](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a><span data-ttu-id="d3c92-262">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d3c92-262">Additional Resources</span></span>

* [<span data-ttu-id="d3c92-263">Создание вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="d3c92-263">Authoring Tag Helpers</span></span>](authoring.md)
* [<span data-ttu-id="d3c92-264">Работа с формами</span><span class="sxs-lookup"><span data-stu-id="d3c92-264">Working with Forms </span></span>](../working-with-forms.md)
* <span data-ttu-id="d3c92-265">[TagHelperSamples на GitHub](https://github.com/dpaquette/TagHelperSamples) содержит вспомогательные тег образцов для работы с [начальной загрузки](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="d3c92-265">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](http://getbootstrap.com/).</span></span>

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
