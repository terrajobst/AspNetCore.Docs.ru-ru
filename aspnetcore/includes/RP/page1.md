# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Сформированные страницы Razor Pages в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Этот учебник описывает страницы Razor Pages, созданные путем формирования шаблонов в предыдущем учебнике. 

[Просмотрите или скачайте](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) пример.

## <a name="the-create-delete-details-and-edit-pages"></a>Страницы Create, Delete, Details и Edit

Изучите страничную модель *Pages/Movies/Index.cshtml.cs*: [!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Страницы Razor Pages являются производными от `PageModel`. Как правило, класс, производный от `PageModel`, называется `<PageName>Model`. Используя [внедрение зависимостей](xref:fundamentals/dependency-injection), конструктор добавляет на страницу `MovieContext`. Этому шаблону соответствуют все сформированные страницы. Дополнительные сведения об асинхронном программировании с использованием Entity Framework см. в разделе [Асинхронный код](xref:data/ef-rp/intro#asynchronous-code).

Когда к странице направляется запрос, метод `OnGetAsync` возвращает на страницу Razor список фильмов. На странице Razor вызывается метод `OnGetAsync` или `OnGet`, инициализирующий состояние для страницы. В этом случае `OnGetAsync` возвращает список фильмов для отображения. 

Когда `OnGet` возвращает `void` или `OnGetAsync` возвращает `Task`, возвращаемый метод не используется. Если возвращаемый тип — `IActionResult` или `Task<IActionResult>`, необходимо предоставить оператор return. Например, метод `OnPostAsync` *Pages/Movies/Create.cshtml.cs*:

<!-- TODO - replace with snippet
[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
 -->

```csharp
public async Task<IActionResult> OnPostAsync()
{
    if (!ModelState.IsValid)
    {
        return Page();
    }

    _context.Movie.Add(Movie);
    await _context.SaveChangesAsync();

    return RedirectToPage("./Index");
}
```
Изучите страницу Razor *Pages/Movies/Index.cshtml*:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor может выполнять переход с HTML на C# или на разметку Razor. Если за символом `@` следует [зарезервированное ключевое слово Razor](xref:mvc/views/razor#razor-reserved-keywords), он переходит на разметку Razor, а если нет, то на C#.

Директива Razor `@page` преобразует файл в действие MVC &mdash;, а значит, он может обрабатывать запросы. Директива `@page` должна быть первой директивой Razor на странице. `@page` — это пример перехода на разметку Razor. Дополнительные сведения см. в статье [Синтаксис Razor](xref:mvc/views/razor#razor-syntax).

Проверьте лямбда-выражение, которое используется в следующем вспомогательном методе HTML.

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

Вспомогательный метод HTML `DisplayNameFor` проверяет свойство `Title`, указанное в лямбда-выражении, и определяет отображаемое имя. Лямбда-выражение проверяется, а не вычисляется. Это означает, что в случае, если `model`, `model.Movie` или `model.Movie[0]` имеют значение `null` или пусты, права доступа не нарушаются. При вычислении лямбда-выражения (например, с помощью `@Html.DisplayFor(modelItem => item.Title)`) вычисляются значения для свойств модели.

<a name="md"></a>
### <a name="the-model-directive"></a>директиву @model 

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

Директива `@model` определяет тип модели, передаваемой на страницу Razor. В приведенном выше примере строка `@model` делает класс, производный от `PageModel`, доступным для страниц Razor. Модель используется на странице во [вспомогательных методах HTML](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` и `@Html.DisplayName`.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData и макет

Рассмотрим следующий код.

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

Выделенный выше код представляет собой пример перехода Razor на C#. Символы `{` и `}` ограничивают блок кода C#.

Базовый класс `PageModel` содержит свойство словаря `ViewData`, позволяющее передать данные в представление. Объекты можно добавить в словарь `ViewData` с помощью шаблона "ключ/значение". В приведенном выше примере в словарь `ViewData` добавляется свойство "Title". Свойство "Title" используется в файле *Pages/_Layout.cshtml*. Ниже показаны первые несколько строк файла *Pages/_Layout.cshtml*.

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

Строка `@*Markup removed for brevity.*@` представляет собой комментарий Razor. В отличие от комментариев HTML (`<!-- -->`) комментарии Razor не отправляются клиенту.

Запустите приложение и проверьте ссылки в проекте (**Главная**, **О программе**, **Контакты**, **Создать**, **Изменить** и **Удалить**). Каждая страница задает заголовок, который отображается на вкладке браузера. При добавлении страницы в избранное заголовок используется в закладках. В настоящее время страницы *Pages/Index.cshtml* и *Pages/Movies/Index.cshtml* могут иметь один и тот же заголовок, но при желании их заголовки можно изменить.

Свойство `Layout` определяется в файле *Pages/_ViewStart.cshtml*:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

Представленный выше код задает файл разметки *Pages/_Layout.cshtml* для всех файлов Razor в папке *Pages*. Дополнительные сведения см. в статье о [макете](xref:mvc/razor-pages/index#layout).

### <a name="update-the-layout"></a>Обновление макета

Измените элемент `<title>` в файле *Pages/_Layout.cshtml*, чтобы использовать более короткую строку.

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

Найдите следующий элемент привязки в файле *Pages/_Layout.cshtml*.

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
Замените указанный выше элемент на следующую разметку.

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

Указанный выше элемент привязки является [вспомогательной функцией тега](xref:mvc/views/tag-helpers/intro). В данном случае он является [вспомогательной функцией тега привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). Атрибут вспомогательной функции тега `asp-page="/Movies/Index"` и его значение создают ссылку на страницу Razor `/Movies/Index`.

Сохраните изменения и протестируйте приложение, нажав на ссылку **RpMovie**. См. файл [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) в GitHub.

### <a name="the-create-code-behind-page"></a>Страница кода программной части Create

Изучите файл кода программной части *Pages/Movies/Create.cshtml.cs*:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

Метод `OnGet` инициализирует все состояния, необходимые для страницы. Страница Create не содержит никаких состояний для инициализации. Метод `Page` создает объект `PageResult`, который формирует страницу *Create.cshtml*.

Для указания согласия на [привязку модели](xref:mvc/models/model-binding) в свойстве `Movie` используется атрибут `[BindProperty]`. Когда форма Create публикует свои значения, среда выполнения ASP.NET Core связывает переданные значения с моделью `Movie`.

Метод `OnPostAsync` выполняется, когда страница публикует данные формы:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Если в модели есть ошибки, форма отображается снова вместе со всеми опубликованными данными этой формы. Большинство ошибок в модели может быть перехвачено на стороне клиента до публикации формы. Пример ошибки в модели — это публикация значения для поля даты, которое нельзя конвертировать в дату. О проверке на стороне клиента и проверке модели мы поговорим подробнее далее в этом учебнике.

Если ошибок в модели нет, данные сохраняются, а браузер переадресуется на страницу индексов.

### <a name="the-create-razor-page"></a>Страница Razor Create

Изучите файл страницы Razor *Pages/Movies/Create.cshtml*:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
