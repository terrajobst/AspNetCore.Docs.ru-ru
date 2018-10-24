---
title: Введение в Razor Pages в ASP.NET Core
author: Rick-Anderson
description: Сведения о применении в ASP.NET Core функции Razor Pages, которая делает создание кодов сценариев для страниц проще и эффективнее по сравнению с MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/12/2018
uid: razor-pages/index
ms.openlocfilehash: 7bd597acedade65f2be53f4f50a436981e576b1f
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326125"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="a7c2d-103">Введение в Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a7c2d-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="a7c2d-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Райан Новак](https://github.com/rynowak) (Ryan Nowak)</span><span class="sxs-lookup"><span data-stu-id="a7c2d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="a7c2d-105">Razor Pages — это новый аспект платформы MVC ASP.NET Core, который делает создание кодов сценариев для страниц проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="a7c2d-106">Если вам нужно руководство, использующее подход "модель-представление-контроллер", см. статью [Начало работы с MVC в ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="a7c2d-107">Этот документ содержит вводные сведения о Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="a7c2d-108">Это не пошаговое руководство.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="a7c2d-109">Если некоторые разделы покажутся вам слишком сложными, см. [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="a7c2d-110">Общие сведения об ASP.NET Core см. в разделе [Введение в ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7c2d-111">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="a7c2d-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="a7c2d-112">Создание проекта Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a7c2d-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a7c2d-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7c2d-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a7c2d-114">Подробные инструкции по созданию проекта Razor Pages с помощью Visual Studio см. в статье [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a7c2d-115">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a7c2d-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7c2d-116">Из командной строки выполните команду `dotnet new webapp`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-116">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a7c2d-117">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-117">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="a7c2d-118">Откройте созданный файл *.csproj* в Visual Studio для Mac.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a7c2d-119">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7c2d-120">Из командной строки выполните команду `dotnet new webapp`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-120">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a7c2d-121">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-121">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a7c2d-122">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="a7c2d-122">.NET Core CLI</span></span>](#tab/netcore-cli)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7c2d-123">Из командной строки выполните команду `dotnet new webapp`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-123">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a7c2d-124">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-124">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="a7c2d-125">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a7c2d-125">Razor Pages</span></span>

<span data-ttu-id="a7c2d-126">Функция Razor Pages активируется в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-126">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="a7c2d-127">Рассмотрим простую страницу: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="a7c2d-127">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="a7c2d-128">Представленный выше код выглядит как файл представления Razor</span><span class="sxs-lookup"><span data-stu-id="a7c2d-128">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="a7c2d-129">и отличается от него только директивой `@page`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="a7c2d-130">Директива `@page` превращает файл в действие MVC, а значит обрабатывает запросы напрямую, минуя контроллер.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="a7c2d-131">Директива `@page` должна быть первой директивой Razor на странице.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="a7c2d-132">Директива `@page` влияет на поведение всех остальных конструкций Razor.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-132">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="a7c2d-133">Похожая страница с использованием класса `PageModel` показана в следующих двух файлах.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-133">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="a7c2d-134">Файл *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-134">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="a7c2d-135">Модель страницы *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-135">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="a7c2d-136">Как правило, файл класса `PageModel` называется так же, как файл Razor Pages, но с расширением *.cs*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-136">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="a7c2d-137">Например, представленная выше страница Razor Pages называется *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-137">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="a7c2d-138">Файл, содержащий класс `PageModel`, называется *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-138">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="a7c2d-139">Сопоставления URL-адресов со страницами определяются расположением конкретной страницы в файловой системе.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-139">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="a7c2d-140">В приведенной ниже таблице показаны пути Razor Pages и соответствующие URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-140">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="a7c2d-141">Имя файла и путь</span><span class="sxs-lookup"><span data-stu-id="a7c2d-141">File name and path</span></span>               | <span data-ttu-id="a7c2d-142">Соответствующий URL</span><span class="sxs-lookup"><span data-stu-id="a7c2d-142">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="a7c2d-143">*/Pages/index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-143">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="a7c2d-144">`/` или `/Index`</span><span class="sxs-lookup"><span data-stu-id="a7c2d-144">`/` or `/Index`</span></span> |
| <span data-ttu-id="a7c2d-145">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-145">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="a7c2d-146">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-146">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="a7c2d-147">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-147">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="a7c2d-148">`/Store` или `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="a7c2d-148">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="a7c2d-149">Примечания:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-149">Notes:</span></span>

* <span data-ttu-id="a7c2d-150">Среда выполнения по умолчанию ищет файлы Razor Pages в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-150">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="a7c2d-151">Если в URL-адресе не указана конкретная страница, по умолчанию открывается страница `Index`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-151">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="a7c2d-152">Создание простой формы</span><span class="sxs-lookup"><span data-stu-id="a7c2d-152">Writing a basic form</span></span>

<span data-ttu-id="a7c2d-153">Функция Razor Pages предназначена для упрощения реализации типовых шаблонов, которые используются в браузерах, при создании приложения.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-153">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="a7c2d-154">[Привязки модели](xref:mvc/models/model-binding), [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и вспомогательные методы HTML *отлично работают* со свойствами, определенными в классе Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-154">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="a7c2d-155">Рассмотрим страницу с простой формой связи для модели `Contact`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-155">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="a7c2d-156">В представленных в этой статье примерах `DbContext` инициализируется в файле [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-156">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="a7c2d-157">Модель данных:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-157">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="a7c2d-158">Контекст базы данных:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-158">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="a7c2d-159">Файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-159">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="a7c2d-160">Модель страницы *Pages/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-160">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="a7c2d-161">Как правило, класс `PageModel` называется `<PageName>Model` и находится в том же пространстве имен, что и страница.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-161">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="a7c2d-162">Класс `PageModel` позволяет разделять логику страницы и ее представление.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-162">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="a7c2d-163">Он определяет обработчики страницы для запросов, отправляемых на страницу, а также данные для ее визуализации.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-163">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="a7c2d-164">Такое разделение позволяет управлять зависимостями страницы путем их [внедрения](xref:fundamentals/dependency-injection) и выполнять [модульное тестирование](xref:test/razor-pages-tests) страниц.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-164">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="a7c2d-165">Страница содержит *метод обработчика* `OnPostAsync`, который выполняется по запросам `POST` (когда пользователь публикует форму).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="a7c2d-166">Методы обработчика можно добавить для любой HTTP-команды.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-166">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="a7c2d-167">Наиболее распространенные обработчики</span><span class="sxs-lookup"><span data-stu-id="a7c2d-167">The most common handlers are:</span></span>

* <span data-ttu-id="a7c2d-168">`OnGet` — инициализация необходимого для страницы состояния.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="a7c2d-169">Пример обработчика [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-169">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="a7c2d-170">`OnPost` — обработка отправленных через форму данных.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="a7c2d-171">Суффикс `Async` не является обязательным, но часто используется для асинхронных функций.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="a7c2d-172">Код `OnPostAsync` в приведенном выше примере похож на то, что обычно пишут в контроллере.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-172">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="a7c2d-173">Этот код типичен для Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-173">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="a7c2d-174">Большинство примитивов MVC, включая [привязку модели](xref:mvc/models/model-binding), [проверку](xref:mvc/models/validation) и результаты действий, являются общими.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-174">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="a7c2d-175">Предыдущий метод `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-175">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="a7c2d-176">Простая схема `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="a7c2d-176">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="a7c2d-177">Проверка на наличие ошибок проверки.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-177">Check for validation errors.</span></span>

*  <span data-ttu-id="a7c2d-178">Если ошибок нет, сохранение данных и перенаправление.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-178">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="a7c2d-179">Если есть ошибки, отображение страницы с сообщениями проверки.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-179">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="a7c2d-180">Проверка на стороне клиента выполняется точно так же, как в традиционных приложениях MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-180">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="a7c2d-181">Во многих случаях ошибки проверки выявляются на клиентском компьютере и на сервер не отправляются.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="a7c2d-182">После успешного ввода данных метод обработчика `OnPostAsync` вызывает метод обработчика `RedirectToPage`, чтобы получить экземпляр `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-182">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="a7c2d-183">`RedirectToPage` — это новый результат действия, аналогичный `RedirectToAction` или `RedirectToRoute`, но настроенный для страниц.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-183">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="a7c2d-184">В приведенном выше примере он выполняет перенаправление на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-184">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="a7c2d-185">Более подробно `RedirectToPage` рассматривается в разделе [Создание URL для страниц](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-185">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="a7c2d-186">Если в отправленной форме имеются ошибки проверки (переданные на сервер), метод обработчика `OnPostAsync` вызывает метод обработчика `Page`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-186">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="a7c2d-187">`Page` возвращает экземпляр `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-187">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="a7c2d-188">Возвращение `Page` аналогично тому, как действия в контроллерах возвращают `View`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-188">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="a7c2d-189">`PageResult` — значение типа возвращаемого значения <!-- Review  --> по умолчанию для метода обработчика.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-189">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="a7c2d-190">Метод обработчика, вернувший `void`, визуализирует страницу.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-190">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="a7c2d-191">Для указания согласия на привязку модели в свойстве `Customer` используется атрибут `[BindProperty]`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-191">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="a7c2d-192">По умолчанию функция Razor Pages привязывает свойства ко всем командам, кроме GET.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-192">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="a7c2d-193">Привязка к свойствам позволяет сократить объем необходимого кода.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-193">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="a7c2d-194">Привязка уменьшает код за счет того, что для визуализации полей формы (`<input asp-for="Customer.Name" />`) и получения входных данных используется одно и то же свойство.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-194">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="a7c2d-195">В целях обеспечения безопасности следует привязать данные запросов GET к свойствам модели страниц.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-195">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="a7c2d-196">Проверьте введенные данные пользователя, прежде чем сопоставлять их со свойствами.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-196">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="a7c2d-197">Это поведение полезно в случаях, когда используются строки запросов и значения маршрутов.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-197">Opting in to this behavior is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="a7c2d-198">Чтобы привязать свойство к запросам GET, задайте для свойства `SupportsGet` атрибута `[BindProperty]` значение `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="a7c2d-198">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="a7c2d-199">Домашняя страница (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="a7c2d-199">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="a7c2d-200">Связанный класс `PageModel` (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a7c2d-200">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="a7c2d-201">Файл *Index.cshtml* содержит следующую разметку для создания ссылки на правку для каждого контактного лица.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-201">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="a7c2d-202">[Вспомогательный тег привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) использует атрибут `asp-route-{value}` для создания ссылки на страницу редактирования.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-202">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="a7c2d-203">Эта ссылка содержит данные о маршруте с идентификатором контактного лица.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-203">The link contains route data with the contact ID.</span></span> <span data-ttu-id="a7c2d-204">Например, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-204">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="a7c2d-205">Файл *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-205">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="a7c2d-206">Первая строка содержит директиву `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-206">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="a7c2d-207">Ограничение маршрутизации `"{id:int}"` указывает, что страница должна принимать обращенные к ней запросы, которые содержат данные о маршруте `int`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-207">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="a7c2d-208">Если запрос к странице не содержит данные о маршруте, которые можно конвертировать в `int`, среда выполнения возвращает ошибку HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-208">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="a7c2d-209">Чтобы сделать идентификатор необязательным, добавьте `?` к ограничению маршрута:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-209">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="a7c2d-210">Файл *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-210">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="a7c2d-211">Файл *Index.cshtml* также содержит разметку для создания кнопки удаления у каждого контакта клиента:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-211">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="a7c2d-212">Во время обработки кнопки удаления в HTML ее `formaction` включает параметры для следующего:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-212">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="a7c2d-213">Идентификатор контакта клиента, указанный атрибутом `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-213">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="a7c2d-214">Параметр `handler`, указанный атрибутом `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-214">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="a7c2d-215">Пример отображенной кнопки удаления с идентификатором контакта клиента `1`:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-215">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="a7c2d-216">При выборе кнопки на сервер отправляется запрос формы `POST`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-216">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="a7c2d-217">По соглашению имя метода обработчика выбирается на основе значения параметра `handler` в соответствии со схемой `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-217">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="a7c2d-218">Так как `handler` — `delete` в этом примере, метод обработчика `OnPostDeleteAsync` используется для обработки запроса `POST`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-218">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="a7c2d-219">Если `asp-page-handler` имеет другое значение, например `remove`, выбирается метод обработчика страницы с именем `OnPostRemoveAsync`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-219">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="a7c2d-220">Метод `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-220">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="a7c2d-221">Принимает `id` из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-221">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="a7c2d-222">Отправляет в базу данных запрос контакта клиента с `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-222">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="a7c2d-223">Если контакт клиента найден, он удаляется из списка контактов.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-223">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="a7c2d-224">База данных обновляется.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-224">The database is updated.</span></span>
* <span data-ttu-id="a7c2d-225">Вызывает `RedirectToPage` для перенаправления на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-225">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-required"></a><span data-ttu-id="a7c2d-226">Маркировка свойств страницы как обязательных</span><span class="sxs-lookup"><span data-stu-id="a7c2d-226">Mark page properties required</span></span>

<span data-ttu-id="a7c2d-227">Свойства класса `PageModel` можно отметить атрибутом [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute):</span><span class="sxs-lookup"><span data-stu-id="a7c2d-227">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="a7c2d-228">Дополнительные сведения см. в статье о [проверке модели](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-228">See [Model validation](xref:mvc/models/validation) for more information.</span></span>

## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="a7c2d-229">Управление запросами HEAD с помощью обработчика OnGet</span><span class="sxs-lookup"><span data-stu-id="a7c2d-229">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="a7c2d-230">Запросы HEAD позволяют получать заголовки для определенного ресурса.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-230">HEAD requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="a7c2d-231">В отличие от запросов GET запросы HEAD не возвращают текст ответа.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-231">Unlike GET requests, HEAD requests don't return a response body.</span></span> 

<span data-ttu-id="a7c2d-232">Обработчик HEAD обычно создается и вызывается для выполнения запросов HEAD:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-232">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="a7c2d-233">Если обработчик HEAD (`OnHead`) не определен, Razor Pages выполнит вызов обработчика страниц GET (`OnGet`) в ASP.NET Core 2.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-233">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="a7c2d-234">В ASP.NET Core 2.1 и 2.2 такая ситуация возникает при использовании [SetCompatibilityVersion](xref:mvc/compatibility-version) в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-234">In ASP.NET Core 2.1 and 2.2, this behavior occurs with the [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.Configure`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="a7c2d-235">Шаблоны по умолчанию создают вызов `SetCompatibilityVersion` в ASP.NET Core 2.1 и 2.2.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-235">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span>

<span data-ttu-id="a7c2d-236">`SetCompatibilityVersion` задает для параметра `AllowMappingHeadRequestsToGetHandler` Razor Pages значение `true`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-236">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="a7c2d-237">Вместо включения всех возможных поведений для версии 2.1 с помощью метода `SetCompatibilityVersion` вы можете задать конкретное поведение.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-237">Rather than opting into all 2.1 behaviors with `SetCompatibilityVersion`, you can explicitly opt-in to specific behaviors.</span></span> <span data-ttu-id="a7c2d-238">Следующий код назначает запросам HEAD обработчик GET.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-238">The following code opts into the mapping HEAD requests to the GET handler.</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="a7c2d-239">XSRF/CSRF и Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a7c2d-239">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="a7c2d-240">Вам не придется писать отдельный код для [проверки подлинности запросов](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-240">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="a7c2d-241">Razor Pages включает создание и проверку маркеров защиты от подделок по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-241">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="a7c2d-242">Использование макетов, частичных реплик, шаблонов и вспомогательных функций тегов с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a7c2d-242">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="a7c2d-243">Pages работает со всеми функциями подсистемы просмотра Razor.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-243">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="a7c2d-244">Макеты, частичные реплики, шаблоны, вспомогательные функции тегов, а также файлы *_ViewStart.cshtml* и *_ViewImports.cshtml* работают точно так же, как и в стандартных представлениях Razor.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-244">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="a7c2d-245">Давайте упростим нашу страницу с помощью некоторых из этих функций.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-245">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7c2d-246">Добавим [макет страницы](xref:mvc/views/layout) в файл *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-246">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a7c2d-247">Добавим [макет страницы](xref:mvc/views/layout) в файл *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-247">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="a7c2d-248">Этот [макет](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="a7c2d-248">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="a7c2d-249">управляет макетом каждой страницы (кроме страниц с отказом от макета);</span><span class="sxs-lookup"><span data-stu-id="a7c2d-249">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="a7c2d-250">импортирует HTML-структуры, такие как JavaScript и таблицы стилей.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-250">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="a7c2d-251">Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-251">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="a7c2d-252">Свойство [Layout](xref:mvc/views/layout#specifying-a-layout) определяется в файле *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-252">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7c2d-253">Макет хранится в папке *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-253">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="a7c2d-254">Pages ищет другие представления (макеты, шаблоны, частичные реплики) в иерархическом порядке, начиная с той папки, где находится текущая страница.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-254">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="a7c2d-255">Макет в папке *Pages/Shared* можно использовать на любой странице Razor, которая находится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-255">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="a7c2d-256">Файл макета следует поместить в папку *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-256">The layout file should go in the *Pages/Shared* folder.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a7c2d-257">Макет хранится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-257">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="a7c2d-258">Pages ищет другие представления (макеты, шаблоны, частичные реплики) в иерархическом порядке, начиная с той папки, где находится текущая страница.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-258">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="a7c2d-259">Макет в папке *Pages* можно использовать на любой странице Razor, которая находится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-259">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

::: moniker-end

<span data-ttu-id="a7c2d-260">Корпорация Майкрософт рекомендует **не** размещать файл макета в папке *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-260">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="a7c2d-261">*Views/Shared* — это шаблон представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-261">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="a7c2d-262">Razor Pages опирается на иерархию папок, а не на условные обозначения путей.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-262">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="a7c2d-263">Поиск представлений в Razor Pages охватывает папку *Pages*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-263">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="a7c2d-264">Макеты, шаблоны и частичные реплики *работают* с контроллерами MVC и стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-264">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="a7c2d-265">Добавим файл *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-265">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="a7c2d-266">`@namespace` описывается далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-266">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="a7c2d-267">Директива `@addTagHelper` добавляет [встроенные вспомогательные теги](xref:mvc/views/tag-helpers/builtin-th/Index) на все страницы в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-267">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="a7c2d-268">Явное применение директивы `@namespace` на странице:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-268">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="a7c2d-269">Данная директива задает пространство имен для страницы.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-269">The directive sets the namespace for the page.</span></span> <span data-ttu-id="a7c2d-270">Включать пространство имен в директиву `@model` не требуется.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-270">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="a7c2d-271">Если директива `@namespace` содержится в файле *_ViewImports.cshtml*, указанное пространство имен определяет префикс для созданного в Pages пространства имен, куда импортируется директива `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-271">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="a7c2d-272">Остальная часть созданного пространства имен (суффикс) представляет собой разделенный точками относительный путь между папкой с файлом *_ViewImports.cshtml* и папкой, содержащей страницу.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-272">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="a7c2d-273">Например, класс `PageModel` в файле *Pages/Customers/Edit.cshtml.cs* задает пространство имен явно.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-273">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="a7c2d-274">Файл *Pages/_ViewImports.cshtml* задает следующее пространство имен:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-274">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="a7c2d-275">Сформированное пространство имен для файла *Pages/Customers/Edit.cshtml* Razor Pages совпадает с пространством имен класса `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-275">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="a7c2d-276">`@namespace` *также работает со стандартными представлениями Razor.*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-276">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="a7c2d-277">Исходный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-277">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="a7c2d-278">Обновленный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-278">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="a7c2d-279">[Начальный проект Razor Pages](#rpvs17) содержит файл *Pages/_ValidationScriptsPartial.cshtml*, который подключает проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-279">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="a7c2d-280">Дополнительные сведения о частичных представлениях см. в <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-280">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="a7c2d-281">Формирование URL-адресов для страниц</span><span class="sxs-lookup"><span data-stu-id="a7c2d-281">URL generation for Pages</span></span>

<span data-ttu-id="a7c2d-282">На представленной выше странице `Create` используется `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-282">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="a7c2d-283">Это приложение имеет следующую структуру файлов и папок:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-283">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="a7c2d-284">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-284">*/Pages*</span></span>

  * <span data-ttu-id="a7c2d-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-285">*Index.cshtml*</span></span>
  * <span data-ttu-id="a7c2d-286">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-286">*/Customers*</span></span>

    * <span data-ttu-id="a7c2d-287">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-287">*Create.cshtml*</span></span>
    * <span data-ttu-id="a7c2d-288">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-288">*Edit.cshtml*</span></span>
    * <span data-ttu-id="a7c2d-289">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-289">*Index.cshtml*</span></span>

<span data-ttu-id="a7c2d-290">После успешного выполнения страницы *Pages/Customers/Create.cshtml* и *Pages/Customers/Edit.cshtml* перенаправляются на страницу *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-290">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="a7c2d-291">Строка `/Index` составляет часть URL-адреса для доступа к указанной выше странице.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-291">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="a7c2d-292">Строка `/Index` позволяет создавать URL-адреса для страницы *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-292">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="a7c2d-293">Пример:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-293">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="a7c2d-294">Имя страницы — это путь к странице из корневой папки */Pages*, включая начальный символ `/` (например, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-294">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="a7c2d-295">Предыдущие образцы создания URL-адреса обеспечивают расширенные параметры и функциональные возможности по сравнению с жестким заданием URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-295">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="a7c2d-296">Формирование URL-адресов включает [маршрутизацию](xref:mvc/controllers/routing) и позволяет генерировать и включать в код параметры в зависимости от того, как определяется маршрут в пути назначения.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-296">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="a7c2d-297">Формирование URL-адресов для страниц поддерживает относительные имена.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-297">URL generation for pages supports relative names.</span></span> <span data-ttu-id="a7c2d-298">В приведенной ниже таблице показано, какая страница индекса выбирается с каждым параметром `RedirectToPage` в *Pages/Customers/Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-298">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="a7c2d-299">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="a7c2d-299">RedirectToPage(x)</span></span>| <span data-ttu-id="a7c2d-300">Страница</span><span class="sxs-lookup"><span data-stu-id="a7c2d-300">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="a7c2d-301">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="a7c2d-301">RedirectToPage("/Index")</span></span> | <span data-ttu-id="a7c2d-302">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-302">*Pages/Index*</span></span> |
| <span data-ttu-id="a7c2d-303">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="a7c2d-303">RedirectToPage("./Index");</span></span> | <span data-ttu-id="a7c2d-304">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-304">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="a7c2d-305">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="a7c2d-305">RedirectToPage("../Index")</span></span> | <span data-ttu-id="a7c2d-306">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-306">*Pages/Index*</span></span> |
| <span data-ttu-id="a7c2d-307">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="a7c2d-307">RedirectToPage("Index")</span></span>  | <span data-ttu-id="a7c2d-308">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="a7c2d-308">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="a7c2d-309">`RedirectToPage("Index")`, `RedirectToPage("./Index")` и `RedirectToPage("../Index")` — это <em>относительные имена</em>.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-309">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="a7c2d-310">Для получения имени целевой страницы параметр `RedirectToPage` <em>комбинируется</em> с путем текущей страницы.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-310">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="a7c2d-311">Привязка относительных имен полезна при создании сайтов со сложной структурой.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-311">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="a7c2d-312">Если для связи между страницами в определенной папке используются относительные имена, эту папку можно переименовать.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-312">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="a7c2d-313">При этом все ссылки останутся рабочими (так как не включают имя папки).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-313">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="viewdata-attribute"></a><span data-ttu-id="a7c2d-314">Атрибут ViewData</span><span class="sxs-lookup"><span data-stu-id="a7c2d-314">ViewData attribute</span></span>

<span data-ttu-id="a7c2d-315">Данные могут передаваться на страницу с помощью атрибута [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-315">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="a7c2d-316">Свойства на контроллерах или моделях страниц Razor, отмеченные атрибутом `[ViewData]`, обладают своими собственными значениями, загружаемыми из [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-316">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="a7c2d-317">В следующем примере класс `AboutModel` содержит свойство `Title`, отмеченное атрибутом `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-317">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="a7c2d-318">Свойство `Title` задает заголовок страницы About.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-318">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="a7c2d-319">На странице About доступ к свойству `Title` осуществляется как доступ к свойству модели.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-319">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="a7c2d-320">В макете заголовок считывается из словаря ViewData.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-320">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="a7c2d-321">TempData</span><span class="sxs-lookup"><span data-stu-id="a7c2d-321">TempData</span></span>

<span data-ttu-id="a7c2d-322">ASP.NET Core позволяет использовать свойство [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) в [контроллере](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-322">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="a7c2d-323">Это свойство хранит данные до тех пор, пока они не будут прочитаны.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-323">This property stores data until it's read.</span></span> <span data-ttu-id="a7c2d-324">Для проверки данных без удаления можно использовать методы `Keep` и `Peek`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-324">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="a7c2d-325">`TempData` удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-325">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="a7c2d-326">Атрибут `[TempData]` появился в ASP.NET 2.0 и поддерживается в контроллерах и на страницах.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-326">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="a7c2d-327">В следующем коде значение `Message` задается с помощью `TempData`:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-327">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="a7c2d-328">Представленная ниже разметка в файле *Pages/Customers/Index.cshtml* отображает значение `Message` с помощью `TempData`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-328">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="a7c2d-329">Модель страницы *Pages/Customers/Index.cshtml.cs* применяет атрибут `[TempData]` к свойству `Message`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-329">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="a7c2d-330">Дополнительные сведения см. в разделе [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-330">See [TempData](xref:fundamentals/app-state#tempdata) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="a7c2d-331">Несколько обработчиков на страницу</span><span class="sxs-lookup"><span data-stu-id="a7c2d-331">Multiple handlers per page</span></span>

<span data-ttu-id="a7c2d-332">Следующая страница формирует разметку для двух обработчиков страницы с помощью вспомогательного тега `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-332">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="a7c2d-333">Форма в предыдущем примере включает две кнопки отправки, каждая из которых отправляет данные на отдельный URL-адрес с помощью `FormActionTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-333">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="a7c2d-334">Атрибут `asp-page-handler` является дополнением к `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-334">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="a7c2d-335">Атрибут `asp-page-handler` формирует URL-адреса, ,которые используются для отправки данных в каждый из методов обработчиков, определенных страницей.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-335">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="a7c2d-336">`asp-page` не задается, так как пример сопоставлен с текущей страницей.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-336">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="a7c2d-337">Модель страницы</span><span class="sxs-lookup"><span data-stu-id="a7c2d-337">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="a7c2d-338">В представленном выше коде используются *именованные методы обработчика*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-338">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="a7c2d-339">Именованные методы обработчика создаются путем размещения определенного текста в имени после `On<HTTP Verb>` и перед `Async` (если есть).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-339">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="a7c2d-340">В приведенном выше примере использовались методы страницы OnPost**JoinList**Async и OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-340">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="a7c2d-341">Если убрать *OnPost* и *Async*, имена обработчиков будут выглядеть как `JoinList` и `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-341">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="a7c2d-342">При использовании представленного выше кода URL-путь для отправки данных в `OnPostJoinListAsync` будет выглядеть как `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-342">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="a7c2d-343">URL-путь для отправки данных в `OnPostJoinListUCAsync` будет иметь вид `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-343">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="a7c2d-344">Пользовательские маршруты</span><span class="sxs-lookup"><span data-stu-id="a7c2d-344">Custom routes</span></span>

<span data-ttu-id="a7c2d-345">С помощью директивы `@page` можно сделать следующее.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-345">Use the `@page` directive to:</span></span>

* <span data-ttu-id="a7c2d-346">Указать пользовательский маршрут к странице.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-346">Specify a custom route to a page.</span></span> <span data-ttu-id="a7c2d-347">Например, можно задать маршрут к странице "Сведения" `/Some/Other/Path`: `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-347">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="a7c2d-348">Добавить сегменты к маршруту страницы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-348">Append segments to a page's default route.</span></span> <span data-ttu-id="a7c2d-349">Например, к такому маршруту можно добавить сегмент item: `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-349">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="a7c2d-350">Добавить параметры к маршруту страницы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-350">Append parameters to a page's default route.</span></span> <span data-ttu-id="a7c2d-351">Например, для страницы с `@page "{id}"` может потребоваться параметр идентификатора `id`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-351">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="a7c2d-352">Поддерживается путь относительно корня, заданный знаком тильды (`~`) в начале пути.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-352">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="a7c2d-353">Например, `@page "~/Some/Other/Path"` равносильно `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-353">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="a7c2d-354">Вы можете изменить строку запроса `?handler=JoinList` в URL-адресе на сегмент маршрута `/JoinList`, указав шаблон маршрута `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-354">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="a7c2d-355">Если вы не хотите, чтобы в URL-адресе отображалась строка запроса `?handler=JoinList`, измените маршрут таким образом, чтобы в качестве пути в URL-адресе указывалось имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-355">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="a7c2d-356">Для настройки маршрута можно добавить после директивы `@page` шаблон маршрута, заключенные в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-356">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="a7c2d-357">При использовании представленного выше кода URL-путь для отправки данных в `OnPostJoinListAsync` будет выглядеть как `http://localhost:5000/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-357">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="a7c2d-358">URL-путь для отправки данных в `OnPostJoinListUCAsync` будет иметь вид `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-358">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="a7c2d-359">Символ `?` после `handler` означает, что параметр маршрута является необязательным.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-359">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="a7c2d-360">Конфигурация и параметры</span><span class="sxs-lookup"><span data-stu-id="a7c2d-360">Configuration and settings</span></span>

<span data-ttu-id="a7c2d-361">Чтобы настроить дополнительные параметры, воспользуйтесь методом расширения `AddRazorPagesOptions` в построителе MVC:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-361">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="a7c2d-362">В настоящее время `RazorPagesOptions` позволяет задавать корневую папку для страниц и добавлять для них обозначения моделей приложений.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-362">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="a7c2d-363">В дальнейшем мы расширим спектр его возможностей.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-363">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="a7c2d-364">Сведения о предварительной компиляции представлений см. в статье [Компиляция представлений Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-364">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="a7c2d-365">[Загрузить или просмотреть пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="a7c2d-365">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="a7c2d-366">См. раздел [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start), который дает дополнительную информацию к введению.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-366">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="a7c2d-367">Указание местонахождения Razor Pages в корне каталога</span><span class="sxs-lookup"><span data-stu-id="a7c2d-367">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="a7c2d-368">По умолчанию Razor Pages находится в корне каталога */Pages*.</span><span class="sxs-lookup"><span data-stu-id="a7c2d-368">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="a7c2d-369">Добавьте [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в корне содержимого Razor Pages ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) приложения:</span><span class="sxs-lookup"><span data-stu-id="a7c2d-369">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="a7c2d-370">Указание местонахождения Razor Pages в пользовательском корневом каталоге</span><span class="sxs-lookup"><span data-stu-id="a7c2d-370">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="a7c2d-371">Добавьте [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в пользовательском корневом каталоге в приложении (укажите относительный путь):</span><span class="sxs-lookup"><span data-stu-id="a7c2d-371">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="a7c2d-372">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a7c2d-372">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
