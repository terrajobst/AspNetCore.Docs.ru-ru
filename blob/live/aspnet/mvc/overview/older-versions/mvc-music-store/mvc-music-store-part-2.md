---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: "Часть 2: Контроллеры | Документы Microsoft"
author: jongalloway
description: "Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store. Вторая часть посвящена контроллеров."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: bdafd751e996e759d516d0fa25b09eff21241ed7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="part-2-controllers"></a><span data-ttu-id="68300-104">Часть 2: контроллеры</span><span class="sxs-lookup"><span data-stu-id="68300-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="68300-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="68300-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="68300-106">MVC Music Store является учебное приложение, которое вводятся и описываются пошаговые способы использования Visual Studio и ASP.NET MVC для веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="68300-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="68300-107">MVC Music Store показана реализация хранилища упрощенных образец продает велосипеды через Интернет альбомы, которое реализует Администрирование сайта основные, вход пользователей и функциональные возможности корзины покупок.</span><span class="sxs-lookup"><span data-stu-id="68300-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="68300-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="68300-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="68300-109">Вторая часть посвящена контроллеров.</span><span class="sxs-lookup"><span data-stu-id="68300-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="68300-110">С традиционной веб-платформами URL-адреса обычно сопоставляются с файлов на диске.</span><span class="sxs-lookup"><span data-stu-id="68300-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="68300-111">Например: запрос на URL-адрес, например «/ Products.aspx» или «/ Products.php» может быть обработан файл «Products.aspx» или «Products.php».</span><span class="sxs-lookup"><span data-stu-id="68300-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="68300-112">Веб-платформы MVC сопоставления URL-адресов серверный код в немного иначе.</span><span class="sxs-lookup"><span data-stu-id="68300-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="68300-113">Вместо сопоставления URL-адреса для файлов, они вместо сопоставления URL-адресов в методы в классах.</span><span class="sxs-lookup"><span data-stu-id="68300-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="68300-114">Эти классы называются «Контроллеры» и они отвечают за обработку входящих HTTP-запросов, обработка введенных пользователем данных, получения и сохранения данных и определения ответа для отправки обратно клиенту (отображения HTML-кода, загрузите файл, перенаправление на другой URL-адрес и т. д.).</span><span class="sxs-lookup"><span data-stu-id="68300-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="68300-115">Добавление HomeController</span><span class="sxs-lookup"><span data-stu-id="68300-115">Adding a HomeController</span></span>

<span data-ttu-id="68300-116">Начнем нашего приложения MVC Music Store, добавив класс контроллера, который будет обрабатывать URL-адреса на домашнюю страницу веб-сайтов.</span><span class="sxs-lookup"><span data-stu-id="68300-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="68300-117">Мы будем следовать соглашениям об именовании по умолчанию ASP.NET MVC и назовите его HomeController.</span><span class="sxs-lookup"><span data-stu-id="68300-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="68300-118">Щелкните правой кнопкой мыши папку «Контроллеры» в обозревателе решений и выберите пункт «Добавить» и «Контроллер...»</span><span class="sxs-lookup"><span data-stu-id="68300-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…"</span></span> <span data-ttu-id="68300-119">команда:</span><span class="sxs-lookup"><span data-stu-id="68300-119">command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="68300-120">Откроется диалоговое окно «Добавить контроллер».</span><span class="sxs-lookup"><span data-stu-id="68300-120">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="68300-121">Имя контроллера «HomeController» и нажмите кнопку Добавить.</span><span class="sxs-lookup"><span data-stu-id="68300-121">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="68300-122">Это создаст новый файл HomeController.cs, с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="68300-122">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="68300-123">Чтобы запустить быстро, давайте замените метод индекс простой метод, который возвращает просто строку.</span><span class="sxs-lookup"><span data-stu-id="68300-123">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="68300-124">Мы выполним два изменения:</span><span class="sxs-lookup"><span data-stu-id="68300-124">We'll make two changes:</span></span>

- <span data-ttu-id="68300-125">Изменение метода для возврата строки вместо ActionResult</span><span class="sxs-lookup"><span data-stu-id="68300-125">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="68300-126">Измените инструкцию return для возвращения «Hello из Home»</span><span class="sxs-lookup"><span data-stu-id="68300-126">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="68300-127">Метод должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="68300-127">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="68300-128">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="68300-128">Running the Application</span></span>

<span data-ttu-id="68300-129">Теперь давайте запуска сайта.</span><span class="sxs-lookup"><span data-stu-id="68300-129">Now let's run the site.</span></span> <span data-ttu-id="68300-130">Мы можно запустить наш веб сервера и испытать сайта, с помощью любого из следующих действий:</span><span class="sxs-lookup"><span data-stu-id="68300-130">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="68300-131">Выберите элемент меню отладки начать отладку ⇨</span><span class="sxs-lookup"><span data-stu-id="68300-131">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="68300-132">Нажмите кнопку с зеленой стрелкой на панели инструментов![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="68300-132">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="68300-133">Используйте сочетание клавиш F5.</span><span class="sxs-lookup"><span data-stu-id="68300-133">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="68300-134">С помощью любого из приведенных выше пунктов компиляции проекта и затем вызвать ASP.NET Development Server, встроены в Visual Web Developer для запуска.</span><span class="sxs-lookup"><span data-stu-id="68300-134">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="68300-135">Уведомление будет отображаться в нижнем углу экрана, чтобы указать, что ASP.NET Development Server начал а отображается номер порта работает в группе.</span><span class="sxs-lookup"><span data-stu-id="68300-135">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="68300-136">Visual Web Developer будет автоматически откройте окно браузера, URL-адрес указывает на наш веб сервера.</span><span class="sxs-lookup"><span data-stu-id="68300-136">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="68300-137">Это позволит быстро опробовать наш веб-приложения:</span><span class="sxs-lookup"><span data-stu-id="68300-137">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="68300-138">Хорошо, была очень быстрый — мы создали новый веб-сайт, добавлена функция три строки, и у нас есть текст в браузере.</span><span class="sxs-lookup"><span data-stu-id="68300-138">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="68300-139">Не rocket обработки и анализа, но это start.</span><span class="sxs-lookup"><span data-stu-id="68300-139">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="68300-140">*Примечание: Visual Web Developer включает сервер разработки ASP.NET, в котором будет выполняться веб-сайта на число случайных свободного «порт». На снимке экрана выше, что сайт работает в `http://localhost:26641/`, поэтому он использует порт 26641. Этот номер порта будет отличаться. Если говорить о like /Store/Browse URL-адреса в этом учебнике, будут рассмотрены после номера порта. При условии, что номер порта 26641, перехода или хранилища или обзора означает перехода `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="68300-140">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="68300-141">Добавление StoreController</span><span class="sxs-lookup"><span data-stu-id="68300-141">Adding a StoreController</span></span>

<span data-ttu-id="68300-142">Мы добавили простой HomeController, который реализует домашнюю страницу веб-сайтов.</span><span class="sxs-lookup"><span data-stu-id="68300-142">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="68300-143">Теперь добавим другого контроллера, мы будем использовать для реализации возможности просмотра хранилища нашей музыки.</span><span class="sxs-lookup"><span data-stu-id="68300-143">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="68300-144">Наш контроллера хранилище поддерживает три сценария:</span><span class="sxs-lookup"><span data-stu-id="68300-144">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="68300-145">Страницы список жанров музыки в нашем music store</span><span class="sxs-lookup"><span data-stu-id="68300-145">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="68300-146">Страница обзора, в котором перечислены все альбомы определенного жанра</span><span class="sxs-lookup"><span data-stu-id="68300-146">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="68300-147">Веб-страницу и отображает сведения о конкретных музыкальный альбом</span><span class="sxs-lookup"><span data-stu-id="68300-147">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="68300-148">Мы начнем, добавив новый класс StoreController...</span><span class="sxs-lookup"><span data-stu-id="68300-148">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="68300-149">Если это еще не сделано, остановите, работающим с приложением, либо закрыть браузер или с помощью команды отладки ⇨ остановить отладку элемента меню.</span><span class="sxs-lookup"><span data-stu-id="68300-149">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="68300-150">Теперь добавьте новый StoreController.</span><span class="sxs-lookup"><span data-stu-id="68300-150">Now add a new StoreController.</span></span> <span data-ttu-id="68300-151">Так же, как мы сделали с HomeController, мы сделаем это путем щелчка правой кнопкой мыши папку «Контроллеры» в обозревателе решений и выбрав команду Add -&gt;контроллера пункта меню</span><span class="sxs-lookup"><span data-stu-id="68300-151">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="68300-152">Наш новый StoreController уже имеет метод «Индекс».</span><span class="sxs-lookup"><span data-stu-id="68300-152">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="68300-153">Мы будем использовать этот метод «Индекс» для реализации нашу страницу списка со списком всех жанров в нашем music store.</span><span class="sxs-lookup"><span data-stu-id="68300-153">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="68300-154">Кроме того, мы добавим два дополнительных методов для реализации в двух других сценариях мы хотим, чтобы наши StoreController для обработки: Обзор и сведения.</span><span class="sxs-lookup"><span data-stu-id="68300-154">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="68300-155">Эти методы (индексов, Обзор и сведения), в нашем контроллера, называются «Действия контроллера» и как вы уже видели с методом действия HomeController.Index (), их задания будет отвечать на запросы на URL-адрес и (в целом) определите, какое содержимое должны отправляться обратно в браузер или пользователь, вызвавший URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="68300-155">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="68300-156">Мы начнем StoreController реализацию, изменив метод theIndex() возвращать строку «Hello из Store.Index()», и мы добавим аналогичные методы для Browse() и Details():</span><span class="sxs-lookup"><span data-stu-id="68300-156">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="68300-157">Запустите проект и найдите следующие URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="68300-157">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="68300-158">/ Store</span><span class="sxs-lookup"><span data-stu-id="68300-158">/Store</span></span>
- <span data-ttu-id="68300-159">/ Store/обзора</span><span class="sxs-lookup"><span data-stu-id="68300-159">/Store/Browse</span></span>
- <span data-ttu-id="68300-160">/ / Сведения о хранилище</span><span class="sxs-lookup"><span data-stu-id="68300-160">/Store/Details</span></span>

<span data-ttu-id="68300-161">Доступ к URL-адресов вызывать методы действий в рамках нашей контроллера и возвращать строку ответы:</span><span class="sxs-lookup"><span data-stu-id="68300-161">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="68300-162">Это здорово, но они просто константных строк.</span><span class="sxs-lookup"><span data-stu-id="68300-162">That's great, but these are just constant strings.</span></span> <span data-ttu-id="68300-163">Давайте сделать их динамическими, таким образом получить сведения из URL-адрес и отобразить их в выходные данные страницы.</span><span class="sxs-lookup"><span data-stu-id="68300-163">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="68300-164">Сначала мы изменим метод действия обзора для извлечения значения строки запроса URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="68300-164">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="68300-165">Мы можно сделать, добавив параметр «жанр» нашей метода действия.</span><span class="sxs-lookup"><span data-stu-id="68300-165">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="68300-166">Когда мы делаем это ASP.NET MVC будет автоматически передавать все строки запроса или параметров отправки форм с именем «жанр» наш метод действия, при вызове.</span><span class="sxs-lookup"><span data-stu-id="68300-166">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="68300-167">*Примечание: Используется вспомогательный метод HttpUtility.HtmlEncode чистку ввод данных пользователем. Это предотвращает добавление Javascript в нашем представления с помощью ссылки, например /Store/Browse пользователями? Жанр =&lt;сценарий&gt;window.location= «http://hackersite.com»&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="68300-167">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="68300-168">Теперь давайте найдите/Store/обзора? Жанр = Disco</span><span class="sxs-lookup"><span data-stu-id="68300-168">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="68300-169">Теперь изменим на действие Details для чтения и отображения входной параметр с именем идентификатора.</span><span class="sxs-lookup"><span data-stu-id="68300-169">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="68300-170">В отличие от наших предыдущему методу мы не внедрение значение идентификатора в качестве параметра строки запроса.</span><span class="sxs-lookup"><span data-stu-id="68300-170">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="68300-171">Вместо этого мы будем внедрять непосредственно в самом URL-АДРЕСЕ.</span><span class="sxs-lookup"><span data-stu-id="68300-171">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="68300-172">Например: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="68300-172">For example: /Store/Details/5.</span></span>

<span data-ttu-id="68300-173">ASP.NET MVC позволяет легко реализовать без необходимости настройки.</span><span class="sxs-lookup"><span data-stu-id="68300-173">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="68300-174">Соглашение о маршрутизации ASP.NET MVC по умолчанию является считать параметр с именем «ID» в сегменте URL-адрес после имени метода действия.</span><span class="sxs-lookup"><span data-stu-id="68300-174">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="68300-175">Если метод действия имеет параметр с именем идентификатор затем ASP.NET MVC будет автоматически передавать сегмента URL-адреса вам как параметр.</span><span class="sxs-lookup"><span data-stu-id="68300-175">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="68300-176">Запустите приложение и перейдите к /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="68300-176">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="68300-177">Давайте повторим, мы использовали пока:</span><span class="sxs-lookup"><span data-stu-id="68300-177">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="68300-178">Мы создали новый проект ASP.NET MVC в Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="68300-178">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="68300-179">Мы обсуждали структура базовой папки приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="68300-179">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="68300-180">Мы узнали, как запустить наш веб-сайт с помощью сервера разработки ASP.NET</span><span class="sxs-lookup"><span data-stu-id="68300-180">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="68300-181">Мы создали два класса контроллера: HomeController и StoreController</span><span class="sxs-lookup"><span data-stu-id="68300-181">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="68300-182">Мы добавили методы действий в нашей контроллеров, которые отвечают на запросы на URL-адрес и возвращают текст в браузере</span><span class="sxs-lookup"><span data-stu-id="68300-182">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


>[!div class="step-by-step"]
<span data-ttu-id="68300-183">[Назад](mvc-music-store-part-1.md)
[Вперед](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="68300-183">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
