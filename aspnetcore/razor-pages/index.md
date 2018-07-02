---
title: Введение в Razor Pages в ASP.NET Core
author: Rick-Anderson
description: Сведения о применении в ASP.NET Core функции Razor Pages, которая делает создание кодов сценариев для страниц проще и эффективнее по сравнению с MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/12/2018
uid: razor-pages/index
ms.openlocfilehash: 601d6ac2cb373c40fb1de5427b0ea6c299fa1f32
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296751"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="3c517-103">Введение в Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c517-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="3c517-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Райан Новак](https://github.com/rynowak) (Ryan Nowak)</span><span class="sxs-lookup"><span data-stu-id="3c517-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="3c517-105">Razor Pages — это новый аспект платформы MVC ASP.NET Core, который делает создание кодов сценариев для страниц проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="3c517-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="3c517-106">Если вам нужно руководство, использующее подход "модель-представление-контроллер", см. статью [Начало работы с MVC в ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="3c517-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="3c517-107">Этот документ содержит вводные сведения о Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3c517-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="3c517-108">Это не пошаговое руководство.</span><span class="sxs-lookup"><span data-stu-id="3c517-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="3c517-109">Если некоторые разделы покажутся вам слишком сложными, см. [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="3c517-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="3c517-110">Общие сведения об ASP.NET Core см. в разделе [Введение в ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="3c517-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c517-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3c517-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="3c517-112">Создание проекта Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3c517-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c517-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c517-113">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="3c517-114">Подробные инструкции по созданию проекта Razor Pages с помощью Visual Studio см. в статье [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="3c517-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c517-115">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3c517-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3c517-116">Из командной строки выполните команду `dotnet new webapp`.</span><span class="sxs-lookup"><span data-stu-id="3c517-116">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3c517-118">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="3c517-118">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="3c517-119">Откройте созданный файл *.csproj* в Visual Studio для Mac.</span><span class="sxs-lookup"><span data-stu-id="3c517-119">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c517-120">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3c517-120">Visual Studio Code</span></span>](#tab/visual-studio-code) 

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3c517-121">Из командной строки выполните команду `dotnet new webapp`.</span><span class="sxs-lookup"><span data-stu-id="3c517-121">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3c517-123">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="3c517-123">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3c517-124">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="3c517-124">.NET Core CLI</span></span>](#tab/netcore-cli) 

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3c517-125">Из командной строки выполните команду `dotnet new webapp`.</span><span class="sxs-lookup"><span data-stu-id="3c517-125">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3c517-127">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="3c517-127">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="3c517-128">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3c517-128">Razor Pages</span></span>

<span data-ttu-id="3c517-129">Функция Razor Pages активируется в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c517-129">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="3c517-130">Рассмотрим простую страницу: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="3c517-130">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="3c517-131">Представленный выше код выглядит как файл представления Razor</span><span class="sxs-lookup"><span data-stu-id="3c517-131">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="3c517-132">и отличается от него только директивой `@page`.</span><span class="sxs-lookup"><span data-stu-id="3c517-132">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="3c517-133">Директива `@page` превращает файл в действие MVC, а значит обрабатывает запросы напрямую, минуя контроллер.</span><span class="sxs-lookup"><span data-stu-id="3c517-133">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="3c517-134">Директива `@page` должна быть первой директивой Razor на странице.</span><span class="sxs-lookup"><span data-stu-id="3c517-134">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="3c517-135">Директива `@page` влияет на поведение всех остальных конструкций Razor.</span><span class="sxs-lookup"><span data-stu-id="3c517-135">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="3c517-136">Похожая страница с использованием класса `PageModel` показана в следующих двух файлах.</span><span class="sxs-lookup"><span data-stu-id="3c517-136">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="3c517-137">Файл *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3c517-137">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="3c517-138">Модель страницы *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c517-138">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="3c517-139">Как правило, файл класса `PageModel` называется так же, как файл Razor Pages, но с расширением *.cs*.</span><span class="sxs-lookup"><span data-stu-id="3c517-139">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="3c517-140">Например, представленная выше страница Razor Pages называется *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3c517-140">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="3c517-141">Файл, содержащий класс `PageModel`, называется *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="3c517-141">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="3c517-142">Сопоставления URL-адресов со страницами определяются расположением конкретной страницы в файловой системе.</span><span class="sxs-lookup"><span data-stu-id="3c517-142">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="3c517-143">В приведенной ниже таблице показаны пути Razor Pages и соответствующие URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="3c517-143">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="3c517-144">Имя файла и путь</span><span class="sxs-lookup"><span data-stu-id="3c517-144">File name and path</span></span>               | <span data-ttu-id="3c517-145">Соответствующий URL</span><span class="sxs-lookup"><span data-stu-id="3c517-145">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="3c517-146">*/Pages/index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3c517-146">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="3c517-147">`/` или `/Index`</span><span class="sxs-lookup"><span data-stu-id="3c517-147">`/` or `/Index`</span></span> |
| <span data-ttu-id="3c517-148">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3c517-148">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="3c517-149">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3c517-149">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="3c517-150">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3c517-150">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="3c517-151">`/Store` или `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="3c517-151">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="3c517-152">Примечания.</span><span class="sxs-lookup"><span data-stu-id="3c517-152">Notes:</span></span>

* <span data-ttu-id="3c517-153">Среда выполнения по умолчанию ищет файлы Razor Pages в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="3c517-153">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="3c517-154">Если в URL-адресе не указана конкретная страница, по умолчанию открывается страница `Index`.</span><span class="sxs-lookup"><span data-stu-id="3c517-154">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="3c517-155">Создание простой формы</span><span class="sxs-lookup"><span data-stu-id="3c517-155">Writing a basic form</span></span>

<span data-ttu-id="3c517-156">Функция Razor Pages предназначена для упрощения реализации типовых шаблонов, которые используются в браузерах, при создании приложения.</span><span class="sxs-lookup"><span data-stu-id="3c517-156">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="3c517-157">[Привязки модели](xref:mvc/models/model-binding), [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и вспомогательные методы HTML *отлично работают* со свойствами, определенными в классе Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3c517-157">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="3c517-158">Рассмотрим страницу с простой формой связи для модели `Contact`.</span><span class="sxs-lookup"><span data-stu-id="3c517-158">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="3c517-159">В представленных в этой статье примерах `DbContext` инициализируется в файле [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="3c517-159">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="3c517-160">Модель данных:</span><span class="sxs-lookup"><span data-stu-id="3c517-160">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="3c517-161">Контекст базы данных:</span><span class="sxs-lookup"><span data-stu-id="3c517-161">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="3c517-162">Файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3c517-162">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="3c517-163">Модель страницы *Pages/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c517-163">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="3c517-164">Как правило, класс `PageModel` называется `<PageName>Model` и находится в том же пространстве имен, что и страница.</span><span class="sxs-lookup"><span data-stu-id="3c517-164">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="3c517-165">Класс `PageModel` позволяет разделять логику страницы и ее представление.</span><span class="sxs-lookup"><span data-stu-id="3c517-165">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="3c517-166">Он определяет обработчики страницы для запросов, отправляемых на страницу, а также данные для ее визуализации.</span><span class="sxs-lookup"><span data-stu-id="3c517-166">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="3c517-167">Такое разделение позволяет управлять зависимостями страницы путем их [внедрения](xref:fundamentals/dependency-injection) и выполнять [модульное тестирование](xref:test/razor-pages-tests) страниц.</span><span class="sxs-lookup"><span data-stu-id="3c517-167">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="3c517-168">Страница содержит *метод обработчика* `OnPostAsync`, который выполняется по запросам `POST` (когда пользователь публикует форму).</span><span class="sxs-lookup"><span data-stu-id="3c517-168">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="3c517-169">Методы обработчика можно добавить для любой HTTP-команды.</span><span class="sxs-lookup"><span data-stu-id="3c517-169">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="3c517-170">Наиболее распространенные обработчики</span><span class="sxs-lookup"><span data-stu-id="3c517-170">The most common handlers are:</span></span>

* <span data-ttu-id="3c517-171">`OnGet` — инициализация необходимого для страницы состояния.</span><span class="sxs-lookup"><span data-stu-id="3c517-171">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="3c517-172">Пример обработчика [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="3c517-172">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="3c517-173">`OnPost` — обработка отправленных через форму данных.</span><span class="sxs-lookup"><span data-stu-id="3c517-173">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="3c517-174">Суффикс `Async` не является обязательным, но часто используется для асинхронных функций.</span><span class="sxs-lookup"><span data-stu-id="3c517-174">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="3c517-175">Код `OnPostAsync` в приведенном выше примере похож на то, что обычно пишут в контроллере.</span><span class="sxs-lookup"><span data-stu-id="3c517-175">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="3c517-176">Этот код типичен для Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3c517-176">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="3c517-177">Большинство примитивов MVC, включая [привязку модели](xref:mvc/models/model-binding), [проверку](xref:mvc/models/validation) и результаты действий, являются общими.</span><span class="sxs-lookup"><span data-stu-id="3c517-177">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="3c517-178">Предыдущий метод `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="3c517-178">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="3c517-179">Простая схема `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="3c517-179">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="3c517-180">Проверка на наличие ошибок проверки.</span><span class="sxs-lookup"><span data-stu-id="3c517-180">Check for validation errors.</span></span>

*  <span data-ttu-id="3c517-181">Если ошибок нет, сохранение данных и перенаправление.</span><span class="sxs-lookup"><span data-stu-id="3c517-181">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="3c517-182">Если есть ошибки, отображение страницы с сообщениями проверки.</span><span class="sxs-lookup"><span data-stu-id="3c517-182">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="3c517-183">Проверка на стороне клиента выполняется точно так же, как в традиционных приложениях MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c517-183">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="3c517-184">Во многих случаях ошибки проверки выявляются на клиентском компьютере и на сервер не отправляются.</span><span class="sxs-lookup"><span data-stu-id="3c517-184">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="3c517-185">После успешного ввода данных метод обработчика `OnPostAsync` вызывает метод обработчика `RedirectToPage`, чтобы получить экземпляр `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="3c517-185">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="3c517-186">`RedirectToPage` — это новый результат действия, аналогичный `RedirectToAction` или `RedirectToRoute`, но настроенный для страниц.</span><span class="sxs-lookup"><span data-stu-id="3c517-186">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="3c517-187">В приведенном выше примере он выполняет перенаправление на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="3c517-187">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="3c517-188">Более подробно `RedirectToPage` рассматривается в разделе [Создание URL для страниц](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="3c517-188">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="3c517-189">Если в отправленной форме имеются ошибки проверки (переданные на сервер), метод обработчика `OnPostAsync` вызывает метод обработчика `Page`.</span><span class="sxs-lookup"><span data-stu-id="3c517-189">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="3c517-190">`Page` возвращает экземпляр `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="3c517-190">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="3c517-191">Возвращение `Page` аналогично тому, как действия в контроллерах возвращают `View`.</span><span class="sxs-lookup"><span data-stu-id="3c517-191">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="3c517-192">`PageResult` — значение типа возвращаемого значения <!-- Review  --> по умолчанию для метода обработчика.</span><span class="sxs-lookup"><span data-stu-id="3c517-192">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="3c517-193">Метод обработчика, вернувший `void`, визуализирует страницу.</span><span class="sxs-lookup"><span data-stu-id="3c517-193">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="3c517-194">Для указания согласия на привязку модели в свойстве `Customer` используется атрибут `[BindProperty]`.</span><span class="sxs-lookup"><span data-stu-id="3c517-194">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="3c517-195">По умолчанию функция Razor Pages привязывает свойства ко всем командам, кроме GET.</span><span class="sxs-lookup"><span data-stu-id="3c517-195">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="3c517-196">Привязка к свойствам позволяет сократить объем необходимого кода.</span><span class="sxs-lookup"><span data-stu-id="3c517-196">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="3c517-197">Привязка уменьшает код за счет того, что для визуализации полей формы (`<input asp-for="Customer.Name" />`) и получения входных данных используется одно и то же свойство.</span><span class="sxs-lookup"><span data-stu-id="3c517-197">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="3c517-198">В целях обеспечения безопасности следует привязать данные запросов GET к свойствам модели страниц.</span><span class="sxs-lookup"><span data-stu-id="3c517-198">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="3c517-199">Проверьте введенные данные пользователя, прежде чем сопоставлять их со свойствами.</span><span class="sxs-lookup"><span data-stu-id="3c517-199">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="3c517-200">Это поведение полезно в случаях, когда используются строки запросов и значения маршрутов.</span><span class="sxs-lookup"><span data-stu-id="3c517-200">Opting in to this behavior is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="3c517-201">Чтобы привязать свойство к запросам GET, задайте для свойства `SupportsGet` атрибута `[BindProperty]` значение `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="3c517-201">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="3c517-202">Домашняя страница (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="3c517-202">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="3c517-203">Связанный класс `PageModel` (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="3c517-203">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="3c517-204">Файл *Index.cshtml* содержит следующую разметку для создания ссылки на правку для каждого контактного лица.</span><span class="sxs-lookup"><span data-stu-id="3c517-204">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="3c517-205">[Вспомогательный тег привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) использует атрибут `asp-route-{value}` для создания ссылки на страницу редактирования.</span><span class="sxs-lookup"><span data-stu-id="3c517-205">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="3c517-206">Эта ссылка содержит данные о маршруте с идентификатором контактного лица.</span><span class="sxs-lookup"><span data-stu-id="3c517-206">The link contains route data with the contact ID.</span></span> <span data-ttu-id="3c517-207">Например, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="3c517-207">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="3c517-208">Файл *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3c517-208">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="3c517-209">Первая строка содержит директиву `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="3c517-209">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="3c517-210">Ограничение маршрутизации `"{id:int}"` указывает, что страница должна принимать обращенные к ней запросы, которые содержат данные о маршруте `int`.</span><span class="sxs-lookup"><span data-stu-id="3c517-210">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="3c517-211">Если запрос к странице не содержит данные о маршруте, которые можно конвертировать в `int`, среда выполнения возвращает ошибку HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="3c517-211">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="3c517-212">Чтобы сделать идентификатор необязательным, добавьте `?` к ограничению маршрута:</span><span class="sxs-lookup"><span data-stu-id="3c517-212">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="3c517-213">Файл *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c517-213">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="3c517-214">Файл *Index.cshtml* также содержит разметку для создания кнопки удаления у каждого контакта клиента:</span><span class="sxs-lookup"><span data-stu-id="3c517-214">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="3c517-215">Во время обработки кнопки удаления в HTML ее `formaction` включает параметры для следующего:</span><span class="sxs-lookup"><span data-stu-id="3c517-215">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="3c517-216">Идентификатор контакта клиента, указанный атрибутом `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="3c517-216">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="3c517-217">Параметр `handler`, указанный атрибутом `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="3c517-217">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="3c517-218">Пример отображенной кнопки удаления с идентификатором контакта клиента `1`:</span><span class="sxs-lookup"><span data-stu-id="3c517-218">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="3c517-219">При выборе кнопки на сервер отправляется запрос формы `POST`.</span><span class="sxs-lookup"><span data-stu-id="3c517-219">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="3c517-220">По соглашению имя метода обработчика выбирается на основе значения параметра `handler` в соответствии со схемой `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="3c517-220">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="3c517-221">Так как `handler` — `delete` в этом примере, метод обработчика `OnPostDeleteAsync` используется для обработки запроса `POST`.</span><span class="sxs-lookup"><span data-stu-id="3c517-221">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="3c517-222">Если `asp-page-handler` имеет другое значение, например `remove`, выбирается метод обработчика страницы с именем `OnPostRemoveAsync`.</span><span class="sxs-lookup"><span data-stu-id="3c517-222">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="3c517-223">Метод `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="3c517-223">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="3c517-224">Принимает `id` из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="3c517-224">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="3c517-225">Отправляет в базу данных запрос контакта клиента с `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="3c517-225">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="3c517-226">Если контакт клиента найден, он удаляется из списка контактов.</span><span class="sxs-lookup"><span data-stu-id="3c517-226">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="3c517-227">База данных обновляется.</span><span class="sxs-lookup"><span data-stu-id="3c517-227">The database is updated.</span></span>
* <span data-ttu-id="3c517-228">Вызывает `RedirectToPage` для перенаправления на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="3c517-228">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-required"></a><span data-ttu-id="3c517-229">Маркировка свойств страницы как обязательных</span><span class="sxs-lookup"><span data-stu-id="3c517-229">Mark page properties required</span></span>

<span data-ttu-id="3c517-230">Свойства класса `PageModel` можно отметить атрибутом [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute):</span><span class="sxs-lookup"><span data-stu-id="3c517-230">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

<span data-ttu-id="3c517-231">[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]</span><span class="sxs-lookup"><span data-stu-id="3c517-231">[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]</span></span>

<span data-ttu-id="3c517-232">Дополнительные сведения см. в статье о [проверке модели](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="3c517-232">See [Model validation](xref:mvc/models/validation) for more information.</span></span>

## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="3c517-233">Управление запросами HEAD с помощью обработчика OnGet</span><span class="sxs-lookup"><span data-stu-id="3c517-233">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="3c517-234">Обработчик HEAD обычно создается и вызывается для выполнения запросов HEAD:</span><span class="sxs-lookup"><span data-stu-id="3c517-234">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span>

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="3c517-235">Если обработчик HEAD (`OnHead`) не определен, Razor Pages выполнит вызов обработчика страниц GET (`OnGet`) в ASP.NET Core 2.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="3c517-235">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="3c517-236">Вы можете явно выбрать такое поведение с помощью [метода SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) в `Startup.Configure` ASP.NET Core 2.1–2.x:</span><span class="sxs-lookup"><span data-stu-id="3c517-236">Opt in to this behavior with the [SetCompatibilityVersion method](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) in `Startup.Configure` for ASP.NET Core 2.1 to 2.x:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="3c517-237">`SetCompatibilityVersion` задает для параметра `AllowMappingHeadRequestsToGetHandler` Razor Pages значение `true`.</span><span class="sxs-lookup"><span data-stu-id="3c517-237">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="3c517-238">Вместо включения всех возможных поведений для версии 2.1 с помощью метода `SetCompatibilityVersion` вы можете задать конкретное поведение.</span><span class="sxs-lookup"><span data-stu-id="3c517-238">Rather than opting into all 2.1 behaviors with `SetCompatibilityVersion`, you can explicitly opt-in to specific behaviors.</span></span> <span data-ttu-id="3c517-239">Следующий код назначает запросам HEAD обработчик GET.</span><span class="sxs-lookup"><span data-stu-id="3c517-239">The following code opts into the mapping HEAD requests to the GET handler.</span></span>


```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```
::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="3c517-240">XSRF/CSRF и Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3c517-240">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="3c517-241">Вам не придется писать отдельный код для [проверки подлинности запросов](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="3c517-241">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="3c517-242">Razor Pages включает создание и проверку маркеров защиты от подделок по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3c517-242">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="3c517-243">Использование макетов, частичных реплик, шаблонов и вспомогательных функций тегов с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3c517-243">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="3c517-244">Pages работает со всеми функциями подсистемы просмотра Razor.</span><span class="sxs-lookup"><span data-stu-id="3c517-244">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="3c517-245">Макеты, частичные реплики, шаблоны, вспомогательные функции тегов, а также файлы *_ViewStart.cshtml* и *_ViewImports.cshtml* работают точно так же, как и в стандартных представлениях Razor.</span><span class="sxs-lookup"><span data-stu-id="3c517-245">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="3c517-246">Давайте упростим нашу страницу с помощью некоторых из этих функций.</span><span class="sxs-lookup"><span data-stu-id="3c517-246">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="3c517-247">Добавим [макет страницы](xref:mvc/views/layout) в файл *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3c517-247">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="3c517-248">Этот [макет](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="3c517-248">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="3c517-249">управляет макетом каждой страницы (кроме страниц с отказом от макета);</span><span class="sxs-lookup"><span data-stu-id="3c517-249">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="3c517-250">импортирует HTML-структуры, такие как JavaScript и таблицы стилей.</span><span class="sxs-lookup"><span data-stu-id="3c517-250">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="3c517-251">Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="3c517-251">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="3c517-252">Свойство [Layout](xref:mvc/views/layout#specifying-a-layout) определяется в файле *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3c517-252">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="3c517-253">Макет хранится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="3c517-253">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="3c517-254">Pages ищет другие представления (макеты, шаблоны, частичные реплики) в иерархическом порядке, начиная с той папки, где находится текущая страница.</span><span class="sxs-lookup"><span data-stu-id="3c517-254">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="3c517-255">Макет в папке *Pages* можно использовать на любой странице Razor, которая находится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="3c517-255">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="3c517-256">Корпорация Майкрософт рекомендует **не** размещать файл макета в папке *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="3c517-256">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="3c517-257">*Views/Shared* — это шаблон представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="3c517-257">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="3c517-258">Razor Pages опирается на иерархию папок, а не на условные обозначения путей.</span><span class="sxs-lookup"><span data-stu-id="3c517-258">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="3c517-259">Поиск представлений в Razor Pages охватывает папку *Pages*.</span><span class="sxs-lookup"><span data-stu-id="3c517-259">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="3c517-260">Макеты, шаблоны и частичные реплики *работают* с контроллерами MVC и стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="3c517-260">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="3c517-261">Добавим файл *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3c517-261">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="3c517-262">`@namespace` описывается далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="3c517-262">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="3c517-263">Директива `@addTagHelper` добавляет [встроенные вспомогательные теги](xref:mvc/views/tag-helpers/builtin-th/Index) на все страницы в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="3c517-263">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="3c517-264">Явное применение директивы `@namespace` на странице:</span><span class="sxs-lookup"><span data-stu-id="3c517-264">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="3c517-265">Данная директива задает пространство имен для страницы.</span><span class="sxs-lookup"><span data-stu-id="3c517-265">The directive sets the namespace for the page.</span></span> <span data-ttu-id="3c517-266">Включать пространство имен в директиву `@model` не требуется.</span><span class="sxs-lookup"><span data-stu-id="3c517-266">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="3c517-267">Если директива `@namespace` содержится в файле *_ViewImports.cshtml*, указанное пространство имен определяет префикс для созданного в Pages пространства имен, куда импортируется директива `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="3c517-267">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="3c517-268">Остальная часть созданного пространства имен (суффикс) представляет собой разделенный точками относительный путь между папкой с файлом *_ViewImports.cshtml* и папкой, содержащей страницу.</span><span class="sxs-lookup"><span data-stu-id="3c517-268">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="3c517-269">Например, класс `PageModel` в файле *Pages/Customers/Edit.cshtml.cs* задает пространство имен явно.</span><span class="sxs-lookup"><span data-stu-id="3c517-269">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="3c517-270">Файл *Pages/_ViewImports.cshtml* задает следующее пространство имен:</span><span class="sxs-lookup"><span data-stu-id="3c517-270">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="3c517-271">Сформированное пространство имен для файла *Pages/Customers/Edit.cshtml* Razor Pages совпадает с пространством имен класса `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="3c517-271">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="3c517-272">`@namespace` *также работает со стандартными представлениями Razor.*</span><span class="sxs-lookup"><span data-stu-id="3c517-272">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="3c517-273">Исходный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3c517-273">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="3c517-274">Обновленный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3c517-274">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="3c517-275">[Начальный проект Razor Pages](#rpvs17) содержит файл *Pages/_ValidationScriptsPartial.cshtml*, который подключает проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="3c517-275">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="3c517-276">Формирование URL-адресов для страниц</span><span class="sxs-lookup"><span data-stu-id="3c517-276">URL generation for Pages</span></span>

<span data-ttu-id="3c517-277">На представленной выше странице `Create` используется `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="3c517-277">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="3c517-278">Это приложение имеет следующую структуру файлов и папок:</span><span class="sxs-lookup"><span data-stu-id="3c517-278">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="3c517-279">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="3c517-279">*/Pages*</span></span>

  * <span data-ttu-id="3c517-280">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3c517-280">*Index.cshtml*</span></span>
  * <span data-ttu-id="3c517-281">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="3c517-281">*/Customers*</span></span>

    * <span data-ttu-id="3c517-282">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3c517-282">*Create.cshtml*</span></span>
    * <span data-ttu-id="3c517-283">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3c517-283">*Edit.cshtml*</span></span>
    * <span data-ttu-id="3c517-284">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3c517-284">*Index.cshtml*</span></span>

<span data-ttu-id="3c517-285">После успешного выполнения страницы *Pages/Customers/Create.cshtml* и *Pages/Customers/Edit.cshtml* перенаправляются на страницу *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3c517-285">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="3c517-286">Строка `/Index` составляет часть URL-адреса для доступа к указанной выше странице.</span><span class="sxs-lookup"><span data-stu-id="3c517-286">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="3c517-287">Строка `/Index` позволяет создавать URL-адреса для страницы *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3c517-287">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="3c517-288">Пример:</span><span class="sxs-lookup"><span data-stu-id="3c517-288">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="3c517-289">Имя страницы — это путь к странице из корневой папки */Pages*, включая начальный символ `/` (например, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="3c517-289">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="3c517-290">Предыдущие образцы создания URL-адреса обеспечивают расширенные параметры и функциональные возможности по сравнению с жестким заданием URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="3c517-290">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="3c517-291">Формирование URL-адресов включает [маршрутизацию](xref:mvc/controllers/routing) и позволяет генерировать и включать в код параметры в зависимости от того, как определяется маршрут в пути назначения.</span><span class="sxs-lookup"><span data-stu-id="3c517-291">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="3c517-292">Формирование URL-адресов для страниц поддерживает относительные имена.</span><span class="sxs-lookup"><span data-stu-id="3c517-292">URL generation for pages supports relative names.</span></span> <span data-ttu-id="3c517-293">В приведенной ниже таблице показано, какая страница индекса выбирается с каждым параметром `RedirectToPage` в *Pages/Customers/Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3c517-293">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="3c517-294">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="3c517-294">RedirectToPage(x)</span></span>| <span data-ttu-id="3c517-295">Страница</span><span class="sxs-lookup"><span data-stu-id="3c517-295">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="3c517-296">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="3c517-296">RedirectToPage("/Index")</span></span> | <span data-ttu-id="3c517-297">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="3c517-297">*Pages/Index*</span></span> |
| <span data-ttu-id="3c517-298">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="3c517-298">RedirectToPage("./Index");</span></span> | <span data-ttu-id="3c517-299">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="3c517-299">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="3c517-300">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="3c517-300">RedirectToPage("../Index")</span></span> | <span data-ttu-id="3c517-301">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="3c517-301">*Pages/Index*</span></span> |
| <span data-ttu-id="3c517-302">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="3c517-302">RedirectToPage("Index")</span></span>  | <span data-ttu-id="3c517-303">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="3c517-303">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="3c517-304">`RedirectToPage("Index")`, `RedirectToPage("./Index")` и `RedirectToPage("../Index")` — это <em>относительные имена</em>.</span><span class="sxs-lookup"><span data-stu-id="3c517-304">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="3c517-305">Для получения имени целевой страницы параметр `RedirectToPage` <em>комбинируется</em> с путем текущей страницы.</span><span class="sxs-lookup"><span data-stu-id="3c517-305">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="3c517-306">Привязка относительных имен полезна при создании сайтов со сложной структурой.</span><span class="sxs-lookup"><span data-stu-id="3c517-306">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="3c517-307">Если для связи между страницами в определенной папке используются относительные имена, эту папку можно переименовать.</span><span class="sxs-lookup"><span data-stu-id="3c517-307">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="3c517-308">При этом все ссылки останутся рабочими (так как не включают имя папки).</span><span class="sxs-lookup"><span data-stu-id="3c517-308">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="viewdata-attribute"></a><span data-ttu-id="3c517-309">Атрибут ViewData</span><span class="sxs-lookup"><span data-stu-id="3c517-309">ViewData attribute</span></span>

<span data-ttu-id="3c517-310">Данные могут передаваться на страницу с помощью атрибута [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="3c517-310">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="3c517-311">Свойства на контроллерах или моделях страниц Razor, отмеченные атрибутом `[ViewData]`, обладают своими собственными значениями, загружаемыми из [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="3c517-311">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="3c517-312">В следующем примере класс `AboutModel` содержит свойство `Title`, отмеченное атрибутом `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="3c517-312">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="3c517-313">Свойство `Title` задает заголовок страницы About.</span><span class="sxs-lookup"><span data-stu-id="3c517-313">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="3c517-314">На странице About доступ к свойству `Title` осуществляется как доступ к свойству модели.</span><span class="sxs-lookup"><span data-stu-id="3c517-314">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="3c517-315">В макете заголовок считывается из словаря ViewData.</span><span class="sxs-lookup"><span data-stu-id="3c517-315">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```
::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="3c517-316">TempData</span><span class="sxs-lookup"><span data-stu-id="3c517-316">TempData</span></span>

<span data-ttu-id="3c517-317">ASP.NET Core позволяет использовать свойство [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) в [контроллере](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="3c517-317">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="3c517-318">Это свойство хранит данные до тех пор, пока они не будут прочитаны.</span><span class="sxs-lookup"><span data-stu-id="3c517-318">This property stores data until it's read.</span></span> <span data-ttu-id="3c517-319">Для проверки данных без удаления можно использовать методы `Keep` и `Peek`.</span><span class="sxs-lookup"><span data-stu-id="3c517-319">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="3c517-320">`TempData` удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса.</span><span class="sxs-lookup"><span data-stu-id="3c517-320">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="3c517-321">Атрибут `[TempData]` появился в ASP.NET 2.0 и поддерживается в контроллерах и на страницах.</span><span class="sxs-lookup"><span data-stu-id="3c517-321">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="3c517-322">В следующем коде значение `Message` задается с помощью `TempData`:</span><span class="sxs-lookup"><span data-stu-id="3c517-322">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="3c517-323">Представленная ниже разметка в файле *Pages/Customers/Index.cshtml* отображает значение `Message` с помощью `TempData`.</span><span class="sxs-lookup"><span data-stu-id="3c517-323">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="3c517-324">Модель страницы *Pages/Customers/Index.cshtml.cs* применяет атрибут `[TempData]` к свойству `Message`.</span><span class="sxs-lookup"><span data-stu-id="3c517-324">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="3c517-325">Дополнительные сведения см. в разделе [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="3c517-325">See [TempData](xref:fundamentals/app-state#tempdata) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="3c517-326">Несколько обработчиков на страницу</span><span class="sxs-lookup"><span data-stu-id="3c517-326">Multiple handlers per page</span></span>

<span data-ttu-id="3c517-327">Следующая страница формирует разметку для двух обработчиков страницы с помощью вспомогательного тега `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="3c517-327">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="3c517-328">Форма в предыдущем примере включает две кнопки отправки, каждая из которых отправляет данные на отдельный URL-адрес с помощью `FormActionTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="3c517-328">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="3c517-329">Атрибут `asp-page-handler` является дополнением к `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="3c517-329">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="3c517-330">Атрибут `asp-page-handler` формирует URL-адреса, ,которые используются для отправки данных в каждый из методов обработчиков, определенных страницей.</span><span class="sxs-lookup"><span data-stu-id="3c517-330">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="3c517-331">`asp-page` не задается, так как пример сопоставлен с текущей страницей.</span><span class="sxs-lookup"><span data-stu-id="3c517-331">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="3c517-332">Модель страницы</span><span class="sxs-lookup"><span data-stu-id="3c517-332">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="3c517-333">В представленном выше коде используются *именованные методы обработчика*.</span><span class="sxs-lookup"><span data-stu-id="3c517-333">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="3c517-334">Именованные методы обработчика создаются путем размещения определенного текста в имени после `On<HTTP Verb>` и перед `Async` (если есть).</span><span class="sxs-lookup"><span data-stu-id="3c517-334">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="3c517-335">В приведенном выше примере использовались методы страницы OnPost**JoinList**Async и OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="3c517-335">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="3c517-336">Если убрать *OnPost* и *Async*, имена обработчиков будут выглядеть как `JoinList` и `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="3c517-336">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="3c517-337">При использовании представленного выше кода URL-путь для отправки данных в `OnPostJoinListAsync` будет выглядеть как `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="3c517-337">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="3c517-338">URL-путь для отправки данных в `OnPostJoinListUCAsync` будет иметь вид `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="3c517-338">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="3c517-339">Пользовательские маршруты</span><span class="sxs-lookup"><span data-stu-id="3c517-339">Custom routes</span></span>

<span data-ttu-id="3c517-340">С помощью директивы `@page` можно сделать следующее.</span><span class="sxs-lookup"><span data-stu-id="3c517-340">Use the `@page` directive to:</span></span>

* <span data-ttu-id="3c517-341">Указать пользовательский маршрут к странице.</span><span class="sxs-lookup"><span data-stu-id="3c517-341">Specify a custom route to a page.</span></span> <span data-ttu-id="3c517-342">Например, можно задать маршрут к странице "Сведения" `/Some/Other/Path`: `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="3c517-342">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="3c517-343">Добавить сегменты к маршруту страницы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3c517-343">Append segments to a page's default route.</span></span> <span data-ttu-id="3c517-344">Например, к такому маршруту можно добавить сегмент item: `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="3c517-344">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="3c517-345">Добавить параметры к маршруту страницы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3c517-345">Append parameters to a page's default route.</span></span> <span data-ttu-id="3c517-346">Например, для страницы с `@page "{id}"` может потребоваться параметр идентификатора `id`.</span><span class="sxs-lookup"><span data-stu-id="3c517-346">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="3c517-347">Поддерживается путь относительно корня, заданный знаком тильды (`~`) в начале пути.</span><span class="sxs-lookup"><span data-stu-id="3c517-347">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="3c517-348">Например, `@page "~/Some/Other/Path"` равносильно `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="3c517-348">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="3c517-349">Вы можете изменить строку запроса `?handler=JoinList` в URL-адресе на сегмент маршрута `/JoinList`, указав шаблон маршрута `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="3c517-349">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="3c517-350">Если вы не хотите, чтобы в URL-адресе отображалась строка запроса `?handler=JoinList`, измените маршрут таким образом, чтобы в качестве пути в URL-адресе указывалось имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="3c517-350">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="3c517-351">Для настройки маршрута можно добавить после директивы `@page` шаблон маршрута, заключенные в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="3c517-351">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="3c517-352">При использовании представленного выше кода URL-путь для отправки данных в `OnPostJoinListAsync` будет выглядеть как `http://localhost:5000/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="3c517-352">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="3c517-353">URL-путь для отправки данных в `OnPostJoinListUCAsync` будет иметь вид `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="3c517-353">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="3c517-354">Символ `?` после `handler` означает, что параметр маршрута является необязательным.</span><span class="sxs-lookup"><span data-stu-id="3c517-354">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="3c517-355">Конфигурация и параметры</span><span class="sxs-lookup"><span data-stu-id="3c517-355">Configuration and settings</span></span>

<span data-ttu-id="3c517-356">Чтобы настроить дополнительные параметры, воспользуйтесь методом расширения `AddRazorPagesOptions` в построителе MVC:</span><span class="sxs-lookup"><span data-stu-id="3c517-356">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="3c517-357">В настоящее время `RazorPagesOptions` позволяет задавать корневую папку для страниц и добавлять для них обозначения моделей приложений.</span><span class="sxs-lookup"><span data-stu-id="3c517-357">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="3c517-358">В дальнейшем мы расширим спектр его возможностей.</span><span class="sxs-lookup"><span data-stu-id="3c517-358">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="3c517-359">Сведения о предварительной компиляции представлений см. в статье [Компиляция представлений Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="3c517-359">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="3c517-360">[Загрузить или просмотреть пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="3c517-360">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="3c517-361">См. раздел [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start), который дает дополнительную информацию к введению.</span><span class="sxs-lookup"><span data-stu-id="3c517-361">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="3c517-362">Указание местонахождения Razor Pages в корне каталога</span><span class="sxs-lookup"><span data-stu-id="3c517-362">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="3c517-363">По умолчанию Razor Pages находится в корне каталога */Pages*.</span><span class="sxs-lookup"><span data-stu-id="3c517-363">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="3c517-364">Добавьте [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в корне содержимого Razor Pages ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) приложения:</span><span class="sxs-lookup"><span data-stu-id="3c517-364">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="3c517-365">Указание местонахождения Razor Pages в пользовательском корневом каталоге</span><span class="sxs-lookup"><span data-stu-id="3c517-365">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="3c517-366">Добавьте [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в пользовательском корневом каталоге в приложении (укажите относительный путь):</span><span class="sxs-lookup"><span data-stu-id="3c517-366">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="3c517-367">См. также</span><span class="sxs-lookup"><span data-stu-id="3c517-367">See also</span></span>

* [<span data-ttu-id="3c517-368">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c517-368">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="3c517-369">Синтаксис Razor</span><span class="sxs-lookup"><span data-stu-id="3c517-369">Razor syntax</span></span>](xref:mvc/views/razor)
* [<span data-ttu-id="3c517-370">Начало работы с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3c517-370">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="3c517-371">Соглашения об авторизации Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3c517-371">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="3c517-372">Пользовательские поставщики моделей маршрутов и страниц Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3c517-372">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* [<span data-ttu-id="3c517-373">Модульные тесты страниц Razor</span><span class="sxs-lookup"><span data-stu-id="3c517-373">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)
