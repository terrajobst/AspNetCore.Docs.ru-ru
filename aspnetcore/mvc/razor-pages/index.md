---
title: "Введение в Razor Pages в ASP.NET Core"
author: Rick-Anderson
description: "Общие сведения о Razor Pages в ASP.NET Core"
keywords: ASP.NET Core, Razor Pages
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: 543399d99af127f943f7e9119fb5d84c8c5bc499
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="d4d50-104">Введение в Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4d50-104">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="d4d50-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Райан Новак](https://github.com/rynowak) (Ryan Nowak)</span><span class="sxs-lookup"><span data-stu-id="d4d50-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="d4d50-106">Razor Pages — это новая функция платформы MVC ASP.NET Core, которая делает создание кодов сценариев для страниц проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="d4d50-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="d4d50-107">Если вам требуется руководство на основе подхода "модель-представление-контроллер", см. статью [Начало работы с MVC ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="d4d50-107">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="d4d50-108">Предварительные требования ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="d4d50-108">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="d4d50-109">Установите [.NET Core](https://www.microsoft.com/net/core) 2.0.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="d4d50-109">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="d4d50-110">Если вы используете Visual Studio, установите [Visual Studio](https://www.visualstudio.com/vs/) 15.3 или более поздней версии, а также следующие рабочие нагрузки.</span><span class="sxs-lookup"><span data-stu-id="d4d50-110">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="d4d50-111">**ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="d4d50-111">**ASP.NET and web development**</span></span>
* <span data-ttu-id="d4d50-112">**Кроссплатформенная разработка .NET Core**</span><span class="sxs-lookup"><span data-stu-id="d4d50-112">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="d4d50-113">Создание проекта Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d4d50-113">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d4d50-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4d50-114">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="d4d50-115">Подробные инструкции по созданию проекта Razor Pages с помощью Visual Studio см. в статье [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="d4d50-115">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d4d50-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="d4d50-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d4d50-117">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-117">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="d4d50-118">Откройте созданный файл *.csproj* в Visual Studio для Mac.</span><span class="sxs-lookup"><span data-stu-id="d4d50-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d4d50-119">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d4d50-119">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="d4d50-120">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-120">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d4d50-121">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d4d50-121">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="d4d50-122">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-122">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="d4d50-123">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d4d50-123">Razor Pages</span></span>

<span data-ttu-id="d4d50-124">Функция Razor Pages активируется в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d4d50-124">Razor Pages is enabled in *Startup.cs*:</span></span>

<span data-ttu-id="d4d50-125">[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=Startup)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-125">[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=Startup)]</span></span>

<span data-ttu-id="d4d50-126">Рассмотрим простую страницу: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="d4d50-126">Consider a basic page: <a name="OnGet"></a></span></span>

<span data-ttu-id="d4d50-127">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-127">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]</span></span>

<span data-ttu-id="d4d50-128">Представленный выше код выглядит как файл представления Razor</span><span class="sxs-lookup"><span data-stu-id="d4d50-128">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="d4d50-129">и отличается от него только директивой `@page`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="d4d50-130">Директива `@page` превращает файл в действие MVC, а значит обрабатывает запросы напрямую, минуя контроллер.</span><span class="sxs-lookup"><span data-stu-id="d4d50-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="d4d50-131">Директива `@page` должна быть первой директивой Razor на странице.</span><span class="sxs-lookup"><span data-stu-id="d4d50-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="d4d50-132">Директива `@page` влияет на поведение всех остальных конструкций Razor.</span><span class="sxs-lookup"><span data-stu-id="d4d50-132">`@page` affects the behavior of other Razor constructs.</span></span> <span data-ttu-id="d4d50-133">Директива [@functions](xref:mvc/views/razor#functions) активирует содержимое на уровне функций.</span><span class="sxs-lookup"><span data-stu-id="d4d50-133">The [@functions](xref:mvc/views/razor#functions) directive enables function-level content.</span></span>

<span data-ttu-id="d4d50-134">В следующих двух файлах показана аналогичная страница с `PageModel` в отдельном файле.</span><span class="sxs-lookup"><span data-stu-id="d4d50-134">A similar page, with the `PageModel` in a separate file, is shown in the following two files.</span></span> <span data-ttu-id="d4d50-135">Файл *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d4d50-135">The *Pages/Index2.cshtml* file:</span></span>

<span data-ttu-id="d4d50-136">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-136">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]</span></span>

<span data-ttu-id="d4d50-137">Файл кода программной части *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d4d50-137">The *Pages/Index2.cshtml.cs* 'code-behind' file:</span></span>

<span data-ttu-id="d4d50-138">[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-138">[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]</span></span>

<span data-ttu-id="d4d50-139">Как правило, файл класса `PageModel` называется так же, как файл Razor Pages, но с расширением *.cs*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-139">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="d4d50-140">Например, представленная выше страница Razor Pages называется *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-140">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="d4d50-141">Файл, содержащий класс `PageModel`, называется *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-141">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="d4d50-142">Для простых страниц сочетание класса `PageModel` с разметкой Razor работает нормально.</span><span class="sxs-lookup"><span data-stu-id="d4d50-142">For simple pages, mixing the `PageModel` class with the Razor markup is fine.</span></span> <span data-ttu-id="d4d50-143">Для более сложного кода рекомендуется хранить код модели страницы отдельно.</span><span class="sxs-lookup"><span data-stu-id="d4d50-143">For more complex code, it's a best practice to keep the page model code separate.</span></span>

<span data-ttu-id="d4d50-144">Сопоставления URL-адресов со страницами определяются расположением конкретной страницы в файловой системе.</span><span class="sxs-lookup"><span data-stu-id="d4d50-144">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="d4d50-145">В приведенной ниже таблице показаны пути Razor Pages и соответствующие URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="d4d50-145">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="d4d50-146">Имя файла и путь</span><span class="sxs-lookup"><span data-stu-id="d4d50-146">File name and path</span></span>               | <span data-ttu-id="d4d50-147">Соответствующий URL</span><span class="sxs-lookup"><span data-stu-id="d4d50-147">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="d4d50-148">*/Pages/index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d4d50-148">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="d4d50-149">`/` или `/Index`</span><span class="sxs-lookup"><span data-stu-id="d4d50-149">`/` or `/Index`</span></span> |
| <span data-ttu-id="d4d50-150">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d4d50-150">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="d4d50-151">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d4d50-151">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="d4d50-152">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d4d50-152">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="d4d50-153">`/Store` или `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="d4d50-153">`/Store` or `/Store/Index`</span></span>  |

<span data-ttu-id="d4d50-154">Примечания.</span><span class="sxs-lookup"><span data-stu-id="d4d50-154">Notes:</span></span>

* <span data-ttu-id="d4d50-155">Среда выполнения по умолчанию ищет файлы Razor Pages в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-155">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="d4d50-156">Если в URL-адресе не указана конкретная страница, по умолчанию открывается страница `Index`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-156">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="d4d50-157">Создание простой формы</span><span class="sxs-lookup"><span data-stu-id="d4d50-157">Writing a basic form</span></span>

<span data-ttu-id="d4d50-158">Функция Razor Pages предназначена для упрощения типовых шаблонов, которые используются в браузерах.</span><span class="sxs-lookup"><span data-stu-id="d4d50-158">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="d4d50-159">[Привязки модели](xref:mvc/models/model-binding), [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и вспомогательные методы HTML *отлично работают* со свойствами, определенными в классе Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d4d50-159">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="d4d50-160">Рассмотрим страницу с простой формой связи для модели `Contact`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-160">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="d4d50-161">В представленных в этой статье примерах `DbContext` инициализируется в файле [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="d4d50-161">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

<span data-ttu-id="d4d50-162">[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-162">[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]</span></span>

<span data-ttu-id="d4d50-163">Модель данных:</span><span class="sxs-lookup"><span data-stu-id="d4d50-163">The data model:</span></span>

<span data-ttu-id="d4d50-164">[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-164">[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]</span></span>

<span data-ttu-id="d4d50-165">Файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d4d50-165">The *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="d4d50-166">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-166">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]</span></span>

<span data-ttu-id="d4d50-167">Файл кода программной части *Pages/Create.cshtml.cs* для представления:</span><span class="sxs-lookup"><span data-stu-id="d4d50-167">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

<span data-ttu-id="d4d50-168">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=ALL)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-168">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=ALL)]</span></span>

<span data-ttu-id="d4d50-169">Как правило, класс `PageModel` называется `<PageName>Model` и находится в том же пространстве имен, что и страница.</span><span class="sxs-lookup"><span data-stu-id="d4d50-169">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span> <span data-ttu-id="d4d50-170">Конвертация страницы с помощью директивы `@functions` для определения обработчиков и страницы с помощью класса `PageModel` не требует существенных изменений.</span><span class="sxs-lookup"><span data-stu-id="d4d50-170">Not much change is needed to convert from a page using `@functions` to define handlers and a page using a `PageModel` class.</span></span>

<span data-ttu-id="d4d50-171">Применение файла кода программной части `PageModel` позволяет выполнять модульное тестирование, но требует написания явного конструктора и класса.</span><span class="sxs-lookup"><span data-stu-id="d4d50-171">Using a `PageModel` code-behind file supports unit testing, but requires you to write an explicit constructor and class.</span></span> <span data-ttu-id="d4d50-172">Страницы без файлов кода программной части `PageModel` поддерживают компиляцию среды выполнения, что может пригодиться в процессе разработки.</span><span class="sxs-lookup"><span data-stu-id="d4d50-172">Pages without `PageModel` code-behind files support runtime compilation, which can be an advantage in development.</span></span>  <!-- review: advantage because you can make changes and refresh the browser without explicitly compiling the app -->

<span data-ttu-id="d4d50-173">Страница содержит *метод обработчика* `OnPostAsync`, который выполняется по запросам `POST` (когда пользователь публикует форму).</span><span class="sxs-lookup"><span data-stu-id="d4d50-173">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="d4d50-174">Методы обработчика можно добавить для любой HTTP-команды.</span><span class="sxs-lookup"><span data-stu-id="d4d50-174">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="d4d50-175">Наиболее распространенные обработчики</span><span class="sxs-lookup"><span data-stu-id="d4d50-175">The most common handlers are:</span></span>

* <span data-ttu-id="d4d50-176">`OnGet` — инициализация необходимого для страницы состояния.</span><span class="sxs-lookup"><span data-stu-id="d4d50-176">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="d4d50-177">Пример обработчика [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="d4d50-177">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="d4d50-178">`OnPost` — обработка отправленных через форму данных.</span><span class="sxs-lookup"><span data-stu-id="d4d50-178">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="d4d50-179">Суффикс `Async` не является обязательным, но часто используется для асинхронных функций.</span><span class="sxs-lookup"><span data-stu-id="d4d50-179">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="d4d50-180">Код `OnPostAsync` в приведенном выше примере похож на то, что обычно пишут в контроллере.</span><span class="sxs-lookup"><span data-stu-id="d4d50-180">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="d4d50-181">Этот код типичен для Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d4d50-181">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="d4d50-182">Большинство примитивов MVC, включая [привязку модели](xref:mvc/models/model-binding), [проверку](xref:mvc/models/validation) и результаты действий, являются общими.</span><span class="sxs-lookup"><span data-stu-id="d4d50-182">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="d4d50-183">Предыдущий метод `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="d4d50-183">The previous `OnPostAsync` method:</span></span>

<span data-ttu-id="d4d50-184">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-184">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync)]</span></span>

<span data-ttu-id="d4d50-185">Простая схема `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="d4d50-185">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="d4d50-186">Проверка на наличие ошибок проверки.</span><span class="sxs-lookup"><span data-stu-id="d4d50-186">Check for validation errors.</span></span>

*  <span data-ttu-id="d4d50-187">Если ошибок нет, сохранение данных и перенаправление.</span><span class="sxs-lookup"><span data-stu-id="d4d50-187">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="d4d50-188">Если есть ошибки, отображение страницы с сообщениями проверки.</span><span class="sxs-lookup"><span data-stu-id="d4d50-188">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="d4d50-189">Проверка на стороне клиента выполняется точно так же, как в традиционных приложениях MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4d50-189">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="d4d50-190">Во многих случаях ошибки проверки выявляются на клиентском компьютере и на сервер не отправляются.</span><span class="sxs-lookup"><span data-stu-id="d4d50-190">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="d4d50-191">После успешного ввода данных метод обработчика `OnPostAsync` вызывает метод обработчика `RedirectToPage`, чтобы получить экземпляр `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-191">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="d4d50-192">`RedirectToPage` — это новый результат действия, аналогичный `RedirectToAction` или `RedirectToRoute`, но настроенный для страниц.</span><span class="sxs-lookup"><span data-stu-id="d4d50-192">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="d4d50-193">В приведенном выше примере он выполняет перенаправление на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="d4d50-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="d4d50-194">Более подробно `RedirectToPage` рассматривается в разделе [Создание URL для страниц](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="d4d50-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="d4d50-195">Если в отправленной форме имеются ошибки проверки (переданные на сервер), метод обработчика `OnPostAsync` вызывает метод обработчика `Page`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-195">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="d4d50-196">`Page` возвращает экземпляр `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-196">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="d4d50-197">Возвращение `Page` аналогично тому, как действия в контроллерах возвращают `View`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-197">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="d4d50-198">`PageResult` — значение типа возвращаемого значения <!-- Review  --> по умолчанию для метода обработчика.</span><span class="sxs-lookup"><span data-stu-id="d4d50-198">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="d4d50-199">Метод обработчика, вернувший `void`, визуализирует страницу.</span><span class="sxs-lookup"><span data-stu-id="d4d50-199">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="d4d50-200">Для указания согласия на привязку модели в свойстве `Customer` используется атрибут `[BindProperty]`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-200">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

<span data-ttu-id="d4d50-201">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=PageModel&highlight=10-11)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-201">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=PageModel&highlight=10-11)]</span></span>

<span data-ttu-id="d4d50-202">По умолчанию функция Razor Pages привязывает свойства ко всем командам, кроме GET.</span><span class="sxs-lookup"><span data-stu-id="d4d50-202">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="d4d50-203">Привязка к свойствам позволяет сократить объем необходимого кода.</span><span class="sxs-lookup"><span data-stu-id="d4d50-203">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="d4d50-204">Привязка уменьшает код за счет того, что для визуализации полей формы (`<input asp-for="Customer.Name" />`) и получения входных данных используется одно и то же свойство.</span><span class="sxs-lookup"><span data-stu-id="d4d50-204">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="d4d50-205">Ниже показана комбинированная версия кода для создания страницы:</span><span class="sxs-lookup"><span data-stu-id="d4d50-205">The following code shows the combined version of the create page:</span></span>

<span data-ttu-id="d4d50-206">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/CreateCombined.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-206">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/CreateCombined.cshtml)]</span></span>

<span data-ttu-id="d4d50-207">Вместо директивы `@model` воспользуемся новой функцией Pages.</span><span class="sxs-lookup"><span data-stu-id="d4d50-207">Rather than using `@model`, we're taking advantage of a new feature for Pages.</span></span> <span data-ttu-id="d4d50-208">По умолчанию созданный класс, производный от `Page`, *сам по себе* является моделью.</span><span class="sxs-lookup"><span data-stu-id="d4d50-208">By default, the generated `Page`-derived class *is* the model.</span></span> <span data-ttu-id="d4d50-209">Но с представлениями Razor лучше использовать *модель представлений*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-209">Using a *view model* with Razor views is a best practice.</span></span> <span data-ttu-id="d4d50-210">С функцией Pages вы получаете модель представлений *автоматически*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-210">With Pages, you get a view model *automatically*.</span></span>

<span data-ttu-id="d4d50-211">Основное отличие состоит в том, что вместо внедрения конструктора внедряются свойства (`@inject`).</span><span class="sxs-lookup"><span data-stu-id="d4d50-211">The main change is replacing constructor injection with injected (`@inject`) properties.</span></span> <span data-ttu-id="d4d50-212">На этой странице для [внедрения зависимостей конструктора](xref:mvc/controllers/dependency-injection#constructor-injection) используется [@inject](xref:mvc/views/razor#inject).</span><span class="sxs-lookup"><span data-stu-id="d4d50-212">This page uses [@inject](xref:mvc/views/razor#inject) for [constructor dependency injection](xref:mvc/controllers/dependency-injection#constructor-injection).</span></span> <span data-ttu-id="d4d50-213">Инструкция `@inject` создает и инициализирует свойство `Db`, которое используется в `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-213">The `@inject` statement generates and initializes the `Db` property that is used in `OnPostAsync`.</span></span> <span data-ttu-id="d4d50-214">Введенные свойства (`@inject`) задаются до выполнения методов обработчика.</span><span class="sxs-lookup"><span data-stu-id="d4d50-214">Injected (`@inject`) properties are set before handler methods run.</span></span>


<span data-ttu-id="d4d50-215">Домашняя страница (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="d4d50-215">The home page (*Index.cshtml*):</span></span>

<span data-ttu-id="d4d50-216">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-216">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]</span></span>

<span data-ttu-id="d4d50-217">Файл кода программной части *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d4d50-217">The code behind *Index.cshtml.cs* file:</span></span>

<span data-ttu-id="d4d50-218">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-218">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]</span></span>

<span data-ttu-id="d4d50-219">Файл *Index.cshtml* содержит следующую разметку для создания ссылки на правку для каждого контактного лица.</span><span class="sxs-lookup"><span data-stu-id="d4d50-219">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

```html
<a asp-page="./Edit" asp-route-id="@contact.Id">edit</a>
```

<span data-ttu-id="d4d50-220">[Вспомогательный тег привязки](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) использует атрибут [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper#route) для создания ссылки на страницу редактирования.</span><span class="sxs-lookup"><span data-stu-id="d4d50-220">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) used the [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper#route) attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="d4d50-221">Эта ссылка содержит данные о маршруте с идентификатором контактного лица.</span><span class="sxs-lookup"><span data-stu-id="d4d50-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="d4d50-222">Например, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-222">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="d4d50-223">Файл *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d4d50-223">The *Pages/Edit.cshtml* file:</span></span>

<span data-ttu-id="d4d50-224">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-224">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]</span></span>

<span data-ttu-id="d4d50-225">Первая строка содержит директиву `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-225">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="d4d50-226">Ограничение маршрутизации `"{id:int}"` указывает, что страница должна принимать обращенные к ней запросы, которые содержат данные о маршруте `int`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-226">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="d4d50-227">Если запрос к странице не содержит данные о маршруте, которые можно конвертировать в `int`, среда выполнения возвращает ошибку HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="d4d50-227">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="d4d50-228">Файл *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d4d50-228">The *Pages/Edit.cshtml.cs* file:</span></span>

<span data-ttu-id="d4d50-229">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-229">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="d4d50-230">XSRF/CSRF и Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d4d50-230">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="d4d50-231">Вам не придется писать отдельный код для [проверки подлинности запросов](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="d4d50-231">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="d4d50-232">Razor Pages включает создание и проверку маркеров защиты от подделок по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d4d50-232">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="d4d50-233">Использование макетов, частичных реплик, шаблонов и вспомогательных функций тегов с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d4d50-233">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="d4d50-234">Pages работает со всеми функциями представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="d4d50-234">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="d4d50-235">Макеты, частичные реплики, шаблоны, вспомогательные функции тегов, а также файлы *_ViewStart.cshtml* и *_ViewImports.cshtml* работают точно так же, как и в стандартных представлениях Razor.</span><span class="sxs-lookup"><span data-stu-id="d4d50-235">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="d4d50-236">Давайте упростим нашу страницу с помощью некоторых из этих функций.</span><span class="sxs-lookup"><span data-stu-id="d4d50-236">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="d4d50-237">Добавим [макет страницы](xref:mvc/views/layout) в файл *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d4d50-237">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

<span data-ttu-id="d4d50-238">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-238">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]</span></span>

<span data-ttu-id="d4d50-239">Этот [макет](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="d4d50-239">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="d4d50-240">управляет макетом каждой страницы (кроме страниц с отказом от макета);</span><span class="sxs-lookup"><span data-stu-id="d4d50-240">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="d4d50-241">импортирует HTML-структуры, такие как JavaScript и таблицы стилей.</span><span class="sxs-lookup"><span data-stu-id="d4d50-241">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="d4d50-242">Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="d4d50-242">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="d4d50-243">Свойство [Layout](xref:mvc/views/layout#specifying-a-layout) определяется в файле *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d4d50-243">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

<span data-ttu-id="d4d50-244">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-244">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]</span></span>

<span data-ttu-id="d4d50-245">Примечание. Макет хранится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-245">Note: The layout is in the *Pages* folder.</span></span> <span data-ttu-id="d4d50-246">Pages ищет другие представления (макеты, шаблоны, частичные реплики) в иерархическом порядке, начиная с той папки, где находится текущая страница.</span><span class="sxs-lookup"><span data-stu-id="d4d50-246">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="d4d50-247">Макет в папке *Pages* можно использовать на любой странице Razor, которая находится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-247">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="d4d50-248">Корпорация Майкрософт рекомендует **не** размещать файл макета в папке *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-248">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="d4d50-249">*Views/Shared* — это шаблон представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="d4d50-249">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="d4d50-250">Razor Pages опирается на иерархию папок, а не на условные обозначения путей.</span><span class="sxs-lookup"><span data-stu-id="d4d50-250">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="d4d50-251">Поиск представлений в Razor Pages охватывает папку *Pages*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-251">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="d4d50-252">Макеты, шаблоны и частичные реплики *работают* с контроллерами MVC и стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="d4d50-252">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="d4d50-253">Добавим файл *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d4d50-253">Add a *Pages/_ViewImports.cshtml* file:</span></span>

<span data-ttu-id="d4d50-254">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-254">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="d4d50-255">`@namespace` описывается далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="d4d50-255">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="d4d50-256">Директива `@addTagHelper` добавляет [встроенные вспомогательные теги](xref:mvc/views/tag-helpers/builtin-th/Index) на все страницы в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-256">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="d4d50-257">Явное применение директивы `@namespace` на странице:</span><span class="sxs-lookup"><span data-stu-id="d4d50-257">When the `@namespace` directive is used explicitly on a page:</span></span>

<span data-ttu-id="d4d50-258">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-258">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]</span></span>

<span data-ttu-id="d4d50-259">Данная директива задает пространство имен для страницы.</span><span class="sxs-lookup"><span data-stu-id="d4d50-259">The directive sets the namespace for the page.</span></span> <span data-ttu-id="d4d50-260">Включать пространство имен в директиву `@model` не требуется.</span><span class="sxs-lookup"><span data-stu-id="d4d50-260">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="d4d50-261">Если директива `@namespace` содержится в файле *_ViewImports.cshtml*, указанное пространство имен определяет префикс для созданного в Pages пространства имен, куда импортируется директива `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-261">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="d4d50-262">Остальная часть созданного пространства имен (суффикс) представляет собой разделенный точками относительный путь между папкой с файлом *_ViewImports.cshtml* и папкой, содержащей страницу.</span><span class="sxs-lookup"><span data-stu-id="d4d50-262">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="d4d50-263">Например, файл кода программной части *Pages/Customers/Edit.cshtml.cs* задает пространство имен явно:</span><span class="sxs-lookup"><span data-stu-id="d4d50-263">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

<span data-ttu-id="d4d50-264">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=namespace)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-264">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=namespace)]</span></span>

<span data-ttu-id="d4d50-265">Файл *Pages/_ViewImports.cshtml* задает следующее пространство имен:</span><span class="sxs-lookup"><span data-stu-id="d4d50-265">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

<span data-ttu-id="d4d50-266">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-266">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]</span></span>

<span data-ttu-id="d4d50-267">Сформированное пространство имен для файла *Pages/Customers/Edit.cshtml* Razor Pages совпадает с именем файла кода программной части.</span><span class="sxs-lookup"><span data-stu-id="d4d50-267">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="d4d50-268">Директива `@namespace` разработана таким образом, чтобы добавляемые в проект классы C# и сгенерированный страницами код *просто работали* без добавления директивы `@using` для файла кода программной части.</span><span class="sxs-lookup"><span data-stu-id="d4d50-268">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="d4d50-269">Примечание. `@namespace` также работает со стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="d4d50-269">Note: `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="d4d50-270">Исходный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d4d50-270">The original *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="d4d50-271">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-271">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]</span></span>

<span data-ttu-id="d4d50-272">Обновленная страница:</span><span class="sxs-lookup"><span data-stu-id="d4d50-272">The updated page:</span></span>

<span data-ttu-id="d4d50-273">Файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d4d50-273">The *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="d4d50-274">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-274">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]</span></span>

<span data-ttu-id="d4d50-275">[Начальный проект Razor Pages](#rpvs17) содержит файл *Pages/_ValidationScriptsPartial.cshtml*, который подключает проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="d4d50-275">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="d4d50-276">Формирование URL-адресов для страниц</span><span class="sxs-lookup"><span data-stu-id="d4d50-276">URL generation for Pages</span></span>

<span data-ttu-id="d4d50-277">На представленной выше странице `Create` используется `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="d4d50-277">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

<span data-ttu-id="d4d50-278">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-278">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync&highlight=10)]</span></span>

<span data-ttu-id="d4d50-279">Это приложение имеет следующую структуру файлов и папок.</span><span class="sxs-lookup"><span data-stu-id="d4d50-279">The app has the following file/folder structure</span></span>

* <span data-ttu-id="d4d50-280">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="d4d50-280">*/Pages*</span></span>

  * <span data-ttu-id="d4d50-281">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d4d50-281">*Index.cshtml*</span></span>
  * <span data-ttu-id="d4d50-282">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="d4d50-282">*/Customer*</span></span>

    * <span data-ttu-id="d4d50-283">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d4d50-283">*Create.cshtml*</span></span>
    * <span data-ttu-id="d4d50-284">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d4d50-284">*Edit.cshtml*</span></span>
    * <span data-ttu-id="d4d50-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d4d50-285">*Index.cshtml*</span></span>

<span data-ttu-id="d4d50-286">После успешного выполнения страницы *Pages/Customers/Create.cshtml* и *Pages/Customers/Edit.cshtml* перенаправляются на страницу *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-286">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="d4d50-287">Строка `/Index` составляет часть URL-адреса для доступа к указанной выше странице.</span><span class="sxs-lookup"><span data-stu-id="d4d50-287">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="d4d50-288">Строка `/Index` позволяет создавать URL-адреса для страницы *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-288">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="d4d50-289">Пример:</span><span class="sxs-lookup"><span data-stu-id="d4d50-289">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="d4d50-290">Имя страницы — это путь к странице из корневой папки */Pages* (включая начальный символ `/`, например `/Index`).</span><span class="sxs-lookup"><span data-stu-id="d4d50-290">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="d4d50-291">Приведенные выше примеры создания URL-адресов задействуют гораздо больше функций, чем просто прописывание URL в коде.</span><span class="sxs-lookup"><span data-stu-id="d4d50-291">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="d4d50-292">Формирование URL-адресов включает [маршрутизацию](xref:mvc/controllers/routing) и позволяет генерировать и включать в код параметры в зависимости от того, как определяется маршрут в пути назначения.</span><span class="sxs-lookup"><span data-stu-id="d4d50-292">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="d4d50-293">Формирование URL-адресов для страниц поддерживает относительные имена.</span><span class="sxs-lookup"><span data-stu-id="d4d50-293">URL generation for pages supports relative names.</span></span> <span data-ttu-id="d4d50-294">В приведенной ниже таблице показано, какая страница индекса выбирается с каждым параметром `RedirectToPage` в *Pages/Customers/Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-294">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="d4d50-295">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="d4d50-295">RedirectToPage(x)</span></span>| <span data-ttu-id="d4d50-296">Страница</span><span class="sxs-lookup"><span data-stu-id="d4d50-296">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="d4d50-297">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="d4d50-297">RedirectToPage("/Index")</span></span> | <span data-ttu-id="d4d50-298">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="d4d50-298">*Pages/Index*</span></span> |
| <span data-ttu-id="d4d50-299">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="d4d50-299">RedirectToPage("./Index");</span></span> | <span data-ttu-id="d4d50-300">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="d4d50-300">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="d4d50-301">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="d4d50-301">RedirectToPage("../Index")</span></span> | <span data-ttu-id="d4d50-302">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="d4d50-302">*Pages/Index*</span></span> |
| <span data-ttu-id="d4d50-303">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="d4d50-303">RedirectToPage("Index")</span></span>  | <span data-ttu-id="d4d50-304">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="d4d50-304">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="d4d50-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")` и `RedirectToPage("../Index")` — это *относительные имена*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="d4d50-306">Для получения имени целевой страницы параметр `RedirectToPage` *комбинируется* с путем текущей страницы.</span><span class="sxs-lookup"><span data-stu-id="d4d50-306">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="d4d50-307">Привязка относительных имен полезна при создании сайтов со сложной структурой.</span><span class="sxs-lookup"><span data-stu-id="d4d50-307">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="d4d50-308">Если для связи между страницами в определенной папке используются относительные имена, эту папку можно переименовать.</span><span class="sxs-lookup"><span data-stu-id="d4d50-308">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="d4d50-309">При этом все ссылки останутся рабочими (так как не включают имя папки).</span><span class="sxs-lookup"><span data-stu-id="d4d50-309">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="d4d50-310">TempData</span><span class="sxs-lookup"><span data-stu-id="d4d50-310">TempData</span></span>

<span data-ttu-id="d4d50-311">ASP.NET Core позволяет использовать свойство [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) в [контроллере](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="d4d50-311">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="d4d50-312">Это свойство хранит данные до тех пор, пока они не будут прочитаны.</span><span class="sxs-lookup"><span data-stu-id="d4d50-312">This property stores data until it is read.</span></span> <span data-ttu-id="d4d50-313">Для проверки данных без удаления можно использовать методы `Keep` и `Peek`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-313">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="d4d50-314">`TempData` удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса.</span><span class="sxs-lookup"><span data-stu-id="d4d50-314">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="d4d50-315">Атрибут `[TempData]` появился в ASP.NET 2.0 и поддерживается в контроллерах и на страницах.</span><span class="sxs-lookup"><span data-stu-id="d4d50-315">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="d4d50-316">В следующем коде значение `Message` задается с помощью `TempData`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-316">The following code sets the value of `Message` using `TempData`.</span></span>
<span data-ttu-id="d4d50-317">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,27-28&name=snippetTemp)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-317">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,27-28&name=snippetTemp)]</span></span>

<span data-ttu-id="d4d50-318">Представленная ниже разметка в файле *Pages/Customers/Index.cshtml* отображает значение `Message` с помощью `TempData`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-318">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="d4d50-319">Файл кода программной части *Pages/Customers/Index.cshtml.cs* применяет атрибут `[TempData]` к свойству `Message`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-319">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="d4d50-320">Дополнительные сведения см. в разделе [TempData](xref:fundamentals/app-state#temp).</span><span class="sxs-lookup"><span data-stu-id="d4d50-320">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="d4d50-321">Несколько обработчиков на страницу</span><span class="sxs-lookup"><span data-stu-id="d4d50-321">Multiple handlers per page</span></span>

<span data-ttu-id="d4d50-322">Следующая страница формирует разметку для двух обработчиков страницы с помощью вспомогательного тега `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="d4d50-322">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

<span data-ttu-id="d4d50-323">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-323">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span></span>

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="d4d50-324">Форма в предыдущем примере включает две кнопки отправки, каждая из которых отправляет данные на отдельный URL-адрес с помощью `FormActionTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-324">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="d4d50-325">Атрибут `asp-page-handler` является дополнением к `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-325">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="d4d50-326">Атрибут `asp-page-handler` формирует URL-адреса, ,которые используются для отправки данных в каждый из методов обработчиков, определенных страницей.</span><span class="sxs-lookup"><span data-stu-id="d4d50-326">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="d4d50-327">`asp-page` не задается, так как пример сопоставлен с текущей страницей.</span><span class="sxs-lookup"><span data-stu-id="d4d50-327">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="d4d50-328">Файл кода программной части:</span><span class="sxs-lookup"><span data-stu-id="d4d50-328">The code-behind file:</span></span>

<span data-ttu-id="d4d50-329">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-329">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]</span></span>

<span data-ttu-id="d4d50-330">В представленном выше коде используются *именованные методы обработчика*.</span><span class="sxs-lookup"><span data-stu-id="d4d50-330">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="d4d50-331">Именованные методы обработчика создаются путем размещения определенного текста в имени после `On<HTTP Verb>` и перед `Async` (если есть).</span><span class="sxs-lookup"><span data-stu-id="d4d50-331">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="d4d50-332">В приведенном выше примере использовались методы страницы OnPost**JoinList**Async и OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="d4d50-332">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="d4d50-333">Если убрать *OnPost* и *Async*, имена обработчиков будут выглядеть как `JoinList` и `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-333">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

<span data-ttu-id="d4d50-334">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-334">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span></span>

<span data-ttu-id="d4d50-335">При использовании представленного выше кода URL-путь для отправки данных в `OnPostJoinListAsync` будет выглядеть как `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-335">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="d4d50-336">URL-путь для отправки данных в `OnPostJoinListUCAsync` будет иметь вид `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="d4d50-336">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="d4d50-337">Настройка маршрутизации</span><span class="sxs-lookup"><span data-stu-id="d4d50-337">Customizing Routing</span></span>

<span data-ttu-id="d4d50-338">Если вы не хотите, чтобы в URL-адресе отображалась строка запроса `?handler=JoinList`, измените маршрут таким образом, чтобы в качестве пути в URL-адресе указывалось имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="d4d50-338">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="d4d50-339">Для настройки маршрута можно добавить после директивы `@page` шаблон маршрута, заключенные в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="d4d50-339">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

<span data-ttu-id="d4d50-340">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-340">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]</span></span>


<span data-ttu-id="d4d50-341">В представленном выше маршруте вместо строки запроса URL-путь содержит имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="d4d50-341">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="d4d50-342">Символ `?` после `handler` означает, что параметр маршрута является необязательным.</span><span class="sxs-lookup"><span data-stu-id="d4d50-342">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="d4d50-343">С помощью директивы `@page` можно добавить к маршруту страницы дополнительные сегменты и параметры.</span><span class="sxs-lookup"><span data-stu-id="d4d50-343">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="d4d50-344">Все, что вы добавите, будет **присоединено** к маршруту страницы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d4d50-344">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="d4d50-345">Использовать для изменения маршрута страницы абсолютный или виртуальный путь (например, вида `"~/Some/Other/Path"`) нельзя.</span><span class="sxs-lookup"><span data-stu-id="d4d50-345">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="d4d50-346">Конфигурация и параметры</span><span class="sxs-lookup"><span data-stu-id="d4d50-346">Configuration and settings</span></span>

<span data-ttu-id="d4d50-347">Чтобы настроить дополнительные параметры, воспользуйтесь методом расширения `AddRazorPagesOptions` в построителе MVC:</span><span class="sxs-lookup"><span data-stu-id="d4d50-347">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

<span data-ttu-id="d4d50-348">[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d4d50-348">[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet1)]</span></span>

<span data-ttu-id="d4d50-349">В настоящее время `RazorPagesOptions` позволяет задавать корневую папку для страниц и добавлять для них обозначения моделей приложений.</span><span class="sxs-lookup"><span data-stu-id="d4d50-349">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="d4d50-350">В дальнейшем мы планируем расширить спектр его возможностей.</span><span class="sxs-lookup"><span data-stu-id="d4d50-350">We hope to enable more extensibility this way in the future.</span></span>

<span data-ttu-id="d4d50-351">Сведения о предварительной компиляции представлений см. в статье [Компиляция представлений Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="d4d50-351">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="d4d50-352">[Загрузить или просмотреть пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="d4d50-352">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="d4d50-353">Представленные общие сведения являются вводными для статьи [Начало работы с Razor Pages в ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="d4d50-353">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>
