---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Часть 2: Контроллеры | Документация Майкрософт'
author: jongalloway
description: В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store. В части 2 рассматривается контроллеров.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8824d5d2f5670aee2df6dc6e74767e4a851dd4ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841099"
---
<a name="part-2-controllers"></a><span data-ttu-id="e5820-104">Часть 2: контроллеры</span><span class="sxs-lookup"><span data-stu-id="e5820-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="e5820-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="e5820-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="e5820-106">Store музыки MVC является учебного приложения, которое вводятся и описываются пошаговые использование ASP.NET MVC и Visual Studio для разработки веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="e5820-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="e5820-107">Store музыки MVC — это реализация хранилища упрощенный пример, который продает музыкальные альбомы через Интернет и реализует базовый сайт администрирования, вход пользователя и корзины для покупок функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="e5820-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="e5820-108">В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="e5820-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="e5820-109">В части 2 рассматривается контроллеров.</span><span class="sxs-lookup"><span data-stu-id="e5820-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="e5820-110">С помощью традиционных веб-платформ URL-адреса обычно сопоставляются файлы на диске.</span><span class="sxs-lookup"><span data-stu-id="e5820-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="e5820-111">Например: запрос на URL-адрес, например «/ Products.aspx» или «/ Products.php» может быть обработан «Products.aspx» или «Products.php» файла.</span><span class="sxs-lookup"><span data-stu-id="e5820-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="e5820-112">Веб-платформы MVC сопоставления URL-адресов серверный код в немного иначе.</span><span class="sxs-lookup"><span data-stu-id="e5820-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="e5820-113">Вместо сопоставления URL-адреса к файлам, они вместо сопоставления URL-адресов методы в классах.</span><span class="sxs-lookup"><span data-stu-id="e5820-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="e5820-114">Эти классы называются «Контроллеры» и они отвечают за обработку входящих HTTP-запросов, обработка введенных пользователем данных, получения и сохранения данных и определения ответа для отправки обратно клиенту (отображения HTML-кода, загрузите файл, перенаправления на другой URL-адрес, т. д.).</span><span class="sxs-lookup"><span data-stu-id="e5820-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="e5820-115">Добавление HomeController</span><span class="sxs-lookup"><span data-stu-id="e5820-115">Adding a HomeController</span></span>

<span data-ttu-id="e5820-116">Начнем наше приложение Music Store MVC, добавив класс контроллера, который будет обрабатывать URL-адреса на домашнюю страницу узла.</span><span class="sxs-lookup"><span data-stu-id="e5820-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="e5820-117">Мы соответствуют соглашениям об именовании по умолчанию для ASP.NET MVC и вызывать его HomeController.</span><span class="sxs-lookup"><span data-stu-id="e5820-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="e5820-118">Щелкните правой кнопкой мыши папку «Контроллеры» в обозревателе решений и выберите «Добавить», а затем команду «Контроллер...»:</span><span class="sxs-lookup"><span data-stu-id="e5820-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="e5820-119">Откроется диалоговое окно «Добавление контроллера».</span><span class="sxs-lookup"><span data-stu-id="e5820-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="e5820-120">Имя контроллера «HomeController» и нажмите кнопку "Добавить".</span><span class="sxs-lookup"><span data-stu-id="e5820-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="e5820-121">Это создаст новый файл HomeController.cs, следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e5820-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="e5820-122">Для запуска с правами, заменим метод Index с простой метод, который возвращает просто строку.</span><span class="sxs-lookup"><span data-stu-id="e5820-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="e5820-123">Мы внесем два изменения:</span><span class="sxs-lookup"><span data-stu-id="e5820-123">We'll make two changes:</span></span>

- <span data-ttu-id="e5820-124">Изменение метода для возврата строки, а не ActionResult</span><span class="sxs-lookup"><span data-stu-id="e5820-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="e5820-125">Измените оператор return для возвращения «Hello из Home»</span><span class="sxs-lookup"><span data-stu-id="e5820-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="e5820-126">Метод должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e5820-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="e5820-127">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="e5820-127">Running the Application</span></span>

<span data-ttu-id="e5820-128">Теперь давайте запустим сайта.</span><span class="sxs-lookup"><span data-stu-id="e5820-128">Now let's run the site.</span></span> <span data-ttu-id="e5820-129">Мы можете запустить наш веб-сервер и попробовать сайта, с помощью любого из следующих::</span><span class="sxs-lookup"><span data-stu-id="e5820-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="e5820-130">Выберите отладки ⇨ начать отладку пункта меню</span><span class="sxs-lookup"><span data-stu-id="e5820-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="e5820-131">Щелкните кнопку с зеленой стрелкой на панели инструментов ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="e5820-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="e5820-132">Используйте сочетание клавиш, F5.</span><span class="sxs-lookup"><span data-stu-id="e5820-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="e5820-133">С помощью любого из описанных выше действий Скомпилируйте наш проект и привести ASP.NET Development Server, реализованная в Visual Web Developer для запуска.</span><span class="sxs-lookup"><span data-stu-id="e5820-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="e5820-134">Уведомление будет отображаться в нижнем углу экрана, чтобы указать, что ASP.NET Development Server запуска и будет отображаться номер порта, что он работает в группе.</span><span class="sxs-lookup"><span data-stu-id="e5820-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="e5820-135">Visual Web Developer будет автоматически откройте окно браузера, URL-адрес указывает на наш веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="e5820-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="e5820-136">Это позволит нам быстро опробовать нашего веб-приложения:</span><span class="sxs-lookup"><span data-stu-id="e5820-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="e5820-137">Хорошо, который был довольно краткий — мы создали новый веб-сайт, добавлена функция трех линий, и у нас есть текст в браузере.</span><span class="sxs-lookup"><span data-stu-id="e5820-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="e5820-138">Не ракеты обработки и анализа, но это start.</span><span class="sxs-lookup"><span data-stu-id="e5820-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="e5820-139">*Примечание: Visual Web Developer включает в себя ASP.NET Development Server, который ваш сайт будет работать на нескольких случайных бесплатный «порт». На снимке экрана выше, узел работает в `http://localhost:26641/`, поэтому он использует порт 26641. Ваш номер порта будет отличаться. Когда мы говорим о like /Store/Browse URL-адреса в этом руководстве, который перейдет после номера порта. При условии, что номер порта 26641, перейдя к/Store/обзора будет означать, перейдя к `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="e5820-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="e5820-140">Добавление StoreController</span><span class="sxs-lookup"><span data-stu-id="e5820-140">Adding a StoreController</span></span>

<span data-ttu-id="e5820-141">Мы добавили простой HomeController, который реализует домашней странице узла.</span><span class="sxs-lookup"><span data-stu-id="e5820-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="e5820-142">Теперь добавим другой контроллер, который мы будем использовать для реализации функциональности обозревателя наших музыкального магазина.</span><span class="sxs-lookup"><span data-stu-id="e5820-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="e5820-143">Контроллер хранилища будет поддерживать три сценария:</span><span class="sxs-lookup"><span data-stu-id="e5820-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="e5820-144">Страницы список жанров музыку в нашей музыкального магазина</span><span class="sxs-lookup"><span data-stu-id="e5820-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="e5820-145">Страница обзора, в котором перечислены все музыкальные альбомы определенного жанра</span><span class="sxs-lookup"><span data-stu-id="e5820-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="e5820-146">Сведения страницы, которая отображает сведения об определенных музыкальных альбомов</span><span class="sxs-lookup"><span data-stu-id="e5820-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="e5820-147">Мы начнем с добавления нового класса StoreController..</span><span class="sxs-lookup"><span data-stu-id="e5820-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="e5820-148">Если это еще не сделано, остановите работу приложения путем закрытие браузера или выбрав отладки ⇨ остановить отладку пункта меню.</span><span class="sxs-lookup"><span data-stu-id="e5820-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="e5820-149">Теперь добавьте новый StoreController.</span><span class="sxs-lookup"><span data-stu-id="e5820-149">Now add a new StoreController.</span></span> <span data-ttu-id="e5820-150">Как мы это делали HomeController, мы сделаем это, щелкнув правой кнопкой мыши на папку «Контроллеры» в обозревателе решений и выбрав команду Add -&gt;контроллера меню</span><span class="sxs-lookup"><span data-stu-id="e5820-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="e5820-151">Наш новый StoreController уже имеет метод «Index».</span><span class="sxs-lookup"><span data-stu-id="e5820-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="e5820-152">Мы будем использовать этот метод «Index», для реализации наш список страницу, которая перечисляет все жанры в нашей музыкального магазина.</span><span class="sxs-lookup"><span data-stu-id="e5820-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="e5820-153">Также мы добавим две дополнительные методы для реализации двух других сценариев мы хотим, чтобы наши StoreController для обработки: Обзор и сведения.</span><span class="sxs-lookup"><span data-stu-id="e5820-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="e5820-154">Эти методы (индекс, Обзор и сведения) в рамках нашего контроллера, называются «Контроллер действия», а как вы уже видели с методом действия HomeController.Index (), их задача состоит в том, чтобы отвечать на запросы URL-адрес и (в общем) определить, какое содержимое должны отправляться обратно в браузер или пользователь, вызвавший URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="e5820-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="e5820-155">Мы начнем наше решение StoreController, изменив метод theIndex() для возврата строки «Hello из Store.Index()», и мы добавим аналогичные методы для Browse() и Details():</span><span class="sxs-lookup"><span data-stu-id="e5820-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="e5820-156">Снова запустите проект и найдите следующие URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="e5820-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="e5820-157">/ Store</span><span class="sxs-lookup"><span data-stu-id="e5820-157">/Store</span></span>
- <span data-ttu-id="e5820-158">/ Store/обзора</span><span class="sxs-lookup"><span data-stu-id="e5820-158">/Store/Browse</span></span>
- <span data-ttu-id="e5820-159">/ Store/сведения</span><span class="sxs-lookup"><span data-stu-id="e5820-159">/Store/Details</span></span>

<span data-ttu-id="e5820-160">Доступ к следующим URL-адресам будет вызывать методы действий в рамках нашего контроллера и возвращают отклики строки:</span><span class="sxs-lookup"><span data-stu-id="e5820-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="e5820-161">Это здорово, но это просто константных строк.</span><span class="sxs-lookup"><span data-stu-id="e5820-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="e5820-162">Сделаем их динамический, поэтому они принимают сведения из URL-адрес и отобразить ее в выходных данных страницы.</span><span class="sxs-lookup"><span data-stu-id="e5820-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="e5820-163">Во-первых, мы изменим метод действия обзора для извлечения значения строки запроса в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="e5820-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="e5820-164">Мы это можно сделать путем добавления параметра «genre» к нашему методу действия.</span><span class="sxs-lookup"><span data-stu-id="e5820-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="e5820-165">При этом ASP.NET MVC автоматически передаст все строки запроса или параметров post формы с именем «genre», наш метод действия при его вызове.</span><span class="sxs-lookup"><span data-stu-id="e5820-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="e5820-166">*Примечание: Используется служебный метод HttpUtility.HtmlEncode чистку входные данные пользователя. Это предотвращает включение Javascript в нашего представления со ссылкой как /Store/Browse пользователями? Жанр =&lt;сценарий&gt;window.location= "http://hackersite.com"&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="e5820-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="e5820-167">Теперь давайте перейдите к/Store/обзора? Жанр = Disco</span><span class="sxs-lookup"><span data-stu-id="e5820-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="e5820-168">Затем изменим на действие Details для чтения и отображения входной параметр с именем ID.</span><span class="sxs-lookup"><span data-stu-id="e5820-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="e5820-169">В отличие от наших предыдущему методу мы не внедрять содержимое значение идентификатора как параметр строки запроса.</span><span class="sxs-lookup"><span data-stu-id="e5820-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="e5820-170">Вместо этого мы внедрим его непосредственно в самой URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="e5820-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="e5820-171">Например: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="e5820-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="e5820-172">ASP.NET MVC позволяет легко сделать это без необходимости ничего настраивать.</span><span class="sxs-lookup"><span data-stu-id="e5820-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="e5820-173">Соглашение о маршрутизации ASP.NET MVC по умолчанию является считать параметр с именем «ID» в сегменте URL-адреса после имени метода действия.</span><span class="sxs-lookup"><span data-stu-id="e5820-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="e5820-174">Если метод действия имеет параметр с именем идентификатор затем ASP.NET MVC автоматически передаст сегмент URL-адреса для вас как параметр.</span><span class="sxs-lookup"><span data-stu-id="e5820-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="e5820-175">Запустите приложение и перейдите к /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="e5820-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="e5820-176">Давайте освежим в памяти, что мы сделали:</span><span class="sxs-lookup"><span data-stu-id="e5820-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="e5820-177">Мы создали новый проект ASP.NET MVC в Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="e5820-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="e5820-178">Мы уже обсудили, структура базовой папки приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e5820-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="e5820-179">Мы узнали, как запустить наш веб-сайт, с использованием сервера разработки ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e5820-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="e5820-180">Мы создали два класса контроллера: HomeController и StoreController</span><span class="sxs-lookup"><span data-stu-id="e5820-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="e5820-181">Мы добавили методы действий на наших контроллеры, которые отвечают на запросы на URL-адрес и возвращают текст в браузер</span><span class="sxs-lookup"><span data-stu-id="e5820-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="e5820-182">[Назад](mvc-music-store-part-1.md)
> [Вперед](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="e5820-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
