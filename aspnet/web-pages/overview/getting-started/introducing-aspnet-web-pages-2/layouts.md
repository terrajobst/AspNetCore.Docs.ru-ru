---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Введение в ASP.NET Web Pages — Создание согласованного макета | Документы Microsoft
author: tfitzmac
description: Этого учебника показано, как использовать макеты для создания согласованного вида для страниц на сайте, который использует веб-страниц ASP.NET. Предполагается, что завершена...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: c2d5c4d8ed8a71979c16d484ab90d283a45de537
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="9888f-104">Общие сведения о веб-страницах ASP.NET - Создание согласованного макета</span><span class="sxs-lookup"><span data-stu-id="9888f-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="9888f-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9888f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9888f-106">Этого учебника показано, как использовать *макеты* Создание согласованного вида для страниц на сайте, который использует веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9888f-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="9888f-107">Предполагается, что завершена ряда через [удаление базы данных в веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="9888f-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="9888f-108">Что вы узнаете следующее.</span><span class="sxs-lookup"><span data-stu-id="9888f-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="9888f-109">Макет страницы равен.</span><span class="sxs-lookup"><span data-stu-id="9888f-109">What a layout page is.</span></span>
> - <span data-ttu-id="9888f-110">Использование страницы макета с динамическим содержимым.</span><span class="sxs-lookup"><span data-stu-id="9888f-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="9888f-111">Инструкции по передаче значений к странице макета.</span><span class="sxs-lookup"><span data-stu-id="9888f-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="9888f-112">О макетах</span><span class="sxs-lookup"><span data-stu-id="9888f-112">About Layouts</span></span>

<span data-ttu-id="9888f-113">Страницы, созданные до сих была завершена, автономные страницы.</span><span class="sxs-lookup"><span data-stu-id="9888f-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="9888f-114">Все они принадлежат тому же сайту, но они не имеют общих элементов или стандартный вид.</span><span class="sxs-lookup"><span data-stu-id="9888f-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="9888f-115">У большинства сайтов согласованного внешнего вида и макета.</span><span class="sxs-lookup"><span data-stu-id="9888f-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="9888f-116">Например, если перейти к [Microsoft.com/web](https://www.microsoft.com/web/) сайта и поиска, вы видите, все страницы отвечали требованиям для общей структуры и визуальную тему:</span><span class="sxs-lookup"><span data-stu-id="9888f-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Структуры заголовка, области навигации и область содержимого, а также нижний колонтитул страницы сайта Microsoft.com/Web](layouts/_static/image1.png)

<span data-ttu-id="9888f-118">*Неэффективным* способ создания для этой структуры можно определить заголовок, панель навигации и нижний колонтитул отдельно для каждой из страниц.</span><span class="sxs-lookup"><span data-stu-id="9888f-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="9888f-119">При этом произойдет дублирование же разметки каждый раз.</span><span class="sxs-lookup"><span data-stu-id="9888f-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="9888f-120">Если требуется какое-либо изменение (например, обновление нижний колонтитул), необходимо изменить каждую страницу отдельно.</span><span class="sxs-lookup"><span data-stu-id="9888f-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="9888f-121">Именно так *страницы макета* бывают.</span><span class="sxs-lookup"><span data-stu-id="9888f-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="9888f-122">Веб-страницах ASP.NET, можно определить макет страницу, которая предоставляет общий контейнер для страниц веб-узла.</span><span class="sxs-lookup"><span data-stu-id="9888f-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="9888f-123">Например страницы макета может содержать заголовок, область навигации и нижний колонтитул.</span><span class="sxs-lookup"><span data-stu-id="9888f-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="9888f-124">Страница макета имеется заполнитель, где становится основным содержимым.</span><span class="sxs-lookup"><span data-stu-id="9888f-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="9888f-125">Затем можно определить отдельные страницы содержимого, которые содержат разметку и код для только к этой странице.</span><span class="sxs-lookup"><span data-stu-id="9888f-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="9888f-126">Страницы содержимого не обязательно должны быть полный HTML-страницы; даже не иметь `<body>` элемента.</span><span class="sxs-lookup"><span data-stu-id="9888f-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="9888f-127">Они также имеют строки кода, который сообщает ASP.NET, какие страницы макета для отображения содержимого.</span><span class="sxs-lookup"><span data-stu-id="9888f-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="9888f-128">Вот рисунок, приблизительно показано, как работает эта связь.</span><span class="sxs-lookup"><span data-stu-id="9888f-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Концептуальная схема, показывающая две страницы содержимого, а страница макета, в который они помещаются](layouts/_static/image2.png)

<span data-ttu-id="9888f-130">Это взаимодействие можно легко понять, когда вы видите в действии.</span><span class="sxs-lookup"><span data-stu-id="9888f-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="9888f-131">В этом учебнике вы измените макет страницы фильмов.</span><span class="sxs-lookup"><span data-stu-id="9888f-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="9888f-132">Добавление страницы макета</span><span class="sxs-lookup"><span data-stu-id="9888f-132">Adding a Layout Page</span></span>

<span data-ttu-id="9888f-133">Вы начнете путем создания макета страницы, которая определяет макет типичная страница с заголовком, нижний колонтитул и область основного содержимого.</span><span class="sxs-lookup"><span data-stu-id="9888f-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="9888f-134">На сайте WebPagesMovies добавить страницу с именем CSHTML  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9888f-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="9888f-135">Символа подчеркивания ( `_` ) символов имеет значение.</span><span class="sxs-lookup"><span data-stu-id="9888f-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="9888f-136">Если имя страницы начинается с символа подчеркивания, ASP.NET не будет отправлять непосредственно этой страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="9888f-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="9888f-137">Это соглашение можно определить страниц, которые необходимы для веб-узла, но пользователи не должны иметь возможность напрямую запрашивать.</span><span class="sxs-lookup"><span data-stu-id="9888f-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="9888f-138">Замените содержимое на странице следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="9888f-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="9888f-139">Как видите, эта разметка является только HTML, который использует `<div>` элементов для определения трех разделов в странице, а также один более `<div>` элемента для хранения трех разделов.</span><span class="sxs-lookup"><span data-stu-id="9888f-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="9888f-140">Нижний колонтитул содержит большой объем кода Razor: `@DateTime.Now.Year`, отображающего положение на странице текущего года.</span><span class="sxs-lookup"><span data-stu-id="9888f-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="9888f-141">Обратите внимание, что ссылка на таблицу стилей с именем *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="9888f-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="9888f-142">Таблица стилей находится где будут определены сведения физическим размещением элементов.</span><span class="sxs-lookup"><span data-stu-id="9888f-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="9888f-143">Вы должны создать позже.</span><span class="sxs-lookup"><span data-stu-id="9888f-143">You'll create that in a moment.</span></span>

<span data-ttu-id="9888f-144">Единственными необычными функция в этом  *\_Layout.cshtml* страница `@Render.Body()` строки.</span><span class="sxs-lookup"><span data-stu-id="9888f-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="9888f-145">Это заполнитель, куда содержимое при слиянии этот макет с другой страницы.</span><span class="sxs-lookup"><span data-stu-id="9888f-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="9888f-146">Добавление CSS-файла</span><span class="sxs-lookup"><span data-stu-id="9888f-146">Adding a .css File</span></span>

<span data-ttu-id="9888f-147">Предпочтительный способ определить фактическое расположение (то есть внешний) элементов на странице — использовать правила таблицы каскадных стилей (CSS).</span><span class="sxs-lookup"><span data-stu-id="9888f-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="9888f-148">Поэтому вы создадите *.css* файл, который имеет правила для нового макета.</span><span class="sxs-lookup"><span data-stu-id="9888f-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="9888f-149">В WebMatrix выберите корневой каталог узла.</span><span class="sxs-lookup"><span data-stu-id="9888f-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="9888f-150">Затем в **файлы** вкладку на ленте, щелкните стрелку под кнопкой **New** и щелкните **новую папку**.</span><span class="sxs-lookup"><span data-stu-id="9888f-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![Параметр «Новая папка» на ленте в группе Создание.](layouts/_static/image3.png)

<span data-ttu-id="9888f-152">Имя новой папки *стили*.</span><span class="sxs-lookup"><span data-stu-id="9888f-152">Name the new folder *Styles*.</span></span>

![Именование в новую папку «Стили»](layouts/_static/image4.png)

<span data-ttu-id="9888f-154">Внутри новой *стили* папки, создайте файл с именем *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="9888f-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Создание нового файла Movies.css](layouts/_static/image5.png)

<span data-ttu-id="9888f-156">Замените содержимое нового *.css* файл со следующим:</span><span class="sxs-lookup"><span data-stu-id="9888f-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="9888f-157">Мы не очень много говорит о эти правила CSS, за исключением того, чтобы отметить следующее.</span><span class="sxs-lookup"><span data-stu-id="9888f-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="9888f-158">Один заключается наряду с определением шрифты и размеры, правила использования абсолютное позиционирование для определения местоположения заголовка, нижнего колонтитула и область основного содержимого.</span><span class="sxs-lookup"><span data-stu-id="9888f-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="9888f-159">Если вы не знакомы с позиционирования в CSS, можно прочитать [позиционирования CSS](http://www.w3schools.com/css/css_positioning.asp) учебника по на сайте W3Schools.</span><span class="sxs-lookup"><span data-stu-id="9888f-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="9888f-160">Другие важно отметить, что в нижней, мы скопировали правила стилей, которые были изначально определяется по отдельности в *Movies.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="9888f-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="9888f-161">Эти правила были использованы в [Общие сведения о отображение данных с помощью веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251580) учебника, чтобы сделать `WebGrid` вспомогательный отображают разметку, которая добавляется в таблицу полосы.</span><span class="sxs-lookup"><span data-stu-id="9888f-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="9888f-162">(Если вы собираетесь использовать *.css* файл для определения стилей может также поместить правила стилей для всего сайта в нем.)</span><span class="sxs-lookup"><span data-stu-id="9888f-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="9888f-163">Обновление файла фильмы используется режим</span><span class="sxs-lookup"><span data-stu-id="9888f-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="9888f-164">Теперь можно обновить существующие файлы в узле, новый макет.</span><span class="sxs-lookup"><span data-stu-id="9888f-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="9888f-165">Откройте *Movies.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="9888f-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="9888f-166">В начале в первой строке кода, добавьте следующие строки:</span><span class="sxs-lookup"><span data-stu-id="9888f-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="9888f-167">Страница теперь начинается выглядеть так:</span><span class="sxs-lookup"><span data-stu-id="9888f-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="9888f-168">Это одной строки кода сообщает ASP.NET, что при *фильмов* страница выполняется, должны сливаться с  *\_Layout.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="9888f-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="9888f-169">Поскольку *Movies.cshtml* использует страницы макета, можно удалить разметка из *Movies.cshtml* страницу, которая занимается  *\_Layout.cshtml*файла.</span><span class="sxs-lookup"><span data-stu-id="9888f-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="9888f-170">Извлеките `<!DOCTYPE>`, `<html>`, и `<body>` открывающих и закрывающих тегов.</span><span class="sxs-lookup"><span data-stu-id="9888f-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="9888f-171">Извлеките всего `<head>` элемент и его содержимое, включая правила стилей для сетки, так как у вас теперь есть этих правил *.css* файла.</span><span class="sxs-lookup"><span data-stu-id="9888f-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="9888f-172">Пока вы его, измените существующий `<h1>` элемент `<h2>` элемент; Однако имеется `<h1>` элемента уже макета страницы.</span><span class="sxs-lookup"><span data-stu-id="9888f-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="9888f-173">Изменение `<h2>` текст «Список фильмов».</span><span class="sxs-lookup"><span data-stu-id="9888f-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="9888f-174">Как правило не нужно сделать эти виды изменений на странице содержимого.</span><span class="sxs-lookup"><span data-stu-id="9888f-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="9888f-175">При запуске веб-узла с странице макета страницы содержимого без этих элементов создания начинается с.</span><span class="sxs-lookup"><span data-stu-id="9888f-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="9888f-176">В этом случае, преобразовании отдельной странице на ту, которая использует структуру, поэтому бит очистки.</span><span class="sxs-lookup"><span data-stu-id="9888f-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="9888f-177">Закончив, *Movies.cshtml* страница будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9888f-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="9888f-178">Тестирование макета</span><span class="sxs-lookup"><span data-stu-id="9888f-178">Testing the Layout</span></span>

<span data-ttu-id="9888f-179">Теперь можно увидеть, как выглядит макет.</span><span class="sxs-lookup"><span data-stu-id="9888f-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="9888f-180">В WebMatrix, щелкните правой кнопкой мыши *Movies.cshtml* и выберите **запуск в браузере**.</span><span class="sxs-lookup"><span data-stu-id="9888f-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="9888f-181">Когда браузер отображает страницу, он выглядит следующим образом эту страницу.</span><span class="sxs-lookup"><span data-stu-id="9888f-181">When the browser displays the page, it looks like this page:</span></span>

![Страница фильмы, отображается с использованием макета](layouts/_static/image6.png)

<span data-ttu-id="9888f-183">ASP.NET было выполнено слияние содержимого страницы Movies.cshtml в  *\_Layout.cshtml* страницы справа где `RenderBody` метод.</span><span class="sxs-lookup"><span data-stu-id="9888f-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="9888f-184">И конечно  *\_Layout.cshtml* страницы ссылки *.css* файл, который определяет внешний вид страницы.</span><span class="sxs-lookup"><span data-stu-id="9888f-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="9888f-185">Обновление страницы AddMovie используется режим</span><span class="sxs-lookup"><span data-stu-id="9888f-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="9888f-186">Реальное преимущество макетов является можно использовать их для всех страниц на сайте.</span><span class="sxs-lookup"><span data-stu-id="9888f-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="9888f-187">Откройте *AddMovie.cshtml* страницы.</span><span class="sxs-lookup"><span data-stu-id="9888f-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="9888f-188">Вы помните, что *AddMovie.cshtml* страницы содержалась ранее некоторые правила CSS для определения внешнего вида сообщений об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="9888f-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="9888f-189">Поскольку у вас *.css* файл свой сайт прямо сейчас, можно переместить эти правила для *.css* файла.</span><span class="sxs-lookup"><span data-stu-id="9888f-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="9888f-190">Удалить их из *AddMovie.cshtml* и добавьте их в конец файла *Movies.css* файла.</span><span class="sxs-lookup"><span data-stu-id="9888f-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="9888f-191">Перемещении следующие правила:</span><span class="sxs-lookup"><span data-stu-id="9888f-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="9888f-192">Теперь внести изменения в со схожими *AddMovie.cshtml* , что и для *Movies.cshtml* — добавить `Layout="~/_Layout.cshtml;` и удалите разметки HTML, который теперь является лишним.</span><span class="sxs-lookup"><span data-stu-id="9888f-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="9888f-193">Изменение `<h1>` элемент `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="9888f-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="9888f-194">Когда закончите, страница будет выглядеть как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="9888f-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="9888f-195">Запустите страницу.</span><span class="sxs-lookup"><span data-stu-id="9888f-195">Run the page.</span></span> <span data-ttu-id="9888f-196">Теперь он будет выглядеть на этом рисунке:</span><span class="sxs-lookup"><span data-stu-id="9888f-196">Now it looks like this illustration:</span></span>

![«Добавить фильмов» страницы к просмотру с использованием макета](layouts/_static/image7.png)

<span data-ttu-id="9888f-198">Вы хотите внести аналогичные изменения страниц на сайте — *EditMovie.cshtml* и *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9888f-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="9888f-199">Тем не менее прежде чем выполнять другое изменение можно внести, делающую немного более гибкий макет.</span><span class="sxs-lookup"><span data-stu-id="9888f-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="9888f-200">Передача сведений об заголовка для страницы макета</span><span class="sxs-lookup"><span data-stu-id="9888f-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="9888f-201"> *\_Layout.cshtml* имеет созданную страницу `<title>` элемент, который имеет значение «Мой сайт фильма».</span><span class="sxs-lookup"><span data-stu-id="9888f-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="9888f-202">Большинство браузеров для отображения текста на вкладке содержимого данного элемента:</span><span class="sxs-lookup"><span data-stu-id="9888f-202">Most browsers display the content of this element as the text on a tab:</span></span>

![Страницы &lt;заголовок&gt; элемента, отображаемого на вкладке браузера](layouts/_static/image8.png)

<span data-ttu-id="9888f-204">Эти сведения заголовка является универсальным.</span><span class="sxs-lookup"><span data-stu-id="9888f-204">This title information is generic.</span></span> <span data-ttu-id="9888f-205">Предположим, что вы хотите текст заголовка точнее к текущей странице.</span><span class="sxs-lookup"><span data-stu-id="9888f-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="9888f-206">(Текст заголовка также используется поисковыми системами для определения, является о страницы.) Можно передать сведения из страницы содержимого, такие как *Movies.cshtml* или *AddMovie.cshtml* для страницы макета, а затем использовать эту информацию для настройки макета страницы визуализирует.</span><span class="sxs-lookup"><span data-stu-id="9888f-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="9888f-207">Откройте *Movies.cshtml* страницу еще раз.</span><span class="sxs-lookup"><span data-stu-id="9888f-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="9888f-208">В коде в верхней добавьте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="9888f-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="9888f-209">`Page` Объект будет доступен для всех *.cshtml* страницы, а также является для этой цели, а именно: для обмена данными между страницей и его макет.</span><span class="sxs-lookup"><span data-stu-id="9888f-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="9888f-210">Откройте<em>\_Layout.cshtml</em> страницы.</span><span class="sxs-lookup"><span data-stu-id="9888f-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="9888f-211">Изменение `<title>` элемент, так что он выглядит как эта разметка:</span><span class="sxs-lookup"><span data-stu-id="9888f-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="9888f-212">Этот код отображает содержимое `Page.Title` свойство прямо в это расположение в странице.</span><span class="sxs-lookup"><span data-stu-id="9888f-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="9888f-213">Запустите *Movies.cshtml* страницы.</span><span class="sxs-lookup"><span data-stu-id="9888f-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="9888f-214">Это время вкладка «Обозреватель» содержится передается как значение `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="9888f-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Вкладка «Обозреватель» отображается название создана динамически](layouts/_static/image9.png)

<span data-ttu-id="9888f-216">Если требуется, просмотрите исходный код страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="9888f-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="9888f-217">Можно видеть, что `<title>` подготавливается к просмотру как элемент `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="9888f-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="9888f-218">**Объект страницы**</span><span class="sxs-lookup"><span data-stu-id="9888f-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="9888f-219">Полезной функцией `Page` — это динамический объект — `Title` свойство не является фиксированной или зарезервированное имя.</span><span class="sxs-lookup"><span data-stu-id="9888f-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="9888f-220">Можно использовать *любой* имя значения `Page` объекта.</span><span class="sxs-lookup"><span data-stu-id="9888f-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="9888f-221">Например, можно так же просто, передано заголовок, используя свойство с именем `Page.CurrentName` или `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="9888f-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="9888f-222">Единственное ограничение, имеет имя следовать обычным правилам для свойства, может иметь имя.</span><span class="sxs-lookup"><span data-stu-id="9888f-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="9888f-223">(Например, имя не может содержать пробел).</span><span class="sxs-lookup"><span data-stu-id="9888f-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="9888f-224">Можно передать любое количество значений с помощью `Page` объекта.</span><span class="sxs-lookup"><span data-stu-id="9888f-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="9888f-225">Если требуется передать сведения о фильмах страницу макета, можно передать значения с помощью примерно `Page.MovieTitle` и `Page.Genre` и `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="9888f-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="9888f-226">(Или другие имена, которые разработан для хранения информации). Единственное требование — это, скорее всего, очевидно, необходимо использовать одинаковые имена в странице содержимого и страницы макета.</span><span class="sxs-lookup"><span data-stu-id="9888f-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="9888f-227">Сведения, можно передать с помощью `Page` объект не ограничиваются только текст, отображаемый на странице макета.</span><span class="sxs-lookup"><span data-stu-id="9888f-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="9888f-228">Можно передавать значение для страницы макета, а затем код на странице макета можно использовать значение, чтобы решить, нужно ли выводить раздел страницы, что *.css* файл для использования и т. д.</span><span class="sxs-lookup"><span data-stu-id="9888f-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="9888f-229">Переданные значения `Page` объекта как любые другие значения, которые можно использовать в коде.</span><span class="sxs-lookup"><span data-stu-id="9888f-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="9888f-230">Это только что значения получаются на странице содержимого и передаются на страницу макета.</span><span class="sxs-lookup"><span data-stu-id="9888f-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="9888f-231">Откройте *AddMovie.cshtml* страницы и добавьте строку в начало кода, который предоставляет заголовок *AddMovie.cshtml* страницы:</span><span class="sxs-lookup"><span data-stu-id="9888f-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="9888f-232">Запустите *AddMovie.cshtml* страницы.</span><span class="sxs-lookup"><span data-stu-id="9888f-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="9888f-233">Отображается новый заголовок существует:</span><span class="sxs-lookup"><span data-stu-id="9888f-233">You see the new title there:</span></span>

![Вкладка «Обозреватель» отображается название «Добавить фильмов» создана динамически](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="9888f-235">Обновление оставшихся страниц используется режим</span><span class="sxs-lookup"><span data-stu-id="9888f-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="9888f-236">Теперь вы можете завершить оставшихся страниц на сайте, чтобы они использовали новый макет.</span><span class="sxs-lookup"><span data-stu-id="9888f-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="9888f-237">Откройте *EditMovie.cshtml* и *DeleteMovie.cshtml* в свою очередь и внесения изменений в каждом.</span><span class="sxs-lookup"><span data-stu-id="9888f-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="9888f-238">Добавьте строку кода, ссылающуюся на странице макета.</span><span class="sxs-lookup"><span data-stu-id="9888f-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="9888f-239">Добавьте строку для установки заголовка страницы:</span><span class="sxs-lookup"><span data-stu-id="9888f-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="9888f-240">или</span><span class="sxs-lookup"><span data-stu-id="9888f-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="9888f-241">Удалите лишние разметку HTML, по сути, оставьте только биты, которые находятся внутри `<body>` элемент (плюс блок кода в верхней).</span><span class="sxs-lookup"><span data-stu-id="9888f-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="9888f-242">Изменение `<h1>` элемент `<h2>` элемента.</span><span class="sxs-lookup"><span data-stu-id="9888f-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="9888f-243">После внесения этих изменений, проверьте все и убедитесь в том, что он правильно отображает и правильность заголовок.</span><span class="sxs-lookup"><span data-stu-id="9888f-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="9888f-244">Parting мысли об макета страницы</span><span class="sxs-lookup"><span data-stu-id="9888f-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="9888f-245">В этом учебнике вы создали  *\_Layout.cshtml* страницы и использовать `RenderBody` метод для слияния содержимого с другой страницы.</span><span class="sxs-lookup"><span data-stu-id="9888f-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="9888f-246">Это базовый шаблон с помощью разметки на веб-страницах.</span><span class="sxs-lookup"><span data-stu-id="9888f-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="9888f-247">Макет страницы обладают дополнительными функциями, которые не материале.</span><span class="sxs-lookup"><span data-stu-id="9888f-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="9888f-248">Например, можно вложить страницы макета — одну страницу макета в свою очередь может ссылаться на другой.</span><span class="sxs-lookup"><span data-stu-id="9888f-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="9888f-249">Вложенные структуры может быть полезно, если вы работаете с подразделах сайта, требующие разные макеты.</span><span class="sxs-lookup"><span data-stu-id="9888f-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="9888f-250">Можно также использовать дополнительные методы (например, `RenderSection`) для настройки с именем разделов макета страницы.</span><span class="sxs-lookup"><span data-stu-id="9888f-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="9888f-251">Сочетание страницы макета и *.css* файлы обладает широкими возможностями.</span><span class="sxs-lookup"><span data-stu-id="9888f-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="9888f-252">Как вы увидите Далее учебника серии, в WebMatrix можно создать сайт на основе *шаблона*, которое позволяет сайт, который имеет стандартные функциональные возможности в нем.</span><span class="sxs-lookup"><span data-stu-id="9888f-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="9888f-253">Шаблоны позволяют рекомендуется использовать макет страницы и CSS для создания сайтов, прекрасно выглядят и имеют функции, например меню.</span><span class="sxs-lookup"><span data-stu-id="9888f-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="9888f-254">Ниже приведен снимок экрана домашней страницы с узла, на основе шаблона, показывающий функции, используемые страницы макета и CSS.</span><span class="sxs-lookup"><span data-stu-id="9888f-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Макет, созданных с использованием шаблона сайта WebMatrix, показывающая заголовок, область навигации, область содержимого, дополнительный раздел и ссылки входа](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="9888f-256">Полный пример для страницы фильма (обновлена для использования страницы макета)</span><span class="sxs-lookup"><span data-stu-id="9888f-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="9888f-257">Страницы для добавления фильма страницы (обновлено для макета)</span><span class="sxs-lookup"><span data-stu-id="9888f-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="9888f-258">Полный пример страницы для страницы удаления фильма (обновлено для макета)</span><span class="sxs-lookup"><span data-stu-id="9888f-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="9888f-259">Полный пример страницы для страницы правки фильма (обновлено для макета)</span><span class="sxs-lookup"><span data-stu-id="9888f-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="9888f-260">В ближайшее время</span><span class="sxs-lookup"><span data-stu-id="9888f-260">Coming Up Next</span></span>

<span data-ttu-id="9888f-261">В следующем уроке будет рассмотрено публикации сайта в Интернете, поэтому всегда можно увидеть.</span><span class="sxs-lookup"><span data-stu-id="9888f-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9888f-262">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9888f-262">Additional Resources</span></span>

- <span data-ttu-id="9888f-263">[Создание согласованного выглядеть](https://go.microsoft.com/fwlink/?LinkID=202891) — статьи, которая содержит некоторые дополнительные сведения о работе с макетами.</span><span class="sxs-lookup"><span data-stu-id="9888f-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="9888f-264">Также описывается передать значение страницу макета, которая показывает или скрывает некоторое содержимое.</span><span class="sxs-lookup"><span data-stu-id="9888f-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="9888f-265">[Вложенные страницы макета в Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — блоги Майк Бринд пример страницы макета вкладывать друг в друга.</span><span class="sxs-lookup"><span data-stu-id="9888f-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="9888f-266">(Включая загрузки страниц).</span><span class="sxs-lookup"><span data-stu-id="9888f-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9888f-267">[Назад](deleting-data.md)
> [Вперед](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="9888f-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
