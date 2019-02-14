---
uid: mvc/overview/getting-started/introduction/adding-search
title: Поиск | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/17/2019
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: ada125c917656f3a83524ff39e53b4cfc041a497
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248385"
---
<a name="search"></a><span data-ttu-id="5cbe7-102">Поиск</span><span class="sxs-lookup"><span data-stu-id="5cbe7-102">Search</span></span>
====================

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="5cbe7-103">Добавление метода поиска и поиска представления</span><span class="sxs-lookup"><span data-stu-id="5cbe7-103">Adding a Search Method and Search View</span></span>

<span data-ttu-id="5cbe7-104">В этом разделе вы добавите возможность поиска `Index` метод действия, который позволяет выполнять поиск фильмов по жанру или имени.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-104">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5cbe7-105">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5cbe7-105">Prerequisites</span></span>

<span data-ttu-id="5cbe7-106">Чтобы сопоставить снимки экрана в этом разделе, необходимо запустить приложение (F5) и добавьте следующие фильмов в базу данных.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-106">To match this section's screenshots, you need to run the application (F5) and add the following movies to the database.</span></span>

| <span data-ttu-id="5cbe7-107">Заголовок</span><span class="sxs-lookup"><span data-stu-id="5cbe7-107">Title</span></span> | <span data-ttu-id="5cbe7-108">Дата выпуска</span><span class="sxs-lookup"><span data-stu-id="5cbe7-108">Release Date</span></span> | <span data-ttu-id="5cbe7-109">Жанра</span><span class="sxs-lookup"><span data-stu-id="5cbe7-109">Genre</span></span> | <span data-ttu-id="5cbe7-110">Цена</span><span class="sxs-lookup"><span data-stu-id="5cbe7-110">Price</span></span> |
| ----- | ------------ | ----- | ----- |
| <span data-ttu-id="5cbe7-111">Ghostbusters</span><span class="sxs-lookup"><span data-stu-id="5cbe7-111">Ghostbusters</span></span> | <span data-ttu-id="5cbe7-112">6/8/1984</span><span class="sxs-lookup"><span data-stu-id="5cbe7-112">6/8/1984</span></span> | <span data-ttu-id="5cbe7-113">Комедия</span><span class="sxs-lookup"><span data-stu-id="5cbe7-113">Comedy</span></span> | <span data-ttu-id="5cbe7-114">6.99</span><span class="sxs-lookup"><span data-stu-id="5cbe7-114">6.99</span></span> |
| <span data-ttu-id="5cbe7-115">Ghostbusters II</span><span class="sxs-lookup"><span data-stu-id="5cbe7-115">Ghostbusters II</span></span> | <span data-ttu-id="5cbe7-116">6/16/1989</span><span class="sxs-lookup"><span data-stu-id="5cbe7-116">6/16/1989</span></span> | <span data-ttu-id="5cbe7-117">Комедия</span><span class="sxs-lookup"><span data-stu-id="5cbe7-117">Comedy</span></span> | <span data-ttu-id="5cbe7-118">6.99</span><span class="sxs-lookup"><span data-stu-id="5cbe7-118">6.99</span></span> |
| <span data-ttu-id="5cbe7-119">Глобально из высших Приматов</span><span class="sxs-lookup"><span data-stu-id="5cbe7-119">Planet of the Apes</span></span> | <span data-ttu-id="5cbe7-120">3/27/1986</span><span class="sxs-lookup"><span data-stu-id="5cbe7-120">3/27/1986</span></span> | <span data-ttu-id="5cbe7-121">Действие</span><span class="sxs-lookup"><span data-stu-id="5cbe7-121">Action</span></span> | <span data-ttu-id="5cbe7-122">5.99</span><span class="sxs-lookup"><span data-stu-id="5cbe7-122">5.99</span></span> |


## <a name="updating-the-index-form"></a><span data-ttu-id="5cbe7-123">Обновления формы индекса</span><span class="sxs-lookup"><span data-stu-id="5cbe7-123">Updating the Index Form</span></span>

<span data-ttu-id="5cbe7-124">Начните с обновления `Index` метода действия к существующему `MoviesController` класса.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-124">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="5cbe7-125">Ниже приведен код:</span><span class="sxs-lookup"><span data-stu-id="5cbe7-125">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="5cbe7-126">Первая часть `Index` метод создает следующие [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) запрос для выбора фильмов:</span><span class="sxs-lookup"><span data-stu-id="5cbe7-126">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="5cbe7-127">Запрос на этом этапе определяется, но еще не было выполнено в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-127">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="5cbe7-128">Если `searchString` содержит строку, запрос фильмов изменяется для фильтрации по значению в строке поиска, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="5cbe7-128">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="5cbe7-129">Приведенный выше код `s => s.Title` представляет собой [лямбда-выражение](https://msdn.microsoft.com/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="5cbe7-129">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="5cbe7-130">Лямбда-выражения используются в основе метод [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) запрашивает в качестве аргументов стандартных методов операторов запроса, такие как [где](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) метод, используемый в приведенном выше коде.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-130">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="5cbe7-131">Запросы LINQ не выполняются, когда они определяются или изменяются путем вызова метода, таких как `Where` или `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-131">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="5cbe7-132">Вместо этого выполнение запроса откладывается, это означает, что вычисление выражения откладывается, пока его реализованного значения будет выполнена итерация или [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) вызывается метод.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-132">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="5cbe7-133">В `Search` примере запрос выполняется в *Index.cshtml* представления.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-133">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="5cbe7-134">Дополнительные сведения об отложенном и немедленном выполнении запросов см. в разделе [Выполнение запроса](https://msdn.microsoft.com/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="5cbe7-134">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="5cbe7-135">[Contains](https://msdn.microsoft.com/library/bb155125.aspx) метод выполняется на базе данных, а не код c# выше.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-135">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="5cbe7-136">В базе данных [Contains](https://msdn.microsoft.com/library/bb155125.aspx) сопоставляется [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), который не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-136">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="5cbe7-137">Теперь вы можете обновить `Index` представление, которое будет отображать форму для пользователя.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-137">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="5cbe7-138">Запустите приложение и перейдите к */Movies/Index*.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-138">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="5cbe7-139">Добавьте в URL-адрес строку запроса, например `?searchString=ghost`.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-139">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="5cbe7-140">Отображаются отфильтрованные фильмы.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-140">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="5cbe7-142">Если вы изменили сигнатуру `Index` методу иметь параметр с именем `id`, `id` параметр будет соответствовать `{id}` заполнитель для значения по умолчанию направляет набора в *приложения\_функции RouteConfig.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-142">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="5cbe7-143">Исходный `Index` метод выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5cbe7-143">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="5cbe7-144">Измененный `Index` метода будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5cbe7-144">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="5cbe7-145">Теперь можно передать заголовок поиска в качестве данных маршрута (сегмент URL-адреса) вместо значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-145">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="5cbe7-146">Тем не менее пользователи вряд ли будут каждый раз изменять URL-адрес для поиска фильмов.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-146">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="5cbe7-147">Итак, теперь вам необходимо добавить пользовательский интерфейс для удобства фильтрации фильмов.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-147">So now you'll add UI to help them filter movies.</span></span> <span data-ttu-id="5cbe7-148">Если вы изменили сигнатуру `Index` метод для тестирования передачи параметра ID привязкой к маршруту, измените его, чтобы ваши `Index` метод принимает строковый параметр с именем `searchString`:</span><span class="sxs-lookup"><span data-stu-id="5cbe7-148">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="5cbe7-149">Откройте *Views\Movies\Index.cshtml* файла и сразу же после `@Html.ActionLink("Create New", "Create")`, добавьте разметку формы, описанных ниже:</span><span class="sxs-lookup"><span data-stu-id="5cbe7-149">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="5cbe7-150">`Html.BeginForm` Вспомогательный создает открывающий `<form>` тега.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-150">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="5cbe7-151">`Html.BeginForm` Вспомогательный вызывает формы post к самому себе, когда пользователь отправляет форму, щелкнув **фильтра** кнопки.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-151">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="5cbe7-152">Visual Studio 2013 имеет значительное усовершенствование при отображения и редактирования файлов представлений.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-152">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="5cbe7-153">При запуске приложения с помощью файла представления откройте Visual Studio 2013 вызывает метод действия правильный контроллера для отображения представления.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-153">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="5cbe7-154">Представление Index открыт в Visual Studio (как показано на приведенном выше рисунке), коснитесь Ctr F5 или F5, чтобы запустить приложение и повторите поиск фильма.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-154">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="5cbe7-155">Существует не `HttpPost` перегрузки `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-155">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="5cbe7-156">Это не требуется, поскольку метод не изменяет состояние приложения, просто выполняет фильтрацию данных.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-156">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="5cbe7-157">Можно добавить следующий метод `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-157">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="5cbe7-158">В этом случае средство вызова действий будет соответствовать `HttpPost Index` метод и `HttpPost Index` метод будет выполняться, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-158">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="5cbe7-160">Тем не менее при добавлении этой версии `HttpPost` метода `Index` существует ограничение на общую реализацию.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-160">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="5cbe7-161">Допустим, вам необходимо добавить в закладки конкретный поиск или отправить друзьям ссылку, по которой они могут просмотреть аналогичный отфильтрованный список фильмов.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-161">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="5cbe7-162">Обратите внимание на то, что URL-адрес запроса HTTP POST совпадает со значением URL-адрес для запроса GET (localhost: xxxxx/Movies/Index) — нет поиска информации в сам URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-162">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="5cbe7-163">Справа, данные строки поиска отправляется на сервер как значения поля формы.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-163">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="5cbe7-164">Это означает, что вы не можете передавать эту информацию поиска закладки или отправить друзьям в URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-164">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="5cbe7-165">Рекомендуется использовать перегрузку `BeginForm` , указывающий, что запрос POST должен добавлять сведения о поиске URL-адрес и что оно должно быть направлено `HttpGet` версии `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-165">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="5cbe7-166">Замените существующий без параметров `BeginForm` метод, используя следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="5cbe7-166">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="5cbe7-168">Теперь при отправки поиска URL-адрес содержит строку запроса поиска.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-168">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="5cbe7-169">Поиск также переносится в метод `HttpGet Index`, даже если у вас определен метод `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-169">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="5cbe7-171">Добавление поиска по жанру</span><span class="sxs-lookup"><span data-stu-id="5cbe7-171">Adding Search by Genre</span></span>

<span data-ttu-id="5cbe7-172">Если вы добавили `HttpPost` версии `Index` метод, удалить его сейчас.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-172">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="5cbe7-173">Далее добавим функцию, которая позволит пользователям поиск фильмов по жанру.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-173">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="5cbe7-174">Замените метод `Index` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5cbe7-174">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="5cbe7-175">Эта версия `Index` метод принимает дополнительный параметр, а именно `movieGenre`.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-175">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="5cbe7-176">Создать первые несколько строк кода `List` объекта, содержащего жанров фильма из базы данных.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-176">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="5cbe7-177">Следующий код определяет запрос LINQ, который извлекает все жанры из базы данных.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-177">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="5cbe7-178">Код использует `AddRange` метод универсального `List` коллекции для добавления в список всех отдельных жанров.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-178">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="5cbe7-179">(Без `Distinct` модификатор, будут добавлены повторяющиеся жанры — например, комедия будут добавлены в нашем примере дважды).</span><span class="sxs-lookup"><span data-stu-id="5cbe7-179">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="5cbe7-180">Код сохраняет список жанров в `ViewBag.MovieGenre` объекта.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-180">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="5cbe7-181">Хранение данных категории (такие фильма жанры) как [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) объекта в `ViewBag`, то доступ к данным категорию в раскрывающемся списке является типичным подходом для приложений MVC.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-181">Storing category data (such a movie genres) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="5cbe7-182">Следующий код показывает, как проверить `movieGenre` параметра.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-182">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="5cbe7-183">Если значение не пустое, код ограничивает запрос для ограничения выбранный фильм, чтобы указанный жанр фильмов.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-183">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="5cbe7-184">Как уже говорилось ранее, запрос не выполняется в базе данных, пока выполняется итерация списка фильмов (что происходит в представлении после `Index` метод действия возвращает).</span><span class="sxs-lookup"><span data-stu-id="5cbe7-184">As stated previously, the query is not run on the database until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="5cbe7-185">Добавив разметку в представление Index для поддержки поиска по жанру</span><span class="sxs-lookup"><span data-stu-id="5cbe7-185">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="5cbe7-186">Добавить `Html.DropDownList` вспомогательный метод для *Views\Movies\Index.cshtml* файла, непосредственно перед `TextBox` вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-186">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="5cbe7-187">Ниже приводится полная разметка.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-187">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="5cbe7-188">В следующем коде:</span><span class="sxs-lookup"><span data-stu-id="5cbe7-188">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="5cbe7-189">Параметр «MovieGenre» предоставляет ключ для `DropDownList` вспомогательный метод для поиска `IEnumerable<SelectListItem>` в `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-189">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="5cbe7-190">`ViewBag` Была заполнена в методе действия:</span><span class="sxs-lookup"><span data-stu-id="5cbe7-190">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="5cbe7-191">Параметр «All» предоставляет метку варианта.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-191">The parameter "All" provides an option label.</span></span> <span data-ttu-id="5cbe7-192">Если проверить этот выбор в браузере, вы увидите, что его атрибут «value» пуста.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-192">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="5cbe7-193">Поскольку нашего контроллера только фильтрует `if` строка не `null` или empty, отправка пустое значение для `movieGenre` показывает все жанров.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-193">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="5cbe7-194">Можно также задать параметр выбран по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-194">You can also set an option to be selected by default.</span></span> <span data-ttu-id="5cbe7-195">Если требуется «Комедия» как параметры по умолчанию, необходимо изменить код в контроллере следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5cbe7-195">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="5cbe7-196">Запустите приложение и перейдите к */Movies/Index*.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-196">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="5cbe7-197">Попробуйте выполнить поиск по жанру, название фильма и по обоим критериям.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-197">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="5cbe7-198">В этом разделе вы создали метод действия поиска и представления, которые позволяют пользователям выполнять поиск по названию фильма и жанр.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-198">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="5cbe7-199">В следующем разделе будут рассмотрены способы добавьте свойство, чтобы `Movie` модель и как добавить инициализатор, который автоматически создает тестовую базу данных.</span><span class="sxs-lookup"><span data-stu-id="5cbe7-199">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5cbe7-200">[Назад](examining-the-edit-methods-and-edit-view.md)
> [Вперед](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="5cbe7-200">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
