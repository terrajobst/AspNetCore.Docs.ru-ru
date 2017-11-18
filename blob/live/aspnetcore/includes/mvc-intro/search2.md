<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="f98e0-101">Предыдущий метод `Index`:</span><span class="sxs-lookup"><span data-stu-id="f98e0-101">The previous `Index` method:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

<span data-ttu-id="f98e0-102">Обновленный метод `Index` с параметром `id`:</span><span class="sxs-lookup"><span data-stu-id="f98e0-102">The updated `Index` method with `id` parameter:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

<span data-ttu-id="f98e0-103">Теперь можно передать заголовок поиска в качестве данных маршрута (сегмент URL-адреса) вместо значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="f98e0-103">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![Представление Index, в URL-адрес которого добавлено слово ghost, возвращает два фильма: Ghostbusters и Ghostbusters 2](../../tutorials/first-mvc-app/search/_static/g2.png)

<span data-ttu-id="f98e0-105">Тем не менее пользователи вряд ли будут каждый раз изменять URL-адрес для поиска фильмов.</span><span class="sxs-lookup"><span data-stu-id="f98e0-105">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="f98e0-106">Итак, теперь вам необходимо добавить пользовательский интерфейс для удобства фильтрации фильмов.</span><span class="sxs-lookup"><span data-stu-id="f98e0-106">So now you'll add UI to help them filter movies.</span></span> <span data-ttu-id="f98e0-107">Если вы изменили сигнатуру метода `Index` для тестирования передачи параметра `ID` с привязкой к маршруту, измените ее снова, чтобы она снова принимала параметр `searchString`:</span><span class="sxs-lookup"><span data-stu-id="f98e0-107">If you changed the signature of the `Index` method to test how to pass the route-bound `ID` parameter, change it back so that it takes a parameter named `searchString`:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

<span data-ttu-id="f98e0-108">Откройте файл *Views/Movies/Index.cshtml* и добавьте разметку `<form>`, которая выделена ниже:</span><span class="sxs-lookup"><span data-stu-id="f98e0-108">Open the *Views/Movies/Index.cshtml* file, and add the `<form>` markup highlighted below:</span></span>

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

<span data-ttu-id="f98e0-109">Тег HTML `<form>` использует [вспомогательную функцию тега Form](../../mvc/views/working-with-forms.md), чтобы при отправке формы строка фильтра передавалась в действие `Index` контроллера movies.</span><span class="sxs-lookup"><span data-stu-id="f98e0-109">The HTML `<form>` tag uses the [Form Tag Helper](../../mvc/views/working-with-forms.md), so when you submit the form, the filter string is posted to the `Index` action of the movies controller.</span></span> <span data-ttu-id="f98e0-110">Сохраните изменения и протестируйте фильтр.</span><span class="sxs-lookup"><span data-stu-id="f98e0-110">Save your changes and then test the filter.</span></span>

![Представление Index со словом ghost в текстовом поле фильтра по названию](../../tutorials/first-mvc-app/search/_static/filter.png)

<span data-ttu-id="f98e0-112">Вопреки ожиданиям, перегрузка `[HttpPost]` для метода `Index` отсутствует.</span><span class="sxs-lookup"><span data-stu-id="f98e0-112">There's no `[HttpPost]` overload of the `Index` method as you might expect.</span></span> <span data-ttu-id="f98e0-113">Она не нужна, поскольку метод не изменяет состояние приложения и просто выполняет фильтрацию данных.</span><span class="sxs-lookup"><span data-stu-id="f98e0-113">You don't need it, because the method isn't changing the state of the app, just filtering data.</span></span>

<span data-ttu-id="f98e0-114">Можно добавить следующий метод `[HttpPost] Index`.</span><span class="sxs-lookup"><span data-stu-id="f98e0-114">You could add the following `[HttpPost] Index` method.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

<span data-ttu-id="f98e0-115">Параметр `notUsed` используется для создания перегрузки метода `Index`.</span><span class="sxs-lookup"><span data-stu-id="f98e0-115">The `notUsed` parameter is used to create an overload for the `Index` method.</span></span> <span data-ttu-id="f98e0-116">Это мы обсудим далее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="f98e0-116">We'll talk about that later in the tutorial.</span></span>

<span data-ttu-id="f98e0-117">При добавлении этого метода вызывающий метод действия будет сопоставлять метод `[HttpPost] Index`, а метод `[HttpPost] Index` будет выполняться, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="f98e0-117">If you add this method, the action invoker would match the `[HttpPost] Index` method, and the `[HttpPost] Index` method would run as shown in the image below.</span></span>

![Окно браузера с ответом приложения From HttpPost Index: фильтр по слову ghost](../../tutorials/first-mvc-app/search/_static/fo.png)

<span data-ttu-id="f98e0-119">Тем не менее при добавлении этой версии `[HttpPost]` метода `Index` существует ограничение на общую реализацию.</span><span class="sxs-lookup"><span data-stu-id="f98e0-119">However, even if you add this `[HttpPost]` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="f98e0-120">Допустим, вам необходимо добавить в закладки конкретный поиск или отправить друзьям ссылку, по которой они могут просмотреть аналогичный отфильтрованный список фильмов.</span><span class="sxs-lookup"><span data-stu-id="f98e0-120">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="f98e0-121">Обратите внимание, что URL-адрес запроса HTTP POST совпадает с URL-адресом запроса GET (localhost:xxxxx/Movies/Index) — в URL-адресе отсутствуют сведения о поиске.</span><span class="sxs-lookup"><span data-stu-id="f98e0-121">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL.</span></span> <span data-ttu-id="f98e0-122">Данные строки поиска отправляются на сервер в виде [значения поля формы](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span><span class="sxs-lookup"><span data-stu-id="f98e0-122">The search string information is sent to the server as a [form field value](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span></span> <span data-ttu-id="f98e0-123">Вы можете проверить это с помощью средств разработчика для браузера или [инструмента Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="f98e0-123">You can verify that with the browser Developer tools or the excellent [Fiddler tool](http://www.telerik.com/fiddler).</span></span> <span data-ttu-id="f98e0-124">На рисунке ниже показаны средства разработчика для браузера Chrome:</span><span class="sxs-lookup"><span data-stu-id="f98e0-124">The image below shows the Chrome browser Developer tools:</span></span>

![Вкладка "Сеть" средств разработчика в Microsoft Edge с телом запроса со значением searchString, равным ghost](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

<span data-ttu-id="f98e0-126">В теле запроса отображается параметр поиска и маркер [XSRF](../../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="f98e0-126">You can see the search parameter and [XSRF](../../security/anti-request-forgery.md) token in the request body.</span></span> <span data-ttu-id="f98e0-127">Обратите внимание, что, как описывается в предыдущем руководстве, [вспомогательная функция тега Form](../../mvc/views/working-with-forms.md) создает маркер защиты от подделки [XSRF](../../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="f98e0-127">Note, as mentioned in the previous tutorial, the [Form Tag Helper](../../mvc/views/working-with-forms.md) generates an [XSRF](../../security/anti-request-forgery.md) anti-forgery token.</span></span> <span data-ttu-id="f98e0-128">Поскольку мы не изменяем данные, проверять маркер безопасности в методе контроллера не нужно.</span><span class="sxs-lookup"><span data-stu-id="f98e0-128">We're not modifying data, so we don't need to validate the token in the controller method.</span></span>

<span data-ttu-id="f98e0-129">Так как параметр поиска находится в теле запроса, а не в URL-адресе, эти сведения о поиске нельзя добавить в закладки или открыть для общего доступа.</span><span class="sxs-lookup"><span data-stu-id="f98e0-129">Because the search parameter is in the request body and not the URL, you can't capture that search information to bookmark or share with others.</span></span> <span data-ttu-id="f98e0-130">Чтобы исправить это, необходимо указать запрос как `HTTP GET`.</span><span class="sxs-lookup"><span data-stu-id="f98e0-130">We'll fix this by specifying the request should be `HTTP GET`.</span></span>
