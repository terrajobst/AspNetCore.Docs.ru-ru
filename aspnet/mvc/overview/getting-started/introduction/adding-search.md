---
uid: mvc/overview/getting-started/introduction/adding-search
title: "Поиск | Документы Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 10457d154f5fda875f7d1054d48daeeba3a50b7c
ms.sourcegitcommit: 2b263e87217658caa42eedc4f9d2d21ef0ab5d59
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2018
---
<a name="search"></a><span data-ttu-id="8e8dc-102">Поиск</span><span class="sxs-lookup"><span data-stu-id="8e8dc-102">Search</span></span>
====================
<span data-ttu-id="8e8dc-103">По [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8e8dc-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="8e8dc-104">Добавление метода поиска и просмотра поиска</span><span class="sxs-lookup"><span data-stu-id="8e8dc-104">Adding a Search Method and Search View</span></span>

<span data-ttu-id="8e8dc-105">В этом разделе вы добавите возможностей поиска в `Index` метод действия, который позволяет искать фильмов по жанру или имени.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-105">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="updating-the-index-form"></a><span data-ttu-id="8e8dc-106">Обновление индекса формы</span><span class="sxs-lookup"><span data-stu-id="8e8dc-106">Updating the Index Form</span></span>

<span data-ttu-id="8e8dc-107">Начните с обновления `Index` метода действия для существующего `MoviesController` класса.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-107">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="8e8dc-108">Ниже приведен код:</span><span class="sxs-lookup"><span data-stu-id="8e8dc-108">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="8e8dc-109">Первая строка `Index` метод создает следующие [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) запрос, чтобы выбрать фильмы:</span><span class="sxs-lookup"><span data-stu-id="8e8dc-109">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="8e8dc-110">Запрос на этом этапе определяется, но еще не было запущено в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-110">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="8e8dc-111">Если `searchString` параметр содержит строку, запрос фильмы изменяется для фильтрации по значению строки поиска, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="8e8dc-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="8e8dc-112">Приведенный выше код `s => s.Title` представляет собой [лямбда-выражение](https://msdn.microsoft.com/en-us/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="8e8dc-112">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/en-us/library/bb397687.aspx).</span></span> <span data-ttu-id="8e8dc-113">Лямбда-выражения, используемые в основе метода [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) запрашивает качестве аргументов стандартных методов операторов запроса, таких как [где](https://msdn.microsoft.com/en-us/library/system.linq.enumerable.where.aspx) метод, используемый в приведенном выше коде.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-113">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/en-us/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="8e8dc-114">LINQ запросы не выполняются при их определении либо изменении путем вызова метода, например `Where` или `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-114">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="8e8dc-115">Вместо этого выполнение запроса отложено, это означает, что вычисление выражения откладывается до его реализованных значение фактически итерации или [ `ToList` ](https://msdn.microsoft.com/en-us/library/bb342261.aspx) вызывается метод.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-115">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/en-us/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="8e8dc-116">В `Search` образца, запрос выполняется в *Index.cshtml* представления.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-116">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="8e8dc-117">Дополнительные сведения об отложенном и немедленном выполнении запросов см. в разделе [Выполнение запроса](https://msdn.microsoft.com/en-us/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="8e8dc-117">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/en-us/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="8e8dc-118">[Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) метод выполняется в базе данных, а не в коде c# выше.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-118">The [Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="8e8dc-119">В базе данных [Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) сопоставляется [SQL LIKE](https://msdn.microsoft.com/en-us/library/ms179859.aspx), которая не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-119">On the database, [Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/en-us/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="8e8dc-120">Теперь вы можете обновить `Index` представление, которое будет отображать формы для пользователя.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-120">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="8e8dc-121">Запустите приложение и перейдите к */фильмов/индекса*.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-121">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="8e8dc-122">Добавьте в URL-адрес строку запроса, например `?searchString=ghost`.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-122">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="8e8dc-123">Отображаются отфильтрованные фильмы.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-123">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="8e8dc-125">Если изменить подпись `Index` методу иметь параметр с именем `id`, `id` параметр будет соответствовать `{id}` заполнитель для значения по умолчанию направляет набор в *приложения\_сценарий RouteConfig.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-125">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="8e8dc-126">Исходный `Index` метод выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="8e8dc-126">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="8e8dc-127">Измененный `Index` метода будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="8e8dc-127">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="8e8dc-128">Теперь можно передать заголовок поиска в качестве данных маршрута (сегмент URL-адреса) вместо значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="8e8dc-129">Тем не менее пользователи вряд ли будут каждый раз изменять URL-адрес для поиска фильмов.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-129">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="8e8dc-130">Поэтому теперь вы добавите пользовательского интерфейса для их фильтрации фильмов.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-130">So now you you'll add UI to help them filter movies.</span></span> <span data-ttu-id="8e8dc-131">Если изменить подпись `Index` метод для проверки как передавать параметр ID привязкой маршрута, изменить его, чтобы вашей `Index` метод принимает строковый параметр с именем `searchString`:</span><span class="sxs-lookup"><span data-stu-id="8e8dc-131">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="8e8dc-132">Откройте *Views\Movies\Index.cshtml* файла и сразу же после `@Html.ActionLink("Create New", "Create")`, добавьте разметку формы, выделены ниже:</span><span class="sxs-lookup"><span data-stu-id="8e8dc-132">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="8e8dc-133">`Html.BeginForm` Вспомогательный создает открывающий `<form>` тег.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-133">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="8e8dc-134">`Html.BeginForm` Вспомогательный вызывает формы post на самого себя, когда пользователь отправляет форму, нажав кнопку **фильтра** кнопки.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-134">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="8e8dc-135">Visual Studio 2013 имеет значительное улучшение работы с низким приоритетом при отображении и изменении Просмотр файлов.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-135">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="8e8dc-136">При запуске приложения с помощью файла представления open, Visual Studio 2013 вызывает метод действия контроллера правильный представление.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-136">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="8e8dc-137">Представление Index открыт в Visual Studio (как показано на приведенном выше рисунке), коснитесь Ctr F5 или F5, чтобы запустить приложение и повторите поиск фильма.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-137">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="8e8dc-138">Имеется не `HttpPost` перегрузки `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-138">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="8e8dc-139">Это не требуется, поскольку метод не изменяет состояние приложения, просто фильтрации данных.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-139">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="8e8dc-140">Можно добавить следующий метод `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-140">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="8e8dc-141">В этом случае средство вызова действий будет соответствовать `HttpPost Index` метода и `HttpPost Index` метод будет выполняться, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-141">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="8e8dc-143">Тем не менее при добавлении этой версии `HttpPost` метода `Index` существует ограничение на общую реализацию.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-143">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="8e8dc-144">Допустим, вам необходимо добавить в закладки конкретный поиск или отправить друзьям ссылку, по которой они могут просмотреть аналогичный отфильтрованный список фильмов.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-144">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="8e8dc-145">Обратите внимание, что URL-адрес для запроса HTTP POST совпадает с URL-адрес для запроса на получение (localhost:xxxxx/фильмов/индекса) — нет поиска сведений в самом URL-АДРЕСЕ.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-145">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="8e8dc-146">Справа теперь данные строки поиска отправляется на сервер как значения поля формы.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-146">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="8e8dc-147">Это означает, что вы не можете получить эту информацию поиска для закладки или отправить друзьям в URL-АДРЕСЕ.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-147">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="8e8dc-148">Рекомендуется использовать перегрузку `BeginForm` , указывающий, что запрос POST следует добавить поиск сведений о URL-адрес и должно быть направлено в `HttpGet` версии `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-148">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="8e8dc-149">Заменить существующий без параметров `BeginForm` метод следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="8e8dc-149">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="8e8dc-151">Теперь при отправке поиска, URL-адрес содержит строку запроса поиска.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-151">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="8e8dc-152">Поиск также переносится в метод `HttpGet Index`, даже если у вас определен метод `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-152">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="8e8dc-154">Добавление поиска по жанру</span><span class="sxs-lookup"><span data-stu-id="8e8dc-154">Adding Search by Genre</span></span>

<span data-ttu-id="8e8dc-155">Если вы добавили `HttpPost` версии `Index` метод, сейчас ее удалить.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-155">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="8e8dc-156">Далее можно добавить функцию, чтобы позволить пользователям поиск фильмов по жанру.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-156">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="8e8dc-157">Замените метод `Index` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="8e8dc-157">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="8e8dc-158">Эта версия `Index` метод принимает дополнительный параметр, а именно `movieGenre`.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-158">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="8e8dc-159">Первые несколько строк кода, создание `List` объекта, содержащего жанров фильмов из базы данных.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-159">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="8e8dc-160">Следующий код определяет запрос LINQ, который извлекает все жанры из базы данных.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-160">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="8e8dc-161">Код использует `AddRange` метод универсального `List` коллекции для добавления в список всех уникальных жанров.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-161">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="8e8dc-162">(Без `Distinct` модификатор, следует добавить повторяющийся жанров — например, комедия добавляется дважды в нашем примере).</span><span class="sxs-lookup"><span data-stu-id="8e8dc-162">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="8e8dc-163">Код сохраняет список жанров в `ViewBag.MovieGenre` объекта.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-163">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="8e8dc-164">Хранение данных категории (такие фильма жанра) как [SelectList](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlist(v=vs.108).aspx) объекта в `ViewBag`, то доступ к данным категории в раскрывающемся списке — типичный подход для приложений MVC.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-164">Storing category data (such a movie genre's) as a [SelectList](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="8e8dc-165">В следующем коде показано, как проверить `movieGenre` параметра.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-165">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="8e8dc-166">Если она не пустая, код далее ограничивает запрос фильмов с целью ограничения фильмы, выбранных для заданного жанра.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-166">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="8e8dc-167">Как уже говорилось ранее, запрос выполняется не на базе данных, пока циклах список фильмов (в представлении, которое производится после `Index` возвращает метод действия).</span><span class="sxs-lookup"><span data-stu-id="8e8dc-167">As stated previously, the query is not run on the data base until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="8e8dc-168">Добавление разметки в представлении индекса для поддержки поиска по жанру</span><span class="sxs-lookup"><span data-stu-id="8e8dc-168">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="8e8dc-169">Добавить `Html.DropDownList` вспомогательный метод для *Views\Movies\Index.cshtml* файла, непосредственно перед `TextBox` вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-169">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="8e8dc-170">Ниже приводится полная разметка.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-170">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="8e8dc-171">В следующем коде:</span><span class="sxs-lookup"><span data-stu-id="8e8dc-171">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="8e8dc-172">Параметр «MovieGenre» предоставляет ключ для `DropDownList` вспомогательный метод для поиска `IEnumerable<SelectListItem>` в `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-172">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="8e8dc-173">`ViewBag` Была заполнена в методе действия:</span><span class="sxs-lookup"><span data-stu-id="8e8dc-173">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="8e8dc-174">Параметр «All» предоставляет метку варианта.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-174">The parameter "All" provides an option label.</span></span> <span data-ttu-id="8e8dc-175">Если проверить выбор в браузере, вы увидите, что его атрибут «value» является пустым.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-175">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="8e8dc-176">Поскольку наш контроллера только фильтрует `if` строка не `null` или является пустым, отправка пустое значение для `movieGenre` показывает все жанров.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-176">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="8e8dc-177">Можно также установить параметр выбран по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-177">You can also set an option to be selected by default.</span></span> <span data-ttu-id="8e8dc-178">Если требуется «Комедия» в качестве дополнения по умолчанию, необходимо изменить код в контроллере следующим образом:</span><span class="sxs-lookup"><span data-stu-id="8e8dc-178">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="8e8dc-179">Запустите приложение и перейдите к */фильмов/индекса*.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-179">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="8e8dc-180">Попробуйте выполните поиск по жанру, названия фильма и обоим критериям.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-180">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="8e8dc-181">В этом разделе вы создали поиск метода действия и представления, которые позволяют пользователям выполнять поиск по название фильма и жанр.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-181">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="8e8dc-182">В следующем разделе будут рассмотрены способы Добавление свойства `Movie` модели и как добавить инициализатор, который автоматически создает тестовую базу данных.</span><span class="sxs-lookup"><span data-stu-id="8e8dc-182">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8e8dc-183">[Назад](examining-the-edit-methods-and-edit-view.md)
[Вперед](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="8e8dc-183">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
