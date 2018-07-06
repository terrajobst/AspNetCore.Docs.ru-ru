---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: Общие сведения о (C#) представлениях ASP.NET MVC | Документация Майкрософт
author: StephenWalther
description: Что такое представлении MVC ASP.NET, и чем она отличается от HTML-страницы? В этом руководстве Стивен Вальтер знакомит вас с представлениями и демонстрирует, как можно t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: adf995529b34c84969125adcc6249ba8e22a7af4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387794"
---
<a name="aspnet-mvc-views-overview-c"></a><span data-ttu-id="6b6c8-104">Общие сведения о (C#) представлениях ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6b6c8-104">ASP.NET MVC Views Overview (C#)</span></span>
====================
<span data-ttu-id="6b6c8-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="6b6c8-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="6b6c8-106">Что такое представлении MVC ASP.NET, и чем она отличается от HTML-страницы?</span><span class="sxs-lookup"><span data-stu-id="6b6c8-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="6b6c8-107">В этом руководстве Стивен Вальтер знакомит вас с представлениями и показано, как можно воспользоваться преимуществами Просмотр данных и вспомогательных методов HTML в представлении.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="6b6c8-108">Цель этого руководства является предоставить Краткое введение в представления ASP.NET MVC, просмотр данных и вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="6b6c8-109">В конце этого руководства необходимо понять, как создавать новые представления и передачи данных из контроллера в представление позволяет создавать содержимое в виде вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="6b6c8-110">Общие сведения о представлениях</span><span class="sxs-lookup"><span data-stu-id="6b6c8-110">Understanding Views</span></span>

<span data-ttu-id="6b6c8-111">Для ASP.NET или страницы ASP ASP.NET MVC не включает все, что соответствует непосредственно на страницу.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-111">For ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="6b6c8-112">В приложении ASP.NET MVC не страницы на диск, который соответствует пути в URL-адрес, который вы вводите в адресной строке браузера.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="6b6c8-113">-Это ближайший страницы в приложении ASP.NET MVC, то вызывается *представление*.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="6b6c8-114">ASP.NET MVC браузера, приложения, входящие запросы сопоставляются действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-114">ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="6b6c8-115">Действие контроллера может вернуть представление.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-115">A controller action might return a view.</span></span> <span data-ttu-id="6b6c8-116">Тем не менее действия контроллера могут применяться другие действия, такие как перенаправление для другого действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="6b6c8-117">В листинге 1 содержит простой контроллер с именем HomeController.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="6b6c8-118">HomeController предоставляет два действия контроллера с именем Index() и Details().</span><span class="sxs-lookup"><span data-stu-id="6b6c8-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="6b6c8-119">**В листинге 1 - HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="6b6c8-119">**Listing 1 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

<span data-ttu-id="6b6c8-120">Первое действие, действие Index(), можно вызвать, введя следующий URL-адрес в адресной строке браузера:</span><span class="sxs-lookup"><span data-stu-id="6b6c8-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="6b6c8-121">/ Home/Index</span><span class="sxs-lookup"><span data-stu-id="6b6c8-121">/Home/Index</span></span>

<span data-ttu-id="6b6c8-122">Второе действие, действие Details(), можно вызвать, введя этот адрес в адресную строку браузера:</span><span class="sxs-lookup"><span data-stu-id="6b6c8-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="6b6c8-123">/ Home/сведения</span><span class="sxs-lookup"><span data-stu-id="6b6c8-123">/Home/Details</span></span>

<span data-ttu-id="6b6c8-124">Index() действие возвращает представление.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-124">The Index() action returns a view.</span></span> <span data-ttu-id="6b6c8-125">Большинство операций, создаваемых вернет представления.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-125">Most actions that you create will return views.</span></span> <span data-ttu-id="6b6c8-126">Тем не менее действия могут возвращать другие типы результатов действий.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="6b6c8-127">Например действие Details() возвращает RedirectToActionResult, который перенаправляет входящий запрос Index() действие.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="6b6c8-128">Действие Index() содержит следующие одной строки кода:</span><span class="sxs-lookup"><span data-stu-id="6b6c8-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="6b6c8-129">View();</span><span class="sxs-lookup"><span data-stu-id="6b6c8-129">View();</span></span>

<span data-ttu-id="6b6c8-130">Эта строка кода возвращает представление, которое должен быть расположен по следующему пути веб-сервера:</span><span class="sxs-lookup"><span data-stu-id="6b6c8-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="6b6c8-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="6b6c8-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="6b6c8-132">Путь к представлению выводится из имени контроллера и имя действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="6b6c8-133">При желании можно явно задать, какие представлении.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="6b6c8-134">Следующая строка кода возвращает представление с именем Fred:</span><span class="sxs-lookup"><span data-stu-id="6b6c8-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="6b6c8-135">Представление (Fred);</span><span class="sxs-lookup"><span data-stu-id="6b6c8-135">View( Fred );</span></span>

<span data-ttu-id="6b6c8-136">Когда выполняется эта строка кода, представление возвращается из следующий путь:</span><span class="sxs-lookup"><span data-stu-id="6b6c8-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="6b6c8-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="6b6c8-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="6b6c8-138">Если вы планируете создавать модульные тесты для своего приложения ASP.NET MVC является хорошей идеей явное имена представлений.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="6b6c8-139">Таким образом, можно создать модульный тест, чтобы убедиться, что ожидаемый Просмотр вернула действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="6b6c8-140">Добавление содержимого в представление</span><span class="sxs-lookup"><span data-stu-id="6b6c8-140">Adding Content to a View</span></span>

<span data-ttu-id="6b6c8-141">Представление — это стандарт (X) HTML-документ, который может содержать скриптов.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="6b6c8-142">Использовать сценарии для добавления динамического содержимого в представление.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="6b6c8-143">Например представление в листинге 2 отображает текущую дату и время.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="6b6c8-144">**В листинге 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="6b6c8-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

<span data-ttu-id="6b6c8-145">Обратите внимание на то, что текст HTML-страницы в листинге 2 содержит следующий скрипт:</span><span class="sxs-lookup"><span data-stu-id="6b6c8-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="6b6c8-146">&lt;% Response.Write(DateTime.Now); %&gt;</span><span class="sxs-lookup"><span data-stu-id="6b6c8-146">&lt;% Response.Write(DateTime.Now);%&gt;</span></span>

<span data-ttu-id="6b6c8-147">Используйте разделители скрипт &lt;% и %&gt; чтобы пометить начало и конец сценария.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="6b6c8-148">Этот скрипт написан на C#.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-148">This script is written in C#.</span></span> <span data-ttu-id="6b6c8-149">Отображает текущую дату и время, вызвав метод Response.Write() для отображения содержимого в браузер.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="6b6c8-150">Разделители скрипт &lt;% и %&gt; может использоваться для выполнения одной или нескольких инструкций.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="6b6c8-151">Так как вы вызываете Response.Write() столь часто, корпорация Майкрософт предоставляет вам ярлык для вызова метода Response.Write().</span><span class="sxs-lookup"><span data-stu-id="6b6c8-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="6b6c8-152">Представление в листинге 3 использует разделители &lt;% = и %&gt; для быстрого вызова Response.Write().</span><span class="sxs-lookup"><span data-stu-id="6b6c8-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="6b6c8-153">**Листинг 3 - Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="6b6c8-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

<span data-ttu-id="6b6c8-154">Можно использовать любой язык .NET для создания динамического содержимого в представлении.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="6b6c8-155">Как правило вы сможете использовать Visual Basic .NET или C# для записи контроллерами и представлениями.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="6b6c8-156">С помощью вспомогательных методов HTML для создания представления содержимого</span><span class="sxs-lookup"><span data-stu-id="6b6c8-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="6b6c8-157">Чтобы упростить добавление содержимого в представление, возможность использовать так называемую *вспомогательный метод HTML*.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="6b6c8-158">Вспомогательный метод HTML, как правило, — это метод, который создает строку.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="6b6c8-159">Вспомогательные методы HTML можно использовать для создания стандартных элементов HTML, такие как текстовые поля, ссылки, раскрывающиеся списки и списки.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="6b6c8-160">Например, в представлении в листинге 4 пользуется три вспомогательных методов HTML--BeginForm(), TextBox() и Password() помощников--для создания имени входа форме (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="6b6c8-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="6b6c8-161">**Листинг 4--\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="6b6c8-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


<span data-ttu-id="6b6c8-162">[![В диалоговом окне нового проекта](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6b6c8-162">[![The New Project dialog box](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="6b6c8-163">**Рис 01**: стандартная форма входа в систему ([Просмотр полноразмерного изображения](asp-net-mvc-views-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="6b6c8-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="6b6c8-164">Все методы вспомогательных методов HTML, называются свойства Html представления.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="6b6c8-165">Например подготовке к просмотру текстового поля, вызвав метод Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="6b6c8-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="6b6c8-166">Обратите внимание, что использовать разделители скрипт &lt;% = и %&gt; при вызове Html.TextBox() и Html.Password() вспомогательные функции.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="6b6c8-167">Эти вспомогательные методы просто возвращают строку.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-167">These helpers simply return a string.</span></span> <span data-ttu-id="6b6c8-168">Необходимо вызвать Response.Write() для отображения строки в браузере.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="6b6c8-169">Использование методов вспомогательный метод HTML не является обязательным.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="6b6c8-170">Они упрощают жизнь за счет сокращения HTML и сценарий, который необходимо написать.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="6b6c8-171">Представление в листинге 5 представляет точное же форму, что в представлении в листинге 4 без использования вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="6b6c8-172">**В листинге 5--\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="6b6c8-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

<span data-ttu-id="6b6c8-173">Также имеется возможность создания собственных вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="6b6c8-174">Например можно создать GridView() вспомогательный метод, который автоматически отображает набора записей базы данных в таблице HTML.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="6b6c8-175">В этом разделе рассматривается в этом руководстве **Создание Custom HTML Helpers**.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="6b6c8-176">Используя данные представления для передачи данных в представление</span><span class="sxs-lookup"><span data-stu-id="6b6c8-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="6b6c8-177">Просмотр данных использовать для передачи данных из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="6b6c8-178">Представить данные представления, как и пакет, который вы отправить по почте.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="6b6c8-179">Все данные, передаваемые из контроллера в представление должно быть отправлено с помощью этого пакета.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="6b6c8-180">Например контроллер в листинге 6 добавляет сообщение для просмотра данных.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="6b6c8-181">**В листинге 6 - ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="6b6c8-181">**Listing 6 - ProductController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

<span data-ttu-id="6b6c8-182">Контроллер ViewData свойство представляет коллекцию пар имя-значение.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="6b6c8-183">В листинге 6 Index() метод добавляет элемент в коллекцию представлений данных, с именем сообщения со значением Hello World!.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="6b6c8-184">При представлении возвращается методом Index(), данные представления автоматически передается в представление.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="6b6c8-185">В представлении в листинге 7 извлекает данные из представления сообщения и отображает сообщение в браузер.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="6b6c8-186">**Листинг 7--\Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="6b6c8-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

<span data-ttu-id="6b6c8-187">Обратите внимание на то, что представление использует преимущества метод Html.Encode() вспомогательный метод HTML, при отображении сообщения.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="6b6c8-188">Вспомогательный метод HTML Html.Encode() кодирует специальные символы, такие как &lt; и &gt; в символы, которые являются безопасными для отображения на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="6b6c8-189">Каждый раз, когда вы отображения содержимого, которые пользователь отправляет на веб-сайт, следует кодировать содержимое для предотвращения атак путем внедрения кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="6b6c8-190">(Потому что мы создали сообщения сами ProductController, мы кое t необходима для кодирования сообщения.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="6b6c8-191">Тем не менее это хорошая привычка всегда вызывать метод Html.Encode(), когда отображение содержимого получена из представления данных в представлении.)</span><span class="sxs-lookup"><span data-stu-id="6b6c8-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="6b6c8-192">В листинге 7 мы воспользовались преимуществами данные представления для передачи простых строковое сообщение из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="6b6c8-193">Просмотр данных также можно использовать для передачи других типов данных, например, коллекцию записей базы данных, из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="6b6c8-194">Например если вы хотите отображения содержимого таблицы Products базы данных в представлении, а затем будет передан коллекции базы данных записей в представлении данных.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="6b6c8-195">Вы также можете передачи строго типизированного представления данных из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="6b6c8-196">В этом разделе рассматривается в этом руководстве **основные сведения о строго типизированные представления данных и представления**.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="6b6c8-197">Сводка</span><span class="sxs-lookup"><span data-stu-id="6b6c8-197">Summary</span></span>

<span data-ttu-id="6b6c8-198">Этот учебник предоставляется краткое введение в представления ASP.NET MVC, просмотр данных и вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="6b6c8-199">В первом разделе вы узнали, как добавить новые представления в проект.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="6b6c8-200">Вы узнали, что необходимо добавить представление папку для его вызова из конкретного контроллера.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="6b6c8-201">Далее мы рассмотрели тема вспомогательных методов HTML.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="6b6c8-202">Вы узнали, как вспомогательных методов HTML, которые позволяют легко создать стандартный HTML-содержимое.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="6b6c8-203">Наконец вы узнали, как пользоваться преимуществами представление данных для передачи данных из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="6b6c8-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6b6c8-204">Вперед</span><span class="sxs-lookup"><span data-stu-id="6b6c8-204">Next</span></span>](creating-custom-html-helpers-cs.md)
