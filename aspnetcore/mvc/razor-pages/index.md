---
title: "Введение в Razor Pages в ASP.NET Core"
author: Rick-Anderson
description: "Учебник по ASP.NET Core на страницах Razor. Включает разделы по MVC Core, ASP.NET Core 2.x, введение в веб-разработку и сведения о Visual Studio 2017. Этот документ содержит общие сведения об использовании страниц Razor в ASP.NET Core для упрощения разработки в сценариях, где применяются страницы."
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: c16959189448337b12dc4902300516b25c200eee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="96387-105">Введение в Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96387-105">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="96387-106">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Райан Новак](https://github.com/rynowak) (Ryan Nowak)</span><span class="sxs-lookup"><span data-stu-id="96387-106">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="96387-107">Razor Pages — это новая функция платформы MVC ASP.NET Core, которая делает создание кодов сценариев для страниц проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="96387-107">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="96387-108">Если вам требуется руководство на основе подхода "модель-представление-контроллер", см. статью [Начало работы с MVC ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="96387-108">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="96387-109">Этот документ содержит вводные сведения о Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="96387-109">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="96387-110">Это не пошаговое руководство.</span><span class="sxs-lookup"><span data-stu-id="96387-110">It's not a step by step tutorial.</span></span> <span data-ttu-id="96387-111">Если некоторые разделы покажутся вам сложными, см. документ [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="96387-111">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="96387-112">Предварительные требования ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="96387-112">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="96387-113">Установите [.NET Core](https://www.microsoft.com/net/core) 2.0.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="96387-113">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="96387-114">Если вы используете Visual Studio, установите [Visual Studio](https://www.visualstudio.com/vs/) 2017 версии 15.3 или более поздней версии, а также следующие рабочие нагрузки:</span><span class="sxs-lookup"><span data-stu-id="96387-114">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="96387-115">**ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="96387-115">**ASP.NET and web development**</span></span>
* <span data-ttu-id="96387-116">**Кроссплатформенная разработка .NET Core**</span><span class="sxs-lookup"><span data-stu-id="96387-116">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="96387-117">Создание проекта Razor Pages</span><span class="sxs-lookup"><span data-stu-id="96387-117">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96387-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96387-118">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="96387-119">Подробные инструкции по созданию проекта Razor Pages с помощью Visual Studio см. в статье [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="96387-119">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="96387-120">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="96387-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="96387-121">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="96387-121">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="96387-122">Откройте созданный файл *.csproj* в Visual Studio для Mac.</span><span class="sxs-lookup"><span data-stu-id="96387-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="96387-123">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="96387-123">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="96387-124">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="96387-124">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="96387-125">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="96387-125">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="96387-126">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="96387-126">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="96387-127">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="96387-127">Razor Pages</span></span>

<span data-ttu-id="96387-128">Функция Razor Pages активируется в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="96387-128">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="96387-129">Рассмотрим простую страницу: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="96387-129">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="96387-130">Представленный выше код выглядит как файл представления Razor</span><span class="sxs-lookup"><span data-stu-id="96387-130">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="96387-131">и отличается от него только директивой `@page`.</span><span class="sxs-lookup"><span data-stu-id="96387-131">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="96387-132">Директива `@page` превращает файл в действие MVC, а значит обрабатывает запросы напрямую, минуя контроллер.</span><span class="sxs-lookup"><span data-stu-id="96387-132">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="96387-133">Директива `@page` должна быть первой директивой Razor на странице.</span><span class="sxs-lookup"><span data-stu-id="96387-133">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="96387-134">Директива `@page` влияет на поведение всех остальных конструкций Razor.</span><span class="sxs-lookup"><span data-stu-id="96387-134">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="96387-135">Похожая страница с использованием класса `PageModel` показана в следующих двух файлах.</span><span class="sxs-lookup"><span data-stu-id="96387-135">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="96387-136">Файл *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="96387-136">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="96387-137">Модель страницы *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="96387-137">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="96387-138">Как правило, файл класса `PageModel` называется так же, как файл Razor Pages, но с расширением *.cs*.</span><span class="sxs-lookup"><span data-stu-id="96387-138">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="96387-139">Например, представленная выше страница Razor Pages называется *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="96387-139">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="96387-140">Файл, содержащий класс `PageModel`, называется *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="96387-140">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="96387-141">Сопоставления URL-адресов со страницами определяются расположением конкретной страницы в файловой системе.</span><span class="sxs-lookup"><span data-stu-id="96387-141">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="96387-142">В приведенной ниже таблице показаны пути Razor Pages и соответствующие URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="96387-142">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="96387-143">Имя файла и путь</span><span class="sxs-lookup"><span data-stu-id="96387-143">File name and path</span></span>               | <span data-ttu-id="96387-144">Соответствующий URL</span><span class="sxs-lookup"><span data-stu-id="96387-144">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="96387-145">*/Pages/index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="96387-145">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="96387-146">`/` или `/Index`</span><span class="sxs-lookup"><span data-stu-id="96387-146">`/` or `/Index`</span></span> |
| <span data-ttu-id="96387-147">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="96387-147">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="96387-148">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="96387-148">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="96387-149">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="96387-149">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="96387-150">`/Store` или `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="96387-150">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="96387-151">Примечания.</span><span class="sxs-lookup"><span data-stu-id="96387-151">Notes:</span></span>

* <span data-ttu-id="96387-152">Среда выполнения по умолчанию ищет файлы Razor Pages в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="96387-152">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="96387-153">Если в URL-адресе не указана конкретная страница, по умолчанию открывается страница `Index`.</span><span class="sxs-lookup"><span data-stu-id="96387-153">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="96387-154">Создание простой формы</span><span class="sxs-lookup"><span data-stu-id="96387-154">Writing a basic form</span></span>

<span data-ttu-id="96387-155">Функция Razor Pages предназначена для упрощения типовых шаблонов, которые используются в браузерах.</span><span class="sxs-lookup"><span data-stu-id="96387-155">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="96387-156">[Привязки модели](xref:mvc/models/model-binding), [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и вспомогательные методы HTML *отлично работают* со свойствами, определенными в классе Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="96387-156">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="96387-157">Рассмотрим страницу с простой формой связи для модели `Contact`.</span><span class="sxs-lookup"><span data-stu-id="96387-157">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="96387-158">В представленных в этой статье примерах `DbContext` инициализируется в файле [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="96387-158">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="96387-159">Модель данных:</span><span class="sxs-lookup"><span data-stu-id="96387-159">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="96387-160">Контекст базы данных:</span><span class="sxs-lookup"><span data-stu-id="96387-160">The db context:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="96387-161">Файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="96387-161">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="96387-162">Модель страницы *Pages/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="96387-162">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="96387-163">Как правило, класс `PageModel` называется `<PageName>Model` и находится в том же пространстве имен, что и страница.</span><span class="sxs-lookup"><span data-stu-id="96387-163">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="96387-164">Класс `PageModel` позволяет разделять логику страницы и ее представление.</span><span class="sxs-lookup"><span data-stu-id="96387-164">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="96387-165">Он определяет обработчики страницы для запросов, отправляемых на страницу, а также данные для ее визуализации.</span><span class="sxs-lookup"><span data-stu-id="96387-165">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="96387-166">Такое разделение позволяет управлять зависимостями страницы путем их [внедрения](xref:fundamentals/dependency-injection) и выполнять [модульное тестирование](xref:testing/razor-pages-testing) страниц.</span><span class="sxs-lookup"><span data-stu-id="96387-166">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="96387-167">Страница содержит *метод обработчика* `OnPostAsync`, который выполняется по запросам `POST` (когда пользователь публикует форму).</span><span class="sxs-lookup"><span data-stu-id="96387-167">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="96387-168">Методы обработчика можно добавить для любой HTTP-команды.</span><span class="sxs-lookup"><span data-stu-id="96387-168">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="96387-169">Наиболее распространенные обработчики</span><span class="sxs-lookup"><span data-stu-id="96387-169">The most common handlers are:</span></span>

* <span data-ttu-id="96387-170">`OnGet` — инициализация необходимого для страницы состояния.</span><span class="sxs-lookup"><span data-stu-id="96387-170">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="96387-171">Пример обработчика [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="96387-171">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="96387-172">`OnPost` — обработка отправленных через форму данных.</span><span class="sxs-lookup"><span data-stu-id="96387-172">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="96387-173">Суффикс `Async` не является обязательным, но часто используется для асинхронных функций.</span><span class="sxs-lookup"><span data-stu-id="96387-173">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="96387-174">Код `OnPostAsync` в приведенном выше примере похож на то, что обычно пишут в контроллере.</span><span class="sxs-lookup"><span data-stu-id="96387-174">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="96387-175">Этот код типичен для Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="96387-175">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="96387-176">Большинство примитивов MVC, включая [привязку модели](xref:mvc/models/model-binding), [проверку](xref:mvc/models/validation) и результаты действий, являются общими.</span><span class="sxs-lookup"><span data-stu-id="96387-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="96387-177">Предыдущий метод `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="96387-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="96387-178">Простая схема `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="96387-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="96387-179">Проверка на наличие ошибок проверки.</span><span class="sxs-lookup"><span data-stu-id="96387-179">Check for validation errors.</span></span>

*  <span data-ttu-id="96387-180">Если ошибок нет, сохранение данных и перенаправление.</span><span class="sxs-lookup"><span data-stu-id="96387-180">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="96387-181">Если есть ошибки, отображение страницы с сообщениями проверки.</span><span class="sxs-lookup"><span data-stu-id="96387-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="96387-182">Проверка на стороне клиента выполняется точно так же, как в традиционных приложениях MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96387-182">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="96387-183">Во многих случаях ошибки проверки выявляются на клиентском компьютере и на сервер не отправляются.</span><span class="sxs-lookup"><span data-stu-id="96387-183">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="96387-184">После успешного ввода данных метод обработчика `OnPostAsync` вызывает метод обработчика `RedirectToPage`, чтобы получить экземпляр `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="96387-184">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="96387-185">`RedirectToPage` — это новый результат действия, аналогичный `RedirectToAction` или `RedirectToRoute`, но настроенный для страниц.</span><span class="sxs-lookup"><span data-stu-id="96387-185">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="96387-186">В приведенном выше примере он выполняет перенаправление на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="96387-186">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="96387-187">Более подробно `RedirectToPage` рассматривается в разделе [Создание URL для страниц](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="96387-187">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="96387-188">Если в отправленной форме имеются ошибки проверки (переданные на сервер), метод обработчика `OnPostAsync` вызывает метод обработчика `Page`.</span><span class="sxs-lookup"><span data-stu-id="96387-188">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="96387-189">`Page` возвращает экземпляр `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="96387-189">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="96387-190">Возвращение `Page` аналогично тому, как действия в контроллерах возвращают `View`.</span><span class="sxs-lookup"><span data-stu-id="96387-190">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="96387-191">`PageResult` — значение типа возвращаемого значения <!-- Review  --> по умолчанию для метода обработчика.</span><span class="sxs-lookup"><span data-stu-id="96387-191">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="96387-192">Метод обработчика, вернувший `void`, визуализирует страницу.</span><span class="sxs-lookup"><span data-stu-id="96387-192">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="96387-193">Для указания согласия на привязку модели в свойстве `Customer` используется атрибут `[BindProperty]`.</span><span class="sxs-lookup"><span data-stu-id="96387-193">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="96387-194">По умолчанию функция Razor Pages привязывает свойства ко всем командам, кроме GET.</span><span class="sxs-lookup"><span data-stu-id="96387-194">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="96387-195">Привязка к свойствам позволяет сократить объем необходимого кода.</span><span class="sxs-lookup"><span data-stu-id="96387-195">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="96387-196">Привязка уменьшает код за счет того, что для визуализации полей формы (`<input asp-for="Customer.Name" />`) и получения входных данных используется одно и то же свойство.</span><span class="sxs-lookup"><span data-stu-id="96387-196">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="96387-197">Домашняя страница (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="96387-197">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="96387-198">Файл кода программной части *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="96387-198">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="96387-199">Файл *Index.cshtml* содержит следующую разметку для создания ссылки на правку для каждого контактного лица.</span><span class="sxs-lookup"><span data-stu-id="96387-199">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="96387-200">[Вспомогательный тег привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) использует атрибут `asp-route-{value}` для создания ссылки на страницу редактирования.</span><span class="sxs-lookup"><span data-stu-id="96387-200">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="96387-201">Эта ссылка содержит данные о маршруте с идентификатором контактного лица.</span><span class="sxs-lookup"><span data-stu-id="96387-201">The link contains route data with the contact ID.</span></span> <span data-ttu-id="96387-202">Например, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="96387-202">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="96387-203">Файл *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="96387-203">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="96387-204">Первая строка содержит директиву `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="96387-204">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="96387-205">Ограничение маршрутизации `"{id:int}"` указывает, что страница должна принимать обращенные к ней запросы, которые содержат данные о маршруте `int`.</span><span class="sxs-lookup"><span data-stu-id="96387-205">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="96387-206">Если запрос к странице не содержит данные о маршруте, которые можно конвертировать в `int`, среда выполнения возвращает ошибку HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="96387-206">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="96387-207">Файл *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="96387-207">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="96387-208">Файл *Index.cshtml* также содержит разметку для создания кнопки удаления у каждого контакта клиента:</span><span class="sxs-lookup"><span data-stu-id="96387-208">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="96387-209">Во время обработки кнопки удаления в HTML ее `formaction` включает параметры для следующего:</span><span class="sxs-lookup"><span data-stu-id="96387-209">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="96387-210">Идентификатор контакта клиента, указанный атрибутом `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="96387-210">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="96387-211">Параметр `handler`, указанный атрибутом `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="96387-211">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="96387-212">Пример отображенной кнопки удаления с идентификатором контакта клиента `1`:</span><span class="sxs-lookup"><span data-stu-id="96387-212">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="96387-213">При выборе кнопки на сервер отправляется запрос формы `POST`.</span><span class="sxs-lookup"><span data-stu-id="96387-213">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="96387-214">По соглашению имя метода обработчика выбирается на основе значения параметра `handler` в соответствии со схемой `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="96387-214">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="96387-215">Так как `handler` — `delete` в этом примере, метод обработчика `OnPostDeleteAsync` используется для обработки запроса `POST`.</span><span class="sxs-lookup"><span data-stu-id="96387-215">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="96387-216">Если `asp-page-handler` имеет другое значение, например `remove`, выбирается метод обработчика страницы с именем `OnPostRemoveAsync`.</span><span class="sxs-lookup"><span data-stu-id="96387-216">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="96387-217">Метод `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="96387-217">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="96387-218">Принимает `id` из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="96387-218">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="96387-219">Отправляет в базу данных запрос контакта клиента с `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="96387-219">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="96387-220">Если контакт клиента найден, он удаляется из списка контактов.</span><span class="sxs-lookup"><span data-stu-id="96387-220">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="96387-221">База данных обновляется.</span><span class="sxs-lookup"><span data-stu-id="96387-221">The database is updated.</span></span>
* <span data-ttu-id="96387-222">Вызывает `RedirectToPage` для перенаправления на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="96387-222">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="96387-223">XSRF/CSRF и Razor Pages</span><span class="sxs-lookup"><span data-stu-id="96387-223">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="96387-224">Вам не придется писать отдельный код для [проверки подлинности запросов](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="96387-224">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="96387-225">Razor Pages включает создание и проверку маркеров защиты от подделок по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="96387-225">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="96387-226">Использование макетов, частичных реплик, шаблонов и вспомогательных функций тегов с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="96387-226">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="96387-227">Pages работает со всеми функциями представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="96387-227">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="96387-228">Макеты, частичные реплики, шаблоны, вспомогательные функции тегов, а также файлы *_ViewStart.cshtml* и *_ViewImports.cshtml* работают точно так же, как и в стандартных представлениях Razor.</span><span class="sxs-lookup"><span data-stu-id="96387-228">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="96387-229">Давайте упростим нашу страницу с помощью некоторых из этих функций.</span><span class="sxs-lookup"><span data-stu-id="96387-229">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="96387-230">Добавим [макет страницы](xref:mvc/views/layout) в файл *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="96387-230">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="96387-231">Этот [макет](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="96387-231">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="96387-232">управляет макетом каждой страницы (кроме страниц с отказом от макета);</span><span class="sxs-lookup"><span data-stu-id="96387-232">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="96387-233">импортирует HTML-структуры, такие как JavaScript и таблицы стилей.</span><span class="sxs-lookup"><span data-stu-id="96387-233">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="96387-234">Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="96387-234">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="96387-235">Свойство [Layout](xref:mvc/views/layout#specifying-a-layout) определяется в файле *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="96387-235">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="96387-236">**Примечание.** Макет хранится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="96387-236">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="96387-237">Pages ищет другие представления (макеты, шаблоны, частичные реплики) в иерархическом порядке, начиная с той папки, где находится текущая страница.</span><span class="sxs-lookup"><span data-stu-id="96387-237">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="96387-238">Макет в папке *Pages* можно использовать на любой странице Razor, которая находится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="96387-238">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="96387-239">Корпорация Майкрософт рекомендует **не** размещать файл макета в папке *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="96387-239">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="96387-240">*Views/Shared* — это шаблон представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="96387-240">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="96387-241">Razor Pages опирается на иерархию папок, а не на условные обозначения путей.</span><span class="sxs-lookup"><span data-stu-id="96387-241">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="96387-242">Поиск представлений в Razor Pages охватывает папку *Pages*.</span><span class="sxs-lookup"><span data-stu-id="96387-242">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="96387-243">Макеты, шаблоны и частичные реплики *работают* с контроллерами MVC и стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="96387-243">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="96387-244">Добавим файл *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="96387-244">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="96387-245">`@namespace` описывается далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="96387-245">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="96387-246">Директива `@addTagHelper` добавляет [встроенные вспомогательные теги](xref:mvc/views/tag-helpers/builtin-th/Index) на все страницы в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="96387-246">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="96387-247">Явное применение директивы `@namespace` на странице:</span><span class="sxs-lookup"><span data-stu-id="96387-247">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="96387-248">Данная директива задает пространство имен для страницы.</span><span class="sxs-lookup"><span data-stu-id="96387-248">The directive sets the namespace for the page.</span></span> <span data-ttu-id="96387-249">Включать пространство имен в директиву `@model` не требуется.</span><span class="sxs-lookup"><span data-stu-id="96387-249">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="96387-250">Если директива `@namespace` содержится в файле *_ViewImports.cshtml*, указанное пространство имен определяет префикс для созданного в Pages пространства имен, куда импортируется директива `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="96387-250">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="96387-251">Остальная часть созданного пространства имен (суффикс) представляет собой разделенный точками относительный путь между папкой с файлом *_ViewImports.cshtml* и папкой, содержащей страницу.</span><span class="sxs-lookup"><span data-stu-id="96387-251">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="96387-252">Например, файл кода программной части *Pages/Customers/Edit.cshtml.cs* задает пространство имен явно:</span><span class="sxs-lookup"><span data-stu-id="96387-252">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="96387-253">Файл *Pages/_ViewImports.cshtml* задает следующее пространство имен:</span><span class="sxs-lookup"><span data-stu-id="96387-253">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="96387-254">Сформированное пространство имен для файла *Pages/Customers/Edit.cshtml* Razor Pages совпадает с именем файла кода программной части.</span><span class="sxs-lookup"><span data-stu-id="96387-254">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="96387-255">Директива `@namespace` разработана таким образом, чтобы добавляемые в проект классы C# и сгенерированный страницами код *просто работали* без добавления директивы `@using` для файла кода программной части.</span><span class="sxs-lookup"><span data-stu-id="96387-255">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="96387-256">**Примечание.** `@namespace` также работает со стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="96387-256">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="96387-257">Исходный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="96387-257">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="96387-258">Обновленный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="96387-258">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="96387-259">[Начальный проект Razor Pages](#rpvs17) содержит файл *Pages/_ValidationScriptsPartial.cshtml*, который подключает проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="96387-259">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="96387-260">Формирование URL-адресов для страниц</span><span class="sxs-lookup"><span data-stu-id="96387-260">URL generation for Pages</span></span>

<span data-ttu-id="96387-261">На представленной выше странице `Create` используется `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="96387-261">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="96387-262">Это приложение имеет следующую структуру файлов и папок:</span><span class="sxs-lookup"><span data-stu-id="96387-262">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="96387-263">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="96387-263">*/Pages*</span></span>

  * <span data-ttu-id="96387-264">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="96387-264">*Index.cshtml*</span></span>
  * <span data-ttu-id="96387-265">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="96387-265">*/Customer*</span></span>

    * <span data-ttu-id="96387-266">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="96387-266">*Create.cshtml*</span></span>
    * <span data-ttu-id="96387-267">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="96387-267">*Edit.cshtml*</span></span>
    * <span data-ttu-id="96387-268">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="96387-268">*Index.cshtml*</span></span>

<span data-ttu-id="96387-269">После успешного выполнения страницы *Pages/Customers/Create.cshtml* и *Pages/Customers/Edit.cshtml* перенаправляются на страницу *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="96387-269">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="96387-270">Строка `/Index` составляет часть URL-адреса для доступа к указанной выше странице.</span><span class="sxs-lookup"><span data-stu-id="96387-270">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="96387-271">Строка `/Index` позволяет создавать URL-адреса для страницы *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="96387-271">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="96387-272">Пример:</span><span class="sxs-lookup"><span data-stu-id="96387-272">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="96387-273">Имя страницы — это путь к странице из корневой папки */Pages* (включая начальный символ `/`, например `/Index`).</span><span class="sxs-lookup"><span data-stu-id="96387-273">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="96387-274">Приведенные выше примеры создания URL-адресов задействуют гораздо больше функций, чем просто прописывание URL в коде.</span><span class="sxs-lookup"><span data-stu-id="96387-274">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="96387-275">Формирование URL-адресов включает [маршрутизацию](xref:mvc/controllers/routing) и позволяет генерировать и включать в код параметры в зависимости от того, как определяется маршрут в пути назначения.</span><span class="sxs-lookup"><span data-stu-id="96387-275">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="96387-276">Формирование URL-адресов для страниц поддерживает относительные имена.</span><span class="sxs-lookup"><span data-stu-id="96387-276">URL generation for pages supports relative names.</span></span> <span data-ttu-id="96387-277">В приведенной ниже таблице показано, какая страница индекса выбирается с каждым параметром `RedirectToPage` в *Pages/Customers/Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="96387-277">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="96387-278">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="96387-278">RedirectToPage(x)</span></span>| <span data-ttu-id="96387-279">Страница</span><span class="sxs-lookup"><span data-stu-id="96387-279">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="96387-280">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="96387-280">RedirectToPage("/Index")</span></span> | <span data-ttu-id="96387-281">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="96387-281">*Pages/Index*</span></span> |
| <span data-ttu-id="96387-282">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="96387-282">RedirectToPage("./Index");</span></span> | <span data-ttu-id="96387-283">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="96387-283">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="96387-284">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="96387-284">RedirectToPage("../Index")</span></span> | <span data-ttu-id="96387-285">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="96387-285">*Pages/Index*</span></span> |
| <span data-ttu-id="96387-286">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="96387-286">RedirectToPage("Index")</span></span>  | <span data-ttu-id="96387-287">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="96387-287">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="96387-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")` и `RedirectToPage("../Index")` — это *относительные имена*.</span><span class="sxs-lookup"><span data-stu-id="96387-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="96387-289">Для получения имени целевой страницы параметр `RedirectToPage` *комбинируется* с путем текущей страницы.</span><span class="sxs-lookup"><span data-stu-id="96387-289">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="96387-290">Привязка относительных имен полезна при создании сайтов со сложной структурой.</span><span class="sxs-lookup"><span data-stu-id="96387-290">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="96387-291">Если для связи между страницами в определенной папке используются относительные имена, эту папку можно переименовать.</span><span class="sxs-lookup"><span data-stu-id="96387-291">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="96387-292">При этом все ссылки останутся рабочими (так как не включают имя папки).</span><span class="sxs-lookup"><span data-stu-id="96387-292">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="96387-293">TempData</span><span class="sxs-lookup"><span data-stu-id="96387-293">TempData</span></span>

<span data-ttu-id="96387-294">ASP.NET Core позволяет использовать свойство [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) в [контроллере](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="96387-294">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="96387-295">Это свойство хранит данные до тех пор, пока они не будут прочитаны.</span><span class="sxs-lookup"><span data-stu-id="96387-295">This property stores data until it's read.</span></span> <span data-ttu-id="96387-296">Для проверки данных без удаления можно использовать методы `Keep` и `Peek`.</span><span class="sxs-lookup"><span data-stu-id="96387-296">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="96387-297">`TempData` удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса.</span><span class="sxs-lookup"><span data-stu-id="96387-297">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="96387-298">Атрибут `[TempData]` появился в ASP.NET 2.0 и поддерживается в контроллерах и на страницах.</span><span class="sxs-lookup"><span data-stu-id="96387-298">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="96387-299">В следующем коде значение `Message` задается с помощью `TempData`:</span><span class="sxs-lookup"><span data-stu-id="96387-299">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="96387-300">Представленная ниже разметка в файле *Pages/Customers/Index.cshtml* отображает значение `Message` с помощью `TempData`.</span><span class="sxs-lookup"><span data-stu-id="96387-300">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="96387-301">Модель страницы *Pages/Customers/Index.cshtml.cs* применяет атрибут `[TempData]` к свойству `Message`.</span><span class="sxs-lookup"><span data-stu-id="96387-301">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="96387-302">Дополнительные сведения см. в разделе [TempData](xref:fundamentals/app-state#temp).</span><span class="sxs-lookup"><span data-stu-id="96387-302">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="96387-303">Несколько обработчиков на страницу</span><span class="sxs-lookup"><span data-stu-id="96387-303">Multiple handlers per page</span></span>

<span data-ttu-id="96387-304">Следующая страница формирует разметку для двух обработчиков страницы с помощью вспомогательного тега `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="96387-304">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="96387-305">Форма в предыдущем примере включает две кнопки отправки, каждая из которых отправляет данные на отдельный URL-адрес с помощью `FormActionTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="96387-305">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="96387-306">Атрибут `asp-page-handler` является дополнением к `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="96387-306">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="96387-307">Атрибут `asp-page-handler` формирует URL-адреса, ,которые используются для отправки данных в каждый из методов обработчиков, определенных страницей.</span><span class="sxs-lookup"><span data-stu-id="96387-307">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="96387-308">`asp-page` не задается, так как пример сопоставлен с текущей страницей.</span><span class="sxs-lookup"><span data-stu-id="96387-308">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="96387-309">Модель страницы</span><span class="sxs-lookup"><span data-stu-id="96387-309">The page model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="96387-310">В представленном выше коде используются *именованные методы обработчика*.</span><span class="sxs-lookup"><span data-stu-id="96387-310">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="96387-311">Именованные методы обработчика создаются путем размещения определенного текста в имени после `On<HTTP Verb>` и перед `Async` (если есть).</span><span class="sxs-lookup"><span data-stu-id="96387-311">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="96387-312">В приведенном выше примере использовались методы страницы OnPost**JoinList**Async и OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="96387-312">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="96387-313">Если убрать *OnPost* и *Async*, имена обработчиков будут выглядеть как `JoinList` и `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="96387-313">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="96387-314">При использовании представленного выше кода URL-путь для отправки данных в `OnPostJoinListAsync` будет выглядеть как `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="96387-314">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="96387-315">URL-путь для отправки данных в `OnPostJoinListUCAsync` будет иметь вид `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="96387-315">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="96387-316">Настройка маршрутизации</span><span class="sxs-lookup"><span data-stu-id="96387-316">Customizing Routing</span></span>

<span data-ttu-id="96387-317">Если вы не хотите, чтобы в URL-адресе отображалась строка запроса `?handler=JoinList`, измените маршрут таким образом, чтобы в качестве пути в URL-адресе указывалось имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="96387-317">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="96387-318">Для настройки маршрута можно добавить после директивы `@page` шаблон маршрута, заключенные в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="96387-318">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="96387-319">В представленном выше маршруте вместо строки запроса URL-путь содержит имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="96387-319">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="96387-320">Символ `?` после `handler` означает, что параметр маршрута является необязательным.</span><span class="sxs-lookup"><span data-stu-id="96387-320">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="96387-321">С помощью директивы `@page` можно добавить к маршруту страницы дополнительные сегменты и параметры.</span><span class="sxs-lookup"><span data-stu-id="96387-321">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="96387-322">Все, что вы добавите, будет **присоединено** к маршруту страницы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="96387-322">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="96387-323">Использовать для изменения маршрута страницы абсолютный или виртуальный путь (например, вида `"~/Some/Other/Path"`) нельзя.</span><span class="sxs-lookup"><span data-stu-id="96387-323">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="96387-324">Конфигурация и параметры</span><span class="sxs-lookup"><span data-stu-id="96387-324">Configuration and settings</span></span>

<span data-ttu-id="96387-325">Чтобы настроить дополнительные параметры, воспользуйтесь методом расширения `AddRazorPagesOptions` в построителе MVC:</span><span class="sxs-lookup"><span data-stu-id="96387-325">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="96387-326">В настоящее время `RazorPagesOptions` позволяет задавать корневую папку для страниц и добавлять для них обозначения моделей приложений.</span><span class="sxs-lookup"><span data-stu-id="96387-326">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="96387-327">В дальнейшем мы расширим спектр его возможностей.</span><span class="sxs-lookup"><span data-stu-id="96387-327">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="96387-328">Сведения о предварительной компиляции представлений см. в статье [Компиляция представлений Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="96387-328">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="96387-329">[Загрузить или просмотреть пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="96387-329">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="96387-330">Представленные общие сведения являются вводными для статьи [Начало работы с Razor Pages в ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="96387-330">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="96387-331">Указание местонахождения Razor Pages в корне каталога</span><span class="sxs-lookup"><span data-stu-id="96387-331">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="96387-332">По умолчанию Razor Pages находится в корне каталога */Pages*.</span><span class="sxs-lookup"><span data-stu-id="96387-332">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="96387-333">Добавьте [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в корне содержимого Razor Pages ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) приложения:</span><span class="sxs-lookup"><span data-stu-id="96387-333">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="96387-334">Указание местонахождения Razor Pages в пользовательском корневом каталоге</span><span class="sxs-lookup"><span data-stu-id="96387-334">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="96387-335">Добавьте [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в пользовательском корневом каталоге в приложении (укажите относительный путь):</span><span class="sxs-lookup"><span data-stu-id="96387-335">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="96387-336">См. также</span><span class="sxs-lookup"><span data-stu-id="96387-336">See also</span></span>

* [<span data-ttu-id="96387-337">Начало работы с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="96387-337">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="96387-338">Соглашения об авторизации Razor Pages</span><span class="sxs-lookup"><span data-stu-id="96387-338">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="96387-339">Пользовательские поставщики моделей маршрутов и страниц Razor Pages</span><span class="sxs-lookup"><span data-stu-id="96387-339">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="96387-340">Модульное тестирование и тестирование интеграции для Razor Pages</span><span class="sxs-lookup"><span data-stu-id="96387-340">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
