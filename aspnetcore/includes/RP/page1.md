# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="31555-101">Сформированные страницы Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31555-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="31555-102">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="31555-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="31555-103">Этот учебник описывает страницы Razor Pages, созданные путем формирования шаблонов в предыдущем учебнике.</span><span class="sxs-lookup"><span data-stu-id="31555-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="31555-104">[Просмотрите или скачайте](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) пример.</span><span class="sxs-lookup"><span data-stu-id="31555-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="31555-105">Страницы Create, Delete, Details и Edit</span><span class="sxs-lookup"><span data-stu-id="31555-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="31555-106">Изучите модель страницы *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="31555-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

<span data-ttu-id="31555-107">Страницы Razor Pages являются производными от `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="31555-107">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="31555-108">Как правило, класс, производный от `PageModel`, называется `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="31555-108">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="31555-109">Используя [внедрение зависимостей](xref:fundamentals/dependency-injection), конструктор добавляет на страницу `MovieContext`.</span><span class="sxs-lookup"><span data-stu-id="31555-109">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="31555-110">Этому шаблону соответствуют все сформированные страницы.</span><span class="sxs-lookup"><span data-stu-id="31555-110">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="31555-111">Дополнительные сведения об асинхронном программировании с использованием Entity Framework см. в разделе [Асинхронный код](xref:data/ef-rp/intro#asynchronous-code).</span><span class="sxs-lookup"><span data-stu-id="31555-111">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="31555-112">Когда к странице направляется запрос, метод `OnGetAsync` возвращает на страницу Razor список фильмов.</span><span class="sxs-lookup"><span data-stu-id="31555-112">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="31555-113">На странице Razor вызывается метод `OnGetAsync` или `OnGet`, инициализирующий состояние для страницы.</span><span class="sxs-lookup"><span data-stu-id="31555-113">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="31555-114">В этом случае `OnGetAsync` возвращает список фильмов для отображения.</span><span class="sxs-lookup"><span data-stu-id="31555-114">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="31555-115">Когда `OnGet` возвращает `void` или `OnGetAsync` возвращает `Task`, возвращаемый метод не используется.</span><span class="sxs-lookup"><span data-stu-id="31555-115">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="31555-116">Если возвращаемый тип — `IActionResult` или `Task<IActionResult>`, необходимо предоставить оператор return.</span><span class="sxs-lookup"><span data-stu-id="31555-116">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="31555-117">Например, метод `OnPostAsync` *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="31555-117">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

<span data-ttu-id="31555-118">Изучите страницу Razor *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="31555-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="31555-119">Razor может выполнять переход с HTML на C# или на разметку Razor.</span><span class="sxs-lookup"><span data-stu-id="31555-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="31555-120">Если за символом `@` следует [зарезервированное ключевое слово Razor](xref:mvc/views/razor#razor-reserved-keywords), он переходит на разметку Razor, а если нет, то на C#.</span><span class="sxs-lookup"><span data-stu-id="31555-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="31555-121">Директива Razor `@page` преобразует файл в действие MVC &mdash;, а значит, он может обрабатывать запросы.</span><span class="sxs-lookup"><span data-stu-id="31555-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="31555-122">Директива `@page` должна быть первой директивой Razor на странице.</span><span class="sxs-lookup"><span data-stu-id="31555-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="31555-123">`@page` — это пример перехода на разметку Razor.</span><span class="sxs-lookup"><span data-stu-id="31555-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="31555-124">Дополнительные сведения см. в статье [Синтаксис Razor](xref:mvc/views/razor#razor-syntax).</span><span class="sxs-lookup"><span data-stu-id="31555-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="31555-125">Проверьте лямбда-выражение, которое используется в следующем вспомогательном методе HTML.</span><span class="sxs-lookup"><span data-stu-id="31555-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="31555-126">Вспомогательный метод HTML `DisplayNameFor` проверяет свойство `Title`, указанное в лямбда-выражении, и определяет отображаемое имя.</span><span class="sxs-lookup"><span data-stu-id="31555-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="31555-127">Лямбда-выражение проверяется, а не вычисляется.</span><span class="sxs-lookup"><span data-stu-id="31555-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="31555-128">Это означает, что в случае, если `model`, `model.Movie` или `model.Movie[0]` имеют значение `null` или пусты, права доступа не нарушаются.</span><span class="sxs-lookup"><span data-stu-id="31555-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="31555-129">При вычислении лямбда-выражения (например, с помощью `@Html.DisplayFor(modelItem => item.Title)`) вычисляются значения для свойств модели.</span><span class="sxs-lookup"><span data-stu-id="31555-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="31555-130">директиву @model </span><span class="sxs-lookup"><span data-stu-id="31555-130">The @model directive</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="31555-131">Директива `@model` определяет тип модели, передаваемой на страницу Razor.</span><span class="sxs-lookup"><span data-stu-id="31555-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="31555-132">В приведенном выше примере строка `@model` делает класс, производный от `PageModel`, доступным для страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="31555-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="31555-133">Модель используется на странице во [вспомогательных методах HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` и `@Html.DisplayName`.</span><span class="sxs-lookup"><span data-stu-id="31555-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="31555-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData и макет</span><span class="sxs-lookup"><span data-stu-id="31555-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="31555-135">Рассмотрим следующий код.</span><span class="sxs-lookup"><span data-stu-id="31555-135">Consider the following code:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="31555-136">Выделенный выше код представляет собой пример перехода Razor на C#.</span><span class="sxs-lookup"><span data-stu-id="31555-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="31555-137">Символы `{` и `}` ограничивают блок кода C#.</span><span class="sxs-lookup"><span data-stu-id="31555-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="31555-138">Базовый класс `PageModel` содержит свойство словаря `ViewData`, позволяющее передать данные в представление.</span><span class="sxs-lookup"><span data-stu-id="31555-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="31555-139">Объекты можно добавить в словарь `ViewData` с помощью шаблона "ключ/значение".</span><span class="sxs-lookup"><span data-stu-id="31555-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="31555-140">В приведенном выше примере в словарь `ViewData` добавляется свойство "Title".</span><span class="sxs-lookup"><span data-stu-id="31555-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="31555-141">Свойство "Title" используется в файле *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="31555-141">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="31555-142">Ниже показаны первые несколько строк файла *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="31555-142">The following markup shows the first few lines of the *Pages/Shared/_Layout.cshtml* file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="31555-143">Свойство "Title" используется в файле *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="31555-143">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="31555-144">Ниже показаны первые несколько строк файла *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="31555-144">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="31555-145">Строка `@*Markup removed for brevity.*@` представляет собой комментарий Razor.</span><span class="sxs-lookup"><span data-stu-id="31555-145">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="31555-146">В отличие от комментариев HTML (`<!-- -->`) комментарии Razor не отправляются клиенту.</span><span class="sxs-lookup"><span data-stu-id="31555-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="31555-147">Запустите приложение и проверьте ссылки в проекте (**Главная**, **О программе**, **Контакты**, **Создать**, **Изменить** и **Удалить**).</span><span class="sxs-lookup"><span data-stu-id="31555-147">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="31555-148">Каждая страница задает заголовок, который отображается на вкладке браузера. При добавлении страницы в избранное заголовок используется в закладках.</span><span class="sxs-lookup"><span data-stu-id="31555-148">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="31555-149">В настоящее время страницы *Pages/Index.cshtml* и *Pages/Movies/Index.cshtml* могут иметь один и тот же заголовок, но при желании их заголовки можно изменить.</span><span class="sxs-lookup"><span data-stu-id="31555-149">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="31555-150">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="31555-150">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="31555-151">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (","), а для отображения данных в форматах для других языков, кроме английского, выполните действия, необходимые для глобализации вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="31555-151">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="31555-152">Инструкции по добавлению десятичной запятой представлены в этом [вопросе 4076 GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="31555-152">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="31555-153">Свойство `Layout` определяется в файле *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="31555-153">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="31555-154">Представленный выше код задает файл разметки *Pages/Shared/_Layout.cshtml* для всех файлов Razor в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="31555-154">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="31555-155">Дополнительные сведения см. в статье о [макете](xref:razor-pages/index#layout).</span><span class="sxs-lookup"><span data-stu-id="31555-155">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="31555-156">Обновление макета</span><span class="sxs-lookup"><span data-stu-id="31555-156">Update the layout</span></span>

<span data-ttu-id="31555-157">Измените элемент `<title>` в файле *Pages/Shared/_Layout.cshtml*, чтобы использовать более короткую строку.</span><span class="sxs-lookup"><span data-stu-id="31555-157">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="31555-158">Найдите следующий элемент привязки в файле *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="31555-158">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="31555-159">Замените указанный выше элемент на следующую разметку.</span><span class="sxs-lookup"><span data-stu-id="31555-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="31555-160">Указанный выше элемент привязки является [вспомогательной функцией тега](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="31555-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="31555-161">В данном случае он является [вспомогательной функцией тега привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="31555-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="31555-162">Атрибут вспомогательной функции тега `asp-page="/Movies/Index"` и его значение создают ссылку на страницу Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="31555-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="31555-163">Сохраните изменения и протестируйте приложение, нажав на ссылку **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="31555-163">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="31555-164">См. файл [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) в GitHub.</span><span class="sxs-lookup"><span data-stu-id="31555-164">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="31555-165">Страничная модель Create</span><span class="sxs-lookup"><span data-stu-id="31555-165">The Create page model</span></span>

<span data-ttu-id="31555-166">Изучите страничную модель *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="31555-166">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]
::: moniker-end


<span data-ttu-id="31555-167">Метод `OnGet` инициализирует все состояния, необходимые для страницы.</span><span class="sxs-lookup"><span data-stu-id="31555-167">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="31555-168">Страница Create не содержит никаких состояний для инициализации, поэтому возвращается `Page`.</span><span class="sxs-lookup"><span data-stu-id="31555-168">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="31555-169">Далее в этом руководстве вы увидите, как метод `OnGet` инициализирует состояние.</span><span class="sxs-lookup"><span data-stu-id="31555-169">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="31555-170">Метод `Page` создает объект `PageResult`, который формирует страницу *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="31555-170">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="31555-171">Для указания согласия на [привязку модели](xref:mvc/models/model-binding) в свойстве `Movie` используется атрибут `[BindProperty]`.</span><span class="sxs-lookup"><span data-stu-id="31555-171">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="31555-172">Когда форма Create публикует свои значения, среда выполнения ASP.NET Core связывает переданные значения с моделью `Movie`.</span><span class="sxs-lookup"><span data-stu-id="31555-172">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="31555-173">Метод `OnPostAsync` выполняется, когда страница публикует данные формы:</span><span class="sxs-lookup"><span data-stu-id="31555-173">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="31555-174">Если в модели есть ошибки, форма отображается снова вместе со всеми опубликованными данными этой формы.</span><span class="sxs-lookup"><span data-stu-id="31555-174">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="31555-175">Большинство ошибок в модели может быть перехвачено на стороне клиента до публикации формы.</span><span class="sxs-lookup"><span data-stu-id="31555-175">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="31555-176">Пример ошибки в модели — это публикация значения для поля даты, которое нельзя конвертировать в дату.</span><span class="sxs-lookup"><span data-stu-id="31555-176">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="31555-177">О проверке на стороне клиента и проверке модели мы поговорим подробнее далее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="31555-177">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="31555-178">Если ошибок в модели нет, данные сохраняются, а браузер переадресуется на страницу индексов.</span><span class="sxs-lookup"><span data-stu-id="31555-178">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="31555-179">Страница Razor Create</span><span class="sxs-lookup"><span data-stu-id="31555-179">The Create Razor Page</span></span>

<span data-ttu-id="31555-180">Изучите файл страницы Razor *Pages/Movies/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="31555-180">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
