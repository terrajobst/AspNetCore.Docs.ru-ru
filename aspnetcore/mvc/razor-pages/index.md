---
title: "Введение в Razor Pages в ASP.NET Core"
author: Rick-Anderson
description: "Сведения о применении в ASP.NET Core функции Razor Pages, которая делает создание кодов сценариев для страниц проще и эффективнее по сравнению с MVC."
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: cb80c38fd0284d5153aebfe7bb515722623a4a34
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="f19a0-103">Введение в Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f19a0-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f19a0-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Райан Новак](https://github.com/rynowak) (Ryan Nowak)</span><span class="sxs-lookup"><span data-stu-id="f19a0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="f19a0-105">Razor Pages — это новая функция платформы MVC ASP.NET Core, которая делает создание кодов сценариев для страниц проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="f19a0-105">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="f19a0-106">Если вам нужно руководство, использующее подход "модель-представление-контроллер", см. статью [Начало работы с MVC в ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="f19a0-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="f19a0-107">Этот документ содержит вводные сведения о Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f19a0-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="f19a0-108">Это не пошаговое руководство.</span><span class="sxs-lookup"><span data-stu-id="f19a0-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="f19a0-109">Если некоторые разделы покажутся вам слишком сложными, см. [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="f19a0-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="f19a0-110">Общие сведения об ASP.NET Core см. в разделе [Введение в ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="f19a0-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="f19a0-111">Предварительные требования ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="f19a0-111">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="f19a0-112">Установите [.NET Core](https://www.microsoft.com/net/core) 2.0.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="f19a0-112">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="f19a0-113">Если вы используете Visual Studio, установите [Visual Studio](https://www.visualstudio.com/vs/) 2017 версии 15.3 или более поздней версии, а также следующие рабочие нагрузки:</span><span class="sxs-lookup"><span data-stu-id="f19a0-113">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="f19a0-114">**ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="f19a0-114">**ASP.NET and web development**</span></span>
* <span data-ttu-id="f19a0-115">**Кроссплатформенная разработка .NET Core**</span><span class="sxs-lookup"><span data-stu-id="f19a0-115">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="f19a0-116">Создание проекта Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f19a0-116">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f19a0-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f19a0-117">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="f19a0-118">Подробные инструкции по созданию проекта Razor Pages с помощью Visual Studio см. в статье [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="f19a0-118">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f19a0-119">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f19a0-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f19a0-120">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-120">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="f19a0-121">Откройте созданный файл *.csproj* в Visual Studio для Mac.</span><span class="sxs-lookup"><span data-stu-id="f19a0-121">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f19a0-122">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f19a0-122">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="f19a0-123">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-123">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f19a0-124">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="f19a0-124">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="f19a0-125">Из командной строки выполните команду `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-125">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="f19a0-126">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f19a0-126">Razor Pages</span></span>

<span data-ttu-id="f19a0-127">Функция Razor Pages активируется в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f19a0-127">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="f19a0-128">Рассмотрим простую страницу: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="f19a0-128">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="f19a0-129">Представленный выше код выглядит как файл представления Razor</span><span class="sxs-lookup"><span data-stu-id="f19a0-129">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="f19a0-130">и отличается от него только директивой `@page`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-130">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="f19a0-131">Директива `@page` превращает файл в действие MVC, а значит обрабатывает запросы напрямую, минуя контроллер.</span><span class="sxs-lookup"><span data-stu-id="f19a0-131">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="f19a0-132">Директива `@page` должна быть первой директивой Razor на странице.</span><span class="sxs-lookup"><span data-stu-id="f19a0-132">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="f19a0-133">Директива `@page` влияет на поведение всех остальных конструкций Razor.</span><span class="sxs-lookup"><span data-stu-id="f19a0-133">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="f19a0-134">Похожая страница с использованием класса `PageModel` показана в следующих двух файлах.</span><span class="sxs-lookup"><span data-stu-id="f19a0-134">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="f19a0-135">Файл *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f19a0-135">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="f19a0-136">Модель страницы *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f19a0-136">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="f19a0-137">Как правило, файл класса `PageModel` называется так же, как файл Razor Pages, но с расширением *.cs*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-137">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="f19a0-138">Например, представленная выше страница Razor Pages называется *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-138">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="f19a0-139">Файл, содержащий класс `PageModel`, называется *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-139">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="f19a0-140">Сопоставления URL-адресов со страницами определяются расположением конкретной страницы в файловой системе.</span><span class="sxs-lookup"><span data-stu-id="f19a0-140">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="f19a0-141">В приведенной ниже таблице показаны пути Razor Pages и соответствующие URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="f19a0-141">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="f19a0-142">Имя файла и путь</span><span class="sxs-lookup"><span data-stu-id="f19a0-142">File name and path</span></span>               | <span data-ttu-id="f19a0-143">Соответствующий URL</span><span class="sxs-lookup"><span data-stu-id="f19a0-143">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="f19a0-144">*/Pages/index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f19a0-144">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="f19a0-145">`/` или `/Index`</span><span class="sxs-lookup"><span data-stu-id="f19a0-145">`/` or `/Index`</span></span> |
| <span data-ttu-id="f19a0-146">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f19a0-146">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="f19a0-147">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f19a0-147">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="f19a0-148">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f19a0-148">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="f19a0-149">`/Store` или `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="f19a0-149">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="f19a0-150">Примечания.</span><span class="sxs-lookup"><span data-stu-id="f19a0-150">Notes:</span></span>

* <span data-ttu-id="f19a0-151">Среда выполнения по умолчанию ищет файлы Razor Pages в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-151">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="f19a0-152">Если в URL-адресе не указана конкретная страница, по умолчанию открывается страница `Index`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-152">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="f19a0-153">Создание простой формы</span><span class="sxs-lookup"><span data-stu-id="f19a0-153">Writing a basic form</span></span>

<span data-ttu-id="f19a0-154">Функция Razor Pages предназначена для упрощения типовых шаблонов, которые используются в браузерах.</span><span class="sxs-lookup"><span data-stu-id="f19a0-154">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="f19a0-155">[Привязки модели](xref:mvc/models/model-binding), [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и вспомогательные методы HTML *отлично работают* со свойствами, определенными в классе Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f19a0-155">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="f19a0-156">Рассмотрим страницу с простой формой связи для модели `Contact`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-156">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="f19a0-157">В представленных в этой статье примерах `DbContext` инициализируется в файле [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="f19a0-157">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="f19a0-158">Модель данных:</span><span class="sxs-lookup"><span data-stu-id="f19a0-158">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="f19a0-159">Контекст базы данных:</span><span class="sxs-lookup"><span data-stu-id="f19a0-159">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="f19a0-160">Файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f19a0-160">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="f19a0-161">Модель страницы *Pages/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f19a0-161">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="f19a0-162">Как правило, класс `PageModel` называется `<PageName>Model` и находится в том же пространстве имен, что и страница.</span><span class="sxs-lookup"><span data-stu-id="f19a0-162">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="f19a0-163">Класс `PageModel` позволяет разделять логику страницы и ее представление.</span><span class="sxs-lookup"><span data-stu-id="f19a0-163">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="f19a0-164">Он определяет обработчики страницы для запросов, отправляемых на страницу, а также данные для ее визуализации.</span><span class="sxs-lookup"><span data-stu-id="f19a0-164">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="f19a0-165">Такое разделение позволяет управлять зависимостями страницы путем их [внедрения](xref:fundamentals/dependency-injection) и выполнять [модульное тестирование](xref:testing/razor-pages-testing) страниц.</span><span class="sxs-lookup"><span data-stu-id="f19a0-165">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="f19a0-166">Страница содержит *метод обработчика* `OnPostAsync`, который выполняется по запросам `POST` (когда пользователь публикует форму).</span><span class="sxs-lookup"><span data-stu-id="f19a0-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="f19a0-167">Методы обработчика можно добавить для любой HTTP-команды.</span><span class="sxs-lookup"><span data-stu-id="f19a0-167">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="f19a0-168">Наиболее распространенные обработчики</span><span class="sxs-lookup"><span data-stu-id="f19a0-168">The most common handlers are:</span></span>

* <span data-ttu-id="f19a0-169">`OnGet` — инициализация необходимого для страницы состояния.</span><span class="sxs-lookup"><span data-stu-id="f19a0-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="f19a0-170">Пример обработчика [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="f19a0-170">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="f19a0-171">`OnPost` — обработка отправленных через форму данных.</span><span class="sxs-lookup"><span data-stu-id="f19a0-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="f19a0-172">Суффикс `Async` не является обязательным, но часто используется для асинхронных функций.</span><span class="sxs-lookup"><span data-stu-id="f19a0-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="f19a0-173">Код `OnPostAsync` в приведенном выше примере похож на то, что обычно пишут в контроллере.</span><span class="sxs-lookup"><span data-stu-id="f19a0-173">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="f19a0-174">Этот код типичен для Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f19a0-174">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="f19a0-175">Большинство примитивов MVC, включая [привязку модели](xref:mvc/models/model-binding), [проверку](xref:mvc/models/validation) и результаты действий, являются общими.</span><span class="sxs-lookup"><span data-stu-id="f19a0-175">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="f19a0-176">Предыдущий метод `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="f19a0-176">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="f19a0-177">Простая схема `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="f19a0-177">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="f19a0-178">Проверка на наличие ошибок проверки.</span><span class="sxs-lookup"><span data-stu-id="f19a0-178">Check for validation errors.</span></span>

*  <span data-ttu-id="f19a0-179">Если ошибок нет, сохранение данных и перенаправление.</span><span class="sxs-lookup"><span data-stu-id="f19a0-179">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="f19a0-180">Если есть ошибки, отображение страницы с сообщениями проверки.</span><span class="sxs-lookup"><span data-stu-id="f19a0-180">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="f19a0-181">Проверка на стороне клиента выполняется точно так же, как в традиционных приложениях MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f19a0-181">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="f19a0-182">Во многих случаях ошибки проверки выявляются на клиентском компьютере и на сервер не отправляются.</span><span class="sxs-lookup"><span data-stu-id="f19a0-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="f19a0-183">После успешного ввода данных метод обработчика `OnPostAsync` вызывает метод обработчика `RedirectToPage`, чтобы получить экземпляр `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-183">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="f19a0-184">`RedirectToPage` — это новый результат действия, аналогичный `RedirectToAction` или `RedirectToRoute`, но настроенный для страниц.</span><span class="sxs-lookup"><span data-stu-id="f19a0-184">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="f19a0-185">В приведенном выше примере он выполняет перенаправление на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="f19a0-185">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="f19a0-186">Более подробно `RedirectToPage` рассматривается в разделе [Создание URL для страниц](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="f19a0-186">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="f19a0-187">Если в отправленной форме имеются ошибки проверки (переданные на сервер), метод обработчика `OnPostAsync` вызывает метод обработчика `Page`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-187">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="f19a0-188">`Page` возвращает экземпляр `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-188">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="f19a0-189">Возвращение `Page` аналогично тому, как действия в контроллерах возвращают `View`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-189">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="f19a0-190">`PageResult` — значение типа возвращаемого значения <!-- Review  --> по умолчанию для метода обработчика.</span><span class="sxs-lookup"><span data-stu-id="f19a0-190">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="f19a0-191">Метод обработчика, вернувший `void`, визуализирует страницу.</span><span class="sxs-lookup"><span data-stu-id="f19a0-191">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="f19a0-192">Для указания согласия на привязку модели в свойстве `Customer` используется атрибут `[BindProperty]`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-192">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="f19a0-193">По умолчанию функция Razor Pages привязывает свойства ко всем командам, кроме GET.</span><span class="sxs-lookup"><span data-stu-id="f19a0-193">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="f19a0-194">Привязка к свойствам позволяет сократить объем необходимого кода.</span><span class="sxs-lookup"><span data-stu-id="f19a0-194">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="f19a0-195">Привязка уменьшает код за счет того, что для визуализации полей формы (`<input asp-for="Customer.Name" />`) и получения входных данных используется одно и то же свойство.</span><span class="sxs-lookup"><span data-stu-id="f19a0-195">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="f19a0-196">В целях обеспечения безопасности следует привязать данные запросов GET к свойствам модели страниц.</span><span class="sxs-lookup"><span data-stu-id="f19a0-196">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="f19a0-197">Проверьте введенные данные пользователя, прежде чем сопоставлять их со свойствами.</span><span class="sxs-lookup"><span data-stu-id="f19a0-197">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="f19a0-198">Эти действия помогут при создании функций, использующих строки запросов и значения маршрутов.</span><span class="sxs-lookup"><span data-stu-id="f19a0-198">Opting in to this behavior is useful when building features which rely on query string or route values.</span></span>
>
> <span data-ttu-id="f19a0-199">Чтобы привязать свойство к запросам GET, задайте для свойства `SupportsGet` атрибута `[BindProperty]` значение `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="f19a0-199">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="f19a0-200">Домашняя страница (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="f19a0-200">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="f19a0-201">Файл кода программной части *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f19a0-201">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="f19a0-202">Файл *Index.cshtml* содержит следующую разметку для создания ссылки на правку для каждого контактного лица.</span><span class="sxs-lookup"><span data-stu-id="f19a0-202">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="f19a0-203">[Вспомогательный тег привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) использует атрибут `asp-route-{value}` для создания ссылки на страницу редактирования.</span><span class="sxs-lookup"><span data-stu-id="f19a0-203">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="f19a0-204">Эта ссылка содержит данные о маршруте с идентификатором контактного лица.</span><span class="sxs-lookup"><span data-stu-id="f19a0-204">The link contains route data with the contact ID.</span></span> <span data-ttu-id="f19a0-205">Например, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-205">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="f19a0-206">Файл *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f19a0-206">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="f19a0-207">Первая строка содержит директиву `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-207">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="f19a0-208">Ограничение маршрутизации `"{id:int}"` указывает, что страница должна принимать обращенные к ней запросы, которые содержат данные о маршруте `int`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-208">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="f19a0-209">Если запрос к странице не содержит данные о маршруте, которые можно конвертировать в `int`, среда выполнения возвращает ошибку HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="f19a0-209">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="f19a0-210">Файл *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f19a0-210">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="f19a0-211">Файл *Index.cshtml* также содержит разметку для создания кнопки удаления у каждого контакта клиента:</span><span class="sxs-lookup"><span data-stu-id="f19a0-211">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="f19a0-212">Во время обработки кнопки удаления в HTML ее `formaction` включает параметры для следующего:</span><span class="sxs-lookup"><span data-stu-id="f19a0-212">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="f19a0-213">Идентификатор контакта клиента, указанный атрибутом `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-213">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="f19a0-214">Параметр `handler`, указанный атрибутом `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-214">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="f19a0-215">Пример отображенной кнопки удаления с идентификатором контакта клиента `1`:</span><span class="sxs-lookup"><span data-stu-id="f19a0-215">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="f19a0-216">При выборе кнопки на сервер отправляется запрос формы `POST`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-216">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="f19a0-217">По соглашению имя метода обработчика выбирается на основе значения параметра `handler` в соответствии со схемой `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-217">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="f19a0-218">Так как `handler` — `delete` в этом примере, метод обработчика `OnPostDeleteAsync` используется для обработки запроса `POST`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-218">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="f19a0-219">Если `asp-page-handler` имеет другое значение, например `remove`, выбирается метод обработчика страницы с именем `OnPostRemoveAsync`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-219">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="f19a0-220">Метод `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="f19a0-220">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="f19a0-221">Принимает `id` из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="f19a0-221">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="f19a0-222">Отправляет в базу данных запрос контакта клиента с `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-222">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="f19a0-223">Если контакт клиента найден, он удаляется из списка контактов.</span><span class="sxs-lookup"><span data-stu-id="f19a0-223">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="f19a0-224">База данных обновляется.</span><span class="sxs-lookup"><span data-stu-id="f19a0-224">The database is updated.</span></span>
* <span data-ttu-id="f19a0-225">Вызывает `RedirectToPage` для перенаправления на корневую страницу индекса (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="f19a0-225">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="f19a0-226">XSRF/CSRF и Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f19a0-226">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="f19a0-227">Вам не придется писать отдельный код для [проверки подлинности запросов](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="f19a0-227">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="f19a0-228">Razor Pages включает создание и проверку маркеров защиты от подделок по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f19a0-228">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="f19a0-229">Использование макетов, частичных реплик, шаблонов и вспомогательных функций тегов с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f19a0-229">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="f19a0-230">Pages работает со всеми функциями представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="f19a0-230">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="f19a0-231">Макеты, частичные реплики, шаблоны, вспомогательные функции тегов, а также файлы *_ViewStart.cshtml* и *_ViewImports.cshtml* работают точно так же, как и в стандартных представлениях Razor.</span><span class="sxs-lookup"><span data-stu-id="f19a0-231">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="f19a0-232">Давайте упростим нашу страницу с помощью некоторых из этих функций.</span><span class="sxs-lookup"><span data-stu-id="f19a0-232">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="f19a0-233">Добавим [макет страницы](xref:mvc/views/layout) в файл *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f19a0-233">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="f19a0-234">Этот [макет](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="f19a0-234">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="f19a0-235">управляет макетом каждой страницы (кроме страниц с отказом от макета);</span><span class="sxs-lookup"><span data-stu-id="f19a0-235">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="f19a0-236">импортирует HTML-структуры, такие как JavaScript и таблицы стилей.</span><span class="sxs-lookup"><span data-stu-id="f19a0-236">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="f19a0-237">Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="f19a0-237">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="f19a0-238">Свойство [Layout](xref:mvc/views/layout#specifying-a-layout) определяется в файле *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f19a0-238">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="f19a0-239">**Примечание.** Макет хранится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-239">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="f19a0-240">Pages ищет другие представления (макеты, шаблоны, частичные реплики) в иерархическом порядке, начиная с той папки, где находится текущая страница.</span><span class="sxs-lookup"><span data-stu-id="f19a0-240">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="f19a0-241">Макет в папке *Pages* можно использовать на любой странице Razor, которая находится в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-241">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="f19a0-242">Корпорация Майкрософт рекомендует **не** размещать файл макета в папке *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-242">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="f19a0-243">*Views/Shared* — это шаблон представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="f19a0-243">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="f19a0-244">Razor Pages опирается на иерархию папок, а не на условные обозначения путей.</span><span class="sxs-lookup"><span data-stu-id="f19a0-244">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="f19a0-245">Поиск представлений в Razor Pages охватывает папку *Pages*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-245">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="f19a0-246">Макеты, шаблоны и частичные реплики *работают* с контроллерами MVC и стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="f19a0-246">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="f19a0-247">Добавим файл *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f19a0-247">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="f19a0-248">`@namespace` описывается далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="f19a0-248">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="f19a0-249">Директива `@addTagHelper` добавляет [встроенные вспомогательные теги](xref:mvc/views/tag-helpers/builtin-th/Index) на все страницы в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-249">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="f19a0-250">Явное применение директивы `@namespace` на странице:</span><span class="sxs-lookup"><span data-stu-id="f19a0-250">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="f19a0-251">Данная директива задает пространство имен для страницы.</span><span class="sxs-lookup"><span data-stu-id="f19a0-251">The directive sets the namespace for the page.</span></span> <span data-ttu-id="f19a0-252">Включать пространство имен в директиву `@model` не требуется.</span><span class="sxs-lookup"><span data-stu-id="f19a0-252">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="f19a0-253">Если директива `@namespace` содержится в файле *_ViewImports.cshtml*, указанное пространство имен определяет префикс для созданного в Pages пространства имен, куда импортируется директива `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-253">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="f19a0-254">Остальная часть созданного пространства имен (суффикс) представляет собой разделенный точками относительный путь между папкой с файлом *_ViewImports.cshtml* и папкой, содержащей страницу.</span><span class="sxs-lookup"><span data-stu-id="f19a0-254">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="f19a0-255">Например, файл кода программной части *Pages/Customers/Edit.cshtml.cs* задает пространство имен явно:</span><span class="sxs-lookup"><span data-stu-id="f19a0-255">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="f19a0-256">Файл *Pages/_ViewImports.cshtml* задает следующее пространство имен:</span><span class="sxs-lookup"><span data-stu-id="f19a0-256">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="f19a0-257">Сформированное пространство имен для файла *Pages/Customers/Edit.cshtml* Razor Pages совпадает с именем файла кода программной части.</span><span class="sxs-lookup"><span data-stu-id="f19a0-257">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="f19a0-258">Директива `@namespace` разработана таким образом, чтобы добавляемые в проект классы C# и сгенерированный страницами код *просто работали* без добавления директивы `@using` для файла кода программной части.</span><span class="sxs-lookup"><span data-stu-id="f19a0-258">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="f19a0-259">**Примечание.** `@namespace` также работает со стандартными представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="f19a0-259">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="f19a0-260">Исходный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f19a0-260">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="f19a0-261">Обновленный файл представления *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f19a0-261">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="f19a0-262">[Начальный проект Razor Pages](#rpvs17) содержит файл *Pages/_ValidationScriptsPartial.cshtml*, который подключает проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="f19a0-262">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="f19a0-263">Формирование URL-адресов для страниц</span><span class="sxs-lookup"><span data-stu-id="f19a0-263">URL generation for Pages</span></span>

<span data-ttu-id="f19a0-264">На представленной выше странице `Create` используется `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="f19a0-264">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="f19a0-265">Это приложение имеет следующую структуру файлов и папок:</span><span class="sxs-lookup"><span data-stu-id="f19a0-265">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="f19a0-266">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="f19a0-266">*/Pages*</span></span>

  * <span data-ttu-id="f19a0-267">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f19a0-267">*Index.cshtml*</span></span>
  * <span data-ttu-id="f19a0-268">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="f19a0-268">*/Customer*</span></span>

    * <span data-ttu-id="f19a0-269">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f19a0-269">*Create.cshtml*</span></span>
    * <span data-ttu-id="f19a0-270">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f19a0-270">*Edit.cshtml*</span></span>
    * <span data-ttu-id="f19a0-271">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f19a0-271">*Index.cshtml*</span></span>

<span data-ttu-id="f19a0-272">После успешного выполнения страницы *Pages/Customers/Create.cshtml* и *Pages/Customers/Edit.cshtml* перенаправляются на страницу *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-272">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="f19a0-273">Строка `/Index` составляет часть URL-адреса для доступа к указанной выше странице.</span><span class="sxs-lookup"><span data-stu-id="f19a0-273">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="f19a0-274">Строка `/Index` позволяет создавать URL-адреса для страницы *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-274">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="f19a0-275">Пример:</span><span class="sxs-lookup"><span data-stu-id="f19a0-275">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="f19a0-276">Имя страницы — это путь к странице из корневой папки */Pages* (включая начальный символ `/`, например `/Index`).</span><span class="sxs-lookup"><span data-stu-id="f19a0-276">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="f19a0-277">Приведенные выше примеры создания URL-адресов задействуют гораздо больше функций, чем просто прописывание URL в коде.</span><span class="sxs-lookup"><span data-stu-id="f19a0-277">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="f19a0-278">Формирование URL-адресов включает [маршрутизацию](xref:mvc/controllers/routing) и позволяет генерировать и включать в код параметры в зависимости от того, как определяется маршрут в пути назначения.</span><span class="sxs-lookup"><span data-stu-id="f19a0-278">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="f19a0-279">Формирование URL-адресов для страниц поддерживает относительные имена.</span><span class="sxs-lookup"><span data-stu-id="f19a0-279">URL generation for pages supports relative names.</span></span> <span data-ttu-id="f19a0-280">В приведенной ниже таблице показано, какая страница индекса выбирается с каждым параметром `RedirectToPage` в *Pages/Customers/Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-280">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="f19a0-281">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="f19a0-281">RedirectToPage(x)</span></span>| <span data-ttu-id="f19a0-282">Страница</span><span class="sxs-lookup"><span data-stu-id="f19a0-282">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="f19a0-283">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="f19a0-283">RedirectToPage("/Index")</span></span> | <span data-ttu-id="f19a0-284">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="f19a0-284">*Pages/Index*</span></span> |
| <span data-ttu-id="f19a0-285">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="f19a0-285">RedirectToPage("./Index");</span></span> | <span data-ttu-id="f19a0-286">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="f19a0-286">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="f19a0-287">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="f19a0-287">RedirectToPage("../Index")</span></span> | <span data-ttu-id="f19a0-288">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="f19a0-288">*Pages/Index*</span></span> |
| <span data-ttu-id="f19a0-289">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="f19a0-289">RedirectToPage("Index")</span></span>  | <span data-ttu-id="f19a0-290">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="f19a0-290">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="f19a0-291">`RedirectToPage("Index")`, `RedirectToPage("./Index")` и `RedirectToPage("../Index")` — это *относительные имена*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-291">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="f19a0-292">Для получения имени целевой страницы параметр `RedirectToPage` *комбинируется* с путем текущей страницы.</span><span class="sxs-lookup"><span data-stu-id="f19a0-292">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="f19a0-293">Привязка относительных имен полезна при создании сайтов со сложной структурой.</span><span class="sxs-lookup"><span data-stu-id="f19a0-293">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="f19a0-294">Если для связи между страницами в определенной папке используются относительные имена, эту папку можно переименовать.</span><span class="sxs-lookup"><span data-stu-id="f19a0-294">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="f19a0-295">При этом все ссылки останутся рабочими (так как не включают имя папки).</span><span class="sxs-lookup"><span data-stu-id="f19a0-295">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="f19a0-296">TempData</span><span class="sxs-lookup"><span data-stu-id="f19a0-296">TempData</span></span>

<span data-ttu-id="f19a0-297">ASP.NET Core позволяет использовать свойство [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) в [контроллере](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="f19a0-297">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="f19a0-298">Это свойство хранит данные до тех пор, пока они не будут прочитаны.</span><span class="sxs-lookup"><span data-stu-id="f19a0-298">This property stores data until it's read.</span></span> <span data-ttu-id="f19a0-299">Для проверки данных без удаления можно использовать методы `Keep` и `Peek`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-299">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="f19a0-300">`TempData` удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса.</span><span class="sxs-lookup"><span data-stu-id="f19a0-300">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="f19a0-301">Атрибут `[TempData]` появился в ASP.NET 2.0 и поддерживается в контроллерах и на страницах.</span><span class="sxs-lookup"><span data-stu-id="f19a0-301">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="f19a0-302">В следующем коде значение `Message` задается с помощью `TempData`:</span><span class="sxs-lookup"><span data-stu-id="f19a0-302">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="f19a0-303">Представленная ниже разметка в файле *Pages/Customers/Index.cshtml* отображает значение `Message` с помощью `TempData`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-303">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="f19a0-304">Модель страницы *Pages/Customers/Index.cshtml.cs* применяет атрибут `[TempData]` к свойству `Message`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-304">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="f19a0-305">Дополнительные сведения см. в разделе [TempData](xref:fundamentals/app-state#temp).</span><span class="sxs-lookup"><span data-stu-id="f19a0-305">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="f19a0-306">Несколько обработчиков на страницу</span><span class="sxs-lookup"><span data-stu-id="f19a0-306">Multiple handlers per page</span></span>

<span data-ttu-id="f19a0-307">Следующая страница формирует разметку для двух обработчиков страницы с помощью вспомогательного тега `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="f19a0-307">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="f19a0-308">Форма в предыдущем примере включает две кнопки отправки, каждая из которых отправляет данные на отдельный URL-адрес с помощью `FormActionTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-308">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="f19a0-309">Атрибут `asp-page-handler` является дополнением к `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-309">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="f19a0-310">Атрибут `asp-page-handler` формирует URL-адреса, ,которые используются для отправки данных в каждый из методов обработчиков, определенных страницей.</span><span class="sxs-lookup"><span data-stu-id="f19a0-310">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="f19a0-311">`asp-page` не задается, так как пример сопоставлен с текущей страницей.</span><span class="sxs-lookup"><span data-stu-id="f19a0-311">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="f19a0-312">Модель страницы</span><span class="sxs-lookup"><span data-stu-id="f19a0-312">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="f19a0-313">В представленном выше коде используются *именованные методы обработчика*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-313">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="f19a0-314">Именованные методы обработчика создаются путем размещения определенного текста в имени после `On<HTTP Verb>` и перед `Async` (если есть).</span><span class="sxs-lookup"><span data-stu-id="f19a0-314">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="f19a0-315">В приведенном выше примере использовались методы страницы OnPost**JoinList**Async и OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="f19a0-315">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="f19a0-316">Если убрать *OnPost* и *Async*, имена обработчиков будут выглядеть как `JoinList` и `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-316">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="f19a0-317">При использовании представленного выше кода URL-путь для отправки данных в `OnPostJoinListAsync` будет выглядеть как `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-317">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="f19a0-318">URL-путь для отправки данных в `OnPostJoinListUCAsync` будет иметь вид `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="f19a0-318">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="f19a0-319">Настройка маршрутизации</span><span class="sxs-lookup"><span data-stu-id="f19a0-319">Customizing Routing</span></span>

<span data-ttu-id="f19a0-320">Если вы не хотите, чтобы в URL-адресе отображалась строка запроса `?handler=JoinList`, измените маршрут таким образом, чтобы в качестве пути в URL-адресе указывалось имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="f19a0-320">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="f19a0-321">Для настройки маршрута можно добавить после директивы `@page` шаблон маршрута, заключенные в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="f19a0-321">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="f19a0-322">В представленном выше маршруте вместо строки запроса URL-путь содержит имя обработчика.</span><span class="sxs-lookup"><span data-stu-id="f19a0-322">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="f19a0-323">Символ `?` после `handler` означает, что параметр маршрута является необязательным.</span><span class="sxs-lookup"><span data-stu-id="f19a0-323">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="f19a0-324">С помощью директивы `@page` можно добавить к маршруту страницы дополнительные сегменты и параметры.</span><span class="sxs-lookup"><span data-stu-id="f19a0-324">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="f19a0-325">Все, что вы добавите, будет **присоединено** к маршруту страницы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f19a0-325">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="f19a0-326">Использовать для изменения маршрута страницы абсолютный или виртуальный путь (например, вида `"~/Some/Other/Path"`) нельзя.</span><span class="sxs-lookup"><span data-stu-id="f19a0-326">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="f19a0-327">Конфигурация и параметры</span><span class="sxs-lookup"><span data-stu-id="f19a0-327">Configuration and settings</span></span>

<span data-ttu-id="f19a0-328">Чтобы настроить дополнительные параметры, воспользуйтесь методом расширения `AddRazorPagesOptions` в построителе MVC:</span><span class="sxs-lookup"><span data-stu-id="f19a0-328">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="f19a0-329">В настоящее время `RazorPagesOptions` позволяет задавать корневую папку для страниц и добавлять для них обозначения моделей приложений.</span><span class="sxs-lookup"><span data-stu-id="f19a0-329">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="f19a0-330">В дальнейшем мы расширим спектр его возможностей.</span><span class="sxs-lookup"><span data-stu-id="f19a0-330">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="f19a0-331">Сведения о предварительной компиляции представлений см. в статье [Компиляция представлений Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="f19a0-331">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="f19a0-332">[Загрузить или просмотреть пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="f19a0-332">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="f19a0-333">См. раздел [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start), который дает дополнительную информацию к введению.</span><span class="sxs-lookup"><span data-stu-id="f19a0-333">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="f19a0-334">Указание местонахождения Razor Pages в корне каталога</span><span class="sxs-lookup"><span data-stu-id="f19a0-334">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="f19a0-335">По умолчанию Razor Pages находится в корне каталога */Pages*.</span><span class="sxs-lookup"><span data-stu-id="f19a0-335">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="f19a0-336">Добавьте [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в корне содержимого Razor Pages ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) приложения:</span><span class="sxs-lookup"><span data-stu-id="f19a0-336">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="f19a0-337">Указание местонахождения Razor Pages в пользовательском корневом каталоге</span><span class="sxs-lookup"><span data-stu-id="f19a0-337">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="f19a0-338">Добавьте [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в пользовательском корневом каталоге в приложении (укажите относительный путь):</span><span class="sxs-lookup"><span data-stu-id="f19a0-338">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="f19a0-339">См. также</span><span class="sxs-lookup"><span data-stu-id="f19a0-339">See also</span></span>

* [<span data-ttu-id="f19a0-340">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f19a0-340">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="f19a0-341">Начало работы с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f19a0-341">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="f19a0-342">Соглашения об авторизации Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f19a0-342">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="f19a0-343">Пользовательские поставщики моделей маршрутов и страниц Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f19a0-343">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="f19a0-344">Модульное тестирование и тестирование интеграции для Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f19a0-344">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
