---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Доступ к вашей модели данных от контроллера | Документы Microsoft
author: Rick-Anderson
description: 'Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасный, гораздо проще выполните и демонстрационных...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: cf896a6a9ce6cb8cd4adb13c3d87c4e7c3095fa6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="6223c-104">Доступ к вашей модели данных из контроллера</span><span class="sxs-lookup"><span data-stu-id="6223c-104">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="6223c-105">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="6223c-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="6223c-106">Доступна обновленная версия этого учебника [здесь](../../getting-started/introduction/getting-started.md) , с использованием ASP.NET MVC 5 и Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="6223c-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="6223c-107">Он является более безопасны, выполните гораздо проще и показаны дополнительные возможности.</span><span class="sxs-lookup"><span data-stu-id="6223c-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="6223c-108">В этом разделе вы создадите новый `MoviesController` класса и написать код, который получает данные фильма и отображает его в браузере, с помощью шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="6223c-108">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="6223c-109">**Постройте приложение** перед переходом к следующему шагу.</span><span class="sxs-lookup"><span data-stu-id="6223c-109">**Build the application** before going on to the next step.</span></span>

<span data-ttu-id="6223c-110">Щелкните правой кнопкой мыши *контроллеров* папки и создайте новый `MoviesController` контроллера.</span><span class="sxs-lookup"><span data-stu-id="6223c-110">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="6223c-111">Следующие параметры не будут отображаться до построения приложения.</span><span class="sxs-lookup"><span data-stu-id="6223c-111">The options below will not appear until you build your application.</span></span> <span data-ttu-id="6223c-112">Выберите следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="6223c-112">Select the following options:</span></span>

- <span data-ttu-id="6223c-113">Имя контроллера: **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="6223c-113">Controller name: **MoviesController**.</span></span> <span data-ttu-id="6223c-114">(Это значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6223c-114">(This is the default.</span></span> <span data-ttu-id="6223c-115">)</span><span class="sxs-lookup"><span data-stu-id="6223c-115">)</span></span>
- <span data-ttu-id="6223c-116">Шаблон: **контроллер MVC с действиями чтения и записи и представлениями, использующий Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="6223c-116">Template: **MVC Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="6223c-117">Класс модели: **фильма (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="6223c-117">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="6223c-118">Класс контекста данных: **MovieDBContext (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="6223c-118">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="6223c-119">Представления: **Razor (CSHTML)**.</span><span class="sxs-lookup"><span data-stu-id="6223c-119">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="6223c-120">(По умолчанию).</span><span class="sxs-lookup"><span data-stu-id="6223c-120">(The default.)</span></span>

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="6223c-122">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="6223c-122">Click **Add**.</span></span> <span data-ttu-id="6223c-123">Экспресс-выпуска Visual Studio создает следующие файлы и папки:</span><span class="sxs-lookup"><span data-stu-id="6223c-123">Visual Studio Express creates the following files and folders:</span></span>

- <span data-ttu-id="6223c-124">*MoviesController.cs* файл в проекте *контроллеров* папки.</span><span class="sxs-lookup"><span data-stu-id="6223c-124">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="6223c-125">Объект *фильмов* папки в проекте *представления* папки.</span><span class="sxs-lookup"><span data-stu-id="6223c-125">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="6223c-126">*CREATE.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml*, и *Index.cshtml* в новом *Views\Movies* папки.</span><span class="sxs-lookup"><span data-stu-id="6223c-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="6223c-127">ASP.NET MVC 4 автоматически создается CRUD (Создание, чтение, обновление и удаление) методы действий и представления автоматически (автоматическое создание CRUD методы действий и представления, называется формирование шаблонов).</span><span class="sxs-lookup"><span data-stu-id="6223c-127">ASP.NET MVC 4 automatically created the CRUD (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="6223c-128">Теперь у вас есть полнофункциональную веб-приложение, позволяющее создавать, список, изменение и удаление записей фильма.</span><span class="sxs-lookup"><span data-stu-id="6223c-128">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="6223c-129">Запустите приложение и перейдите к `Movies` контроллера путем добавления */Movies* URL-адрес в адресной строке браузера.</span><span class="sxs-lookup"><span data-stu-id="6223c-129">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="6223c-130">Так как приложение полагается на маршрутизацию по умолчанию (определенный в *Global.asax* файл), запрос браузера `http://localhost:xxxxx/Movies` направляется по умолчанию `Index` метод действия `Movies` контроллера.</span><span class="sxs-lookup"><span data-stu-id="6223c-130">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="6223c-131">Другими словами, запрос браузера `http://localhost:xxxxx/Movies` фактически является таким же, как запрос браузера `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="6223c-131">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="6223c-132">Результат — пустой список фильмов, так как вы их еще не добавили.</span><span class="sxs-lookup"><span data-stu-id="6223c-132">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a><span data-ttu-id="6223c-133">Создание фильма</span><span class="sxs-lookup"><span data-stu-id="6223c-133">Creating a Movie</span></span>

<span data-ttu-id="6223c-134">Щелкните ссылку **Create New** (Создать).</span><span class="sxs-lookup"><span data-stu-id="6223c-134">Select the **Create New** link.</span></span> <span data-ttu-id="6223c-135">Введите некоторые сведения о фильма и нажмите кнопку **создать** кнопки.</span><span class="sxs-lookup"><span data-stu-id="6223c-135">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image3.png)

<span data-ttu-id="6223c-136">Щелкнув **создать** кнопка вызывает отправку формы на сервер, где фильма сведения сохраняются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="6223c-136">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="6223c-137">Затем можно будет перенаправлен на */Movies* URL-адрес, где можно просмотреть только что созданный фильма в списке.</span><span class="sxs-lookup"><span data-stu-id="6223c-137">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="6223c-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span><span class="sxs-lookup"><span data-stu-id="6223c-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span></span>

<span data-ttu-id="6223c-139">Создайте еще несколько записей фильмов.</span><span class="sxs-lookup"><span data-stu-id="6223c-139">Create a couple more movie entries.</span></span> <span data-ttu-id="6223c-140">Попробуйте воспользоваться ссылками **Изменить**, **Сведения** и **Удалить** — все они работают.</span><span class="sxs-lookup"><span data-stu-id="6223c-140">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="6223c-141">Изучение созданного кода</span><span class="sxs-lookup"><span data-stu-id="6223c-141">Examining the Generated Code</span></span>

<span data-ttu-id="6223c-142">Откройте *Controllers\MoviesController.cs* файл и проверьте созданный `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="6223c-142">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="6223c-143">Часть контроллер фильма с `Index` метода показан ниже.</span><span class="sxs-lookup"><span data-stu-id="6223c-143">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="6223c-144">В следующей строке из `MoviesController` класс создает экземпляры фильма контекста базы данных, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="6223c-144">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="6223c-145">Контекст базы данных фильма можно использовать для запроса, редактировать и удалять элементы.</span><span class="sxs-lookup"><span data-stu-id="6223c-145">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="6223c-146">Запрос на `Movies` возвращает все записи в `Movies` фильма базы данных, а затем передает результаты в `Index` представления.</span><span class="sxs-lookup"><span data-stu-id="6223c-146">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="6223c-147">Строго типизированные моделей и @model ключевое слово</span><span class="sxs-lookup"><span data-stu-id="6223c-147">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="6223c-148">Ранее в этом учебнике показано, как контроллер может передать объекты или данные шаблона представления с помощью `ViewBag` объекта.</span><span class="sxs-lookup"><span data-stu-id="6223c-148">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="6223c-149">`ViewBag` Является динамическим объектом, который предоставляет удобный способ позднего связывания для передачи данных в представление.</span><span class="sxs-lookup"><span data-stu-id="6223c-149">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="6223c-150">ASP.NET MVC предоставляет также возможность передавать строго типизированные данные или объекты для шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="6223c-150">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="6223c-151">Это строго типизированными подход позволяет лучше проверка во время компиляции кода и обширные возможности IntelliSense в редакторе Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6223c-151">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Studio editor.</span></span> <span data-ttu-id="6223c-152">Механизм формирования шаблонов в Visual Studio используется этот подход с `MoviesController` класс и представление шаблонов при создании методов и представления.</span><span class="sxs-lookup"><span data-stu-id="6223c-152">The scaffolding mechanism in Visual Studio used this approach with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="6223c-153">В *Controllers\MoviesController.cs* Проверьте созданный файл `Details` метод.</span><span class="sxs-lookup"><span data-stu-id="6223c-153">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="6223c-154">Часть контроллер фильма с `Details` метода показан ниже.</span><span class="sxs-lookup"><span data-stu-id="6223c-154">A portion of the movie controller with the `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

<span data-ttu-id="6223c-155">Если `Movie` находится экземпляр `Movie` модель передается в представлении сведений.</span><span class="sxs-lookup"><span data-stu-id="6223c-155">If a `Movie` is found, an instance of the `Movie` model is passed to the Details view.</span></span> <span data-ttu-id="6223c-156">Изучение содержимого *Views\Movies\Details.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="6223c-156">Examine the contents of the *Views\Movies\Details.cshtml* file.</span></span>

<span data-ttu-id="6223c-157">Включив `@model` инструкции в верхней части представления файла шаблона, можно указать тип объекта, который ожидает представления.</span><span class="sxs-lookup"><span data-stu-id="6223c-157">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="6223c-158">При создании контроллера movie Visual Studio автоматически включает следующий оператор `@model` в начало файла *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6223c-158">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="6223c-159">Эта директива `@model` обеспечивает доступ к фильму, который контроллер передал в представление с использованием строго типизированного объекта `Model`.</span><span class="sxs-lookup"><span data-stu-id="6223c-159">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="6223c-160">Например, в *Details.cshtml* шаблона, код передает каждого фильма поля для `DisplayNameFor` и [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) вспомогательных методов HTML со строгой типизацией `Model` объекта.</span><span class="sxs-lookup"><span data-stu-id="6223c-160">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="6223c-161">Создание и редактирование методов и шаблонов представлений также передать объект модели фильма.</span><span class="sxs-lookup"><span data-stu-id="6223c-161">The Create and Edit methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="6223c-162">Изучите *Index.cshtml* Просмотр шаблона и `Index` метод в *MoviesController.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="6223c-162">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="6223c-163">Обратите внимание на то, как код создает [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) объект при вызове `View` вспомогательный метод `Index` метода действия.</span><span class="sxs-lookup"><span data-stu-id="6223c-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="6223c-164">Затем код передает это `Movies` список из контроллера в представление:</span><span class="sxs-lookup"><span data-stu-id="6223c-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

<span data-ttu-id="6223c-165">При создании контроллера фильм, экспресс-выпуска Visual Studio автоматически включаются следующие `@model` инструкции в верхней части *Index.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="6223c-165">When you created the movie controller, Visual Studio Express automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="6223c-166">Это `@model` директива позволяет получить доступ к списку фильмы, контроллера передается в представление, с помощью `Model` объекта со строгим типом.</span><span class="sxs-lookup"><span data-stu-id="6223c-166">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="6223c-167">Например, в *Index.cshtml* шаблона, код выполняет цикл по фильмы, выполняя `foreach` инструкции через строго типизированный `Model` объекта:</span><span class="sxs-lookup"><span data-stu-id="6223c-167">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="6223c-168">Поскольку `Model` объект является типобезопасным (как `IEnumerable<Movie>` объекта), каждая `item` объект в цикле типизируется как `Movie`.</span><span class="sxs-lookup"><span data-stu-id="6223c-168">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="6223c-169">Помимо прочих преимуществ это означает получение проверки кода во время компиляции и полная поддержка технологии IntelliSense в редакторе кода.</span><span class="sxs-lookup"><span data-stu-id="6223c-169">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="6223c-171">Работа с SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="6223c-171">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="6223c-172">Entity Framework Code First обнаружил, что указывает строку подключения базы данных, который был предоставлен `Movies` базы данных, которая не существует, поэтому Code First база данных создана автоматически.</span><span class="sxs-lookup"><span data-stu-id="6223c-172">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="6223c-173">Убедитесь, что он был создан путем поиска в *приложения\_данные* папки.</span><span class="sxs-lookup"><span data-stu-id="6223c-173">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="6223c-174">Если вы не видите *Movies.mdf* файла, нажмите кнопку **Показать все файлы** кнопку в **обозревателе решений** инструментов, нажмите кнопку **обновление** кнопку, а затем разверните *приложения\_данные* папки.</span><span class="sxs-lookup"><span data-stu-id="6223c-174">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="6223c-175">Дважды щелкните *Movies.mdf* Открытие **ОБОЗРЕВАТЕЛЬ баз данных**, затем разверните **таблиц** папку для просмотра фильмов таблицы.</span><span class="sxs-lookup"><span data-stu-id="6223c-175">Double-click *Movies.mdf* to open **DATABASE EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span>

<span data-ttu-id="6223c-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="6223c-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="6223c-177">Если не отображается в обозревателе базы данных, из **средства** последовательно выберите пункты **подключение к базе данных**, затем отменить **Выбор источника данных** диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="6223c-177">If the database explorer doesn't appear, from the **TOOLS** menu, select **Connect to Database**, then cancel the **Choose Data Source** dialog.</span></span> <span data-ttu-id="6223c-178">Это заставит откройте обозреватель баз данных.</span><span class="sxs-lookup"><span data-stu-id="6223c-178">This will force open the database explorer.</span></span>


> [!NOTE]
> <span data-ttu-id="6223c-179">Если вы используете VWD или Visual Studio 2010 и появляется сообщение об ошибке похожи на любую из следующих действий:</span><span class="sxs-lookup"><span data-stu-id="6223c-179">If you are using VWD or Visual Studio 2010 and get an error similar to any of the following following:</span></span>
> 
> - <span data-ttu-id="6223c-180">Базы данных "C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF "не удается открыть, поскольку она имеет версию 706.</span><span class="sxs-lookup"><span data-stu-id="6223c-180">The database 'C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES.MDF' cannot be opened because it is version 706.</span></span> <span data-ttu-id="6223c-181">Данный сервер поддерживает версию 655 и более ранних версий.</span><span class="sxs-lookup"><span data-stu-id="6223c-181">This server supports version 655 and earlier.</span></span> <span data-ttu-id="6223c-182">Переход на предыдущую версию не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="6223c-182">A downgrade path is not supported.</span></span>
> - <span data-ttu-id="6223c-183">&quot;InvalidOperation исключение было обработано пользовательским кодом&quot; переданном объекте SqlConnection не указан исходный каталог.</span><span class="sxs-lookup"><span data-stu-id="6223c-183">&quot;InvalidOperation Exception was unhandled by user code&quot; The supplied SqlConnection does not specify an initial catalog.</span></span>
> 
> <span data-ttu-id="6223c-184">Необходимо установить [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) и [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span><span class="sxs-lookup"><span data-stu-id="6223c-184">You need to install the [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) and [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span></span> <span data-ttu-id="6223c-185">Проверьте `MovieDBContext` строку подключения, заданную на предыдущей странице.</span><span class="sxs-lookup"><span data-stu-id="6223c-185">Verify the `MovieDBContext` connection string specified on the previous page.</span></span>


<span data-ttu-id="6223c-186">Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Показать таблицу данных** для просмотра данных, вы создали.</span><span class="sxs-lookup"><span data-stu-id="6223c-186">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image8.png)

<span data-ttu-id="6223c-187">Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Открыть определение таблицы** для просмотра структуры, Entity Framework Code First создаются таблицы.</span><span class="sxs-lookup"><span data-stu-id="6223c-187">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

<span data-ttu-id="6223c-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span><span class="sxs-lookup"><span data-stu-id="6223c-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span></span>

![](accessing-your-models-data-from-a-controller/_static/image10.png)

<span data-ttu-id="6223c-189">Обратите внимание как схема `Movies` таблицы сопоставляется `Movie` класса, созданного ранее.</span><span class="sxs-lookup"><span data-stu-id="6223c-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="6223c-190">Entity Framework Code First автоматически создается эту схему на основе вашего `Movie` класса.</span><span class="sxs-lookup"><span data-stu-id="6223c-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="6223c-191">После завершения закрыть подключение, щелкните правой кнопкой мыши *MovieDBContext* и выбрав **закрыть подключение**.</span><span class="sxs-lookup"><span data-stu-id="6223c-191">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="6223c-192">(Если не закрывать соединение, может появиться ошибка очередном запуске проекта).</span><span class="sxs-lookup"><span data-stu-id="6223c-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="6223c-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="6223c-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span></span>

<span data-ttu-id="6223c-194">Теперь у вас есть базы данных и страницу простого списка для отображения содержимого из него.</span><span class="sxs-lookup"><span data-stu-id="6223c-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="6223c-195">В следующем уроке мы изучите остальной части кода формирования шаблонов и добавьте `SearchIndex` метод и `SearchIndex` представление, позволяющее искать фильмов в этой базе данных.</span><span class="sxs-lookup"><span data-stu-id="6223c-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6223c-196">[Назад](adding-a-model.md)
> [Вперед](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="6223c-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
