
## <a name="test-the-app"></a><span data-ttu-id="d2fa1-101">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="d2fa1-101">Test the app</span></span>

* <span data-ttu-id="d2fa1-102">Запустите приложение и коснитесь ссылки **Фильм Mvc**.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-102">Run the app and tap the **Mvc Movie** link.</span></span>
* <span data-ttu-id="d2fa1-103">Коснитесь ссылки **Создать** и создайте фильм.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-103">Tap the **Create New** link and create a movie.</span></span>

  ![Создание представления с полями для жанра, цены, дата выхода и названия](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* <span data-ttu-id="d2fa1-105">В поле `Price` нельзя вводить десятичные точки или запятые.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-105">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="d2fa1-106">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (","), а для отображения данных в форматах для других языков, кроме английского, выполните действия, необходимые для глобализации вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-106">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="d2fa1-107">Дополнительные сведения см. на странице https://github.com/aspnet/Docs/issues/4076 и в разделе [Дополнительные ресурсы](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="d2fa1-107">See https://github.com/aspnet/Docs/issues/4076 and [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="d2fa1-108">А пока вводите целые числа, такие как 10.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-108">For now, just enter whole numbers like 10.</span></span>

<a name="displayformatdatelocal"></a>

* <span data-ttu-id="d2fa1-109">В некоторых регионах необходимо указывать формат даты.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-109">In some locales you need to specify the date format.</span></span> <span data-ttu-id="d2fa1-110">См. выделенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-110">See the highlighted code below.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

<span data-ttu-id="d2fa1-111">`DataAnnotations` мы обсудим далее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-111">We'll talk about `DataAnnotations` later in the tutorial.</span></span>

<span data-ttu-id="d2fa1-112">При выборе ссылки **Создать** форма отправляется на сервер, где сведения о фильме сохраняются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-112">Tapping **Create** causes the form to be posted to the server, where the movie information is saved in a database.</span></span> <span data-ttu-id="d2fa1-113">Приложение перенаправляется на URL-адрес */Movies*, где отображаются сведения о только что созданном фильме.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-113">The app redirects to the */Movies* URL, where the newly created movie information is displayed.</span></span>

![Представление фильмов с указанием вновь созданного фильма](../../tutorials/first-mvc-app/adding-model/_static/h.png)

<span data-ttu-id="d2fa1-115">Создайте еще несколько записей фильмов.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-115">Create a couple more movie entries.</span></span> <span data-ttu-id="d2fa1-116">Попробуйте воспользоваться ссылками **Изменить**, **Сведения** и **Удалить** — все они работают.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-116">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>
