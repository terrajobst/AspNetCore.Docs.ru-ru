---
title: "Сформированные страницы Razor Pages в ASP.NET Core"
author: rick-anderson
description: "Описание страниц Razor Pages, созданных путем формирования шаблонов."
keywords: ASP.NET Core,Razor Pages,Razor,MVC
ms.author: riande
manager: wpickett
ms.date: 7/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: 02cbc7c7caf5128167dd3ecfdc0e2340f4876df5
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2017
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="78983-104">Сформированные страницы Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78983-104">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="78983-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="78983-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="78983-106">В этом учебнике изучаются страницы Razor Pages, созданные путем формирования шаблонов в [предыдущем учебнике](xref:tutorials/razor-pages/page).</span><span class="sxs-lookup"><span data-stu-id="78983-106">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/page).</span></span> 

<span data-ttu-id="78983-107">[Просмотрите или скачайте](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) пример.</span><span class="sxs-lookup"><span data-stu-id="78983-107">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="78983-108">Страницы Create, Delete, Details и Edit</span><span class="sxs-lookup"><span data-stu-id="78983-108">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="78983-109">Изучите файл кода программной части *Pages/Movies/Index.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="78983-109">Examine the *Pages/Movies/Index.cshtml.cs* code-behind file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml.cs)]</span></span>

<span data-ttu-id="78983-110">Страницы Razor Pages являются производными от `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="78983-110">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="78983-111">Как правило, класс, производный от `PageModel`, называется `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="78983-111">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="78983-112">Используя [внедрение зависимостей](xref:fundamentals/dependency-injection), конструктор добавляет на страницу `MovieContext`.</span><span class="sxs-lookup"><span data-stu-id="78983-112">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="78983-113">Этому шаблону соответствуют все сформированные страницы.</span><span class="sxs-lookup"><span data-stu-id="78983-113">All the scaffolded pages follow this pattern.</span></span>

<span data-ttu-id="78983-114">Когда к странице направляется запрос, метод `OnGetAsync` возвращает на страницу Razor список фильмов.</span><span class="sxs-lookup"><span data-stu-id="78983-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="78983-115">На странице Razor вызывается метод `OnGetAsync` или `OnGet`, инициализирующий состояние для страницы.</span><span class="sxs-lookup"><span data-stu-id="78983-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="78983-116">В этом случае `OnGetAsync` возвращает список фильмов для отображения.</span><span class="sxs-lookup"><span data-stu-id="78983-116">In this case, `OnGetAsync` gets a list of movies to display.</span></span>

<span data-ttu-id="78983-117">Изучите страницу Razor *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="78983-117">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

<span data-ttu-id="78983-118">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="78983-118">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml)]</span></span>

<span data-ttu-id="78983-119">Razor может выполнять переход с HTML на C# или на разметку Razor.</span><span class="sxs-lookup"><span data-stu-id="78983-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="78983-120">Если за символом `@` следует [зарезервированное ключевое слово Razor](xref:mvc/views/razor#razor-reserved-keywords), он переходит на разметку Razor, а если нет, то на C#.</span><span class="sxs-lookup"><span data-stu-id="78983-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="78983-121">Директива Razor `@page` преобразует файл в действие MVC &mdash;, а значит, он может обрабатывать запросы.</span><span class="sxs-lookup"><span data-stu-id="78983-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="78983-122">Директива `@page` должна быть первой директивой Razor на странице.</span><span class="sxs-lookup"><span data-stu-id="78983-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="78983-123">`@page` — это пример перехода на разметку Razor.</span><span class="sxs-lookup"><span data-stu-id="78983-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="78983-124">Дополнительные сведения см. в статье [Синтаксис Razor](xref:mvc/views/razor#razor-syntax).</span><span class="sxs-lookup"><span data-stu-id="78983-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="78983-125">Проверьте лямбда-выражение, которое используется в следующем вспомогательном методе HTML.</span><span class="sxs-lookup"><span data-stu-id="78983-125">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.Movie[0].Title))`

<span data-ttu-id="78983-126">Вспомогательный метод HTML `DisplayNameFor` проверяет свойство `Title`, указанное в лямбда-выражении, и определяет отображаемое имя.</span><span class="sxs-lookup"><span data-stu-id="78983-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="78983-127">Лямбда-выражение проверяется, а не вычисляется.</span><span class="sxs-lookup"><span data-stu-id="78983-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="78983-128">Это означает, что в случае, если `model`, `model.Movies` или `model.Movies[0]` имеют значение `null` или пусты, права доступа не нарушаются.</span><span class="sxs-lookup"><span data-stu-id="78983-128">That means there is no access violation when `model`, `model.Movies`, or `model.Movies[0]` are `null` or empty.</span></span> <span data-ttu-id="78983-129">При вычислении лямбда-выражения (например, с помощью `@Html.DisplayFor(modelItem => item.Title)`) вычисляются значения для свойств модели.</span><span class="sxs-lookup"><span data-stu-id="78983-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="78983-130">директиву @model;</span><span class="sxs-lookup"><span data-stu-id="78983-130">The @model directive</span></span>

<span data-ttu-id="78983-131">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml?range=1-2&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="78983-131">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml?range=1-2&highlight=2)]</span></span>

<span data-ttu-id="78983-132">Директива `@model` определяет тип модели, передаваемой на страницу Razor.</span><span class="sxs-lookup"><span data-stu-id="78983-132">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="78983-133">В приведенном выше примере строка `@model` делает класс, производный от `PageModel`, доступным для страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="78983-133">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="78983-134">Модель используется на странице во [вспомогательных методах HTML](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` и `@Html.DisplayName`.</span><span class="sxs-lookup"><span data-stu-id="78983-134">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="78983-135"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData и макет</span><span class="sxs-lookup"><span data-stu-id="78983-135"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="78983-136">Рассмотрим следующий код.</span><span class="sxs-lookup"><span data-stu-id="78983-136">Consider the following code:</span></span>

<span data-ttu-id="78983-137">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml?range=1-6&highlight=4-)]</span><span class="sxs-lookup"><span data-stu-id="78983-137">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml?range=1-6&highlight=4-)]</span></span>

<span data-ttu-id="78983-138">Выделенный выше код представляет собой пример перехода Razor на C#.</span><span class="sxs-lookup"><span data-stu-id="78983-138">The preceding highligted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="78983-139">Символы `{` и `}` ограничивают блок кода C#.</span><span class="sxs-lookup"><span data-stu-id="78983-139">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="78983-140">Базовый класс `Controller` содержит свойство словаря `ViewData`, позволяющее передать данные в представление.</span><span class="sxs-lookup"><span data-stu-id="78983-140">The `Controller` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="78983-141">Объекты можно добавить в словарь `ViewData` с помощью шаблона "ключ/значение".</span><span class="sxs-lookup"><span data-stu-id="78983-141">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="78983-142">В приведенном выше примере в словарь `ViewData` добавляется свойство "Title".</span><span class="sxs-lookup"><span data-stu-id="78983-142">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> <span data-ttu-id="78983-143">Свойство "Title" используется в файле *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="78983-143">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="78983-144">Ниже показаны первые несколько строк файла *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="78983-144">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

<span data-ttu-id="78983-145">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]</span><span class="sxs-lookup"><span data-stu-id="78983-145">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]</span></span>

<span data-ttu-id="78983-146">Строка `@*Markup removed for brevity.*@` представляет собой комментарий Razor.</span><span class="sxs-lookup"><span data-stu-id="78983-146">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="78983-147">В отличие от комментариев HTML (`<!-- -->`) комментарии Razor не отправляются клиенту.</span><span class="sxs-lookup"><span data-stu-id="78983-147">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="78983-148">Запустите приложение и проверьте ссылки в проекте (**Главная**, **О программе**, **Контакты**, **Создать**, **Изменить** и **Удалить**).</span><span class="sxs-lookup"><span data-stu-id="78983-148">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="78983-149">Каждая страница задает заголовок, который отображается на вкладке браузера. При добавлении страницы в избранное заголовок используется в закладках.</span><span class="sxs-lookup"><span data-stu-id="78983-149">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="78983-150">В настоящее время страницы *Pages/Index.cshtml* и *Pages/Movies/Index.cshtml* могут иметь один и тот же заголовок, но при желании их заголовки можно изменить.</span><span class="sxs-lookup"><span data-stu-id="78983-150">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

<span data-ttu-id="78983-151">Свойство `Layout` определяется в файле *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="78983-151">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

<span data-ttu-id="78983-152">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="78983-152">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]</span></span>

<span data-ttu-id="78983-153">Представленный выше код задает файл разметки *Pages/_Layout.cshtml* для всех файлов Razor в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="78983-153">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="78983-154">Дополнительные сведения см. в статье о [макете](xref:mvc/razor-pages/index#layout).</span><span class="sxs-lookup"><span data-stu-id="78983-154">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="78983-155">Обновление макета</span><span class="sxs-lookup"><span data-stu-id="78983-155">Update the layout</span></span>

<span data-ttu-id="78983-156">Измените элемент `<title>` в файле *Pages/_Layout.cshtml*, чтобы использовать более короткую строку.</span><span class="sxs-lookup"><span data-stu-id="78983-156">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

<span data-ttu-id="78983-157">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6-)]</span><span class="sxs-lookup"><span data-stu-id="78983-157">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6-)]</span></span>

<span data-ttu-id="78983-158">Найдите следующий элемент привязки в файле *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="78983-158">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="78983-159">Замените указанный выше элемент на следующую разметку.</span><span class="sxs-lookup"><span data-stu-id="78983-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="78983-160">Указанный выше элемент привязки является [вспомогательной функцией тега](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="78983-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="78983-161">В данном случае он является [вспомогательной функцией тега привязки](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper).</span><span class="sxs-lookup"><span data-stu-id="78983-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper).</span></span> <span data-ttu-id="78983-162">Атрибут вспомогательной функции тега `asp-page="/Movies/Index"` и его значение создают ссылку на страницу Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="78983-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="78983-163">Сохраните изменения и протестируйте приложение, нажав на ссылку **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="78983-163">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="78983-164">См. файл [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) в GitHub.</span><span class="sxs-lookup"><span data-stu-id="78983-164">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-code-behind-page"></a><span data-ttu-id="78983-165">Страница кода программной части Create</span><span class="sxs-lookup"><span data-stu-id="78983-165">The Create code-behind page</span></span>

<span data-ttu-id="78983-166">Изучите файл кода программной части *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="78983-166">Examine the *Pages/Movies/Create.cshtml.cs* code-behind file:</span></span>

<span data-ttu-id="78983-167">[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml.cs?name=snippetALL)]</span><span class="sxs-lookup"><span data-stu-id="78983-167">[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml.cs?name=snippetALL)]</span></span>

<span data-ttu-id="78983-168">Метод `OnGet` инициализирует все состояния, необходимые для страницы.</span><span class="sxs-lookup"><span data-stu-id="78983-168">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="78983-169">Страница Create не содержит никаких состояний для инициализации.</span><span class="sxs-lookup"><span data-stu-id="78983-169">The Create page doesn't have any state to initialize.</span></span> <span data-ttu-id="78983-170">Метод `Page` создает объект `PageResult`, который формирует страницу *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="78983-170">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="78983-171">Для указания согласия на [привязку модели](xref:mvc/models/model-binding) в свойстве `Movie` используется атрибут `[BindProperty]`.</span><span class="sxs-lookup"><span data-stu-id="78983-171">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="78983-172">Когда форма Create публикует свои значения, среда выполнения ASP.NET Core связывает переданные значения с моделью `Movie`.</span><span class="sxs-lookup"><span data-stu-id="78983-172">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="78983-173">Метод `OnPostAsync` выполняется, когда страница публикует данные формы:</span><span class="sxs-lookup"><span data-stu-id="78983-173">The `OnPostAsync` method is run when the page posts form data:</span></span>

<span data-ttu-id="78983-174">[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml.cs?name=snippetPost)]</span><span class="sxs-lookup"><span data-stu-id="78983-174">[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml.cs?name=snippetPost)]</span></span>

<span data-ttu-id="78983-175">Если в модели есть ошибки, форма отображается снова вместе со всеми опубликованными данными этой формы.</span><span class="sxs-lookup"><span data-stu-id="78983-175">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="78983-176">Большинство ошибок в модели может быть перехвачено на стороне клиента до публикации формы.</span><span class="sxs-lookup"><span data-stu-id="78983-176">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="78983-177">Пример ошибки в модели — это публикация значения для поля даты, которое нельзя конвертировать в дату.</span><span class="sxs-lookup"><span data-stu-id="78983-177">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="78983-178">О проверке на стороне клиента и проверке модели мы поговорим подробнее далее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="78983-178">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="78983-179">Если ошибок в модели нет, данные сохраняются, а браузер переадресуется на страницу индексов.</span><span class="sxs-lookup"><span data-stu-id="78983-179">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="78983-180">Страница Razor Create</span><span class="sxs-lookup"><span data-stu-id="78983-180">The Create Razor Page</span></span>

<span data-ttu-id="78983-181">Изучите файл страницы Razor *Pages/Movies/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="78983-181">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

<span data-ttu-id="78983-182">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="78983-182">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml)]</span></span>

<span data-ttu-id="78983-183">Visual Studio выделяет тег `<form method="post">` отдельным шрифтом, который используется для вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="78983-183">Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers.</span></span> <span data-ttu-id="78983-184">Элемент `<form method="post">` представляет собой [вспомогательную функцию тега Form](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="78983-184">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="78983-185">Вспомогательная функция тега Form автоматически включает [маркер защиты от подделки](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="78983-185">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

![Представление страницы Create.cshtml в VS17](page/_static/th.png)

<span data-ttu-id="78983-187">Ядро формирования шаблонов создает разметку Razor для каждого поля в модели (кроме ID) следующего вида:</span><span class="sxs-lookup"><span data-stu-id="78983-187">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

<span data-ttu-id="78983-188">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml?range=15-20)]</span><span class="sxs-lookup"><span data-stu-id="78983-188">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml?range=15-20)]</span></span>

<span data-ttu-id="78983-189">[Вспомогательные функции тегов Validation](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` и ` <span asp-validation-for`) отображают ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="78983-189">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="78983-190">Более подробно проверка рассматривается далее в этой серии статей.</span><span class="sxs-lookup"><span data-stu-id="78983-190">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="78983-191">[Вспомогательная функция тега Label](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) создает подпись к метке и атрибут `for` для свойства `Title`.</span><span class="sxs-lookup"><span data-stu-id="78983-191">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="78983-192">[Вспомогательная функция тега Input](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) использует атрибуты [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) и создает HTML-атрибуты, необходимые для проверки jQuery на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="78983-192">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="78983-193">В следующем учебнике рассматривается SQL Server LocalDB и заполнение базы данных.</span><span class="sxs-lookup"><span data-stu-id="78983-193">The next tutorial explains SQL Server LocalDB and seeding the database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="78983-194">[Назад: Добавление модели](xref:tutorials/razor-pages/modelz)
[Далее: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="78983-194">[Previous: Adding a model](xref:tutorials/razor-pages/modelz)
[Next: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span></span>
