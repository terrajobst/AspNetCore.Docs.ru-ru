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
ms.openlocfilehash: 72ab979c6c718544955ae5734903ec936fc5afbc
ms.sourcegitcommit: 195b2b331434f74334c5c5b7dfeba62d744a1e38
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/15/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="08039-104">Введение в Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08039-104">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="08039-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Райан Новак](https://github.com/rynowak) (Ryan Nowak)</span><span class="sxs-lookup"><span data-stu-id="08039-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="08039-106">Razor Pages — это новая функция платформы MVC ASP.NET Core, которая делает создание кодов сценариев для страниц проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="08039-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="08039-107">Если вам требуется руководство на основе подхода "модель-представление-контроллер", см. статью [Начало работы с MVC ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="08039-107">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="08039-108">Предварительные требования ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="08039-108">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="08039-109">Установите [.NET Core](https://www.microsoft.com/net/core) 2.0.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="08039-109">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="08039-110">Если вы используете Visual Studio, установите [Visual Studio](https://www.visualstudio.com/vs/) 2017 версии 15.3 или более поздней версии, а также следующие рабочие нагрузки:</span><span class="sxs-lookup"><span data-stu-id="08039-110">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="08039-111">**ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="08039-111">**ASP.NET and web development**</span></span>
* <span data-ttu-id="08039-112">**Кроссплатформенная разработка .NET Core**</span><span class="sxs-lookup"><span data-stu-id="08039-112">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="08039-113">Создание проекта Razor Pages</span><span class="sxs-lookup"><span data-stu-id="08039-113">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08039-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08039-114">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="08039-115">Подробные инструкции по созданию проекта Razor Pages с помощью Visual Studio см. в статье [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="08039-115">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08039-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08039-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="08039-117">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="08039-117">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="08039-118">Откройте созданный файл *.csproj* в Visual Studio для Mac.</span><span class="sxs-lookup"><span data-stu-id="08039-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08039-119">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="08039-119">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="08039-120">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="08039-120">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="08039-121">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="08039-121">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="08039-122">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="08039-122">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="08039-123">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="08039-123">Razor Pages</span></span>

<span data-ttu-id="08039-124">Функция Razor Pages активируется в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="08039-124">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="08039-125">Рассмотрим простую страницу: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="08039-125">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="08039-126">Представленный выше код выглядит как файл представления Razor</span><span class="sxs-lookup"><span data-stu-id="08039-126">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="08039-127">и отличается от него только директивой `@page`.</span><span class="sxs-lookup"><span data-stu-id="08039-127">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="08039-128">Директива `@page` превращает файл в действие MVC, а значит обрабатывает запросы напрямую, минуя контроллер.</span><span class="sxs-lookup"><span data-stu-id="08039-128">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="08039-129">Директива `@page` должна быть первой директивой Razor на странице.</span><span class="sxs-lookup"><span data-stu-id="08039-129">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="08039-130">Директива `@page` влияет на поведение всех остальных конструкций Razor.</span><span class="sxs-lookup"><span data-stu-id="08039-130">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="08039-131">Похожая страница с использованием класса `PageModel` показана в следующих двух файлах.</span><span class="sxs-lookup"><span data-stu-id="08039-131">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="08039-132">Файл *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="08039-132">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="08039-133">Файл кода "программной части" *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="08039-133">The *Pages/Index2.cshtml.cs* "code-behind" file:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="08039-134">Как правило, файл класса `PageModel` называется так же, как файл Razor Pages, но с расширением *.cs*.</span><span class="sxs-lookup"><span data-stu-id="08039-134">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="08039-135">Например, представленная выше страница Razor Pages называется *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="08039-135">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="08039-136">Файл, содержащий класс `PageModel`, называется *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="08039-136">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="08039-137">Сопоставления URL-адресов со страницами определяются расположением конкретной страницы в файловой системе.</span><span class="sxs-lookup"><span data-stu-id="08039-137">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="08039-138">В приведенной ниже таблице показаны пути Razor Pages и соответствующие URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="08039-138">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="08039-139">Имя файла и путь</span><span class="sxs-lookup"><span data-stu-id="08039-139">File name and path</span></span>               | <span data-ttu-id="08039-140">Соответствующий URL</span><span class="sxs-lookup"><span data-stu-id="08039-140">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="08039-141">*/Pages/index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="08039-141">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="08039-142">`/` или `/Index`</span><span class="sxs-lookup"><span data-stu-id="08039-142">`/` or `/Index`</span></span> |
| <span data-ttu-id="08039-143">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="08039-143">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="08039-144">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="08039-144">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="08039-145">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="08039-145">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="08039-146">`/Store` или `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="08039-146">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="08039-147">Примечания.</span><span class="sxs-lookup"><span data-stu-id="08039-147">Notes:</span></span>

* <span data-ttu-id="08039-148">Среда выполнения по умолчанию ищет файлы Razor Pages в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="08039-148">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="08039-149">Если в URL-адресе не указана конкретная страница, по умолчанию открывается страница `Index`.</span><span class="sxs-lookup"><span data-stu-id="08039-149">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="08039-150">Создание простой формы</span><span class="sxs-lookup"><span data-stu-id="08039-150">Writing a basic form</span></span>

<span data-ttu-id="08039-151">Функция Razor Pages предназначена для упрощения типовых шаблонов, которые используются в браузерах.</span><span class="sxs-lookup"><span data-stu-id="08039-151">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="08039-152">[Привязки модели](xref:mvc/models/model-binding), [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и вспомогательные методы HTML *отлично работают* со свойствами, определенными в классе Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="08039-152">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="08039-153">Рассмотрим страницу с простой формой связи для модели `Contact`.</span><span class="sxs-lookup"><span data-stu-id="08039-153">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="08039-154">В представленных в этой статье примерах `DbContext` инициализируется в файле [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="08039-154">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="08039-155">Модель данных:</span><span class="sxs-lookup"><span data-stu-id="08039-155">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="08039-156">Файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="08039-156">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="08039-157">Файл кода программной части *Pages/Create.cshtml.cs* для представления:</span><span class="sxs-lookup"><span data-stu-id="08039-157">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="08039-158">Как правило, класс `PageModel` называется `<PageName>Model` и находится в том же пространстве имен, что и страница.</span><span class="sxs-lookup"><span data-stu-id="08039-158">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="08039-159">Применение файла кода программной части `PageModel` позволяет выполнять модульное тестирование, но требует написания явного конструктора и класса.</span><span class="sxs-lookup"><span data-stu-id="08039-159">Using a `PageModel` code-behind file supports unit testing, but requires you to write an explicit constructor and class.</span></span> <span data-ttu-id="08039-160">Страницы без файлов кода программной части `PageModel` поддерживают компиляцию среды выполнения, что может пригодиться в процессе разработки.</span><span class="sxs-lookup"><span data-stu-id="08039-160">Pages without `PageModel` code-behind files support runtime compilation, which can be an advantage in development.</span></span>  <!-- review: advantage because you can make changes and refresh the browser without explicitly compiling the app -->

<span data-ttu-id="08039-161">Страница содержит *метод обработчика* `OnPostAsync`, который выполняется по запросам `POST` (когда пользователь публикует форму).</span><span class="sxs-lookup"><span data-stu-id="08039-161">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="08039-162">Методы обработчика можно добавить для любой HTTP-команды.</span><span class="sxs-lookup"><span data-stu-id="08039-162">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="08039-163">Наиболее распространенные обработчики</span><span class="sxs-lookup"><span data-stu-id="08039-163">The most common handlers are:</span></span>

* <span data-ttu-id="08039-164">`OnGet` — инициализация необходимого для страницы состояния.</span><span class="sxs-lookup"><span data-stu-id="08039-164">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="08039-165">Пример обработчика [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="08039-165">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="08039-166">`OnPost` — обработка отправленных через форму данных.</span><span class="sxs-lookup"><span data-stu-id="08039-166">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="08039-167">Суффикс `Async` не является обязательным, но часто используется для асинхронных функций.</span><span class="sxs-lookup"><span data-stu-id="08039-167">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="08039-168">Код `OnPostAsync` в приведенном выше примере похож на то, что обычно пишут в контроллере.</span><span class="sxs-lookup"><span data-stu-id="08039-168">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="08039-169">Этот код типичен для Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="08039-169">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="08039-170">Большинство примитивов MVC, включая [привязку модели](xref:mvc/models/model-binding), [проверку](xref:mvc/models/validation) и результаты действий, являются общими.</span><span class="sxs-lookup"><span data-stu-id="08039-170">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="08039-171">Предыдущий метод `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="08039-171">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="08039-172">Простая схема `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="08039-172">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="08039-173">Проверка на наличие ошибок проверки.</span><span class="sxs-lookup"><span data-stu-id="08039-173">Check for validation errors.</span></span>

*  <span data-ttu-id="08039-174">Если ошибок нет, сохранение данных и перенаправление.</span><span class="sxs-lookup"><span data-stu-id="08039-174">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="08039-175">Если есть ошибки, отображение страницы с сообщениями проверки.</span><span class="sxs-lookup"><span data-stu-id="08039-175">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="08039-176">Проверка на стороне клиента выполняется точно так же, как в традиционных приложениях MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08039-176">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="08039-177">Во многих случаях ошибки проверки выявляются на клиентском компьютере и на сервер не отправляются.</span><span class="sxs-lookup"><span data-stu-id="08039-177">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="08039-178">После успешного ввода данных метод обработчика `OnPostAsync` вызывает метод обработчика `RedirectToPage`, чтобы получить экземпляр `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="08039-178">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="08039-179">`RedirectToPage` — это новый результат действия, аналогичный `RedirectToAction` или `RedirectToRoute`, но настроенный для страниц.</span><span class="sxs-lookup"><span data-stu-id="08039-179">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="08039-180">В приведенном выше примере он выполняет перенаправление на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="08039-180">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="08039-181">Более подробно `RedirectToPage` рассматривается в разделе [Создание URL для страниц](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="08039-181">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="08039-182">Если в отправленной форме имеются ошибки проверки (переданные на сервер), метод обработчика `OnPostAsync` вызывает метод обработчика `Page`.</span><span class="sxs-lookup"><span data-stu-id="08039-182">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="08039-183">`Page` возвращает экземпляр `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="08039-183">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="08039-184">Возвращение `Page` аналогично тому, как действия в контроллерах возвращают `View`.</span><span class="sxs-lookup"><span data-stu-id="08039-184">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="08039-185">`PageResult` — значение типа возвращаемого значения <!-- Review  --> по умолчанию для метода обработчика.</span><span class="sxs-lookup"><span data-stu-id="08039-185">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="08039-186">Метод обработчика, вернувший `void`, визуализирует страницу.</span><span class="sxs-lookup"><span data-stu-id="08039-186">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="08039-187">Для указания согласия на привязку модели в свойстве `Customer` используется атрибут `[BindProperty]`.</span><span class="sxs-lookup"><span data-stu-id="08039-187">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="08039-188">По умолчанию функция Razor Pages привязывает свойства ко всем командам, кроме GET.</span><span class="sxs-lookup"><span data-stu-id="08039-188">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="08039-189">Привязка к свойствам позволяет сократить объем необходимого кода.</span><span class="sxs-lookup"><span data-stu-id="08039-189">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="08039-190">Привязка уменьшает код за счет того, что для визуализации полей формы (`<input asp-for="Customer.Name" />`) и получения входных данных используется одно и то же свойство.</span><span class="sxs-lookup"><span data-stu-id="08039-190">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="08039-191">Домашняя страница (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="08039-191">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="08039-192">Файл кода программной части *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="08039-192">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="08039-193">Файл *Index.cshtml* содержит следующую разметку для создания ссылки на правку для каждого контактного лица.</span><span class="sxs-lookup"><span data-stu-id="08039-193">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

```cshtml
<a asp-page="./Edit" asp-route-id="@contact.Id">edit</a>
```

<span data-ttu-id="08039-194">[Вспомогательный тег привязки](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) использует атрибут [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper#route) для создания ссылки на страницу редактирования.</span><span class="sxs-lookup"><span data-stu-id="08039-194">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) used the [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper#route) attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="08039-195">Эта ссылка содержит данные о маршруте с идентификатором контактного лица.</span><span class="sxs-lookup"><span data-stu-id="08039-195">The link contains route data with the contact ID.</span></span> <span data-ttu-id="08039-196">Например, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="08039-196">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="08039-197">Файл *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="08039-197">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="08039-198">Первая строка содержит директиву `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="08039-198">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="08039-199">Ограничение маршрутизации `"{id:int}"` указывает, что страница должна принимать обращенные к ней запросы, которые содержат данные о маршруте `int`.</span><span class="sxs-lookup"><span data-stu-id="08039-199">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="08039-200">Если запрос к странице не содержит данные о маршруте, которые можно конвертировать в `int`, среда выполнения возвращает ошибку HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="08039-200">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="08039-201">Файл *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="08039-201">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="08039-202">XSRF/CSRF и Razor Pages</span><span class="sxs-lookup"><span data-stu-id="08039-202">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="08039-203">Вам не придется писать отдельный код для [проверки подлинности запросов](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="08039-203">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="08039-204">Razor Pages включает создание и проверку маркеров защиты от подделок по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="08039-204">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="08039-205">Использование макетов, частичных реплик, шаблонов и вспомогательных функций тегов с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="08039-205">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="08039-206">Pages работает со всеми функциями представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="08039-206">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="08039-207">Макеты, частичные реплики, шаблоны, вспомогательные функции тегов, а также файлы *_ViewStart.cshtml* и *_ViewImports.cshtml* работают точно так же, как и в стандартных представлениях Razor.</span><span class="sxs-lookup"><span data-stu-id="08039-207">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="08039-208">Давайте упростим нашу страницу с помощью некоторых из этих функций.</span><span class="sxs-lookup"><span data-stu-id="08039-208">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="08039-209">Добавим [макет страницы](xref:mvc/views/layout) в файл *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="08039-209">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="08039-210">Этот [макет](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="08039-210">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="08039-211">управляет макетом каждой страницы (кроме страниц с отказом от макета);</span><span class="sxs-lookup"><span data-stu-id="08039-211">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="08039-212">импортирует HTML-структуры, такие как JavaScript и таблицы стилей.</span><span class="sxs-lookup"><span data-stu-id="08039-212">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="08039-213">Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="08039-213">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="08039-214">Свойство [Layout](xref:mvc/views/layout#specifying-a-layout) определяется в файле *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="08039-214">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="08039-215">**Примечание.** Макет хранится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="08039-215">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="08039-216">Pages ищет другие представления (макеты, шаблоны, частичные реплики) в иерархическом порядке, начиная с той папки, где находится текущая страница.</span><span class="sxs-lookup"><span data-stu-id="08039-216">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="08039-217">Макет в папке *Pages* можно использовать на любой странице Razor, которая находится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="08039-217">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="08039-218">Корпорация Майкрософт рекомендует **не** размещать файл макета в папке *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="08039-218">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="08039-219">*Views/Shared* — это шаблон представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="08039-219">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="08039-220">Razor Pages опирается на иерархию папок, а не на условные обозначения путей.</span><span class="sxs-lookup"><span data-stu-id="08039-220">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="08039-221">Поиск представлений в Razor Pages охватывает папку *Pages*.</span><span class="sxs-lookup"><span data-stu-id="08039-221">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="08039-222">Макеты, шаблоны и частичные реплики *работают* с контроллерами MVC и стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="08039-222">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="08039-223">Добавим файл *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="08039-223">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="08039-224">`@namespace` описывается далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="08039-224">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="08039-225">Директива `@addTagHelper` добавляет [встроенные вспомогательные теги](xref:mvc/views/tag-helpers/builtin-th/Index) на все страницы в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="08039-225">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="08039-226">Явное применение директивы `@namespace` на странице:</span><span class="sxs-lookup"><span data-stu-id="08039-226">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="08039-227">Данная директива задает пространство имен для страницы.</span><span class="sxs-lookup"><span data-stu-id="08039-227">The directive sets the namespace for the page.</span></span> <span data-ttu-id="08039-228">Включать пространство имен в директиву `@model` не требуется.</span><span class="sxs-lookup"><span data-stu-id="08039-228">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="08039-229">Если директива `@namespace` содержится в файле *_ViewImports.cshtml*, указанное пространство имен определяет префикс для созданного в Pages пространства имен, куда импортируется директива `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="08039-229">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="08039-230">Остальная часть созданного пространства имен (суффикс) представляет собой разделенный точками относительный путь между папкой с файлом *_ViewImports.cshtml* и папкой, содержащей страницу.</span><span class="sxs-lookup"><span data-stu-id="08039-230">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="08039-231">Например, файл кода программной части *Pages/Customers/Edit.cshtml.cs* задает пространство имен явно:</span><span class="sxs-lookup"><span data-stu-id="08039-231">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="08039-232">Файл *Pages/_ViewImports.cshtml* задает следующее пространство имен:</span><span class="sxs-lookup"><span data-stu-id="08039-232">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="08039-233">Сформированное пространство имен для файла *Pages/Customers/Edit.cshtml* Razor Pages совпадает с именем файла кода программной части.</span><span class="sxs-lookup"><span data-stu-id="08039-233">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="08039-234">Директива `@namespace` разработана таким образом, чтобы добавляемые в проект классы C# и сгенерированный страницами код *просто работали* без добавления директивы `@using` для файла кода программной части.</span><span class="sxs-lookup"><span data-stu-id="08039-234">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="08039-235">**Примечание.** `@namespace` также работает со стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="08039-235">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="08039-236">Исходный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="08039-236">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="08039-237">Обновленный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="08039-237">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="08039-238">[Начальный проект Razor Pages](#rpvs17) содержит файл *Pages/_ValidationScriptsPartial.cshtml*, который подключает проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="08039-238">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="08039-239">Формирование URL-адресов для страниц</span><span class="sxs-lookup"><span data-stu-id="08039-239">URL generation for Pages</span></span>

<span data-ttu-id="08039-240">На представленной выше странице `Create` используется `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="08039-240">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="08039-241">Это приложение имеет следующую структуру файлов и папок:</span><span class="sxs-lookup"><span data-stu-id="08039-241">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="08039-242">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="08039-242">*/Pages*</span></span>

  * <span data-ttu-id="08039-243">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="08039-243">*Index.cshtml*</span></span>
  * <span data-ttu-id="08039-244">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="08039-244">*/Customer*</span></span>

    * <span data-ttu-id="08039-245">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="08039-245">*Create.cshtml*</span></span>
    * <span data-ttu-id="08039-246">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="08039-246">*Edit.cshtml*</span></span>
    * <span data-ttu-id="08039-247">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="08039-247">*Index.cshtml*</span></span>

<span data-ttu-id="08039-248">После успешного выполнения страницы *Pages/Customers/Create.cshtml* и *Pages/Customers/Edit.cshtml* перенаправляются на страницу *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="08039-248">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="08039-249">Строка `/Index` составляет часть URL-адреса для доступа к указанной выше странице.</span><span class="sxs-lookup"><span data-stu-id="08039-249">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="08039-250">Строка `/Index` позволяет создавать URL-адреса для страницы *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="08039-250">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="08039-251">Пример:</span><span class="sxs-lookup"><span data-stu-id="08039-251">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="08039-252">Имя страницы — это путь к странице из корневой папки */Pages* (включая начальный символ `/`, например `/Index`).</span><span class="sxs-lookup"><span data-stu-id="08039-252">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="08039-253">Приведенные выше примеры создания URL-адресов задействуют гораздо больше функций, чем просто прописывание URL в коде.</span><span class="sxs-lookup"><span data-stu-id="08039-253">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="08039-254">Формирование URL-адресов включает [маршрутизацию](xref:mvc/controllers/routing) и позволяет генерировать и включать в код параметры в зависимости от того, как определяется маршрут в пути назначения.</span><span class="sxs-lookup"><span data-stu-id="08039-254">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="08039-255">Формирование URL-адресов для страниц поддерживает относительные имена.</span><span class="sxs-lookup"><span data-stu-id="08039-255">URL generation for pages supports relative names.</span></span> <span data-ttu-id="08039-256">В приведенной ниже таблице показано, какая страница индекса выбирается с каждым параметром `RedirectToPage` в *Pages/Customers/Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="08039-256">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="08039-257">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="08039-257">RedirectToPage(x)</span></span>| <span data-ttu-id="08039-258">Страница</span><span class="sxs-lookup"><span data-stu-id="08039-258">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="08039-259">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="08039-259">RedirectToPage("/Index")</span></span> | <span data-ttu-id="08039-260">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="08039-260">*Pages/Index*</span></span> |
| <span data-ttu-id="08039-261">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="08039-261">RedirectToPage("./Index");</span></span> | <span data-ttu-id="08039-262">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="08039-262">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="08039-263">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="08039-263">RedirectToPage("../Index")</span></span> | <span data-ttu-id="08039-264">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="08039-264">*Pages/Index*</span></span> |
| <span data-ttu-id="08039-265">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="08039-265">RedirectToPage("Index")</span></span>  | <span data-ttu-id="08039-266">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="08039-266">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="08039-267">`RedirectToPage("Index")`, `RedirectToPage("./Index")` и `RedirectToPage("../Index")` — это *относительные имена*.</span><span class="sxs-lookup"><span data-stu-id="08039-267">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="08039-268">Для получения имени целевой страницы параметр `RedirectToPage` *комбинируется* с путем текущей страницы.</span><span class="sxs-lookup"><span data-stu-id="08039-268">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="08039-269">Привязка относительных имен полезна при создании сайтов со сложной структурой.</span><span class="sxs-lookup"><span data-stu-id="08039-269">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="08039-270">Если для связи между страницами в определенной папке используются относительные имена, эту папку можно переименовать.</span><span class="sxs-lookup"><span data-stu-id="08039-270">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="08039-271">При этом все ссылки останутся рабочими (так как не включают имя папки).</span><span class="sxs-lookup"><span data-stu-id="08039-271">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="08039-272">TempData</span><span class="sxs-lookup"><span data-stu-id="08039-272">TempData</span></span>

<span data-ttu-id="08039-273">ASP.NET Core позволяет использовать свойство [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) в [контроллере](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="08039-273">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="08039-274">Это свойство хранит данные до тех пор, пока они не будут прочитаны.</span><span class="sxs-lookup"><span data-stu-id="08039-274">This property stores data until it is read.</span></span> <span data-ttu-id="08039-275">Для проверки данных без удаления можно использовать методы `Keep` и `Peek`.</span><span class="sxs-lookup"><span data-stu-id="08039-275">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="08039-276">`TempData` удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса.</span><span class="sxs-lookup"><span data-stu-id="08039-276">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="08039-277">Атрибут `[TempData]` появился в ASP.NET 2.0 и поддерживается в контроллерах и на страницах.</span><span class="sxs-lookup"><span data-stu-id="08039-277">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="08039-278">В следующем коде значение `Message` задается с помощью `TempData`:</span><span class="sxs-lookup"><span data-stu-id="08039-278">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="08039-279">Представленная ниже разметка в файле *Pages/Customers/Index.cshtml* отображает значение `Message` с помощью `TempData`.</span><span class="sxs-lookup"><span data-stu-id="08039-279">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="08039-280">Файл кода программной части *Pages/Customers/Index.cshtml.cs* применяет атрибут `[TempData]` к свойству `Message`.</span><span class="sxs-lookup"><span data-stu-id="08039-280">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="08039-281">Дополнительные сведения см. в разделе [TempData](xref:fundamentals/app-state#temp).</span><span class="sxs-lookup"><span data-stu-id="08039-281">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="08039-282">Несколько обработчиков на страницу</span><span class="sxs-lookup"><span data-stu-id="08039-282">Multiple handlers per page</span></span>

<span data-ttu-id="08039-283">Следующая страница формирует разметку для двух обработчиков страницы с помощью вспомогательного тега `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="08039-283">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="08039-284">Форма в предыдущем примере включает две кнопки отправки, каждая из которых отправляет данные на отдельный URL-адрес с помощью `FormActionTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="08039-284">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="08039-285">Атрибут `asp-page-handler` является дополнением к `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="08039-285">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="08039-286">Атрибут `asp-page-handler` формирует URL-адреса, ,которые используются для отправки данных в каждый из методов обработчиков, определенных страницей.</span><span class="sxs-lookup"><span data-stu-id="08039-286">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="08039-287">`asp-page` не задается, так как пример сопоставлен с текущей страницей.</span><span class="sxs-lookup"><span data-stu-id="08039-287">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="08039-288">Файл кода программной части:</span><span class="sxs-lookup"><span data-stu-id="08039-288">The code-behind file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="08039-289">В представленном выше коде используются *именованные методы обработчика*.</span><span class="sxs-lookup"><span data-stu-id="08039-289">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="08039-290">Именованные методы обработчика создаются путем размещения определенного текста в имени после `On<HTTP Verb>` и перед `Async` (если есть).</span><span class="sxs-lookup"><span data-stu-id="08039-290">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="08039-291">В приведенном выше примере использовались методы страницы OnPost**JoinList**Async и OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="08039-291">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="08039-292">Если убрать *OnPost* и *Async*, имена обработчиков будут выглядеть как `JoinList` и `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="08039-292">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="08039-293">При использовании представленного выше кода URL-путь для отправки данных в `OnPostJoinListAsync` будет выглядеть как `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="08039-293">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="08039-294">URL-путь для отправки данных в `OnPostJoinListUCAsync` будет иметь вид `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="08039-294">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="08039-295">Настройка маршрутизации</span><span class="sxs-lookup"><span data-stu-id="08039-295">Customizing Routing</span></span>

<span data-ttu-id="08039-296">Если вы не хотите, чтобы в URL-адресе отображалась строка запроса `?handler=JoinList`, измените маршрут таким образом, чтобы в качестве пути в URL-адресе указывалось имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="08039-296">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="08039-297">Для настройки маршрута можно добавить после директивы `@page` шаблон маршрута, заключенные в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="08039-297">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="08039-298">В представленном выше маршруте вместо строки запроса URL-путь содержит имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="08039-298">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="08039-299">Символ `?` после `handler` означает, что параметр маршрута является необязательным.</span><span class="sxs-lookup"><span data-stu-id="08039-299">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="08039-300">С помощью директивы `@page` можно добавить к маршруту страницы дополнительные сегменты и параметры.</span><span class="sxs-lookup"><span data-stu-id="08039-300">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="08039-301">Все, что вы добавите, будет **присоединено** к маршруту страницы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="08039-301">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="08039-302">Использовать для изменения маршрута страницы абсолютный или виртуальный путь (например, вида `"~/Some/Other/Path"`) нельзя.</span><span class="sxs-lookup"><span data-stu-id="08039-302">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="08039-303">Конфигурация и параметры</span><span class="sxs-lookup"><span data-stu-id="08039-303">Configuration and settings</span></span>

<span data-ttu-id="08039-304">Чтобы настроить дополнительные параметры, воспользуйтесь методом расширения `AddRazorPagesOptions` в построителе MVC:</span><span class="sxs-lookup"><span data-stu-id="08039-304">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="08039-305">В настоящее время `RazorPagesOptions` позволяет задавать корневую папку для страниц и добавлять для них обозначения моделей приложений.</span><span class="sxs-lookup"><span data-stu-id="08039-305">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="08039-306">В дальнейшем мы планируем расширить спектр его возможностей.</span><span class="sxs-lookup"><span data-stu-id="08039-306">We hope to enable more extensibility this way in the future.</span></span>

<span data-ttu-id="08039-307">Сведения о предварительной компиляции представлений см. в статье [Компиляция представлений Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="08039-307">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="08039-308">[Загрузить или просмотреть пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="08039-308">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="08039-309">Представленные общие сведения являются вводными для статьи [Начало работы с Razor Pages в ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="08039-309">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>
