---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: "Добавление Create-метод и создать представление | Документы Microsoft"
author: shanselman
description: "Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Создание простого веб-приложения, чтение и запись из базы данных."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 36b3d6ef0432292f21ecd8f29ea2d88ee8867436
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
<a name="adding-a-create-method-and-create-view"></a><span data-ttu-id="d1377-104">Добавление создайте метод и создание представления</span><span class="sxs-lookup"><span data-stu-id="d1377-104">Adding a Create Method and Create View</span></span>
====================
<span data-ttu-id="d1377-105">по [Скотт Хансельман](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="d1377-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="d1377-106">Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d1377-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="d1377-107">Вы создадите простое веб-приложение выполняет чтение и запись из базы данных.</span><span class="sxs-lookup"><span data-stu-id="d1377-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="d1377-108">Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.</span><span class="sxs-lookup"><span data-stu-id="d1377-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="d1377-109">В этом разделе мы собираемся реализовать поддержку, необходимую для позволяют пользователям создавать новые фильмы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d1377-109">In this section we are going to implement the support necessary to enable users to create new movies in our database.</span></span> <span data-ttu-id="d1377-110">Нам предстоит выполнить путем реализации фильмов или создать URL-адрес действия.</span><span class="sxs-lookup"><span data-stu-id="d1377-110">We'll do this by implementing the /Movies/Create URL action.</span></span>

<span data-ttu-id="d1377-111">Реализация фильмов или создать URL-адрес является в два этапа.</span><span class="sxs-lookup"><span data-stu-id="d1377-111">Implementing the /Movies/Create URL is a two step process.</span></span> <span data-ttu-id="d1377-112">Пользователь посещает в первую очередь фильмов или создать URL-адрес мы хотим отобразить их в HTML-форму, можно заполнить для ввода новых фильмов.</span><span class="sxs-lookup"><span data-stu-id="d1377-112">When a user first visits the /Movies/Create URL we want to show them an HTML form that they can fill out to enter a new movie.</span></span> <span data-ttu-id="d1377-113">Затем когда пользователь отправляет форму и записи данных обратно на сервер, мы хотим получить отправленное содержимое и сохраните его в базу данных.</span><span class="sxs-lookup"><span data-stu-id="d1377-113">Then, when the user submits the form and posts the data back to the server, we want to retrieve the posted contents and save it into our database.</span></span>

<span data-ttu-id="d1377-114">Следующие два действия в двух методах Create() реализуется в нашем классе MoviesController.</span><span class="sxs-lookup"><span data-stu-id="d1377-114">We'll implement these two steps within two Create() methods within our MoviesController class.</span></span> <span data-ttu-id="d1377-115">Будет отображаться один метод &lt;формы&gt; , пользователь должен заполнять для создания новых фильмов.</span><span class="sxs-lookup"><span data-stu-id="d1377-115">One method will show the &lt;form&gt; that the user should fill out to create a new movie.</span></span> <span data-ttu-id="d1377-116">Второй способ обработки данных при отправке &lt;формы&gt; обратно на сервер и сохранить новый фильм в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d1377-116">The second method will handle processing the posted data when the user submits the &lt;form&gt; back to the server, and save a new Movie within our database.</span></span>

<span data-ttu-id="d1377-117">Ниже приведен полный код мы добавим наш MoviesController класс, чтобы реализовать это:</span><span class="sxs-lookup"><span data-stu-id="d1377-117">Below is the code we'll add to our MoviesController class to implement this:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

<span data-ttu-id="d1377-118">Приведенный выше код содержит весь код, он понадобится в нашем контроллера.</span><span class="sxs-lookup"><span data-stu-id="d1377-118">The above code contains all of the code that we'll need within our Controller.</span></span>

<span data-ttu-id="d1377-119">Теперь рассмотрим процедуру внедрения Create View шаблон, который будет использоваться для отображения формы для пользователя.</span><span class="sxs-lookup"><span data-stu-id="d1377-119">Let's now implement the Create View template that we'll use to display a form to the user.</span></span> <span data-ttu-id="d1377-120">Мы будем щелкните правой кнопкой мыши в первый метод Create и выберите «Добавить представление» для создания шаблона представления для нашу форму фильма.</span><span class="sxs-lookup"><span data-stu-id="d1377-120">We'll right click in the first Create method and select "Add View" to create the view template for our Movie form.</span></span>

<span data-ttu-id="d1377-121">Мы выберем мы будет передавать Просмотр шаблона «Фильм» как класс представления данных, которые указывают, что мы хотим «на основе скаффолдинга» шаблон «Создать».</span><span class="sxs-lookup"><span data-stu-id="d1377-121">We'll select that we are going to pass the view template a "Movie" as its view data class, and indicate that we want to "scaffold" a "Create" template.</span></span>

<span data-ttu-id="d1377-122">[![Добавить представление](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d1377-122">[![Add View](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span></span>

<span data-ttu-id="d1377-123">После нажатия кнопки "Добавить" \Movies\Create.aspx Просмотр шаблона создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="d1377-123">After you click the Add button, \Movies\Create.aspx View template will be created for you.</span></span> <span data-ttu-id="d1377-124">Так как мы выбрали «Создать» из раскрывающегося списка «Просмотр содержимого», диалоговое окно добавления представления автоматически «формирования шаблонов» содержимое по умолчанию для нас.</span><span class="sxs-lookup"><span data-stu-id="d1377-124">Because we selected "Create" from the "view content" dropdown, the Add View dialog automatically "scaffolded" some default content for us.</span></span> <span data-ttu-id="d1377-125">Формирование шаблонов создана HTML &lt;формы&gt;, место для ошибки проверки сообщений для перехода, и с момента формирования шаблонов знает о фильмах, она создана метки и поля для каждого свойства класса.</span><span class="sxs-lookup"><span data-stu-id="d1377-125">The scaffolding created an HTML &lt;form&gt;, a place for validation error messages to go, and since scaffolding knows about Movies, it created Label and Fields for each property of our class.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

<span data-ttu-id="d1377-126">Так как идентификатор базе данных автоматически предоставляет фильм, удалим эти поля этой эталонной модели. Идентификатор из наших создать представление.</span><span class="sxs-lookup"><span data-stu-id="d1377-126">Since our database automatically gives a Movie an ID, let's remove those fields that reference model.Id from our Create View.</span></span> <span data-ttu-id="d1377-127">Удалить строки 7 после &lt;условных обозначений&gt;поля&lt;/legend&gt; как они показывают поля «идентификатор» в том, что мы не хотим.</span><span class="sxs-lookup"><span data-stu-id="d1377-127">Remove the 7 lines after &lt;legend&gt;Fields&lt;/legend&gt; as they show the ID field that we don't want.</span></span>

<span data-ttu-id="d1377-128">Теперь давайте создать новый фильма и добавить его в базу данных.</span><span class="sxs-lookup"><span data-stu-id="d1377-128">Let's now create a new movie and add it to the database.</span></span> <span data-ttu-id="d1377-129">Мы это сделать, запустив приложение еще раз и посетите «/ фильмов» связать URL-адрес и нажмите кнопку «Создать» для добавления новых фильмов.</span><span class="sxs-lookup"><span data-stu-id="d1377-129">We'll do this by running the application again and visit the "/Movies" URL and click the "Create" link to add a new Movie.</span></span>

<span data-ttu-id="d1377-130">[![Создание - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d1377-130">[![Create - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span></span>

<span data-ttu-id="d1377-131">После нажатия кнопки «Создать», мы будем публиковать обратно (через HTTP POST) данные в этой форме /Movies/Create метода, который мы только что создали.</span><span class="sxs-lookup"><span data-stu-id="d1377-131">When we click the Create button, we'll be posting back (via HTTP POST) the data on this form to the /Movies/Create method that we just created.</span></span> <span data-ttu-id="d1377-132">Так же, как при система автоматически заняло параметр «numTimes» и «name», вне URL-адрес и сопоставить их с параметрами метода ранее система автоматически получить дополнительные поля формы POST и сопоставить их с объектом.</span><span class="sxs-lookup"><span data-stu-id="d1377-132">Just like when the system automatically took the "numTimes" and "name " parameter out of the URL and mapped them to parameters on a method earlier, the system will automatically take the Form Fields from a POST and map them to an object.</span></span> <span data-ttu-id="d1377-133">В этом случае значений из полей в формате HTML, такие как «ReleaseDate» и «Title» будет автоматически помещен в необходимые свойства нового экземпляра фильма.</span><span class="sxs-lookup"><span data-stu-id="d1377-133">In this case, values from fields in HTML like "ReleaseDate" and "Title" will automatically be put into the correct properties of a new instance of a Movie.</span></span>

<span data-ttu-id="d1377-134">Давайте взглянем на второй метод Create из наших MoviesController еще раз.</span><span class="sxs-lookup"><span data-stu-id="d1377-134">Let's look at the second Create method from our MoviesController again.</span></span> <span data-ttu-id="d1377-135">Обратите внимание на то, как он принимает объект «Фильма» в качестве аргумента:</span><span class="sxs-lookup"><span data-stu-id="d1377-135">Notice how it takes a "Movie" object as an argument:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

<span data-ttu-id="d1377-136">Этот объект фильма затем передается [HttpPost] версия наш метод действия Create, и мы хранится в базе данных, а затем перенаправляется пользователь метод действия Index(), который будет отображать сохраненный результат в списке фильма:</span><span class="sxs-lookup"><span data-stu-id="d1377-136">This Movie object was then passed to the [HttpPost] version of our Create action method, and we saved it in the database and then redirected the user back to the Index() action method which will show the saved result in the movie list:</span></span>

<span data-ttu-id="d1377-137">[![Список фильмов - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d1377-137">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span></span>

<span data-ttu-id="d1377-138">Мы не проверки, если нашей фильмы указаны правильно, однако, а базы данных не позволяет сохранить с без названия фильма.</span><span class="sxs-lookup"><span data-stu-id="d1377-138">We aren't checking if our movies are correct, though, and the database won't allow us to save a movie with no Title.</span></span> <span data-ttu-id="d1377-139">Было бы неплохо, если мы может уведомить пользователя, что перед базы данных возникла ошибка.</span><span class="sxs-lookup"><span data-stu-id="d1377-139">It'd be nice if we could tell the user that before the database threw an error.</span></span> <span data-ttu-id="d1377-140">Дальнейшие действия будут выполнены, добавляя поддержку проверки для нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="d1377-140">We'll do this next by adding validation support to our application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d1377-141">[Назад](getting-started-with-mvc-part5.md)
[Вперед](getting-started-with-mvc-part7.md)</span><span class="sxs-lookup"><span data-stu-id="d1377-141">[Previous](getting-started-with-mvc-part5.md)
[Next](getting-started-with-mvc-part7.md)</span></span>
