---
title: Вспомогательные функции тегов в ASP.NET Core
author: rick-anderson
description: Сведения о вспомогательных функциях тегов и их использовании в ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2018
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: c2af9099fe439e1cdbf9ba86ffae3b2b0f67391e
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751719"
---
# <a name="tag-helpers-in-aspnet-core"></a><span data-ttu-id="ab1af-103">Вспомогательные функции тегов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab1af-103">Tag Helpers in ASP.NET Core</span></span>

<span data-ttu-id="ab1af-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="ab1af-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="ab1af-105">Что такое вспомогательные функции тегов?</span><span class="sxs-lookup"><span data-stu-id="ab1af-105">What are Tag Helpers?</span></span>

<span data-ttu-id="ab1af-106">Вспомогательные функции тегов позволяют серверному коду участвовать в создании и отрисовке HTML-элементов в файлах Razor.</span><span class="sxs-lookup"><span data-stu-id="ab1af-106">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="ab1af-107">Например, встроенная функция `ImageTagHelper` может добавить номер версии к имени изображения.</span><span class="sxs-lookup"><span data-stu-id="ab1af-107">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="ab1af-108">При каждом изменении изображения сервер генерирует новую уникальную версию изображения, поэтому клиенты гарантированно получат текущую версию изображения (вместо устаревшего кэшированного изображения).</span><span class="sxs-lookup"><span data-stu-id="ab1af-108">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="ab1af-109">Существует множество встроенных вспомогательных функций тегов для общих задач — например, для создания форм, ссылок, загрузки ресурсов и т. д. Кроме того, огромное количество функций доступно в общедоступных репозиториях GitHub и в качестве пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="ab1af-109">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="ab1af-110">Вспомогательные функции тегов разрабатываются на C# и предназначены для HTML-элементов на основе имени элемента, имени атрибута или родительского тега.</span><span class="sxs-lookup"><span data-stu-id="ab1af-110">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="ab1af-111">Например, встроенная функция `LabelTagHelper` может работать с HTML-элементом `<label>`, если применены атрибуты `LabelTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="ab1af-111">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="ab1af-112">Если вы знакомы со [вспомогательными методами HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), то поймете, что вспомогательные функции тегов сокращают количество явных переходов между HTML и C# в представлениях Razor.</span><span class="sxs-lookup"><span data-stu-id="ab1af-112">If you're familiar with [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="ab1af-113">Во многих случаях вспомогательные методы HTML располагают альтернативными вариантами для определенной вспомогательной функции тега, но следует отметить, что вспомогательные функции тегов не заменяют вспомогательные методы HTML и для каждого вспомогательного метода HTML не существует конкретной вспомогательной функции тега.</span><span class="sxs-lookup"><span data-stu-id="ab1af-113">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="ab1af-114">Более подробно различия описаны в разделе о [сравнении вспомогательных методов HTML и вспомогательных функций тегов](#tag-helpers-compared-to-html-helpers).</span><span class="sxs-lookup"><span data-stu-id="ab1af-114">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="ab1af-115">Возможности и преимущества вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="ab1af-115">What Tag Helpers provide</span></span>

<span data-ttu-id="ab1af-116">**Большая схожесть с HTML**. В большинстве случаев разметка Razor, где используются вспомогательные функции тегов, выглядит как стандартный HTML.</span><span class="sxs-lookup"><span data-stu-id="ab1af-116">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="ab1af-117">Разработчики интерфейсной части, работающие с HTML, CSS и JavaScript, могут редактировать Razor без изучения синтаксиса C# Razor.</span><span class="sxs-lookup"><span data-stu-id="ab1af-117">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="ab1af-118">**Полнофункциональная среда IntelliSense для создания разметки HTML и Razor**. Этим вспомогательные функции тегов существенно отличаются от вспомогательных методов HTML, более раннего подхода для создания серверной разметки в представлениях Razor.</span><span class="sxs-lookup"><span data-stu-id="ab1af-118">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="ab1af-119">Более подробно различия описаны в разделе о [сравнении вспомогательных методов HTML и вспомогательных функций тегов](#tag-helpers-compared-to-html-helpers).</span><span class="sxs-lookup"><span data-stu-id="ab1af-119">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="ab1af-120">Возможности среды IntelliSense объясняются в статье [Поддержка IntelliSense для вспомогательных функций тегов](#intellisense-support-for-tag-helpers).</span><span class="sxs-lookup"><span data-stu-id="ab1af-120">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="ab1af-121">Даже разработчики с опытом в области синтаксиса Razor C# работают более продуктивно, используя вспомогательные функции тегов, нежели создавая разметку C# Razor.</span><span class="sxs-lookup"><span data-stu-id="ab1af-121">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="ab1af-122">**Вы сможете работать более продуктивно и создавать более надежный, устойчивый и безопасный код, используя информацию, которая доступна только на сервере**. Например, когда ранее требовалось изменить изображение, нужно было изменить имя изображения.</span><span class="sxs-lookup"><span data-stu-id="ab1af-122">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="ab1af-123">Изображения нужно было обязательно кэшировать по причинам эффективности, и до тех пор, пока не менялось имя изображения, клиенты могли получать его устаревшую копию.</span><span class="sxs-lookup"><span data-stu-id="ab1af-123">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="ab1af-124">Исторически после редактирования изображения нужно было обязательно менять его имя и обновлять каждую ссылку на него в веб приложении.</span><span class="sxs-lookup"><span data-stu-id="ab1af-124">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="ab1af-125">Мало того, что это трудоемкий процесс, так он еще подвержен возникновению ошибок (можно пропустить ссылку, случайно ввести неправильную строку и т. д.). Встроенная функция `ImageTagHelper` может сделать это автоматически.</span><span class="sxs-lookup"><span data-stu-id="ab1af-125">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="ab1af-126">Функция `ImageTagHelper` добавляет номер версии к имени изображения, поэтому при каждом изменении изображения сервер автоматически генерирует новую уникальную версию изображения.</span><span class="sxs-lookup"><span data-stu-id="ab1af-126">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="ab1af-127">Клиенты гарантированно получают текущее изображение.</span><span class="sxs-lookup"><span data-stu-id="ab1af-127">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="ab1af-128">Все это достигается с помощью функции `ImageTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="ab1af-128">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="ab1af-129">Большинство встроенных вспомогательных функций тегов работают со стандартными HTML-элементами и предоставляют для них атрибуты на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="ab1af-129">Most built-in Tag Helpers target standard HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="ab1af-130">Например, элемент `<input>`, используемый во многих представлениях в папке *Представления/учетная запись*, содержит атрибут `asp-for`.</span><span class="sxs-lookup"><span data-stu-id="ab1af-130">For example, the `<input>` element used in many views in the *Views/Account* folder contains the `asp-for` attribute.</span></span> <span data-ttu-id="ab1af-131">Этот атрибут извлекает имя свойства указанной модели в готовый для просмотра HTML-код.</span><span class="sxs-lookup"><span data-stu-id="ab1af-131">This attribute extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="ab1af-132">Рассмотрим представление Razor с помощью следующей модели:</span><span class="sxs-lookup"><span data-stu-id="ab1af-132">Consider a Razor view with the following model:</span></span>

```csharp
public class Movie
{
    public int ID { get; set; }
    public string Title { get; set; }
    public DateTime ReleaseDate { get; set; }
    public string Genre { get; set; }
    public decimal Price { get; set; }
}
```

<span data-ttu-id="ab1af-133">Следующая разметка Razor:</span><span class="sxs-lookup"><span data-stu-id="ab1af-133">The following Razor markup:</span></span>

```cshtml
<label asp-for="Movie.Title"></label>
```

<span data-ttu-id="ab1af-134">Генерирует следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="ab1af-134">Generates the following HTML:</span></span>

```html
<label for="Movie_Title">Title</label>
```

<span data-ttu-id="ab1af-135">Атрибут `asp-for` предоставляется свойством `For` в [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="ab1af-135">The `asp-for` attribute is made available by the `For` property in the [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0).</span></span> <span data-ttu-id="ab1af-136">Дополнительные сведения см. в статье [Создание вспомогательных функций тегов](xref:mvc/views/tag-helpers/authoring).</span><span class="sxs-lookup"><span data-stu-id="ab1af-136">See [Author Tag Helpers](xref:mvc/views/tag-helpers/authoring) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="ab1af-137">Управление областью действия вспомогательной функции тега</span><span class="sxs-lookup"><span data-stu-id="ab1af-137">Managing Tag Helper scope</span></span>

<span data-ttu-id="ab1af-138">Управление областью действия вспомогательных функций тегов осуществляется с помощью сочетания `@addTagHelper`, `@removeTagHelper` и символа отказа "!".</span><span class="sxs-lookup"><span data-stu-id="ab1af-138">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="ab1af-139">Директива `@addTagHelper` обеспечивает доступность вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="ab1af-139">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="ab1af-140">При создании веб-приложения ASP.NET Core с именем *AuthoringTagHelpers* в проект будет добавлен следующий файл *Views/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ab1af-140">If you create a new ASP.NET Core web app named *AuthoringTagHelpers*, the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="ab1af-141">Директива `@addTagHelper` делает вспомогательные функции тегов доступными в представлении.</span><span class="sxs-lookup"><span data-stu-id="ab1af-141">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="ab1af-142">В этом случае файлом представления является *Pages/_ViewImports.cshtml*, который по умолчанию наследуется всеми файлами в папке *Pages* и вложенными папками. Таким образом, вспомогательные функции тегов доступны для всех.</span><span class="sxs-lookup"><span data-stu-id="ab1af-142">In this case, the view file is *Pages/_ViewImports.cshtml*, which by default is inherited by all files in the *Pages* folder and sub-folders; making Tag Helpers available.</span></span> <span data-ttu-id="ab1af-143">В приведенном выше коде используется синтаксис знаков подстановки ("\*") для указания того, что все вспомогательные функции тегов в указанной сборке (*Microsoft.AspNetCore.Mvc.TagHelpers*) будут доступны для каждого файла представления в каталоге *Views* или вложенном каталоге.</span><span class="sxs-lookup"><span data-stu-id="ab1af-143">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or sub-directory.</span></span> <span data-ttu-id="ab1af-144">Первый параметр после директивы `@addTagHelper` указывает загружаемые вспомогательные функции тегов (мы используем "\*" для всех вспомогательных функций тегов), а второй параметр "Microsoft.AspNetCore.Mvc.TagHelpers" указывает сборку, содержащую вспомогательные функции тегов.</span><span class="sxs-lookup"><span data-stu-id="ab1af-144">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="ab1af-145">*Microsoft.AspNetCore.Mvc.TagHelpers* — это сборка для встроенных вспомогательных функций тегов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ab1af-145">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="ab1af-146">Чтобы использовать все вспомогательные функции тегов в этом проекте (где создается сборка *AuthoringTagHelpers*), нужно сделать следующее:</span><span class="sxs-lookup"><span data-stu-id="ab1af-146">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="ab1af-147">Если проект содержит `EmailTagHelper` с пространством имен по умолчанию (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), можно указать полное имя (FQN) вспомогательной функции тега:</span><span class="sxs-lookup"><span data-stu-id="ab1af-147">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="ab1af-148">Чтобы добавить вспомогательную функцию тега в представление с помощью FQN, сначала добавьте FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), а затем — имя сборки (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="ab1af-148">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="ab1af-149">Большинство разработчиков предпочитают использовать синтаксис знаков подстановки "\*".</span><span class="sxs-lookup"><span data-stu-id="ab1af-149">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="ab1af-150">Синтаксис знаков подстановки позволяет вставлять знак подстановки "\*" в FQN в качестве суффикса.</span><span class="sxs-lookup"><span data-stu-id="ab1af-150">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="ab1af-151">Например, любая из следующих директив добавит `EmailTagHelper`:</span><span class="sxs-lookup"><span data-stu-id="ab1af-151">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="ab1af-152">Как упоминалось ранее, добавление директивы `@addTagHelper` в файл *Views/_ViewImports.cshtml* делает вспомогательную функцию тега доступной для всех файлов представлений в каталоге *Views* и вложенных каталогах.</span><span class="sxs-lookup"><span data-stu-id="ab1af-152">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and sub-directories.</span></span> <span data-ttu-id="ab1af-153">Директиву `@addTagHelper` можно использовать в конкретных файлах представлений, если вы хотите, чтобы вспомогательная функция тега находилась только в этих представлениях.</span><span class="sxs-lookup"><span data-stu-id="ab1af-153">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="ab1af-154">Директива `@removeTagHelper` удаляет вспомогательные функций тегов</span><span class="sxs-lookup"><span data-stu-id="ab1af-154">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="ab1af-155">У директивы `@removeTagHelper` есть те же два параметра, что и у директивы `@addTagHelper`, и она удаляет ранее добавленную вспомогательную функцию тега.</span><span class="sxs-lookup"><span data-stu-id="ab1af-155">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="ab1af-156">Например, если применить директиву `@removeTagHelper` к определенному представлению, она удалит из него указанную вспомогательную функцию тега.</span><span class="sxs-lookup"><span data-stu-id="ab1af-156">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="ab1af-157">Использование директивы `@removeTagHelper` в файле *Views/Folder/_ViewImports.cshtml* приводит к удалению указанной вспомогательной функции тега из всех представлений в *папке*.</span><span class="sxs-lookup"><span data-stu-id="ab1af-157">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="ab1af-158">Контроль области действия вспомогательной функции тега с помощью файла *_ViewImports.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ab1af-158">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="ab1af-159">Можно добавить файл *_ViewImports.cshtml* в любую папку представления, и подсистема представлений применит директивы из этого файла и файла *Views/_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ab1af-159">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="ab1af-160">Если вы добавили пустой файл *Views/Home/_ViewImports.cshtml* для представлений *Home*, ничего не изменится, поскольку файл *_ViewImports.cshtml* является дополнительным.</span><span class="sxs-lookup"><span data-stu-id="ab1af-160">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="ab1af-161">Любая директива `@addTagHelper`, добавляемая в файл *Views/Home/_ViewImports.cshtml* (и не входящая в файл по умолчанию *Views/_ViewImports.cshtml*), будет предоставлять эти вспомогательные функции тегов для представлений только в папке *Home*.</span><span class="sxs-lookup"><span data-stu-id="ab1af-161">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="ab1af-162">Отказ от использования отдельных элементов</span><span class="sxs-lookup"><span data-stu-id="ab1af-162">Opting out of individual elements</span></span>

<span data-ttu-id="ab1af-163">Можно отключить вспомогательную функцию тега на уровне элемента с помощью символа отказа от использования ("!").</span><span class="sxs-lookup"><span data-stu-id="ab1af-163">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="ab1af-164">Например, с помощью данного символа отключена проверка `Email` в `<span>`:</span><span class="sxs-lookup"><span data-stu-id="ab1af-164">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="ab1af-165">Символ отказа от использования вспомогательной функции тега необходимо применять в открывающем и закрывающем тегах.</span><span class="sxs-lookup"><span data-stu-id="ab1af-165">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="ab1af-166">(Редактор Visual Studio автоматически добавляет символ отказа в закрывающий тег, если такой символ добавлен в открывающий тег.)</span><span class="sxs-lookup"><span data-stu-id="ab1af-166">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="ab1af-167">После добавления символа отказа атрибуты элемента и вспомогательной функции больше не будут отображаться отдельным шрифтом.</span><span class="sxs-lookup"><span data-stu-id="ab1af-167">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="ab1af-168">Применение `@tagHelperPrefix` для явного использования вспомогательной функции тега</span><span class="sxs-lookup"><span data-stu-id="ab1af-168">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="ab1af-169">Директива `@tagHelperPrefix` позволяет указать строку префикса тега для включения поддержки вспомогательной функции тега и явного использования этой функции.</span><span class="sxs-lookup"><span data-stu-id="ab1af-169">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="ab1af-170">Например, можно добавить следующую разметку в файл *Views/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ab1af-170">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```cshtml
@tagHelperPrefix th:
```
<span data-ttu-id="ab1af-171">На приведенном ниже рисунке кода задан префикс `th:` вспомогательной функции тега, поэтому вспомогательную функцию тега поддерживают только элементы с префиксом `th:` (элементы с поддержкой вспомогательной функции тега выделены особым шрифтом).</span><span class="sxs-lookup"><span data-stu-id="ab1af-171">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="ab1af-172">У элементов `<label>` и `<input>` есть префикс вспомогательной функции тега и поддержка этой функции, а у элемента `<span>` этого префикса и поддержки нет.</span><span class="sxs-lookup"><span data-stu-id="ab1af-172">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element doesn't.</span></span>

![изображение](intro/_static/thp.png)

<span data-ttu-id="ab1af-174">Правила иерархии, которые применяются к `@addTagHelper`, также применяются и к `@tagHelperPrefix`.</span><span class="sxs-lookup"><span data-stu-id="ab1af-174">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="ab1af-175">Поддержка Intellisense для вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="ab1af-175">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="ab1af-176">Веб-приложение ASP.NET Core, создаваемое в Visual Studio, добавляет пакет NuGet "Microsoft.AspNetCore.Razor.Tools".</span><span class="sxs-lookup"><span data-stu-id="ab1af-176">When you create a new ASP.NET Core web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="ab1af-177">Это пакет добавляет инструментарий вспомогательной функции тега.</span><span class="sxs-lookup"><span data-stu-id="ab1af-177">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="ab1af-178">Рекомендуется написать элемент HTML `<label>`.</span><span class="sxs-lookup"><span data-stu-id="ab1af-178">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="ab1af-179">Когда вы введете `<l` в редакторе Visual Studio, IntelliSense отобразит подходящие элементы:</span><span class="sxs-lookup"><span data-stu-id="ab1af-179">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![изображение](intro/_static/label.png)

<span data-ttu-id="ab1af-181">Выводится не только справка по HTML, но и значок ("@" symbol with "<>" под ней).</span><span class="sxs-lookup"><span data-stu-id="ab1af-181">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![изображение](intro/_static/tagSym.png)

<span data-ttu-id="ab1af-183">Он показывает элемент, на который нацелены вспомогательные функции тегов.</span><span class="sxs-lookup"><span data-stu-id="ab1af-183">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="ab1af-184">Чистые элементы HTML (например, `fieldset`) отображают значок "<>".</span><span class="sxs-lookup"><span data-stu-id="ab1af-184">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="ab1af-185">Чистый тег HTML `<label>` отображается (с помощью базовой цветовой гаммы Visual Studio) коричневым цветом, атрибуты — красным, а значения атрибутов — синим.</span><span class="sxs-lookup"><span data-stu-id="ab1af-185">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![изображение](intro/_static/LableHtmlTag.png)

<span data-ttu-id="ab1af-187">После того как вы введете `<label`, IntelliSense выведет перечень доступных атрибутов HTML и CSS и атрибутов для вспомогательной функции тега:</span><span class="sxs-lookup"><span data-stu-id="ab1af-187">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![изображение](intro/_static/labelattr.png)

<span data-ttu-id="ab1af-189">Функция завершения операторов IntelliSense позволяет заполнить выражение выбранным значением с помощью клавиши TAB:</span><span class="sxs-lookup"><span data-stu-id="ab1af-189">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![изображение](intro/_static/stmtcomplete.png)

<span data-ttu-id="ab1af-191">После ввода атрибута вспомогательной функции тега шрифты тега и атрибута изменяются.</span><span class="sxs-lookup"><span data-stu-id="ab1af-191">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="ab1af-192">При использовании базовых цветовых схем Visual Studio "Синяя" или "Светлая" шрифт будет пурпурным с полужирным начертанием.</span><span class="sxs-lookup"><span data-stu-id="ab1af-192">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="ab1af-193">Если используется тема "Темная", шрифт будет сине-зеленым с полужирным начертанием.</span><span class="sxs-lookup"><span data-stu-id="ab1af-193">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="ab1af-194">Изображения в этом документе были созданы с помощью темы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ab1af-194">The images in this document were taken using the default theme.</span></span>

![изображение](intro/_static/labelaspfor2.png)

<span data-ttu-id="ab1af-196">Вы можете использовать сочетание клавиш Visual Studio *CompleteWord* (CTRL+пробел [по умолчанию](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) внутри двойных кавычек (""), чтобы переключиться на C# так же, как если бы вы находились в классе C#.</span><span class="sxs-lookup"><span data-stu-id="ab1af-196">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="ab1af-197">IntelliSense отобразит все методы и свойства на модели страницы.</span><span class="sxs-lookup"><span data-stu-id="ab1af-197">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="ab1af-198">Методы и свойства доступны, поскольку типом свойства является `ModelExpression`.</span><span class="sxs-lookup"><span data-stu-id="ab1af-198">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="ab1af-199">На рисунке ниже выполняется редактирование представления `Register`, поэтому доступен `RegisterViewModel`.</span><span class="sxs-lookup"><span data-stu-id="ab1af-199">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![изображение](intro/_static/intellemail.png)

<span data-ttu-id="ab1af-201">IntelliSense выводит свойства и методы, доступные для модели на странице.</span><span class="sxs-lookup"><span data-stu-id="ab1af-201">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="ab1af-202">Благодаря широким возможностям IntelliSense помогает выбрать класс CSS:</span><span class="sxs-lookup"><span data-stu-id="ab1af-202">The rich IntelliSense environment helps you select the CSS class:</span></span>

![изображение](intro/_static/iclass.png)

![изображение](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="ab1af-205">Сравнение вспомогательных функций тегов со вспомогательными методами HTML</span><span class="sxs-lookup"><span data-stu-id="ab1af-205">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="ab1af-206">Вспомогательные функции тегов присоединяются к HTML-элементам в представлениях Razor, тогда как [вспомогательные методы HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) вызываются как методы, смешанные с HTML, в представлениях Razor.</span><span class="sxs-lookup"><span data-stu-id="ab1af-206">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="ab1af-207">Рассмотрим следующую разметку Razor, которая создает метку HTML с классом CSS "caption":</span><span class="sxs-lookup"><span data-stu-id="ab1af-207">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="ab1af-208">Символ at (`@`) указывает Razor, что это начало кода.</span><span class="sxs-lookup"><span data-stu-id="ab1af-208">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="ab1af-209">Следующие два параметра ("FirstName" и "First Name:") представляют собой строки, поэтому [IntelliSense](/visualstudio/ide/using-intellisense) помочь здесь не может.</span><span class="sxs-lookup"><span data-stu-id="ab1af-209">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="ab1af-210">Последний аргумент:</span><span class="sxs-lookup"><span data-stu-id="ab1af-210">The last argument:</span></span>

```cshtml
new {@class="caption"}
```

<span data-ttu-id="ab1af-211">является анонимным объектом, который используется для представления атрибутов.</span><span class="sxs-lookup"><span data-stu-id="ab1af-211">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="ab1af-212">Поскольку <strong>class</strong> является зарезервированным ключевым словом в C#, используется символ `@`, чтобы заставить C# интерпретировать "@class=" как символ (имя свойства).</span><span class="sxs-lookup"><span data-stu-id="ab1af-212">Because <strong>class</strong> is a reserved keyword in C#, you use the `@` symbol to force C# to interpret "@class=" as a symbol (property name).</span></span> <span data-ttu-id="ab1af-213">Для разработчиков интерфейсной части (тех, кто знаком с HTML, CSS, JavaScript и другими клиентскими технологиями, но не знаком с C# и Razor), большая часть этой строки непонятна.</span><span class="sxs-lookup"><span data-stu-id="ab1af-213">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="ab1af-214">И вся строка должны быть написана без помощи IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="ab1af-214">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="ab1af-215">С помощью функции `LabelTagHelper` та же разметка может быть написана следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ab1af-215">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

![изображение](intro/_static/label2.png)

<span data-ttu-id="ab1af-217">При использовании вспомогательной функции тега, как только вы вводите `<l` в редакторе Visual Studio, IntelliSense отобразит подходящие элементы:</span><span class="sxs-lookup"><span data-stu-id="ab1af-217">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![изображение](intro/_static/label.png)

<span data-ttu-id="ab1af-219">IntelliSense помогает написать всю строку.</span><span class="sxs-lookup"><span data-stu-id="ab1af-219">IntelliSense helps you write the entire line.</span></span> <span data-ttu-id="ab1af-220">Функция `LabelTagHelper` также задает содержимое значения атрибута `asp-for` ("FirstName") на "First Name". Она преобразует свойства в стиле Camel в выражения, состоящие из имени свойства с пробелом, где все пишется с прописной буквы.</span><span class="sxs-lookup"><span data-stu-id="ab1af-220">The `LabelTagHelper` also defaults to setting the content of the `asp-for` attribute value ("FirstName") to "First Name"; It converts camel-cased properties to a sentence composed of the property name with a space where each new upper-case letter occurs.</span></span> <span data-ttu-id="ab1af-221">В следующей разметке:</span><span class="sxs-lookup"><span data-stu-id="ab1af-221">In the following markup:</span></span>

![изображение](intro/_static/label2.png)

<span data-ttu-id="ab1af-223">генерируется:</span><span class="sxs-lookup"><span data-stu-id="ab1af-223">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

<span data-ttu-id="ab1af-224">Преобразование из стиля Camel в стиль предложения не используется при добавлении содержимого в `<label>`.</span><span class="sxs-lookup"><span data-stu-id="ab1af-224">The camel-cased to sentence-cased content isn't used if you add content to the `<label>`.</span></span> <span data-ttu-id="ab1af-225">Пример:</span><span class="sxs-lookup"><span data-stu-id="ab1af-225">For example:</span></span>

![изображение](intro/_static/1stName.png)

<span data-ttu-id="ab1af-227">генерируется:</span><span class="sxs-lookup"><span data-stu-id="ab1af-227">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

<span data-ttu-id="ab1af-228">На следующем рисунке кода показана часть Form представления Razor *Views/Account/Register.cshtml*, созданного из шаблона прежних версий MVC 4.5.x ASP.NET, входящего в состав Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="ab1af-228">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the legacy ASP.NET 4.5.x MVC template included with Visual Studio 2015.</span></span>

![изображение](intro/_static/regCS.png)

<span data-ttu-id="ab1af-230">Редактор Visual Studio отображает код C# на сером фоне.</span><span class="sxs-lookup"><span data-stu-id="ab1af-230">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="ab1af-231">Например, вспомогательный метод HTML `AntiForgeryToken`:</span><span class="sxs-lookup"><span data-stu-id="ab1af-231">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```cshtml
@Html.AntiForgeryToken()
```

<span data-ttu-id="ab1af-232">отображается на сером фоне.</span><span class="sxs-lookup"><span data-stu-id="ab1af-232">is displayed with a grey background.</span></span> <span data-ttu-id="ab1af-233">Большая часть разметки в представлении Register является кодом C#.</span><span class="sxs-lookup"><span data-stu-id="ab1af-233">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="ab1af-234">Сравните это с эквивалентным подходом, где используются вспомогательные функции тегов:</span><span class="sxs-lookup"><span data-stu-id="ab1af-234">Compare that to the equivalent approach using Tag Helpers:</span></span>

![изображение](intro/_static/regTH.png)

<span data-ttu-id="ab1af-236">Разметка здесь более чистая, ее проще читать, редактировать и поддерживать, чем при использовании вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="ab1af-236">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="ab1af-237">Количество C# кода уменьшено до минимума — здесь есть только то, что необходимо знать серверу.</span><span class="sxs-lookup"><span data-stu-id="ab1af-237">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="ab1af-238">Редактор Visual Studio отображает разметку, с которой работает вспомогательная функция тега, особым шрифтом.</span><span class="sxs-lookup"><span data-stu-id="ab1af-238">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="ab1af-239">Рассмотрим группу *Email*:</span><span class="sxs-lookup"><span data-stu-id="ab1af-239">Consider the *Email* group:</span></span>

[!code-csharp[](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="ab1af-240">У каждого из атрибутов "asp-" есть значение "Email", но "Email" не является строкой.</span><span class="sxs-lookup"><span data-stu-id="ab1af-240">Each of the "asp-" attributes has a value of "Email", but "Email" isn't a string.</span></span> <span data-ttu-id="ab1af-241">В данном контексте "Email" — это свойство модельного выражения C# для `RegisterViewModel`.</span><span class="sxs-lookup"><span data-stu-id="ab1af-241">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="ab1af-242">Редактор Visual Studio помогает написать **всю** разметку, если вы используете вспомогательную функцию тега, а если вы применяете вспомогательные методы HTML, помощи Visual Studio для большей части кода можно не ожидать.</span><span class="sxs-lookup"><span data-stu-id="ab1af-242">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="ab1af-243">Дополнительные сведения о работе со вспомогательными функциями тегов в редакторе Visual Studio см. в статье [Поддержка IntelliSense для вспомогательных функций тегов](#intellisense-support-for-tag-helpers).</span><span class="sxs-lookup"><span data-stu-id="ab1af-243">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="ab1af-244">Сравнение вспомогательных функций тегов с серверными веб-элементами управления</span><span class="sxs-lookup"><span data-stu-id="ab1af-244">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="ab1af-245">Вспомогательным функциям тегов не принадлежит элемент, с которыми они связаны. Они просто участвуют в отрисовке элемента и содержимого.</span><span class="sxs-lookup"><span data-stu-id="ab1af-245">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="ab1af-246">[Серверные веб-элементы управления](https://msdn.microsoft.com/library/7698y1f0.aspx) ASP.NET объявляются и вызываются на странице.</span><span class="sxs-lookup"><span data-stu-id="ab1af-246">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="ab1af-247">[Серверные веб-элементы управления](https://msdn.microsoft.com/library/zsyt68f1.aspx) имеют нестандартный жизненный цикл, что усложняет разработку и отладку.</span><span class="sxs-lookup"><span data-stu-id="ab1af-247">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="ab1af-248">Серверные веб-элементы управления позволяют добавлять функционал в элементы DOM с помощью клиентского элемента управления.</span><span class="sxs-lookup"><span data-stu-id="ab1af-248">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="ab1af-249">У вспомогательных функций тегов отсутствует DOM.</span><span class="sxs-lookup"><span data-stu-id="ab1af-249">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="ab1af-250">Серверные веб-элементы управления включают в себя автоматическое обнаружение браузера.</span><span class="sxs-lookup"><span data-stu-id="ab1af-250">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="ab1af-251">Вспомогательные функции тегов ничего не знают о браузере.</span><span class="sxs-lookup"><span data-stu-id="ab1af-251">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="ab1af-252">Несколько вспомогательных функций тегов могут работать с одним и тем же элементом (см. статью о [предотвращении конфликтов вспомогательной функции тега](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts)), серверные веб-элементы управления обычно нельзя комбинировать.</span><span class="sxs-lookup"><span data-stu-id="ab1af-252">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="ab1af-253">Вспомогательные функции тегов могут менять тег и содержимое HTML-элементов, с которыми они связаны, но напрямую они не влияют больше ни на какие элементы на странице.</span><span class="sxs-lookup"><span data-stu-id="ab1af-253">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="ab1af-254">Серверные веб-элементы управления имеют менее конкретную область действия и могут выполнять действия, которые влияют на другие части страницы, и это иногда может привести к побочным эффектам.</span><span class="sxs-lookup"><span data-stu-id="ab1af-254">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="ab1af-255">Серверные веб-элементы управления используют преобразователи типов для преобразования строк в объекты.</span><span class="sxs-lookup"><span data-stu-id="ab1af-255">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="ab1af-256">Со вспомогательными функциями тегов вы работаете изначально в C#, поэтому преобразование типов не требуется.</span><span class="sxs-lookup"><span data-stu-id="ab1af-256">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="ab1af-257">Серверные веб-элементы управления используют [System.ComponentModel](/dotnet/api/system.componentmodel), чтобы реализовать поведение компонентов и элементов управления во время разработки и выполнения.</span><span class="sxs-lookup"><span data-stu-id="ab1af-257">Web Server controls use [System.ComponentModel](/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="ab1af-258">`System.ComponentModel` содержит базовые классы и интерфейсы для реализации атрибутов и преобразователей типов, привязки к источникам данных и лицензирования компонентов.</span><span class="sxs-lookup"><span data-stu-id="ab1af-258">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="ab1af-259">Сравните это со вспомогательными функциями тегов, которые обычно являются производными от `TagHelper`, а базовый класс `TagHelper` предоставляет только два метода — `Process` и `ProcessAsync`.</span><span class="sxs-lookup"><span data-stu-id="ab1af-259">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="ab1af-260">Настройка шрифта элемента вспомогательной функции тега</span><span class="sxs-lookup"><span data-stu-id="ab1af-260">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="ab1af-261">Вы можете настроить шрифт и цвет, последовательно выбрав **Сервис** > **Параметры** > **Среда** > **Шрифты и цвета**:</span><span class="sxs-lookup"><span data-stu-id="ab1af-261">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![изображение](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a><span data-ttu-id="ab1af-263">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ab1af-263">Additional resources</span></span>

* [<span data-ttu-id="ab1af-264">Создание вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="ab1af-264">Author Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)
* [<span data-ttu-id="ab1af-265">Работа с формами </span><span class="sxs-lookup"><span data-stu-id="ab1af-265">Working with Forms </span></span>](xref:mvc/views/working-with-forms)
* <span data-ttu-id="ab1af-266">[TagHelperSamples на GitHub](https://github.com/dpaquette/TagHelperSamples) содержит примеры вспомогательной функции тега для работы с [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="ab1af-266">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](http://getbootstrap.com/).</span></span>
