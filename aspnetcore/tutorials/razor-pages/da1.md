---
title: "Изменение созданных страниц"
author: rick-anderson
description: "Обновление созданных страниц для оптимизации интерфейса."
keywords: ASP.NET Core, Razor Pages
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 8b2018bbf83cbb4b5c9139605fbb97d1c5be959f
ms.sourcegitcommit: f2fb0b45284e4f8c4a9c422bec790aede7c1f0ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2017
---
# <a name="updating-the-generated-pages"></a><span data-ttu-id="5dfb0-104">Изменение созданных страниц</span><span class="sxs-lookup"><span data-stu-id="5dfb0-104">Updating the generated pages</span></span>

<span data-ttu-id="5dfb0-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="5dfb0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5dfb0-106">Все готово для приложения Movie, но презентация далеко не идеальна.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="5dfb0-107">Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов: **Release Date**.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="5dfb0-109">Обновление созданного кода</span><span class="sxs-lookup"><span data-stu-id="5dfb0-109">Update the generated code</span></span>

<span data-ttu-id="5dfb0-110">Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки кода:</span><span class="sxs-lookup"><span data-stu-id="5dfb0-110">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

<span data-ttu-id="5dfb0-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]</span><span class="sxs-lookup"><span data-stu-id="5dfb0-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]</span></span>

<span data-ttu-id="5dfb0-112">Щелкните правой кнопкой мыши строку, подчеркнутую волнистой красной линией, и выберите пункт **Быстрые действия и рефакторинг**.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-112">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Контекстное меню с пунктом **Быстрые действия и рефакторинг**.](da1/qa.png)


<span data-ttu-id="5dfb0-114">Выберите `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-114">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations в верхней части списка.](da1/da.png)

  <span data-ttu-id="5dfb0-116">Visual Studio добавит `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-116">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="5dfb0-117">Пространство имен [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) будет рассмотрено в следующем учебнике.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-117">We'll cover [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) in the next tutorial.</span></span> <span data-ttu-id="5dfb0-118">Атрибут [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) определяет отображаемое имя поля (в этом случае "Release Date" вместо "ReleaseDate").</span><span class="sxs-lookup"><span data-stu-id="5dfb0-118">The [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="5dfb0-119">Атрибут [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) определяет тип данных (Date), поэтому сведения о времени, хранящиеся в поле, не отображаются.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-119">The [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribute specifies the type of the data (Date), so the time information stored in the field is not displayed.</span></span>

<span data-ttu-id="5dfb0-120">Перейдите к Pages/Movies и наведите указатель мыши на ссылку **Edit**, чтобы увидеть конечный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-120">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Окно браузера с указателем, наведенным на ссылку "Edit" (Изменить), и URL-адресом ссылки http://localhost:1234/Movies/Edit/5](da1/edit7.png)

<span data-ttu-id="5dfb0-122">Ссылки **Edit**, **Details** и **Delete** создаются [вспомогательной функцией тегов привязки](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) в файле *Pages/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-122">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) in the *Pages/Movies/Index.cshtml* file.</span></span>

<span data-ttu-id="5dfb0-123">[!code-cshtml[Main](razor-pages-start\snapshot_sample\RazorPagesMovie\Pages\Movie\Index.cshtml?highlight=16-18&range=32-)]</span><span class="sxs-lookup"><span data-stu-id="5dfb0-123">[!code-cshtml[Main](razor-pages-start\snapshot_sample\RazorPagesMovie\Pages\Movie\Index.cshtml?highlight=16-18&range=32-)]</span></span>

<span data-ttu-id="5dfb0-124">[Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) позволяют серверному коду участвовать в создании и отображении HTML-элементов в файлах Razor.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-124">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="5dfb0-125">В приведенном выше коде `AnchorTagHelper` динамически создает значение атрибута HTML `href` на основе страницы Razor (маршрут является относительным), атрибут `asp-page` и идентификатор маршрута (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="5dfb0-125">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="5dfb0-126">Дополнительные сведения см. в разделе [Формирование URL-адресов для страниц](xref:mvc/razor-pages/index#url-generation-for-pages).</span><span class="sxs-lookup"><span data-stu-id="5dfb0-126">See [URL generation for Pages](xref:mvc/razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="5dfb0-127">Для изучения созданной разметки используйте функцию **просмотра исходного кода** в вашем любимом браузере.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-127">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="5dfb0-128">Ниже показана часть созданного кода HTML:</span><span class="sxs-lookup"><span data-stu-id="5dfb0-128">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>

```

<span data-ttu-id="5dfb0-129">В динамически созданных ссылках идентификаторы фильмов передаются с помощью строки запроса (например, `http://localhost:5000/Movies/Details?id=2`).</span><span class="sxs-lookup"><span data-stu-id="5dfb0-129">The dynamically-generated links pass the movie ID with a query string (for example, `http://localhost:5000/Movies/Details?id=2` ).</span></span> 

<span data-ttu-id="5dfb0-130">Обновите страницы Razor Edit, Details и Delete так, чтобы использовался шаблон маршрута "{id:int}".</span><span class="sxs-lookup"><span data-stu-id="5dfb0-130">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="5dfb0-131">Измените директиву страницы для каждой из этих страниц на `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-131">Change the page directive for each of these pages to `@page "{id:int}"`.</span></span> <span data-ttu-id="5dfb0-132">Запустите приложение и просмотрите исходный код.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-132">Run the app and then view source.</span></span> <span data-ttu-id="5dfb0-133">Созданный код HTML добавляет идентификатор в путь URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="5dfb0-133">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="5dfb0-134">Запрос к странице с шаблоном маршрута "{id:int}", который **не** включает в себя целое число, приводит к ошибке HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="5dfb0-134">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="5dfb0-135">Например, `http://localhost:5000/Movies/Details` приведет к ошибке 404.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-135">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="5dfb0-136">Чтобы сделать идентификатор необязательным, добавьте `?` к ограничению маршрута:</span><span class="sxs-lookup"><span data-stu-id="5dfb0-136">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a><span data-ttu-id="5dfb0-137">Обновление обработки исключений нежесткой блокировки</span><span class="sxs-lookup"><span data-stu-id="5dfb0-137">Update concurrency exception handling</span></span>

<span data-ttu-id="5dfb0-138">Измените метод `OnPostAsync` в файле *Pages/Movies/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-138">Update the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file.</span></span> <span data-ttu-id="5dfb0-139">Эти изменения выделены в следующем примере кода:</span><span class="sxs-lookup"><span data-stu-id="5dfb0-139">The following highlighted code shows the changes:</span></span>

<span data-ttu-id="5dfb0-140">[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Edit.cshtml.cs?name=snippet1&highlight=17-24)]</span><span class="sxs-lookup"><span data-stu-id="5dfb0-140">[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Edit.cshtml.cs?name=snippet1&highlight=17-24)]</span></span>

<span data-ttu-id="5dfb0-141">Приведенный выше код обнаруживает исключения нежесткой блокировки, только когда один работающий параллельно клиент удаляет фильм, а другой публикует изменения для этого фильма.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-141">The previous code only detects concurrency exceptions when the first concurrent client deletes the movie, and the second concurrent client posts changes to the movie.</span></span>

<span data-ttu-id="5dfb0-142">Чтобы протестировать блок `catch`, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-142">To test the `catch` block:</span></span>

* <span data-ttu-id="5dfb0-143">Задайте точку останова в `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="5dfb0-143">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="5dfb0-144">Измените фильм.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-144">Edit a movie.</span></span>
* <span data-ttu-id="5dfb0-145">В другом окне браузера щелкните ссылку **Delete** для этого же фильма, а затем удалите его.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-145">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="5dfb0-146">В первом окне браузера опубликуйте изменения для фильма.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-146">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="5dfb0-147">В рабочем коде, как правило, обнаруживаются конфликты нежесткой блокировки, когда два или несколько клиентов одновременно изменяют запись.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-147">Production code would generally detect concurrency conflicts when two or more clients concurrently updated a record.</span></span> <span data-ttu-id="5dfb0-148">Дополнительные сведения см. в статье [Обработка конфликтов параллелизма](xref:data/ef-mvc/concurrency).</span><span class="sxs-lookup"><span data-stu-id="5dfb0-148">See [Handling concurrency conflicts](xref:data/ef-mvc/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="5dfb0-149">Проверка публикации и привязки</span><span class="sxs-lookup"><span data-stu-id="5dfb0-149">Posting and binding review</span></span>

<span data-ttu-id="5dfb0-150">Изучите файл *Pages/Movies/Edit.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Edit.cshtml.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="5dfb0-150">Examine the *Pages/Movies/Edit.cshtml.cs* file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Edit.cshtml.cs?name=snippet2)]</span></span>

<span data-ttu-id="5dfb0-151">При выполнении HTTP-запроса GET к странице Movies/Edit (например, `http://localhost:5000/Movies/Edit/2`) происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="5dfb0-151">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="5dfb0-152">Метод `OnGetAsync` извлекает запись фильма из базы данных и возвращает метод `Page`.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-152">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="5dfb0-153">Метод `Page` отображает страницу Razor *Pages/Movies/Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-153">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="5dfb0-154">Файл *Pages/Movies/Edit.cshtml* содержит директиву модели (`@model RazorPagesMovie.Pages.Movies.EditModel`), которая делает модель фильма доступной на странице.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-154">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the the movie model available on the page.</span></span>
* <span data-ttu-id="5dfb0-155">Отображается форма Edit со значениями из записи фильма.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-155">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="5dfb0-156">При публикации страницы Movies/Edit происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="5dfb0-156">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="5dfb0-157">Значения формы на странице привязываются к свойству `Movie`.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-157">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="5dfb0-158">Атрибут `[BindProperty]` обеспечивает [привязку модели](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="5dfb0-158">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

```csharp
[BindProperty]
public Movie Movie { get; set; }
```

* <span data-ttu-id="5dfb0-159">При наличии ошибок в состоянии модели (например, `ReleaseDate` невозможно преобразовать в дату) форма публикуется снова с предоставленными значениями.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-159">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is posted again with the submitted values.</span></span>
* <span data-ttu-id="5dfb0-160">Если ошибки модели отсутствуют, данные фильма сохраняются.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-160">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="5dfb0-161">Методы HTTP GET на страницах Razor Index, Create и Delete работают аналогично.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-161">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="5dfb0-162">Метод HTTP POST `OnPostAsync` на странице Razor Create работает аналогично методу `OnPostAsync` на странице Razor Edit.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-162">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="5dfb0-163">В следующем учебнике будет добавлена функция поиска.</span><span class="sxs-lookup"><span data-stu-id="5dfb0-163">Search is added in the next tutorial.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5dfb0-164">[Назад: работа с SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Добавление поиска](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="5dfb0-164">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
