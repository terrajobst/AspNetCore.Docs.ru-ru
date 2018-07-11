---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Доступ к данным модели из контроллера | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: a3f3f4a030650ff65b070528c5efa1605be764a0
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38120157"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="d2bbb-102">Доступ к данным модели из контроллера</span><span class="sxs-lookup"><span data-stu-id="d2bbb-102">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="d2bbb-103">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d2bbb-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="d2bbb-104">В этом разделе вы создадите новый `MoviesController` класса и написать код, который извлекает данные фильма и отображает его в браузере с помощью шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="d2bbb-105">**Постройте приложение** перед переходом к следующему шагу.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="d2bbb-106">Если вы не создадите приложение, вы получите ошибку при добавлении контроллера.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="d2bbb-107">В обозревателе решений щелкните правой кнопкой мыши *контроллеров* папку и нажмите кнопку **добавить**, затем **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="d2bbb-108">В **Добавление шаблона** диалоговом окне щелкните **контроллер MVC 5 с представлениями, использующий Entity Framework**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="d2bbb-109">Выберите **Movie (MvcMovie.Models)** класс модели.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="d2bbb-110">Выберите **MovieDBContext (MvcMovie.Models)** для класса контекста данных.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="d2bbb-111">Имя контроллера введите **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="d2bbb-112">На следующем рисунке показано завершенное диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="d2bbb-113">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-113">Click **Add**.</span></span> <span data-ttu-id="d2bbb-114">(Если отобразится сообщение об ошибке, возможно, не была построена приложения перед началом добавления контроллера.) Visual Studio создает следующие файлы и папки:</span><span class="sxs-lookup"><span data-stu-id="d2bbb-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="d2bbb-115">*MoviesController.cs* файл *контроллеров* папки.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="d2bbb-116">Объект *Views\Movies* папки.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="d2bbb-117">*CREATE.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, и *Index.cshtml* в новом *Views\Movies* папки.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="d2bbb-118">Visual Studio автоматически создается [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (Создание, чтение, обновление и удаление) методы действий и представления для вас (автоматическое создание действия CRUD-методы и представления называется формированием шаблонов).</span><span class="sxs-lookup"><span data-stu-id="d2bbb-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="d2bbb-119">Теперь у вас есть полнофункциональное веб-приложение, которое служит для создания, перечисления, редактирования и удаления записей фильмов.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="d2bbb-120">Запустите приложение и щелкнуть **MVC Movie** ссылку (или перейдите к `Movies` контроллер, добавляя */Movies* на URL-адрес в адресной строке браузера).</span><span class="sxs-lookup"><span data-stu-id="d2bbb-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="d2bbb-121">Так как приложение полагается на маршрутизацию по умолчанию (определенный в *приложения\_Start\RouteConfig.cs* файл), запрос браузера `http://localhost:xxxxx/Movies` направляется по умолчанию `Index` метод действия `Movies` контроллера.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="d2bbb-122">Другими словами, запрос браузера `http://localhost:xxxxx/Movies` так же, как запрос браузера `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="d2bbb-123">Результатом является пустой список фильмов, так как вы их еще не добавили.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="d2bbb-124">Создание фильма</span><span class="sxs-lookup"><span data-stu-id="d2bbb-124">Creating a Movie</span></span>

<span data-ttu-id="d2bbb-125">Щелкните ссылку **Create New** (Создать).</span><span class="sxs-lookup"><span data-stu-id="d2bbb-125">Select the **Create New** link.</span></span> <span data-ttu-id="d2bbb-126">Введите некоторые сведения о фильм, а затем нажмите кнопку **создать** кнопки.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-126">Enter some details about a movie and then click the **Create** button.</span></span>


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="d2bbb-127">Вы не сможете вводить десятичные точки или запятые в поле «Цена».</span><span class="sxs-lookup"><span data-stu-id="d2bbb-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="d2bbb-128">Поддержка проверки jQuery для английского, используйте запятую (&quot;,&quot;) для десятичного разделителя и форматов даты неанглийские США, необходимо включить *globalize.js* и конкретными  *cultures/globalize.cultures.js* файла (из [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) и JavaScript, чтобы использовать `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="d2bbb-129">Я покажу, как это сделать в следующем учебном курсе.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="d2bbb-130">А пока вводите целые числа, такие как 10.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-130">For now, just enter whole numbers like 10.</span></span>


<span data-ttu-id="d2bbb-131">Щелкнув **создать** кнопка вызывает отправку формы на сервер, где сведения о фильме сохраняются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="d2bbb-132">Затем вы перейдете к */Movies* URL-адрес, где вы увидите только что созданном фильме в листинг.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="d2bbb-133">Создайте еще несколько записей фильмов.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-133">Create a couple more movie entries.</span></span> <span data-ttu-id="d2bbb-134">Попробуйте воспользоваться ссылками **Изменить**, **Сведения** и **Удалить** — все они работают.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="d2bbb-135">Изучение созданного кода</span><span class="sxs-lookup"><span data-stu-id="d2bbb-135">Examining the Generated Code</span></span>

<span data-ttu-id="d2bbb-136">Откройте *Controllers\MoviesController.cs* файла и изучите созданный `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="d2bbb-137">Часть контроллера movie с `Index` метод приведен ниже.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="d2bbb-138">Запрос на `Movies` контроллер возвращает все записи в `Movies` таблицу и затем передает результаты в `Index` представления.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="d2bbb-139">В следующей строке из `MoviesController` класс создает экземпляр контекста базы данных фильмов, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="d2bbb-140">Контекст базы данных movie позволяет запрашивать, изменять и удалять элементы.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="d2bbb-141">Строго типизированные модели и @model ключевое слово</span><span class="sxs-lookup"><span data-stu-id="d2bbb-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="d2bbb-142">Ранее в этом учебнике вы видели, как контроллер может передать данные или объекты в шаблон представления с помощью `ViewBag` объекта.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="d2bbb-143">`ViewBag` Является динамическим объектом, который предоставляет удобный способ для передачи информации в представление с поздним связыванием.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="d2bbb-144">MVC также предоставляет возможность передавать *строго* типизированные объекты шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="d2bbb-145">Такой подход строго типизированные позволяет лучше компиляции во время проверки кода и более широкие [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) в редакторе Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="d2bbb-146">Этот подход используется механизм формирования шаблонов в Visual Studio (то есть передача *строго* типизированной модели) с `MoviesController` класс и представление шаблонов при создании методов и представлений.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="d2bbb-147">В *Controllers\MoviesController.cs* изучите созданный файл `Details` метод.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="d2bbb-148">`Details` Метод приведен ниже.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="d2bbb-149">`id` Параметр обычно передается в качестве данных маршрута, например `http://localhost:1234/movies/details/1` задаст контроллер к контроллеру фильмов, действие `details` и `id` 1.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="d2bbb-150">Можно также передавать в идентификаторе со строкой запроса следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d2bbb-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="d2bbb-151">Если `Movie` находится экземпляр `Movie` модель передается `Details` представления:</span><span class="sxs-lookup"><span data-stu-id="d2bbb-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="d2bbb-152">Изучение содержимого *Views\Movies\Details.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="d2bbb-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="d2bbb-153">Включив `@model` инструкция в верхней части файла шаблона представления, можно указать тип объекта, который будет ожидаться представлением.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="d2bbb-154">При создании контроллера movie Visual Studio автоматически включает следующий оператор `@model` в начало файла *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d2bbb-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="d2bbb-155">Эта директива `@model` обеспечивает доступ к фильму, который контроллер передал в представление с использованием строго типизированного объекта `Model`.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="d2bbb-156">Например, в *Details.cshtml* шаблона, код передает каждое поле фильма `DisplayNameFor` и [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) вспомогательных методов HTML со строго типизированным `Model` объекта.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="d2bbb-157">`Create` И `Edit` методы и шаблоны представлений также передать объект модели фильма.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="d2bbb-158">Изучите *Index.cshtml* шаблон представления и `Index` метод в *MoviesController.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="d2bbb-159">Обратите внимание на то, как в коде создается [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) объекта при вызове `View` вспомогательный метод в `Index` метода действия.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="d2bbb-160">Затем код передает это `Movies` список `Index` метода действия в представление:</span><span class="sxs-lookup"><span data-stu-id="d2bbb-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="d2bbb-161">При создании контроллера movie Visual Studio автоматически включает следующий `@model` инструкция в верхней части *Index.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="d2bbb-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="d2bbb-162">Это `@model` директива позволяет просматривать список фильмов, который контроллер передал в представление с помощью `Model` строго типизированного объекта.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="d2bbb-163">Например, в *Index.cshtml* шаблона, код перебирает фильмы во время работы `foreach` инструкции для строго типизированного `Model` объекта:</span><span class="sxs-lookup"><span data-stu-id="d2bbb-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="d2bbb-164">Так как `Model` строго типизированного объекта (как `IEnumerable<Movie>` объекта), каждая `item` объект в цикле типизируется как `Movie`.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="d2bbb-165">Помимо прочих преимуществ это означает, что получение проверки кода во время компиляции и полная поддержка IntelliSense в редакторе кода:</span><span class="sxs-lookup"><span data-stu-id="d2bbb-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="d2bbb-167">Работа с SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="d2bbb-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="d2bbb-168">Entity Framework Code First обнаружил, что указывает строку подключения базы данных, который был предоставлен `Movies` базы данных, которая не существует, поэтому Code First создает базу данных автоматически.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="d2bbb-169">Убедитесь, что он создан путем поиска *приложения\_данных* папки.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="d2bbb-170">Если вы не видите *Movies.mdf* щелкните **Показать все файлы** кнопку в **обозревателе решений** панели инструментов нажмите кнопку **обновить** кнопки, а затем разверните *приложения\_данных* папки.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="d2bbb-171">Дважды щелкните *Movies.mdf* открыть **ОБОЗРЕВАТЕЛЯ СЕРВЕРОВ**, затем разверните **таблиц** папки для просмотра таблицы фильмы.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="d2bbb-172">Обратите внимание на значок ключа рядом с идентификатором.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-172">Note the key icon next to ID.</span></span> <span data-ttu-id="d2bbb-173">По умолчанию EF сделает свойство с именем идентификатор первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="d2bbb-174">Дополнительные сведения о EF и MVC, см. в разделе отличную руководстве том Дайкстра на [MVC и EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="d2bbb-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="d2bbb-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="d2bbb-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="d2bbb-176">Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Показать таблицу данных** для просмотра данных, вы создали.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="d2bbb-177">Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Открыть определение таблицы** для просмотра таблицы структуры, Entity Framework Code First создана автоматически.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="d2bbb-178">Обратите внимание, что как схема `Movies` таблицы сопоставляется `Movie` класс, созданный ранее.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="d2bbb-179">Entity Framework Code First автоматически создается эта схема на основе вашего `Movie` класса.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="d2bbb-180">Когда вы закончите, закрыть соединение, щелкнув правой кнопкой мыши *MovieDBContext* и выбрав **закрыть подключение**.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="d2bbb-181">(Если не закрыть соединение, может появиться ошибка очередном запуске проекта).</span><span class="sxs-lookup"><span data-stu-id="d2bbb-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="d2bbb-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="d2bbb-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span></span>

<span data-ttu-id="d2bbb-183">Теперь у вас есть база данных и страницы для отображения, редактирования, обновления и удаления данных.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-183">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="d2bbb-184">В следующем учебном курсе мы просмотреть остаток шаблонный код и добавить `SearchIndex` метод и `SearchIndex` представление, которое дает возможность поиска фильмов в этой базе данных.</span><span class="sxs-lookup"><span data-stu-id="d2bbb-184">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="d2bbb-185">Дополнительные сведения об использовании Entity Framework с MVC см. в разделе [Создание модели данных Entity Framework для приложения ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="d2bbb-185">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d2bbb-186">[Назад](creating-a-connection-string.md)
> [Вперед](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="d2bbb-186">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
