---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: "Часть 3: Представления и ViewModels | Документы Microsoft"
author: jongalloway
description: "Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store. Часть 3 описывает представления и ViewModels."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 5b38f88283c5d2d93f0bab283dbd9451855d95e3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="fc6d1-104">Часть 3: Представления и ViewModels</span><span class="sxs-lookup"><span data-stu-id="fc6d1-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="fc6d1-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="fc6d1-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="fc6d1-106">MVC Music Store является учебное приложение, которое вводятся и описываются пошаговые способы использования Visual Studio и ASP.NET MVC для веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="fc6d1-107">MVC Music Store показана реализация хранилища упрощенных образец продает велосипеды через Интернет альбомы, которое реализует Администрирование сайта основные, вход пользователей и функциональные возможности корзины покупок.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="fc6d1-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="fc6d1-109">Часть 3 описывает представления и ViewModels.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="fc6d1-110">Пока мы просто возврат строк из действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="fc6d1-111">Это хорошо способ получить представление о работе контроллеров, но это не будет способа построения реальных веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="fc6d1-112">Мы собираемся требуется более эффективный способ создания HTML-кода обратно в браузерах, посетите наш сайт — один, где можно использовать файлы шаблонов для упрощения настройки HTML-содержимого, отправка обратно.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="fc6d1-113">Именно это сделать представления.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="fc6d1-114">Добавление шаблона представления</span><span class="sxs-lookup"><span data-stu-id="fc6d1-114">Adding a View template</span></span>

<span data-ttu-id="fc6d1-115">Чтобы использовать шаблон представления, мы изменить индекс HomeController метод для возврата ActionResult и возвращать View(), например, ниже:</span><span class="sxs-lookup"><span data-stu-id="fc6d1-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="fc6d1-116">Выше изменение означает, что вместо возвратил строку, вместо этого мы хотим использовать «Представление» для получения результата обратно.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="fc6d1-117">Теперь мы добавим соответствующий шаблон представления в свой проект.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="fc6d1-118">Для этого мы будем поместите курсор в методе действия, индекс, текст затем щелкните правой кнопкой мыши и выберите «Добавить представление».</span><span class="sxs-lookup"><span data-stu-id="fc6d1-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="fc6d1-119">Откроется диалоговое окно добавления представления:</span><span class="sxs-lookup"><span data-stu-id="fc6d1-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="fc6d1-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fc6d1-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="fc6d1-121">Диалоговое окно «Добавление представления» позволяет быстро и легко создавать файлы шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="fc6d1-122">По умолчанию «Добавление представления» диалогового окна предварительно определяет имя шаблона представления для создания, чтобы он соответствовал метод действия, который будет его использовать.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="fc6d1-123">Так как мы использовали контекстное меню «Добавление представления» в методе действия Index() нашей HomeController, диалоговое окно «Добавление представления» выше совпадает с «Индекс» подставлен по умолчанию имя представления.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="fc6d1-124">Не нужно изменять любые параметры в этом диалоговом окне, нажмите кнопку Добавить.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="fc6d1-125">После нажатия кнопки "Добавить", Visual Web Developer создаст новый Index.cshtml Просмотр шаблона нам в каталоге \Views\Home папка создается, если еще не существует.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="fc6d1-126">Имя и расположение файла «Index.cshtml» важен и проходит соглашения об именовании ASP.NET MVC по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="fc6d1-127">Имя каталога, \Views\Home, соответствует контроллеру - называется HomeController.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="fc6d1-128">Имя шаблона просмотр индекса, соответствует метод действия контроллера, который будет применяться для отображения представления.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="fc6d1-129">ASP.NET MVC позволяет избежать необходимости явно указывать имя или расположение шаблона представления, если мы используем это соглашение об именовании для возврата представления.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="fc6d1-130">Он будет по умолчанию отображать \Views\Home\Index.cshtml Просмотр шаблона при написании кода ниже в нашем HomeController:</span><span class="sxs-lookup"><span data-stu-id="fc6d1-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="fc6d1-131">Visual Web Developer создается и открывается шаблон представления «Index.cshtml», после мы нажата кнопка «Добавить» в диалоговом окне «Добавление представления».</span><span class="sxs-lookup"><span data-stu-id="fc6d1-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="fc6d1-132">Содержимое Index.cshtml приведены ниже.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="fc6d1-133">Это представление используется синтаксис Razor, который является более лаконичны, чем веб-форм обработчик представлений, используемый в предыдущих версиях ASP.NET MVC и веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="fc6d1-134">Обработчик представлений Web Forms по-прежнему доступен в ASP.NET MVC 3, но многим разработчикам кажутся представлений Razor действительно хорошо подходит разработки ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="fc6d1-135">Первые три строки установки с помощью ViewBag.Title заголовка страницы.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="fc6d1-136">Мы рассмотрим как это работает более подробно скоро, но сначала давайте обновления текст заголовка текста и просмотрите страницу.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="fc6d1-137">Обновление &lt;h2&gt; тег, чтобы сказать «это — домашняя страница», как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="fc6d1-138">Запуск приложения показано, что наш новый текст отображается на домашней странице.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="fc6d1-139">С использованием макета для общих элементов сайта</span><span class="sxs-lookup"><span data-stu-id="fc6d1-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="fc6d1-140">У большинства веб-сайтов содержимого, которое совместно используется много страниц: навигации, нижние колонтитулы, изображения логотипа, ссылки на таблицы стилей, и т. д. Представлений Razor делает это облегчает управление с помощью страница с именем \_Layout.cshtml, который автоматически был создан для нас расположен в папке/представления/Shared.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="fc6d1-141">Дважды щелкните эту папку для просмотра содержимого, в которой будут показаны ниже.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="fc6d1-142">Будет отображаться содержимое из наших отдельных представлений @RenderBodyкоманда () и любые типичного содержимого, мы хотим находиться за пределами, добавляемыми \_Layout.cshtml разметки.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="fc6d1-143">Будем нашей MVC Music Store могут иметь общие заголовок со ссылками нашей область домашней страницы и хранилища на всех страницах на сайте, и, будет добавлен в шаблон непосредственно над ней @RenderBodyинструкции ().</span><span class="sxs-lookup"><span data-stu-id="fc6d1-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="fc6d1-144">Обновление таблицы стилей</span><span class="sxs-lookup"><span data-stu-id="fc6d1-144">Updating the StyleSheet</span></span>

<span data-ttu-id="fc6d1-145">Шаблон пустого проекта содержит очень упрощенный CSS-файл, включающий только стили, используемые для отображения сообщений проверки.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="fc6d1-146">Наш конструктор обеспечивает некоторые дополнительные CSS и изображения для определяет внешний вид и поведение для наших сайта, поэтому мы добавим их сейчас.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="fc6d1-147">Обновленные CSS-файл и изображения включены в каталоге содержимого MvcMusicStore Assets.zip, находящейся на [MVC Music Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="fc6d1-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="fc6d1-148">Мы выберите оба параметра в проводнике Windows и разместить их в папку содержимого наши решения в Visual Web Developer, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="fc6d1-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="fc6d1-149">Вам будет предложено подтвердить, следует ли перезаписать существующий файл Site.css.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="fc6d1-150">Нажмите кнопку Да.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="fc6d1-151">Папки содержимого приложения теперь будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fc6d1-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="fc6d1-152">Теперь давайте запустить приложение и увидеть, как изменения будут отображаться на домашней странице.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="fc6d1-153">Рассмотрим, что изменено: метод действия индекса HomeController найден и отображаются шаблона \Views\Home\Index.cshtmlView, несмотря на то, что наш код называется «возвращаемого View()», поскольку нашей Просмотр шаблона, а затем стандартного соглашения об именовании.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="fc6d1-154">Домашняя страница отображает простой приветственное сообщение, которое определено в шаблоне \Views\Home\Index.cshtml представления.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="fc6d1-155">Использование домашней странице нашей \_Layout.cshtml шаблона, и поэтому приветственное сообщение, содержащихся в макет узла стандартный HTML.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="fc6d1-156">С помощью модели для передачи данных в нашем представление</span><span class="sxs-lookup"><span data-stu-id="fc6d1-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="fc6d1-157">Просмотр шаблона, который просто отображает жестко закодировано HTML не будет делать очень интересно веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="fc6d1-158">Для создания динамического веб-узла, мы вместо этого потребуется передачи данных из наших действий контроллера наших шаблонов представлений.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="fc6d1-159">В шаблон Model-View-Controller термин модели относится к объектов, которые представляют данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="fc6d1-160">Часто объекты модели соответствуют таблицам в базе данных, но это не обязательно.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="fc6d1-161">Методы действий контроллера, которые возвращают ActionResult можно передать объект модели представления.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="fc6d1-162">Это позволяет контроллеру четко пакета все данные, необходимые для создания ответа, а затем передать эти сведения для шаблона представления, используемый для создания соответствующих HTML-ответа.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="fc6d1-163">Это проще всего понять путем его просмотр в действии, так что давайте начнем.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="fc6d1-164">Сначала мы создадим некоторые классы модели для представления жанров и альбомам в нашем хранилище.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="fc6d1-165">Давайте начнем с создания класса жанра.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="fc6d1-166">Щелкните правой кнопкой мыши папку «Модели» в проекте, выберите параметр «Добавить класс» и присвойте файлу имя «Genre.cs».</span><span class="sxs-lookup"><span data-stu-id="fc6d1-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="fc6d1-167">Затем добавьте строку открытого свойства имени класса, который был создан:</span><span class="sxs-lookup"><span data-stu-id="fc6d1-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="fc6d1-168">*Примечание: В случае, вам интересно {get; задать;} нотации делает использование C# автоматически реализуемые свойства функции. Это дает преимущества свойства без необходимости нами для объявления резервным полем.*</span><span class="sxs-lookup"><span data-stu-id="fc6d1-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="fc6d1-169">Затем выполните эти шаги, чтобы создать класс альбома (с именем Album.cs), имеет заголовок и жанр свойство.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="fc6d1-170">Теперь можно изменять StoreController для использования представлений, отображающих динамические сведения из нашей модели.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="fc6d1-171">При - прямо сейчас - для демонстрационных целей мы назвали нашей альбомы, на основе идентификатора запроса, мы Показывать эти сведения, как в представлении ниже.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="fc6d1-172">Мы начнем, изменив действие сведения о хранилище для отображения сведений для одного альбома.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="fc6d1-173">Добавьте оператор «using» в начало **StoreControllers** класса для включения пространства имен MvcMusicStore.Models, поэтому мы не нужно вводить MvcMusicStore.Models.Album мы хотим использовать класс альбома.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="fc6d1-174">Должен появиться в разделе «директивы using» этого класса как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="fc6d1-175">Далее будет обновлена на действие Details контроллер, чтобы он возвращал ActionResult, а не строкой, как это делалось с помощью метода HomeController индекса.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="fc6d1-176">Теперь можно изменять логику для возвращения объекта альбом в представление.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="fc6d1-177">Далее в этом учебнике мы будем извлекать данные из базы данных, но в настоящий момент мы будем использовать «пустой данных», чтобы приступить к работе.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="fc6d1-178">*Примечание: Если вы знакомы с C#, вы могут предположить, что с помощью var означает, что наши переменной альбом позднего связывания. Неправильный — вывод типа на основе на то, что мы назначения переменной, чтобы определить, что этом альбоме имеет тип альбом и компиляция альбом локальной переменной как тип альбома, поэтому мы получаем проверки во время компиляции и в редакторе кода Visual Studio использует компилятор C# Поддержка.*</span><span class="sxs-lookup"><span data-stu-id="fc6d1-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="fc6d1-179">Теперь создадим представление шаблона, который использует нашей альбом для создания HTML-ответа.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="fc6d1-180">Перед этим необходимо построить проект, чтобы добавить представление диалогового окна известно о наших только что созданный класс альбома.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="fc6d1-181">Можно построить проект, выбрав Debug⇨Build MvcMusicStore элемента меню (дополнение, можно использовать сочетание клавиш Ctrl-Shift-B для построения проекта).</span><span class="sxs-lookup"><span data-stu-id="fc6d1-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="fc6d1-182">Мы настроили наши вспомогательные классы, мы готовы выполнить сборку шаблона наши представления.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="fc6d1-183">Щелкните правой кнопкой мыши внутри метода сведения и выберите пункт «Добавить представление...»</span><span class="sxs-lookup"><span data-stu-id="fc6d1-183">Right-click within the Details method and select "Add View…"</span></span> <span data-ttu-id="fc6d1-184">в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-184">from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="fc6d1-185">Мы собираемся создавать новый шаблон представления, как и раньше с HomeController.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-185">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="fc6d1-186">Поскольку мы создаем из StoreController он будет по умолчанию создаваться в файле \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-186">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="fc6d1-187">В отличие от ранее, мы собираемся представление флажок «Создать строго типизированной».</span><span class="sxs-lookup"><span data-stu-id="fc6d1-187">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="fc6d1-188">Затем мы собираемся выберите нами класса «Альбом» в «Класс представления данных —» drop-downlist.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-188">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="fc6d1-189">Это приведет к диалогового окна «Добавление представления», чтобы создать шаблон представления, который ожидает альбом объекта передается его для использования.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-189">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="fc6d1-190">При нажатии кнопки «Добавить» наши \Views\Store\Details.cshtml Просмотр шаблона создается, содержащий следующий код.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-190">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="fc6d1-191">Обратите внимание, первую строку, которая указывает, что данное представление является строго типизированный класс нашей альбома.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-191">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="fc6d1-192">Обработчик представлений Razor понимает, что он передан объект альбома, чтобы мы могли легко получить доступ к свойствам модели и даже воспользоваться преимуществами технологии IntelliSense в редакторе Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-192">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="fc6d1-193">Обновление &lt;h2&gt; тег, отображается свойство название альбома, изменив эту строку, чтобы выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-193">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="fc6d1-194">Обратите внимание, что IntelliSense запускается при вводе в течение периода времени @Model ключевое слово, показывающий свойства и методы, которые поддерживает класс альбома.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-194">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="fc6d1-195">Теперь давайте повторного запуска проекта и посетите URL-адрес хранилища, сведения и 5.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-195">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="fc6d1-196">Мы рассмотрим подробные сведения о альбом как ниже.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-196">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="fc6d1-197">Теперь мы выполним подобное обновление для метода действия Обзор хранилища.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-197">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="fc6d1-198">Обновить метод, чтобы он возвращал ActionResult и измените логику метода, он создает новый объект жанр и возвращает его в представление.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-198">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="fc6d1-199">Щелкните правой кнопкой мыши в методе обзора и выберите пункт «Добавить представление...»</span><span class="sxs-lookup"><span data-stu-id="fc6d1-199">Right-click in the Browse method and select "Add View…"</span></span> <span data-ttu-id="fc6d1-200">в контекстном меню, затем добавьте представление, которое является строго типизированной добавить строго типизированный класс жанра.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-200">from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="fc6d1-201">Обновление &lt;h2&gt; элемент в представлении кода (в /Views/Store/Browse.cshtml) для отображения сведений жанра.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-201">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="fc6d1-202">Теперь давайте повторного запуска проекта и найдите/Store/обзора? Жанр = Disco URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-202">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="fc6d1-203">Мы рассмотрим, как ниже отображается страница «Обзор».</span><span class="sxs-lookup"><span data-stu-id="fc6d1-203">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="fc6d1-204">Наконец, создадим немного более сложными, обновление **хранения индекса** метода действия и представления для отображения списка всех жанров в нашем хранилище.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-204">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="fc6d1-205">Сделаем с помощью список жанров как объект нашей модели, а не только одного жанра.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-205">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="fc6d1-206">Щелкните правой кнопкой мыши в методе действия индекс хранилища и выберите Добавить представление, как и прежде, выделите как класс модели и нажмите кнопку Добавить.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-206">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="fc6d1-207">Сначала мы изменим @model объявления, чтобы указать, что представление будет ожидать жанр несколько объектов вместо того, чтобы только один.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-207">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="fc6d1-208">Изменение в первой строке /Store/Index.cshtml следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fc6d1-208">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="fc6d1-209">Это значение определяет представлений Razor, он будет работать объекта модели, который может содержать несколько объектов жанра.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-209">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="fc6d1-210">Мы используем IEnumerable&lt;жанр&gt; вместо списка&lt;жанр&gt; так, как это более общий, позволяющего изменить тип нашей модели в более поздней версии на любой тип объекта, который поддерживает интерфейс IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-210">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="fc6d1-211">Далее предстоит перебор жанр объекты в модели, как показано в приведенном ниже примере кода завершенные.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-211">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="fc6d1-212">Обратите внимание, что у нас есть полную поддержку IntelliSense вводе данного кода, так что если мы введем «@Model.»</span><span class="sxs-lookup"><span data-stu-id="fc6d1-212">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="fc6d1-213">мы видим, что все методы и свойства, поддерживаемые IEnumerable типа жанра.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-213">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="fc6d1-214">В нашем «цикл» Visual Web Developer знает, каждый элемент имеет тип жанра, поэтому мы видим IntelliSence для каждого типа жанра.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-214">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="fc6d1-215">Затем функции формирования шаблонов проверить объект жанр и определить, что каждый будет иметь свойство Name, поэтому циклически и записывает их. Он также создает редактирования, подробные сведения и удаления ссылок для каждого отдельного элемента.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-215">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="fc6d1-216">Мы будем пользоваться, далее в нашем директор магазина, но сейчас мы предлагаем возможность простого списка.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-216">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="fc6d1-217">Когда мы запустите приложение и перейдите в "/ store", мы видим, отображается список жанров и count.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-217">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="fc6d1-218">Добавление ссылки между страницами</span><span class="sxs-lookup"><span data-stu-id="fc6d1-218">Adding Links between pages</span></span>

<span data-ttu-id="fc6d1-219">Наш "/ store" URL-адрес, в настоящее время список жанров список имен жанр просто в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-219">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="fc6d1-220">Изменим это, что вместо обычного текста вместо дает ссылку имена жанра, чтобы соответствующие хранилища или обзора URL-адрес, щелкнув жанр музыки, как произойдет переход «Disco» / Store/обзора? жанр = Disco URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-220">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="fc6d1-221">Следует обновить шаблон представления нашей \Views\Store\Index.cshtml выходные данные этих ссылок, с помощью кода как ниже **(не вводите в — нам улучшить на нем)**:</span><span class="sxs-lookup"><span data-stu-id="fc6d1-221">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="fc6d1-222">Это работает, но это может привести к более поздней версии проблемы, так как он основывается на строку.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-222">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="fc6d1-223">Для экземпляра Если требуется переименовать контроллер, необходимо поиск в наш код ищет ссылки, должны быть обновлены.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-223">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="fc6d1-224">Альтернативный подход, который может использоваться — воспользоваться преимуществом метода вспомогательный метод HTML.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-224">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="fc6d1-225">ASP.NET MVC включает методы вспомогательный метод HTML, которые доступны из наш код шаблона представления для выполнения различных задач, так же, как это.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-225">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="fc6d1-226">Вспомогательный метод Html.ActionLink() является особенно удобно и упрощает создание HTML &lt;&gt; ссылки, а также отвечает за нежелательные сведения, такие как всегда убеждаться в том пути URL-адрес правильно URL-кодированием.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-226">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="fc6d1-227">Html.ActionLink() имеет несколько перегрузок другой, чтобы обеспечить возможность указания столько информации, сколько необходимо для ссылок.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-227">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="fc6d1-228">В самом простом случае необходимо указать только текст ссылки и метод действия для перехода при щелчке гиперссылки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-228">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="fc6d1-229">Например мы можно связать с «/ Store /» Index() метода на странице сведения о хранилище с текстом ссылки «Перейти к хранилища индекса» с помощью следующего вызова.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-229">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="fc6d1-230">*Примечание: В этом случае нам не нужно указать имя контроллера, так как мы просто ссылка другого действия в одном контроллере, который готовится к просмотру текущего представления.*</span><span class="sxs-lookup"><span data-stu-id="fc6d1-230">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="fc6d1-231">Наш ссылки на странице «Обзор» необходимо передать параметр, однако, мы будем использовать другую перегрузку метода Html.ActionLink, который принимает три параметра:</span><span class="sxs-lookup"><span data-stu-id="fc6d1-231">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="fc6d1-232">Текст ссылки, который будет отображаться название жанра</span><span class="sxs-lookup"><span data-stu-id="fc6d1-232">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="fc6d1-233">Имя действия контроллера (Обзор)</span><span class="sxs-lookup"><span data-stu-id="fc6d1-233">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="fc6d1-234">Значения параметров маршрута, указав имя (жанр) и значение (жанр имя)</span><span class="sxs-lookup"><span data-stu-id="fc6d1-234">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="fc6d1-235">Усовершенствование, что все вместе, Вот каким образом, мы записываем эти связи в представление индекс хранилища:</span><span class="sxs-lookup"><span data-stu-id="fc6d1-235">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="fc6d1-236">Теперь если мы еще раз запустить нашего проекта и получить доступ к URL-адрес /Store/ будет рассмотрено список жанров.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-236">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="fc6d1-237">Каждом жанре представляет собой гиперссылку — при нажатии нам потребуется для наших/Store/обзора? жанр =*[жанр]* URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="fc6d1-237">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="fc6d1-238">HTML-код для списка жанр выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fc6d1-238">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


>[!div class="step-by-step"]
<span data-ttu-id="fc6d1-239">[Назад](mvc-music-store-part-2.md)
[Вперед](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="fc6d1-239">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
