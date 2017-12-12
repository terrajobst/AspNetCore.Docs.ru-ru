---
title: "Введение в Razor Pages в ASP.NET Core"
author: Rick-Anderson
description: "Этот документ содержит общие сведения об использовании страниц Razor в ASP.NET Core для упрощения разработки в сценариях, где применяются страницы."
keywords: "ASP.NET Core, страницы Razor"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: 36dd2ad01f93ab1093bad84a58504a150c70ea16
ms.sourcegitcommit: 4925a91ef4130ddb333f187ab13defe66f2c6cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="54017-104">Введение в Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54017-104">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="54017-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Райан Новак](https://github.com/rynowak) (Ryan Nowak)</span><span class="sxs-lookup"><span data-stu-id="54017-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="54017-106">Razor Pages — это новая функция платформы MVC ASP.NET Core, которая делает создание кодов сценариев для страниц проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="54017-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="54017-107">Если вам требуется руководство на основе подхода "модель-представление-контроллер", см. статью [Начало работы с MVC ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="54017-107">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="54017-108">Этот документ содержит вводные сведения о Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="54017-108">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="54017-109">Это не пошаговое руководство.</span><span class="sxs-lookup"><span data-stu-id="54017-109">It's not a step by step tutorial.</span></span> <span data-ttu-id="54017-110">Если некоторые разделы покажутся вам сложными, см. документ [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="54017-110">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="54017-111">Предварительные требования ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="54017-111">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="54017-112">Установите [.NET Core](https://www.microsoft.com/net/core) 2.0.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="54017-112">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="54017-113">Если вы используете Visual Studio, установите [Visual Studio](https://www.visualstudio.com/vs/) 2017 версии 15.3 или более поздней версии, а также следующие рабочие нагрузки:</span><span class="sxs-lookup"><span data-stu-id="54017-113">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="54017-114">**ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="54017-114">**ASP.NET and web development**</span></span>
* <span data-ttu-id="54017-115">**Кроссплатформенная разработка .NET Core**</span><span class="sxs-lookup"><span data-stu-id="54017-115">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="54017-116">Создание проекта Razor Pages</span><span class="sxs-lookup"><span data-stu-id="54017-116">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="54017-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="54017-117">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="54017-118">Подробные инструкции по созданию проекта Razor Pages с помощью Visual Studio см. в статье [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="54017-118">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="54017-119">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="54017-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="54017-120">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="54017-120">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="54017-121">Откройте созданный файл *.csproj* в Visual Studio для Mac.</span><span class="sxs-lookup"><span data-stu-id="54017-121">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="54017-122">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="54017-122">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="54017-123">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="54017-123">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="54017-124">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="54017-124">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="54017-125">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="54017-125">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="54017-126">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="54017-126">Razor Pages</span></span>

<span data-ttu-id="54017-127">Функция Razor Pages активируется в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="54017-127">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="54017-128">Рассмотрим простую страницу: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="54017-128">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="54017-129">Представленный выше код выглядит как файл представления Razor</span><span class="sxs-lookup"><span data-stu-id="54017-129">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="54017-130">и отличается от него только директивой `@page`.</span><span class="sxs-lookup"><span data-stu-id="54017-130">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="54017-131">Директива `@page` превращает файл в действие MVC, а значит обрабатывает запросы напрямую, минуя контроллер.</span><span class="sxs-lookup"><span data-stu-id="54017-131">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="54017-132">Директива `@page` должна быть первой директивой Razor на странице.</span><span class="sxs-lookup"><span data-stu-id="54017-132">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="54017-133">Директива `@page` влияет на поведение всех остальных конструкций Razor.</span><span class="sxs-lookup"><span data-stu-id="54017-133">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="54017-134">Похожая страница с использованием класса `PageModel` показана в следующих двух файлах.</span><span class="sxs-lookup"><span data-stu-id="54017-134">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="54017-135">Файл *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="54017-135">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="54017-136">Файл кода "программной части" *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="54017-136">The *Pages/Index2.cshtml.cs* "code-behind" file:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="54017-137">Как правило, файл класса `PageModel` называется так же, как файл Razor Pages, но с расширением *.cs*.</span><span class="sxs-lookup"><span data-stu-id="54017-137">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="54017-138">Например, представленная выше страница Razor Pages называется *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="54017-138">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="54017-139">Файл, содержащий класс `PageModel`, называется *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="54017-139">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="54017-140">Сопоставления URL-адресов со страницами определяются расположением конкретной страницы в файловой системе.</span><span class="sxs-lookup"><span data-stu-id="54017-140">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="54017-141">В приведенной ниже таблице показаны пути Razor Pages и соответствующие URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="54017-141">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="54017-142">Имя файла и путь</span><span class="sxs-lookup"><span data-stu-id="54017-142">File name and path</span></span>               | <span data-ttu-id="54017-143">Соответствующий URL</span><span class="sxs-lookup"><span data-stu-id="54017-143">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="54017-144">*/Pages/index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="54017-144">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="54017-145">`/` или `/Index`</span><span class="sxs-lookup"><span data-stu-id="54017-145">`/` or `/Index`</span></span> |
| <span data-ttu-id="54017-146">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="54017-146">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="54017-147">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="54017-147">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="54017-148">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="54017-148">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="54017-149">`/Store` или `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="54017-149">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="54017-150">Примечания.</span><span class="sxs-lookup"><span data-stu-id="54017-150">Notes:</span></span>

* <span data-ttu-id="54017-151">Среда выполнения по умолчанию ищет файлы Razor Pages в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="54017-151">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="54017-152">Если в URL-адресе не указана конкретная страница, по умолчанию открывается страница `Index`.</span><span class="sxs-lookup"><span data-stu-id="54017-152">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="54017-153">Создание простой формы</span><span class="sxs-lookup"><span data-stu-id="54017-153">Writing a basic form</span></span>

<span data-ttu-id="54017-154">Функция Razor Pages предназначена для упрощения типовых шаблонов, которые используются в браузерах.</span><span class="sxs-lookup"><span data-stu-id="54017-154">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="54017-155">[Привязки модели](xref:mvc/models/model-binding), [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и вспомогательные методы HTML *отлично работают* со свойствами, определенными в классе Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="54017-155">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="54017-156">Рассмотрим страницу с простой формой связи для модели `Contact`.</span><span class="sxs-lookup"><span data-stu-id="54017-156">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="54017-157">В представленных в этой статье примерах `DbContext` инициализируется в файле [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="54017-157">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="54017-158">Модель данных:</span><span class="sxs-lookup"><span data-stu-id="54017-158">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="54017-159">Контекст базы данных:</span><span class="sxs-lookup"><span data-stu-id="54017-159">The db context:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="54017-160">Файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="54017-160">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="54017-161">Файл кода программной части *Pages/Create.cshtml.cs* для представления:</span><span class="sxs-lookup"><span data-stu-id="54017-161">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="54017-162">Как правило, класс `PageModel` называется `<PageName>Model` и находится в том же пространстве имен, что и страница.</span><span class="sxs-lookup"><span data-stu-id="54017-162">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="54017-163">Применение файла кода программной части `PageModel` позволяет выполнять модульное тестирование, но требует написания явного конструктора и класса.</span><span class="sxs-lookup"><span data-stu-id="54017-163">Using a `PageModel` code-behind file supports unit testing, but requires you to write an explicit constructor and class.</span></span> <span data-ttu-id="54017-164">Страницы без файлов кода программной части `PageModel` поддерживают компиляцию среды выполнения, что может пригодиться в процессе разработки.</span><span class="sxs-lookup"><span data-stu-id="54017-164">Pages without `PageModel` code-behind files support runtime compilation, which can be an advantage in development.</span></span>  <!-- review: advantage because you can make changes and refresh the browser without explicitly compiling the app -->

<span data-ttu-id="54017-165">Страница содержит *метод обработчика* `OnPostAsync`, который выполняется по запросам `POST` (когда пользователь публикует форму).</span><span class="sxs-lookup"><span data-stu-id="54017-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="54017-166">Методы обработчика можно добавить для любой HTTP-команды.</span><span class="sxs-lookup"><span data-stu-id="54017-166">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="54017-167">Наиболее распространенные обработчики</span><span class="sxs-lookup"><span data-stu-id="54017-167">The most common handlers are:</span></span>

* <span data-ttu-id="54017-168">`OnGet` — инициализация необходимого для страницы состояния.</span><span class="sxs-lookup"><span data-stu-id="54017-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="54017-169">Пример обработчика [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="54017-169">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="54017-170">`OnPost` — обработка отправленных через форму данных.</span><span class="sxs-lookup"><span data-stu-id="54017-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="54017-171">Суффикс `Async` не является обязательным, но часто используется для асинхронных функций.</span><span class="sxs-lookup"><span data-stu-id="54017-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="54017-172">Код `OnPostAsync` в приведенном выше примере похож на то, что обычно пишут в контроллере.</span><span class="sxs-lookup"><span data-stu-id="54017-172">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="54017-173">Этот код типичен для Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="54017-173">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="54017-174">Большинство примитивов MVC, включая [привязку модели](xref:mvc/models/model-binding), [проверку](xref:mvc/models/validation) и результаты действий, являются общими.</span><span class="sxs-lookup"><span data-stu-id="54017-174">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="54017-175">Предыдущий метод `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="54017-175">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="54017-176">Простая схема `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="54017-176">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="54017-177">Проверка на наличие ошибок проверки.</span><span class="sxs-lookup"><span data-stu-id="54017-177">Check for validation errors.</span></span>

*  <span data-ttu-id="54017-178">Если ошибок нет, сохранение данных и перенаправление.</span><span class="sxs-lookup"><span data-stu-id="54017-178">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="54017-179">Если есть ошибки, отображение страницы с сообщениями проверки.</span><span class="sxs-lookup"><span data-stu-id="54017-179">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="54017-180">Проверка на стороне клиента выполняется точно так же, как в традиционных приложениях MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54017-180">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="54017-181">Во многих случаях ошибки проверки выявляются на клиентском компьютере и на сервер не отправляются.</span><span class="sxs-lookup"><span data-stu-id="54017-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="54017-182">После успешного ввода данных метод обработчика `OnPostAsync` вызывает метод обработчика `RedirectToPage`, чтобы получить экземпляр `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="54017-182">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="54017-183">`RedirectToPage` — это новый результат действия, аналогичный `RedirectToAction` или `RedirectToRoute`, но настроенный для страниц.</span><span class="sxs-lookup"><span data-stu-id="54017-183">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="54017-184">В приведенном выше примере он выполняет перенаправление на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="54017-184">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="54017-185">Более подробно `RedirectToPage` рассматривается в разделе [Создание URL для страниц](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="54017-185">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="54017-186">Если в отправленной форме имеются ошибки проверки (переданные на сервер), метод обработчика `OnPostAsync` вызывает метод обработчика `Page`.</span><span class="sxs-lookup"><span data-stu-id="54017-186">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="54017-187">`Page` возвращает экземпляр `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="54017-187">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="54017-188">Возвращение `Page` аналогично тому, как действия в контроллерах возвращают `View`.</span><span class="sxs-lookup"><span data-stu-id="54017-188">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="54017-189">`PageResult` — значение типа возвращаемого значения <!-- Review  --> по умолчанию для метода обработчика.</span><span class="sxs-lookup"><span data-stu-id="54017-189">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="54017-190">Метод обработчика, вернувший `void`, визуализирует страницу.</span><span class="sxs-lookup"><span data-stu-id="54017-190">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="54017-191">Для указания согласия на привязку модели в свойстве `Customer` используется атрибут `[BindProperty]`.</span><span class="sxs-lookup"><span data-stu-id="54017-191">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="54017-192">По умолчанию функция Razor Pages привязывает свойства ко всем командам, кроме GET.</span><span class="sxs-lookup"><span data-stu-id="54017-192">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="54017-193">Привязка к свойствам позволяет сократить объем необходимого кода.</span><span class="sxs-lookup"><span data-stu-id="54017-193">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="54017-194">Привязка уменьшает код за счет того, что для визуализации полей формы (`<input asp-for="Customer.Name" />`) и получения входных данных используется одно и то же свойство.</span><span class="sxs-lookup"><span data-stu-id="54017-194">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="54017-195">Домашняя страница (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="54017-195">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="54017-196">Файл кода программной части *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="54017-196">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="54017-197">Файл *Index.cshtml* содержит следующую разметку для создания ссылки на правку для каждого контактного лица.</span><span class="sxs-lookup"><span data-stu-id="54017-197">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="54017-198">[Вспомогательный тег привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) использует атрибут [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#route) для создания ссылки на страницу редактирования.</span><span class="sxs-lookup"><span data-stu-id="54017-198">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#route) attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="54017-199">Эта ссылка содержит данные о маршруте с идентификатором контактного лица.</span><span class="sxs-lookup"><span data-stu-id="54017-199">The link contains route data with the contact ID.</span></span> <span data-ttu-id="54017-200">Например, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="54017-200">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="54017-201">Файл *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="54017-201">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="54017-202">Первая строка содержит директиву `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="54017-202">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="54017-203">Ограничение маршрутизации `"{id:int}"` указывает, что страница должна принимать обращенные к ней запросы, которые содержат данные о маршруте `int`.</span><span class="sxs-lookup"><span data-stu-id="54017-203">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="54017-204">Если запрос к странице не содержит данные о маршруте, которые можно конвертировать в `int`, среда выполнения возвращает ошибку HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="54017-204">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="54017-205">Файл *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="54017-205">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="54017-206">Файл *Index.cshtml* также содержит разметку для создания кнопки удаления у каждого контакта клиента:</span><span class="sxs-lookup"><span data-stu-id="54017-206">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="54017-207">Во время обработки кнопки удаления в HTML ее `formaction` включает параметры для следующего:</span><span class="sxs-lookup"><span data-stu-id="54017-207">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="54017-208">Идентификатор контакта клиента, указанный атрибутом `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="54017-208">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="54017-209">Параметр `handler`, указанный атрибутом `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="54017-209">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="54017-210">Пример отображенной кнопки удаления с идентификатором контакта клиента `1`:</span><span class="sxs-lookup"><span data-stu-id="54017-210">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="54017-211">При выборе кнопки на сервер отправляется запрос формы `POST`.</span><span class="sxs-lookup"><span data-stu-id="54017-211">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="54017-212">По соглашению имя метода обработчика выбирается на основе значения параметра `handler` в соответствии со схемой `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="54017-212">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="54017-213">Так как `handler` — `delete` в этом примере, метод обработчика `OnPostDeleteAsync` используется для обработки запроса `POST`.</span><span class="sxs-lookup"><span data-stu-id="54017-213">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="54017-214">Если `asp-page-handler` имеет другое значение, например `remove`, выбирается метод обработчика страницы с именем `OnPostRemoveAsync`.</span><span class="sxs-lookup"><span data-stu-id="54017-214">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="54017-215">Метод `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="54017-215">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="54017-216">Принимает `id` из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="54017-216">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="54017-217">Отправляет в базу данных запрос контакта клиента с `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="54017-217">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="54017-218">Если контакт клиента найден, он удаляется из списка контактов.</span><span class="sxs-lookup"><span data-stu-id="54017-218">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="54017-219">База данных обновляется.</span><span class="sxs-lookup"><span data-stu-id="54017-219">The database is updated.</span></span>
* <span data-ttu-id="54017-220">Вызывает `RedirectToPage` для перенаправления на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="54017-220">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="54017-221">XSRF/CSRF и Razor Pages</span><span class="sxs-lookup"><span data-stu-id="54017-221">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="54017-222">Вам не придется писать отдельный код для [проверки подлинности запросов](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="54017-222">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="54017-223">Razor Pages включает создание и проверку маркеров защиты от подделок по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="54017-223">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="54017-224">Использование макетов, частичных реплик, шаблонов и вспомогательных функций тегов с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="54017-224">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="54017-225">Pages работает со всеми функциями представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="54017-225">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="54017-226">Макеты, частичные реплики, шаблоны, вспомогательные функции тегов, а также файлы *_ViewStart.cshtml* и *_ViewImports.cshtml* работают точно так же, как и в стандартных представлениях Razor.</span><span class="sxs-lookup"><span data-stu-id="54017-226">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="54017-227">Давайте упростим нашу страницу с помощью некоторых из этих функций.</span><span class="sxs-lookup"><span data-stu-id="54017-227">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="54017-228">Добавим [макет страницы](xref:mvc/views/layout) в файл *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="54017-228">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="54017-229">Этот [макет](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="54017-229">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="54017-230">управляет макетом каждой страницы (кроме страниц с отказом от макета);</span><span class="sxs-lookup"><span data-stu-id="54017-230">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="54017-231">импортирует HTML-структуры, такие как JavaScript и таблицы стилей.</span><span class="sxs-lookup"><span data-stu-id="54017-231">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="54017-232">Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="54017-232">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="54017-233">Свойство [Layout](xref:mvc/views/layout#specifying-a-layout) определяется в файле *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="54017-233">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="54017-234">**Примечание.** Макет хранится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="54017-234">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="54017-235">Pages ищет другие представления (макеты, шаблоны, частичные реплики) в иерархическом порядке, начиная с той папки, где находится текущая страница.</span><span class="sxs-lookup"><span data-stu-id="54017-235">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="54017-236">Макет в папке *Pages* можно использовать на любой странице Razor, которая находится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="54017-236">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="54017-237">Корпорация Майкрософт рекомендует **не** размещать файл макета в папке *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="54017-237">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="54017-238">*Views/Shared* — это шаблон представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="54017-238">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="54017-239">Razor Pages опирается на иерархию папок, а не на условные обозначения путей.</span><span class="sxs-lookup"><span data-stu-id="54017-239">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="54017-240">Поиск представлений в Razor Pages охватывает папку *Pages*.</span><span class="sxs-lookup"><span data-stu-id="54017-240">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="54017-241">Макеты, шаблоны и частичные реплики *работают* с контроллерами MVC и стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="54017-241">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="54017-242">Добавим файл *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="54017-242">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="54017-243">`@namespace` описывается далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="54017-243">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="54017-244">Директива `@addTagHelper` добавляет [встроенные вспомогательные теги](xref:mvc/views/tag-helpers/builtin-th/Index) на все страницы в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="54017-244">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="54017-245">Явное применение директивы `@namespace` на странице:</span><span class="sxs-lookup"><span data-stu-id="54017-245">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="54017-246">Данная директива задает пространство имен для страницы.</span><span class="sxs-lookup"><span data-stu-id="54017-246">The directive sets the namespace for the page.</span></span> <span data-ttu-id="54017-247">Включать пространство имен в директиву `@model` не требуется.</span><span class="sxs-lookup"><span data-stu-id="54017-247">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="54017-248">Если директива `@namespace` содержится в файле *_ViewImports.cshtml*, указанное пространство имен определяет префикс для созданного в Pages пространства имен, куда импортируется директива `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="54017-248">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="54017-249">Остальная часть созданного пространства имен (суффикс) представляет собой разделенный точками относительный путь между папкой с файлом *_ViewImports.cshtml* и папкой, содержащей страницу.</span><span class="sxs-lookup"><span data-stu-id="54017-249">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="54017-250">Например, файл кода программной части *Pages/Customers/Edit.cshtml.cs* задает пространство имен явно:</span><span class="sxs-lookup"><span data-stu-id="54017-250">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="54017-251">Файл *Pages/_ViewImports.cshtml* задает следующее пространство имен:</span><span class="sxs-lookup"><span data-stu-id="54017-251">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="54017-252">Сформированное пространство имен для файла *Pages/Customers/Edit.cshtml* Razor Pages совпадает с именем файла кода программной части.</span><span class="sxs-lookup"><span data-stu-id="54017-252">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="54017-253">Директива `@namespace` разработана таким образом, чтобы добавляемые в проект классы C# и сгенерированный страницами код *просто работали* без добавления директивы `@using` для файла кода программной части.</span><span class="sxs-lookup"><span data-stu-id="54017-253">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="54017-254">**Примечание.** `@namespace` также работает со стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="54017-254">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="54017-255">Исходный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="54017-255">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="54017-256">Обновленный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="54017-256">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="54017-257">[Начальный проект Razor Pages](#rpvs17) содержит файл *Pages/_ValidationScriptsPartial.cshtml*, который подключает проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="54017-257">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="54017-258">Формирование URL-адресов для страниц</span><span class="sxs-lookup"><span data-stu-id="54017-258">URL generation for Pages</span></span>

<span data-ttu-id="54017-259">На представленной выше странице `Create` используется `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="54017-259">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="54017-260">Это приложение имеет следующую структуру файлов и папок:</span><span class="sxs-lookup"><span data-stu-id="54017-260">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="54017-261">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="54017-261">*/Pages*</span></span>

  * <span data-ttu-id="54017-262">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="54017-262">*Index.cshtml*</span></span>
  * <span data-ttu-id="54017-263">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="54017-263">*/Customer*</span></span>

    * <span data-ttu-id="54017-264">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="54017-264">*Create.cshtml*</span></span>
    * <span data-ttu-id="54017-265">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="54017-265">*Edit.cshtml*</span></span>
    * <span data-ttu-id="54017-266">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="54017-266">*Index.cshtml*</span></span>

<span data-ttu-id="54017-267">После успешного выполнения страницы *Pages/Customers/Create.cshtml* и *Pages/Customers/Edit.cshtml* перенаправляются на страницу *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="54017-267">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="54017-268">Строка `/Index` составляет часть URL-адреса для доступа к указанной выше странице.</span><span class="sxs-lookup"><span data-stu-id="54017-268">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="54017-269">Строка `/Index` позволяет создавать URL-адреса для страницы *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="54017-269">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="54017-270">Пример:</span><span class="sxs-lookup"><span data-stu-id="54017-270">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="54017-271">Имя страницы — это путь к странице из корневой папки */Pages* (включая начальный символ `/`, например `/Index`).</span><span class="sxs-lookup"><span data-stu-id="54017-271">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="54017-272">Приведенные выше примеры создания URL-адресов задействуют гораздо больше функций, чем просто прописывание URL в коде.</span><span class="sxs-lookup"><span data-stu-id="54017-272">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="54017-273">Формирование URL-адресов включает [маршрутизацию](xref:mvc/controllers/routing) и позволяет генерировать и включать в код параметры в зависимости от того, как определяется маршрут в пути назначения.</span><span class="sxs-lookup"><span data-stu-id="54017-273">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="54017-274">Формирование URL-адресов для страниц поддерживает относительные имена.</span><span class="sxs-lookup"><span data-stu-id="54017-274">URL generation for pages supports relative names.</span></span> <span data-ttu-id="54017-275">В приведенной ниже таблице показано, какая страница индекса выбирается с каждым параметром `RedirectToPage` в *Pages/Customers/Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="54017-275">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="54017-276">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="54017-276">RedirectToPage(x)</span></span>| <span data-ttu-id="54017-277">Страница</span><span class="sxs-lookup"><span data-stu-id="54017-277">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="54017-278">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="54017-278">RedirectToPage("/Index")</span></span> | <span data-ttu-id="54017-279">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="54017-279">*Pages/Index*</span></span> |
| <span data-ttu-id="54017-280">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="54017-280">RedirectToPage("./Index");</span></span> | <span data-ttu-id="54017-281">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="54017-281">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="54017-282">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="54017-282">RedirectToPage("../Index")</span></span> | <span data-ttu-id="54017-283">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="54017-283">*Pages/Index*</span></span> |
| <span data-ttu-id="54017-284">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="54017-284">RedirectToPage("Index")</span></span>  | <span data-ttu-id="54017-285">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="54017-285">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="54017-286">`RedirectToPage("Index")`, `RedirectToPage("./Index")` и `RedirectToPage("../Index")` — это *относительные имена*.</span><span class="sxs-lookup"><span data-stu-id="54017-286">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="54017-287">Для получения имени целевой страницы параметр `RedirectToPage` *комбинируется* с путем текущей страницы.</span><span class="sxs-lookup"><span data-stu-id="54017-287">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="54017-288">Привязка относительных имен полезна при создании сайтов со сложной структурой.</span><span class="sxs-lookup"><span data-stu-id="54017-288">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="54017-289">Если для связи между страницами в определенной папке используются относительные имена, эту папку можно переименовать.</span><span class="sxs-lookup"><span data-stu-id="54017-289">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="54017-290">При этом все ссылки останутся рабочими (так как не включают имя папки).</span><span class="sxs-lookup"><span data-stu-id="54017-290">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="54017-291">TempData</span><span class="sxs-lookup"><span data-stu-id="54017-291">TempData</span></span>

<span data-ttu-id="54017-292">ASP.NET Core позволяет использовать свойство [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) в [контроллере](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="54017-292">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="54017-293">Это свойство хранит данные до тех пор, пока они не будут прочитаны.</span><span class="sxs-lookup"><span data-stu-id="54017-293">This property stores data until it is read.</span></span> <span data-ttu-id="54017-294">Для проверки данных без удаления можно использовать методы `Keep` и `Peek`.</span><span class="sxs-lookup"><span data-stu-id="54017-294">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="54017-295">`TempData` удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса.</span><span class="sxs-lookup"><span data-stu-id="54017-295">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="54017-296">Атрибут `[TempData]` появился в ASP.NET 2.0 и поддерживается в контроллерах и на страницах.</span><span class="sxs-lookup"><span data-stu-id="54017-296">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="54017-297">В следующем коде значение `Message` задается с помощью `TempData`:</span><span class="sxs-lookup"><span data-stu-id="54017-297">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="54017-298">Представленная ниже разметка в файле *Pages/Customers/Index.cshtml* отображает значение `Message` с помощью `TempData`.</span><span class="sxs-lookup"><span data-stu-id="54017-298">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="54017-299">Файл кода программной части *Pages/Customers/Index.cshtml.cs* применяет атрибут `[TempData]` к свойству `Message`.</span><span class="sxs-lookup"><span data-stu-id="54017-299">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="54017-300">Дополнительные сведения см. в разделе [TempData](xref:fundamentals/app-state#temp).</span><span class="sxs-lookup"><span data-stu-id="54017-300">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="54017-301">Несколько обработчиков на страницу</span><span class="sxs-lookup"><span data-stu-id="54017-301">Multiple handlers per page</span></span>

<span data-ttu-id="54017-302">Следующая страница формирует разметку для двух обработчиков страницы с помощью вспомогательного тега `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="54017-302">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="54017-303">Форма в предыдущем примере включает две кнопки отправки, каждая из которых отправляет данные на отдельный URL-адрес с помощью `FormActionTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="54017-303">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="54017-304">Атрибут `asp-page-handler` является дополнением к `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="54017-304">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="54017-305">Атрибут `asp-page-handler` формирует URL-адреса, ,которые используются для отправки данных в каждый из методов обработчиков, определенных страницей.</span><span class="sxs-lookup"><span data-stu-id="54017-305">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="54017-306">`asp-page` не задается, так как пример сопоставлен с текущей страницей.</span><span class="sxs-lookup"><span data-stu-id="54017-306">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="54017-307">Файл кода программной части:</span><span class="sxs-lookup"><span data-stu-id="54017-307">The code-behind file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="54017-308">В представленном выше коде используются *именованные методы обработчика*.</span><span class="sxs-lookup"><span data-stu-id="54017-308">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="54017-309">Именованные методы обработчика создаются путем размещения определенного текста в имени после `On<HTTP Verb>` и перед `Async` (если есть).</span><span class="sxs-lookup"><span data-stu-id="54017-309">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="54017-310">В приведенном выше примере использовались методы страницы OnPost**JoinList**Async и OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="54017-310">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="54017-311">Если убрать *OnPost* и *Async*, имена обработчиков будут выглядеть как `JoinList` и `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="54017-311">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="54017-312">При использовании представленного выше кода URL-путь для отправки данных в `OnPostJoinListAsync` будет выглядеть как `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="54017-312">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="54017-313">URL-путь для отправки данных в `OnPostJoinListUCAsync` будет иметь вид `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="54017-313">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="54017-314">Настройка маршрутизации</span><span class="sxs-lookup"><span data-stu-id="54017-314">Customizing Routing</span></span>

<span data-ttu-id="54017-315">Если вы не хотите, чтобы в URL-адресе отображалась строка запроса `?handler=JoinList`, измените маршрут таким образом, чтобы в качестве пути в URL-адресе указывалось имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="54017-315">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="54017-316">Для настройки маршрута можно добавить после директивы `@page` шаблон маршрута, заключенные в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="54017-316">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="54017-317">В представленном выше маршруте вместо строки запроса URL-путь содержит имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="54017-317">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="54017-318">Символ `?` после `handler` означает, что параметр маршрута является необязательным.</span><span class="sxs-lookup"><span data-stu-id="54017-318">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="54017-319">С помощью директивы `@page` можно добавить к маршруту страницы дополнительные сегменты и параметры.</span><span class="sxs-lookup"><span data-stu-id="54017-319">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="54017-320">Все, что вы добавите, будет **присоединено** к маршруту страницы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="54017-320">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="54017-321">Использовать для изменения маршрута страницы абсолютный или виртуальный путь (например, вида `"~/Some/Other/Path"`) нельзя.</span><span class="sxs-lookup"><span data-stu-id="54017-321">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="54017-322">Конфигурация и параметры</span><span class="sxs-lookup"><span data-stu-id="54017-322">Configuration and settings</span></span>

<span data-ttu-id="54017-323">Чтобы настроить дополнительные параметры, воспользуйтесь методом расширения `AddRazorPagesOptions` в построителе MVC:</span><span class="sxs-lookup"><span data-stu-id="54017-323">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="54017-324">В настоящее время `RazorPagesOptions` позволяет задавать корневую папку для страниц и добавлять для них обозначения моделей приложений.</span><span class="sxs-lookup"><span data-stu-id="54017-324">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="54017-325">В дальнейшем мы расширим спектр его возможностей.</span><span class="sxs-lookup"><span data-stu-id="54017-325">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="54017-326">Сведения о предварительной компиляции представлений см. в статье [Компиляция представлений Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="54017-326">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="54017-327">[Загрузить или просмотреть пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="54017-327">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="54017-328">Представленные общие сведения являются вводными для статьи [Начало работы с Razor Pages в ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="54017-328">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="54017-329">Указание местонахождения Razor Pages в корне каталога</span><span class="sxs-lookup"><span data-stu-id="54017-329">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="54017-330">По умолчанию Razor Pages находится в корне каталога */Pages*.</span><span class="sxs-lookup"><span data-stu-id="54017-330">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="54017-331">Добавьте [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в корне содержимого Razor Pages ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) приложения:</span><span class="sxs-lookup"><span data-stu-id="54017-331">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="54017-332">Указание местонахождения Razor Pages в пользовательском корневом каталоге</span><span class="sxs-lookup"><span data-stu-id="54017-332">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="54017-333">Добавьте [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в пользовательском корневом каталоге в приложении (укажите относительный путь):</span><span class="sxs-lookup"><span data-stu-id="54017-333">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="54017-334">См. также</span><span class="sxs-lookup"><span data-stu-id="54017-334">See also</span></span>

* [<span data-ttu-id="54017-335">Начало работы с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="54017-335">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="54017-336">Соглашения об авторизации Razor Pages</span><span class="sxs-lookup"><span data-stu-id="54017-336">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="54017-337">Пользовательские поставщики моделей маршрутов и страниц Razor Pages</span><span class="sxs-lookup"><span data-stu-id="54017-337">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="54017-338">Модульное тестирование и тестирование интеграции для Razor Pages</span><span class="sxs-lookup"><span data-stu-id="54017-338">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
