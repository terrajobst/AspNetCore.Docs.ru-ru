---
title: Введение в Razor Pages в ASP.NET Core
author: Rick-Anderson
description: Сведения о применении в ASP.NET Core функции Razor Pages, которая делает создание кодов сценариев для страниц проще и эффективнее по сравнению с MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 5/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: 651d47ce20f3269340f0796f487e2f1a2a155710
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="c88a6-103">Введение в Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c88a6-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="c88a6-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Райан Новак](https://github.com/rynowak) (Ryan Nowak)</span><span class="sxs-lookup"><span data-stu-id="c88a6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="c88a6-105">Razor Pages — это новый аспект платформы MVC ASP.NET Core, который делает создание кодов сценариев для страниц проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="c88a6-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="c88a6-106">Если вам нужно руководство, использующее подход "модель-представление-контроллер", см. статью [Начало работы с MVC в ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="c88a6-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="c88a6-107">Этот документ содержит вводные сведения о Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c88a6-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="c88a6-108">Это не пошаговое руководство.</span><span class="sxs-lookup"><span data-stu-id="c88a6-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="c88a6-109">Если некоторые разделы покажутся вам слишком сложными, см. [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="c88a6-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="c88a6-110">Общие сведения об ASP.NET Core см. в разделе [Введение в ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="c88a6-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c88a6-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c88a6-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="c88a6-112">Создание проекта Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c88a6-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c88a6-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c88a6-113">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="c88a6-114">Подробные инструкции по созданию проекта Razor Pages с помощью Visual Studio см. в статье [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="c88a6-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c88a6-115">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="c88a6-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="c88a6-116">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-116">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="c88a6-117">Откройте созданный файл *.csproj* в Visual Studio для Mac.</span><span class="sxs-lookup"><span data-stu-id="c88a6-117">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c88a6-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c88a6-118">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="c88a6-119">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-119">Run `dotnet new razor` from the command line.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c88a6-120">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="c88a6-120">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="c88a6-121">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-121">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="c88a6-122">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c88a6-122">Razor Pages</span></span>

<span data-ttu-id="c88a6-123">Функция Razor Pages активируется в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c88a6-123">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="c88a6-124">Рассмотрим простую страницу: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="c88a6-124">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="c88a6-125">Представленный выше код выглядит как файл представления Razor</span><span class="sxs-lookup"><span data-stu-id="c88a6-125">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="c88a6-126">и отличается от него только директивой `@page`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-126">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="c88a6-127">Директива `@page` превращает файл в действие MVC, а значит обрабатывает запросы напрямую, минуя контроллер.</span><span class="sxs-lookup"><span data-stu-id="c88a6-127">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="c88a6-128">Директива `@page` должна быть первой директивой Razor на странице.</span><span class="sxs-lookup"><span data-stu-id="c88a6-128">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="c88a6-129">Директива `@page` влияет на поведение всех остальных конструкций Razor.</span><span class="sxs-lookup"><span data-stu-id="c88a6-129">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="c88a6-130">Похожая страница с использованием класса `PageModel` показана в следующих двух файлах.</span><span class="sxs-lookup"><span data-stu-id="c88a6-130">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="c88a6-131">Файл *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c88a6-131">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="c88a6-132">Модель страницы *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c88a6-132">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="c88a6-133">Как правило, файл класса `PageModel` называется так же, как файл Razor Pages, но с расширением *.cs*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-133">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="c88a6-134">Например, представленная выше страница Razor Pages называется *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-134">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="c88a6-135">Файл, содержащий класс `PageModel`, называется *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-135">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="c88a6-136">Сопоставления URL-адресов со страницами определяются расположением конкретной страницы в файловой системе.</span><span class="sxs-lookup"><span data-stu-id="c88a6-136">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="c88a6-137">В приведенной ниже таблице показаны пути Razor Pages и соответствующие URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="c88a6-137">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="c88a6-138">Имя файла и путь</span><span class="sxs-lookup"><span data-stu-id="c88a6-138">File name and path</span></span>               | <span data-ttu-id="c88a6-139">Соответствующий URL</span><span class="sxs-lookup"><span data-stu-id="c88a6-139">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="c88a6-140">*/Pages/index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c88a6-140">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="c88a6-141">`/` или `/Index`</span><span class="sxs-lookup"><span data-stu-id="c88a6-141">`/` or `/Index`</span></span> |
| <span data-ttu-id="c88a6-142">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c88a6-142">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="c88a6-143">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c88a6-143">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="c88a6-144">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c88a6-144">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="c88a6-145">`/Store` или `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="c88a6-145">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="c88a6-146">Примечания.</span><span class="sxs-lookup"><span data-stu-id="c88a6-146">Notes:</span></span>

* <span data-ttu-id="c88a6-147">Среда выполнения по умолчанию ищет файлы Razor Pages в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-147">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="c88a6-148">Если в URL-адресе не указана конкретная страница, по умолчанию открывается страница `Index`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-148">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="c88a6-149">Создание простой формы</span><span class="sxs-lookup"><span data-stu-id="c88a6-149">Writing a basic form</span></span>

<span data-ttu-id="c88a6-150">Функция Razor Pages предназначена для упрощения реализации типовых шаблонов, которые используются в браузерах, при создании приложения.</span><span class="sxs-lookup"><span data-stu-id="c88a6-150">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="c88a6-151">[Привязки модели](xref:mvc/models/model-binding), [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и вспомогательные методы HTML *отлично работают* со свойствами, определенными в классе Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c88a6-151">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="c88a6-152">Рассмотрим страницу с простой формой связи для модели `Contact`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-152">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="c88a6-153">В представленных в этой статье примерах `DbContext` инициализируется в файле [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="c88a6-153">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="c88a6-154">Модель данных:</span><span class="sxs-lookup"><span data-stu-id="c88a6-154">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="c88a6-155">Контекст базы данных:</span><span class="sxs-lookup"><span data-stu-id="c88a6-155">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="c88a6-156">Файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c88a6-156">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="c88a6-157">Модель страницы *Pages/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c88a6-157">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="c88a6-158">Как правило, класс `PageModel` называется `<PageName>Model` и находится в том же пространстве имен, что и страница.</span><span class="sxs-lookup"><span data-stu-id="c88a6-158">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="c88a6-159">Класс `PageModel` позволяет разделять логику страницы и ее представление.</span><span class="sxs-lookup"><span data-stu-id="c88a6-159">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="c88a6-160">Он определяет обработчики страницы для запросов, отправляемых на страницу, а также данные для ее визуализации.</span><span class="sxs-lookup"><span data-stu-id="c88a6-160">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="c88a6-161">Такое разделение позволяет управлять зависимостями страницы путем их [внедрения](xref:fundamentals/dependency-injection) и выполнять [модульное тестирование](xref:testing/razor-pages-testing) страниц.</span><span class="sxs-lookup"><span data-stu-id="c88a6-161">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="c88a6-162">Страница содержит *метод обработчика* `OnPostAsync`, который выполняется по запросам `POST` (когда пользователь публикует форму).</span><span class="sxs-lookup"><span data-stu-id="c88a6-162">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="c88a6-163">Методы обработчика можно добавить для любой HTTP-команды.</span><span class="sxs-lookup"><span data-stu-id="c88a6-163">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="c88a6-164">Наиболее распространенные обработчики</span><span class="sxs-lookup"><span data-stu-id="c88a6-164">The most common handlers are:</span></span>

* <span data-ttu-id="c88a6-165">`OnGet` — инициализация необходимого для страницы состояния.</span><span class="sxs-lookup"><span data-stu-id="c88a6-165">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="c88a6-166">Пример обработчика [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="c88a6-166">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="c88a6-167">`OnPost` — обработка отправленных через форму данных.</span><span class="sxs-lookup"><span data-stu-id="c88a6-167">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="c88a6-168">Суффикс `Async` не является обязательным, но часто используется для асинхронных функций.</span><span class="sxs-lookup"><span data-stu-id="c88a6-168">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="c88a6-169">Код `OnPostAsync` в приведенном выше примере похож на то, что обычно пишут в контроллере.</span><span class="sxs-lookup"><span data-stu-id="c88a6-169">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="c88a6-170">Этот код типичен для Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c88a6-170">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="c88a6-171">Большинство примитивов MVC, включая [привязку модели](xref:mvc/models/model-binding), [проверку](xref:mvc/models/validation) и результаты действий, являются общими.</span><span class="sxs-lookup"><span data-stu-id="c88a6-171">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="c88a6-172">Предыдущий метод `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="c88a6-172">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="c88a6-173">Простая схема `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="c88a6-173">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="c88a6-174">Проверка на наличие ошибок проверки.</span><span class="sxs-lookup"><span data-stu-id="c88a6-174">Check for validation errors.</span></span>

*  <span data-ttu-id="c88a6-175">Если ошибок нет, сохранение данных и перенаправление.</span><span class="sxs-lookup"><span data-stu-id="c88a6-175">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="c88a6-176">Если есть ошибки, отображение страницы с сообщениями проверки.</span><span class="sxs-lookup"><span data-stu-id="c88a6-176">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="c88a6-177">Проверка на стороне клиента выполняется точно так же, как в традиционных приложениях MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c88a6-177">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="c88a6-178">Во многих случаях ошибки проверки выявляются на клиентском компьютере и на сервер не отправляются.</span><span class="sxs-lookup"><span data-stu-id="c88a6-178">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="c88a6-179">После успешного ввода данных метод обработчика `OnPostAsync` вызывает метод обработчика `RedirectToPage`, чтобы получить экземпляр `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-179">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="c88a6-180">`RedirectToPage` — это новый результат действия, аналогичный `RedirectToAction` или `RedirectToRoute`, но настроенный для страниц.</span><span class="sxs-lookup"><span data-stu-id="c88a6-180">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="c88a6-181">В приведенном выше примере он выполняет перенаправление на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="c88a6-181">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="c88a6-182">Более подробно `RedirectToPage` рассматривается в разделе [Создание URL для страниц](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="c88a6-182">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="c88a6-183">Если в отправленной форме имеются ошибки проверки (переданные на сервер), метод обработчика `OnPostAsync` вызывает метод обработчика `Page`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-183">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="c88a6-184">`Page` возвращает экземпляр `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-184">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="c88a6-185">Возвращение `Page` аналогично тому, как действия в контроллерах возвращают `View`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-185">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="c88a6-186">`PageResult` — значение типа возвращаемого значения <!-- Review  --> по умолчанию для метода обработчика.</span><span class="sxs-lookup"><span data-stu-id="c88a6-186">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="c88a6-187">Метод обработчика, вернувший `void`, визуализирует страницу.</span><span class="sxs-lookup"><span data-stu-id="c88a6-187">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="c88a6-188">Для указания согласия на привязку модели в свойстве `Customer` используется атрибут `[BindProperty]`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-188">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="c88a6-189">По умолчанию функция Razor Pages привязывает свойства ко всем командам, кроме GET.</span><span class="sxs-lookup"><span data-stu-id="c88a6-189">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="c88a6-190">Привязка к свойствам позволяет сократить объем необходимого кода.</span><span class="sxs-lookup"><span data-stu-id="c88a6-190">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="c88a6-191">Привязка уменьшает код за счет того, что для визуализации полей формы (`<input asp-for="Customer.Name" />`) и получения входных данных используется одно и то же свойство.</span><span class="sxs-lookup"><span data-stu-id="c88a6-191">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="c88a6-192">В целях обеспечения безопасности следует привязать данные запросов GET к свойствам модели страниц.</span><span class="sxs-lookup"><span data-stu-id="c88a6-192">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="c88a6-193">Проверьте введенные данные пользователя, прежде чем сопоставлять их со свойствами.</span><span class="sxs-lookup"><span data-stu-id="c88a6-193">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="c88a6-194">Это поведение полезно в случаях, когда используются строки запросов и значения маршрутов.</span><span class="sxs-lookup"><span data-stu-id="c88a6-194">Opting in to this behavior is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="c88a6-195">Чтобы привязать свойство к запросам GET, задайте для свойства `SupportsGet` атрибута `[BindProperty]` значение `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="c88a6-195">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="c88a6-196">Домашняя страница (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="c88a6-196">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="c88a6-197">Файл кода программной части *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c88a6-197">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="c88a6-198">Файл *Index.cshtml* содержит следующую разметку для создания ссылки на правку для каждого контактного лица.</span><span class="sxs-lookup"><span data-stu-id="c88a6-198">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="c88a6-199">[Вспомогательный тег привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) использует атрибут `asp-route-{value}` для создания ссылки на страницу редактирования.</span><span class="sxs-lookup"><span data-stu-id="c88a6-199">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="c88a6-200">Эта ссылка содержит данные о маршруте с идентификатором контактного лица.</span><span class="sxs-lookup"><span data-stu-id="c88a6-200">The link contains route data with the contact ID.</span></span> <span data-ttu-id="c88a6-201">Например, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-201">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="c88a6-202">Файл *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c88a6-202">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="c88a6-203">Первая строка содержит директиву `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-203">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="c88a6-204">Ограничение маршрутизации `"{id:int}"` указывает, что страница должна принимать обращенные к ней запросы, которые содержат данные о маршруте `int`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-204">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="c88a6-205">Если запрос к странице не содержит данные о маршруте, которые можно конвертировать в `int`, среда выполнения возвращает ошибку HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="c88a6-205">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="c88a6-206">Чтобы сделать идентификатор необязательным, добавьте `?` к ограничению маршрута:</span><span class="sxs-lookup"><span data-stu-id="c88a6-206">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="c88a6-207">Файл *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c88a6-207">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="c88a6-208">Файл *Index.cshtml* также содержит разметку для создания кнопки удаления у каждого контакта клиента:</span><span class="sxs-lookup"><span data-stu-id="c88a6-208">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="c88a6-209">Во время обработки кнопки удаления в HTML ее `formaction` включает параметры для следующего:</span><span class="sxs-lookup"><span data-stu-id="c88a6-209">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="c88a6-210">Идентификатор контакта клиента, указанный атрибутом `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-210">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="c88a6-211">Параметр `handler`, указанный атрибутом `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-211">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="c88a6-212">Пример отображенной кнопки удаления с идентификатором контакта клиента `1`:</span><span class="sxs-lookup"><span data-stu-id="c88a6-212">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="c88a6-213">При выборе кнопки на сервер отправляется запрос формы `POST`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-213">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="c88a6-214">По соглашению имя метода обработчика выбирается на основе значения параметра `handler` в соответствии со схемой `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-214">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="c88a6-215">Так как `handler` — `delete` в этом примере, метод обработчика `OnPostDeleteAsync` используется для обработки запроса `POST`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-215">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="c88a6-216">Если `asp-page-handler` имеет другое значение, например `remove`, выбирается метод обработчика страницы с именем `OnPostRemoveAsync`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-216">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="c88a6-217">Метод `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="c88a6-217">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="c88a6-218">Принимает `id` из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="c88a6-218">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="c88a6-219">Отправляет в базу данных запрос контакта клиента с `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-219">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="c88a6-220">Если контакт клиента найден, он удаляется из списка контактов.</span><span class="sxs-lookup"><span data-stu-id="c88a6-220">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="c88a6-221">База данных обновляется.</span><span class="sxs-lookup"><span data-stu-id="c88a6-221">The database is updated.</span></span>
* <span data-ttu-id="c88a6-222">Вызывает `RedirectToPage` для перенаправления на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="c88a6-222">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-required"></a><span data-ttu-id="c88a6-223">Маркировка свойств страницы как обязательных</span><span class="sxs-lookup"><span data-stu-id="c88a6-223">Mark page properties required</span></span>

<span data-ttu-id="c88a6-224">Свойства класса `PageModel` можно отметить атрибутом [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute):</span><span class="sxs-lookup"><span data-stu-id="c88a6-224">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

<span data-ttu-id="c88a6-225">[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]</span><span class="sxs-lookup"><span data-stu-id="c88a6-225">[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]</span></span>

<span data-ttu-id="c88a6-226">Дополнительные сведения см. в статье о [проверке модели](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="c88a6-226">See [Model validation](xref:mvc/models/validation) for more information.</span></span>

## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="c88a6-227">Управление запросами HEAD с помощью обработчика OnGet</span><span class="sxs-lookup"><span data-stu-id="c88a6-227">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="c88a6-228">Обработчик HEAD обычно создается и вызывается для выполнения запросов HEAD:</span><span class="sxs-lookup"><span data-stu-id="c88a6-228">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span>

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="c88a6-229">Если обработчик HEAD (`OnHead`) не определен, Razor Pages выполнит вызов обработчика страниц GET (`OnGet`) в ASP.NET Core 2.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="c88a6-229">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="c88a6-230">Вы можете явно выбрать такое поведение с помощью [метода SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) в `Startup.Configure` ASP.NET Core 2.1–2.x:</span><span class="sxs-lookup"><span data-stu-id="c88a6-230">Opt in to this behavior with the [SetCompatibilityVersion method](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) in `Startup.Configure` for ASP.NET Core 2.1 to 2.x:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="c88a6-231">`SetCompatibilityVersion` задает для параметра `AllowMappingHeadRequestsToGetHandler` Razor Pages значение `true`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-231">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="c88a6-232">Вместо включения всех возможных поведений для версии 2.1 с помощью метода `SetCompatibilityVersion` вы можете задать конкретное поведение.</span><span class="sxs-lookup"><span data-stu-id="c88a6-232">Rather than opting into all 2.1 behaviors with `SetCompatibilityVersion`, you can explicitly opt-in to specific behaviors.</span></span> <span data-ttu-id="c88a6-233">Следующий код назначает запросам HEAD обработчик GET.</span><span class="sxs-lookup"><span data-stu-id="c88a6-233">The following code opts into the mapping HEAD requests to the GET handler.</span></span>


```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```
::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="c88a6-234">XSRF/CSRF и Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c88a6-234">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="c88a6-235">Вам не придется писать отдельный код для [проверки подлинности запросов](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="c88a6-235">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="c88a6-236">Razor Pages включает создание и проверку маркеров защиты от подделок по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c88a6-236">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="c88a6-237">Использование макетов, частичных реплик, шаблонов и вспомогательных функций тегов с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c88a6-237">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="c88a6-238">Pages работает со всеми функциями подсистемы просмотра Razor.</span><span class="sxs-lookup"><span data-stu-id="c88a6-238">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="c88a6-239">Макеты, частичные реплики, шаблоны, вспомогательные функции тегов, а также файлы *_ViewStart.cshtml* и *_ViewImports.cshtml* работают точно так же, как и в стандартных представлениях Razor.</span><span class="sxs-lookup"><span data-stu-id="c88a6-239">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="c88a6-240">Давайте упростим нашу страницу с помощью некоторых из этих функций.</span><span class="sxs-lookup"><span data-stu-id="c88a6-240">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="c88a6-241">Добавим [макет страницы](xref:mvc/views/layout) в файл *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c88a6-241">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="c88a6-242">Этот [макет](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="c88a6-242">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="c88a6-243">управляет макетом каждой страницы (кроме страниц с отказом от макета);</span><span class="sxs-lookup"><span data-stu-id="c88a6-243">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="c88a6-244">импортирует HTML-структуры, такие как JavaScript и таблицы стилей.</span><span class="sxs-lookup"><span data-stu-id="c88a6-244">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="c88a6-245">Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="c88a6-245">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="c88a6-246">Свойство [Layout](xref:mvc/views/layout#specifying-a-layout) определяется в файле *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c88a6-246">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="c88a6-247">Макет хранится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-247">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="c88a6-248">Pages ищет другие представления (макеты, шаблоны, частичные реплики) в иерархическом порядке, начиная с той папки, где находится текущая страница.</span><span class="sxs-lookup"><span data-stu-id="c88a6-248">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="c88a6-249">Макет в папке *Pages* можно использовать на любой странице Razor, которая находится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-249">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="c88a6-250">Корпорация Майкрософт рекомендует **не** размещать файл макета в папке *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-250">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="c88a6-251">*Views/Shared* — это шаблон представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="c88a6-251">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="c88a6-252">Razor Pages опирается на иерархию папок, а не на условные обозначения путей.</span><span class="sxs-lookup"><span data-stu-id="c88a6-252">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="c88a6-253">Поиск представлений в Razor Pages охватывает папку *Pages*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-253">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="c88a6-254">Макеты, шаблоны и частичные реплики *работают* с контроллерами MVC и стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="c88a6-254">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="c88a6-255">Добавим файл *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c88a6-255">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="c88a6-256">`@namespace` описывается далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="c88a6-256">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="c88a6-257">Директива `@addTagHelper` добавляет [встроенные вспомогательные теги](xref:mvc/views/tag-helpers/builtin-th/Index) на все страницы в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-257">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="c88a6-258">Явное применение директивы `@namespace` на странице:</span><span class="sxs-lookup"><span data-stu-id="c88a6-258">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="c88a6-259">Данная директива задает пространство имен для страницы.</span><span class="sxs-lookup"><span data-stu-id="c88a6-259">The directive sets the namespace for the page.</span></span> <span data-ttu-id="c88a6-260">Включать пространство имен в директиву `@model` не требуется.</span><span class="sxs-lookup"><span data-stu-id="c88a6-260">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="c88a6-261">Если директива `@namespace` содержится в файле *_ViewImports.cshtml*, указанное пространство имен определяет префикс для созданного в Pages пространства имен, куда импортируется директива `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-261">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="c88a6-262">Остальная часть созданного пространства имен (суффикс) представляет собой разделенный точками относительный путь между папкой с файлом *_ViewImports.cshtml* и папкой, содержащей страницу.</span><span class="sxs-lookup"><span data-stu-id="c88a6-262">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="c88a6-263">Например, файл кода программной части *Pages/Customers/Edit.cshtml.cs* задает пространство имен явно:</span><span class="sxs-lookup"><span data-stu-id="c88a6-263">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="c88a6-264">Файл *Pages/_ViewImports.cshtml* задает следующее пространство имен:</span><span class="sxs-lookup"><span data-stu-id="c88a6-264">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="c88a6-265">Сформированное пространство имен для файла *Pages/Customers/Edit.cshtml* Razor Pages совпадает с именем файла кода программной части.</span><span class="sxs-lookup"><span data-stu-id="c88a6-265">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="c88a6-266">Директива `@namespace` разработана таким образом, чтобы добавляемые в проект классы C# и сгенерированный страницами код *просто работали* без добавления директивы `@using` для файла кода программной части.</span><span class="sxs-lookup"><span data-stu-id="c88a6-266">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="c88a6-267">`@namespace` *также работает со стандартными представлениями Razor.*</span><span class="sxs-lookup"><span data-stu-id="c88a6-267">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="c88a6-268">Исходный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c88a6-268">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="c88a6-269">Обновленный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c88a6-269">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="c88a6-270">[Начальный проект Razor Pages](#rpvs17) содержит файл *Pages/_ValidationScriptsPartial.cshtml*, который подключает проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="c88a6-270">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="c88a6-271">Формирование URL-адресов для страниц</span><span class="sxs-lookup"><span data-stu-id="c88a6-271">URL generation for Pages</span></span>

<span data-ttu-id="c88a6-272">На представленной выше странице `Create` используется `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="c88a6-272">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="c88a6-273">Это приложение имеет следующую структуру файлов и папок:</span><span class="sxs-lookup"><span data-stu-id="c88a6-273">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="c88a6-274">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="c88a6-274">*/Pages*</span></span>

  * <span data-ttu-id="c88a6-275">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c88a6-275">*Index.cshtml*</span></span>
  * <span data-ttu-id="c88a6-276">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="c88a6-276">*/Customers*</span></span>

    * <span data-ttu-id="c88a6-277">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c88a6-277">*Create.cshtml*</span></span>
    * <span data-ttu-id="c88a6-278">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c88a6-278">*Edit.cshtml*</span></span>
    * <span data-ttu-id="c88a6-279">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c88a6-279">*Index.cshtml*</span></span>

<span data-ttu-id="c88a6-280">После успешного выполнения страницы *Pages/Customers/Create.cshtml* и *Pages/Customers/Edit.cshtml* перенаправляются на страницу *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-280">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="c88a6-281">Строка `/Index` составляет часть URL-адреса для доступа к указанной выше странице.</span><span class="sxs-lookup"><span data-stu-id="c88a6-281">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="c88a6-282">Строка `/Index` позволяет создавать URL-адреса для страницы *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-282">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="c88a6-283">Пример:</span><span class="sxs-lookup"><span data-stu-id="c88a6-283">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="c88a6-284">Имя страницы — это путь к странице из корневой папки */Pages*, включая начальный символ `/` (например, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="c88a6-284">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="c88a6-285">Предыдущие образцы создания URL-адреса обеспечивают расширенные параметры и функциональные возможности по сравнению с жестким заданием URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="c88a6-285">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="c88a6-286">Формирование URL-адресов включает [маршрутизацию](xref:mvc/controllers/routing) и позволяет генерировать и включать в код параметры в зависимости от того, как определяется маршрут в пути назначения.</span><span class="sxs-lookup"><span data-stu-id="c88a6-286">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="c88a6-287">Формирование URL-адресов для страниц поддерживает относительные имена.</span><span class="sxs-lookup"><span data-stu-id="c88a6-287">URL generation for pages supports relative names.</span></span> <span data-ttu-id="c88a6-288">В приведенной ниже таблице показано, какая страница индекса выбирается с каждым параметром `RedirectToPage` в *Pages/Customers/Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-288">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="c88a6-289">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="c88a6-289">RedirectToPage(x)</span></span>| <span data-ttu-id="c88a6-290">Страница</span><span class="sxs-lookup"><span data-stu-id="c88a6-290">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="c88a6-291">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="c88a6-291">RedirectToPage("/Index")</span></span> | <span data-ttu-id="c88a6-292">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="c88a6-292">*Pages/Index*</span></span> |
| <span data-ttu-id="c88a6-293">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="c88a6-293">RedirectToPage("./Index");</span></span> | <span data-ttu-id="c88a6-294">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="c88a6-294">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="c88a6-295">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="c88a6-295">RedirectToPage("../Index")</span></span> | <span data-ttu-id="c88a6-296">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="c88a6-296">*Pages/Index*</span></span> |
| <span data-ttu-id="c88a6-297">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="c88a6-297">RedirectToPage("Index")</span></span>  | <span data-ttu-id="c88a6-298">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="c88a6-298">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="c88a6-299">`RedirectToPage("Index")`, `RedirectToPage("./Index")` и `RedirectToPage("../Index")` — это <em>относительные имена</em>.</span><span class="sxs-lookup"><span data-stu-id="c88a6-299">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="c88a6-300">Для получения имени целевой страницы параметр `RedirectToPage` <em>комбинируется</em> с путем текущей страницы.</span><span class="sxs-lookup"><span data-stu-id="c88a6-300">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="c88a6-301">Привязка относительных имен полезна при создании сайтов со сложной структурой.</span><span class="sxs-lookup"><span data-stu-id="c88a6-301">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="c88a6-302">Если для связи между страницами в определенной папке используются относительные имена, эту папку можно переименовать.</span><span class="sxs-lookup"><span data-stu-id="c88a6-302">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="c88a6-303">При этом все ссылки останутся рабочими (так как не включают имя папки).</span><span class="sxs-lookup"><span data-stu-id="c88a6-303">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="viewdata-attribute"></a><span data-ttu-id="c88a6-304">Атрибут ViewData</span><span class="sxs-lookup"><span data-stu-id="c88a6-304">ViewData attribute</span></span>

<span data-ttu-id="c88a6-305">Данные могут передаваться на страницу с помощью атрибута [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="c88a6-305">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="c88a6-306">Свойства на контроллерах или моделях страниц Razor, отмеченные атрибутом `[ViewData]`, обладают своими собственными значениями, загружаемыми из [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="c88a6-306">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="c88a6-307">В следующем примере класс `AboutModel` содержит свойство `Title`, отмеченное атрибутом `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-307">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="c88a6-308">Свойство `Title` задает заголовок страницы About.</span><span class="sxs-lookup"><span data-stu-id="c88a6-308">The `Title` property is set to the title of the About page:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="c88a6-309">На странице About доступ к свойству `Title` осуществляется как доступ к свойству модели.</span><span class="sxs-lookup"><span data-stu-id="c88a6-309">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="c88a6-310">В макете заголовок считывается из словаря ViewData.</span><span class="sxs-lookup"><span data-stu-id="c88a6-310">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```
::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="c88a6-311">TempData</span><span class="sxs-lookup"><span data-stu-id="c88a6-311">TempData</span></span>

<span data-ttu-id="c88a6-312">ASP.NET Core позволяет использовать свойство [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) в [контроллере](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="c88a6-312">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="c88a6-313">Это свойство хранит данные до тех пор, пока они не будут прочитаны.</span><span class="sxs-lookup"><span data-stu-id="c88a6-313">This property stores data until it's read.</span></span> <span data-ttu-id="c88a6-314">Для проверки данных без удаления можно использовать методы `Keep` и `Peek`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-314">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="c88a6-315">`TempData` удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса.</span><span class="sxs-lookup"><span data-stu-id="c88a6-315">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="c88a6-316">Атрибут `[TempData]` появился в ASP.NET 2.0 и поддерживается в контроллерах и на страницах.</span><span class="sxs-lookup"><span data-stu-id="c88a6-316">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="c88a6-317">В следующем коде значение `Message` задается с помощью `TempData`:</span><span class="sxs-lookup"><span data-stu-id="c88a6-317">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="c88a6-318">Представленная ниже разметка в файле *Pages/Customers/Index.cshtml* отображает значение `Message` с помощью `TempData`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-318">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="c88a6-319">Модель страницы *Pages/Customers/Index.cshtml.cs* применяет атрибут `[TempData]` к свойству `Message`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-319">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="c88a6-320">Дополнительные сведения см. в разделе [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="c88a6-320">See [TempData](xref:fundamentals/app-state#tempdata) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="c88a6-321">Несколько обработчиков на страницу</span><span class="sxs-lookup"><span data-stu-id="c88a6-321">Multiple handlers per page</span></span>

<span data-ttu-id="c88a6-322">Следующая страница формирует разметку для двух обработчиков страницы с помощью вспомогательного тега `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="c88a6-322">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="c88a6-323">Форма в предыдущем примере включает две кнопки отправки, каждая из которых отправляет данные на отдельный URL-адрес с помощью `FormActionTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-323">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="c88a6-324">Атрибут `asp-page-handler` является дополнением к `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-324">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="c88a6-325">Атрибут `asp-page-handler` формирует URL-адреса, ,которые используются для отправки данных в каждый из методов обработчиков, определенных страницей.</span><span class="sxs-lookup"><span data-stu-id="c88a6-325">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="c88a6-326">`asp-page` не задается, так как пример сопоставлен с текущей страницей.</span><span class="sxs-lookup"><span data-stu-id="c88a6-326">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="c88a6-327">Модель страницы</span><span class="sxs-lookup"><span data-stu-id="c88a6-327">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="c88a6-328">В представленном выше коде используются *именованные методы обработчика*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-328">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="c88a6-329">Именованные методы обработчика создаются путем размещения определенного текста в имени после `On<HTTP Verb>` и перед `Async` (если есть).</span><span class="sxs-lookup"><span data-stu-id="c88a6-329">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="c88a6-330">В приведенном выше примере использовались методы страницы OnPost**JoinList**Async и OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="c88a6-330">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="c88a6-331">Если убрать *OnPost* и *Async*, имена обработчиков будут выглядеть как `JoinList` и `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-331">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="c88a6-332">При использовании представленного выше кода URL-путь для отправки данных в `OnPostJoinListAsync` будет выглядеть как `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-332">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="c88a6-333">URL-путь для отправки данных в `OnPostJoinListUCAsync` будет иметь вид `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="c88a6-333">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>



## <a name="customizing-routing"></a><span data-ttu-id="c88a6-334">Настройка маршрутизации</span><span class="sxs-lookup"><span data-stu-id="c88a6-334">Customizing Routing</span></span>

<span data-ttu-id="c88a6-335">Если вы не хотите, чтобы в URL-адресе отображалась строка запроса `?handler=JoinList`, измените маршрут таким образом, чтобы в качестве пути в URL-адресе указывалось имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="c88a6-335">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="c88a6-336">Для настройки маршрута можно добавить после директивы `@page` шаблон маршрута, заключенные в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="c88a6-336">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="c88a6-337">В представленном выше маршруте вместо строки запроса URL-путь содержит имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="c88a6-337">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="c88a6-338">Символ `?` после `handler` означает, что параметр маршрута является необязательным.</span><span class="sxs-lookup"><span data-stu-id="c88a6-338">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="c88a6-339">С помощью директивы `@page` можно добавить к маршруту страницы дополнительные сегменты и параметры.</span><span class="sxs-lookup"><span data-stu-id="c88a6-339">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="c88a6-340">Все, что вы добавите, будет **присоединено** к маршруту страницы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c88a6-340">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="c88a6-341">Использовать для изменения маршрута страницы абсолютный или виртуальный путь (например, вида `"~/Some/Other/Path"`) нельзя.</span><span class="sxs-lookup"><span data-stu-id="c88a6-341">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="c88a6-342">Конфигурация и параметры</span><span class="sxs-lookup"><span data-stu-id="c88a6-342">Configuration and settings</span></span>

<span data-ttu-id="c88a6-343">Чтобы настроить дополнительные параметры, воспользуйтесь методом расширения `AddRazorPagesOptions` в построителе MVC:</span><span class="sxs-lookup"><span data-stu-id="c88a6-343">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="c88a6-344">В настоящее время `RazorPagesOptions` позволяет задавать корневую папку для страниц и добавлять для них обозначения моделей приложений.</span><span class="sxs-lookup"><span data-stu-id="c88a6-344">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="c88a6-345">В дальнейшем мы расширим спектр его возможностей.</span><span class="sxs-lookup"><span data-stu-id="c88a6-345">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="c88a6-346">Сведения о предварительной компиляции представлений см. в статье [Компиляция представлений Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="c88a6-346">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="c88a6-347">[Загрузить или просмотреть пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="c88a6-347">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="c88a6-348">См. раздел [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start), который дает дополнительную информацию к введению.</span><span class="sxs-lookup"><span data-stu-id="c88a6-348">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="c88a6-349">Указание местонахождения Razor Pages в корне каталога</span><span class="sxs-lookup"><span data-stu-id="c88a6-349">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="c88a6-350">По умолчанию Razor Pages находится в корне каталога */Pages*.</span><span class="sxs-lookup"><span data-stu-id="c88a6-350">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="c88a6-351">Добавьте [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в корне содержимого Razor Pages ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) приложения:</span><span class="sxs-lookup"><span data-stu-id="c88a6-351">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="c88a6-352">Указание местонахождения Razor Pages в пользовательском корневом каталоге</span><span class="sxs-lookup"><span data-stu-id="c88a6-352">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="c88a6-353">Добавьте [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в пользовательском корневом каталоге в приложении (укажите относительный путь):</span><span class="sxs-lookup"><span data-stu-id="c88a6-353">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="c88a6-354">См. также</span><span class="sxs-lookup"><span data-stu-id="c88a6-354">See also</span></span>

* [<span data-ttu-id="c88a6-355">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c88a6-355">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="c88a6-356">Синтаксис Razor</span><span class="sxs-lookup"><span data-stu-id="c88a6-356">Razor syntax</span></span>](xref:mvc/views/razor)
* [<span data-ttu-id="c88a6-357">Начало работы с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c88a6-357">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="c88a6-358">Соглашения об авторизации Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c88a6-358">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="c88a6-359">Пользовательские поставщики моделей маршрутов и страниц Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c88a6-359">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-conventions)
* [<span data-ttu-id="c88a6-360">Модульные и интеграционные тесты для Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c88a6-360">Razor Pages unit and integration tests</span></span>](xref:testing/razor-pages-testing)
