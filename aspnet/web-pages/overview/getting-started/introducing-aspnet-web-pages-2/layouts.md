---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Знакомство с веб-страниц ASP.NET — Создание согласованного макета | Документация Майкрософт
author: tfitzmac
description: Этом руководстве показано, как использовать макеты для создания согласованного вида для страниц на сайте, который использует веб-страниц ASP.NET. Предполагается, что вы выполнили...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 8f3d9e8a6f6a0179224e18faf11db3dc1510a095
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837204"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="15757-104">Знакомство с веб-страниц ASP.NET — Создание согласованного макета</span><span class="sxs-lookup"><span data-stu-id="15757-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="15757-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="15757-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="15757-106">Этом руководстве показано, как использовать *макеты* для создания согласованного вида для страниц на сайте, который использует веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="15757-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="15757-107">Предполагается, вы выполнили рядов через [удаление базы данных в ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="15757-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="15757-108">Вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="15757-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="15757-109">— Какие страницы макета.</span><span class="sxs-lookup"><span data-stu-id="15757-109">What a layout page is.</span></span>
> - <span data-ttu-id="15757-110">Как объединить макета страниц с динамическим содержимым.</span><span class="sxs-lookup"><span data-stu-id="15757-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="15757-111">Как передавать значения для страницы макета.</span><span class="sxs-lookup"><span data-stu-id="15757-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="15757-112">О макетах</span><span class="sxs-lookup"><span data-stu-id="15757-112">About Layouts</span></span>

<span data-ttu-id="15757-113">Страницы, вы создали ранее все были завершения автономные страницы.</span><span class="sxs-lookup"><span data-stu-id="15757-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="15757-114">Все они принадлежат тому же сайту, но у них нет общих элементов или стандартный вид.</span><span class="sxs-lookup"><span data-stu-id="15757-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="15757-115">У большинства сайтов, согласованный внешний вид и макет.</span><span class="sxs-lookup"><span data-stu-id="15757-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="15757-116">Например, если вы собираетесь [Microsoft.com/web](https://www.microsoft.com/web/) сайта и оглядитесь, вы увидите соблюдать все страницы для общей структуры и визуальной темы:</span><span class="sxs-lookup"><span data-stu-id="15757-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Страница сайта Microsoft.com/Web структуры заголовка, области навигации и область содержимого, а также нижний колонтитул](layouts/_static/image1.png)

<span data-ttu-id="15757-118">*Неэффективным* способом для создания этого макета будет определить заголовок, панель навигации и нижний колонтитул отдельно для каждой из страниц.</span><span class="sxs-lookup"><span data-stu-id="15757-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="15757-119">При этом произойдет дублирование та же разметка каждый раз.</span><span class="sxs-lookup"><span data-stu-id="15757-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="15757-120">Если вы хотите изменить что-то (например, обновление нижнего колонтитула), необходимо изменить каждую страницу отдельно.</span><span class="sxs-lookup"><span data-stu-id="15757-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="15757-121">Именно здесь *страницы макета* бывают.</span><span class="sxs-lookup"><span data-stu-id="15757-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="15757-122">Веб-страницах ASP.NET, вы можете определить страницу макета, который предоставляет общий контейнер для страниц на сайте.</span><span class="sxs-lookup"><span data-stu-id="15757-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="15757-123">Например на странице макета может содержать заголовок, область навигации и нижний колонтитул.</span><span class="sxs-lookup"><span data-stu-id="15757-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="15757-124">Страница макета с заполнителем, от основного содержимого назначения.</span><span class="sxs-lookup"><span data-stu-id="15757-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="15757-125">Затем можно определить отдельных страниц содержимого, которые содержат разметку и код только для этой страницы.</span><span class="sxs-lookup"><span data-stu-id="15757-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="15757-126">Страницы содержимого не нужно быть полный HTML-страницы; даже не нужно иметь `<body>` элемент.</span><span class="sxs-lookup"><span data-stu-id="15757-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="15757-127">Они также имеют строку кода, о том, какие страницы макета для отображения содержимого в ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="15757-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="15757-128">Ниже приведен рисунок, приблизительно показано, как работает эта связь.</span><span class="sxs-lookup"><span data-stu-id="15757-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Концептуальная схема, показывающая две страницы содержимого и макета страницы, в котором они работают](layouts/_static/image2.png)

<span data-ttu-id="15757-130">Это взаимодействие можно легко понять, когда увидите его в действии.</span><span class="sxs-lookup"><span data-stu-id="15757-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="15757-131">В этом учебнике вам предстоит изменить Чтобы использовать макет страницы фильмов.</span><span class="sxs-lookup"><span data-stu-id="15757-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="15757-132">Добавление страницы макета</span><span class="sxs-lookup"><span data-stu-id="15757-132">Adding a Layout Page</span></span>

<span data-ttu-id="15757-133">Начнем с создания макета страницы, которая определяет типичная страница макета с верхний колонтитул, нижний колонтитул и область основного содержимого.</span><span class="sxs-lookup"><span data-stu-id="15757-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="15757-134">В узле WebPagesMovies добавьте CSHTML-странице с именем  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="15757-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="15757-135">Символа подчеркивания ( `_` ) символ важен.</span><span class="sxs-lookup"><span data-stu-id="15757-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="15757-136">Если имя страницы начинается с символа подчеркивания, ASP.NET не будет отправлять непосредственно эту страницу в браузер.</span><span class="sxs-lookup"><span data-stu-id="15757-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="15757-137">Это соглашение позволяет определить страниц, которые необходимы для вашего сайта, но, что пользователи не должны иметь возможность напрямую запрашивать.</span><span class="sxs-lookup"><span data-stu-id="15757-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="15757-138">Замените содержимое на странице следующее:</span><span class="sxs-lookup"><span data-stu-id="15757-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="15757-139">Как вы видите, эта разметка является просто HTML, который использует `<div>` элементы для определения трех разделов в странице, а также один более `<div>` элемента для хранения трех разделов.</span><span class="sxs-lookup"><span data-stu-id="15757-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="15757-140">Нижний колонтитул содержит немного кода Razor: `@DateTime.Now.Year`, отображающего текущий год в этом месте страницы.</span><span class="sxs-lookup"><span data-stu-id="15757-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="15757-141">Обратите внимание, что ссылка на таблицу стилей с именем *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="15757-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="15757-142">Таблица стилей —, где определены аспекты физическим размещением элементов.</span><span class="sxs-lookup"><span data-stu-id="15757-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="15757-143">Вы создадите, чуть позже.</span><span class="sxs-lookup"><span data-stu-id="15757-143">You'll create that in a moment.</span></span>

<span data-ttu-id="15757-144">Единственными необычными функции в этом  *\_Layout.cshtml* страница `@Render.Body()` строки.</span><span class="sxs-lookup"><span data-stu-id="15757-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="15757-145">Это заполнитель, куда содержимое при слиянии этот макет с другой страницы.</span><span class="sxs-lookup"><span data-stu-id="15757-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="15757-146">Добавление CSS-файл</span><span class="sxs-lookup"><span data-stu-id="15757-146">Adding a .css File</span></span>

<span data-ttu-id="15757-147">Предпочтительный способ определить, фактическому расположению (то есть внешний) элементы на странице — использовать правила стиля таблицы каскадных стилей (CSS).</span><span class="sxs-lookup"><span data-stu-id="15757-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="15757-148">Так вы создадите *.css* файл, содержащий правила для нового макета.</span><span class="sxs-lookup"><span data-stu-id="15757-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="15757-149">В WebMatrix выберите корень сайта.</span><span class="sxs-lookup"><span data-stu-id="15757-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="15757-150">Затем в **файлы** вкладку на ленте, щелкните стрелку под кнопкой **New** а затем нажмите кнопку **новую папку**.</span><span class="sxs-lookup"><span data-stu-id="15757-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![Параметр «Новая папка» в разделе New на ленте.](layouts/_static/image3.png)

<span data-ttu-id="15757-152">Назовите новую папку *стили*.</span><span class="sxs-lookup"><span data-stu-id="15757-152">Name the new folder *Styles*.</span></span>

![Именование в новую папку «Стили»](layouts/_static/image4.png)

<span data-ttu-id="15757-154">Внутри новой *стили* папке создайте файл с именем *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="15757-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Создание нового файла Movies.css](layouts/_static/image5.png)

<span data-ttu-id="15757-156">Замените содержимое нового *.css* файл со следующими:</span><span class="sxs-lookup"><span data-stu-id="15757-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="15757-157">Мы не очень много говорит о эти правила CSS, за исключением Примечание две вещи.</span><span class="sxs-lookup"><span data-stu-id="15757-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="15757-158">Один том, что в дополнение к установке шрифтах и размерах, правила использовать абсолютное позиционирование для определения местоположения заголовка, нижнего колонтитула и основную область содержимого.</span><span class="sxs-lookup"><span data-stu-id="15757-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="15757-159">Если вы не знакомы с размещением в CSS, можно прочитать [позиционирования CSS](http://www.w3schools.com/css/css_positioning.asp) руководства на веб-сайте W3Schools.</span><span class="sxs-lookup"><span data-stu-id="15757-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="15757-160">Необходимо отметить, что в нижней части мы скопировали правила стилей, которые были изначально определяется по отдельности в *Movies.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="15757-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="15757-161">Эти правила были использованы в [Общие сведения о отображение данных с помощью ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) руководству, чтобы сделать `WebGrid` вспомогательный объект отображать разметку, которая добавляется в таблицу полосы.</span><span class="sxs-lookup"><span data-stu-id="15757-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="15757-162">(Если вы собираетесь использовать *.css* файл для определения стилей, также могут представлять правила стилей для всего узла в его.)</span><span class="sxs-lookup"><span data-stu-id="15757-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="15757-163">Обновление файла фильмы используется режим</span><span class="sxs-lookup"><span data-stu-id="15757-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="15757-164">Теперь можно обновить существующие файлы в свой сайт для использования нового макета.</span><span class="sxs-lookup"><span data-stu-id="15757-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="15757-165">Откройте *Movies.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="15757-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="15757-166">В верхней части в первой строке кода, добавьте следующие строки:</span><span class="sxs-lookup"><span data-stu-id="15757-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="15757-167">Теперь страницы начинается таким образом:</span><span class="sxs-lookup"><span data-stu-id="15757-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="15757-168">Это одной строкой кода сообщает ASP.NET, что при *фильмы* страница выполняется, она должна быть объединена с  *\_Layout.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="15757-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="15757-169">Так как *Movies.cshtml* использует страницы макета, можно удалить разметку из *Movies.cshtml* страницы, Разобравшись с  *\_Layout.cshtml*файл.</span><span class="sxs-lookup"><span data-stu-id="15757-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="15757-170">Убрать `<!DOCTYPE>`, `<html>`, и `<body>` открывающих и закрывающих тегов.</span><span class="sxs-lookup"><span data-stu-id="15757-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="15757-171">Удалить весь `<head>` элемент и его содержимое, включая правила стилей для сетки, так как у вас теперь есть этих правил *.css* файл.</span><span class="sxs-lookup"><span data-stu-id="15757-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="15757-172">Хотя вы взялись за это, измените существующий `<h1>` элемент `<h2>` элемент; у вас есть `<h1>` элемент уже на странице макета.</span><span class="sxs-lookup"><span data-stu-id="15757-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="15757-173">Изменение `<h2>` текст «Список фильмов».</span><span class="sxs-lookup"><span data-stu-id="15757-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="15757-174">Обычно вам не нужно вносить такого рода изменений на странице содержимого.</span><span class="sxs-lookup"><span data-stu-id="15757-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="15757-175">При начинают веб-узла с помощью страницы макета, страницы содержимого, не все эти элементы создаются с самого начала.</span><span class="sxs-lookup"><span data-stu-id="15757-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="15757-176">Таким образом Однако вы преобразуете отдельной странице, который использует макет, поэтому очистки.</span><span class="sxs-lookup"><span data-stu-id="15757-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="15757-177">Когда закончите, *Movies.cshtml* страница будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="15757-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="15757-178">Тестирование макета</span><span class="sxs-lookup"><span data-stu-id="15757-178">Testing the Layout</span></span>

<span data-ttu-id="15757-179">Теперь вы увидите, как выглядит макет.</span><span class="sxs-lookup"><span data-stu-id="15757-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="15757-180">В WebMatrix, щелкните правой кнопкой мыши *Movies.cshtml* и укажите **запустить в браузере**.</span><span class="sxs-lookup"><span data-stu-id="15757-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="15757-181">Когда браузер отображает страницу, он выглядит эту страницу:</span><span class="sxs-lookup"><span data-stu-id="15757-181">When the browser displays the page, it looks like this page:</span></span>

![Фильмы страницы к просмотру с помощью макета](layouts/_static/image6.png)

<span data-ttu-id="15757-183">ASP.NET объединен содержимое страницы Movies.cshtml в  *\_Layout.cshtml* странице правой where `RenderBody` является метод.</span><span class="sxs-lookup"><span data-stu-id="15757-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="15757-184">И, конечно же  *\_Layout.cshtml* странице ссылки *.css* файл, который определяет внешний вид страницы.</span><span class="sxs-lookup"><span data-stu-id="15757-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="15757-185">Обновление страницы AddMovie используется режим</span><span class="sxs-lookup"><span data-stu-id="15757-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="15757-186">Настоящее преимущество макетов является, то их можно использовать для всех страниц узла.</span><span class="sxs-lookup"><span data-stu-id="15757-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="15757-187">Откройте *AddMovie.cshtml* страницы.</span><span class="sxs-lookup"><span data-stu-id="15757-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="15757-188">Вы помните, что *AddMovie.cshtml* страницы курировал некоторые правила CSS в его, чтобы определить внешний вид сообщений об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="15757-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="15757-189">Так как у вас есть *.css* файл для свой сайт прямо сейчас, вы можете переместить правила, чтобы они *.css* файл.</span><span class="sxs-lookup"><span data-stu-id="15757-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="15757-190">Удалить их из *AddMovie.cshtml* и добавьте их в нижнюю часть *Movies.css* файла.</span><span class="sxs-lookup"><span data-stu-id="15757-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="15757-191">При перемещении следующие правила:</span><span class="sxs-lookup"><span data-stu-id="15757-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="15757-192">Теперь внесите изменения в со схожими *AddMovie.cshtml* , это было сделано для *Movies.cshtml* — добавить `Layout="~/_Layout.cshtml;` и удаление разметки HTML, который теперь является лишним.</span><span class="sxs-lookup"><span data-stu-id="15757-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="15757-193">Изменение `<h1>` элемент `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="15757-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="15757-194">Когда все будет готово, страница будет выглядеть как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="15757-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="15757-195">Откройте страницу.</span><span class="sxs-lookup"><span data-stu-id="15757-195">Run the page.</span></span> <span data-ttu-id="15757-196">Теперь она выглядит как на этом рисунке:</span><span class="sxs-lookup"><span data-stu-id="15757-196">Now it looks like this illustration:</span></span>

![Страница «Добавить фильмы», отображаются с помощью макета](layouts/_static/image7.png)

<span data-ttu-id="15757-198">Вы хотите внести аналогичные изменения к страницам на сайте — *EditMovie.cshtml* и *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="15757-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="15757-199">Тем не менее прежде чем выполнять, выполненные еще одно изменение макет, который обеспечивает дополнительную гибкость.</span><span class="sxs-lookup"><span data-stu-id="15757-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="15757-200">Передача сведений об заголовок на страницу макета</span><span class="sxs-lookup"><span data-stu-id="15757-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="15757-201">*\_Layout.cshtml* имеет созданную страницу `<title>` элемент, который имеет значение «Мой узел фильма».</span><span class="sxs-lookup"><span data-stu-id="15757-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="15757-202">Большинство браузеров для отображения содержимого этого элемента как текст на вкладке:</span><span class="sxs-lookup"><span data-stu-id="15757-202">Most browsers display the content of this element as the text on a tab:</span></span>

![Страницы &lt;title&gt; элементу, отображаемому на вкладке браузера](layouts/_static/image8.png)

<span data-ttu-id="15757-204">Эта информация заголовка является универсальным.</span><span class="sxs-lookup"><span data-stu-id="15757-204">This title information is generic.</span></span> <span data-ttu-id="15757-205">Предположим, что требуется текст заголовка, точнее в текущую страницу.</span><span class="sxs-lookup"><span data-stu-id="15757-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="15757-206">(Текст заголовка также используется поисковые системы для определения того, что страница не о.) Можно передать сведения из страницы содержания как *Movies.cshtml* или *AddMovie.cshtml* для макета страницы, а затем с помощью визуализирует эту информацию для настройки макета страницы.</span><span class="sxs-lookup"><span data-stu-id="15757-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="15757-207">Откройте *Movies.cshtml* странице еще раз.</span><span class="sxs-lookup"><span data-stu-id="15757-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="15757-208">В коде, в верхней добавьте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="15757-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="15757-209">`Page` Объект будет доступен на всех *.cshtml* страницы, а также предназначен для этой цели, а именно: для совместного использования информации между страницей и его макет.</span><span class="sxs-lookup"><span data-stu-id="15757-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="15757-210">Откройте<em>\_Layout.cshtml</em> страницы.</span><span class="sxs-lookup"><span data-stu-id="15757-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="15757-211">Изменение `<title>` элемент, чтобы он выглядел Эта разметка:</span><span class="sxs-lookup"><span data-stu-id="15757-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="15757-212">Этот код отображает содержимое `Page.Title` свойство прямо в этом месте страницы.</span><span class="sxs-lookup"><span data-stu-id="15757-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="15757-213">Запустите *Movies.cshtml* страницы.</span><span class="sxs-lookup"><span data-stu-id="15757-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="15757-214">На этот раз на вкладке браузера показывает было передано в качестве значения `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="15757-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![На вкладке браузера, отображается название динамически создан](layouts/_static/image9.png)

<span data-ttu-id="15757-216">Если требуется, просмотрите исходный код страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="15757-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="15757-217">Можно заметить, что `<title>` элемент отображается как `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="15757-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="15757-218">**Объект страницы**</span><span class="sxs-lookup"><span data-stu-id="15757-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="15757-219">Полезной возможностью `Page` — это динамический объект — `Title` свойство не является именем фиксированный или зарезервированный.</span><span class="sxs-lookup"><span data-stu-id="15757-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="15757-220">Можно использовать *любой* имя для значения `Page` объекта.</span><span class="sxs-lookup"><span data-stu-id="15757-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="15757-221">Например, то может так же легко передать заголовок, используя свойство с именем `Page.CurrentName` или `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="15757-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="15757-222">Единственным ограничением является то, что имя должно следовать нормальным правилам для какие свойства можно присвоить имя.</span><span class="sxs-lookup"><span data-stu-id="15757-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="15757-223">(Например, имя не может содержать пробел).</span><span class="sxs-lookup"><span data-stu-id="15757-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="15757-224">Можно передать любое количество значений с помощью `Page` объекта.</span><span class="sxs-lookup"><span data-stu-id="15757-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="15757-225">Если вы хотите передать сведения о фильме на страницу макета, можно передать значения с помощью примерно `Page.MovieTitle` и `Page.Genre` и `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="15757-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="15757-226">(Или другие имена, которые разработаны для хранения информации). Единственное требование — это, вероятно, очевидно, этого необходимо использовать одинаковые имена в странице содержимого и макета страницы.</span><span class="sxs-lookup"><span data-stu-id="15757-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="15757-227">Сведения, указываемые с помощью `Page` объекта не ограничивается только текст, отображаемый на странице макета.</span><span class="sxs-lookup"><span data-stu-id="15757-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="15757-228">Можно передать значение на страницу макета, а затем код на странице макета можно использовать значение, чтобы решить, следует ли отображать раздел страницы, что *.css* нужно использовать и т. д.</span><span class="sxs-lookup"><span data-stu-id="15757-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="15757-229">Значения, указываемые в `Page` объекта как и любые другие значения, используется в коде.</span><span class="sxs-lookup"><span data-stu-id="15757-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="15757-230">Это только что значения получаются на странице содержимого и передаются на страницу макета.</span><span class="sxs-lookup"><span data-stu-id="15757-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="15757-231">Откройте *AddMovie.cshtml* странице и добавьте строку в начало кода с заголовком для *AddMovie.cshtml* страницы:</span><span class="sxs-lookup"><span data-stu-id="15757-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="15757-232">Запустите *AddMovie.cshtml* страницы.</span><span class="sxs-lookup"><span data-stu-id="15757-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="15757-233">Вы увидите новый заголовок существует:</span><span class="sxs-lookup"><span data-stu-id="15757-233">You see the new title there:</span></span>

![На вкладке браузера, отображается название «Добавить фильмы» создана динамически](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="15757-235">Обновление оставшихся страниц используется режим</span><span class="sxs-lookup"><span data-stu-id="15757-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="15757-236">Теперь вы можете завершить оставшихся страницах на сайте, чтобы они использовали новый макет.</span><span class="sxs-lookup"><span data-stu-id="15757-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="15757-237">Откройте *EditMovie.cshtml* и *DeleteMovie.cshtml* в свою очередь и внесите те же изменения в каждом.</span><span class="sxs-lookup"><span data-stu-id="15757-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="15757-238">Добавьте строку кода, который содержит ссылки на страницу макета:</span><span class="sxs-lookup"><span data-stu-id="15757-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="15757-239">Добавьте строку для установки заголовка страницы:</span><span class="sxs-lookup"><span data-stu-id="15757-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="15757-240">или</span><span class="sxs-lookup"><span data-stu-id="15757-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="15757-241">Удалите все лишние разметку HTML, по сути, оставьте только биты, которые находятся внутри `<body>` элемент (а также блок кода в верхней).</span><span class="sxs-lookup"><span data-stu-id="15757-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="15757-242">Изменение `<h1>` элемента, который будет `<h2>` элемент.</span><span class="sxs-lookup"><span data-stu-id="15757-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="15757-243">После внесения этих изменений, проверить каждую и убедитесь, что он правильно отображает и правильность заголовок.</span><span class="sxs-lookup"><span data-stu-id="15757-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="15757-244">Parting мысли о страниц макета</span><span class="sxs-lookup"><span data-stu-id="15757-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="15757-245">В этом руководстве вы создали  *\_Layout.cshtml* странице и использовать `RenderBody` метод для слияния содержимого с другой страницы.</span><span class="sxs-lookup"><span data-stu-id="15757-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="15757-246">Это типичное использование макетов веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="15757-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="15757-247">Макет страницы обладают дополнительными функциями, мы не рассмотрели здесь.</span><span class="sxs-lookup"><span data-stu-id="15757-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="15757-248">Например, можно создавать вложенные страницы макета — одну страницу макета в свою очередь может ссылаться на друга.</span><span class="sxs-lookup"><span data-stu-id="15757-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="15757-249">Вложенным макетам, можно использовать в том случае, если вы работаете с подразделах сайта, требующие различных макетах.</span><span class="sxs-lookup"><span data-stu-id="15757-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="15757-250">Можно также использовать дополнительные методы (например, `RenderSection`) для настройки с именем разделах на странице макета.</span><span class="sxs-lookup"><span data-stu-id="15757-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="15757-251">Сочетание страницы макета и *.css* файлы обладает широкими возможностями.</span><span class="sxs-lookup"><span data-stu-id="15757-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="15757-252">Как можно будет увидеть в следующей серии руководств, в WebMatrix можно создать сайт на основе *шаблона*, которое позволяет веб-узла с стандартные функциональные возможности, в нем.</span><span class="sxs-lookup"><span data-stu-id="15757-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="15757-253">Шаблоны пригодились макета страниц и CSS для создания сайтов, который отлично выглядеть при просмотре и имеют функции, такие как меню.</span><span class="sxs-lookup"><span data-stu-id="15757-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="15757-254">Ниже приведен снимок экрана домашней страницы с сайта на основе шаблона, показывающий функции, использующие макета страниц и CSS.</span><span class="sxs-lookup"><span data-stu-id="15757-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Макет, создаваемого шаблоном сайта WebMatrix, отображение заголовка, области навигации, области содержимого, дополнительный раздел, а также ссылки для входа](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="15757-256">Полный пример для фильма странице (использование страницы "Макет")</span><span class="sxs-lookup"><span data-stu-id="15757-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="15757-257">Целая страница для добавления страницы фильма (обновлен для макета)</span><span class="sxs-lookup"><span data-stu-id="15757-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="15757-258">Полный пример страницы для страницы "Delete фильма" (обновлен для макета)</span><span class="sxs-lookup"><span data-stu-id="15757-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="15757-259">Полный пример страницы для страницы "Edit фильма" (обновлен для макета)</span><span class="sxs-lookup"><span data-stu-id="15757-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="15757-260">В ближайшее время</span><span class="sxs-lookup"><span data-stu-id="15757-260">Coming Up Next</span></span>

<span data-ttu-id="15757-261">В следующем руководстве вы узнаете, как для публикации своего сайта к Интернету, поэтому всегда можно увидеть.</span><span class="sxs-lookup"><span data-stu-id="15757-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="15757-262">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="15757-262">Additional Resources</span></span>

- <span data-ttu-id="15757-263">[Создание согласованного выглядеть](https://go.microsoft.com/fwlink/?LinkID=202891) — статьи, содержатся более подробные сведения о работе с макетами.</span><span class="sxs-lookup"><span data-stu-id="15757-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="15757-264">Он также описывает значение передается на страницу макета, которая показывает или скрывает некоторое содержимое.</span><span class="sxs-lookup"><span data-stu-id="15757-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="15757-265">[Вложенные страницы макета в Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — блоги Майк Бринд пример демонстрирует Вкладывание страницы макета.</span><span class="sxs-lookup"><span data-stu-id="15757-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="15757-266">(Включает в себя загрузки страниц).</span><span class="sxs-lookup"><span data-stu-id="15757-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="15757-267">[Назад](deleting-data.md)
> [Вперед](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="15757-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
