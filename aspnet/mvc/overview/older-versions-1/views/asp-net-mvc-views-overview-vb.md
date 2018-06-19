---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: ASP.NET MVC представления Обзор (VB) | Документы Microsoft
author: StephenWalther
description: Что такое представление ASP.NET MVC и как это отличается от HTML-страницу? В этом учебнике Стивен Вальтер содержит описание представления и показано, как можно t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: a64c70851d13b923964dfd1cf3bad55612ae0d0f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870860"
---
<a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="0c339-104">ASP.NET MVC представления Обзор (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="0c339-104">ASP.NET MVC Views Overview (VB)</span></span>
====================
<span data-ttu-id="0c339-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="0c339-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="0c339-106">Что такое представление ASP.NET MVC и как это отличается от HTML-страницу?</span><span class="sxs-lookup"><span data-stu-id="0c339-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="0c339-107">В этом учебнике Стивен Вальтер содержит описание представления и показано, как можно воспользоваться преимуществами Просмотр данных и вспомогательных методов HTML в представлении.</span><span class="sxs-lookup"><span data-stu-id="0c339-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="0c339-108">Этот учебник призван предоставляют краткое введение в ASP.NET MVC представления данных в представлении и вспомогательные методы HTML.</span><span class="sxs-lookup"><span data-stu-id="0c339-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="0c339-109">Изучив этот учебник необходимо понять, как создавать новые представления и передачи данных из контроллера в представление использования вспомогательных методов HTML для создания содержимого в представлении.</span><span class="sxs-lookup"><span data-stu-id="0c339-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="0c339-110">Основные сведения о представлениях</span><span class="sxs-lookup"><span data-stu-id="0c339-110">Understanding Views</span></span>

<span data-ttu-id="0c339-111">В отличие от страниц ASP или ASP.NET ASP.NET MVC не включает все, что соответствует непосредственно на страницу.</span><span class="sxs-lookup"><span data-stu-id="0c339-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="0c339-112">В приложении ASP.NET MVC не страницы на диск, который соответствует пути в URL-адрес, введите в адресной строке браузера.</span><span class="sxs-lookup"><span data-stu-id="0c339-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="0c339-113">-Это ближайший страницы в приложении ASP.NET MVC, то вызывается *представление*.</span><span class="sxs-lookup"><span data-stu-id="0c339-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="0c339-114">В приложении ASP.NET MVC входящие запросы браузера сопоставляются действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="0c339-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="0c339-115">Представление может возвратить следующие действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="0c339-115">A controller action might return a view.</span></span> <span data-ttu-id="0c339-116">Однако действия контроллера может выполнять действия, такие как перенаправление на другое действие контроллера другого типа.</span><span class="sxs-lookup"><span data-stu-id="0c339-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="0c339-117">Листинг 1 содержит простой контроллер с именем HomeController.</span><span class="sxs-lookup"><span data-stu-id="0c339-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="0c339-118">HomeController предоставляет два действия контроллера, с именем Index() и Details().</span><span class="sxs-lookup"><span data-stu-id="0c339-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="0c339-119">**Листинг 1 - HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="0c339-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="0c339-120">Первое действие, действие Index(), можно вызвать, введя следующий URL-адрес в адресную строку браузера:</span><span class="sxs-lookup"><span data-stu-id="0c339-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="0c339-121">/ Home/Index</span><span class="sxs-lookup"><span data-stu-id="0c339-121">/Home/Index</span></span>

<span data-ttu-id="0c339-122">Второе действие, действие Details(), можно вызвать, введя этот адрес в адресную строку браузера:</span><span class="sxs-lookup"><span data-stu-id="0c339-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="0c339-123">-Home/подробности</span><span class="sxs-lookup"><span data-stu-id="0c339-123">/Home/Details</span></span>

<span data-ttu-id="0c339-124">Действие Index() возвращает представление.</span><span class="sxs-lookup"><span data-stu-id="0c339-124">The Index() action returns a view.</span></span> <span data-ttu-id="0c339-125">Большинство действий, которые вы создаете вернет представления.</span><span class="sxs-lookup"><span data-stu-id="0c339-125">Most actions that you create will return views.</span></span> <span data-ttu-id="0c339-126">Тем не менее действие, которое может возвращать другие результаты действий.</span><span class="sxs-lookup"><span data-stu-id="0c339-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="0c339-127">Например действие Details() возвращает RedirectToActionResult, который перенаправляет Index() действие входящего запроса.</span><span class="sxs-lookup"><span data-stu-id="0c339-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="0c339-128">Действие Index() содержит следующие одной строки кода:</span><span class="sxs-lookup"><span data-stu-id="0c339-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="0c339-129">View()</span><span class="sxs-lookup"><span data-stu-id="0c339-129">View()</span></span>

<span data-ttu-id="0c339-130">Эта строка кода возвращает представление, которое должен быть расположен по следующему пути на веб-сервере:</span><span class="sxs-lookup"><span data-stu-id="0c339-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="0c339-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="0c339-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="0c339-132">Путь к представлению выводится из имени контроллера и имя действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="0c339-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="0c339-133">При желании можно явно задать, какие представления.</span><span class="sxs-lookup"><span data-stu-id="0c339-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="0c339-134">Следующий код возвращает представление с именем Fred:</span><span class="sxs-lookup"><span data-stu-id="0c339-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="0c339-135">Представление (Fred)</span><span class="sxs-lookup"><span data-stu-id="0c339-135">View( Fred )</span></span>

<span data-ttu-id="0c339-136">При выполнении этой строки кода, представления возвращается из следующей папки:</span><span class="sxs-lookup"><span data-stu-id="0c339-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="0c339-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="0c339-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="0c339-138">Если вы планируете создавать модульные тесты для приложения ASP.NET MVC является рекомендуется явно указывать имена представлений.</span><span class="sxs-lookup"><span data-stu-id="0c339-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="0c339-139">Таким образом, можно создать модульный тест, чтобы убедиться, что ожидаемое представление возвращенный действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="0c339-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="0c339-140">Добавление содержимого к представлению</span><span class="sxs-lookup"><span data-stu-id="0c339-140">Adding Content to a View</span></span>

<span data-ttu-id="0c339-141">Представление — это стандарт (HTML-документ, который может содержать скриптов X).</span><span class="sxs-lookup"><span data-stu-id="0c339-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="0c339-142">Использовать сценарии для добавления к представлению динамического содержимого.</span><span class="sxs-lookup"><span data-stu-id="0c339-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="0c339-143">Например представление в списке 2 отображает текущую дату и время.</span><span class="sxs-lookup"><span data-stu-id="0c339-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="0c339-144">**Листинг 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="0c339-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="0c339-145">Обратите внимание, что текст HTML-страницы в списке 2 содержит следующий скрипт:</span><span class="sxs-lookup"><span data-stu-id="0c339-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="0c339-146">&lt;% Response.Write(DateTime.Now) %&gt;</span><span class="sxs-lookup"><span data-stu-id="0c339-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="0c339-147">Используйте разделители сценарий &lt;% и %&gt; чтобы пометить начало и конец скрипта.</span><span class="sxs-lookup"><span data-stu-id="0c339-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="0c339-148">Этот сценарий написан на языке Visual basic.</span><span class="sxs-lookup"><span data-stu-id="0c339-148">This script is written in Visual basic.</span></span> <span data-ttu-id="0c339-149">Отображает текущую дату и время, вызвав метод Response.Write() для отображения содержимого в браузер.</span><span class="sxs-lookup"><span data-stu-id="0c339-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="0c339-150">Разделители сценарий &lt;% и %&gt; может использоваться для выполнения одной или нескольких инструкций.</span><span class="sxs-lookup"><span data-stu-id="0c339-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="0c339-151">Поскольку Response.Write() вызвать так часто, корпорация Майкрософт предоставит вам ярлык для вызова метода Response.Write().</span><span class="sxs-lookup"><span data-stu-id="0c339-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="0c339-152">Представление в списке 3 использует разделители &lt;% = и %&gt; для быстрого вызова Response.Write().</span><span class="sxs-lookup"><span data-stu-id="0c339-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="0c339-153">**Листинг 3 - Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="0c339-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="0c339-154">Можно использовать любой язык .NET для создания динамического содержимого в представлении.</span><span class="sxs-lookup"><span data-stu-id="0c339-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="0c339-155">Как правило вы получите использовать Visual Basic .NET или C# для записи контроллеры и представления.</span><span class="sxs-lookup"><span data-stu-id="0c339-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="0c339-156">С помощью вспомогательных методов HTML для создания представления содержимого</span><span class="sxs-lookup"><span data-stu-id="0c339-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="0c339-157">Для упрощения добавления содержимого в представлении вы сможете использовать какой-либо величины называется *вспомогательный метод HTML*.</span><span class="sxs-lookup"><span data-stu-id="0c339-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="0c339-158">Вспомогательный метод HTML, обычно является метод, который создает строку.</span><span class="sxs-lookup"><span data-stu-id="0c339-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="0c339-159">Вспомогательные методы HTML можно использовать для создания стандартных HTML-элементов, таких как текстовые поля, ссылки, раскрывающихся списков и списки.</span><span class="sxs-lookup"><span data-stu-id="0c339-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="0c339-160">Например, в представлении в листинге 4 пользуется три вспомогательных методов HTML — помощников BeginForm(), TextBox() и Password() — для создания имени входа формы (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="0c339-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="0c339-161">**Листинг 4 — \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="0c339-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


<span data-ttu-id="0c339-162">[![Диалоговое окно нового проекта](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0c339-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="0c339-163">**На рисунке 01**: стандартные формы входа ([Просмотр полноразмерное изображение](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="0c339-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="0c339-164">Все вспомогательные методы HTML методы вызываются для свойства Html представления.</span><span class="sxs-lookup"><span data-stu-id="0c339-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="0c339-165">Например путем вызова метода Html.TextBox() подготовке текстового поля.</span><span class="sxs-lookup"><span data-stu-id="0c339-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="0c339-166">Обратите внимание, использовать разделители сценарий &lt;% = и %&gt; при вызове Html.TextBox() и Html.Password() вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="0c339-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="0c339-167">Эти вспомогательные методы просто возвращают строку.</span><span class="sxs-lookup"><span data-stu-id="0c339-167">These helpers simply return a string.</span></span> <span data-ttu-id="0c339-168">Необходимо вызвать Response.Write() для отображения строки в браузере.</span><span class="sxs-lookup"><span data-stu-id="0c339-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="0c339-169">С помощью методов вспомогательный метод HTML является необязательным.</span><span class="sxs-lookup"><span data-stu-id="0c339-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="0c339-170">Они упрощения работы за счет сокращения HTML и сценарий, который необходимо написать.</span><span class="sxs-lookup"><span data-stu-id="0c339-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="0c339-171">Представление, в листинге 5 представляет точное же форму, как в представлении в листинге 4 без использования вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="0c339-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="0c339-172">**Список 5 — \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="0c339-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="0c339-173">Также имеется возможность создания собственных вспомогательные методы HTML.</span><span class="sxs-lookup"><span data-stu-id="0c339-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="0c339-174">Например можно создать GridView() вспомогательный метод, который автоматически отображается в HTML-таблицу набора записей базы данных.</span><span class="sxs-lookup"><span data-stu-id="0c339-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="0c339-175">В этом разделе мы исследуем в учебнике **Создание Custom HTML Helpers**.</span><span class="sxs-lookup"><span data-stu-id="0c339-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="0c339-176">Используя данные представления для передачи данных в представлении</span><span class="sxs-lookup"><span data-stu-id="0c339-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="0c339-177">Просмотр данных используется для передачи данных из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="0c339-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="0c339-178">Представить данные представления, как и пакет, который можно отправить по почте.</span><span class="sxs-lookup"><span data-stu-id="0c339-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="0c339-179">Все данные, передаваемые из контроллера в представление должно быть отправлено с помощью этого пакета.</span><span class="sxs-lookup"><span data-stu-id="0c339-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="0c339-180">Например контроллер в список 6 добавляет сообщение для просмотра данных.</span><span class="sxs-lookup"><span data-stu-id="0c339-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="0c339-181">**Перечисление 6 - ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="0c339-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="0c339-182">Контроллер свойство ViewData представляет коллекцию пар имя-значение.</span><span class="sxs-lookup"><span data-stu-id="0c339-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="0c339-183">В список 6 метод Index() добавляет элемент в коллекцию представлений данных, с именем сообщение со значением Hello World!.</span><span class="sxs-lookup"><span data-stu-id="0c339-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="0c339-184">При представлении возвращается методом Index(), данные представления автоматически передается в представление.</span><span class="sxs-lookup"><span data-stu-id="0c339-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="0c339-185">Представление со списком 7 извлекает данные из представления сообщения и отображает сообщение в браузере.</span><span class="sxs-lookup"><span data-stu-id="0c339-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="0c339-186">**Листинг 7 — \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="0c339-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="0c339-187">Обратите внимание, что представление использует метод Html.Encode() вспомогательный метод HTML при обработке сообщения.</span><span class="sxs-lookup"><span data-stu-id="0c339-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="0c339-188">Вспомогательный метод HTML Html.Encode() кодирует специальные символы, такие как &lt; и &gt; в символы, которые могут использоваться для отображения на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="0c339-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="0c339-189">При подготовке содержимого, которое пользователь отправляет на веб-сайт следует кодировать содержимое для предотвращения атак путем внедрения кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0c339-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="0c339-190">(Потому что мы создали сообщения сами ProductController, мы не хотите t действительно требуется для кодирования сообщений.</span><span class="sxs-lookup"><span data-stu-id="0c339-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="0c339-191">Тем не менее это хорошая привычка всегда вызывайте метод Html.Encode(), если отображение содержимого извлечь из представления данных в представлении.)</span><span class="sxs-lookup"><span data-stu-id="0c339-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="0c339-192">В поле со списком 7 мы воспользовался передачи простое строковое сообщение из контроллера в представление данных представления.</span><span class="sxs-lookup"><span data-stu-id="0c339-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="0c339-193">Просмотр данных также можно использовать для передачи в другие типы данных, например набора записей базы данных, из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="0c339-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="0c339-194">Например если требуется отобразить содержимое таблицы Products базы данных в представлении, следует передавать коллекции базы данных записей в представлении данных.</span><span class="sxs-lookup"><span data-stu-id="0c339-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="0c339-195">Вы также можете передачи данных строго типизированного представления из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="0c339-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="0c339-196">В этом разделе мы исследуем в учебнике **основные сведения о строго типизированного представления данных и представления**.</span><span class="sxs-lookup"><span data-stu-id="0c339-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="0c339-197">Сводка</span><span class="sxs-lookup"><span data-stu-id="0c339-197">Summary</span></span>

<span data-ttu-id="0c339-198">Этот учебник предоставляется краткое введение в ASP.NET MVC представления данных в представлении и вспомогательные методы HTML.</span><span class="sxs-lookup"><span data-stu-id="0c339-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="0c339-199">В первом разделе вы узнали, как добавить новые представления в проект.</span><span class="sxs-lookup"><span data-stu-id="0c339-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="0c339-200">Вы узнали, что необходимо добавить представление в папку правой вызывать из конкретного контроллера.</span><span class="sxs-lookup"><span data-stu-id="0c339-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="0c339-201">Далее мы рассмотрели разделе вспомогательные методы HTML.</span><span class="sxs-lookup"><span data-stu-id="0c339-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="0c339-202">Вы узнали, как вспомогательных методов HTML, которые позволяют легко создавать стандартные HTML-содержимого.</span><span class="sxs-lookup"><span data-stu-id="0c339-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="0c339-203">Наконец вы узнали, как пользоваться преимуществами данные представления для передачи данных из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="0c339-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0c339-204">[Назад](passing-data-to-view-master-pages-cs.md)
> [Вперед](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0c339-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
