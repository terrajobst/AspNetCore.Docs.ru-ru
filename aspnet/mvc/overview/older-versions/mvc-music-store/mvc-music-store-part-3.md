---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Часть 3: Views и ViewModels | Документация Майкрософт'
author: jongalloway
description: В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store. В части 3 описывается Views и ViewModels.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 8fd89c2a448877bf13a7828f545ffcd400f63bb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837414"
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="95d4b-104">Часть 3: Views и ViewModels</span><span class="sxs-lookup"><span data-stu-id="95d4b-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="95d4b-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="95d4b-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="95d4b-106">Store музыки MVC является учебного приложения, которое вводятся и описываются пошаговые использование ASP.NET MVC и Visual Studio для разработки веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="95d4b-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="95d4b-107">Store музыки MVC — это реализация хранилища упрощенный пример, который продает музыкальные альбомы через Интернет и реализует базовый сайт администрирования, вход пользователя и корзины для покупок функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="95d4b-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="95d4b-108">В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="95d4b-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="95d4b-109">В части 3 описывается Views и ViewModels.</span><span class="sxs-lookup"><span data-stu-id="95d4b-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="95d4b-110">Пока мы просто возврат строк из действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="95d4b-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="95d4b-111">Это отличный способ получить представление о том, как работают контроллеры, но это не будет способа построения реальных веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="95d4b-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="95d4b-112">Мы собираемся требуется более эффективный способ создания HTML-кода обратно в браузерах, посетите наш сайт – один где файлы шаблонов можно использовать для более легко настроить содержимое HTML обратной отправки.</span><span class="sxs-lookup"><span data-stu-id="95d4b-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="95d4b-113">Именно это сделать представления.</span><span class="sxs-lookup"><span data-stu-id="95d4b-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="95d4b-114">Добавление шаблона представления</span><span class="sxs-lookup"><span data-stu-id="95d4b-114">Adding a View template</span></span>

<span data-ttu-id="95d4b-115">Чтобы использовать шаблон представления, мы измените метод Index в HomeController для возврата ActionResult и он возвращал View(), например, ниже:</span><span class="sxs-lookup"><span data-stu-id="95d4b-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="95d4b-116">Указанное выше изменение означает, что вместо возвратил строку, вместо этого мы хотим использовать «Представление» для создания результата обратно.</span><span class="sxs-lookup"><span data-stu-id="95d4b-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="95d4b-117">Теперь мы добавим соответствующий шаблон представления для нашего проекта.</span><span class="sxs-lookup"><span data-stu-id="95d4b-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="95d4b-118">Для этого мы будем поместите текстовый курсор в методе действия индекса, а затем щелкните правой кнопкой мыши и выберите «Add View».</span><span class="sxs-lookup"><span data-stu-id="95d4b-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="95d4b-119">Откроется диалоговое окно Добавление представления:</span><span class="sxs-lookup"><span data-stu-id="95d4b-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="95d4b-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="95d4b-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="95d4b-121">Диалоговое окно «Добавление представления» позволяет нам быстро и легко создавать файлы шаблонов представления.</span><span class="sxs-lookup"><span data-stu-id="95d4b-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="95d4b-122">По умолчанию «Добавление представления» диалоговое окно предварительно заполняет имя шаблона представления для создания, чтобы он соответствовал метод действия, который будет его использовать.</span><span class="sxs-lookup"><span data-stu-id="95d4b-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="95d4b-123">Так как мы использовали контекстного меню «Добавление представления» в методе действия Index() наших HomeController, указанном выше диалоговом окне «Добавление представления» имеет «Index» как имя представления, предварительно заданные по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="95d4b-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="95d4b-124">Нам не нужно изменить какие-либо из параметров в этом диалоговом окне, поэтому нажмите кнопку «Добавить».</span><span class="sxs-lookup"><span data-stu-id="95d4b-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="95d4b-125">После нажатия кнопки «Добавить», Visual Web Developer создаст новый Index.cshtml, шаблон представления для нас в каталоге \Views\Home, создание папки в том случае, если еще не существует.</span><span class="sxs-lookup"><span data-stu-id="95d4b-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="95d4b-126">Имя и расположение файла «Index.cshtml» важна и следует за соглашения об именовании по умолчанию ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="95d4b-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="95d4b-127">Имя каталога, \Views\Home, соответствующий контроллер — которого совпадает с именем HomeController.</span><span class="sxs-lookup"><span data-stu-id="95d4b-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="95d4b-128">Имя шаблона представления, индекс, соответствующий метод действия контроллера, который будет применяться для отображения представления.</span><span class="sxs-lookup"><span data-stu-id="95d4b-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="95d4b-129">ASP.NET MVC позволяет избежать необходимости явно указывать имя или расположение шаблона представления, когда мы используем это соглашение об именовании, чтобы вернуть представление.</span><span class="sxs-lookup"><span data-stu-id="95d4b-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="95d4b-130">Он будет по умолчанию обрабатывать шаблон представления \Views\Home\Index.cshtml при написании кода, например, ниже в нашей HomeController:</span><span class="sxs-lookup"><span data-stu-id="95d4b-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="95d4b-131">Visual Web Developer создается и открывается шаблон представления «Index.cshtml», после того как мы щелкнули кнопку «Добавить» в диалоговом окне «Добавление представления».</span><span class="sxs-lookup"><span data-stu-id="95d4b-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="95d4b-132">Ниже приведены значения Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="95d4b-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="95d4b-133">Это представление используется синтаксис Razor, который является более лаконичен, чем подсистема просмотра веб-форм, используемых в веб-форм ASP.NET и предыдущих версиях ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="95d4b-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="95d4b-134">Подсистема просмотра веб-форм по-прежнему доступен в ASP.NET MVC 3, но многие разработчики считают представлений Razor действительно хорошо подходит разработки ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="95d4b-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="95d4b-135">Первые три строки задание заголовка страницы, с помощью ViewBag.Title.</span><span class="sxs-lookup"><span data-stu-id="95d4b-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="95d4b-136">Мы рассмотрим принцип более подробно скоро, но сначала давайте обновления текста заголовка текста и просмотра страницы.</span><span class="sxs-lookup"><span data-stu-id="95d4b-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="95d4b-137">Обновление &lt;h2&gt; тег, чтобы сказать «This — на домашней странице», как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="95d4b-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="95d4b-138">Выполняется это приложение показывает, что наш новый текст отображается на домашней странице.</span><span class="sxs-lookup"><span data-stu-id="95d4b-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="95d4b-139">С помощью макета для общих элементов сайта</span><span class="sxs-lookup"><span data-stu-id="95d4b-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="95d4b-140">У большинства веб-сайтов контента, который совместно используется много страниц: навигации, колонтитулов, изображения логотипа, ссылки на таблицы стилей, и т.д. Подсистема просмотра Razor можно с легкостью управлять с помощью страница с именем \_Layout.cshtml, который автоматически был создан для нас в папке/Views/Shared.</span><span class="sxs-lookup"><span data-stu-id="95d4b-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="95d4b-141">Дважды щелкните эту папку, чтобы просмотреть содержимое, которые приведены ниже.</span><span class="sxs-lookup"><span data-stu-id="95d4b-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="95d4b-142">Будет отображать содержимое из наших отдельных представлений @RenderBodyкоманды (), а также любого типичного содержимого, мы хотим находиться за пределами, могут добавляться к \_Layout.cshtml разметки.</span><span class="sxs-lookup"><span data-stu-id="95d4b-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="95d4b-143">Нам понадобятся Store музыки наших MVC должны быть распространенных заголовок со ссылками наших область домашней страницы и Store на всех страницах на сайте, поэтому мы добавим, к шаблону прямо над этой @RenderBodyоператор ().</span><span class="sxs-lookup"><span data-stu-id="95d4b-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="95d4b-144">Обновление таблицы стилей</span><span class="sxs-lookup"><span data-stu-id="95d4b-144">Updating the StyleSheet</span></span>

<span data-ttu-id="95d4b-145">Шаблон пустого проекта включает в себя очень упрощенный CSS-файл, включающий только стили, используемые для отображения сообщений проверки.</span><span class="sxs-lookup"><span data-stu-id="95d4b-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="95d4b-146">Наши конструктор предоставляет некоторые дополнительные CSS и изображений для определения внешнего вида и поведения, мы добавим в сейчас.</span><span class="sxs-lookup"><span data-stu-id="95d4b-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="95d4b-147">Обновленный файл CSS и изображения включаются в каталоге содержимого MvcMusicStore Assets.zip, доступный в [MVC-музыка-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="95d4b-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="95d4b-148">Мы выберите оба из них в обозревателе Windows и перетащить их в папку содержимого наше решение в Visual Web Developer, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="95d4b-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="95d4b-149">Вам будет предложено подтвердить, следует ли перезаписать существующий файл Site.css.</span><span class="sxs-lookup"><span data-stu-id="95d4b-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="95d4b-150">Нажмите "Да".</span><span class="sxs-lookup"><span data-stu-id="95d4b-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="95d4b-151">В папку содержимого приложения теперь будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="95d4b-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="95d4b-152">Теперь давайте запустите приложение и увидеть, как изменения будут выглядеть на домашней странице.</span><span class="sxs-lookup"><span data-stu-id="95d4b-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="95d4b-153">Давайте рассмотрим, что изменилось: метод действия Index в HomeController найден и отображается шаблон \Views\Home\Index.cshtmlView, несмотря на то, что наш код называется «return View()», поскольку наш шаблон представления, а затем стандартного соглашения об именовании.</span><span class="sxs-lookup"><span data-stu-id="95d4b-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="95d4b-154">Домашняя страница отображает простой приветственное сообщение, которое определено в шаблоне представления \Views\Home\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="95d4b-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="95d4b-155">На домашней странице используется наш \_Layout.cshtml шаблона, и поэтому содержится приветственное сообщение в формате HTML стандартный узла.</span><span class="sxs-lookup"><span data-stu-id="95d4b-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="95d4b-156">Использование модели для передачи данных нашего представления</span><span class="sxs-lookup"><span data-stu-id="95d4b-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="95d4b-157">Шаблон представления, который просто отображает жестко HTML не собираетесь заставить очень интересных веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="95d4b-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="95d4b-158">Для создания динамического веб-узла, мы вместо этого потребуется для передачи информации из наших действий контроллера с наших шаблонов представлений.</span><span class="sxs-lookup"><span data-stu-id="95d4b-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="95d4b-159">В шаблоне модель-представление-контроллер термин относится модели для объектов, которые представляют данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="95d4b-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="95d4b-160">Часто объекты модели соответствуют таблицам в базе данных, но им не пришлось.</span><span class="sxs-lookup"><span data-stu-id="95d4b-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="95d4b-161">Методы действий контроллера, которые возвращают ActionResult можно передать объект модели представления.</span><span class="sxs-lookup"><span data-stu-id="95d4b-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="95d4b-162">Это позволяет контроллеру четко пакета все сведения, необходимые для создания ответа, а затем передать эту информацию в шаблон представления, используемый для создания соответствующих HTML-ответа.</span><span class="sxs-lookup"><span data-stu-id="95d4b-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="95d4b-163">Это самый простой для понимания, просмотреть его в действии, Давайте приступим к.</span><span class="sxs-lookup"><span data-stu-id="95d4b-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="95d4b-164">Сначала мы создадим некоторые классы модели, чтобы представлять жанры и альбомов в нашем магазине.</span><span class="sxs-lookup"><span data-stu-id="95d4b-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="95d4b-165">Давайте начнем с создания класса жанра.</span><span class="sxs-lookup"><span data-stu-id="95d4b-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="95d4b-166">Щелкните правой кнопкой мыши папку «Модели» в проекте, выберите параметр «Добавить класс» и назовите файл «Genre.cs».</span><span class="sxs-lookup"><span data-stu-id="95d4b-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="95d4b-167">Затем добавьте строку открытого свойства Name в класс, который был создан:</span><span class="sxs-lookup"><span data-stu-id="95d4b-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="95d4b-168">*Примечание: В случае, вам интересно {get; set;} нотации, делает использование C# автоматически реализуемые свойства функции. Мы получаем преимуществ свойства без необходимости нам для объявления резервным полем.*</span><span class="sxs-lookup"><span data-stu-id="95d4b-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="95d4b-169">Затем выполните те же действия, чтобы создать класс Album (с именем Album.cs), имеет название и жанр свойство:</span><span class="sxs-lookup"><span data-stu-id="95d4b-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="95d4b-170">Теперь мы можем изменить StoreController использовать представления, отображающие динамических сведений из нашей модели.</span><span class="sxs-lookup"><span data-stu-id="95d4b-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="95d4b-171">Если параметр - прямо сейчас - для демонстрационных целей мы назвали наш альбомов, на основании идентификатора запроса, мы удалось отобразить эту информацию, как показано в представлении ниже.</span><span class="sxs-lookup"><span data-stu-id="95d4b-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="95d4b-172">Мы начнем, изменив на действие Store Details, поэтому он отображает сведения об одном альбома.</span><span class="sxs-lookup"><span data-stu-id="95d4b-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="95d4b-173">Добавьте оператор «using» в верхней части **StoreControllers** класса для включения пространства имен MvcMusicStore.Models, поэтому нам не нужно вводить MvcMusicStore.Models.Album мы хотим использовать класс альбома.</span><span class="sxs-lookup"><span data-stu-id="95d4b-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="95d4b-174">Раздел «директивы using» этого класса теперь должен выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="95d4b-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="95d4b-175">После этого мы обновим действие контроллера Details, чтобы он возвращал ActionResult, а не строку, как мы сделали с метод HomeController Index.</span><span class="sxs-lookup"><span data-stu-id="95d4b-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="95d4b-176">Теперь можно изменить логику, чтобы возвратить объект альбома в представление.</span><span class="sxs-lookup"><span data-stu-id="95d4b-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="95d4b-177">Далее в этом руководстве мы будем извлекать данные из базы данных, но прямо сейчас мы будем использовать «пустой данных» Чтобы приступить к работе.</span><span class="sxs-lookup"><span data-stu-id="95d4b-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="95d4b-178">*Примечание: Если вы не знакомы с C#, вы может предположить, что использование функции var означает, что нашей переменной альбом с поздним связыванием. Неправильный — компилятор C# использует определение типа на основе что мы назначении переменной для определения этого альбома имеет тип альбома и компиляции альбома локальной переменной как типа альбома, поэтому мы получаем проверки во время компиляции и редактор кода Visual Studio Поддержка.*</span><span class="sxs-lookup"><span data-stu-id="95d4b-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="95d4b-179">Теперь создадим представление шаблона, который использует наши альбома для создания HTML-ответа.</span><span class="sxs-lookup"><span data-stu-id="95d4b-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="95d4b-180">Перед этим необходимо построить проект, чтобы добавить представление диалогового окна знал о наших только что созданный класс альбома.</span><span class="sxs-lookup"><span data-stu-id="95d4b-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="95d4b-181">Можно построить проект, выбрав Debug⇨Build MvcMusicStore пункта меню (дополнение, можно использовать сочетание клавиш Ctrl + Shift + B для сборки проекта).</span><span class="sxs-lookup"><span data-stu-id="95d4b-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="95d4b-182">Теперь, когда мы настроили наш вспомогательные классы, мы готовы создать наш шаблон представления.</span><span class="sxs-lookup"><span data-stu-id="95d4b-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="95d4b-183">Щелкните правой кнопкой мыши в методе, сведения и выберите «Добавить представление...» в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="95d4b-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="95d4b-184">Мы собираемся создать новый шаблон представления, как и раньше с HomeController.</span><span class="sxs-lookup"><span data-stu-id="95d4b-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="95d4b-185">Поскольку мы создаем из StoreController его по умолчанию создается в файле \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="95d4b-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="95d4b-186">В отличие от ранее, мы собираемся установите флажок «Создать строго типизированный» представление.</span><span class="sxs-lookup"><span data-stu-id="95d4b-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="95d4b-187">Затем мы собираемся выберите наш класс «Альбом» в «Представление данных класса» drop-downlist.</span><span class="sxs-lookup"><span data-stu-id="95d4b-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="95d4b-188">В результате диалогового окна «Добавление представления», чтобы создать шаблон представления, который ожидает, что альбом объекта передается его для использования.</span><span class="sxs-lookup"><span data-stu-id="95d4b-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="95d4b-189">При нажатии кнопки «Добавить» наши \Views\Store\Details.cshtml шаблона представления создается, содержит следующий код.</span><span class="sxs-lookup"><span data-stu-id="95d4b-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="95d4b-190">Обратите внимание, что первая строка, которая указывает, что данное представление со строгой типизацией к нашему классу альбома.</span><span class="sxs-lookup"><span data-stu-id="95d4b-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="95d4b-191">Подсистема просмотра Razor понимает, что он передан объект "альбом", чтобы мы могли легко получить доступ к свойствам модели и даже можете использовать IntelliSense в редакторе Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="95d4b-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="95d4b-192">Обновление &lt;h2&gt; тег, чтобы оно отображало свойство Title альбома, изменив эту строку, чтобы выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="95d4b-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="95d4b-193">Обратите внимание на то, что IntelliSense запускается при вводе поставить точку после @Model ключевое слово, показывающий свойства и методы, которые поддерживает класс альбома.</span><span class="sxs-lookup"><span data-stu-id="95d4b-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="95d4b-194">Давайте теперь запустите наш проект и посетите URL-адрес сведений/Store/5.</span><span class="sxs-lookup"><span data-stu-id="95d4b-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="95d4b-195">Подробнее см. сведения о альбом как ниже.</span><span class="sxs-lookup"><span data-stu-id="95d4b-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="95d4b-196">Теперь сделаем подобное обновление для метода действия Обзор Store.</span><span class="sxs-lookup"><span data-stu-id="95d4b-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="95d4b-197">Обновите метод, чтобы он возвращал ActionResult и изменить логику метода, поэтому он создает новый объект жанр и возвращает его в представление.</span><span class="sxs-lookup"><span data-stu-id="95d4b-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="95d4b-198">Щелкните правой кнопкой мыши в методе обзора и выберите «Добавить представление...» в контекстном меню, затем добавьте представление, которое является строго типизированным добавьте строго типизированный класс жанра.</span><span class="sxs-lookup"><span data-stu-id="95d4b-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="95d4b-199">Обновление &lt;h2&gt; элемент в представлении кода (в /Views/Store/Browse.cshtml) для отображения сведений жанра.</span><span class="sxs-lookup"><span data-stu-id="95d4b-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="95d4b-200">Теперь давайте повторно запустите нашего проекта и перейдите к/Store/обзора? Жанр = Disco URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="95d4b-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="95d4b-201">Мы увидим под отображается как странице "Обзор".</span><span class="sxs-lookup"><span data-stu-id="95d4b-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="95d4b-202">Наконец, создадим немного более сложная операция обновления для **Store индекс** метода действия и представление для отображения списка всех жанров в нашем магазине.</span><span class="sxs-lookup"><span data-stu-id="95d4b-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="95d4b-203">Сделаем с помощью список жанров как наш объект модели, а не просто единый жанр фильма.</span><span class="sxs-lookup"><span data-stu-id="95d4b-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="95d4b-204">Щелкните правой кнопкой мыши в методе действия индекса Store и выберите Add View, так как перед, выделите как класс модели и нажмите кнопку "Добавить".</span><span class="sxs-lookup"><span data-stu-id="95d4b-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="95d4b-205">Сначала мы изменим @model объявление, чтобы указать, что представление будет ожидать несколько жанр объектов вместо одного.</span><span class="sxs-lookup"><span data-stu-id="95d4b-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="95d4b-206">Изменение в первой строке /Store/Index.cshtml следующим образом:</span><span class="sxs-lookup"><span data-stu-id="95d4b-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="95d4b-207">Это сообщает представлений Razor, он будет работать с помощью модели объекта, который может содержать несколько объектов жанра.</span><span class="sxs-lookup"><span data-stu-id="95d4b-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="95d4b-208">Мы используем IEnumerable&lt;жанр&gt; вместо списка&lt;жанр&gt; , так как он является более универсальным, что позволило нам изменить тип нашей модели в более поздней версии на любой тип объекта, который поддерживает интерфейс IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="95d4b-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="95d4b-209">Далее мы будем цикла по жанру объектам в модели, как показано в приведенном ниже коде завершенные.</span><span class="sxs-lookup"><span data-stu-id="95d4b-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="95d4b-210">Обратите внимание на то, что у нас есть полностью поддерживается технология IntelliSense вводе этот код таким образом, когда мы введите "@Model.»</span><span class="sxs-lookup"><span data-stu-id="95d4b-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="95d4b-211">мы видим, все методы и свойства, поддерживаемые IEnumerable типа жанра.</span><span class="sxs-lookup"><span data-stu-id="95d4b-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="95d4b-212">Внутри нашего цикла «foreach» Visual Web Developer знает, что каждый элемент имеет тип жанра, поэтому мы видим IntelliSence для каждого типа жанра.</span><span class="sxs-lookup"><span data-stu-id="95d4b-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="95d4b-213">Затем компонент формирования шаблонов проверить объект жанр и установлено, что каждый будет свойство Name, поэтому он перебирает и записывает их. Она также создает ссылки Edit, сведения и Delete для каждого отдельного элемента.</span><span class="sxs-lookup"><span data-stu-id="95d4b-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="95d4b-214">Мы будем пользоваться, далее в наш Менеджер магазина, но сейчас мы бы хотели иметь вместо простой список.</span><span class="sxs-lookup"><span data-stu-id="95d4b-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="95d4b-215">Когда мы запустите приложение и перейдите к/Store, мы видим, что отображается число и список жанров.</span><span class="sxs-lookup"><span data-stu-id="95d4b-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="95d4b-216">Добавление ссылок между страницами</span><span class="sxs-lookup"><span data-stu-id="95d4b-216">Adding Links between pages</span></span>

<span data-ttu-id="95d4b-217">Наш URL/Store со списком жанров в настоящее время список имен жанр просто в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="95d4b-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="95d4b-218">Изменим его, чтобы вместо обычного текста вместо этого оставить ссылку имена жанр соответствующий Store или просмотра URL-адрес, таким образом, щелкнув жанр музыки, как производится переход «Disco» / Store/обзора? жанр = Disco URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="95d4b-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="95d4b-219">Следует обновить шаблон представления \Views\Store\Index.cshtml выходные данные этих ссылок, с помощью кода как ниже **(не ввести этот текст — мы собираемся над ними)**:</span><span class="sxs-lookup"><span data-stu-id="95d4b-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="95d4b-220">Это работает, но это может привести к проблемы в более поздней версии, поскольку он основан на строку.</span><span class="sxs-lookup"><span data-stu-id="95d4b-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="95d4b-221">Например если нам нужно переименовать контроллер, мы бы нужно выполнить поиск по наш код, который ищет ссылки, должны быть обновлены.</span><span class="sxs-lookup"><span data-stu-id="95d4b-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="95d4b-222">Альтернативный подход, который мы можем использовать — воспользоваться методом вспомогательный метод HTML.</span><span class="sxs-lookup"><span data-stu-id="95d4b-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="95d4b-223">ASP.NET MVC включает методы вспомогательный метод HTML, которые доступны из наш код шаблона представления для выполнения различных задач, так же, как это.</span><span class="sxs-lookup"><span data-stu-id="95d4b-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="95d4b-224">Вспомогательный метод Html.ActionLink() является особенно полезен и позволяет легко создавать HTML &lt;&gt; ссылкой и берет на себя раздражает такие сведения, как убедиться, пути URL-адресов осуществляется должным образом в кодировке URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="95d4b-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="95d4b-225">Html.ActionLink() имеет несколько различных перегрузок, позволяющий указывать столько информации, который необходим для ссылки.</span><span class="sxs-lookup"><span data-stu-id="95d4b-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="95d4b-226">В самом простом случае нужно будет указать только текст ссылки и метод действия для перехода при щелчке по гиперссылке на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="95d4b-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="95d4b-227">Например мы можно связать с «/ Store /» метод Index() на странице сведений Store с помощью текст ссылки «Перейдите к Store индекс» с помощью следующего вызова:</span><span class="sxs-lookup"><span data-stu-id="95d4b-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="95d4b-228">*Примечание: В этом случае нам не нужно указать имя контроллера, так как мы просто ссылка другое действие, в том же контроллере, обрабатывающей текущего представления.*</span><span class="sxs-lookup"><span data-stu-id="95d4b-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="95d4b-229">Наши ссылки на странице "Обзор" потребуется для передачи параметра, однако, поэтому мы будем использовать другую перегрузку метода Html.ActionLink, который принимает три параметра:</span><span class="sxs-lookup"><span data-stu-id="95d4b-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="95d4b-230">Текст ссылки, который будет отображаться название жанра</span><span class="sxs-lookup"><span data-stu-id="95d4b-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="95d4b-231">Имя действия контроллера (Обзор)</span><span class="sxs-lookup"><span data-stu-id="95d4b-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="95d4b-232">Значения параметров маршрута, указав имя (жанр) и значение (название жанра)</span><span class="sxs-lookup"><span data-stu-id="95d4b-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="95d4b-233">Усовершенствование, что все вместе, вот как будет написан этих ссылок в представление Store Index:</span><span class="sxs-lookup"><span data-stu-id="95d4b-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="95d4b-234">Теперь, когда мы снова запустить наш проект и указать URL-адрес /Store/ мы увидим список жанров.</span><span class="sxs-lookup"><span data-stu-id="95d4b-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="95d4b-235">Каждого жанра является гиперссылкой — при щелчке нам потребуется для наших/Store/обзора? жанр =*[жанр]* URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="95d4b-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="95d4b-236">HTML-код для списка жанр выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="95d4b-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="95d4b-237">[Назад](mvc-music-store-part-2.md)
> [Вперед](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="95d4b-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
