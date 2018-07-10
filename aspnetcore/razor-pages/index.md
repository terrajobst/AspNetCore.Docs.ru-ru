---
title: Введение в Razor Pages в ASP.NET Core
author: Rick-Anderson
description: Сведения о применении в ASP.NET Core функции Razor Pages, которая делает создание кодов сценариев для страниц проще и эффективнее по сравнению с MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/12/2018
uid: razor-pages/index
ms.openlocfilehash: 49bed6cc150a74ff8b72848f276c55c2490b6fa5
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889146"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="8df02-103">Введение в Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8df02-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="8df02-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Райан Новак](https://github.com/rynowak) (Ryan Nowak)</span><span class="sxs-lookup"><span data-stu-id="8df02-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="8df02-105">Razor Pages — это новый аспект платформы MVC ASP.NET Core, который делает создание кодов сценариев для страниц проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="8df02-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="8df02-106">Если вам нужно руководство, использующее подход "модель-представление-контроллер", см. статью [Начало работы с MVC в ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="8df02-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="8df02-107">Этот документ содержит вводные сведения о Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8df02-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="8df02-108">Это не пошаговое руководство.</span><span class="sxs-lookup"><span data-stu-id="8df02-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="8df02-109">Если некоторые разделы покажутся вам слишком сложными, см. [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="8df02-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="8df02-110">Общие сведения об ASP.NET Core см. в разделе [Введение в ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="8df02-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8df02-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="8df02-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="8df02-112">Создание проекта Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8df02-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8df02-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8df02-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8df02-114">Подробные инструкции по созданию проекта Razor Pages с помощью Visual Studio см. в статье [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="8df02-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8df02-115">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="8df02-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8df02-116">Из командной строки выполните команду `dotnet new webapp`.</span><span class="sxs-lookup"><span data-stu-id="8df02-116">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8df02-117">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="8df02-117">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="8df02-118">Откройте созданный файл *.csproj* в Visual Studio для Mac.</span><span class="sxs-lookup"><span data-stu-id="8df02-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8df02-119">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8df02-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8df02-120">Из командной строки выполните команду `dotnet new webapp`.</span><span class="sxs-lookup"><span data-stu-id="8df02-120">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8df02-121">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="8df02-121">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8df02-122">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="8df02-122">.NET Core CLI</span></span>](#tab/netcore-cli)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8df02-123">Из командной строки выполните команду `dotnet new webapp`.</span><span class="sxs-lookup"><span data-stu-id="8df02-123">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8df02-124">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="8df02-124">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="8df02-125">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8df02-125">Razor Pages</span></span>

<span data-ttu-id="8df02-126">Функция Razor Pages активируется в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8df02-126">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="8df02-127">Рассмотрим простую страницу: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="8df02-127">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="8df02-128">Представленный выше код выглядит как файл представления Razor</span><span class="sxs-lookup"><span data-stu-id="8df02-128">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="8df02-129">и отличается от него только директивой `@page`.</span><span class="sxs-lookup"><span data-stu-id="8df02-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="8df02-130">Директива `@page` превращает файл в действие MVC, а значит обрабатывает запросы напрямую, минуя контроллер.</span><span class="sxs-lookup"><span data-stu-id="8df02-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="8df02-131">Директива `@page` должна быть первой директивой Razor на странице.</span><span class="sxs-lookup"><span data-stu-id="8df02-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="8df02-132">Директива `@page` влияет на поведение всех остальных конструкций Razor.</span><span class="sxs-lookup"><span data-stu-id="8df02-132">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="8df02-133">Похожая страница с использованием класса `PageModel` показана в следующих двух файлах.</span><span class="sxs-lookup"><span data-stu-id="8df02-133">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="8df02-134">Файл *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8df02-134">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="8df02-135">Модель страницы *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="8df02-135">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="8df02-136">Как правило, файл класса `PageModel` называется так же, как файл Razor Pages, но с расширением *.cs*.</span><span class="sxs-lookup"><span data-stu-id="8df02-136">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="8df02-137">Например, представленная выше страница Razor Pages называется *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8df02-137">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="8df02-138">Файл, содержащий класс `PageModel`, называется *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="8df02-138">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="8df02-139">Сопоставления URL-адресов со страницами определяются расположением конкретной страницы в файловой системе.</span><span class="sxs-lookup"><span data-stu-id="8df02-139">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="8df02-140">В приведенной ниже таблице показаны пути Razor Pages и соответствующие URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="8df02-140">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="8df02-141">Имя файла и путь</span><span class="sxs-lookup"><span data-stu-id="8df02-141">File name and path</span></span>               | <span data-ttu-id="8df02-142">Соответствующий URL</span><span class="sxs-lookup"><span data-stu-id="8df02-142">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="8df02-143">*/Pages/index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8df02-143">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="8df02-144">`/` или `/Index`</span><span class="sxs-lookup"><span data-stu-id="8df02-144">`/` or `/Index`</span></span> |
| <span data-ttu-id="8df02-145">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8df02-145">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="8df02-146">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8df02-146">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="8df02-147">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8df02-147">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="8df02-148">`/Store` или `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="8df02-148">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="8df02-149">Примечания.</span><span class="sxs-lookup"><span data-stu-id="8df02-149">Notes:</span></span>

* <span data-ttu-id="8df02-150">Среда выполнения по умолчанию ищет файлы Razor Pages в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="8df02-150">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="8df02-151">Если в URL-адресе не указана конкретная страница, по умолчанию открывается страница `Index`.</span><span class="sxs-lookup"><span data-stu-id="8df02-151">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="8df02-152">Создание простой формы</span><span class="sxs-lookup"><span data-stu-id="8df02-152">Writing a basic form</span></span>

<span data-ttu-id="8df02-153">Функция Razor Pages предназначена для упрощения реализации типовых шаблонов, которые используются в браузерах, при создании приложения.</span><span class="sxs-lookup"><span data-stu-id="8df02-153">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="8df02-154">[Привязки модели](xref:mvc/models/model-binding), [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и вспомогательные методы HTML *отлично работают* со свойствами, определенными в классе Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8df02-154">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="8df02-155">Рассмотрим страницу с простой формой связи для модели `Contact`.</span><span class="sxs-lookup"><span data-stu-id="8df02-155">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="8df02-156">В представленных в этой статье примерах `DbContext` инициализируется в файле [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="8df02-156">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="8df02-157">Модель данных:</span><span class="sxs-lookup"><span data-stu-id="8df02-157">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="8df02-158">Контекст базы данных:</span><span class="sxs-lookup"><span data-stu-id="8df02-158">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="8df02-159">Файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8df02-159">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="8df02-160">Модель страницы *Pages/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="8df02-160">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="8df02-161">Как правило, класс `PageModel` называется `<PageName>Model` и находится в том же пространстве имен, что и страница.</span><span class="sxs-lookup"><span data-stu-id="8df02-161">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="8df02-162">Класс `PageModel` позволяет разделять логику страницы и ее представление.</span><span class="sxs-lookup"><span data-stu-id="8df02-162">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="8df02-163">Он определяет обработчики страницы для запросов, отправляемых на страницу, а также данные для ее визуализации.</span><span class="sxs-lookup"><span data-stu-id="8df02-163">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="8df02-164">Такое разделение позволяет управлять зависимостями страницы путем их [внедрения](xref:fundamentals/dependency-injection) и выполнять [модульное тестирование](xref:test/razor-pages-tests) страниц.</span><span class="sxs-lookup"><span data-stu-id="8df02-164">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="8df02-165">Страница содержит *метод обработчика* `OnPostAsync`, который выполняется по запросам `POST` (когда пользователь публикует форму).</span><span class="sxs-lookup"><span data-stu-id="8df02-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="8df02-166">Методы обработчика можно добавить для любой HTTP-команды.</span><span class="sxs-lookup"><span data-stu-id="8df02-166">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="8df02-167">Наиболее распространенные обработчики</span><span class="sxs-lookup"><span data-stu-id="8df02-167">The most common handlers are:</span></span>

* <span data-ttu-id="8df02-168">`OnGet` — инициализация необходимого для страницы состояния.</span><span class="sxs-lookup"><span data-stu-id="8df02-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="8df02-169">Пример обработчика [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="8df02-169">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="8df02-170">`OnPost` — обработка отправленных через форму данных.</span><span class="sxs-lookup"><span data-stu-id="8df02-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="8df02-171">Суффикс `Async` не является обязательным, но часто используется для асинхронных функций.</span><span class="sxs-lookup"><span data-stu-id="8df02-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="8df02-172">Код `OnPostAsync` в приведенном выше примере похож на то, что обычно пишут в контроллере.</span><span class="sxs-lookup"><span data-stu-id="8df02-172">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="8df02-173">Этот код типичен для Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8df02-173">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="8df02-174">Большинство примитивов MVC, включая [привязку модели](xref:mvc/models/model-binding), [проверку](xref:mvc/models/validation) и результаты действий, являются общими.</span><span class="sxs-lookup"><span data-stu-id="8df02-174">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="8df02-175">Предыдущий метод `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="8df02-175">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="8df02-176">Простая схема `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="8df02-176">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="8df02-177">Проверка на наличие ошибок проверки.</span><span class="sxs-lookup"><span data-stu-id="8df02-177">Check for validation errors.</span></span>

*  <span data-ttu-id="8df02-178">Если ошибок нет, сохранение данных и перенаправление.</span><span class="sxs-lookup"><span data-stu-id="8df02-178">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="8df02-179">Если есть ошибки, отображение страницы с сообщениями проверки.</span><span class="sxs-lookup"><span data-stu-id="8df02-179">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="8df02-180">Проверка на стороне клиента выполняется точно так же, как в традиционных приложениях MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8df02-180">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="8df02-181">Во многих случаях ошибки проверки выявляются на клиентском компьютере и на сервер не отправляются.</span><span class="sxs-lookup"><span data-stu-id="8df02-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="8df02-182">После успешного ввода данных метод обработчика `OnPostAsync` вызывает метод обработчика `RedirectToPage`, чтобы получить экземпляр `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="8df02-182">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="8df02-183">`RedirectToPage` — это новый результат действия, аналогичный `RedirectToAction` или `RedirectToRoute`, но настроенный для страниц.</span><span class="sxs-lookup"><span data-stu-id="8df02-183">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="8df02-184">В приведенном выше примере он выполняет перенаправление на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="8df02-184">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="8df02-185">Более подробно `RedirectToPage` рассматривается в разделе [Создание URL для страниц](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="8df02-185">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="8df02-186">Если в отправленной форме имеются ошибки проверки (переданные на сервер), метод обработчика `OnPostAsync` вызывает метод обработчика `Page`.</span><span class="sxs-lookup"><span data-stu-id="8df02-186">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="8df02-187">`Page` возвращает экземпляр `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="8df02-187">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="8df02-188">Возвращение `Page` аналогично тому, как действия в контроллерах возвращают `View`.</span><span class="sxs-lookup"><span data-stu-id="8df02-188">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="8df02-189">`PageResult` — значение типа возвращаемого значения <!-- Review  --> по умолчанию для метода обработчика.</span><span class="sxs-lookup"><span data-stu-id="8df02-189">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="8df02-190">Метод обработчика, вернувший `void`, визуализирует страницу.</span><span class="sxs-lookup"><span data-stu-id="8df02-190">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="8df02-191">Для указания согласия на привязку модели в свойстве `Customer` используется атрибут `[BindProperty]`.</span><span class="sxs-lookup"><span data-stu-id="8df02-191">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="8df02-192">По умолчанию функция Razor Pages привязывает свойства ко всем командам, кроме GET.</span><span class="sxs-lookup"><span data-stu-id="8df02-192">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="8df02-193">Привязка к свойствам позволяет сократить объем необходимого кода.</span><span class="sxs-lookup"><span data-stu-id="8df02-193">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="8df02-194">Привязка уменьшает код за счет того, что для визуализации полей формы (`<input asp-for="Customer.Name" />`) и получения входных данных используется одно и то же свойство.</span><span class="sxs-lookup"><span data-stu-id="8df02-194">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="8df02-195">В целях обеспечения безопасности следует привязать данные запросов GET к свойствам модели страниц.</span><span class="sxs-lookup"><span data-stu-id="8df02-195">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="8df02-196">Проверьте введенные данные пользователя, прежде чем сопоставлять их со свойствами.</span><span class="sxs-lookup"><span data-stu-id="8df02-196">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="8df02-197">Это поведение полезно в случаях, когда используются строки запросов и значения маршрутов.</span><span class="sxs-lookup"><span data-stu-id="8df02-197">Opting in to this behavior is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="8df02-198">Чтобы привязать свойство к запросам GET, задайте для свойства `SupportsGet` атрибута `[BindProperty]` значение `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="8df02-198">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="8df02-199">Домашняя страница (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="8df02-199">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="8df02-200">Связанный класс `PageModel` (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="8df02-200">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="8df02-201">Файл *Index.cshtml* содержит следующую разметку для создания ссылки на правку для каждого контактного лица.</span><span class="sxs-lookup"><span data-stu-id="8df02-201">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="8df02-202">[Вспомогательный тег привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) использует атрибут `asp-route-{value}` для создания ссылки на страницу редактирования.</span><span class="sxs-lookup"><span data-stu-id="8df02-202">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="8df02-203">Эта ссылка содержит данные о маршруте с идентификатором контактного лица.</span><span class="sxs-lookup"><span data-stu-id="8df02-203">The link contains route data with the contact ID.</span></span> <span data-ttu-id="8df02-204">Например, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="8df02-204">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="8df02-205">Файл *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8df02-205">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="8df02-206">Первая строка содержит директиву `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="8df02-206">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="8df02-207">Ограничение маршрутизации `"{id:int}"` указывает, что страница должна принимать обращенные к ней запросы, которые содержат данные о маршруте `int`.</span><span class="sxs-lookup"><span data-stu-id="8df02-207">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="8df02-208">Если запрос к странице не содержит данные о маршруте, которые можно конвертировать в `int`, среда выполнения возвращает ошибку HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="8df02-208">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="8df02-209">Чтобы сделать идентификатор необязательным, добавьте `?` к ограничению маршрута:</span><span class="sxs-lookup"><span data-stu-id="8df02-209">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="8df02-210">Файл *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="8df02-210">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="8df02-211">Файл *Index.cshtml* также содержит разметку для создания кнопки удаления у каждого контакта клиента:</span><span class="sxs-lookup"><span data-stu-id="8df02-211">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="8df02-212">Во время обработки кнопки удаления в HTML ее `formaction` включает параметры для следующего:</span><span class="sxs-lookup"><span data-stu-id="8df02-212">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="8df02-213">Идентификатор контакта клиента, указанный атрибутом `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="8df02-213">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="8df02-214">Параметр `handler`, указанный атрибутом `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="8df02-214">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="8df02-215">Пример отображенной кнопки удаления с идентификатором контакта клиента `1`:</span><span class="sxs-lookup"><span data-stu-id="8df02-215">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="8df02-216">При выборе кнопки на сервер отправляется запрос формы `POST`.</span><span class="sxs-lookup"><span data-stu-id="8df02-216">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="8df02-217">По соглашению имя метода обработчика выбирается на основе значения параметра `handler` в соответствии со схемой `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="8df02-217">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="8df02-218">Так как `handler` — `delete` в этом примере, метод обработчика `OnPostDeleteAsync` используется для обработки запроса `POST`.</span><span class="sxs-lookup"><span data-stu-id="8df02-218">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="8df02-219">Если `asp-page-handler` имеет другое значение, например `remove`, выбирается метод обработчика страницы с именем `OnPostRemoveAsync`.</span><span class="sxs-lookup"><span data-stu-id="8df02-219">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="8df02-220">Метод `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="8df02-220">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="8df02-221">Принимает `id` из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="8df02-221">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="8df02-222">Отправляет в базу данных запрос контакта клиента с `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="8df02-222">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="8df02-223">Если контакт клиента найден, он удаляется из списка контактов.</span><span class="sxs-lookup"><span data-stu-id="8df02-223">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="8df02-224">База данных обновляется.</span><span class="sxs-lookup"><span data-stu-id="8df02-224">The database is updated.</span></span>
* <span data-ttu-id="8df02-225">Вызывает `RedirectToPage` для перенаправления на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="8df02-225">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-required"></a><span data-ttu-id="8df02-226">Маркировка свойств страницы как обязательных</span><span class="sxs-lookup"><span data-stu-id="8df02-226">Mark page properties required</span></span>

<span data-ttu-id="8df02-227">Свойства класса `PageModel` можно отметить атрибутом [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute):</span><span class="sxs-lookup"><span data-stu-id="8df02-227">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="8df02-228">Дополнительные сведения см. в статье о [проверке модели](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="8df02-228">See [Model validation](xref:mvc/models/validation) for more information.</span></span>

## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="8df02-229">Управление запросами HEAD с помощью обработчика OnGet</span><span class="sxs-lookup"><span data-stu-id="8df02-229">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="8df02-230">Обработчик HEAD обычно создается и вызывается для выполнения запросов HEAD:</span><span class="sxs-lookup"><span data-stu-id="8df02-230">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span>

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="8df02-231">Если обработчик HEAD (`OnHead`) не определен, Razor Pages выполнит вызов обработчика страниц GET (`OnGet`) в ASP.NET Core 2.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="8df02-231">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="8df02-232">Вы можете явно выбрать такое поведение с помощью [метода SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) в `Startup.Configure` ASP.NET Core 2.1–2.x:</span><span class="sxs-lookup"><span data-stu-id="8df02-232">Opt in to this behavior with the [SetCompatibilityVersion method](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) in `Startup.Configure` for ASP.NET Core 2.1 to 2.x:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="8df02-233">`SetCompatibilityVersion` задает для параметра `AllowMappingHeadRequestsToGetHandler` Razor Pages значение `true`.</span><span class="sxs-lookup"><span data-stu-id="8df02-233">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="8df02-234">Вместо включения всех возможных поведений для версии 2.1 с помощью метода `SetCompatibilityVersion` вы можете задать конкретное поведение.</span><span class="sxs-lookup"><span data-stu-id="8df02-234">Rather than opting into all 2.1 behaviors with `SetCompatibilityVersion`, you can explicitly opt-in to specific behaviors.</span></span> <span data-ttu-id="8df02-235">Следующий код назначает запросам HEAD обработчик GET.</span><span class="sxs-lookup"><span data-stu-id="8df02-235">The following code opts into the mapping HEAD requests to the GET handler.</span></span>


```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```
::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="8df02-236">XSRF/CSRF и Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8df02-236">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="8df02-237">Вам не придется писать отдельный код для [проверки подлинности запросов](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="8df02-237">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="8df02-238">Razor Pages включает создание и проверку маркеров защиты от подделок по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8df02-238">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="8df02-239">Использование макетов, частичных реплик, шаблонов и вспомогательных функций тегов с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8df02-239">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="8df02-240">Pages работает со всеми функциями подсистемы просмотра Razor.</span><span class="sxs-lookup"><span data-stu-id="8df02-240">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="8df02-241">Макеты, частичные реплики, шаблоны, вспомогательные функции тегов, а также файлы *_ViewStart.cshtml* и *_ViewImports.cshtml* работают точно так же, как и в стандартных представлениях Razor.</span><span class="sxs-lookup"><span data-stu-id="8df02-241">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="8df02-242">Давайте упростим нашу страницу с помощью некоторых из этих функций.</span><span class="sxs-lookup"><span data-stu-id="8df02-242">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8df02-243">Добавим [макет страницы](xref:mvc/views/layout) в файл *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8df02-243">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8df02-244">Добавим [макет страницы](xref:mvc/views/layout) в файл *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8df02-244">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="8df02-245">Этот [макет](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="8df02-245">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="8df02-246">управляет макетом каждой страницы (кроме страниц с отказом от макета);</span><span class="sxs-lookup"><span data-stu-id="8df02-246">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="8df02-247">импортирует HTML-структуры, такие как JavaScript и таблицы стилей.</span><span class="sxs-lookup"><span data-stu-id="8df02-247">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="8df02-248">Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="8df02-248">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="8df02-249">Свойство [Layout](xref:mvc/views/layout#specifying-a-layout) определяется в файле *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8df02-249">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8df02-250">Макет хранится в папке *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="8df02-250">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="8df02-251">Pages ищет другие представления (макеты, шаблоны, частичные реплики) в иерархическом порядке, начиная с той папки, где находится текущая страница.</span><span class="sxs-lookup"><span data-stu-id="8df02-251">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="8df02-252">Макет в папке *Pages/Shared* можно использовать на любой странице Razor, которая находится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="8df02-252">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="8df02-253">Файл макета следует поместить в папку *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="8df02-253">The layout file should go in the *Pages/Shared* folder.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8df02-254">Макет хранится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="8df02-254">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="8df02-255">Pages ищет другие представления (макеты, шаблоны, частичные реплики) в иерархическом порядке, начиная с той папки, где находится текущая страница.</span><span class="sxs-lookup"><span data-stu-id="8df02-255">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="8df02-256">Макет в папке *Pages* можно использовать на любой странице Razor, которая находится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="8df02-256">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

::: moniker-end

<span data-ttu-id="8df02-257">Корпорация Майкрософт рекомендует **не** размещать файл макета в папке *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="8df02-257">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="8df02-258">*Views/Shared* — это шаблон представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="8df02-258">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="8df02-259">Razor Pages опирается на иерархию папок, а не на условные обозначения путей.</span><span class="sxs-lookup"><span data-stu-id="8df02-259">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="8df02-260">Поиск представлений в Razor Pages охватывает папку *Pages*.</span><span class="sxs-lookup"><span data-stu-id="8df02-260">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="8df02-261">Макеты, шаблоны и частичные реплики *работают* с контроллерами MVC и стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="8df02-261">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="8df02-262">Добавим файл *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8df02-262">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="8df02-263">`@namespace` описывается далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="8df02-263">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="8df02-264">Директива `@addTagHelper` добавляет [встроенные вспомогательные теги](xref:mvc/views/tag-helpers/builtin-th/Index) на все страницы в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="8df02-264">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="8df02-265">Явное применение директивы `@namespace` на странице:</span><span class="sxs-lookup"><span data-stu-id="8df02-265">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="8df02-266">Данная директива задает пространство имен для страницы.</span><span class="sxs-lookup"><span data-stu-id="8df02-266">The directive sets the namespace for the page.</span></span> <span data-ttu-id="8df02-267">Включать пространство имен в директиву `@model` не требуется.</span><span class="sxs-lookup"><span data-stu-id="8df02-267">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="8df02-268">Если директива `@namespace` содержится в файле *_ViewImports.cshtml*, указанное пространство имен определяет префикс для созданного в Pages пространства имен, куда импортируется директива `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="8df02-268">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="8df02-269">Остальная часть созданного пространства имен (суффикс) представляет собой разделенный точками относительный путь между папкой с файлом *_ViewImports.cshtml* и папкой, содержащей страницу.</span><span class="sxs-lookup"><span data-stu-id="8df02-269">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="8df02-270">Например, класс `PageModel` в файле *Pages/Customers/Edit.cshtml.cs* задает пространство имен явно.</span><span class="sxs-lookup"><span data-stu-id="8df02-270">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="8df02-271">Файл *Pages/_ViewImports.cshtml* задает следующее пространство имен:</span><span class="sxs-lookup"><span data-stu-id="8df02-271">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="8df02-272">Сформированное пространство имен для файла *Pages/Customers/Edit.cshtml* Razor Pages совпадает с пространством имен класса `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="8df02-272">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="8df02-273">`@namespace` *также работает со стандартными представлениями Razor.*</span><span class="sxs-lookup"><span data-stu-id="8df02-273">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="8df02-274">Исходный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8df02-274">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="8df02-275">Обновленный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8df02-275">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="8df02-276">[Начальный проект Razor Pages](#rpvs17) содержит файл *Pages/_ValidationScriptsPartial.cshtml*, который подключает проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="8df02-276">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="8df02-277">Формирование URL-адресов для страниц</span><span class="sxs-lookup"><span data-stu-id="8df02-277">URL generation for Pages</span></span>

<span data-ttu-id="8df02-278">На представленной выше странице `Create` используется `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="8df02-278">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="8df02-279">Это приложение имеет следующую структуру файлов и папок:</span><span class="sxs-lookup"><span data-stu-id="8df02-279">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="8df02-280">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="8df02-280">*/Pages*</span></span>

  * <span data-ttu-id="8df02-281">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8df02-281">*Index.cshtml*</span></span>
  * <span data-ttu-id="8df02-282">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="8df02-282">*/Customers*</span></span>

    * <span data-ttu-id="8df02-283">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8df02-283">*Create.cshtml*</span></span>
    * <span data-ttu-id="8df02-284">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8df02-284">*Edit.cshtml*</span></span>
    * <span data-ttu-id="8df02-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8df02-285">*Index.cshtml*</span></span>

<span data-ttu-id="8df02-286">После успешного выполнения страницы *Pages/Customers/Create.cshtml* и *Pages/Customers/Edit.cshtml* перенаправляются на страницу *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8df02-286">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="8df02-287">Строка `/Index` составляет часть URL-адреса для доступа к указанной выше странице.</span><span class="sxs-lookup"><span data-stu-id="8df02-287">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="8df02-288">Строка `/Index` позволяет создавать URL-адреса для страницы *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8df02-288">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="8df02-289">Пример:</span><span class="sxs-lookup"><span data-stu-id="8df02-289">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="8df02-290">Имя страницы — это путь к странице из корневой папки */Pages*, включая начальный символ `/` (например, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="8df02-290">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="8df02-291">Предыдущие образцы создания URL-адреса обеспечивают расширенные параметры и функциональные возможности по сравнению с жестким заданием URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="8df02-291">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="8df02-292">Формирование URL-адресов включает [маршрутизацию](xref:mvc/controllers/routing) и позволяет генерировать и включать в код параметры в зависимости от того, как определяется маршрут в пути назначения.</span><span class="sxs-lookup"><span data-stu-id="8df02-292">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="8df02-293">Формирование URL-адресов для страниц поддерживает относительные имена.</span><span class="sxs-lookup"><span data-stu-id="8df02-293">URL generation for pages supports relative names.</span></span> <span data-ttu-id="8df02-294">В приведенной ниже таблице показано, какая страница индекса выбирается с каждым параметром `RedirectToPage` в *Pages/Customers/Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8df02-294">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="8df02-295">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="8df02-295">RedirectToPage(x)</span></span>| <span data-ttu-id="8df02-296">Страница</span><span class="sxs-lookup"><span data-stu-id="8df02-296">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="8df02-297">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="8df02-297">RedirectToPage("/Index")</span></span> | <span data-ttu-id="8df02-298">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="8df02-298">*Pages/Index*</span></span> |
| <span data-ttu-id="8df02-299">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="8df02-299">RedirectToPage("./Index");</span></span> | <span data-ttu-id="8df02-300">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="8df02-300">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="8df02-301">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="8df02-301">RedirectToPage("../Index")</span></span> | <span data-ttu-id="8df02-302">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="8df02-302">*Pages/Index*</span></span> |
| <span data-ttu-id="8df02-303">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="8df02-303">RedirectToPage("Index")</span></span>  | <span data-ttu-id="8df02-304">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="8df02-304">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="8df02-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")` и `RedirectToPage("../Index")` — это <em>относительные имена</em>.</span><span class="sxs-lookup"><span data-stu-id="8df02-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="8df02-306">Для получения имени целевой страницы параметр `RedirectToPage` <em>комбинируется</em> с путем текущей страницы.</span><span class="sxs-lookup"><span data-stu-id="8df02-306">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="8df02-307">Привязка относительных имен полезна при создании сайтов со сложной структурой.</span><span class="sxs-lookup"><span data-stu-id="8df02-307">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="8df02-308">Если для связи между страницами в определенной папке используются относительные имена, эту папку можно переименовать.</span><span class="sxs-lookup"><span data-stu-id="8df02-308">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="8df02-309">При этом все ссылки останутся рабочими (так как не включают имя папки).</span><span class="sxs-lookup"><span data-stu-id="8df02-309">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="viewdata-attribute"></a><span data-ttu-id="8df02-310">Атрибут ViewData</span><span class="sxs-lookup"><span data-stu-id="8df02-310">ViewData attribute</span></span>

<span data-ttu-id="8df02-311">Данные могут передаваться на страницу с помощью атрибута [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="8df02-311">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="8df02-312">Свойства на контроллерах или моделях страниц Razor, отмеченные атрибутом `[ViewData]`, обладают своими собственными значениями, загружаемыми из [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="8df02-312">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="8df02-313">В следующем примере класс `AboutModel` содержит свойство `Title`, отмеченное атрибутом `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="8df02-313">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="8df02-314">Свойство `Title` задает заголовок страницы About.</span><span class="sxs-lookup"><span data-stu-id="8df02-314">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="8df02-315">На странице About доступ к свойству `Title` осуществляется как доступ к свойству модели.</span><span class="sxs-lookup"><span data-stu-id="8df02-315">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="8df02-316">В макете заголовок считывается из словаря ViewData.</span><span class="sxs-lookup"><span data-stu-id="8df02-316">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```
::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="8df02-317">TempData</span><span class="sxs-lookup"><span data-stu-id="8df02-317">TempData</span></span>

<span data-ttu-id="8df02-318">ASP.NET Core позволяет использовать свойство [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) в [контроллере](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="8df02-318">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="8df02-319">Это свойство хранит данные до тех пор, пока они не будут прочитаны.</span><span class="sxs-lookup"><span data-stu-id="8df02-319">This property stores data until it's read.</span></span> <span data-ttu-id="8df02-320">Для проверки данных без удаления можно использовать методы `Keep` и `Peek`.</span><span class="sxs-lookup"><span data-stu-id="8df02-320">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="8df02-321">`TempData` удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса.</span><span class="sxs-lookup"><span data-stu-id="8df02-321">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="8df02-322">Атрибут `[TempData]` появился в ASP.NET 2.0 и поддерживается в контроллерах и на страницах.</span><span class="sxs-lookup"><span data-stu-id="8df02-322">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="8df02-323">В следующем коде значение `Message` задается с помощью `TempData`:</span><span class="sxs-lookup"><span data-stu-id="8df02-323">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="8df02-324">Представленная ниже разметка в файле *Pages/Customers/Index.cshtml* отображает значение `Message` с помощью `TempData`.</span><span class="sxs-lookup"><span data-stu-id="8df02-324">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="8df02-325">Модель страницы *Pages/Customers/Index.cshtml.cs* применяет атрибут `[TempData]` к свойству `Message`.</span><span class="sxs-lookup"><span data-stu-id="8df02-325">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="8df02-326">Дополнительные сведения см. в разделе [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="8df02-326">See [TempData](xref:fundamentals/app-state#tempdata) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="8df02-327">Несколько обработчиков на страницу</span><span class="sxs-lookup"><span data-stu-id="8df02-327">Multiple handlers per page</span></span>

<span data-ttu-id="8df02-328">Следующая страница формирует разметку для двух обработчиков страницы с помощью вспомогательного тега `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="8df02-328">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="8df02-329">Форма в предыдущем примере включает две кнопки отправки, каждая из которых отправляет данные на отдельный URL-адрес с помощью `FormActionTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="8df02-329">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="8df02-330">Атрибут `asp-page-handler` является дополнением к `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="8df02-330">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="8df02-331">Атрибут `asp-page-handler` формирует URL-адреса, ,которые используются для отправки данных в каждый из методов обработчиков, определенных страницей.</span><span class="sxs-lookup"><span data-stu-id="8df02-331">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="8df02-332">`asp-page` не задается, так как пример сопоставлен с текущей страницей.</span><span class="sxs-lookup"><span data-stu-id="8df02-332">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="8df02-333">Модель страницы</span><span class="sxs-lookup"><span data-stu-id="8df02-333">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="8df02-334">В представленном выше коде используются *именованные методы обработчика*.</span><span class="sxs-lookup"><span data-stu-id="8df02-334">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="8df02-335">Именованные методы обработчика создаются путем размещения определенного текста в имени после `On<HTTP Verb>` и перед `Async` (если есть).</span><span class="sxs-lookup"><span data-stu-id="8df02-335">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="8df02-336">В приведенном выше примере использовались методы страницы OnPost**JoinList**Async и OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="8df02-336">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="8df02-337">Если убрать *OnPost* и *Async*, имена обработчиков будут выглядеть как `JoinList` и `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="8df02-337">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="8df02-338">При использовании представленного выше кода URL-путь для отправки данных в `OnPostJoinListAsync` будет выглядеть как `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="8df02-338">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="8df02-339">URL-путь для отправки данных в `OnPostJoinListUCAsync` будет иметь вид `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="8df02-339">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="8df02-340">Пользовательские маршруты</span><span class="sxs-lookup"><span data-stu-id="8df02-340">Custom routes</span></span>

<span data-ttu-id="8df02-341">С помощью директивы `@page` можно сделать следующее.</span><span class="sxs-lookup"><span data-stu-id="8df02-341">Use the `@page` directive to:</span></span>

* <span data-ttu-id="8df02-342">Указать пользовательский маршрут к странице.</span><span class="sxs-lookup"><span data-stu-id="8df02-342">Specify a custom route to a page.</span></span> <span data-ttu-id="8df02-343">Например, можно задать маршрут к странице "Сведения" `/Some/Other/Path`: `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="8df02-343">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="8df02-344">Добавить сегменты к маршруту страницы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8df02-344">Append segments to a page's default route.</span></span> <span data-ttu-id="8df02-345">Например, к такому маршруту можно добавить сегмент item: `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="8df02-345">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="8df02-346">Добавить параметры к маршруту страницы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8df02-346">Append parameters to a page's default route.</span></span> <span data-ttu-id="8df02-347">Например, для страницы с `@page "{id}"` может потребоваться параметр идентификатора `id`.</span><span class="sxs-lookup"><span data-stu-id="8df02-347">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="8df02-348">Поддерживается путь относительно корня, заданный знаком тильды (`~`) в начале пути.</span><span class="sxs-lookup"><span data-stu-id="8df02-348">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="8df02-349">Например, `@page "~/Some/Other/Path"` равносильно `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="8df02-349">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="8df02-350">Вы можете изменить строку запроса `?handler=JoinList` в URL-адресе на сегмент маршрута `/JoinList`, указав шаблон маршрута `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="8df02-350">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="8df02-351">Если вы не хотите, чтобы в URL-адресе отображалась строка запроса `?handler=JoinList`, измените маршрут таким образом, чтобы в качестве пути в URL-адресе указывалось имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="8df02-351">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="8df02-352">Для настройки маршрута можно добавить после директивы `@page` шаблон маршрута, заключенные в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="8df02-352">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="8df02-353">При использовании представленного выше кода URL-путь для отправки данных в `OnPostJoinListAsync` будет выглядеть как `http://localhost:5000/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="8df02-353">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="8df02-354">URL-путь для отправки данных в `OnPostJoinListUCAsync` будет иметь вид `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="8df02-354">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="8df02-355">Символ `?` после `handler` означает, что параметр маршрута является необязательным.</span><span class="sxs-lookup"><span data-stu-id="8df02-355">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="8df02-356">Конфигурация и параметры</span><span class="sxs-lookup"><span data-stu-id="8df02-356">Configuration and settings</span></span>

<span data-ttu-id="8df02-357">Чтобы настроить дополнительные параметры, воспользуйтесь методом расширения `AddRazorPagesOptions` в построителе MVC:</span><span class="sxs-lookup"><span data-stu-id="8df02-357">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="8df02-358">В настоящее время `RazorPagesOptions` позволяет задавать корневую папку для страниц и добавлять для них обозначения моделей приложений.</span><span class="sxs-lookup"><span data-stu-id="8df02-358">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="8df02-359">В дальнейшем мы расширим спектр его возможностей.</span><span class="sxs-lookup"><span data-stu-id="8df02-359">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="8df02-360">Сведения о предварительной компиляции представлений см. в статье [Компиляция представлений Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="8df02-360">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="8df02-361">[Загрузить или просмотреть пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="8df02-361">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="8df02-362">См. раздел [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start), который дает дополнительную информацию к введению.</span><span class="sxs-lookup"><span data-stu-id="8df02-362">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="8df02-363">Указание местонахождения Razor Pages в корне каталога</span><span class="sxs-lookup"><span data-stu-id="8df02-363">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="8df02-364">По умолчанию Razor Pages находится в корне каталога */Pages*.</span><span class="sxs-lookup"><span data-stu-id="8df02-364">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="8df02-365">Добавьте [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в корне содержимого Razor Pages ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) приложения:</span><span class="sxs-lookup"><span data-stu-id="8df02-365">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="8df02-366">Указание местонахождения Razor Pages в пользовательском корневом каталоге</span><span class="sxs-lookup"><span data-stu-id="8df02-366">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="8df02-367">Добавьте [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в пользовательском корневом каталоге в приложении (укажите относительный путь):</span><span class="sxs-lookup"><span data-stu-id="8df02-367">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="8df02-368">См. также</span><span class="sxs-lookup"><span data-stu-id="8df02-368">See also</span></span>

* [<span data-ttu-id="8df02-369">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8df02-369">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="8df02-370">Синтаксис Razor</span><span class="sxs-lookup"><span data-stu-id="8df02-370">Razor syntax</span></span>](xref:mvc/views/razor)
* [<span data-ttu-id="8df02-371">Начало работы с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8df02-371">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="8df02-372">Соглашения об авторизации Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8df02-372">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="8df02-373">Пользовательские поставщики моделей маршрутов и страниц Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8df02-373">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* [<span data-ttu-id="8df02-374">Модульные тесты страниц Razor</span><span class="sxs-lookup"><span data-stu-id="8df02-374">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)
