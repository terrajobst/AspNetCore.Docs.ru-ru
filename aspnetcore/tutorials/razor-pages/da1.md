---
title: Изменение созданных страниц в приложении ASP.NET Core
author: rick-anderson
description: Сведения об изменении созданных страниц в приложении ASP.NET Core.
ms.author: riande
ms.date: 12/20/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 19f54e04e748f5cf0469c2b5428800694d2c9344
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346110"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="61b49-103">Изменение созданных страниц в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61b49-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="61b49-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="61b49-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="61b49-105">Приложение для работы с фильмами подготовлено, но представление данных далеко от идеала.</span><span class="sxs-lookup"><span data-stu-id="61b49-105">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="61b49-106">Вместо **ReleaseDate** должно стоять **Дата выпуска** (два слова).</span><span class="sxs-lookup"><span data-stu-id="61b49-106">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="61b49-108">Обновление созданного кода</span><span class="sxs-lookup"><span data-stu-id="61b49-108">Update the generated code</span></span>

<span data-ttu-id="61b49-109">Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки кода:</span><span class="sxs-lookup"><span data-stu-id="61b49-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=12,17)]

<span data-ttu-id="61b49-110">Заметка к данным `[Column(TypeName = "decimal(18, 2)")]` позволяет Entity Framework Core корректно сопоставить `Price` с валютой в базе данных.</span><span class="sxs-lookup"><span data-stu-id="61b49-110">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="61b49-111">Дополнительные сведения см. в разделе [Типы данных](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="61b49-111">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="61b49-112">Пространство имен [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) будет рассмотрено в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="61b49-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="61b49-113">Атрибут [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) определяет отображаемое имя поля (в этом случае "Release Date" вместо "ReleaseDate").</span><span class="sxs-lookup"><span data-stu-id="61b49-113">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="61b49-114">Атрибут [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) определяет тип данных (Date), поэтому сведения о времени, хранящиеся в поле, не отображаются.</span><span class="sxs-lookup"><span data-stu-id="61b49-114">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="61b49-115">Перейдите к Pages/Movies и наведите указатель мыши на ссылку **Edit**, чтобы увидеть конечный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="61b49-115">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Окно браузера с указателем, наведенным на ссылку Edit (Изменить), и URL-адресом ссылки http://localhost:1234/Movies/Edit/5](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="61b49-117">Ссылки **Edit**, **Details** и **Delete** создаются [вспомогательной функцией тегов привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) в файле *Pages/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="61b49-117">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="61b49-118">[Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) позволяют серверному коду участвовать в создании и отображении HTML-элементов в файлах Razor.</span><span class="sxs-lookup"><span data-stu-id="61b49-118">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="61b49-119">В приведенном выше коде `AnchorTagHelper` динамически создает значение атрибута HTML `href` на основе страницы Razor (маршрут является относительным), атрибут `asp-page` и идентификатор маршрута (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="61b49-119">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="61b49-120">Дополнительные сведения см. в разделе [Формирование URL-адресов для страниц](xref:razor-pages/index#url-generation-for-pages).</span><span class="sxs-lookup"><span data-stu-id="61b49-120">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="61b49-121">Для изучения созданной разметки используйте функцию **просмотра исходного кода** в вашем любимом браузере.</span><span class="sxs-lookup"><span data-stu-id="61b49-121">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="61b49-122">Ниже показана часть созданного кода HTML:</span><span class="sxs-lookup"><span data-stu-id="61b49-122">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="61b49-123">В динамически созданных ссылках идентификаторы фильмов передаются с помощью строки запроса (например, `?id=1` в `https://localhost:5001/Movies/Details?id=1`).</span><span class="sxs-lookup"><span data-stu-id="61b49-123">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="61b49-124">Обновите страницы Razor Edit, Details и Delete так, чтобы использовался шаблон маршрута "{id:int}".</span><span class="sxs-lookup"><span data-stu-id="61b49-124">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="61b49-125">Измените директиву страницы для каждой из этих страниц c `@page` на `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="61b49-125">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="61b49-126">Запустите приложение и просмотрите исходный код.</span><span class="sxs-lookup"><span data-stu-id="61b49-126">Run the app and then view source.</span></span> <span data-ttu-id="61b49-127">Созданный код HTML добавляет идентификатор в путь URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="61b49-127">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="61b49-128">Запрос к странице с шаблоном маршрута "{id:int}", который **не** включает в себя целое число, приводит к ошибке HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="61b49-128">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="61b49-129">Например, `http://localhost:5000/Movies/Details` приведет к ошибке 404.</span><span class="sxs-lookup"><span data-stu-id="61b49-129">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="61b49-130">Чтобы сделать идентификатор необязательным, добавьте `?` к ограничению маршрута:</span><span class="sxs-lookup"><span data-stu-id="61b49-130">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="61b49-131">Чтобы проверить поведение `@page "{id:int?}"`:</span><span class="sxs-lookup"><span data-stu-id="61b49-131">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="61b49-132">Задайте директиву страницы *Pages/Movies/Details.cshtml* как `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="61b49-132">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="61b49-133">Установите точку останова `public async Task<IActionResult> OnGetAsync(int? id)` (в *Pages/Movies/Details.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="61b49-133">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="61b49-134">Перейдите к `https://localhost:5001/Movies/Details/`.</span><span class="sxs-lookup"><span data-stu-id="61b49-134">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="61b49-135">Из-за директивы `@page "{id:int}"` точка останова не достигается.</span><span class="sxs-lookup"><span data-stu-id="61b49-135">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="61b49-136">Механизм маршрутизации возвращает ошибку HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="61b49-136">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="61b49-137">При использовании `@page "{id:int?}"` метод `OnGetAsync` возвращает `NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="61b49-137">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

<span data-ttu-id="61b49-138">Хотя это не рекомендуется, можно написать метод `OnGetAsync` (в *Pages/Movies/Delete.cshtml.cs*) как:</span><span class="sxs-lookup"><span data-stu-id="61b49-138">Although not recommended, you could write the `OnGetAsync` method (in *Pages/Movies/Delete.cshtml.cs*) as:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Delete.cshtml.cs?name=snippet)]

<span data-ttu-id="61b49-139">Протестируйте приведенный выше код:</span><span class="sxs-lookup"><span data-stu-id="61b49-139">Test the preceding code:</span></span>

* <span data-ttu-id="61b49-140">Выберите ссылку **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="61b49-140">Select a **Delete** link.</span></span>
* <span data-ttu-id="61b49-141">Удалите код из URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="61b49-141">Remove the ID from the URL.</span></span> <span data-ttu-id="61b49-142">Например, измените `https://localhost:5001/Movies/Delete/8` на `https://localhost:5001/Movies/Delete`.</span><span class="sxs-lookup"><span data-stu-id="61b49-142">For example, change `https://localhost:5001/Movies/Delete/8` to `https://localhost:5001/Movies/Delete`.</span></span>
* <span data-ttu-id="61b49-143">Проведите пошаговое выполнение в отладчике.</span><span class="sxs-lookup"><span data-stu-id="61b49-143">Step through the code in the debugger.</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="61b49-144">Проверка обработки исключений нежесткой блокировки</span><span class="sxs-lookup"><span data-stu-id="61b49-144">Review concurrency exception handling</span></span>

<span data-ttu-id="61b49-145">Проверьте метод `OnPostAsync` в файле *Pages/Movies/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="61b49-145">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="61b49-146">Приведенный выше код обнаруживает исключения нежесткой блокировки, когда один клиент удаляет фильм, а другой публикует изменения для этого фильма.</span><span class="sxs-lookup"><span data-stu-id="61b49-146">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="61b49-147">Чтобы протестировать блок `catch`, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="61b49-147">To test the `catch` block:</span></span>

* <span data-ttu-id="61b49-148">Задайте точку останова в `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="61b49-148">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="61b49-149">Выберите команду **Изменить** у фильма, внесите изменения, но не вводите **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="61b49-149">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="61b49-150">В другом окне браузера щелкните ссылку **Delete** для этого же фильма, а затем удалите его.</span><span class="sxs-lookup"><span data-stu-id="61b49-150">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="61b49-151">В первом окне браузера опубликуйте изменения для фильма.</span><span class="sxs-lookup"><span data-stu-id="61b49-151">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="61b49-152">Коду в рабочей среде может потребоваться обнаружение конфликтов параллелизма.</span><span class="sxs-lookup"><span data-stu-id="61b49-152">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="61b49-153">Дополнительные сведения см. в статье [Обработка конфликтов параллелизма](xref:data/ef-rp/concurrency).</span><span class="sxs-lookup"><span data-stu-id="61b49-153">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="61b49-154">Проверка публикации и привязки</span><span class="sxs-lookup"><span data-stu-id="61b49-154">Posting and binding review</span></span>

<span data-ttu-id="61b49-155">Изучите файл *Pages/Movies/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="61b49-155">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

<span data-ttu-id="61b49-156">При выполнении HTTP-запроса GET к странице Movies/Edit (например, `http://localhost:5000/Movies/Edit/2`) происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="61b49-156">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="61b49-157">Метод `OnGetAsync` извлекает запись фильма из базы данных и возвращает метод `Page`.</span><span class="sxs-lookup"><span data-stu-id="61b49-157">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="61b49-158">Метод `Page` отображает страницу Razor *Pages/Movies/Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="61b49-158">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="61b49-159">Файл *Pages/Movies/Edit.cshtml* содержит директиву модели (`@model RazorPagesMovie.Pages.Movies.EditModel`), которая делает модель фильма доступной на странице.</span><span class="sxs-lookup"><span data-stu-id="61b49-159">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="61b49-160">Отображается форма Edit со значениями из записи фильма.</span><span class="sxs-lookup"><span data-stu-id="61b49-160">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="61b49-161">При публикации страницы Movies/Edit происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="61b49-161">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="61b49-162">Значения формы на странице привязываются к свойству `Movie`.</span><span class="sxs-lookup"><span data-stu-id="61b49-162">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="61b49-163">Атрибут `[BindProperty]` обеспечивает [привязку модели](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="61b49-163">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="61b49-164">При наличии ошибок в состоянии модели (например, `ReleaseDate` невозможно преобразовать в дату) форма отображается с предоставленными значениями.</span><span class="sxs-lookup"><span data-stu-id="61b49-164">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is displayed with the submitted values.</span></span>
* <span data-ttu-id="61b49-165">Если ошибки модели отсутствуют, данные фильма сохраняются.</span><span class="sxs-lookup"><span data-stu-id="61b49-165">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="61b49-166">Методы HTTP GET на страницах Razor Index, Create и Delete работают аналогично.</span><span class="sxs-lookup"><span data-stu-id="61b49-166">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="61b49-167">Метод HTTP POST `OnPostAsync` на странице Razor Create работает аналогично методу `OnPostAsync` на странице Razor Edit.</span><span class="sxs-lookup"><span data-stu-id="61b49-167">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="61b49-168">В следующем учебнике будет добавлена функция поиска.</span><span class="sxs-lookup"><span data-stu-id="61b49-168">Search is added in the next tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61b49-169">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="61b49-169">Additional resources</span></span>

* [<span data-ttu-id="61b49-170">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="61b49-170">YouTube version of this tutorial</span></span>](https://youtu.be/yLnnleREMtQ)

> [!div class="step-by-step"]
> <span data-ttu-id="61b49-171">[Предыдущая статья. Работа с базой данных](xref:tutorials/razor-pages/sql)
> [Следующая статья. Добавление поиска](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="61b49-171">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>
